---
title: Utilisation de machines virtuelles et de groupes de sécurité réseau dans Azure Bastion
description: Cet article décrit comment incorporer des accès au groupe de sécurité réseau avec Azure Bastion
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: charwen
ms.openlocfilehash: 1fc261c31a1190536f3128ed6472d9ca76dfce7e
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86112195"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>Utiliser l’accès au groupe de sécurité réseau et Azure Bastion

Lorsque vous utilisez Azure Bastion, vous pouvez utiliser des groupes de sécurité réseau (NSG). Pour plus d’informations, consultez [Groupes de sécurité](../virtual-network/security-overview.md).

:::image type="content" source="./media/bastion-nsg/figure-1.png" alt-text="Groupe de sécurité réseau":::

Dans ce diagramme :

* l’hôte Bastion est déployé dans le réseau virtuel.
* L’utilisateur se connecte au portail Azure à l’aide de n’importe quel navigateur HTML5.
* L’utilisateur accède à la machine virtuelle Azure avec un protocole RDP/SSH.
* Intégration de la connexion : session RDP/SSH en un clic dans le navigateur
* Aucune adresse IP publique n’est requise sur la machine virtuelle Azure.

## <a name="network-security-groups"></a><a name="nsg"></a>Groupes de sécurité réseau

Cette section décrit le trafic réseau entre l’utilisateur et Azure Bastion et jusqu’aux machines virtuelles cibles de votre réseau virtuel :

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure Bastion est spécifiquement déployé dans le sous-réseau ***AzureBastionSubnet***.

* **Trafic d’entrée :**

   * **Trafic d’entrée à partir d’un réseau Internet public :** Azure Bastion créera une adresse IP publique et le port 443 devra être activé pour le trafic d’entrée de cette adresse. Il n’est PAS nécessaire d’ouvrir le port 3389/22 sur le sous-réseau AzureBastionSubnet.
   * **Trafic d’entrée à partir du plan de contrôle d’Azure Bastion :** pour la connectivité du plan de contrôle, activez le port 443 pour un accès entrant à partir de l’étiquette de service **GatewayManager**. Ceci permet au plan de contrôle, c’est-à-dire au gestionnaire de passerelle, de communiquer avec Azure Bastion.


   :::image type="content" source="./media/bastion-nsg/inbound.png" alt-text="Entrant":::

* **Trafic de sortie :**

   * **Trafic de sortie vers les machines virtuelles cibles :** Azure Bastion atteindra les machines virtuelles cibles via l’adresse IP privée. Les groupes de sécurité réseau doivent autoriser le trafic de sortie vers d’autres sous-réseaux de machines virtuelles cibles sur les ports 3389 et 22.
   * **Trafic de sortie vers d’autres points de terminaison publics dans Azure :** Azure Bastion doit pouvoir se connecter à différents points de terminaison publics dans Azure (par exemple, pour stocker les journaux de diagnostic et de mesure). Azure Bastion a donc besoin d’un accès sortant sur le port 443 vers l’étiquette de service **AzureCloud**.


   :::image type="content" source="./media/bastion-nsg/outbound.png" alt-text="Sortante":::

### <a name="target-vm-subnet"></a>Sous-réseau de la machine virtuelle cible
Il s’agit du sous-réseau qui contient la machine virtuelle cible à laquelle vous souhaitez vous connecter via RDP/SSH.

   * **Trafic d’entrée à partir d’Azure Bastion :** Azure Bastion atteindra les machines virtuelles cibles via l’adresse IP privée. Les ports RDP et SSH (3389 et 22 respectivement) doivent être ouverts du côté de la machine virtuelle cible sur l’adresse IP privée. Une bonne pratique consiste à ajouter la plage d’adresses IP du sous-réseau Azure Bastion à cette règle pour autoriser Bastion à ouvrir ces ports sur les machines virtuelles cibles de votre sous-réseau de machines virtuelles cible uniquement.


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Azure Bastion, consultez les [Questions fréquentes (FAQ)](bastion-faq.md).
