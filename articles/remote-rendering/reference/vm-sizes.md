---
title: Tailles de machine virtuelle
description: Décrit les tailles de machine virtuelle distinctes qui peuvent être allouées.
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.openlocfilehash: e8e439a055b71ed291573965c561ee31610e3ed4
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121610"
---
# <a name="vm-sizes"></a>Tailles de machine virtuelle

Le service de rendu peut opérer sur deux différents types de machines dans Azure, appelés `Standard` et `Premium`.

## <a name="polygon-limits"></a>Limites de polygones

Il existe une limite inconditionnelle de **20 millions de polygones** pour la taille de machine virtuelle `Standard`. Cette limitation n’existe pas pour la taille `Premium`.

Quand le convertisseur sur une machine virtuelle standard atteint cette limitation, il bascule le rendu sur un damier d’arrière-plan :

![Damier](media/checkerboard.png)

## <a name="allocate-the-vm"></a>Allouer la machine virtuelle

Le type de machine virtuelle souhaité doit être spécifié au moment de l’initialisation de la session de rendu. Il ne peut pas être modifié pendant une session en cours d’exécution. Les exemples de code suivants illustrent l’emplacement où la taille de machine virtuelle doit être spécifiée :

```cs
async void CreateRenderingSession(AzureFrontend frontend)
{
    RenderingSessionCreationParams sessionCreationParams = new RenderingSessionCreationParams();
    sessionCreationParams.Size = RenderingSessionVmSize.Standard; // or  RenderingSessionVmSize.Premium

    AzureSession session = await frontend.CreateNewRenderingSessionAsync(sessionCreationParams).AsTask();
}
```

```cpp
void CreateRenderingSession(ApiHandle<AzureFrontend> frontend)
{
    RenderingSessionCreationParams sessionCreationParams;
    sessionCreationParams.Size = RenderingSessionVmSize::Standard; // or  RenderingSessionVmSize::Premium

    if (auto createSessionAsync = frontend->CreateNewRenderingSessionAsync(sessionCreationParams))
    {
        // ...
    }
}
```

Pour les [exemples de scripts PowerShell](../samples/powershell-example-scripts.md), la taille de la machine virtuelle doit être spécifiée dans le fichier `arrconfig.json` :

```json
{
  "accountSettings": {
    ...
  },
  "renderingSessionSettings": {
    "vmSize": "<standard or premium>",
    ...
  },
```

### <a name="how-the-renderer-evaluates-the-number-of-polygons"></a>Comment le convertisseur évalue-t-il le nombre de polygones ?

Le nombre de polygones pris en compte pour le test de limitation est le nombre de polygones qui sont réellement transmis au convertisseur. Cette géométrie correspond généralement à la somme de tous les modèles instanciés, mais il existe également des exceptions. La géométrie suivante n’est **pas incluse** :
* Instances de modèle chargées situées entièrement à l’extérieur du tronc d’affichage
* Modèles ou parties de modèle basculés en invisible, à l’aide du [composant de remplacement d’état hiérarchique](../overview/features/override-hierarchical-state.md)

Par conséquent, il est possible d’écrire une application qui cible la taille `standard` et qui charge plusieurs modèles avec un nombre de polygones proche de la limite pour chaque modèle unique. Quand l’application n’affiche qu’un seul modèle à la fois, le damier n’est pas déclenché.

### <a name="how-to-determine-the-number-of-polygons"></a>Comment déterminer le nombre de polygones ?

Il existe deux façons de déterminer le nombre de polygones d’un modèle ou d’une scène qui contribuent à la limite budgétaire de la taille de machine virtuelle `standard` :
* Du côté conversion du modèle, récupérez le [fichier json de sortie de conversion](../how-tos/conversion/get-information.md) et vérifiez l’entrée `numFaces` dans la [section *inputStatistics*](../how-tos/conversion/get-information.md#the-inputstatistics-section).
* Si votre application traite du contenu dynamique, le nombre de polygones rendus peut être interrogé de manière dynamique pendant l’exécution. Utilisez une [requête d’évaluation des performances](../overview/features/performance-queries.md#performance-assessment-queries) et recherchez le membre `polygonsRendered` dans le struct `FrameStatistics`. Le champ `polygonsRendered` est défini sur `bad` lorsque le convertisseur atteint la limite du polygone. Le damier d’arrière-plan apparaît toujours en fondu avec un certain délai, afin de garantir qu’une action utilisateur puisse être effectuée après cette requête asynchrone. L’action utilisateur peut par exemple masquer ou supprimer des instances de modèle.

## <a name="pricing"></a>Tarifs

Pour obtenir une description détaillée des tarifs pour chaque type de machine virtuelle, consultez la page [Tarification Remote Rendering](https://azure.microsoft.com/pricing/details/remote-rendering).

## <a name="next-steps"></a>Étapes suivantes
* [Exemples de scripts PowerShell](../samples/powershell-example-scripts.md)
* [Conversion de modèle](../how-tos/conversion/model-conversion.md)

