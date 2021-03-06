---
title: Reflets du ciel
description: Décrit comment configurer des cartes d’environnement pour les reflets du ciel
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: be3dc2b113cb21c2dfb54a29e7f426e0d925c6d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83759113"
---
# <a name="sky-reflections"></a>Reflets du ciel

Dans Azure Remote Rendering, une texture de ciel est utilisée pour éclairer les objets de façon réaliste. Dans les applications de réalité augmentée, cette texture doit ressembler à votre environnement réel, pour que les objets paraissent convaincants. Cet article explique comment modifier la texture du ciel.

> [!NOTE]
> La texture du ciel est également appelée *carte d’environnement*. Ces termes sont utilisés ici indifféremment.

## <a name="object-lighting"></a>Éclairage des objets

Azure Remote Rendering utilise le *rendu physique réaliste* (PBR) pour les calculs d’éclairage réaliste. Bien que vous puissiez ajouter des [sources de lumière](lights.md) à votre scène, l’utilisation d’une bonne texture de ciel produit le meilleur des effets.

Les images ci-dessous montrent les résultats de l’éclairage de différentes surfaces, obtenus uniquement avec une texture de ciel :

| Roughness  | 0                                        | 0,25                                          | 0.5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| Non métallique  | ![Dielectric0](media/dielectric-0.png)   | ![GreenPointPark](media/dielectric-0.25.png)  | ![GreenPointPark](media/dielectric-0.5.png)  | ![GreenPointPark](media/dielectric-0.75.png)  | ![GreenPointPark](media/dielectric-1.png)  |
| Métallique      | ![GreenPointPark](media/metallic-0.png)  | ![GreenPointPark](media/metallic-0.25.png)    | ![GreenPointPark](media/metallic-0.5.png)    | ![GreenPointPark](media/metallic-0.75.png)    | ![GreenPointPark](media/metallic-1.png)    |

Pour plus d’informations sur le modèle d’éclairage, reportez-vous au chapitre sur les [matériaux](../../concepts/materials.md).

> [!IMPORTANT]
> Azure Remote Rendering utilise la texture du ciel uniquement pour les modèles d’éclairage. Il n’effectue pas le rendu du ciel sous forme d’arrière-plan, car les applications de réalité augmentée disposent déjà d’un arrière-plan approprié : le monde réel.

## <a name="changing-the-sky-texture"></a>Modification de la texture du ciel

Pour modifier la carte d’environnement, il suffit de [charger une texture](../../concepts/textures.md) et modifier l’élément `SkyReflectionSettings` de la session :

```cs
LoadTextureAsync _skyTextureLoad = null;
void ChangeEnvironmentMap(AzureSession session)
{
    _skyTextureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap));

    _skyTextureLoad.Completed += (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                try
                {
                    session.Actions.SkyReflectionSettings.SkyReflectionTexture = res.Result;
                }
                catch (RRException exception)
                {
                    System.Console.WriteLine($"Setting sky reflection failed: {exception.Message}");
                }
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
        };
}
```

```cpp
void ChangeEnvironmentMap(ApiHandle<AzureSession> session)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::CubeMap;
    params.TextureUrl = "builtin://VeniceSunset";
    ApiHandle<LoadTextureAsync> skyTextureLoad = *session->Actions()->LoadTextureFromSASAsync(params);

    skyTextureLoad->Completed([&](ApiHandle<LoadTextureAsync> res)
    {
        if (res->IsRanToCompletion())
        {
            ApiHandle<SkyReflectionSettings> settings = *session->Actions()->SkyReflectionSettings();
            settings->SkyReflectionTexture(*res->Result());
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}

```

Notez que la variante `LoadTextureFromSASAsync` est utilisée ci-dessus, car une texture intégrée est chargée. En cas de chargement à partir de [stockages d’objets blob liés](../../how-tos/create-an-account.md#link-storage-accounts), utilisez la variante `LoadTextureAsync`.

## <a name="sky-texture-types"></a>Types de texture de ciel

Vous pouvez utiliser des *[cartes cubiques](https://en.wikipedia.org/wiki/Cube_mapping)* et des *textures 2D* en tant que cartes d’environnement.

Toutes les textures doivent se présenter dans un [format de texture pris en charge](../../concepts/textures.md#supported-texture-formats). Vous n’avez pas besoin de fournir de mipmaps pour les textures de ciel.

### <a name="cube-environment-maps"></a>Cartes d’environnement en cube

Pour référence, voici une carte cubique non wrappée :

![Carte cubique non wrappée](media/Cubemap-example.png)

Utilisez `AzureSession.Actions.LoadTextureAsync`/ `LoadTextureFromSASAsync` avec `TextureType.CubeMap` pour charger les textures de carte cubique.

### <a name="sphere-environment-maps"></a>Cartes d’environnement en sphère

Lors de l’utilisation d’une texture 2D comme carte d’environnement, l’image doit se trouver dans un [espace de coordonnées sphériques](https://en.wikipedia.org/wiki/Spherical_coordinate_system).

![Image de ciel dans des coordonnées sphériques](media/spheremap-example.png)

Utilisez `AzureSession.Actions.LoadTextureAsync` avec `TextureType.Texture2D` pour charger des cartes d’environnement sphériques.

## <a name="built-in-environment-maps"></a>Cartes d’environnement intégrées

Azure Remote Rendering fournit quelques cartes d’environnement intégrées qui sont toujours disponibles. Toutes les cartes d’environnement intégrées sont des cartes cubiques.

|Identificateur                         | Description                                              | Illustration                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Divers éclairages directs, éclairage intérieur lumineux    | ![Autoshop](media/autoshop.png)
|builtin://BoilerRoom               | Intérieur clair, lumineux, plusieurs sources d’éclairage naturel      | ![BoilerRoom](media/boiler-room.png)
|builtin://ColorfulStudio           | Lumières de différentes couleurs et intensités, dans un décor intérieur moyennement éclairé  | ![ColorfulStudio](media/colorful-studio.png)
|builtin://Hangar                   | Éclairage de grande salle, moyennement lumineux                     | ![SmallHangar](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Décor intérieur sombre avec lumière contrastée              | ![IndustrialPipeAndValve](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Pièce claire, ambiance de jour, lumière vive provenant d’une fenêtre     | ![Lebombo](media/lebombo.png)
|builtin://SataraNight              | Sol sombre et nuit noire avec de nombreuses lumières environnantes   | ![SataraNight](media/satara-night.png)
|builtin://SunnyVondelpark          | Lumière du soleil vive et ombre contrastée                      | ![SunnyVondelpark](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Ciel clair avec sol modérément éclairé            | ![Syferfontein](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Soleil et ombre modérément contrastés                         | ![TearsOfSteelBridge](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Soir, lumière de coucher de soleil à l’approche du crépuscule                    | ![VeniceSunset](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Sol dans l’ombre ; tonalités vives, vert luxuriant et lumière blanche | ![WhippleCreekRegionalPark](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Ambiante diurne, lumière vive et sol clair                 | ![WinterRiver](media/winter-river.png)
|builtin://DefaultSky               | Identique à TearsOfSteelBridge                               | ![DefaultSky](media/tears-of-steel-bridge.png)

## <a name="next-steps"></a>Étapes suivantes

* [Lumières](../../overview/features/lights.md)
* [Matériaux](../../concepts/materials.md)
* [Textures](../../concepts/textures.md)
* [L’outil en ligne de commande TexConv](../../resources/tools/tex-conv.md)
