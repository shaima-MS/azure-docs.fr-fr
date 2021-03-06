---
title: Matériaux couleur
description: Décrit le type de matériau de couleur.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: af33a777d2d6ef53965c2168ac0abee00f59bc50
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021379"
---
# <a name="color-materials"></a>Matériaux couleur

*Les matériaux de couleur* constituent l’un des types de matériaux [pris en charge](../../concepts/materials.md) dans Azure Remote Rendering. Ils sont utilisés pour les [maillages](../../concepts/meshes.md) qui ne doivent recevoir aucun type d’éclairage, mais demeurer plutôt pleinement éclairés à tous moments. Ce peut être le cas pour les matériaux « lumineux », tels que les tableaux de bord de voiture, les ampoules, ou pour les données qui intègrent déjà l’éclairage statique, telles que les modèles obtenus par [photogrammétrie](https://en.wikipedia.org/wiki/Photogrammetry).

Les matériaux de couleur sont plus avantageux à restituer que [les matériaux PBR](pbr-materials.md) en raison de leur modèle d’ombrage plus simple. Ils prennent également en charge différents modes de transparence.

## <a name="common-material-properties"></a>Propriétés de matériau communes

Ces propriétés sont communes à tous les matériaux :

* **albedoColor :** cette couleur est multipliée par d’autres couleurs, telles que les couleurs *albedoMap* ou *:::no-loc text="vertex":::* . Si la propriété *transparency* est activée sur un matériau, le canal alpha est utilisé pour ajuster l’opacité, avec `1` pour entièrement opaque et `0` pour entièrement transparent. Le blanc constitue la valeur par défaut.

  > [!NOTE]
  > Étant donné que les matériaux de couleur ne reflètent pas l’environnement, un matériau de couleur entièrement transparent devient invisible. Il en va différemment pour [les matériaux PBR](pbr-materials.md).

* **albedoMap :** une [texture 2D](../../concepts/textures.md) pour les valeurs albedo par pixel.

* **alphaClipEnabled** et **alphaClipThreshold :** si la valeur de *alphaClipEnabled* est true, tous les pixels dont la valeur alpha albedo est inférieure à *alphaClipThreshold* ne seront pas représentés. Le détourage alpha peut être utilisé, même sans activation de la transparence, et s’avère nettement plus rapide à rendre. Les matériaux avec détourage alpha sont cependant plus lents à rendre que les matériaux entièrement opaques. Par défaut, le détourage alpha est désactivé.

* **textureCoordinateScale** et **textureCoordinateOffset :** l’échelle est multipliée par les coordonnées de la texture UV, le décalage y est ajouté. Peut être utilisée pour étirer et décaler les textures. La valeur par défaut de l’échelle est de (1, 1) et le décalage est de (0, 0).

* **useVertexColor :** si le maillage comprend des couleurs :::no-loc text="vertex"::: et que cette option est activée, la couleur :::no-loc text="vertex"::: des maillages est multipliée par *albedoColor* et *albedoMap*. Par défaut *useVertexColor* est désactivée.

* **isDoubleSided :** si la bilatéralité est définie sur true, les triangles bénéficiant de ce matériau sont rendus même si la caméra regarde vers les faces arrière. Par défaut, cette option est désactivée. Consultez aussi le [ rendu :::no-loc text="Single-sided":::](single-sided-rendering.md).

## <a name="color-material-properties"></a>Propriétés des matériaux de couleur

Les propriétés suivantes sont spécifiques aux matériaux de couleur :

* **vertexMix :** cette valeur comprise entre `0` et `1` précise dans quelles proportions la couleur :::no-loc text="vertex"::: dans un [maillage](../../concepts/meshes.md) contribue à la couleur finale. Avec la valeur par défaut de 1, la couleur :::no-loc text="vertex"::: est multipliée complètement par la couleur albedo. Avec la valeur 0, les couleurs :::no-loc text="vertex"::: sont entièrement ignorées.

* **transparencyMode :** contrairement aux [matériaux PBR](pbr-materials.md), les matériaux de couleur se distinguent par différents modes de transparence :

  1. **Opaque :** le mode par défaut désactive la transparence. Par contre, le détourage alpha est toujours possible et doit être préféré, s’il est suffisant.
  
  1. **AlphaBlended :** ce mode est similaire au mode de transparence pour les matériaux PBR. Il doit être utilisé pour les matériaux transparents, tels que le verre.

  1. **Additive :** ce mode est le mode de transparence le plus simple et le plus avantageux. La contribution du matériau est ajoutée à l’image rendue. Ce mode peut être utilisé pour simuler des objets lumineux (mais toujours transparents), tels que les marqueurs utilisés pour mettre en évidence des objets importants.

## <a name="next-steps"></a>Étapes suivantes

* [Matériaux PBR](pbr-materials.md)
* [Textures](../../concepts/textures.md)
* [Maillages](../../concepts/meshes.md)
