---
title: Tutoriel sur l’exportation de données à partir d’Azure Data Box | Microsoft Docs
description: Découvrez les prérequis du déploiement et la façon d’exporter les données à partir d’Azure Data Box
services: databox
author: priestlg
ms.service: databox
ms.subservice: pod
ms.topic: how-to
ms.date: 07/10/2020
ms.author: v-grpr
ms.openlocfilehash: 5b17488ebab8dc29221723fabf8efe0c00467509
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86208937"
---
# <a name="tutorial-create-export-order-for-azure-data-box-preview"></a>Tutoriel : Créer une commande d’exportation pour Azure Data Box (préversion)

Azure Data Box est une solution hybride qui vous permet de déplacer des données à partir d’Azure vers votre emplacement. Ce tutoriel explique comment créer une commande d’exportation pour Azure Data Box. La raison principale pour créer une commande d’exportation est la récupération d’urgence, dans le cas où le stockage local est compromis et qu’une sauvegarde doit être restaurée.

Ce tutoriel vous fournira des informations sur :

> [!div class="checklist"]
>
> * Prérequis pour l’exportation
> * Commander un Data Box pour l’exportation
> * Suivre la commande d’exportation
> * Annuler la commande d’exportation

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Prérequis

Respectez les prérequis de configuration suivants pour l’appareil et le service Data Box avant de commander l’appareil.

### <a name="for-service"></a>Pour le service

[!INCLUDE [Data Box service prerequisites](../../includes/data-box-supported-subscriptions.md)]

* Assurez-vous de disposer d’un groupe de ressources existant que vous pouvez utiliser avec votre Azure Data Box.

