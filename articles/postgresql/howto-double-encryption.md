---
title: Chiffrement double d’infrastructure - Portail Azure - Azure Database pour PostgreSQL
description: Découvrez comment configurer et gérer le chiffrement double d’infrastructure pour votre Azure Database pour PostgreSQL.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 6612fe38adcd3c8002dd4a11122b5bb2e797a4dd
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102172"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-postgresql"></a>Chiffrement double d’infrastructure pour Azure Database pour PostgreSQL

Découvrez comment configurer et gérer le chiffrement double d’infrastructure pour votre Azure Database pour PostgreSQL.

## <a name="prerequisites"></a>Prérequis

* Vous devez avoir un abonnement Azure et être un administrateur de cet abonnement.

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---portal"></a>Créer un serveur Azure Database pour PostgreSQL avec un chiffrement double d’infrastructure - Portail

Procédez comme suit pour créer un serveur Azure Database pour MySQL avec le chiffrement double d’infrastructure à partir de Portail Azure :

1. Sélectionnez **Créer une ressource** (+) dans l’angle supérieur gauche du portail.

2. Sélectionnez **Bases de données** > **Azure Database pour PostgreSQL**. Vous pouvez également entrer PostgreSQL dans la zone de recherche pour trouver le service. Activation de l’option de déploiement **Serveur unique**.

   ![La base de données « Azure Database pour PostgreSQL » dans le menu](./media/quickstart-create-database-portal/1-create-database.png)

3. Fournissez les informations basiques du serveur. Sélectionnez **Paramètres supplémentaires** et cochez la case **chiffrement double d’infrastructure** pour activer le paramètre.

    ![Sélections Azure Database pour PostgreSQL](./media/howto-infrastructure-double-encryption/infrastructure-encryption-selected.png)

4. Sélectionnez **Vérifier + créer** pour provisionner le serveur.

    ![Résumé Azure Database pour PostgreSQL](./media/howto-infrastructure-double-encryption/infrastructure-encryption-summary.png)

5. Une fois le serveur créé, vous pouvez valider le double chiffrement d’infrastructure en vérifiant l’état dans le panneau **Chiffrement des données**.

    ![Validation Azure Database pour MySQL](./media/howto-infrastructure-double-encryption/infrastructure-encryption-validation.png)

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---cli"></a>Créer un serveur Azure Database pour PostgreSQL avec un chiffrement double d’infrastructure - CLI

Procédez comme suit pour créer un serveur Azure Database pour MySQL avec le chiffrement double d’infrastructure à partir de l’interface de ligne de commande :

Cet exemple crée un groupe de ressources nommé `myresourcegroup` à l’emplacement `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
L’exemple suivant crée un serveur PostgreSQL 11 dans la région USA Ouest, nommé `mydemoserver`, dans votre groupe de ressources `myresourcegroup` avec l’identifiant d’administrateur serveur `myadmin`. Il s’agit d’un serveur à **usage général**, de **4e génération** avec **2 vCores**. Cela activera également le double chiffrement d’infrastructure pour le serveur créé. Remplacez `<server_admin_password>` par votre propre valeur.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 11 --infrastructure-encryption >Enabled/Disabled>
```

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le chiffrement des données, consultez [Chiffrement double d’infrastructure des données Azure Database pour PostgreSQL](concepts-Infrastructure-double-encryption.md).

