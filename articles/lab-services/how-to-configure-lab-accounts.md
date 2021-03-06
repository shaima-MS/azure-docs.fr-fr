---
title: Configurer l’arrêt automatique des machines virtuelles dans Azure Lab Services
description: Cet article explique comment configurer l’arrêt automatique des machines virtuelles dans le compte lab.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 783e3b310b3ad06f637453f0e1b11f6a78beec3a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445812"
---
# <a name="configure-automatic-shutdown-of-vms-on-disconnect-setting-for-a-lab-account"></a>Configurer l’arrêt automatique des machines virtuelles lors de la déconnexion d’un compte lab
Vous pouvez activer ou désactiver l’arrêt automatique des machines virtuelles Lab Windows (modèle ou étudiant) après la déconnexion d’une connexion Bureau à distance. Vous pouvez également spécifier la durée pendant laquelle le service Azure Lab Services doit attendre que l’utilisateur se reconnecte avant de s’arrêter automatiquement.

## <a name="enable-automatic-shutdown"></a>Activer l’arrêt automatique

1. Dans la page **Compte lab**, sélectionnez **Paramètres lab** dans le menu de gauche.
2. Sélectionnez l’option **Arrêter automatiquement les machines virtuelles quand les utilisateurs se déconnectent**.
3. Spécifier la durée pendant laquelle le service Azure Lab Services doit attendre que l’utilisateur se reconnecte avant d’arrêter automatiquement les machines virtuelles.

    ![Paramètre d’arrêt automatique au niveau du compte Lab](./media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

    Ce paramètre s’applique à tous les laboratoires créés dans le compte Lab. Un créateur de laboratoire (enseignant) peut remplacer ce paramètre au niveau du laboratoire. La modification apportée à ce paramètre au niveau du compte Lab n’affecte que les laboratoires créés après la modification.

    Pour désactiver ce paramètre, désactivez la case à cocher de l’option **Arrêter automatiquement les machines virtuelles lorsque les utilisateurs se déconnectent** sur cette page. 

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur la façon dont un propriétaire de laboratoire peut configurer ou remplacer ce paramètre au niveau du laboratoire, consultez [cet article](how-to-enable-shutdown-disconnect.md)
