---
title: Se connecter aux services de stockage Azure
titleSuffix: Azure Machine Learning
description: Découvrez comment utiliser des magasins de données pour vous connecter en toute sécurité aux services de stockage Azure lors de l’entraînement avec Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 07/22/2020
ms.custom: how-to, seodec18, devx-track-python
ms.openlocfilehash: 90de785d56e50885a13d43faa77f087d1235ea18
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852529"
---
# <a name="connect-to-azure-storage-services"></a>Se connecter aux services de stockage Azure
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Dans cet article, découvrez comment vous **connecter aux services de stockage Azure par le biais des magasins de données Azure Machine Learning**. Les banques de données se connectent en toute sécurité à votre service de stockage Azure sans avoir à compromettre vos informations d’authentification et l’intégrité de votre source de données d’origine. Ils stockent des informations de connexion, comme votre ID d’abonnement et votre autorisation de jeton, dans votre [Key Vault](https://azure.microsoft.com/services/key-vault/) associé à l’espace de travail, de sorte que vous pouvez accéder à votre stockage en toute sécurité sans avoir à coder en dur ces informations dans vos scripts. Vous pouvez utiliser le [kit de développement logiciel (SDK) Azure Machine Learning Python](#python) ou [Azure Machine Learning Studio](#studio) pour créer et inscrire des banques de données.

Si vous préférez créer et gérer des banques de données à l’aide de l’extension Azure Machine Learning VS Code, consultez le [guide de procédures de gestion des ressources VS Code](how-to-manage-resources-vscode.md#datastores).

Vous pouvez créer des magasins de données à partir de ces [solutions de stockage Azure](#matrix). **Pour les solutions de stockage non prises en charge**, et pour réduire le coût de sortie des données pendant les expériences de Machine Learning, [déplacez vos données](#move) vers une solution de stockage Azure prise en charge.  

Pour comprendre où les magasins de données interviennent dans flux de travail d’accès aux données d’Azure Machine Learning, consultez l’article [Accéder en toute sécurité aux données](concept-data.md#data-workflow).

## <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants :
- Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, créez un compte gratuit avant de commencer. Essayez la [version gratuite ou payante d’Azure Machine Learning](https://aka.ms/AMLFree).

- Un compte de stockage Azure avec un [type de stockage pris en charge](#matrix).

- Le [Kit de développement logiciel (SDK) Azure Machine Learning pour Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ou l’accès à [Azure Machine Learning studio](https://ml.azure.com/).

- Un espace de travail Azure Machine Learning.
  
  [Créez un espace de travail Azure Machine Learning](how-to-manage-workspace.md) ou utilisez un espace de travail existant via le Kit de développement logiciel (SDK) Python. 

    Importez les classes `Workspace` et `Datastore`, puis chargez vos informations d’abonnement à partir du fichier `config.json` à l’aide de la fonction `from_config()`. Cette fonction recherche le fichier JSON dans le répertoire actif par défaut, mais vous pouvez également spécifier un paramètre de chemin d’accès pour pointer vers le fichier à l’aide de `from_config(path="your/file/path")`.

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

    Quand vous créez un espace de travail, un conteneur d’objets blob Azure et un partage de fichiers Azure sont inscrits automatiquement comme magasins de données dans l’espace de travail. sous les noms `workspaceblobstore` et `workspacefilestore` respectivement. Le `workspaceblobstore` est utilisé pour stocker les artefacts de l’espace de travail et vos journaux d’expérience de Machine Learning. Il est également défini en tant que **magasin de données par défaut** et ne peut pas être supprimé de l’espace de travail. Le `workspacefilestore` est utilisé pour stocker des notebooks et des scripts R autorisés via une [instance de calcul](https://docs.microsoft.com/azure/machine-learning/concept-compute-instance#accessing-files).
    
    > [!NOTE]
    > Le concepteur Azure Machine Learning (préversion) crée un magasin de données nommé **azureml_globaldatasets** automatiquement lorsque vous ouvrez un exemple dans la page d’accueil du concepteur. Ce magasin de données contient uniquement des exemples de jeux de données. Veuillez **ne pas** utiliser ce magasin de données pour accéder à des données confidentielles.

<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>Types de services de stockage de données pris en charge

Les magasins de données prennent actuellement en charge le stockage des informations de connexion dans les services de stockage figurant dans la matrice suivante.

| Type de&nbsp;stockage | Type&nbsp;d’authentification | [Azure&nbsp;Machine&nbsp;Learning studio](https://ml.azure.com/) | [SDK Azure&nbsp;Machine&nbsp;Learning&nbsp; Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) |  [CLI Azure&nbsp;Machine&nbsp;Learning](reference-azure-machine-learning-cli.md) | [API REST Azure&nbsp;Machine&nbsp;Learning&nbsp;](https://docs.microsoft.com/rest/api/azureml/) | VS Code
---|---|---|---|---|---|---
[Stockage&nbsp;Blob&nbsp;Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)| Clé de compte <br> Jeton SAS | ✓ | ✓ | ✓ |✓ |✓
[Partage&nbsp;de fichiers&nbsp;Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)| Clé de compte <br> Jeton SAS | ✓ | ✓ | ✓ |✓|✓
[Azure&nbsp;Data Lake&nbsp;Storage Gen&nbsp;1](https://docs.microsoft.com/azure/data-lake-store/)| Principal du service| ✓ | ✓ | ✓ |✓|
[Azure&nbsp;Data Lake&nbsp;Storage Gen&nbsp;2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)| Principal du service| ✓ | ✓ | ✓ |✓|
[Azure&nbsp;SQL&nbsp;Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)| Authentification SQL <br>Principal du service| ✓ | ✓ | ✓ |✓|
[Azure&nbsp;PostgreSQL](https://docs.microsoft.com/azure/postgresql/overview) | Authentification SQL| ✓ | ✓ | ✓ |✓|
[Azure&nbsp;Database&nbsp;pour&nbsp;MySQL](https://docs.microsoft.com/azure/mysql/overview) | Authentification SQL|  | ✓* | ✓* |✓*|
[Databricks&nbsp;File&nbsp;System](https://docs.microsoft.com/azure/databricks/data/databricks-file-system)| Aucune authentification | | ✓** | ✓ ** |✓** |

* MySQL est uniquement pris en charge pour le pipeline [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py). <br>
** Databricks est uniquement pris en charge pour le pipeline [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py).

### <a name="storage-guidance"></a>Conseils liés au stockage

Nous vous recommandons de créer un magasin de données pour un [conteneur Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). Les stockages Standard et Premium sont tous deux disponibles pour les objets blob. Le stockage Premium est plus cher, mais ses vitesses de débit supérieures vous feront gagner du temps sur vos exécutions d’entraînement, en particulier si vous effectuez l’entraînement sur un jeu de données volumineux. Pour plus d’informations sur le coût des comptes de stockage, consultez la [calculatrice de prix Azure](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

[Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction?toc=/azure/storage/blobs/toc.json) s’appuie sur le stockage Blob Azure et a été conçu pour l’analytique Big Data des entreprises. Une caractéristique fondamentale de Data Lake Storage Gen2 est l’ajout d’un [espace de noms hiérarchique](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) au stockage Blob. L’espace de noms hiérarchique organise les objets/fichiers dans une hiérarchie de répertoires pour offrir un accès efficace aux données.

## <a name="storage-access-and-permissions"></a>Accès et autorisations pour le stockage

Pour s’assurer que vous vous connectez en toute sécurité à votre service de stockage Azure, Azure Machine Learning doit avoir l’autorisation d’accéder au conteneur de stockage de données correspondant. Cet accès dépend des informations d’authentification utilisées pour inscrire le magasin de données. 

### <a name="virtual-network"></a>Réseau virtuel 

Si votre compte de stockage de données se trouve sur un **réseau virtuel**, des étapes de configuration supplémentaires sont nécessaires pour s’assurer qu’Azure Machine Learning a accès à vos données. Pour vous assurer que les étapes de configuration appropriées sont appliquées lors de la création et de l’enregistrement de votre magasin de données, consultez [Isolement réseau et confidentialité](how-to-enable-virtual-network.md#machine-learning-studio).  

### <a name="access-validation"></a>Validation de l’accès

**Dans le cadre du processus de création et d’inscription du magasin de données initial**, Azure Machine Learning vérifie automatiquement que le service de stockage sous-jacent existe et que le principal fourni par l’utilisateur (nom d’utilisateur, principal de service ou jeton SAS) a accès au stockage spécifié.

**Après la création** du magasin de données, cette validation est effectuée uniquement pour les méthodes qui requièrent l’accès au conteneur de stockage sous-jacent, et **non** chaque fois que des objets du magasin de données sont récupérés. Par exemple, la validation se produit si vous souhaitez télécharger des fichiers à partir de votre magasin de données ; mais si vous souhaitez simplement modifier votre magasin de données par défaut, la validation ne se produit pas.

Pour authentifier votre accès au service de stockage sous-jacent, vous pouvez fournir votre clé de compte, des jetons de signature d’accès partagé (SAS) ou le principal de service dans la méthode `register_azure_*()` correspondante du type de magasin de données que vous souhaitez créer. La [matrice de types de stockage](#matrix) répertorie les types d’authentification pris en charge qui correspondent à chaque type de magasin de données.

Vous trouverez des informations sur la clé de compte, le jeton SAS et le principal de service sur votre [portail Azure](https://portal.azure.com).

* Si vous envisagez d’utiliser une clé de compte ou un jeton SAP pour l’authentification, sélectionnez **Comptes de stockage** dans le volet gauche, puis choisissez le compte de stockage que vous souhaitez inscrire. 
  * La page **Vue d’ensemble** fournit des informations telles que le nom du compte, le conteneur et le nom du partage de fichiers. 
      1. Pour les clés de compte, accédez à **Clés d’accès** dans le volet **Paramètres**. 
      1. Pour les jetons SAP, accédez à **Signatures d’accès partagé** dans le volet **Paramètres**.

* Si vous prévoyez d’utiliser un principal du service pour l’authentification, accédez à vos **Inscriptions d’applications**, puis sélectionnez l’application que vous souhaitez utiliser. 
    * La page **Vue d’ensemble** correspondante contient des informations requises comme l’ID de locataire et l’ID de client.

> [!IMPORTANT]
> Pour des raisons de sécurité, vous devrez peut-être changer vos clés d’accès pour un compte Stockage Azure (clé de compte ou jeton SAS). Dans ce cas, veillez à synchroniser les nouvelles informations d’identification avec votre espace de travail et les magasins de données qui y sont connectés. Découvrez comment synchroniser vos informations d’identification mises à jour avec [ces étapes](how-to-change-storage-access-key.md). 

### <a name="permissions"></a>Autorisations

Pour le conteneur d’objets BLOB Azure et le stockage Azure Data Lake Gen2, assurez-vous que vos informations d’authentification ont l’accès **Lecteur des données Blob du stockage**. En savoir plus sur le [Lecteur des données blob du stockage](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader). 

<a name="python"></a>

## <a name="create-and-register-datastores-via-the-sdk"></a>Créer et inscrire des magasins de données via le SDK

Quand vous inscrivez une solution de stockage Azure en tant que magasin de données, ce dernier est automatiquement créé et inscrit dans un espace de travail spécifique. Pour savoir où trouver les informations d’authentification requises, consultez la section [accès au stockage et autorisations](#storage-access-and-permissions).

Dans cette section, vous trouverez des exemples de création et d’inscription d’un magasin de données via le kit de développement logiciel (SDK) Python pour les types de stockage suivants. Les paramètres fournis dans ces exemples sont les **paramètres requis** pour créer et inscrire un magasin de données.

* [Conteneur d’objets blob Azure](#azure-blob-container)
* [Partage de fichiers Azure](#azure-file-share)
* [Azure Data Lake Storage Gen2](#azure-data-lake-storage-generation-2)

 Pour créer des magasins de données pour les autres services de stockage pris en charge, consultez la [documentation de référence des méthodes `register_azure_*` applicables](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#methods).

Si vous préférez une expérience à moindre code, consultez [Créer des magasins de données dans Azure Machine Learning Studio](#studio).

> [!NOTE]
> Le nom du magasin de données doit contenir uniquement des lettres minuscules, des chiffres et des traits de soulignement. 

### <a name="azure-blob-container"></a>Conteneur d’objets blob Azure

Pour inscrire un conteneur d’objets blob Azure comme magasin de données, utilisez [`register_azure_blob_container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

Le code suivant crée le magasin de données `blob_datastore_name` et l’inscrit auprès de l’espace de travail `ws`. Ce magasin de données accède au conteneur d'objets blob `my-container-name` sur le compte de stockage `my-account-name` avec la clé d'accès au compte fournie.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account access key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

### <a name="azure-file-share"></a>Partage de fichiers Azure

Pour inscrire un partage de fichiers Azure comme magasin de données, utilisez [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). 

Le code suivant crée le magasin de données `file_datastore_name` et l’inscrit auprès de l’espace de travail `ws`. Ce magasin de données accède au partage de fichiers `my-fileshare-name` sur le compte de stockage `my-account-name` avec la clé d'accès au compte fournie.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account access key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                     datastore_name=file_datastore_name, 
                                                     file_share_name=file_share_name, 
                                                     account_name=account_name,
                                                     account_key=account_key)
```

### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage Gen 2

Pour un magasin de données Azure Data Lake Storage Gen 2 (ADLS Gen 2), utilisez [register_azure_data_lake_gen2()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) pour inscrire un magasin de données d’informations d’identification connecté à un stockage Azure DataLake Gen 2 avec des [autorisations de principal du service](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal). 

Pour utiliser votre principal de service, vous devez [inscrire votre application](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) et accorder au principal de service un accès **Lecteur des données Blob du stockage**. Découvrez-en plus sur la [configuration du contrôle d’accès pour ADLS Gen 2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control). 

Le code suivant crée le magasin de données `adlsgen2_datastore_name` et l’inscrit auprès de l’espace de travail `ws`. Ce magasin de données accède au système de fichiers `test` dans le compte de stockage `account_name` en utilisant les informations d’identification du principal de service fournies.

```python 
adlsgen2_datastore_name = 'adlsgen2datastore'

subscription_id=os.getenv("ADL_SUBSCRIPTION", "<my_subscription_id>") # subscription id of ADLS account
resource_group=os.getenv("ADL_RESOURCE_GROUP", "<my_resource_group>") # resource group of ADLS account

account_name=os.getenv("ADLSGEN2_ACCOUNTNAME", "<my_account_name>") # ADLS Gen2 account name
tenant_id=os.getenv("ADLSGEN2_TENANT", "<my_tenant_id>") # tenant id of service principal
client_id=os.getenv("ADLSGEN2_CLIENTID", "<my_client_id>") # client id of service principal
client_secret=os.getenv("ADLSGEN2_CLIENT_SECRET", "<my_client_secret>") # the secret of service principal

adlsgen2_datastore = Datastore.register_azure_data_lake_gen2(workspace=ws,
                                                             datastore_name=adlsgen2_datastore_name,
                                                             account_name=account_name, # ADLS Gen2 account name
                                                             filesystem='test', # ADLS Gen2 filesystem
                                                             tenant_id=tenant_id, # tenant id of service principal
                                                             client_id=client_id, # client id of service principal
                                                             client_secret=client_secret) # the secret of service principal
```

<a name="studio"></a>


## <a name="create-datastores-in-the-studio"></a>Créer des magasins de données dans Studio 


Créez un magasin de données en quelques étapes avec Azure Machine Learning Studio.

> [!IMPORTANT]
> Si votre compte de stockage de données se trouve sur un réseau virtuel, des étapes de configuration supplémentaires sont nécessaires pour s’assurer que Studio a accès à vos données. Pour vous assurer que les étapes de configuration appropriées sont appliquées, consultez [Isolement réseau et confidentialité](how-to-enable-virtual-network.md#machine-learning-studio). 

1. Connectez-vous à [Azure Machine Learning Studio](https://ml.azure.com/).
1. Sélectionnez **Magasins de données** dans le volet gauche sous **Gérer**.
1. Sélectionnez **+ Nouveau magasin de données**.
1. Remplissez le formulaire de création d’un magasin de données. Le formulaire est mis à jour intelligemment en fonction du type de stockage Azure et du type d’authentification que vous sélectionnez. Pour savoir où trouver les informations d’authentification requises pour remplir ce formulaire, consultez la section [accès au stockage et autorisations](#access-validation).

L’exemple suivant montre à quoi ressemble le formulaire quand vous créez un **magasin de données d’objets blob Azure** : 
    
![Formulaire de création d’un magasin de données](media/how-to-access-data/new-datastore-form.png)

<a name="train"></a>
## <a name="use-data-in-your-datastores"></a>Utiliser des données dans vos magasins de données

Après avoir créé un magasin de données, [créer un Azure Machine Learning DataSet](how-to-create-register-datasets.md) pour interagir avec vos données. Les jeux de données intègrent vos données dans un objet consommable évalué tardivement pour les tâches de Machine Learning, comme la formation. Ils offrent également la possibilité de [télécharger ou de monter](how-to-train-with-datasets.md#mount-vs-download) des fichiers de n’importe quel format à partir de services de stockage Azure, comme le stockage d’objets BLOB Azure et ADLS Gen2. Vous pouvez également les utiliser pour charger des données tabulaires dans un dataframe Pandas ou Spark.

<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Récupérer les magasins de données à partir de votre espace de travail

Pour obtenir un magasin de données spécifique inscrit dans l’espace de travail actif, utilisez la méthode statique [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) sur la classe `Datastore` :

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Pour obtenir la liste des banques de données inscrites avec un espace de travail donné, vous pouvez utiliser la propriété [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) sur un objet d’espace de travail :

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Pour obtenir le magasin de données par défaut de l’espace de travail, utilisez cette ligne :

```Python
datastore = ws.get_default_datastore()
```
Vous pouvez également modifier le magasin de données par défaut avec le code suivant. Cette possibilité est prise en charge uniquement via le Kit de développement logiciel (SDK). 

```Python
 ws.set_default_datastore(new_default_datastore)
```

## <a name="access-data-during-scoring"></a>Accéder aux données pendant le scoring

Azure Machine Learning offre plusieurs moyens d’utiliser vos modèles pour le scoring. Certaines de ces méthodes ne fournissent pas d’accès aux magasins de données. Utilisez le tableau suivant pour comprendre les méthodes qui vous permettent d’accéder aux magasins de données pendant le scoring :

| Méthode | Accès aux magasins de données | Description |
| ----- | :-----: | ----- |
| [Prédiction par lots](how-to-use-parallel-run-step.md) | ✔ | Effectuez des prédictions sur de grandes quantités de données de façon asynchrone. |
| [Service web](how-to-deploy-and-where.md) | &nbsp; | Déployez des modèles comme un service web. |
| [Module Azure IoT Edge](how-to-deploy-and-where.md) | &nbsp; | Déployez des modèles sur des appareils IoT Edge. |

Dans les situations où le SDK ne fournit pas d’accès aux magasins de données, vous pouvez créer du code personnalisé à l’aide du SDK Azure approprié pour accéder aux données. Par exemple, le [kit de développement logiciel (SDK) de stockage Azure pour Python](https://github.com/Azure/azure-storage-python) est une bibliothèque cliente que vous pouvez utiliser pour accéder aux données stockées dans des objets blob ou des fichiers.

<a name="move"></a>

## <a name="move-data-to-supported-azure-storage-solutions"></a>Déplacer des données vers des solutions de stockage Azure prises en charge

Azure Machine Learning prend en charge l’accès aux données à partir des stockages suivants : Stockage Blob Azure, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database et Azure Database pour PostgreSQL. Si vous utilisez un stockage non pris en charge, nous vous recommandons de déplacer vos données vers des solutions de stockage Azure prises en charge en utilisant [Azure Data Factory et ces étapes](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool). Déplacer vos données vers un stockage pris en charge peut vous aider à réduire les coûts de sortie des données pendant les expériences Machine Learning. 

Azure Data Factory fournit un moyen de transfert des données efficace et résilient avec plus de 80 connecteurs prédéfinis, sans coût supplémentaire. Ces connecteurs incluent les services de données Azure, les sources de données locales, Amazon S3 et Redshift, et Google BigQuery.

## <a name="next-steps"></a>Étapes suivantes

* [Créer un jeu de données Azure Machine Learning](how-to-create-register-datasets.md)
* [Entraîner un modèle](how-to-train-ml-models.md)
* [Déployer un modèle](how-to-deploy-and-where.md)
