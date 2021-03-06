---
title: 'Démarrage rapide : Appeler votre point de terminaison Recherche personnalisée Bing avec Python | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Utiliser ce guide de démarrage rapide pour commencer à demander des résultats de recherche à partir de votre instance Recherche personnalisée Bing, avec Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-python
ms.openlocfilehash: b29d9921de0226cb8660b3f7e3c6d3e0d4a22754
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852512"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Démarrage rapide : Appeler votre point de terminaison Recherche personnalisée Bing avec Python

Utilisez ce guide de démarrage rapide pour découvrir comment demander des résultats de recherche à partir de votre instance Recherche personnalisée Bing. Bien que cette application est écrite en Python, l’API Recherche personnalisée Bing constitue un service web RESTful compatible avec la plupart des langages de programmation. Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Prérequis

- Une instance Recherche personnalisée Bing. Pour plus d’informations, consultez [Démarrage rapide : Créer votre première instance de Recherche personnalisée Bing](quick-start.md).
- [Python](https://www.python.org/) 2.x ou 3.x.

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Créer et initialiser l’application

- Créez un fichier Python dans votre éditeur ou IDE favori, puis ajoutez les instructions d’importation suivantes. Créez des variables pour votre clé d’abonnement, votre ID de configuration personnalisée et un terme de recherche.

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Envoyer et recevoir une requête de recherche 

1. Construisez l’URL de requête en ajoutant votre terme de recherche au paramètre de requête `q=`, et l’ID de configuration personnalisée de votre instance de recherche au paramètre `customconfig=`. Séparez les paramètres par une esperluette (`&`). Vous pouvez utiliser le point de terminaison global dans le code suivant, ou le point de terminaison de [sous-domaine personnalisé](../../cognitive-services/cognitive-services-custom-subdomains.md) affiché dans le portail Azure pour votre ressource.

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Envoyez la requête à votre instance Recherche personnalisée Bing et affichez les résultats de recherche retournés.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une application web Recherche personnalisée](./tutorials/custom-search-web-page.md)
