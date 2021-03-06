---
title: Utiliser le Portail Microsoft Azure pour configurer des clés gérées par le client
titleSuffix: Azure Storage
description: Découvrez comment utiliser le portail Azure afin de configurer des clés gérées par le client avec Azure Key Vault pour le chiffrement du stockage Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/13/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: a216714939dc45fd1b220f24414a527969ab7fcb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029566"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Configurer des clés gérées par le client avec Azure Key Vault à l’aide du Portail Microsoft Azure

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Cet article explique comment configurer un coffre de clés Azure avec des clés gérées par le client à l’aide du [portail Azure](https://portal.azure.com/). Pour savoir comment créer un coffre de clés à l’aide du portail Azure, consultez [Démarrage rapide : Définir et récupérer un secret depuis Azure Key Vault à l’aide du portail Azure](../../key-vault/secrets/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Configurer Azure Key Vault

Pour utiliser des clés gérées par le client avec le chiffrement Stockage Azure, deux propriétés doivent être configurées sur le coffre de clés, **Suppression réversible** et **Ne pas vider**. Ces propriétés ne sont pas activées par défaut, mais peuvent être activées à l’aide de PowerShell ou d’Azure CLI sur un coffre de clés nouveau ou existant.

Pour savoir comment activer ces propriétés sur un coffre de clés existant, consultez les sections intitulées **Activation de la suppression réversible** et **Activation de la protection contre le vidage** dans l’un des articles suivants :

- [Guide pratique pour utiliser la suppression réversible avec Azure Power​Shell](../../key-vault/general/soft-delete-powershell.md).
- [Guide pratique pour utiliser la suppression réversible avec Azure CLI](../../key-vault/general/soft-delete-cli.md).

Le chiffrement du stockage Azure prend en charge les clés RSA et RSA-HSM dans les tailles 2048, 3072 et 4096. Pour plus d’informations sur les clés, consultez **Clés Key Vault** dans [À propos des clés, des secrets et des certificats Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="enable-customer-managed-keys"></a>Activer des clés gérées par le client

Pour activer des clés gérées par le client dans le portail Azure, procédez comme suit :

1. Accédez à votre compte de stockage.
1. Sur le panneau **Paramètres** du compte de stockage, cliquez sur **Chiffrement**. Sélectionnez l’option **Clés gérées par le client**, comme illustré dans l’image suivante.

    ![Capture d’écran du portail affichant l’option de chiffrement](./media/storage-encryption-keys-portal/portal-configure-encryption-keys.png)

## <a name="specify-a-key"></a>Spécifier une clé

Après avoir activé les clés gérées par le client, vous pourrez spécifier une clé à associer au compte de stockage. Vous pouvez également indiquer si le stockage Azure doit faire pivoter automatiquement la clé gérée par le client, ou si vous allez faire pivoter la clé manuellement.

### <a name="specify-a-key-from-a-key-vault"></a>Spécifiez une clé à partir d’un coffre de clés

Lorsque vous sélectionnez une clé gérée par le client à partir d’un coffre de clés, la rotation automatique de la clé est automatiquement activée. Pour gérer manuellement la version de la clé, spécifiez l’URI de la clé à la place et incluez la version de la clé. Pour plus d’informations, consultez [Spécifier une clé en tant qu’URI](#specify-a-key-as-a-uri).

Pour spécifier une clé à partir d’un coffre de clés, procédez comme suit :

1. Choisissez l’option **Sélectionner dans le coffre de clés**.
1. Sélectionnez **Sélectionner un coffre de clés et une clé**.
1. Sélectionnez le coffre de clés contenant la clé que vous souhaitez utiliser.
1. Sélectionnez la clé dans le coffre de clés.

   ![Capture d’écran montrant comment sélectionner un coffre de clés et une clé](./media/storage-encryption-keys-portal/portal-select-key-from-key-vault.png)

1. Enregistrez vos modifications.

### <a name="specify-a-key-as-a-uri"></a>Spécifier une clé en tant qu’URI

Lorsque vous spécifiez l’URI de la clé, omettez la version de la clé pour activer la rotation automatique de la clé gérée par le client. Si vous incluez la version de clé dans l’URI de la clé, la rotation automatique n’est pas activée et vous devez gérer la version de la clé vous-même. Pour plus d’informations sur la mise à jour de la version de la clé, consultez [Mettre à jour manuellement la version de la clé](#manually-update-the-key-version).

Pour spécifier une clé en tant qu’URI, procédez comme suit :

1. Pour localiser l’URI de la clé dans le portail Azure, naviguez jusqu'à votre coffre de clés, puis sélectionnez le paramètre **Clés**. Sélectionnez la clé souhaitée, puis cliquez dessus pour afficher ses versions. Sélectionnez une version de clé pour afficher les paramètres de cette version.
1. Copiez la valeur du champ **Identificateur de clé**, qui fournit l’URI.

    ![Capture d’écran montrant l’URI de la clé du coffre de clés](media/storage-encryption-keys-portal/portal-copy-key-identifier.png)

1. Dans les paramètres de **clé de chiffrement** de votre compte de stockage, choisissez l’option **Entrer l’URI de la clé**.
1. Collez l’URI que vous avez copié dans le champ **URI de clé**. Omettez la version de la clé de l’URI pour activer la rotation automatique.

   ![Capture d’écran montrant comment entrer l’URI d’une clé](./media/storage-encryption-keys-portal/portal-specify-key-uri.png)

1. Spécifiez l’abonnement qui contient le coffre de clés.
1. Enregistrez vos modifications.

Une fois que vous avez spécifié la clé, le portail Azure indique si la rotation de clé automatique est activée et affiche la version de clé en cours d’utilisation pour le chiffrement.

:::image type="content" source="media/storage-encryption-keys-portal/portal-auto-rotation-enabled.png" alt-text="Capture d’écran montrant la rotation automatique des clés gérées par le client activée":::

## <a name="manually-update-the-key-version"></a>Mettre à jour manuellement la version de la clé

Par défaut, le stockage Azure effectue automatiquement une rotation automatique des clés gérées par le client, comme décrit dans les sections précédentes. Si vous choisissez de gérer vous-même la version de la clé, vous devez mettre à jour la version de clé spécifiée pour le compte de stockage chaque fois que vous créez une nouvelle version de la clé.

Pour mettre à jour le compte de stockage afin d’utiliser la nouvelle version de la clé, procédez comme suit :

1. Accédez à votre compte de stockage et affichez les paramètres de **chiffrement**.
1. Saisissez l’URI de la nouvelle version de clé. Vous pouvez également sélectionner à nouveau le coffre de clés et la clé pour mettre à jour la version.
1. Enregistrez vos modifications.

## <a name="switch-to-a-different-key"></a>Passer à une clé différente

Pour modifier la clé utilisée pour le chiffrement Azure Storage, procédez comme suit :

1. Accédez à votre compte de stockage et affichez les paramètres de **chiffrement**.
1. Entrez l’URI de la nouvelle clé. Vous pouvez également sélectionner le coffre de clés et choisir une nouvelle clé.
1. Enregistrez vos modifications.

## <a name="disable-customer-managed-keys"></a>Désactiver les clés gérées par le client

Lorsque vous désactivez les clés gérées par le client, votre compte de stockage est de nouveau chiffré avec des clés gérées par Microsoft. Pour désactiver les clés gérées par le client, procédez comme suit :

1. Accédez à votre compte de stockage et affichez les paramètres de **chiffrement**.
1. Désactivez la case à cocher en regard du paramètre **Utiliser votre propre clé**.

## <a name="next-steps"></a>Étapes suivantes

- [Chiffrement du stockage Azure pour les données au repos](storage-service-encryption.md)
- [Qu’est-ce qu’Azure Key Vault ?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