* Assurez-vous que votre compte de stockage Azure à partir duquel vous souhaitez exporter les données est l’un des types de compte de stockage pris en charge, comme décrit dans [Comptes de stockage pris en charge pour Data Box](data-box-system-requirements.md#supported-storage-accounts).

### <a name="for-device"></a>Pour l’appareil

Avant de commencer, assurez-vous que :

* Vous disposez d’un ordinateur hôte connecté au réseau du centre de données. Vous allez copier les données à partir d’Azure Data Box vers cet ordinateur. Votre ordinateur hôte doit exécuter un système d’exploitation pris en charge comme décrit dans [Conditions requises pour le système Azure Data Box](data-box-system-requirements.md).

* Votre centre de données doit avoir un réseau haut débit. Nous vous recommandons vivement d’utiliser au minimum une connexion 10 GbE. Si vous ne disposez pas d’une connexion 10-GbE, vous pouvez utiliser une liaison de données 1-GbE, mais les vitesses de copie en seront impactées.

## <a name="order-data-box-for-export"></a>Commander Data Box pour l’exportation

Procédez comme suit dans le portail Azure pour commander un appareil.

1. Utilisez vos informations d’identification Microsoft Azure pour vous connecter à cette URL : [https://portal.azure.com](https://portal.azure.com).

2. Sélectionnez **+ Créer une ressource**, puis recherchez *Azure Data Box*. Sélectionnez **Azure Data Box**.

   ![Créer une ressource](media/data-box-deploy-export-ordered/azure-data-box-export-00b.png)

3. Sélectionnez **Create** (Créer).

   ![Créer Azure Data Box](media/data-box-deploy-export-ordered/azure-data-box-export-00c.png)

4. Vérifiez si le service Azure Data Box est disponible dans votre région. Entrez ou sélectionnez les informations suivantes, puis sélectionnez **Appliquer**.

    |Paramètre  |Valeur  |
    |---------|---------|
    |Type de transfert     | Sélectionnez **Exporter vers Azure**.        |
    |Abonnement     | Sélectionnez un abonnement EA, CSP ou Azure pour le service Data Box. <br> L’abonnement est lié à votre compte de facturation.       |
    |Resource group     |    Sélectionnez un groupe de ressources existant. <br> Un groupe de ressources est un conteneur logique pour les ressources qui peuvent être gérées ou déployées ensemble.         |
    |Région Azure source    |    Sélectionnez la région Azure où vos données se trouvent actuellement.         |
    |Pays de destination     |     Sélectionnez le pays où vous souhaitez expédier l’appareil.        |

   ![Sélectionner vos paramètres Data Box](media/data-box-deploy-export-ordered/azure-data-box-export-01.png)

5. Sélectionnez **Data Box**. La capacité maximale utilisable pour une commande unique est de 80 To. Vous pouvez créer plusieurs commandes pour des tailles de données supérieures.

   ![Sélectionner la capacité Data Box](media/data-box-deploy-export-ordered/azure-data-box-export-02b.png)

6. Dans **Commande**, indiquez les détails de la commande **De base**. Entrez ou sélectionnez les informations suivantes, puis sélectionnez **Suivant**.

    |Paramètre  |Valeur  |
    |---------|---------|
    |Abonnement     | L’abonnement est automatiquement renseigné en fonction de la sélection antérieure.|
    |Resource group | Groupe de ressources que vous avez sélectionné précédemment. |
    |Nom de la commande d’exportation     |  Indiquez un nom convivial pour suivre la commande. <br> Le nom peut comporter entre 3 et 24 caractères qui peuvent être des lettres, des chiffres et des traits d’union. <br> Il doit commencer et se terminer par une lettre ou un chiffre.      |

    ![Détails de base de la commande d’exportation](media/data-box-deploy-export-ordered/azure-data-box-export-03.png)

    Sélectionnez **Suivant : Sélection des données** pour continuer.

7. Dans **Sélection des données**, sélectionnez **Ajouter un compte de stockage et un type d’exportation**.

    ![Ajouter un compte de stockage et un type d’exportation](media/data-box-deploy-export-ordered/azure-data-box-export-03b.png)

8. Dans **Sélectionner l’option d’exportation**, spécifiez les détails de l’option d’exportation. Entrez ou sélectionnez les informations suivantes, puis sélectionnez **Ajouter**.

    |Paramètre  |Valeur  |
    |---------|---------|
    |Compte de stockage     | Compte de Stockage Azure à partir duquel vous voulez exporter des données. |
    |Type d’exportation     | Spécifie le type de données à exporter à partir de **Tous les objets** et **Utiliser un fichier XML**.<ul><li> **Tous les objets** : spécifie que le travail exporte toutes les données en fonction de votre sélection sous **Options de transfert**.</li><li> **Utiliser un fichier XML** : spécifie un fichier XML qui contient un ensemble de chemins et de préfixes pour les objets Blob et/ou les fichiers à exporter à partir du compte de stockage. Le fichier XML doit se trouver dans le conteneur du compte de stockage sélectionné, et la sélection à partir de partages de fichiers n’est pas prise en charge actuellement. Le fichier doit être un fichier .xml non vide.</li></ul>        |
    |Options de transfert     |  Spécifie les options de transfert de données à partir de **Sélectionner tout**, **Tous les objets Blob** et **Tous les fichiers**. <ul><li> **Sélectionner tout** : spécifie que tous les objets Blob et les fichiers Azure sont exportés. Si vous utilisez un compte de stockage qui prend uniquement en charge les objets Blob (Compte de stockage Blob), l’option **Tous les fichiers** ne peut pas être sélectionnée.</li><li> **Tous les objets Blob** : spécifie que seuls les objets Blob de blocs et de pages sont exportés.</li><li> **Tous les fichiers** : spécifie que tous les fichiers sont exportés à l’exclusion des objets Blob. Le type de compte de stockage que vous avez (GPv1 et GPv2, stockage Premium ou stockage Blob) détermine les types de données que vous pouvez exporter. Pour plus d’informations, consultez [Comptes de stockage pris en charge pour l’exportation](../storage/common/storage-import-export-requirements.md#supported-storage-types).</li></ul>         |
    |Inclure le journal détaillé     | Indique si vous souhaitez un fichier journal détaillé qui contient une liste de tous les fichiers qui ont bien été exportés.        |

    > [!NOTE]
    >
    > Si vous sélectionnez **Utiliser un fichier XML** pour le paramètre **Type d’exportation**, vous devez vous assurer que le fichier xml contient des chemins et/ou des préfixes valides. Vous devez créer et fournir le fichier XML.  Si le fichier n’est pas valide ou si aucune donnée ne correspond aux chemins spécifiés, la commande contient des données partielles ou aucune donnée n’est exportée.

    Pour savoir comment ajouter un fichier XML à un conteneur, consultez [Commande d’exportation à l’aide d’un fichier XML](data-box-deploy-export-ordered.md#export-order-using-xml-file).

   ![Sélectionner l’option d’exportation](media/data-box-deploy-export-ordered/azure-data-box-export-04b.png)

   Pour voir un exemple de l’entrée XML, consultez [Exemple d’entrée XML](data-box-deploy-export-ordered.md#sample-xml-file)

9. Dans **Sélection des données**, passez en revue vos paramètres, puis sélectionnez **Suivant : Coordonnées>** .

   ![Coordonnées](media/data-box-deploy-export-ordered/azure-data-box-export-05.png)

10. Dans **Coordonnées**, sélectionnez **+ Ajouter une adresse de livraison** pour entrer vos informations de livraison.

    ![Ajouter une adresse de livraison](media/data-box-deploy-export-ordered/azure-data-box-export-06.png)

11. Dans **Ajouter une adresse de livraison**, indiquez vos nom et prénom, le nom et l’adresse postale de la société, et un numéro de téléphone valide. Sélectionnez **Valider**. Le service valide l’adresse d’expédition de disponibilité du service. Si le service est disponible pour l’adresse de livraison indiquée, vous recevez une notification à cet effet.

    ![Valider l’adresse de livraison](media/data-box-deploy-export-ordered/azure-data-box-export-07.png)

    Si vous commandez dans une région où l’expédition autogérée est disponible, vous pouvez sélectionner cette option. Pour plus d’informations sur l’expédition gérée par vous-même, consultez [Utiliser l’expédition autogérée](data-box-portal-customer-managed-shipping.md).

12. Sélectionnez **Ajouter une adresse de livraison** une fois que les détails de l’expédition ont été correctement validés.

13. Dans **Coordonnées**, vérifiez votre adresse de livraison et votre adresse e-mail. Le service envoie des notifications par courrier électronique concernant les mises à jour de l’état de la commande aux adresses de messagerie spécifiées.

    Nous vous recommandons d’utiliser un e-mail de groupe afin de continuer à recevoir des notifications si un administrateur du groupe quitte l’entreprise.

    ![Détails de la commande](media/data-box-deploy-export-ordered/azure-data-box-export-09.png)

14. Sélectionnez **Suivant : Passer en revue + commander>** . Vous devez accepter les conditions générales pour poursuivre la création de la commande.

15. Sélectionnez **Commander**. La création d’une commande peut prendre quelques minutes.

    ![Validation de la commande](media/data-box-deploy-export-ordered/azure-data-box-export-10.png)

## <a name="export-order-using-xml-file"></a>Commande d’exportation à l’aide d’un fichier XML

Si vous sélectionnez **Utiliser un fichier XML**, vous pouvez spécifier des conteneurs et des objets Blob spécifiques (page et bloc) à exporter. Vous devez suivre les spécifications dans le [tableau d’exemple de fichier XML](#sample-xml-file) pour mettre en forme votre fichier XML. Les étapes ci-dessous vous montrent comment utiliser un fichier XML pour l’exportation de vos données :

1. Pour **Type d’exportation**, sélectionnez **Utiliser un fichier XML**. Il s’agit de votre fichier XML qui spécifie les objets Blob et les fichiers Azure que vous souhaitez exporter. Pour ajouter le fichier XML, sélectionnez **Cliquez ici pour sélectionner un fichier XML**.
     ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-01.png)

2. Sélectionnez **+ Conteneur** pour créer un conteneur.
    ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-02.png)

3. Dans l’onglet **Nouveau conteneur** qui s’affiche à partir du côté droit du portail Azure, ajoutez un nom pour le conteneur. Le nom doit être en minuscules et vous pouvez inclure des chiffres et des tirets « - ». Sélectionnez ensuite le **Niveau d’accès public** dans la zone de liste déroulante. Nous vous recommandons de choisir **Privé (accès non anonyme)** pour empêcher d’autres utilisateurs d’accéder à vos données. Pour plus d’informations sur les niveaux d’accès au conteneur, consultez [Autorisations d’accès au conteneur](../storage/blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-04.png)

4. Sélectionnez **Create** (Créer).

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-07.png)

   Si votre conteneur est correctement créé, vous recevrez le message suivant :

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-09.png)

5. Sélectionnez le conteneur que vous avez créé, puis double-cliquez dessus.

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-08.png)

6. Le double-clic sur le conteneur affiche la vue Propriétés du conteneur. Vous souhaitez maintenant joindre (ou accédez à) votre fichier XML qui contient la liste des objets Blob et/ou des fichiers Azure que vous souhaitez exporter. Sélectionnez **Télécharger**.

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-10c.png)

