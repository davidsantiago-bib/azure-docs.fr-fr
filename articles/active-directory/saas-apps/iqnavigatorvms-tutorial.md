---
title: 'Didacticiel : Intégration d’Azure Active Directory à IQNavigator VMS | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et IQNavigator VMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5a0700a63d21d089573f757716e08fb03665b28
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58164992"
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a>Tutoriel : Intégration d’Azure Active Directory à IQNavigator VMS

Dans ce didacticiel, vous allez apprendre à intégrer IQNavigator VMS à Azure Active Directory (Azure AD).

L’intégration d’IQNavigator VMS à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à IQNavigator VMS
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à IQNavigator (via l’authentification unique) avec leurs comptes Azure AD
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Conditions préalables

Pour configurer l’intégration d’Azure AD à IQNavigator, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un IQNavigator VMS l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois ici [Offre d’essai](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de IQNavigator VMS à partir de la galerie
1. Configuration et test de l’authentification unique Azure AD

## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Ajout de IQNavigator VMS à partir de la galerie
Pour configurer l’intégration d’IQNavigator à Azure AD, vous devez ajouter IQNavigator à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter IQNavigator VMS à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

1. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
1. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

1. Dans la zone de recherche, saisissez **IQNavigator**.

    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

1. Dans le panneau de résultats, sélectionnez **IQNavigator**, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD à IQNavigator VMS basé sur un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur IQNavigator VMS équivalent dans Azure AD. En d’autres termes, une relation entre l’utilisateur Azure AD et l’utilisateur associé dans IQNavigator VMS doit être établie.

Dans IQNavigator VMS, attribuez la valeur du **nom d’utilisateur** dans Azure AD comme valeur **Nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec IQNavigator VMS, exécutez les sections principales suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Création d’un utilisateur de test IQNavigator VMS](#creating-a-iqnavigator-vms-test-user)**  - pour avoir un équivalent de Britta Simon dans IQNavigator VMS lié à la représentation Azure AD de l’utilisateur.
1. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application IQNavigator VMS.

**Pour configurer l’authentification unique Azure AD avec IQNavigator VMS, procédez comme suit :**

1. Dans le portail Azure, dans la page d’intégration de l’application **IQNavigator VMS**, cliquez sur **Authentification unique**.

    ![Configurer l'authentification unique][4]

1. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

1. Dans la section **Domaine et URL IQNavigator VMS**, procédez comme suit :

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    a. Dans la zone de texte **Identificateur**, saisissez l’URL :`iqn.com`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`

1. Cochez l’option **Afficher les paramètres d’URL avancés**, puis procédez comme suit :

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    Dans la zone de texte **État de relais**, entrez une URL en utilisant le modèle suivant : `https://<subdomain>.iqnavigator.com`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de réponse et l’état de relais réels. Pour obtenir ces valeurs, contactez [l’équipe de support client IQNavigator VMS](https://www.beeline.com/iqn-product-support/).

1. Dans la section  **Certificat de signature SAML** , cliquez sur le bouton Copier pour copier l’ **URL des métadonnées de fédération de l’application** , puis collez-la dans le Bloc-notes.
    
    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_metadataurl.png)

1. L’application IQNavigator attend la valeur d’identificateur utilisateur unique dans la revendication Identificateur de nom. Le client peut mapper la valeur correcte pour la revendication Identificateur de nom. Dans ce cas, nous avons mappé l’user.UserPrincipalName aux fins de démonstration. Mais, en fonction des paramètres de votre organisation, vous devez mapper la valeur correcte.

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

1. Cliquez sur le bouton **Enregistrer** .

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_general_400.png)

1. Dans la section **Configuration dIQNavigator VMS**, cliquez sur **Configurer IQNavigator VMS** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez **l’URL de déconnexion, l’ID d’entité SAML et l’URL du service d’authentification unique SAML** à partir de la **section Référence rapide.**

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png)

1. Pour configurer l’authentification unique côté **IQNavigator VMS**, vous devez envoyer **l’URL des métadonnées de fédération de l’application**, **l’URL de déconnexion, l’ID d’entité SAML et l’URL du service d’authentification unique SAML** à [l’équipe du support technique IQNavigator VMS](https://www.beeline.com/iqn-product-support/). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_01.png) 

1. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_02.png)

1. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.

    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_03.png)

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/iqnavigatorvms-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.

### <a name="creating-an-iqnavigator-vms-test-user"></a>Création d’un utilisateur de test IQNavigator VMS

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans IQNavigator VMS. Travailler avec [l’équipe de support d’IQNavigator VMS](https://www.beeline.com/iqn-product-support/) pour ajouter des utilisateurs dans le compte IQNavigator VMS.

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à IQNavigator VMS.

![Affecter des utilisateurs][200]

**Pour attribuer Britta Simon à IQNavigator VMS, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201]

1. Dans la liste des applications, sélectionnez **IQNavigator VMS**.

    ![Configurer l'authentification unique](./media/iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png)

1. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202]

1. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

1. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette IQNavigator VMS dans le volet d’accès, vous devez être connecté automatiquement à votre application IQNavigator VMS.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/iqnavigatorvms-tutorial/tutorial_general_203.png

