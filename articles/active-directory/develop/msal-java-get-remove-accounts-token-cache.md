---
title: Obtenir et supprimer des comptes dans le cache de jeton (MSAL4j) | Azure
titleSuffix: Microsoft identity platform
description: Découvrez comment afficher et supprimer des comptes depuis le cache de jeton à l’aide de la bibliothèque d’authentification Microsoft pour Java.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev, devx-track-java
ms.openlocfilehash: 6267c2118b7bac9befb42102d1118faac639bf49
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87312552"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Obtenir et supprimer des comptes du cache de jeton à l’aide de MSAL pour Java

MSAL pour Java fournit un cache de jeton en mémoire par défaut. Le cache de jeton en mémoire dure le temps que dure l’instance de l’application.

## <a name="see-which-accounts-are-in-the-cache"></a>Afficher les comptes qui se trouvent dans le cache

Vous pouvez vérifier quels comptes se trouvent dans le cache en appelant `PublicClientApplication.getAccounts()`, comme indiqué dans l’exemple suivant :

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Supprimer des comptes du cache

Pour supprimer un compte du cache, recherchez le compte qui doit être supprimé, puis appelez `PublicClientApplicatoin.removeAccount()`, comme indiqué dans l’exemple suivant :

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>En savoir plus

Si vous utilisez MSAL pour Java, découvrez la [Sérialisation du cache de jeton personnalisé dans MSAL pour Java](msal-java-token-cache-serialization.md).
