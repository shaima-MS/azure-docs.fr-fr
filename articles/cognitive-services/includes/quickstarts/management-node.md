---
title: 'Démarrage rapide : Bibliothèque de client de gestion Azure pour Node.js'
description: Dans ce guide de démarrage rapide, vous allez vous familiariser avec la bibliothèque de client de gestion Azure pour Node.js.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 59063912b2976890976e3c27bc4f2d93b6392e37
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607620"
---
[Documentation de référence](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest) | [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices) | [Package (NPM)](https://www.npmjs.com/package/@azure/arm-cognitiveservices) | [Exemples](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices#sample-code)

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure valide - [Créer un abonnement gratuitement](https://azure.microsoft.com/free/)
* Version actuelle de [Node.js](https://nodejs.org/)

## <a name="create-an-azure-service-principal"></a>Créer un principal de service Azure

Pour que votre application interagisse avec votre compte Azure, un principal de service Azure doit gérer les autorisations. Suivez les instructions de l'article [Créer un principal de service Azure](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0).

Lorsque vous créez un principal du service, vous constatez qu'il dispose d'une valeur de secret, d'un ID et d'un ID d'application. Enregistrez l'ID d'application et le secret à un emplacement temporaire. Vous en aurez besoin ultérieurement.

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Avant de créer une ressource Cognitive Services, votre compte doit disposer d'un groupe de ressources Azure pour la contenir. Si vous n'avez pas encore de groupe de ressources, créez-en un sur le [portail Azure](https://ms.portal.azure.com/).

## <a name="create-a-new-nodejs-application"></a>Création d’une application Node.js

Dans une fenêtre de console (telle que cmd, PowerShell ou bash), créez un répertoire pour votre application et accédez-y. 

```console
mkdir myapp && cd myapp
```

Exécutez la commande `npm init` pour créer une application de nœud avec un fichier `package.json`. 

```console
npm init
```

Avant de continuer, créez un fichier nommé _index.js_.

### <a name="install-the-client-library"></a>Installer la bibliothèque de client

Installez les packages NPM suivants :

```console
npm install @azure/arm-cognitiveservices
npm install @azure/ms-rest-js
npm install @azure/ms-rest-nodeauth
```

Le fichier `package.json` de votre application sera mis à jour avec les dépendances.

### <a name="import-libraries"></a>Importer des bibliothèques

Ouvrez votre script _index.js_ et importez les bibliothèques suivantes.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_imports)]

## <a name="authenticate-the-client"></a>Authentifier le client

Ajoutez les champs suivants à la racine de votre script et complétez-les avec les valeurs appropriées à l'aide du principal de service que vous avez créé et des informations de votre compte Azure.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_constants)]

Ajoutez ensuite la fonction `quickstart` suivante pour gérer le travail principal de votre programme. Le premier bloc de code génère un objet **CognitiveServicesManagementClient** à l'aide des variables d'identification que vous avez entrées ci-dessus. Cet objet est nécessaire pour toutes vos opérations de gestion Azure.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main_auth)]

## <a name="call-management-functions"></a>Appeler les fonctions de gestion

Ajoutez le code suivant à la fin de votre fonction `quickstart` pour répertorier les ressources disponibles, créer un exemple de ressource, répertorier les ressources que vous possédez, puis supprimer l'exemple de ressource. Vous définirez ces fonctions au cours des étapes suivantes.

## <a name="create-a-cognitive-services-resource"></a>Créer une ressource Cognitive Services

### <a name="choose-a-service-and-pricing-tier"></a>Choisir un service et son niveau tarifaire