7. Vous avez correctement ajouté le fichier XML au conteneur. Seuls les objets Blob et les fichiers Azure que vous avez spécifiés dans ce fichier XML seront exportés.

   ![Fichier XML](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-12.png)

## <a name="track-the-order"></a>Suivre la commande

Une fois la commande passée, vous pouvez suivre son état à partir du portail Azure. Accédez à votre commande Data Box, puis à **Vue d’ensemble** pour voir l’état. Le portail affiche la commande avec l’état **Commandé**.

Une fois la préparation de l’appareil terminée, la copie des données démarre à partir des comptes de stockage sélectionnés. Le portail affiche la commande avec l’état **Copie de données en cours**.

![Commande d’exportation Data Box traitée](media/data-box-deploy-export-ordered/azure-data-box-export-15b.png)

Data Box copie les données du ou des comptes de stockage source(s). Une fois la copie des données terminée, Data Box est verrouillé et le portail affiche la commande avec l’état **Copie terminée**.

![Copie des données d’exportation Data Box terminée](media/data-box-deploy-export-ordered/azure-data-box-export-15c.png)

Si l’appareil n’est pas disponible, vous recevez une notification. Si l’appareil est disponible, Microsoft identifie l’appareil à expédier et le prépare. Pendant la préparation de l’appareil, les actions suivantes se produisent :

