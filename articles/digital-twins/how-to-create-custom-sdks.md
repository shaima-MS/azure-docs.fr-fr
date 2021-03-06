---
title: Création de kits SDK personnalisés pour Azure Digital Twins avec AutoRest
titleSuffix: Azure Digital Twins
description: Découvrez comment générer des kits de développement logiciel (SDK) personnalisés pour utiliser Azure Digital Twins avec des langages autres que C#.
author: baanders
ms.author: baanders
ms.date: 4/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: devx-track-javascript
ms.openlocfilehash: a93e0b6d29bb10e5e71f48134916cac9cd563fb2
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420037"
---
# <a name="create-custom-sdks-for-azure-digital-twins-using-autorest"></a>Création de kits SDK personnalisés pour Azure Digital Twins avec AutoRest

Pour le moment, le seul Kit de développement logiciel (SDK) de plan de données publié permettant d’interagir avec les API Azure Digital Twins est destiné à .NET (C#). Pour plus d’informations sur le Kit de développement logiciel (SDK) .NET et les API en général, consultez [*Guide pratique : Utiliser les API et les kits de développement logiciel (SDK) Azure Digital Twins*](how-to-use-apis-sdks.md). Si vous travaillez avec un autre langage, cet article vous montre comment générer votre propre Kit de développement logiciel (SDK) de plan de données dans le langage de votre choix à l’aide d’AutoRest.

>[!NOTE]
> Si vous le souhaitez, vous pouvez également utiliser AutoRest pour générer un Kit de développement logiciel (SDK) de plan de contrôle. Pour ce faire, suivez les étapes de cet article en utilisant le [fichier Swagger (OpenAPI) du plan de contrôle](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/preview/2020-03-01-preview) au lieu de celui du plan de données.

## <a name="set-up-your-machine"></a>Configurer votre machine

Pour générer un kit SDK, vous aurez besoin des éléments suivants :
* [AutoRest](https://github.com/Azure/autorest), version 2.0.4413 (la version 3 n’est pas prise en charge pour le moment)
* [Node.js](https://nodejs.org), qui fait partie des prérequis d’AutoRest
* Le [fichier Swagger (OpenAPI) du plan de données](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins/preview/2020-05-31-preview) d’Azure Digital Twins intitulé *digitaltwins.json* et le dossier d’exemples qui l’accompagne Téléchargez le fichier Swagger et son dossier d’exemples sur votre ordinateur local.

Une fois que votre machine est équipée de tous les éléments de la liste ci-dessus, vous êtes prêt à utiliser AutoRest pour créer le kit SDK.

## <a name="create-the-sdk-with-autorest"></a>Création du kit SDK avec AutoRest 

Si vous avez installé Node.js, vous pouvez exécuter cette commande pour vérifier que vous disposez de la bonne version d’AutoRest :
```cmd/sh
npm install -g autorest@2.0.4413
```

Pour exécuter AutoRest sur le fichier Swagger Azure Digital Twins, procédez comme suit :
1. Copiez le fichier Swagger Azure Digital Twins et le dossier d’exemples qui l’accompagne dans un répertoire de travail.
2. Utilisez une fenêtre d’invite de commandes pour basculer vers ce répertoire de travail.
3. Exécutez AutoRest avec la commande suivante. Remplacez l’espace réservé `<language>` par le langage de votre choix : `--python`, `--java`, `--go`, etc. (Vous trouverez la liste complète des options dans le [LISEZMOI AutoRest](https://github.com/Azure/autorest).)

```cmd/sh
autorest --input-file=adtApiSwagger.json --<language> --output-folder=ADTApi --add-credentials --azure-arm --namespace=ADTApi
```

En résulte un nouveau dossier nommé *ADTApi* dans votre répertoire de travail. Les fichiers du Kit de développement logiciel (SDK) générés auront l’espace de noms *ADTApi*. Vous continuerez à utiliser cet espace de noms dans les autres exemples d’utilisation de cet article.

AutoRest prend en charge un large éventail de générateurs de code de langage.

## <a name="add-the-sdk-to-a-visual-studio-project"></a>Ajout du kit SDK à un projet Visual Studio

Il est possible d’inclure directement les fichiers générés par AutoRest dans une solution .NET. Toutefois, il est probable que vous souhaitiez inclure le Kit de développement logiciel (SDK) Azure Digital Twins dans plusieurs projets distincts (vos applications clientes, applications Azure Functions, etc.). Pour cette raison, il peut être utile de créer un projet distinct (une bibliothèque de classes .NET) à partir des fichiers générés. Vous pouvez ensuite inclure ce projet de bibliothèque de classes dans plusieurs solutions en tant que référence de projet.

Cette section fournit les instructions à suivre pour générer le kit SDK sous la forme d’une bibliothèque de classes, qui constitue son propre projet et peut être incluse dans d’autres projets. Cette procédure est donnée pour **Visual Studio** (cliquez [ici](https://visualstudio.microsoft.com/downloads/) pour installer la dernière version).

Voici la procédure à suivre :

1. Créez une solution Visual Studio pour une bibliothèque de classes.
2. Utilisez *ADTApi* comme nom du projet.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit pour sélectionner le projet *ADTApi* de la solution générée, puis choisissez *Ajouter > Élément existant…* .
4. Recherchez le dossier dans lequel vous avez généré le kit SDK et sélectionnez les fichiers au niveau racine.
5. Appuyez sur « OK ».
6. Ajoutez un dossier au projet (cliquez avec le bouton droit pour sélectionner le projet dans l’Explorateur de solutions, puis sélectionnez *Ajouter > Nouveau dossier*).
7. Nommez le dossier *Modèles*.
8. Cliquez avec le bouton droit pour sélectionner le dossier *Modèles* dans l’Explorateur de solutions, puis sélectionnez *Ajouter un élément existant…* .
9. Sélectionnez les fichiers dans le dossier *Modèles* du kit SDK généré et appuyez sur « OK ».

Les références suivantes sont nécessaires dans votre projet pour générer le kit SDK :
* `Microsoft.Rest.ClientRuntime`
* `Microsoft.Rest.ClientRuntime.Azure`

Pour les ajouter, ouvrez *Outils > Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution…* .

1. Dans le volet, vérifiez que l’onglet *Parcourir* est sélectionné.
2. Recherchez *Microsoft.Rest*.
3. Sélectionnez les packages `ClientRuntime` et `ClientRuntime.Azure`, puis ajoutez-les à votre solution.

Vous pouvez maintenant générer le projet et l’inclure comme référence de projet dans toutes les applications Azure Digital Twins que vous écrivez.

## <a name="general-guidelines-for-generated-sdks"></a>Instructions générales pour les kits SDK générés

Cette section contient des informations générales sur le kit SDK généré, ainsi que les instructions à suivre pour l’utiliser.

### <a name="synchronous-and-asynchronous-calls"></a>Appels synchrones et asynchrones

Toutes les fonctions du kit SDK sont disponibles en version synchrone et asynchrone.

### <a name="typed-and-untyped-data"></a>Données typées et non typées

Les appels d’API REST retournent généralement des objets fortement typés. Toutefois, étant donné qu’Azure Digital Twins permet aux utilisateurs de définir leurs propres types personnalisés pour les jumeaux, il n’existe pour de nombreux appels Azure Digital Twins aucun moyen de prédéfinir des données de retour statiques. Au lieu de cela, les API retournent des types de wrappers fortement typés le cas échéant, et les données de jumeaux de type personnalisé se trouvent dans des objets JSON.NET (utilisés partout où le type de données « object » apparaît dans les signatures d’API). Vous pouvez effectuer une conversion de type (transtypage) sur ces objets là où c’est nécessaire dans votre code.

### <a name="error-handling"></a>Gestion des erreurs

Chaque fois qu’une erreur se produit dans le kit SDK (y compris des erreurs HTTP comme 404), ce dernier lève une exception. C’est pourquoi il est important d’encapsuler tous les appels d’API dans des blocs try/catch.

Voici un extrait de code qui tente d’ajouter un jumeau et intercepte les erreurs dans ce processus :

```csharp
try
{
    await client.DigitalTwins.AddAsync(id, initData);
    Console.WriteLine($"Created a twin successfully: {id}");
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
}
```

### <a name="paging"></a>Pagination

AutoRest génère deux types de modèles de pagination pour le kit SDK :
* Une pour toutes les API, à l’exception de l’API de requête
* Une pour l’API de requête

Dans le modèle de pagination hors requête, il existe deux versions de chaque appel :
* Une qui permet d’effectuer l’appel initial (par exemple, `DigitalTwins.ListEdges()`)
* Une qui permet d’obtenir les pages suivantes. Ces appels ont un suffixe « Next » (par exemple, `DigitalTwins.ListEdgesNext()`).

Voici un extrait de code qui montre comment récupérer une liste paginée de relations sortantes à partir d’Azure Digital Twins :
```csharp
try
{
    // List to hold the results in
    List<object> relList = new List<object>();
    // Enumerate the IPage object returned to get the results
    // ListAsync will throw if an error occurs
    IPage<object> relPage = await client.DigitalTwins.ListEdgesAsync(id);
    relList.AddRange(relPage);
    // If there are more pages, the NextPageLink in the page is set
    while (relPage.NextPageLink != null)
    {
        // Get more pages...
        relPage = await client.DigitalTwins.ListEdgesNextAsync(relPage.NextPageLink);
        relList.AddRange(relPage);
    }
    Console.WriteLine($"Found {relList.Count} relationships on {id}");
    // Do something with each object found
    // As relationships are custom types, they are JSON.Net types
    foreach (JObject r in relList)
    {
        string relId = r.Value<string>("$edgeId");
        string relName = r.Value<string>("$relationship");
        Console.WriteLine($"Found relationship {relId} from {id}");
    }
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error retrieving relationships on {id}: {e.Response.StatusCode}");
}
```

Le deuxième modèle n’est généré que pour l’API de requête. Il utilise explicitement `continuationToken`.

Voici un exemple utilisant ce modèle :

```csharp
string query = "SELECT * FROM digitaltwins";
string conToken = null; // continuation token from the query
int page = 0;
try
{
    // Repeat the query while there are pages
    do
    {
        QuerySpecification spec = new QuerySpecification(query, conToken);
        QueryResult qr = await client.Query.QueryTwinsAsync(spec);
        page++;
        Console.WriteLine($"== Query results page {page}:");
        if (qr.Items != null)
        {
            // Query returns are JObjects
            foreach(JObject o in qr.Items)
            {
                string twinId = o.Value<string>("$dtId");
                Console.WriteLine($"  Found {twinId}");
            }
        }
        Console.WriteLine($"== End query results page {page}");
        conToken = qr.ContinuationToken;
    } while (conToken != null);
} catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error in twin query: ${e.Response.StatusCode}");
}
```

## <a name="next-steps"></a>Étapes suivantes

Suivez la procédure de création d’une application cliente dans laquelle vous pouvez utiliser votre kit SDK :
* [*Tutoriel : Coder une application cliente*](tutorial-code.md)