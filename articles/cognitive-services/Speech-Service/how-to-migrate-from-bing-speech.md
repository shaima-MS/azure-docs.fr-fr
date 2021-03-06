---
title: Migrer de la Reconnaissance vocale Bing vers le service Speech
titleSuffix: Azure Cognitive Services
description: Découvrez comment migrer un abonnement Reconnaissance vocale Bing existant vers le service Speech d’Azure Cognitive Services.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: nitinme
ms.openlocfilehash: 7b78bdb070cdf1364fe7fbdc75f175be7ce145ff
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656451"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrer de la Reconnaissance vocale Bing vers le service Speech

Référez-vous à cet article pour migrer vos applications de l’API Reconnaissance vocale Bing vers le service Speech.

Cet article décrit les différences qui existent entre les API Reconnaissance vocale Bing et le service Speech, et suggère des stratégies pour la migration de vos applications. Votre clé d’abonnement API Reconnaissance vocale Bing ne fonctionnera pas avec le service Speech. Vous avez besoin d’un nouvel abonnement au service Speech.

Une clé unique d’abonnement au service Speech permet d’accéder aux fonctionnalités suivantes. Elles sont mesurées séparément pour que seules les fonctions que vous utilisez vous soient facturées.

* [Reconnaissance vocale](speech-to-text.md)
* [Reconnaissance vocale personnalisée](https://cris.ai)
* [Synthèse vocale](text-to-speech.md)
* [Voix de synthèse vocale personnalisées](how-to-customize-voice-font.md)
* [Traduction vocale](speech-translation.md) (n’inclut pas la [traduction de texte](../translator/translator-info-overview.md))

Le [Kit de développement logiciel (SDK) Speech](speech-sdk.md) constitue un remplacement fonctionnel des bibliothèques clientes de Reconnaissance vocale Bing, mais utilise une API différente.

## <a name="comparison-of-features"></a>Comparaison des fonctionnalités

Le service Speech est très similaire à la Reconnaissance vocale Bing, à l’exception des différences suivantes.

| Fonctionnalité | Reconnaissance vocale Bing | Service Speech | Détails |
|--|--|--|--|
| Kit de développement logiciel (SDK) C# | :heavy_check_mark: | :heavy_check_mark: | Le service Speech prend en charge Windows 10, la plateforme Windows universelle (UWP) et .NET Standard 2.0. |
| KIT DE DÉVELOPPEMENT LOGICIEL C++ | :heavy_minus_sign: | :heavy_check_mark: | Le service Speech prend en charge Windows et Linux. |
| Kit de développement logiciel (SDK) Java | :heavy_check_mark: | :heavy_check_mark: | Le service Speech prend en charge Android et Speech Devices. |
| Reconnaissance vocale continue | 10 minutes | Illimitée (avec le Kit de développement logiciel) | Les protocoles WebSocket du service Speech et de la Reconnaissance vocale Bing prennent en charge jusqu’à 10 minutes par appel. Toutefois, le Kit de développement logiciel (SDK) Speech reconnecte ou déconnecte à l’issue du délai d’expiration. |
| Résultats partiels ou intermédiaires | :heavy_check_mark: | :heavy_check_mark: | Avec le protocole WebSockets ou le Kit de développement logiciel (SDK). |
| Modèles vocaux personnalisés | :heavy_check_mark: | :heavy_check_mark: | La Reconnaissance vocale Bing nécessite un abonnement Custom Speech séparé. |
| Polices de la voix personnalisées | :heavy_check_mark: | :heavy_check_mark: | La Reconnaissance vocale Bing nécessite un abonnement Custom Voice séparé. |
| Voix 24 kHz | :heavy_minus_sign: | :heavy_check_mark: |
| Reconnaissance de l’intention vocale | Nécessite un appel d’API LUIS séparé | Intégrée (avec Kit de développement logiciel (SDK)) | Vous pouvez utiliser une clé LUIS avec le service Speech. |
| Reconnaissance de l’intention simple | :heavy_minus_sign: | :heavy_check_mark: |
| Transcription par lots de fichiers audio longs | :heavy_minus_sign: | :heavy_check_mark: |
| Mode de reconnaissance | Manuel via URI de point de terminaison | Automatique | Le mode de reconnaissance n’est pas disponible dans le service Speech. |
| Lieu du point de terminaison | Global | Zones géographiques | Les points de terminaison régionaux améliorent la latence. |
| API REST | :heavy_check_mark: | :heavy_check_mark: | Les API REST du service Speech sont compatibles avec la Reconnaissance vocale Bing (point de terminaison différent). Les API REST prennent en charge les fonctionnalités de synthèse vocale et de reconnaissance vocale limitée. |
| Protocoles WebSocket | :heavy_check_mark: | :heavy_check_mark: | L’API WebSocket du service Speech est compatible avec la Reconnaissance vocale Bing (point de terminaison différent). Migrez vers le Kit de développement logiciel (SDK) Speech si possible afin de simplifier votre code. |
| Appels d’API de service à service | :heavy_check_mark: | :heavy_minus_sign: | Fournis dans la Reconnaissance vocale Bing via la bibliothèque de services C#. |
| Kit de développement logiciel (SDK) open source | :heavy_check_mark: | :heavy_minus_sign: |

Le service Speech utilise un modèle tarifaire basé sur un temps donné (plutôt que sur les transactions). Pour plus de détails, consultez les [tarifs du service Speech](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="migration-strategies"></a>Stratégies de migration

Si votre organisation ou vous-même disposez d’applications en développement ou en production qui utilisent l’API Reconnaissance vocale Bing, vous devez les mettre à jour pour qu’elles utilisent le service Speech dès que possible. Pour accéder aux kits SDK, aux exemples de code et aux tutoriels disponibles, consultez la [documentation du service Speech](index.yml).

Les [API REST](rest-apis.md) du service Speech sont compatibles avec les API Reconnaissance vocale Bing. Si vous utilisez les API REST Reconnaissance vocale Bing, vous devez uniquement changer le point de terminaison REST et basculer vers une clé d’abonnement Speech.

Les protocoles WebSocket du service Speech sont également compatibles avec ceux utilisés par la Reconnaissance vocale Bing. Pour tout nouveau développement, nous vous recommandons d’utiliser le kit de développement logiciel (SDK) Speech plutôt que WebSockets. Il est judicieux d’également migrer le code existant vers le kit de développement logiciel (SDK). Cependant, comme avec les API REST, le code existant utilisant la Reconnaissance vocale Bing via des WebSockets nécessite uniquement une modification du point de terminaison et une clé mise à jour.

Si vous utilisez une bibliothèque de client Reconnaissance vocale Bing pour un langage de programmation spécifique, la migration vers le [Kit de développement logiciel (SDK) Speech](speech-sdk.md) nécessite l’apport de modifications à votre application, car l’API est différente. Le kit de développement logiciel (SDK) Speech peut simplifier votre code tout en vous donnant accès à de nouvelles fonctionnalités. Le kit SDK Speech est disponible dans un large éventail de langages de programmation. Les API sont similaires sur toutes les plateformes, et facilitent le développement multi-plateforme.

Le service Speech n’offre pas de point de terminaison global. Vous devez déterminer si votre application fonctionne efficacement lorsqu’elle utilise un point de terminaison régional unique pour tout son trafic. Autrement, utilisez la géolocalisation pour déterminer le point de terminaison plus efficace. Vous devez disposer d’un abonnement Speech distinct pour chaque région.

Si votre application utilise des connexions de longue durée et ne peut pas utiliser un kit SDK disponible, vous pouvez utiliser une connexion WebSockets. Gérez la limite de délai d’expiration de 10 minutes en vous reconnectant aux moments appropriés.

Pour prendre en main du kit de développement logiciel (SDK) Speech :

1. Téléchargez le [Kit de développement logiciel (SDK) Speech](speech-sdk.md).
1. Parcourez les [guides de démarrage rapide](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet) et les [tutoriels](how-to-recognize-intents-from-speech-csharp.md) relatifs au service Speech. Consultez également les [exemples de code](samples.md) pour vous familiariser avec les nouvelles API.
1. Mettez à jour votre application pour utiliser le service Speech.

## <a name="support"></a>Support

Les clients de Reconnaissance vocale Bing doivent contacter le service clientèle en ouvrant un [ticket de support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Vous pouvez également nous contacter si vous avez besoin d’un [plan de support technique](https://azure.microsoft.com/support/plans/).

Pour la prise en charge du service Speech, du SDK et de l’API, consultez la [page de support](support.md) du service Speech.

## <a name="next-steps"></a>Étapes suivantes

* [Essayer gratuitement le service Speech](get-started.md)
* [Démarrage rapide : Reconnaissance vocale dans une application UWP à l’aide du kit SDK Speech](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Voir aussi
* [Notes de publication du service Speech](releasenotes.md)
* [Qu’est-ce que le Service Speech](overview.md)
* [Documentation sur le SDK Speech et sur le service Speech](speech-sdk.md#get-the-speech-sdk)