* Des partages SMB sont créés pour chaque compte de stockage associé à l’appareil.
* Pour chaque partage, des informations d’identification d’accès (nom d’utilisateur et mot de passe) sont générées.
* L’appareil est verrouillé et est accessible uniquement à l’aide du mot de passe de déverrouillage de l’appareil. Pour récupérer le mot de passe, vous devez vous connecter à votre compte du portail Azure et sélectionner **Détails de l’appareil**.

Ensuite, Microsoft prépare et achemine l’appareil via un transporteur régional. Vous recevez un numéro de suivi une fois l’appareil expédié. Le portail affiche la commande dont l’état est **Distribué**.

![Commande d’exportation Data Box envoyée](media/data-box-deploy-export-ordered/azure-data-box-export-16.png)

Si l’expédition autogérée a été sélectionnée, vous recevez une notification par e-mail avec les étapes suivantes lorsque l’appareil est prêt à être récupéré à partir du centre de donnée. Pour plus d’informations sur l’expédition autogérée, consultez [Expédition autogérée](data-box-portal-customer-managed-shipping.md).

![Expédition autogérée prête pour la collecte](media/data-box-deploy-export-ordered/azure-data-box-export-17.png)

## <a name="cancel-the-order"></a>Annuler la commande

Pour annuler cette commande, dans le portail Azure, accédez à **Vue d’ensemble**, puis sélectionnez **Annuler** dans la barre de commandes.

Après avoir passé la commande, vous pouvez l’annuler à tout moment tant que son traitement n’a pas commencé.

Pour supprimer une commande annulée, accédez à **Vue d’ensemble**, puis sélectionnez **Supprimer** dans la barre de commandes.

## <a name="sample-xml-file"></a>Exemple de fichier XML

Le fichier XML suivant montre un exemple de noms d’objets Blob, de préfixes d’objets Blob et de fichiers Azure contenus dans le format XML que la commande d’exportation utilise lorsque vous sélectionnez l’option **Utiliser un fichier XML** :

```xml
<?xml version="1.0" encoding="utf-8"?>
   <!-- BlobList/prefix/Container list for Blob storage for export  -->
   <BlobList>
      <BlobPath>/8tbpageblob/8tbpageblob/8tbpageblob</BlobPath>
      <BlobPathPrefix>/blockblob4dot75tbdata/</BlobPathPrefix>
      <BlobPathPrefix>/1tbfilepageblob</BlobPathPrefix>
      <BlobPathPrefix>/1tbfile/</BlobPathPrefix>
      <BlobPathPrefix>/8mbfiles/</BlobPathPrefix>
      <BlobPathPrefix>/64mbfiles/</BlobPathPrefix>
   </BlobList>
   <!-- FileList/prefix/Share list for Azure File storage for export  -->
   <AzureFileList>
      <FilePathPrefix>/64mbfiles/</FilePathPrefix>
      <FilePathPrefix>/4mbfiles/prefix2/subprefix</FilePathPrefix>
      <FilePathPrefix>/1tbfile/prefix</FilePathPrefix>
   </AzureFileList>
```

Quelques points importants concernant les fichiers XML :