Lorsque vous créez une nouvelle ressource, vous devez connaître le « type » du service que vous souhaitez utiliser, ainsi que le [niveau tarifaire](https://azure.microsoft.com/pricing/details/cognitive-services/) (ou référence SKU) souhaité. Ces informations et d'autres vous serviront de paramètres lors de la création de la ressource. La fonction suivante répertorie les « types » de ressources Cognitive Services disponibles.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

Consultez la liste des références SKU et des informations tarifaires ci-dessous. 

#### <a name="multi-service"></a>Multiservice

| Service                    | Type                      |
|----------------------------|---------------------------|
| Plusieurs services. Pour plus d’informations, consultez la page des [tarifs](https://azure.microsoft.com/pricing/details/cognitive-services/).            | `CognitiveServices`     |


#### <a name="vision"></a>Vision

| Service                    | Type                      |
|----------------------------|---------------------------|
| Vision par ordinateur            | `ComputerVision`          |
| Custom Vision - Prédiction | `CustomVision.Prediction` |
| Custom Vision - Formation   | `CustomVision.Training`   |
| Face                       | `Face`                    |
| Form Recognizer            | `FormRecognizer`          |
| Ink Recognizer             | `InkRecognizer`           |

#### <a name="search"></a>Recherche

| Service            | Type                  |
|--------------------|-----------------------|
| Suggestion automatique Bing   | `Bing.Autosuggest.v7` |
| Recherche personnalisée Bing | `Bing.CustomSearch`   |
| Recherche d’entité Bing | `Bing.EntitySearch`   |
| Bing Search        | `Bing.Search.v7`      |
| Vérification orthographique Bing   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Speech

| Service            | Type                 |
|--------------------|----------------------|
| Services Speech    | `SpeechServices`     |
| Reconnaissance vocale | `SpeakerRecognition` |

#### <a name="language"></a>Langage

| Service            | Type                |
|--------------------|---------------------|
| Compréhension de formulaire | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Analyse de texte     | `TextAnalytics`     |
| Traduction de texte   | `TextTranslation`   |

#### <a name="decision"></a>Décision

| Service           | Type               |
|-------------------|--------------------|
| Le détecteur d’anomalies  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizer      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Niveaux tarifaires et facturation

Les niveaux tarifaires (et le montant facturé) sont basés sur le nombre de transactions que vous envoyez à l’aide de vos informations d’authentification. Chaque niveau tarifaire spécifie :
* le nombre maximal de transactions par seconde (TPS) autorisées ;
* les fonctionnalités de service activées dans le niveau tarifaire ;
* le coût d'un nombre prédéfini de transactions. Si vous dépassez ce nombre, des frais supplémentaires vous seront facturés, comme indiqué dans les [détails de la tarification](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) de votre service.

> [!NOTE]
> De nombreuses ressources Cognitive Services disposent d'un niveau gratuit que vous pouvez utiliser pour tester le service. Pour utiliser le niveau gratuit, utilisez `F0` en tant que référence SKU de votre ressource.

## <a name="create-a-cognitive-services-resource"></a>Créer une ressource Cognitive Services

Pour créer une ressource Cognitive Services et s'y abonner, utilisez la fonction **Create**. Cette fonction ajoute une nouvelle ressource facturable au groupe de ressources que vous transmettez. Lorsque vous créez votre nouvelle ressource, vous devez connaître le « type » du service que vous souhaitez utiliser, ainsi que son niveau tarifaire (ou référence SKU) et un emplacement Azure. La fonction suivante utilise tous ces arguments et crée une ressource.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_create)]

## <a name="view-your-resources"></a>Afficher vos ressources

Pour afficher toutes les ressources disponibles sous votre compte Azure (dans tous les groupes de ressources), utilisez la fonction suivante :

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list)]

## <a name="delete-a-resource"></a>Supprimer une ressource

La fonction suivante supprime la ressource spécifiée du groupe de ressources donné.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_delete)]

## <a name="run-the-application"></a>Exécution de l'application

Ajoutez le code suivant en bas de votre script pour appeler votre fonction `quickstart` principale avec gestion des erreurs.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main)]

Puis, dans la fenêtre de votre console, exécutez l'application à l'aide de la commande `node`.

```console
node index.js
```

## <a name="see-also"></a>Voir aussi

* [Documentation de référence du SDK de gestion Azure](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest)
* [Présentation d’Azure Cognitive Services](../../Welcome.md)
* [Authentifier des requêtes auprès d’Azure Cognitive Services](../../authentication.md)
* [Créer une ressource en utilisant le portail Azure](../../cognitive-services-apis-create-account.md)