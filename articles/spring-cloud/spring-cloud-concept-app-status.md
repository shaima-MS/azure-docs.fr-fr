---
title: Présentation de l’état des applications dans Azure Spring Cloud
description: En savoir plus sur les catégories d’état des applications dans Azure Spring Cloud
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: e3ef202a1a98b8193b55bcc4c2cb616d4a2000d8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037761"
---
# <a name="understanding-app-status-in-azure-spring-cloud"></a>Présentation de l’état des applications dans Azure Spring Cloud

L’interface utilisateur d’Azure Spring Cloud fournit des informations sur l’état des applications en cours d’exécution.  Il existe une option **Applications** pour chaque groupe de ressources dans un abonnement qui affiche l’état général des types d’applications.  Pour chaque type d’application, il y a un affichage des **instances d’application**.

## <a name="apps-status"></a>État des applications
Pour afficher l’état général d’un type d’application, sélectionnez **Applications** dans le volet de navigation gauche d’un groupe de ressources. Le résultat affiche l’état de l’application déployée :

* **État d’approvisionnement** indique l’état d’approvisionnement du déploiement.
* **Instance en cours d’exécution** indique le nombre d’instances d’application en cours d’exécution ou le nombre souhaité d’instances d’application. Si l’application doit être arrêtée, cette colonne indique *Arrêté*.
* **Instance inscrite** indique le nombre d’instances d’application inscrite à Eureka ou le nombre souhaité d’instances d’application. Si l’application doit être arrêtée, cette colonne indique *Arrêté*.


 ![État des applications](media/spring-cloud-concept-app-status/apps-ui-status.png)

**Les différents états de déploiement signalés sont les suivants :**

| Enum | Définition |
|:--:|:----------------:|
| Exécution en cours | Le déploiement DOIT être en cours d’exécution. |
| Arrêté | Le déploiement DOIT être arrêté. |

**L’état d’approvisionnement n’est accessible qu’à partir de l’interface CLI.  Les différents états signalés sont les suivants :**

| Enum | Définition |
|:--:|:----------------:|
| Creating | La ressource est en cours de création. |
| Mise à jour | La ressource est en cours de mise à jour. |
| Opération réussie | A approvisionné les ressources et déploie le binaire. |
| Échec | N’a pas atteint l’objectif *Opération réussie*. |
| En cours de suppression | La ressource est en cours de suppression. Cela empêche l’opération et la ressource n’est pas disponible dans cet état. |

## <a name="app-instances-status"></a>État des instances d’application

Pour afficher l’état d’une instance spécifique d’une application déployée, cliquez sur le **nom** de l’application dans l’interface utilisateur **Applications**. Les résultats afficheront :
* **État** : indique que l’instance est en cours d’exécution ou son état
* **État de la découverte** : état inscrit de l’instance d’application dans le serveur Eureka

 ![État des instances d’application](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

**Les différents états d’instance signalés sont les suivants :**

| Enum | Définition |
|:--:|:----------------:|
| Démarrage en cours | Le binaire est correctement déployé sur l’instance donnée. L’instance qui démarre le fichier jar peut échouer, car jar ne peut pas s’exécuter correctement. |
| Exécution en cours | L’instance fonctionne. |
| Échec | L’instance d’application n’a pas pu démarrer le binaire de l’utilisateur après plusieurs tentatives. |
| Fin d’exécution | L’instance d’application est en cours d’arrêt. |

**Les différents états de découverte de l’instance qui sont signalés sont les suivants :**

| Enum | Définition |
|:--:|:----------------:|
| UP | L’instance d’application est inscrite auprès d’Eureka et prête à recevoir le trafic. |
| OUT_OF_SERVICE | L’instance d’application est inscrite auprès d’Eureka et peut recevoir le trafic, mais s’arrête pour le trafic intentionnellement. |
| INACTIF | L’instance d’application n’est pas inscrite auprès d’Eureka ou est inscrite, mais ne peut pas recevoir le trafic. |


## <a name="see-also"></a>Voir aussi
* [Préparer une application Spring Java pour le déploiement dans Azure Spring Cloud](spring-cloud-tutorial-prepare-app-deployment.md)