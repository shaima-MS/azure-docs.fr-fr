---
title: Configurer une connexion à une source de données à l’aide d’une identité managée (préversion)
titleSuffix: Azure Cognitive Search
description: Découvrez comment configurer une connexion d’indexeur à une source de données à l’aide d’une identité managée (préversion).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 6b07236fd639c9878c59523f78de5215b173ffc6
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88553162"
---
# <a name="set-up-an-indexer-connection-to-a-data-source-using-a-managed-identity-preview"></a>Configurer une connexion d’indexeur à une source de données à l’aide d’une identité managée (préversion)

> [!IMPORTANT] 
> La prise en charge de la configuration d’une connexion à une source de données à l’aide d’une identité managée fait actuellement l’objet d’une préversion publique. Les fonctionnalités en préversion sont fournies sans contrat de niveau de service et ne sont pas recommandées pour les charges de travail de production.

Un [indexeur](search-indexer-overview.md) dans Recherche cognitive Azure est un robot qui permet d’extraire des données de votre source de données vers Recherche cognitive Azure. Un indexeur obtient une connexion à la source de données à partir de l’objet source de données que vous créez. L’objet source de données comprend généralement les informations d’identification de la source de données cible. Par exemple, l’objet source de données peut inclure une clé de compte Stockage Azure si vous souhaitez indexer les données d’un conteneur de stockage blob.

Dans de nombreux cas, le fait de fournir des informations d’identification directement dans l’objet source de données n’est pas un problème, mais quelques difficultés peuvent se présenter :
* Comment conserver les informations d’identification en toute sécurité dans mon code qui crée l’objet source des données ?
* Si la clé ou le mot de passe de mon compte sont compromis et que je dois les modifier, je dois mettre à jour mes objets sources de données avec la nouvelle clé ou le nouveau mot de passe du compte afin que mon indexeur puisse de nouveau se connecter à la source de données.

Ces problèmes peuvent être résolus en configurant votre connexion à l’aide d’une identité managée.

## <a name="using-managed-identities"></a>Utilisation des identités managées

[Identités managées](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) est une fonctionnalité qui fournit aux services Azure une identité gérée automatiquement dans Azure Active Directory (Azure AD). Vous pouvez utiliser cette fonctionnalité dans Recherche cognitive Azure pour créer un objet source de données doté d’une chaîne de connexion qui n’inclut aucune information d’identification. Au lieu de cela, votre service de recherche disposera de l’accès à la source de données via le contrôle d’accès en fonction du rôle (RBAC).

En configurant une source de données à l’aide d’une identité managée, vous pouvez modifier vos informations d’identification de source de données le cas échéant : vos indexeurs pourront toujours se connecter à la source de données. Vous pouvez également créer des objets sources de données dans votre code sans avoir à inclure de clé de compte ni à utiliser Key Vault pour récupérer une clé de compte.

## <a name="limitations"></a>Limites

Les sources de données suivantes prennent en charge la configuration d’une connexion d’indexeur à l’aide d’identités managées. 

* [Stockage Blob Azure, Azure Data Lake Storage Gen2 (préversion), Stockage Table Azure](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)

Actuellement, les fonctionnalités suivantes ne prennent pas en charge l’utilisation des identités managées pour configurer la connexion :
* Base de connaissances
* Compétences personnalisées
 
## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur la configuration d’une connexion d’indexeur à l’aide d’identités managées :

* [Stockage Blob Azure, Azure Data Lake Storage Gen2 (préversion), Stockage Table Azure](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)