---
title: Échec de création ou de suppression d’une base de données ou d’une table dans l’Explorateur de données Azure
description: Cet article décrit la procédure de résolution des problèmes pour la création et la suppression des bases de données et tables dans l’Explorateur de données Azure.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 62eccebab81745f85450390f64b6b22f3c17b32e
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758320"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Résolution de problèmes : Échec de création ou de suppression d’une base de données ou d’une table dans l’Explorateur de données Azure

Dans l’Explorateur de données Azure, vous utilisez régulièrement des bases de données et des tables. Cet article indique la procédure de résolution des problèmes qui peuvent se poser.

## <a name="creating-a-database"></a>Création d’une base de données

1. Veillez à disposer des autorisations appropriées. Pour créer une base de données, vous devez être membre du rôle *Collaborateur* ou *Propriétaire* de l’abonnement Azure. Si nécessaire, demandez à l’administrateur de votre abonnement de vous ajouter au rôle approprié.

1. Vérifiez l’absence d’erreurs de validation liées au nom de la base de données. Le nom doit être composé de caractères alphanumériques, avec une longueur maximale de 260 caractères.

1. Vérifiez que les valeurs de mise en cache et de conservation de la base de données se situent dans les plages autorisées. La conservation doit être comprise entre 1 et 36 500 jours (100 ans). La mise en cache doit être comprise entre 1 et la valeur maximale définie pour la conservation.

## <a name="deleting-or-renaming-a-database"></a>Suppression ou renommage d’une base de données

Vérifiez que vous disposez des autorisations appropriées. Pour supprimer ou renommer une base de données, vous devez être un membre du rôle *Collaborateur* ou *Propriétaire* pour l’abonnement Azure. Si nécessaire, demandez à l’administrateur de votre abonnement de vous ajouter au rôle approprié.

## <a name="creating-a-table"></a>Création d’une table

1. Vérifiez que vous disposez des autorisations appropriées. Pour créer une table, vous devez être membre du rôle *Administrateur de base de données* ou *Utilisateur de base de données* dans la base de données ou du rôle *Collaborateur* ou *Propriétaire* dans l’abonnement Azure. Si nécessaire, demandez à l’administrateur de votre abonnement ou cluster de vous ajouter au rôle approprié.

    Pour plus d’informations sur les autorisations, consultez [Gérer des autorisations de base de données](manage-database-permissions.md).

1. Vérifiez qu’une table portant le même nom n’existe pas déjà. Si elle existe, vous pouvez : Créer une table avec un nom différent ; Renommez la table existante (nécessite *table admin* rôle) ; ou supprimez la table existante (nécessite *administrateur de base de données* rôle). Utilisez les commandes suivantes.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Suppression ou renommage d’une table

Veillez à disposer des autorisations appropriées. Pour supprimer ou renommer une table, vous devez être membre du rôle *Administrateur de base de données* ou *Administrateur de table* dans la base de données. Si nécessaire, demandez à l’administrateur de votre abonnement ou cluster de vous ajouter au rôle approprié.

Pour plus d’informations sur les autorisations, consultez [Gérer des autorisations de base de données](manage-database-permissions.md).

## <a name="general-guidance"></a>Règle générale

1. Consultez le [tableau de bord d’état du service Azure](https://azure.microsoft.com/status/). Recherchez l’état de l’Explorateur de données Azure dans la région où vous tentez d’utiliser une base de données ou une table.

    Si l’état n’est pas **correct** (coche verte), réessayez une fois que l’état s’est amélioré.

1. Si vous avez toujours besoin d’aide pour résoudre votre problème, ouvrez une demande de support dans le [portail Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Étapes suivantes

[Vérifier l’intégrité des clusters](check-cluster-health.md)