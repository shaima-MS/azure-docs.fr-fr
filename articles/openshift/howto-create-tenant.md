---
title: Créer un locataire Azure AD pour Azure Red Hat OpenShift
description: Voici comment créer un locataire Azure Active Directory (Azure AD) pour héberger votre cluster Microsoft Azure Red Hat OpenShift.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: ad03538cafcce9c1d660d0f2ac5eb3c6ae5f4f38
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84694903"
---
# <a name="create-an-azure-ad-tenant-for-azure-red-hat-openshift"></a>Créer un locataire Azure AD pour Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift nécessite un locataire [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) pour créer votre cluster. Un *locataire* est une instance Azure AD dédiée reçue par une organisation ou un développeur d’applications lorsque l’un ou l’autre crée une relation avec Microsoft en s’inscrivant à Azure, Microsoft Intune ou Microsoft 365. Chaque locataire Azure AD est distinct et séparé des autres locataires Azure AD. Il dispose de ses propres identités professionnelles et scolaires et de ses propres inscriptions d’application.

Si vous ne disposez pas d’un locataire Azure AD, suivez ces instructions pour en créer un.

## <a name="create-a-new-azure-ad-tenant"></a>Créer un nouveau locataire Azure AD

Pour créer un locataire :

1. Connectez-vous au [Portail Azure](https://portal.azure.com/)en utilisant le compte que vous souhaitez associer à votre cluster Azure Red Hat OpenShift.
2. Ouvrez le [panneau Azure Active Directory](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) pour créer un locataire (aussi connu en tant que nouveau *Azure Active Directory*).
3. Fournissez un **nom d’organisation**.
4. Fournissez un **nom de domaine initial**. *onmicrosoft.com* lui est adjoint. Ici, vous pouvez reprendre la valeur de *Nom de l’organisation*.
5. Choisissez un pays ou une région où le locataire sera créé.
6. Cliquez sur **Créer**.
7. Après avoir créé votre locataire Azure AD, cliquez sur le lien **Cliquez ici pour gérer votre nouveau répertoire**. Le nom de votre nouveau locataire doit figurer dans le coin supérieur droit du Portail Azure :  

    ![Capture d’écran du portail montrant le nom du locataire en haut à droite][tenantcallout]  

8. Prenez note de l’*ID locataire* afin de pouvoir spécifier ultérieurement où créer votre cluster Azure Red Hat OpenShift. Dans le portail, vous devez maintenant voir le panneau de vue d’ensemble d’Azure Active Directory pour votre nouveau locataire. Sélectionnez **Propriétés**, puis copiez la valeur de votre **ID de répertoire**. Nous nous référerons à cette valeur en tant que `TENANT` dans le didacticiel [Créer un cluster Azure Red Hat OpenShift](tutorial-create-cluster.md).

[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="resources"></a>Ressources

Consultez la [documentation Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) pour plus d’informations sur les [locataires Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment créer un principal de service, générer une URL de rappel de clé secrète client et d’authentification, et créer un utilisateur Active Directory pour tester des applications sur votre cluster Azure Red Hat OpenShift.

[Créer un utilisateur et un objet d’application Azure AD](howto-aad-app-configuration.md)
