---
title: Déplacer des machines virtuelles Azure vers un nouveau groupe d’abonnements ou de ressources
description: Utilisez Azure Resource Manager pour déplacer des machines virtuelles vers un nouveau groupe de ressources ou abonnement.
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: e812f2cee44fc48dccbd8ab66a3343e087790803
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063094"
---
# <a name="move-guidance-for-virtual-machines"></a>Conseils pour le déplacement de machines virtuelles

Cet article décrit les scénarios actuellement non pris en charge et les étapes pour déplacer des machines virtuelles avec sauvegarde.

## <a name="scenarios-not-supported"></a>Scénarios non pris en charge

Les scénarios suivants ne sont pas encore pris en charge :

* Les disques managés dans les zones de disponibilité ne peuvent pas être déplacés vers un autre abonnement.
* Les groupes de machines virtuelles identiques avec un équilibreur de charge de référence SKU Standard ou avec une adresse IP publique de référence SKU Standard ne peuvent pas être déplacés.
* Les machines virtuelles créées à partir de ressources Place de marché et auxquelles des plans sont associés ne peuvent pas être déplacées entre des abonnements. Annulez l'approvisionnement de la machine virtuelle dans l’abonnement actuel, puis redéployez-la dans le nouvel abonnement.
* Les machines virtuelles d'un réseau virtuel existant ne peuvent pas être déplacées vers un nouvel abonnement si vous ne déplacez pas toutes les ressources dans le réseau virtuel.
* Les machines virtuelles et les groupes de machines virtuelles identiques basse priorité ne peuvent pas être déplacés entre des groupes de ressources ou abonnements.
* Les machines virtuelles d’un groupe à haute disponibilité ne peuvent pas être déplacées individuellement.

## <a name="azure-disk-encryption"></a>Azure Disk Encryption

Vous ne pouvez pas déplacer une machine virtuelle intégrée à un coffre de clés pour implémenter [Azure Disk Encryption pour les machines virtuelles Linux ](../../../virtual-machines/linux/disk-encryption-overview.md) ou [Azure Disk Encryption pour les machines virtuelles Windows](../../../virtual-machines/windows/disk-encryption-overview.md). Pour déplacer la machine virtuelle, vous devez désactiver le chiffrement.

```azurecli-interactive
az vm encryption disable --resource-group demoRG --name myVm1
```

```azurepowershell-interactive
Disable-AzVMDiskEncryption -ResourceGroupName demoRG -VMName myVm1
```

## <a name="virtual-machines-with-azure-backup"></a>Machines virtuelles avec Sauvegarde Azure

Pour déplacer des machines virtuelles configurées avec le service Sauvegarde Azure, vous devez supprimer les points de restauration du coffre.

Si la [suppression réversible](../../../backup/backup-azure-security-feature-cloud.md) est activée pour votre machine virtuelle, vous ne pouvez pas déplacer la machine virtuelle alors que ces points de restauration sont conservés. Vous pouvez [désactiver la suppression réversible](../../../backup/backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete) ou attendre 14 jours après la suppression des points de restauration.

### <a name="portal"></a>Portail

1. Interrompez temporairement la sauvegarde et conservez les données de sauvegarde.
2. Pour déplacer des machines virtuelles configurées avec Sauvegarde Azure, effectuez les étapes suivantes :

   1. Recherchez l’emplacement de votre machine virtuelle.
   2. Recherchez un groupe de ressources dont le modèle de nommage est le suivant : `AzureBackupRG_<location of your VM>_1`. Par exemple, *AzureBackupRG_westus2_1*.
   3. Dans le Portail Azure, cochez la case **Afficher les types masqués**.
   4. Recherchez la ressource de type **Microsoft. Microsoft.Compute/restorePointCollections** dont le modèle de nommage est `AzureBackup_<name of your VM that you're trying to move>_###########`.
   5. Supprimez cette ressource. Cette opération supprime uniquement les points de récupération instantanée, et non les données sauvegardées dans le coffre.
   6. Une fois l’opération de suppression terminée, vous pouvez déplacer votre machine virtuelle.

3. Déplacez la machine virtuelle vers le groupe de ressources cible.
4. Reprenez la sauvegarde.

### <a name="powershell"></a>PowerShell

* Recherchez l’emplacement de votre machine virtuelle.
* Recherchez un groupe de ressources dont le modèle de nommage est le suivant : `AzureBackupRG_<location of your VM>_1`, par exemple, AzureBackupRG_westus2_1
* Si vous utilisez PowerShell, utilisez l’applet de commande `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1`
* Recherchez la ressource du type `Microsoft.Compute/restorePointCollections` avec le modèle de nommage `AzureBackup_<name of your VM that you're trying to move>_###########`
* Supprimez cette ressource. Cette opération supprime uniquement les points de récupération instantanée, et non les données sauvegardées dans le coffre.

### <a name="azure-cli"></a>Azure CLI

* Recherchez l’emplacement de votre machine virtuelle.
* Recherchez un groupe de ressources dont le modèle de nommage est le suivant : `AzureBackupRG_<location of your VM>_1`, par exemple, AzureBackupRG_westus2_1
* Si vous utilisez l’interface CLI, utilisez `az resource list -g AzureBackupRG_<location of your VM>_1`
* Recherchez la ressource du type `Microsoft.Compute/restorePointCollections` avec le modèle de nommage `AzureBackup_<name of your VM that you're trying to move>_###########`
* Supprimez cette ressource. Cette opération supprime uniquement les points de récupération instantanée, et non les données sauvegardées dans le coffre.

## <a name="next-steps"></a>Étapes suivantes

* Pour obtenir les commandes permettant de déplacer des ressources, consultez [Déplacer des ressources vers un nouveau groupe de ressources ou un nouvel abonnement](../move-resource-group-and-subscription.md).

* Pour plus d’informations sur le déplacement des coffres Recovery Services pour la sauvegarde, consultez [Limitations de Recovery Services](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).
