---
title: 'Azure ExpressRoute : Contrôles de sécurité'
description: Découvrez les contrôles de sécurité dans Azure ExpressRoute, qui sont des qualités ou des fonctionnalités qui permettent de prévenir, de détecter et de répondre aux failles de sécurité.
services: expressroute
ms.service: expressroute
author: msmbaldwin
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 6a4589d1aa768548f2ae9ffa01f289c823c0b2e5
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192418"
---
# <a name="security-controls-for-azure-expressroute"></a>Contrôles de sécurité pour Azure ExpressRoute

Cet article décrit les contrôles de sécurité intégrés dans Azure ExpressRoute.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Réseau

| Contrôle de sécurité | Oui/Non | Notes |
|---|---|--|
| Prise en charge du point de terminaison de service| N/A |  |
| Prise en charge de l’injection de réseau virtuel| N/A | |
| Prise en charge de l’isolement réseau et de l’installation de pare-feu| Oui | Chaque client est contenu dans son propre domaine de routage et acheminé vers son propre réseau virtuel |
| Prise en charge du tunneling forcé| N/A | Via le protocole de passerelle frontière (BGP). |

## <a name="monitoring--logging"></a>Supervision et journalisation

| Contrôle de sécurité | Oui/Non | Notes|
|---|---|--|
| Prise en charge de la supervision Azure (Log analytics, App insights, etc.)| Oui | Consultez [Supervision, métriques et alertes ExpressRoute](expressroute-monitoring-metrics-alerts.md).|
| Journalisation et audit du plan de gestion et de contrôle| Oui |  |
| Journalisation et audit du plan de données| Non |   |

## <a name="identity"></a>Identité

| Contrôle de sécurité | Oui/Non | Notes|
|---|---|--|
| Authentification| Oui | Compte de service de passerelle pour Microsoft (GWM) (contrôleur) ; Accès JIT (juste-à-temps) pour le développement et l’exploitation. |
| Autorisation|  Oui |Compte de service de passerelle pour Microsoft (GWM) (contrôleur) ; Accès JIT (juste-à-temps) pour le développement et l’exploitation. |

## <a name="data-protection"></a>Protection de données

| Contrôle de sécurité | Oui/Non | Notes |
|---|---|--|
| Chiffrement côté serveur au repos : Clés managées par Microsoft |  N/A | ExpressRoute ne stocke pas les données client. |
| Chiffrement côté serveur au repos : clés gérées par le client (BYOK) | N/A |  |
| Chiffrement au niveau des colonnes (Azure Data Services)| N/A | |
| Chiffrement en transit (comme ExpressRoute, de réseau virtuel et de réseau virtuel à réseau virtuel)| Non | |
| Appels d’API chiffrés| Oui | Via [Azure Resource Manager](../azure-resource-manager/index.yml) et HTTPS. |


## <a name="configuration-management"></a>Gestion des configurations

| Contrôle de sécurité | Oui/Non | Notes|
|---|---|--|
| Prise en charge de la gestion de la configuration (gestion de version de la configuration, etc.)| Oui | Via le fournisseur de ressources réseau (NRP). |

## <a name="next-steps"></a>Étapes suivantes

- Apprenez-en plus sur les [contrôles de sécurité intégrés des services Azure](../security/fundamentals/security-controls.md).