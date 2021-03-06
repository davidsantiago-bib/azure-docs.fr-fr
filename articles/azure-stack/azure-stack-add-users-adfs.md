---
title: Ajouter des utilisateurs pour Azure Stack ADFS | Microsoft Docs
description: Découvrez comment ajouter des utilisateurs pour les déploiements ADFS d’Azure Stack
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2019
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: 1b47739200c79317273ea0c788f21a7ee4a3b818
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648504"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Ajouter des utilisateurs Azure Stack dans AD FS
Vous pouvez utiliser le composant logiciel enfichable **Utilisateurs et ordinateurs Active Directory** pour ajouter des utilisateurs supplémentaires à un environnement Azure Stack en utilisant AD FS comme son fournisseur d’identité.

## <a name="add-windows-server-active-directory-users"></a>Ajouter des utilisateurs Windows Server Active Directory
> [!TIP]
> Cet exemple utilise le répertoire actif ASDK azurestack.local par défaut. 

1. Connectez-vous à un ordinateur avec un compte fournissant un accès aux outils d’administration Windows, puis ouvrez une nouvelle console MMC (Microsoft Management Console).
2. Sélectionnez **Fichier > Ajouter ou supprimer un composant logiciel enfichable**.
3. Sélectionnez **Utilisateurs et ordinateurs Active Directory** > **AzureStack.local** > **Utilisateurs**.
4. Sélectionnez **Action** > **Nouveau** > **Utilisateur**.
5. Dans Nouvel objet – Utilisateur, fournissez les informations utilisateur. Sélectionnez **Suivant**.
6. Fournissez un mot de passe et confirmez-le.
7. Sélectionnez **Suivant** pour finaliser les valeurs. Sélectionnez **Terminer** pour créer l’utilisateur.


## <a name="next-steps"></a>Étapes suivantes
[Créer les principaux du service](azure-stack-create-service-principals.md)