---
title: Fichier include
description: Fichier include
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/14/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f6175a797b14077cafacaca1f2fd48f36e945d9e
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87425100"
---
L’activation de disques partagés est disponible uniquement pour un sous-ensemble de types de disques. Seuls les disques Ultra et les disques SSD Premium peuvent activer des disques partagés. Chaque disque managé pour lequel la fonctionnalité Disques partagés est activée est soumis aux limitations suivantes, selon le type disque :

### <a name="ultra-disks"></a>Disques Ultra

Les disques Ultra ont leur propre liste de limitations sans lien avec les disques partagés. Pour connaître les limitations des disques Ultra, reportez-vous à [Utilisation de disques Ultra Azure](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md).

Quand vous partagez des disques Ultra, ils présentent les limitations supplémentaires suivantes :

- Limités à la prise en charge d’Azure Resource Manager ou des kits SDK. 
- Seuls les disques de base peuvent être utilisés avec certaines versions de la fonctionnalité Cluster de basculement de Windows Server. Pour plus d’informations, consultez [Configuration matérielle requise pour le clustering de basculement et options de stockage](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

Les disques Ultra partagés sont disponibles dans toutes les régions qui prennent en charge les disques Ultra par défaut, et vous n’avez pas besoin de vous inscrire pour pouvoir les utiliser.

### <a name="premium-ssds"></a>SSD Premium

- Actuellement pris en charge dans la région USA Centre-Ouest uniquement.
- Limités à la prise en charge d’Azure Resource Manager ou des kits SDK. 
- Activation possible uniquement sur des disques de données, et non sur des disques de système d’exploitation.
- La mise en cache de l’hôte **ReadOnly** n’est pas disponible pour les disques SSD Premium avec `maxShares>1`.
- Le bursting n’est pas disponible pour les disques SSD Premium avec `maxShares>1`.
- Lorsque vous utilisez des groupes à haute disponibilité et des groupes de machines virtuelles identiques avec des disques partagés Azure, [l’alignement du domaine d’erreur de stockage](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) sur un domaine d’erreur de machine virtuelle n’est pas appliqué pour le disque de données partagé.
- Lorsque des [groupes de placement de proximité](../articles/virtual-machines/windows/proximity-placement-groups.md) sont utilisés, toutes les machines virtuelles qui se partagent un disque doivent faire partie du même groupe de placement de proximité.
- Seuls les disques de base peuvent être utilisés avec certaines versions de la fonctionnalité Cluster de basculement de Windows Server. Pour plus d’informations, consultez [Configuration matérielle requise pour le clustering de basculement et options de stockage](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- La prise en charge de Sauvegarde Azure et d’Azure Site Recovery n’est pas encore disponible.

Si vous souhaitez essayer les disques SSD Premium partagés, [inscrivez-vous pour y accéder](https://aka.ms/AzureSharedDiskGASignUp).