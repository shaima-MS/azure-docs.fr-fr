---
title: Expérience utilisateur de la connexion par SMS pour le numéro de téléphone (préversion) - Azure AD
description: Découvrir plus d’informations sur l’expérience utilisateur de la connexion par SMS pour des numéros de téléphone nouveaux ou existants
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 864225238c10ab2fda96e95448790201cc8a16ae
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422995"
---
# <a name="use-your-phone-number-as-a-user-name-preview"></a>Utiliser votre numéro de téléphone comme nom d’utilisateur (préversion)

L’inscription d’un appareil donne à votre téléphone l’accès aux services de votre organisation, mais ne permet pas à votre organisation d’accéder à votre téléphone. Si vous êtes administrateur, vous trouverez plus d’informations dans [Configurer et activer des utilisateurs pour l’authentification par SMS](../authentication/howto-authentication-sms-signin.md).

Si votre organisation n’a pas rendu la connexion par SMS disponible, vous ne verrez pas d’option pour celle-ci lors de l’inscription d’un téléphone auprès de votre compte.  

## <a name="when-you-have-a-new-phone-number"></a>Si vous avez un nouveau numéro de téléphone

Si vous recevez un nouveau téléphone ou un nouveau numéro et que vous l’inscrivez auprès d’une organisation pour laquelle la connexion par SMS est disponible, vous effectuez le processus d’inscription de téléphone normal :

1. Sélectionnez **Ajouter une méthode**.
1. Sélectionnez **Téléphone**.
1. Entrez le numéro de téléphone et sélectionnez **M’envoyer un code par SMS**.
1. Une fois que vous avez entré le code, sélectionnez **Suivant**.
1. Vous verrez une invite indiquant « Vérifié par SMS. Votre téléphone a été inscrit. »

> [!Important]
> En raison d’un problème connu dans la préversion, pendant une brève période, l’ajout du numéro de téléphone n’inscrit pas le numéro pour la connexion par SMS. Vous devrez vous connecter avec le numéro ajouté, puis suivre les invites pour inscrire le numéro pour la connexion par SMS.

### <a name="when-the-phone-number-is-in-use"></a>Quand le numéro de téléphone est déjà utilisé

Si vous essayez d’utiliser un numéro de téléphone déjà utilisé par une autre personne de votre organisation, le message suivant s’affiche :

![Message d’erreur quand votre numéro de téléphone est déjà utilisé](media/sms-sign-in-explainer/sms-sign-in-error.png)

Contactez votre administrateur pour résoudre le problème.

## <a name="when-you-have-an-existing-number"></a>Quand vous avez un numéro existant

Si vous utilisez déjà un numéro de téléphone avec une organisation et si vous utilisez votre numéro de téléphone comme nom d’utilisateur, les étapes suivantes peuvent vous aider à vous connecter.

1. Quand la connexion par SMS est disponible, une bannière s’affiche pour vous demander si vous souhaitez activer le numéro de téléphone pour la connexion par SMS :

    [![Bannière pour activer la connexion par SMS pour un numéro de téléphone](media/sms-sign-in-explainer/sms-sign-in-banner.png)](media/sms-sign-in-explainer/sms-sign-in-banner.png#lightbox)

1. En outre, un bouton **Activer** apparaît si vous sélectionnez le caret sur la vignette de la méthode par téléphone :

    [![Bannière pour activer la connexion par SMS pour un numéro de téléphone](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Pour activer la méthode, sélectionnez **Activer**. Vous êtes invité à confirmer l’action :

    ![Boîte de dialogue de confirmation pour activer la connexion par SMS pour un numéro de téléphone](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. Sélectionnez **Activer**.

## <a name="when-you-remove-your-phone-number"></a>Quand vous supprimez votre numéro de téléphone

1. Pour supprimer le numéro de téléphone, sélectionnez le bouton Supprimer dans la vignette de la méthode par téléphone de connexion par SMS.

    [![Bannière pour supprimer la connexion par SMS pour un numéro de téléphone](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Quand vous êtes invité à confirmer l’action, sélectionnez **OK**.

Vous ne pouvez pas supprimer un numéro de téléphone qui est utilisé comme méthode de connexion par défaut. Pour supprimer le numéro, vous devez d’abord changer la méthode de connexion par défaut, puis supprimer à nouveau le numéro de téléphone.
