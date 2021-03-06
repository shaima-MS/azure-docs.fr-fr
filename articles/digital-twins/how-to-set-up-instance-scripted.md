---
title: Configurer une instance et l’authentification (procédure scriptée)
titleSuffix: Azure Digital Twins
description: Découvrez comment configurer une instance du service Azure Digital Twins, en exécutant un script de déploiement automatisé
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 82ddba67a6bd9fcc56b74c3f830663228feb945b
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009696"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-scripted"></a>Configurer une instance Azure Digital Twins et l’authentification (procédure scriptée)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Cet article explique comment **configurer une nouvelle instance Azure Digital Twins**, notamment la création de l’instance et la configuration de l’authentification. À l’issue de cet article, vous aurez une instance Azure Digital Twins prête pour la programmation.

Cette version de cet article effectue ces étapes en exécutant un [exemple de **script de déploiement automatisé**](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/) qui simplifie le processus. 
* Pour afficher les étapes manuelles d’interface CLI que le script exécute en arrière-plan, consultez la version CLI de cet article : [*Guide pratique : Configurer une instance et l’authentification (CLI)* ](how-to-set-up-instance-cli.md).
* Pour afficher les étapes manuelles utilisant le portail Azure, consultez la version de cet article relative au portail : [*Guide pratique : Configurer une instance et l’authentification (portail)* ](how-to-set-up-instance-portal.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="run-the-deployment-script"></a>Exécuter le script de déploiement

Cet article utilise un exemple de code Azure Digital Twins pour déployer de façon semi-automatique une instance Azure Digital Twins et l’authentification requise. Il peut également être utilisé comme point de départ pour écrire vos propres interactions scriptées.

L’exemple de script est écrit dans PowerShell. Il fait partie des [exemples Azure Digital Twins](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/), que vous pouvez télécharger sur votre machine en accédant à cet exemple de lien et en sélectionnant le bouton *Télécharger le fichier ZIP* sous le titre.

Dans l’exemple de dossier téléchargé, le script de déploiement se trouve dans _Azure_Digital_Twins_samples.zip > scripts > **deploy.ps1**_.

Voici les étapes à suivre pour exécuter le script de déploiement dans Cloud Shell.
1. Accédez à une fenêtre [Azure Cloud Shell](https://shell.azure.com/) dans votre navigateur. Connectez-vous à l’aide de cette commande :
    ```azurecli-interactive
    az login
    ```
    Si l’interface CLI peut ouvrir votre navigateur par défaut, elle le fait et charge une page de connexion Azure par la même occasion. Sinon, ouvrez une page de navigateur à l’adresse *https://aka.ms/devicelogin* et entrez le code d’autorisation affiché dans votre terminal.
 
2. Une fois connecté, examinez la barre d’icône de la fenêtre Cloud Shell. Sélectionnez l’icône « Charger/Télécharger des fichiers » et choisissez « Charger ».

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-upload.png" alt-text="Fenêtre Cloud Shell présentant la sélection de l’option de chargement":::

    Accédez au fichier _**deploy.ps1**_ sur votre machine et sélectionnez « Ouvrir ». Cette opération charge le fichier dans Cloud Shell pour que vous puissiez l’exécuter dans la fenêtre Cloud Shell.

3. Exécutez le script en envoyant la commande `./deploy.ps1` dans la fenêtre Cloud Shell. Lorsque le script exécute les étapes de configuration automatisée, vous êtes invité à transmettre les valeurs suivantes :
    * Pour l’instance : l’*ID d’abonnement* de votre abonnement Azure à utiliser
    * Pour l’instance : un *emplacement* où vous souhaitez déployer l’instance. Pour voir quelles régions prennent en charge Azure Digital Twins, visitez [*Disponibilité des produits Azure par région*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
    * Pour l’instance : un nom de *groupe de ressources*. Vous pouvez utiliser un groupe de ressources existant ou entrer un nouveau nom pour en créer un.
    * Pour l’instance : un *nom* de votre instance Azure Digital Twins. Le nom de la nouvelle instance doit être unique dans la région pour votre abonnement (ce qui signifie que si votre abonnement a une autre instance Azure Digital Twins dans cette région, qui utilise déjà le nom que vous choisissez, vous devrez choisir un autre nom).
    * Pour l’inscription de l’application : un *nom d’affichage d’application Azure AD* à associer à l’inscription. Cette inscription d’application est l’emplacement où vous configurez les autorisations d’accès aux [API d’Azure Digital Twins](how-to-use-apis-sdks.md). Plus tard, l’application cliente s’authentifie auprès de l’inscription de l’application et les autorisations d’accès configurées sont alors accordées aux API.
    * Pour l’inscription de l’application : une *URL de réponse de l’application Azure AD* pour l’application Azure AD. Vous pouvez utiliser `http://localhost`.

Le script créera une instance Azure Digital Twins, attribuera à votre utilisateur Azure le rôle de *propriétaire Azure Digital Twins (préversion)* sur l’instance et configurera une inscription d’application Azure AD que votre application cliente utilisera.

Voici un extrait du journal de sortie du script :

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png" alt-text="Fenêtre Cloud Shell présentant le journal d’entrée et de sortie par le biais de l’exécution du script de déploiement" lightbox="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png":::

Si le script s’exécute correctement, l’affichage final indique `Deployment completed successfully`. Sinon, traitez le message d’erreur et réexécutez le script. Cela permet de contourner les étapes que vous avez déjà effectuées et de demander une nouvelle entrée au stade où vous vous étiez arrêté.

À l’issue de l’exécution du script, vous disposez maintenant d’une instance Azure Digital Twins prête à l’emploi avec des autorisations pour la gérer, et vous avez des autorisations de configuration pour qu’une application cliente y accède.

> [!NOTE]
> Le script attribue actuellement le rôle de gestion requis dans Azure Digital Twins (*Propriétaire Azure Digital Twins (préversion)* ) au même utilisateur qui exécute ce script à partir de Cloud Shell. Si vous devez attribuer ce rôle à une autre personne chargée de gérer cette instance, vous pouvez le faire via le portail Azure ([instructions](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) ou l’interface CLI ([instructions](how-to-set-up-instance-cli.md#set-up-user-access-permissions)).

## <a name="collect-important-values"></a>Collecter les valeurs importantes

Il existe plusieurs valeurs importantes parmi les ressources configurées par le script dont vous pouvez avoir besoin lorsque vous continuez à travailler avec votre instance Azure Digital Twins. Dans cette section, vous allez utiliser le [portail Azure](https://portal.azure.com) pour collecter ces valeurs. Vous devez les enregistrer dans un emplacement sûr ou revenir à cette section pour les retrouver plus tard lorsque vous en avez besoin.

Si d’autres utilisateurs doivent programmer sur l’instance, vous devez également partager ces valeurs avec eux.

### <a name="collect-instance-values"></a>Collecter les valeurs d’instance

Dans le [portail Azure](https://portal.azure.com), recherchez votre instance Azure Digital Twins en recherchant le nom de votre instance dans la barre de recherche du portail.

Si vous sélectionnez cette instance, sa page de *présentation* apparaît. Notez son *nom*, son *groupe de ressources* et son *nom d’hôte*. Vous en aurez peut-être besoin plus tard pour identifier votre instance et vous y connecter.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Mise en surbrillance des valeurs importantes de la page de présentation de l’instance":::

### <a name="collect-app-registration-values"></a>Collecter les valeurs d’inscription de l’application 

Il existe deux valeurs importantes provenant de l’inscription de l’application, qui seront nécessaires ultérieurement pour [écrire le code d’authentification de l’application cliente pour les API Azure Digital Twins](how-to-authenticate-client.md). 

Pour les trouver, suivez [ce lien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) pour accéder à la page de présentation de l’inscription de l’application Azure AD dans le portail Azure. Cette page affiche toutes les inscriptions d’application qui ont été créées dans votre abonnement.

Vous devez voir apparaître l’inscription d’application que vous venez de créer dans cette liste. Sélectionnez-la pour afficher ses détails :

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Vue du portail des valeurs importantes pour l’inscription de l’application":::

Prenez note de *l’ID d’application (client)* et de *l’ID de répertoire (locataire)* affichés sur **votre** page. Si vous n’êtes pas la personne chargée d’écrire du code pour les applications clientes, vous devez partager ces valeurs avec la personne qui en sera chargée.

## <a name="verify-success"></a>Vérifier la réussite de l’exécution

Si vous souhaitez vérifier la création de vos ressources et autorisations configurées par le script, vous pouvez les consulter dans le [portail Azure](https://portal.azure.com).

### <a name="verify-instance"></a>Vérifier l’instance

Pour vérifier que votre instance a été créée, accédez à la [page Azure Digital Twins](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) dans le portail Azure. Cette page liste toutes vos instances Azure Digital Twins. Recherchez le nom de votre instance nouvellement créée dans la liste.

### <a name="verify-user-role-assignment"></a>Vérifier l’attribution du rôle utilisateur

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

> [!NOTE]
> Rappelez-vous que le script affecte actuellement ce rôle nécessaire au même utilisateur qui exécute le script à partir de Cloud Shell. Si vous devez attribuer ce rôle à une autre personne chargée de gérer cette instance, vous pouvez le faire via le portail Azure ([instructions](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) ou l’interface CLI ([instructions](how-to-set-up-instance-cli.md#set-up-user-access-permissions)).

### <a name="verify-app-registration"></a>Vérifier l’inscription de l’application

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

Tout d’abord, vérifiez que les paramètres des autorisations Azure Digital Twins ont été correctement définis sur l’inscription. Pour ce faire, sélectionnez *Manifeste* dans la barre de menus pour afficher le code du manifeste de l’inscription de l’application. Faites défiler la fenêtre de code vers le bas et recherchez ces champs sous `requiredResourceAccess`. Les valeurs doivent correspondre à celles de la capture d’écran ci-dessous :

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

## <a name="other-possible-steps-for-your-organization"></a>Autres étapes possibles pour votre organisation

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment connecter votre application cliente à votre instance en écrivant le code d’authentification de l’application cliente :
* [*Guide pratique : Écrire le code d’authentification de l’application*](how-to-authenticate-client.md)
