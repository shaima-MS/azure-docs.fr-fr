---
title: Résolution des erreurs intermittentes de connexion sortante dans Azure App Service
description: Résoudre les erreurs de connexion intermittentes et les problèmes de performances liés dans Azure App Service
author: v-miegge
manager: barbkess
ms.topic: troubleshooting
ms.date: 07/24/2020
ms.author: ramakoni
ms.custom: security-recommendations
ms.openlocfilehash: 5e1f2108c5607917c77330f362952f960e57e03a
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87447911"
---
# <a name="troubleshooting-intermittent-outbound-connection-errors-in-azure-app-service"></a>Résolution des erreurs intermittentes de connexion sortante dans Azure App Service

Cet article vous aide à résoudre les erreurs de connexion intermittentes et les problèmes de performances liés dans [Azure App Service](https://docs.microsoft.com/azure/app-service/overview). Cette rubrique fournit des informations supplémentaires et des méthodologies de dépannage pour l’épuisement des ports SNAT. Si vous avez besoin d’une aide supplémentaire à quelque étape que ce soit dans cet article, contactez les experts Azure sur les [forums MSDN Azure et Stack Overflow](https://azure.microsoft.com/support/forums/). Vous pouvez également signaler un incident au support Azure. Accédez au [site du support Azure](https://azure.microsoft.com/support/options/), puis sélectionnez **Obtenir un support**.

## <a name="symptoms"></a>Symptômes

Les applications et fonctions hébergées sur Azure App Service peuvent présenter un ou plusieurs des symptômes suivants :

* Temps de réponse lent sur l’ensemble ou une partie des instances d’un plan de service.
* Erreurs 5xx ou **Passerelle incorrecte** intermittentes
* Messages d'erreur de dépassement de délai d'expiration
* Impossible de se connecter aux points de terminaison externes (tels que SQLDB, Service Fabric, d’autres services d’application, etc.)

## <a name="cause"></a>Cause

L’une des causes majeures de ces symptômes est que l’instance d’application ne peut pas ouvrir une nouvelle connexion au point de terminaison externe, car elle a atteint l’une des limites suivantes :

* Connexions TCP : Le nombre de connexions sortantes qui peuvent être établies est limité. Cette valeur est associée à la taille du Worker utilisé.
* Ports SNAT : Comme évoqué dans [Connexions sortantes dans Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections), Azure utilise la traduction d’adresses réseau source (SNAT) et un équilibreur de charge (non exposé aux clients) pour communiquer avec les points de terminaison en dehors d’Azure dans l’espace d’adressage IP public. Chaque instance sur Azure App Service reçoit initialement un nombre pré-alloué de **128** ports SNAT. Cette limite affecte l’ouverture des connexions à la même combinaison hôte/port. Si votre application crée des connexions à une combinaison d’adresses et de ports, vous n’utiliserez pas vos ports SNAT. Les ports SNAT sont utilisés lorsque vous répétez des appels à la même combinaison adresse/port. Une fois qu’un port a été libéré, le port peut être réutilisé en fonction des besoins. L’équilibreur de charge réseau Azure récupère le port SNAT à partir des connexions fermées seulement après avoir attendu 4 minutes.

Lorsque des applications ou des fonctions ouvrent rapidement une nouvelle connexion, elles peuvent rapidement épuiser leur quota pré-alloué de 128 ports. Elles sont ensuite bloquées jusqu’à ce qu’un nouveau port SNAT soit disponible, soit par le biais de l’allocation dynamique de ports SNAT supplémentaires, soit par réutilisation d’un port SNAT récupéré. Les applications ou fonctions qui sont bloquées en raison de cette incapacité à créer de nouvelles connexions commenceront à rencontrer un ou plusieurs des problèmes décrits dans la section **Symptômes** de cet article.

## <a name="avoiding-the-problem"></a>Éviter le problème

Si votre destination est un service Azure qui prend en charge les points de terminaison de service, vous pouvez éviter les problèmes d’épuisement des ports SNAT en utilisant l’[intégration de réseau virtuel régional](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet) et des points de terminaison de service ou des points de terminaison privés. Lorsque vous utilisez l’intégration de réseau virtuel régional et placez des points de terminaison de service sur le sous-réseau d’intégration, le trafic sortant de votre application vers ces services n’aura pas de restrictions de ports SNAT sortants. De même, si vous utilisez l’intégration de réseau virtuel régional et des points de terminaison privés, aucun problème de ports SNAT sortants n’est lié à cette destination. 

Éviter le problème de port SNAT consiste à éviter la création de nouvelles connexions de façon répétée sur le même hôte et le même port.

Les stratégies générales d’atténuation de l’épuisement des ports SNAT sont décrites dans la [section Résolution des problèmes](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#problemsolving) de la documentation **Connexions sortantes d’Azure**. Parmi ces stratégies, les éléments suivants s’appliquent aux applications et aux fonctions hébergées sur Azure App Service.

### <a name="modify-the-application-to-use-connection-pooling"></a>Modifier l’application pour utiliser un regroupement de connexions

* Pour le regroupement des connexions HTTP, passez en revue [Connexions HTTP au pool avec HttpClientFactory](https://docs.microsoft.com/aspnet/core/performance/performance-best-practices#pool-http-connections-with-httpclientfactory).
* Pour plus d’informations sur le regroupement de connexions SQL Server, consultez [Regroupement de connexions SQL Server (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).
* Pour implémenter le regroupement avec des applications Entity Framework, consultez [Regroupement DbContext](https://docs.microsoft.com/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling).

Voici une collection de liens permettant d’implémenter le regroupement de connexions par différente pile de solutions.

#### <a name="node"></a>Nœud

Par défaut, les connexions pour NodeJS ne sont pas gardées actives. Les bases de données et packages ci-dessous sont couramment utilisés pour le regroupement de connexions et contiennent des exemples d’implémentation.

* [MySQL](https://github.com/mysqljs/mysql#pooling-connections)
* [MongoDB](https://blog.mlab.com/2017/05/mongodb-connection-pooling-for-express-applications/)
* [PostgreSQL](https://node-postgres.com/features/pooling)
* [SQL Server](https://github.com/tediousjs/node-mssql#connection-pools)

HTTP Keep-alive

* [agentkeepalive](https://www.npmjs.com/package/agentkeepalive)
* [Documentation Node.js v13.9.0](https://nodejs.org/api/http.html)

#### <a name="java"></a>Java

Les bibliothèques ci-dessous sont couramment utilisées pour le regroupement de connexions JDBC et contiennent des exemples d’implémentation : Regroupement de connexions JDBC.

* [Tomcat 8](https://tomcat.apache.org/tomcat-8.0-doc/jdbc-pool.html)
* [C3p0](https://github.com/swaldman/c3p0)
* [HikariCP](https://github.com/brettwooldridge/HikariCP)
* [Apache DBCP](https://commons.apache.org/proper/commons-dbcp/)

Regroupement de connexions HTTP

* [Gestion des connexions Apache](https://hc.apache.org/httpcomponents-client-ga/tutorial/html/connmgmt.html)
* [Classe PoolingHttpClientConnectionManager](http://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/org/apache/http/impl/conn/PoolingHttpClientConnectionManager.html)

#### <a name="php"></a>PHP

Bien que PHP ne prenne pas en charge le regroupement de connexions, vous pouvez essayer d’utiliser des connexions de base de données persistantes à votre serveur back-end.
 
* MySQL Server

   * [Connexions MySQLi](https://www.php.net/manual/mysqli.quickstart.connections.php) pour les versions récentes
   * [mysql_pconnect](https://www.php.net/manual/function.mysql-pconnect.php) pour les versions anciennes de PHP

* Autres sources de données

   * [Gestion des connexions PHP](https://www.php.net/manual/en/pdo.connections.php)

#### <a name="python"></a>Python

* [MySQL](https://github.com/mysqljs/mysql#pooling-connections)
* [MongoDB](https://blog.mlab.com/2017/05/mongodb-connection-pooling-for-express-applications/)
* [PostgreSQL](https://node-postgres.com/features/pooling)
* [SQL Server](https://github.com/tediousjs/node-mssql#connection-pools) (REMARQUE : SQLAlchemy peut être utilisé avec d’autres bases de données à côté de Microsoft SQL Server)
* [HTTP Keep-alive](https://requests.readthedocs.io/en/master/user/advanced/#keep-alive) (Keep-Alive est automatique lors de l’utilisation de [session-objects](https://requests.readthedocs.io/en/master/user/advanced/#keep-alive)).

Pour les autres environnements, consultez les documents spécifiques aux fournisseurs ou aux pilotes pour l’implémentation du regroupement de connexions dans vos applications.

### <a name="modify-the-application-to-reuse-connections"></a>Modifier l’application pour réutiliser des connexions

*  Pour obtenir des conseils supplémentaires et des exemples sur la gestion des connexions dans Azure Functions, consultez [Gérer les connexions dans Azure Functions](https://docs.microsoft.com/azure/azure-functions/manage-connections).

### <a name="modify-the-application-to-use-less-aggressive-retry-logic"></a>Modifier l’application pour utiliser une logique de nouvelle tentative moins agressive

* Pour obtenir des instructions et des exemples supplémentaires, consultez [Modèle Nouvelle tentative](https://docs.microsoft.com/azure/architecture/patterns/retry).

### <a name="use-keepalives-to-reset-the-outbound-idle-timeout"></a>Utiliser des conservations de connexion active pour réinitialiser le délai d’inactivité en sortie

* Pour implémenter KeepAlive pour les applications Node.js, consultez [Mon application node émet un nombre excessif d’appels sortants](https://docs.microsoft.com/azure/app-service/app-service-web-nodejs-best-practices-and-troubleshoot-guide#my-node-application-is-making-excessive-outbound-calls).

### <a name="additional-guidance-specific-to-app-service"></a>Conseils supplémentaires spécifiques à App Service :

* Un [test de charge](https://docs.microsoft.com/azure/devops/test/load-test/app-service-web-app-performance-test) doit simuler des données réelles avec une vitesse d’alimentation constante. Le test d’applications et de fonctions avec une charge réelle peut vous aider à identifier et résoudre les problèmes d’épuisement des ports SNAT à l’avance.
* Vérifiez que les services back-end peuvent renvoyer des réponses rapidement. Pour résoudre les problèmes de performances liés à Azure SQL Database, consultez [Résoudre les problèmes de performances liés à Azure SQL Database avec Intelligence Insights](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance#recommended-troubleshooting-flow).
* Mettez à l’échelle le plan App Service avec plus d’instances. Pour plus d’informations sur la mise à l’échelle, consultez [Mise à l’échelle des applications dans un environnement Azure App Service](https://docs.microsoft.com/azure/app-service/manage-scale-up). Un certain nombre de ports SNAT sont alloués à chaque instance de Worker dans un plan App Service. Si vous répartissez votre utilisation sur davantage d’instances, vous pouvez vous retrouver avec une utilisation de ports SNAT par instance en dessous de la limite recommandée de 100 connexions sortantes par point de terminaison distant unique.
* Envisagez de passer à [App Service Environment (ASE)](https://docs.microsoft.com/azure/app-service/environment/using-an-ase), où une seule adresse IP sortante vous est attribuée, et où les limites pour les connexions et les ports SNAT sont bien plus élevées.

L’évitement des limites TCP sortantes est plus facile à résoudre, car les limites sont définies en fonction de la taille de votre worker. Vous pouvez voir les limites dans [Limites numériques des machines virtuelles bac à sable - Connexions TCP](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits)

|Nom de la limite|Description|Petite (A1)|Moyenne (A2)|Grande (A3)|Niveau isolé (ASE)|
|---|---|---|---|---|---|
|Connexions|Nombre de connexions sur l’ensemble de la machine virtuelle|1920|3968|8064|16 000|

Pour éviter les limites TCP sortantes, vous pouvez augmenter la taille de vos workers ou effectuer une montée en charge horizontale.

## <a name="troubleshooting"></a>Dépannage

La connaissance des deux types de limites de connexions sortantes et de ce que fait votre application devrait faciliter le dépannage. Si vous savez que votre application effectue un grand nombre d’appels au même compte de stockage, vous pourriez soupçonner une limite de SNAT. Si votre application crée un grand nombre d’appels à des points de terminaison sur Internet, vous pourriez soupçonner d’avoir atteint la limite de la machine virtuelle.

Si vous ne connaissez pas suffisamment le comportement de l’application pour déterminer la cause rapidement, il existe des outils et des techniques disponibles dans App Service pour faciliter cette détermination.

### <a name="find-snat-port-allocation-information"></a>Trouver les informations sur l’allocation de ports SNAT

Vous pouvez utiliser les [Diagnostics App Service](https://docs.microsoft.com/azure/app-service/overview-diagnostics) pour rechercher des informations sur l’allocation des ports SNAT et observer la métrique d’allocation de ports SNAT d’un site App Service. Pour rechercher des informations sur l’allocation de ports SNAT, procédez comme suit :

1. Pour accéder aux diagnostics App Service, accédez à votre application App Service ou à votre environnement App Service dans le [Portail Azure](https://portal.azure.com/). Dans le volet de navigation de gauche, sélectionnez **Diagnostiquer et résoudre les problèmes**.
2. Sélectionner la catégorie Disponibilité et performances
3. Sélectionnez la vignette d’épuisement des ports SNAT dans la liste des vignettes disponibles sous la catégorie. La pratique consiste à rester en dessous de 128.
Si vous en avez besoin, vous pouvez toujours ouvrir un ticket de support, et l’ingénieur du support technique obtiendra la métrique à partir du serveur principal pour vous.

Notez que dans la mesure où l’utilisation des ports SNAT n’est pas disponible en tant que métrique, il n’est pas possible de mettre à l’échelle automatiquement en fonction de l’utilisation des ports SNAT ou de configurer la mise à l’échelle automatique en fonction de la métrique d’allocation des ports SNAT.

### <a name="tcp-connections-and-snat-ports"></a>Connexions TCP et ports SNAT

Les connexions TCP et les ports SNAT ne sont pas directement liés. Un détecteur d’utilisation des connexions TCP est inclus dans le panneau Diagnostiquer et résoudre les problèmes de n’importe quel site App Service. Recherchez l’expression « Connexions TCP » pour trouver cette métrique.

* Les ports SNAT sont utilisés uniquement pour les flux de réseau externe, tandis que le nombre total de connexions TCP comprend les connexions de bouclage local.
* Un port SNAT peut être partagé par différents flux, si les flux sont différents en matière de protocole, d’adresse IP ou de port. La métrique Connexions TCP compte chaque connexion TCP.
* La limite de connexions TCP se produit au niveau de l’instance worker. L’équilibrage de charge sortant du réseau Azure n’utilise pas la métrique de connexions TCP pour la limitation des ports SNAT.
* Les limites des connexions TCP sont décrites dans [Limites numériques des machines virtuelles sandbox - Connexions TCP](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits)

|Nom de la limite|Description|Petite (A1)|Moyenne (A2)|Grande (A3)|Niveau isolé (ASE)|
|---|---|---|---|---|---|
|Connexions|Nombre de connexions sur l’ensemble de la machine virtuelle|1920|3968|8064|16 000|

### <a name="webjobs-and-database-connections"></a>WebJobs et connexions de base de données
 
Si les ports SNAT sont épuisés, et que les WebJobs ne parviennent pas à se connecter à SQL Database, il n’existe aucune métrique pour afficher le nombre de connexions ouvertes par chaque processus d’application web individuel. Pour trouver les WebJobs problématiques, déplacez plusieurs WebJobs vers un autre plan App Service pour voir si la situation s’améliore ou si un problème persiste dans l’un des plans. Répétez le processus jusqu’à ce que vous trouviez le WebJob problématique.

### <a name="using-snat-ports-sooner"></a>Utiliser les ports SNAT plus tôt

Vous ne pouvez pas modifier les paramètres Azure pour libérer les ports SNAT utilisés plus tôt, car tous les ports SNAT sont publiés conformément aux conditions ci-dessous et le comportement est normal.
 
* Si le client ou serveur envoie un paquet FINACK, le [port SNAT est mis à disposition](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#tcp-snat-port-release) après un délai de 240 secondes.
* Si une instance RST est visible, le port SNAT est mis à disposition après un délai de 15 secondes.
* Si le délai d’inactivité est atteint, le port est mis à disposition.
 
## <a name="additional-information"></a>Informations supplémentaires

* [SNAT avec App Service](https://4lowtherabbit.github.io/blogs/2019/10/SNAT/)
* [Résoudre les problèmes de baisse de performances d’une application dans Azure App Service](https://docs.microsoft.com/azure/app-service/troubleshoot-performance-degradation)
