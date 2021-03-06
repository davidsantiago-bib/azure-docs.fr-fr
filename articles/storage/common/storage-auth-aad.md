---
title: Authentifier l’accès aux objets BLOB Azure et files d’attente à l’aide d’Azure Active Directory | Microsoft Docs
description: Authentifier l’accès aux objets BLOB Azure et files d’attente à l’aide d’Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/28/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e72400a759b976b1a2a6864b2fa7d7d91e16c62f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619286"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Authentifier l’accès aux objets BLOB Azure et files d’attente à l’aide d’Azure Active Directory

Le Stockage Azure prend en charge l’authentification et l’autorisation avec Azure Active Directory (AD) pour les services de Blob et de File d’attente. Avec Azure AD, vous pouvez utiliser le contrôle d’accès basé sur un rôle (RBAC) pour accorder l’accès aux utilisateurs, groupes ou principaux de service d’application. 

L’authentification des utilisateurs et des applications à l’aide des informations d’identification Azure AD est plus sécurisée et plus facile à utiliser que les autres modes d’autorisation. Même si vous pouvez continuer à utiliser l’autorisation de clé partagée avec vos applications, avec Azure AD, vous n’avez plus besoin de stocker votre clé d’accès de compte avec votre code. Vous pouvez également continuer à utiliser des signatures d’accès partagé (SAP) pour accorder un accès affiné aux ressources de votre compte de stockage. Toutefois, Azure AD offre des fonctionnalités similaires sans nécessiter de gestion des jetons SAP, ni de révocation des SAP compromises. Dans la mesure du possible, Microsoft recommande d’utiliser l’authentification Azure AD pour les applications Stockage Azure.

Authentification et autorisation avec les informations d’identification Azure AD est disponible pour tous les v2 à usage général v1 à usage général et comptes de stockage d’objets Blob dans toutes les régions publiques. Seuls les comptes de stockage créé avec la prise en charge du modèle de déploiement Azure Resource Manager d’Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Vue d’ensemble d’Azure AD pour les objets BLOB et files d’attente

La première étape pour utiliser l’intégration d’Azure AD avec Stockage Azure est d’attribuer des rôles RBAC pour les données de stockage à votre principal de service (utilisateur, groupe ou principal de service d’application) ou des identités managées pour les ressources Azure. Les rôles RBAC englobent des jeux d’autorisations communs pour les conteneurs et les files d’attente. Pour en savoir plus sur l’attribution des rôles RBAC pour le stockage Azure, consultez [gérer les droits d’accès aux données de stockage avec RBAC](storage-auth-aad-rbac.md).

Pour utiliser Azure AD afin d’autoriser l’accès aux ressources de stockage dans vos applications, vous devez demander un jeton d’accès OAuth 2.0 dans votre code. Pour savoir comment demander un jeton d’accès et l’utiliser pour autoriser les demandes vers le stockage Azure, consultez [s’authentifier avec Azure AD à partir d’une application de stockage Azure](storage-auth-aad-app.md). Si vous utilisez une identité gérée, consultez [authentifier l’accès aux objets BLOB et files d’attente avec Azure managed identités pour les ressources Azure](storage-auth-aad-msi.md).

Azure CLI et PowerShell prennent désormais en charge la connexion via une identité Azure AD. Une fois que vous vous connectez à une identité Azure AD, votre session s’exécute sous cette identité. Pour plus d’informations, consultez [utiliser une identité Azure AD pour accéder au stockage Azure avec PowerShell ou CLI](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>Rôles RBAC pour objets blob et files d’attente

Azure Active Directory (Azure AD) autorise les droits d’accès aux ressources sécurisées via [RBAC (contrôle d’accès en fonction du rôle)](../../role-based-access-control/overview.md). Stockage Azure définit un ensemble de rôles RBAC intégrés qui englobent des ensembles communs d’autorisations permettant d’accéder aux données blob et file d’attente. Vous pouvez également définir des rôles personnalisés pour l’accès aux données blob et file d’attente.

Lorsqu’un rôle RBAC est attribué à un principal de sécurité Azure AD, Azure accorde l’accès à ces ressources pour cette entité de sécurité. L’accès peut être limité au niveau de l’abonnement, du groupe de ressources, du compte de stockage ou d’un conteneur ou d’une file d’attente individuelle. Un principal de sécurité Azure AD peut être un utilisateur, un groupe, un principal de service d’application, ou un [identité managée pour les ressources Azure](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Pour savoir comment affecter une RBAC intégrée pour les ressources de stockage Azure, consultez une des rubriques suivantes :

- [Accorder l’accès aux données blob et file d’attente Azure avec RBAC dans le portail Azure](storage-auth-aad-rbac-portal.md)
- [Accorder l’accès à des données d’objet blob et file d’attente Azure avec RBAC à l’aide d’Azure CLI](storage-auth-aad-rbac-cli.md)
- [Accorder l’accès à des données d’objet blob et file d’attente Azure avec RBAC à l’aide de PowerShell](storage-auth-aad-rbac-powershell.md)

### <a name="access-permissions-granted-by-rbac-roles"></a>Autorisations d’accès accordées par les rôles RBAC 

Pour plus d’informations sur les autorisations nécessaires pour appeler des opérations relatives au Stockage Azure, consultez [Autorisations d’appel d’opérations REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="resource-scope"></a>Portée de la ressource

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="next-steps"></a>Étapes suivantes

- [Support de stockage Azure pour Azure Active Directory en fonction de contrôle d’accès à la disposition générale](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
- [S’authentifier auprès d’Azure Active Directory à partir d’une application pour accéder aux objets blob et aux files d’attente](storage-auth-aad-app.md)
- [Authentifier l’accès aux objets BLOB et files d’attente avec des identités gérées pour les ressources Azure](storage-auth-aad-msi.md)
- Azure Files prend en charge l’authentification avec Azure AD sur SMB uniquement pour les machines virtuelles jointes au domaine (préversion). Pour en savoir plus sur l’utilisation d’Azure AD sur SMB pour Azure Files, consultez [Vue d’ensemble de l’authentification Azure Active Directory sur SMB pour Azure Files (préversion)](../files/storage-files-active-directory-overview.md).