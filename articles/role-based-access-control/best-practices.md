---
title: Meilleures pratiques pour Azure RBAC
description: Meilleures pratiques d'utilisation du contrôle d’accès en fonction du rôle Azure (Azure RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 053e86f3493c7a11a3cbbaad0871e45345697878
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82735332"
---
# <a name="best-practices-for-azure-rbac"></a>Meilleures pratiques pour Azure RBAC

Cet article décrit quelques-unes des meilleures pratiques d'utilisation du contrôle d’accès en fonction du rôle Azure (Azure RBAC). Ces meilleures pratiques sont issues de notre expérience d’Azure RBAC, mais également de celle des clients, comme vous.

## <a name="only-grant-the-access-users-need"></a>Accorder uniquement l’accès nécessaire aux utilisateurs

Avec Azure RBAC, vous pouvez séparer les tâches au sein de votre équipe et accorder aux utilisateurs uniquement les accès nécessaires pour accomplir leur travail. Plutôt que de donner à tous des autorisations illimitées au sein de votre abonnement ou de vos ressources Azure, vous pouvez autoriser uniquement certaines actions sur une étendue donnée.

Lorsque vous planifiez votre stratégie de contrôle d’accès, vous pouvez accorder aux utilisateurs les privilèges minimaux pour effectuer leur travail. Le diagramme suivant illustre un modèle suggéré pour l’utilisation d’Azure RBAC.

![Azure RBAC et privilège minimum](./media/best-practices/rbac-least-privilege.png)

Pour obtenir des informations sur la façon d’ajouter des attributions de rôle, consultez [Ajouter ou supprimer des attributions de rôle Azure à l’aide du portail Azure](role-assignments-portal.md).

## <a name="limit-the-number-of-subscription-owners"></a>Limiter le nombre de propriétaires d’abonnements

Vous devez disposer d'un maximum de trois propriétaires d’abonnement afin de réduire le risque de violation par un propriétaire compromis. Cette recommandation peut être supervisée dans Azure Security Center. Pour obtenir d’autres recommandations relatives aux identités et aux accès dans Security Center, consultez [Recommandations de sécurité - Guide de référence](../security-center/recommendations-reference.md).

## <a name="use-azure-ad-privileged-identity-management"></a>Utiliser Azure AD Privileged Identity Management

Pour protéger des comptes privilégiés contre les cyberattaques malveillantes, vous pouvez utiliser Azure Active Directory Privileged Identity Management (PIM) afin de réduire le temps d’exposition des privilèges et d’augmenter la visibilité quant à leur utilisation via des rapports et des alertes. PIM permet de protéger les comptes privilégiés en fournissant un accès privilégié de type juste-à-temps aux ressources Azure AD et Azure. L’accès peut être lié au laps de temps après lequel les privilèges sont automatiquement révoqués. 

Pour plus d’informations, consultez [Qu’est-ce qu’Azure AD Privileged Identity Management ?](../active-directory/privileged-identity-management/pim-configure.md).

## <a name="next-steps"></a>Étapes suivantes

- [Résoudre les problèmes liés à Azure RBAC](troubleshooting.md)