* Les balises XML respectent la casse et doivent correspondre exactement à ce qui est spécifié dans l’exemple ci-dessus.
* Les balises d’ouverture et de fermeture doivent correspondre.
* Des balises XML ou une mise en forme incorrectes peuvent entraîner l’échec de l’exportation des données.
* Aucune donnée n’est exportée si le préfixe d’objet Blob et/ou de fichier n’est pas valide.

### <a name="examples-of-valid-blob-paths"></a>Exemples de chemins d’objets blob valides

Le tableau suivant présente des exemples de chemins d’accès d’objet blob valides :

   | Sélecteur | Chemin d'accès d'objet blob | Description |
   | --- | --- | --- |
   | Starts With (Commence par) |/ |Exporte tous les objets blob présents dans le compte de stockage. |
   | Starts With (Commence par) |/$root/ |Exporte tous les objets blob présents dans le conteneur racine. |
   | Starts With (Commence par) |/containers |Exporte tous les objets Blob présents dans un conteneur commençant par le préfixe **containers** |
   | Starts With (Commence par) |/container-name/ |Exporte tous les objets Blob présents dans le conteneur **container-name** |
   | Starts With (Commence par) |/container-name/prefix |Exporte tous les objets Blob présents dans le conteneur **container-name** qui commencent par le préfixe **prefix** |
   | Égal à |$root/logo.bmp |Exporte l'objet blob **logo.bmp** présent dans le conteneur racine. |
   | Égal à |8tbpageblob/mydata.txt |Exporte l’objet Blob **mydata.txt** dans le conteneur **8tbpageblob** |

## <a name="sample-log-files"></a>Exemple de fichiers journaux

Cette section fournit des exemples de fichiers journaux qui sont générés au cours de l’exportation. Les fichiers journaux d’erreur sont générés automatiquement. Pour générer le fichier journal détaillé, vous devez sélectionner **Inclure un journal détaillé** dans le portail Azure lors de la configuration de la commande d’exportation.
Pour plus d’informations sur les journaux de copie et les journaux détaillés, consultez [Journaux de copie](data-box-deploy-export-copy-data.md#copy-data-from-data-box).

<!-- ### Verbose log

The following log files show examples of verbose logging when you select **Include verbose log**:

```xml
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3.4" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/helloEndWithDot..txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test..txt" Size="12" crc64="17906086011702236012"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/.......txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/copylogb08fa3095564421bb550d775fff143ed====..txt" Size="53638" crc64="1147139997367113454"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/testmaxChars-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-12345679" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file0" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file1" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000001" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000000" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed10.dat" Size="65536" crc64="10746682179555216785"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/LiveSiteReport_Oct.xlsx" Size="7028" crc64="6103506546789189963"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/NE_Oct_GeoReport.xlsx" Size="103197" crc64="13305485882546035852"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed1.dat" Size="65536" crc64="3140622834011462581"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-0" Size="1048576" crc64="16086591317856295272"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000001" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-0" Size="4194304" crc64="1339017920798612765"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000000" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-1" Size="8388608" crc64="3963298606737216548"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-1" Size="1048576" crc64="11061759121415905887"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/XLS-10MB.xls" Size="1199104" crc64="2218419493992437463"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-0" Size="8388608" crc64="1072783424245035917"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-1" Size="4194304" crc64="9991307204216370812"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/VL_Piracy_Negtive10_TPNameAndGCS.xlsx" Size="12398699" crc64="13526033021067702820"></File>
```

### Copy logs

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box). -->

<!-- The following xml shows an example of the copy log when the export is successful:

```xml
<CopyLog Summary="Summary">
  <Status>Succeeded</Status>
    <TotalFiles_Blobs>27</TotalFiles_Blobs>
    <FilesErrored>0</FilesErrored>
</CopyLog>
```

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box).

The following xml shows an example of the copy log when the export has errors:

```xml
<ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/wastorage.v140.3.0.2.nupkg">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444.hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444 (1).hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>4</TotalFiles_Blobs>
  <FilesErrored>3</FilesErrored>
</CopyLog>
``` -->

## <a name="next-steps"></a>Étapes suivantes

Ce tutoriel vous a apporté des connaissances concernant Azure Data Box, notamment concernant les points suivants :

> [!div class="checklist"]
>
> * Prérequis pour l’exportation
> * Commander un Data Box pour l’exportation
> * Suivre la commande d’exportation
> * Annuler la commande d’exportation

Passez au tutoriel suivant pour apprendre à configurer votre Data Box.

> [!div class="nextstepaction"]
> [Configurer votre Azure Data Box](./data-box-deploy-set-up.md)
