---
title: Copier la capture instantanée d'un disque managé vers un abonnement - Exemple CLI, machine virtuelle Windows
description: Exemple de script Azure CLI - Copier (ou déplacer) la capture instantanée d'un disque managé vers un abonnement identique ou différent avec Azure CLI, sur une machine virtuelle Windows
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 05b73bf62bfce456e65fde153379690bc9823e6a
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056735"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli-on-a-windows-vm"></a>Copier la capture instantanée d'un disque managé vers un abonnement identique ou différent avec Azure CLI, sur une machine virtuelle Windows

Ce script copie la capture instantanée d’un disque managé vers un abonnement identique ou différent. Utilisez ce script dans les situations suivantes :

1. Migrez une capture instantanée du stockage Premium (Premium_LRS) vers le stockage Standard (Standard_LRS ou Standard_ZRS) pour réduire vos coûts.
1. Migrez une capture instantanée du stockage localement redondant (Premium_LRS, Standard_LRS) vers un stockage redondant interzone (Standard_ZRS) pour bénéficier d’une fiabilité accure de stockage ZRS.
1. Déplacez une capture instantanée vers un autre abonnement dans la même région pour allonger la rétention de données.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer une capture instantanée dans l’abonnement cible à l’aide de l’ID de la capture instantanée source. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az snapshot show](/cli/azure/snapshot) | Obtient toutes les propriétés d’une capture instantanée en utilisant les propriétés de nom et de groupe de ressources de la capture instantanée. La propriété de l’identifiant est utilisée pour copier la capture instantanée vers un autre abonnement.  |
| [az snapshot create](/cli/azure/snapshot) | Copie une capture instantanée en créant une capture instantanée dans un abonnement différent à l’aide de l’identifiant et du nom de la capture instantanée parente.  |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Vous trouverez des exemples supplémentaires de scripts CLI de machine virtuelle et de disques managés dans la [documentation relative aux machines virtuelles Windows Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
