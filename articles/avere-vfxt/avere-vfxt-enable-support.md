---
title: Activer le support pour Avere vFXT - Azure
description: Découvrez comment activer les chargements de données de support à partir d’Avere vFXT pour Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 12/14/2019
ms.author: rohogue
ms.openlocfilehash: fc86cca1b73eb58a038a0a6da109e86b995709c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85505475"
---
# <a name="enable-support-uploads"></a>Activer les chargements de données de support

Avere vFXT pour Azure peut charger automatiquement des données de support relatives à votre cluster. Ces chargements permettent aux équipes de support de délivrer un meilleur service client.

## <a name="steps-to-enable-uploads"></a>Étapes à suivre pour activer les chargements

Pour activer le support, effectuez les étapes suivantes à partir du Panneau de configuration Avere. Pour savoir comment ouvrir le Panneau de configuration, consultez [Accéder au cluster vFXT](avere-vfxt-cluster-gui.md).

1. Accédez à l’onglet **Settings** (Paramètres) en haut.
1. Cliquez sur le lien **Support** sur la gauche et acceptez la politique de confidentialité.

   ![Capture d’écran montrant le panneau de configuration d’Avere et une fenêtre contextuelle avec le bouton Confirmer pour accepter la politique de confidentialité](media/avere-vfxt-privacy-policy.png)

1. Dans la page Configuration du support, ouvrez la section **Customer Info** (Informations client) en cliquant sur le triangle situé à gauche.
1. Cliquez sur le bouton **Revalidate upload information** (Revalider les informations de chargement).
1. Définissez le nom de support du cluster dans **Unique Cluster Name** (Nom unique du cluster). Vérifiez que ce nom identifie de façon unique votre cluster afin de faciliter son identification par le personnel.
1. Cochez les cases **Statistics Monitoring** (Analyse des statistiques), **General Information Upload** (Chargement des informations générales) et **Crash Information Upload** (Chargement des informations d’incident).
1. Cliquez sur **Envoyer**.

   ![Capture d’écran montrant la section des informations du client complétée dans la page des paramètres de support](media/avere-vfxt-support-info.png)

1. Cliquez sur le triangle à gauche de **Secure Proactive Support (SPS)** pour développer la section.
1. Cochez la case **Enable SPS Link** (Activer le lien SPS).
1. Cliquez sur **Envoyer**.

   ![Capture d’écran montrant la section Secure Proactive Support complétée dans la page des paramètres de support](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Étapes suivantes

Si vous avez besoin d’ajouter un système de stockage local ou cloud existant au cluster, suivez les instructions détaillées dans [Configurer un stockage](avere-vfxt-add-storage.md).

Si vous êtes prêt à attacher des clients au cluster, consultez [Monter le cluster Avere vFXT](avere-vfxt-mount-clients.md).
