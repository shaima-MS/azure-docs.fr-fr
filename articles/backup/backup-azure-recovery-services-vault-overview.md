---
title: Vue d’ensemble des coffres Recovery Services
description: Vue d’ensemble et comparaison entre les coffres Recovery Services et les coffres de sauvegarde Azure.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 0e1d061c6baf31fad2e937a604098f0baff6086d
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041899"
---
# <a name="recovery-services-vaults-overview"></a>Vue d’ensemble des coffres Recovery Services

Cet article décrit les fonctionnalités d’un coffre Recovery Services. Un coffre Recovery Services est une entité de stockage dans Azure qui héberge des données. Les données sont généralement des copies de données ou des informations de configuration pour des machines virtuelles, des charges de travail, des serveurs ou des stations de travail. Vous pouvez utiliser des coffres Recovery Services afin de stocker des données de sauvegarde pour divers services Azure tels que des machines virtuelles IaaS (Windows ou Linux) et des bases de données Azure SQL. Les coffres Recovery Services prennent en charge System Center DPM, Windows Server, le serveur de sauvegarde Azure et bien plus. Les coffres Recovery Services facilitent l’organisation de vos données de sauvegarde, tout en réduisant le temps de gestion. Les coffres Recovery Services sont basés sur le modèle Resource Manager d’Azure, qui fournit des fonctionnalités telles que :

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Comparaison entre les coffres Recovery Services et les coffres de sauvegarde

- **Fonctionnalités enrichies pour sécuriser les données de sauvegarde** : avec les coffres Recovery Services, la sauvegarde Azure offre des fonctionnalités de sécurité pour protéger les sauvegardes cloud. Ces fonctionnalités de sécurité vous garantissent de pouvoir sécuriser vos sauvegardes et récupérer en toute sécurité des données même si des serveurs de production et de sauvegarde sont compromis. [En savoir plus](backup-azure-security-feature.md)

- **Supervision centralisée de votre environnement informatique hybride** : avec les coffres Recovery Services, vous pouvez non seulement superviser vos [machines virtuelles Azure IaaS](backup-azure-manage-vms.md), mais aussi vos [ressources locales](backup-azure-manage-windows-server.md#manage-backup-items) à partir d’un portail centralisé. [En savoir plus](backup-azure-monitoring-built-in-monitor.md)

- **Contrôle d’accès en fonction du rôle (RBAC)** : RBAC offre un contrôle très précis de la gestion des accès dans Azure. [Azure offre différents rôles intégrés](../role-based-access-control/built-in-roles.md), et Sauvegarde Microsoft Azure comprend trois [rôles intégrés pour gérer les points de récupération](backup-rbac-rs-vault.md). Les coffres Recovery Services sont compatibles avec RBAC, ce qui limite les accès en sauvegarde et en restauration à l’ensemble défini des rôles d’utilisateur. [En savoir plus](backup-rbac-rs-vault.md)

- **Suppression réversible** :  Avec la suppression réversible, même si un intervenant malveillant supprime une sauvegarde (ou même si les données de sauvegarde sont accidentellement supprimées), les données de sauvegarde sont conservées pendant 14 jours supplémentaires, ce qui permet la récupération de cet élément de sauvegarde sans perte de données. La conservation des données de sauvegarde pendant 14 jours supplémentaires dans l’état « suppression réversible » n’engendre pas de frais pour le client. [Plus d’informations](backup-azure-security-feature-cloud.md)

- **Restauration inter-région** :  La restauration inter-région (CRR) peut être utilisée pour restaurer des machines virtuelles Azure dans une région secondaire, qui est une région jumelée Azure. Si Azure déclare un incident dans la région primaire, les données répliquées dans la région secondaire peuvent être restaurées dans la région secondaire afin d’atténuer le temps d’arrêt réel de leur environnement dans la région principale. [Plus d’informations](backup-azure-arm-restore-vms.md#cross-region-restore)

## <a name="storage-settings-in-the-recovery-services-vault"></a>Paramètres de stockage dans le coffre Recovery Services

Un coffre Recovery Services est une entité qui stocke les sauvegardes et les points de récupération créés au fil du temps. Le coffre Recovery Services contient également les stratégies de sauvegarde associées aux machines virtuelles protégées.

- La Sauvegarde Azure gère automatiquement le stockage du coffre. Découvrez comment les [paramètres de stockage peuvent être modifiés](./backup-create-rs-vault.md#set-storage-redundancy).

- Pour en savoir plus sur la redondance du stockage, consultez les articles suivants sur la redondance [géographique](../storage/common/storage-redundancy.md) et la redondance [locale](../storage/common/storage-redundancy.md).

### <a name="additional-resources"></a>Ressources supplémentaires

- [Scénarios pris en charge et non pris en charge par le coffre](backup-support-matrix.md#vault-support)
- [Foire aux questions sur le coffre](backup-azure-backup-faq.md)

## <a name="azure-advisor"></a>Azure Advisor

[Azure Advisor](../advisor/index.yml) est un consultant cloud personnalisé qui permet d’optimiser l’utilisation d’Azure. Il analyse votre utilisation d’Azure et fournit des recommandations en temps utile pour vous permettre d’optimiser et de sécuriser vos déploiements. Les recommandations sont divisées en quatre catégories : Haute disponibilité, sécurité, performances et coût.

Azure Advisor fournit des [recommandations](../advisor/advisor-high-availability-recommendations.md#protect-your-virtual-machine-data-from-accidental-deletion) toutes les heures pour les machines virtuelles qui ne sont pas sauvegardées. Ainsi, vous ne risquez pas d’oublier de sauvegarder des machines virtuelles importantes. Vous pouvez également contrôler les recommandations en les répétant.  Vous pouvez sélectionner la recommandation et activer la sauvegarde sur les machines virtuelles en ligne en spécifiant le coffre (où les sauvegardes vont être stockées) et la stratégie de sauvegarde (planification des sauvegardes et rétention des copies de sauvegarde).

![Azure Advisor](./media/backup-azure-recovery-services-vault-overview/azure-advisor.png)

## <a name="next-steps"></a>Étapes suivantes

Consultez les articles suivants pour :</br>
[Sauvegarder une machine virtuelle IaaS](backup-azure-arm-vms-prepare.md)</br>
[Sauvegarder un serveur de sauvegarde Azure](backup-azure-microsoft-azure-backup.md)</br>
[Sauvegarder un serveur Windows Server](backup-windows-with-mars-agent.md)
