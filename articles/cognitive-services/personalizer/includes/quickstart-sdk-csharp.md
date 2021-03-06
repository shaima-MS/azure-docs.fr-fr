---
title: Fichier include
description: Fichier include
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 07/30/2020
ms.openlocfilehash: fdae79912e6fe3bf2f7d55b7405cb7883e484c47
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88602445"
---
[Documentation de référence](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Package (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Exemples](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer)

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services)
* Version actuelle de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="using-this-quickstart"></a>Utilisation de ce guide de démarrage rapide

Plusieurs étapes sont nécessaires pour utiliser ce guide de démarrage rapide :

* Dans le portail Azure, créer une ressource Personalizer
* Sur le portail Azure, pour la ressource Personalizer, dans la page **Configuration**, définissez la fréquence de mise à jour des modèles sur un intervalle très court.
* Dans un éditeur de code, créer un fichier de code et le modifier
* À partir de la ligne de commande ou du terminal, installer le SDK
* À partir de la ligne de commande ou du terminal, exécuter le fichier de code

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-c-application"></a>Créer une application C#

Créez une application .NET Core dans votre éditeur ou IDE favori.

Dans une fenêtre de console (par exemple cmd, PowerShell ou Bash), utilisez la commande `new` dotnet pour créer une application console avec le nom `personalizer-quickstart`. Cette commande crée un projet C# « Hello World » simple avec un seul fichier source : `Program.cs`.

```console
dotnet new console -n personalizer-quickstart
```

Déplacez vos répertoires vers le dossier d’application nouvellement créé. Vous pouvez générer l’application avec :

```console
dotnet build
```

La sortie de génération ne doit contenir aucun avertissement ni erreur.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>Installer le Kit de développement logiciel (SDK)

Dans le répertoire de l’application, installez la bibliothèque de client Personalizer pour .NET avec la commande suivante :

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Si vous utilisez l’IDE Visual Studio, la bibliothèque de client est disponible sous forme de package NuGet téléchargeable.

## <a name="object-model"></a>Modèle objet

Le client Personalizer est un objet [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) qui s’authentifie auprès d’Azure à l’aide de Microsoft.Rest.ServiceClientCredentials, qui contient votre clé.

Pour demander l’élément de contenu le mieux adapté, créez un [RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview), puis passez-le à la méthode [client.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). La méthode Rank retourne un RankResponse.

Pour envoyer un score de récompense à Personalizer, créez un [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview), puis passez-le à la méthode [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview).

Ce guide de démarrage rapide n’accorde pas d’importance à la détermination du score de récompense. Dans un système de production, déterminer les éléments ayant un impact sur le [score de récompense](../concept-rewards.md) (et dans quelle mesure) peut être un processus complexe, que vous pouvez décider de modifier au fil du temps. Cette décision de conception doit être l’une des principales décisions à prendre pour votre architecture Personalizer.

## <a name="code-examples"></a>Exemples de code

Ces extraits de code montrent comment effectuer les tâches suivantes avec la bibliothèque de client Personalizer pour .NET :

* [Créer un client Personalizer](#create-a-personalizer-client)
* [API Rank](#request-the-best-action)
* [API Reward](#send-a-reward)

## <a name="add-the-dependencies"></a>Ajouter les dépendances

À partir du répertoire de projet, ouvrez le fichier **Program.cs** dans votre éditeur ou votre IDE favori. Remplacez le code `using` existant par les directives `using` suivantes :

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Ajouter des informations sur la ressource Personalizer

Dans la classe **Program**, modifiez les variables de clé et de point de terminaison en haut du fichier de code selon la clé et le point de terminaison Azure de votre ressource. 

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Créer un client Personalizer

Ensuite, créez une méthode pour retourner un client Personalizer. Le paramètre de la méthode est `PERSONALIZER_RESOURCE_ENDPOINT` et l’ApiKey est `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Obtenir des produits alimentaires sous forme d’actions à classer

Les actions représentent les choix de contenu parmi lesquels Personalizer doit sélectionner l’élément de contenu le mieux adapté. Ajoutez les méthodes suivantes à la classe Program pour représenter l’ensemble d’actions et leurs caractéristiques. 

[!code-csharp[Food items as actions](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Obtenir les préférences utilisateur pour le contexte

Ajoutez les méthodes suivantes à la classe Program pour obtenir les entrées d’un utilisateur à partir de la ligne de commande : heure de la journée et préférence alimentaire actuelle. Elles seront utilisées comme des caractéristiques de contexte.

[!code-csharp[Present time out day preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTastePreference)]

Les deux méthodes utilisent la méthode `GetKey` pour lire la sélection de l’utilisateur à partir de la ligne de commande.

[!code-csharp[Read user's choice from the command line](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Créer la boucle d’apprentissage

La boucle d’apprentissage Personalizer est un cycle d’appels aux API [Rank](#request-the-best-action) et [Reward](#send-a-reward). Dans ce guide de démarrage rapide, chaque appel Rank en vue de personnaliser le contenu est suivi d’un appel Reward qui indique à Personalizer l’efficacité du service.

Le code ci-après applique, en boucle, le cycle suivant : il demande à l’utilisateur d’indiquer ses préférences à partir de la ligne de commande, il envoie ces informations à Personalizer en vue de sélectionner le contenu le mieux adapté, il présente la sélection au client qui peut faire son choix dans la liste, puis il envoie un score de récompense à Personalizer, en indiquant l’efficacité du service concernant le choix du contenu.

[!code-csharp[Learning loop](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=mainLoop)]

Ajoutez les méthodes suivantes, qui [obtiennent les choix de contenu](#get-food-items-as-rankable-actions) avant d’exécuter le fichier de code :

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-the-best-action"></a>Demander l’action la mieux adaptée

Pour traiter la requête Rank, le programme demande les préférences de l’utilisateur afin de créer un `currentContext` avec les choix de contenu. Le processus peut créer du contenu à exclure des actions (`excludeActions`). Pour recevoir la réponse, la requête Rank a besoin des actions et de leurs caractéristiques, des caractéristiques currentContext, des excludeActions et d’un ID d’événement unique.

Ce guide de démarrage rapide utilise des caractéristiques de contexte simples basées sur l’heure de la journée et les préférences alimentaires de l’utilisateur. Dans les systèmes de production, il peut être important de déterminer et d’[évaluer](../concept-feature-evaluation.md) les [actions et caractéristiques](../concepts-features.md).

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Envoyer une récompense

Pour obtenir le score de récompense à envoyer dans la requête Reward, le programme récupère la sélection de l’utilisateur à partir de la ligne de commande, attribue une valeur numérique à la sélection, puis envoie à l’API Reward l’ID d’événement unique et le score de récompense sous la forme d’une valeur numérique.

Dans le cadre de ce guide de démarrage rapide, un simple numéro est attribué en tant que score de récompense : 0 ou 1. Dans les systèmes de production, il peut être important de déterminer ce qui doit être envoyé à l’appel [Reward](../concept-rewards.md) (et à quel moment) selon vos besoins.

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=reward)]

## <a name="run-the-program"></a>Exécuter le programme

Exécutez l’application avec la commande `run` dotnet à partir de votre répertoire d’application.

```console
dotnet run
```

![Ce programme de démarrage rapide pose quelques questions pour recueillir les préférences de l’utilisateur, appelées « caractéristiques », puis fournit l’action classée en premier.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

Le [code source associé à ce guide de démarrage rapide](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer) est disponible.
