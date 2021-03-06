---
title: Chiffrement côté serveur de Disques managés Azure - Azure CLI
description: Le Stockage Azure protège vos données en les chiffrant au repos avant de les rendre persistantes dans des clusters de stockage. Vous pouvez compter sur des clés gérées par Microsoft pour le chiffrement de vos disques managés, ou bien utiliser des clés gérées par le client pour gérer le chiffrement avec vos propres clés.
author: roygara
ms.date: 07/10/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: e0a1f97cc7467d115ecc8462a301e45f90d73818
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449144"
---
# <a name="server-side-encryption-of-azure-disk-storage"></a>Chiffrement côté serveur de stockage sur disque Azure

Le chiffrement côté serveur protège vos données et vous aide à honorer les engagements de votre entreprise en matière de sécurité et de conformité. SSE chiffre automatiquement vos données stockées sur des disques managés Azure (système d’exploitation et disques de données) au repos par défaut lors de leur conservation dans le cloud. 

Les données dans les disque managés Azure sont chiffrées en toute transparence à l’aide du [chiffrement AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256 bits, un des chiffrements par blocs les plus puissants actuellement disponibles, et sont conformes à la norme FIPS 140-2. Pour plus d’informations sur les modules cryptographiques des disque managés Azure, consultez [API de chiffrement : nouvelle génération](/windows/desktop/seccng/cng-portal)

Le chiffrement côté serveur n’a pas d’impact sur les performances des disques managés et n’entraîne aucun coût supplémentaire. 

## <a name="about-encryption-key-management"></a>À propos de la gestion des clés de chiffrement

Vous pouvez vous appuyer sur les clés gérées par la plateforme pour le chiffrement de votre disque managé, ou vous pouvez gérer le chiffrement en utilisant vos propres clés. Si vous choisissez de gérer le chiffrement avec vos propres clés, vous pouvez spécifier une *clé gérée par le client* à utiliser pour le chiffrement et le déchiffrement de toutes les données dans les disques managés. 

Les sections suivantes décrivent en détail chacune des options de gestion des clés.

### <a name="platform-managed-keys"></a>Clés gérées par la plateforme

Par défaut, les disques managés utilisent des clés de chiffrement gérées par la plateforme. L’ensemble des nouveaux disques managés, des instantanés et des images ainsi que les données écrites sur des disques managés existants sont automatiquement chiffrés au repos avec des clés gérées par les plateformes.

### <a name="customer-managed-keys"></a>Clés gérées par le client

[!INCLUDE [virtual-machines-managed-disks-description-customer-managed-keys](../../../includes/virtual-machines-managed-disks-description-customer-managed-keys.md)]

#### <a name="restrictions"></a>Restrictions

Pour le moment, les clés gérées par le client sont soumises aux restrictions suivantes :

- Si cette fonctionnalité est activée pour votre disque, vous ne pouvez pas la désactiver.
    Pour contourner le problème, vous devez [copier toutes les données](disks-upload-vhd-to-managed-disk-cli.md#copy-a-managed-disk) sur un autre disque managé qui n’utilise pas de clés gérées par le client.
[!INCLUDE [virtual-machines-managed-disks-customer-managed-keys-restrictions](../../../includes/virtual-machines-managed-disks-customer-managed-keys-restrictions.md)]

> [!IMPORTANT]
> Les clés managées par le client s’appuient sur des identités managées pour ressources Azure, une fonctionnalité d’Azure Active Directory (Azure AD). Quand vous configurez des clés gérées par le client, une identité managée est automatiquement affectée à vos ressources à l’arrière plan. Si par la suite vous déplacez l’abonnement, le groupe de ressources ou le disque managé d’un répertoire Azure AD à un autre, l’identité managée associée aux disques gérés n’est pas transférée vers le nouveau locataire, de sorte que les clés gérées par le client peuvent ne plus fonctionner. Pour plus d’informations, consultez [Transfert d’un abonnement entre des répertoires Azure AD](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="encryption-at-host---end-to-end-encryption-for-your-vm-data"></a>Chiffrement à l’hôte : chiffrement de bout en bout pour vos données de machine virtuelle

Lorsque vous activez le chiffrement sur l’hôte, il démarre sur l’hôte de machine virtuelle lui-même, le serveur Azure auquel votre machine virtuelle est allouée. Les données de votre disque temporaire et des caches du système d’exploitation/du disque de données sont stockées sur cet hôte de machine virtuelle. Après l’activation du chiffrement sur l’hôte, toutes ces données sont chiffrées au repos et les flux sont chiffrés dans le service de stockage, où elles sont conservées. En fait, le chiffrement sur l’hôte chiffre vos données de bout en bout. Le chiffrement sur l’hôte n’utilise pas le processeur de votre machine virtuelle et n’a pas d’impact sur ses performances. 

Les disques temporaires sont chiffrés au repos avec des clés gérées par la plateforme lorsque vous activez le chiffrement de bout en bout. Les caches du système d’exploitation et du disque de données sont chiffrés au repos avec des clés gérées par le client ou par la plateforme, selon le type de chiffrement de disque sélectionné. Par exemple, si un disque est chiffré avec des clés gérées par le client, le cache du disque est chiffré avec les clés gérées par le client et, si un disque est chiffré à l’aide de clés gérées par la plateforme, le cache du disque est chiffré avec les clés gérées par la plateforme.

### <a name="restrictions"></a>Restrictions

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

#### <a name="supported-regions"></a>Régions prises en charge

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

#### <a name="supported-vm-sizes"></a>Tailles des machines virtuelles prises en charge

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="double-encryption-at-rest"></a>Double chiffrement des données au repos

Les clients sensibles haute sécurité qui sont concernés par les risques associés à un algorithme de chiffrement particulier, une implémentation ou une clé compromise, peuvent désormais choisir une couche supplémentaire de chiffrement à l’aide d’un algorithme/mode de chiffrement différent au niveau de la couche d’infrastructure à l’aide de clés de chiffrement gérées par la plateforme. Cette nouvelle couche peut être appliquée à des disques, à des captures instantanées et à des images, qui seront toutes chiffrées au repos avec un double chiffrement.

### <a name="supported-regions"></a>Régions prises en charge

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](../../../includes/virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Chiffrement côté serveur et chiffrement de disque Azure

[Azure Disk Encryption](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) utilise la fonctionnalité [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux pour chiffrer des disques managés avec des clés gérées par le client au sein de la machine virtuelle invitée.  Le chiffrement côté serveur avec des clés gérées par le client améliore l’utilisation de Azure Disk Encryption en vous permettant d’utiliser des types et des images de système d’exploitation pour vos machines virtuelles en chiffrant les données dans le service de stockage.

## <a name="next-steps"></a>Étapes suivantes

- Activez le chiffrement de bout en bout à l’aide du chiffrement sur l’hôte avec [CLI](disks-enable-host-based-encryption-cli.md) ou le [portail Azure](disks-enable-host-based-encryption-portal.md).
- Activez le double chiffrement au repos pour les disques managés avec [CLI](disks-enable-double-encryption-at-rest-cli.md) ou le [portail Azure](disks-enable-double-encryption-at-rest-portal.md).
- Activez les clés gérées par le client pour les disques managés avec [CLI](disks-enable-customer-managed-keys-cli.md) ou le [portail Azure](disks-enable-customer-managed-keys-portal.md).
- [Qu’est-ce qu’Azure Key Vault ?](../../key-vault/general/overview.md)
