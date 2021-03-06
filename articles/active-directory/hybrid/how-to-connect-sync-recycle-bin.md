---
title: 'Synchronisation d’Azure AD Connect : Activer la Corbeille AD | Microsoft Docs'
description: Cette rubrique recommande l’utilisation de la fonctionnalité Corbeille d’AD avec Azure AD Connect.
services: active-directory
keywords: Corbeille AD, suppression accidentelle, ancre source
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed71e53a8cedc2907ac06dd75f11f9c762a78772
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85357203"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Synchronisation d’Azure AD Connect : Activer la Corbeille Active Directory
Il est recommandé d’activer la fonctionnalité Corbeille d’AD pour vos annuaires Active Directory locaux, qui sont synchronisés avec Azure AD. 

Si vous avez supprimé accidentellement un objet utilisateur AD local et le restaurez à l’aide de cette fonctionnalité, Azure AD restaure l’objet utilisateur Azure AD correspondant.  Pour plus d’informations sur la fonctionnalité Corbeille d’AD, reportez-vous à l’article [Présentation du scénario pour la restauration d’objets Active Directory supprimés](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Avantages de l’activation de la Corbeille d’AD
Cette fonctionnalité vous aide à restaurer des objets utilisateur Azure AD en procédant comme suit :

* Si vous avez supprimé accidentellement un objet utilisateur AD local, l’objet utilisateur Azure AD correspondant sera supprimé lors du prochain cycle de synchronisation. Par défaut, Azure AD conserve les objets utilisateur Azure AD supprimés en état de suppression temporaire pendant 30 jours.

* Si vous avez activé la fonctionnalité Corbeille d’AD locale, vous pouvez restaurer l’objet utilisateur supprimé localement sans modifier sa valeur d’ancre source. Lorsque l’objet utilisateur AD local récupéré est synchronisé avec Azure AD, Azure AD est restaure l’objet utilisateur Azure AD supprimé temporairement correspondant. Pour plus d’informations sur l’attribut Ancre source, consultez l’article [Azure AD Connect : Principes de conception](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Si vous n’avez pas activé la fonctionnalité Corbeille d’AD locale, vous pourriez avoir à créer un objet d’utilisateur AD pour remplacer l’objet supprimé. Si le service de synchronisation Azure AD Connect est configuré pour utiliser un attribut AD généré par le système (comme ObjectGuid) comme attribut d’ancre source, l’objet utilisateur AD nouvellement créé n’aura pas la même valeur d’ancre source que l’objet utilisateur AD supprimé. Lorsque l’objet utilisateur AD nouvellement créé est synchronisé avec Azure AD, Azure AD crée un nouvel objet d’utilisateur Azure AD au lieu de restaurer l’objet utilisateur Azure AD supprimé temporairement.

> [!NOTE]
> Par défaut, AD Azure conserve les objets d’utilisateur Azure AD dans un état supprimé temporairement pendant 30 jours avant suppression définitive. Toutefois, les administrateurs peuvent accélérer la suppression de ces objets. Une fois que les objets sont définitivement supprimés, ils ne peuvent plus être restaurés, même si la fonctionnalité Corbeille AD locale est activée.

## <a name="next-steps"></a>Étapes suivantes
**Rubriques de présentation**

* [Synchronisation Azure AD Connect : Comprendre et personnaliser la synchronisation](how-to-connect-sync-whatis.md)

* [Intégration des identités locales dans Azure Active Directory](whatis-hybrid-identity.md)
