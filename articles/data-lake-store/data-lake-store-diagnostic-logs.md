---
title: Affichage des journaux de diagnostic d’Azure Data Lake Storage Gen1 | Microsoft Docs
description: 'Comprendre comment configurer les journaux de diagnostic et y accéder pour Data Lake Storage Gen1 '
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: a0bb320abb31b38461102e0e9a062ea0c2af51fb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959576"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-storage-gen1"></a>Accès aux journaux de diagnostic d’Azure Data Lake Storage Gen1
Découvrez comment activer la journalisation des diagnostics pour votre compte Azure Data Lake Storage Gen1 et comment afficher les journaux collectés pour votre compte.

Les organisations peuvent activer la journalisation de diagnostic pour leur compte Azure Data Lake Storage Gen1 afin de collecter des pistes d’audit d’accès aux données qui fournissent des informations telles que la liste des utilisateurs qui accèdent aux données, la fréquence à laquelle les données sont consultées, la quantité de données stockée sur le compte, etc. Quand cette fonctionnalité est activée, les diagnostics et/ou les demandes sont enregistrés sur la base du meilleur effort. Les entrées de journal des demandes et des diagnostics sont créées uniquement si des demandes sont effectuées sur le point de terminaison de service.

## <a name="prerequisites"></a>Conditions préalables
* **Un abonnement Azure**. Consultez la page [Obtention d’un essai gratuit d’Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Compte Azure Data Lake Storage Gen1**. Suivez les instructions de [Prise en main d’Azure Data Lake Storage Gen1 avec le portail Azure](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-storage-gen1-account"></a>Activer la journalisation de diagnostic pour votre compte Azure Data Lake Storage Gen1
1. Inscrivez-vous au nouveau [portail Azure](https://portal.azure.com).
2. Ouvrez votre compte Azure Data Lake Storage Gen1 et dans le panneau de votre compte Azure Data Lake Storage Gen1, cliquez sur **Paramètres de diagnostic**.
3. Dans le panneau **Paramètres de diagnostic**, cliquez sur **Activer les diagnostics**.

    ![Activer la journalisation des diagnostics](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Activer les journaux de diagnostic")

3. Dans le panneau **Paramètres de diagnostic**, apportez les modifications suivantes pour configurer la journalisation des diagnostics.
   
    ![Activer la journalisation des diagnostics](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Activer les journaux de diagnostic")
   
   * Dans le champ **Nom**, entrez une valeur pour la configuration du journal de diagnostic.
   * Vous pouvez choisir de stocker/traiter les données de manières différentes.
     
        * Sélectionnez l’option **Archive to a storage account (Archiver dans un compte de stockage)** pour stocker les journaux dans un compte de stockage Azure. Utilisez cette option si vous souhaitez archiver les données qui seront traitées par lots à une date ultérieure. Si vous sélectionnez cette option, vous devez fournir un compte de stockage Azure sur lequel enregistrer les journaux.
        
        * Sélectionnez l’option **Stream to an event hub (Transmettre à un Event Hub)** pour transmettre les données journalisées à un Event Hub Azure. Vous allez probablement utiliser cette option si vous disposez d’un pipeline de traitement en aval pour analyser les journaux entrants en temps réel. Si vous sélectionnez cette option, vous devez fournir les informations relatives au Event Hub Azure que vous souhaitez utiliser.

        * Sélectionnez l’option **envoyer à Log Analytique** à utiliser le service Azure Monitor pour analyser les données de journal généré. Si vous sélectionnez cette option, vous devez fournir des informations détaillées concernant l’espace de travail Log Analytics que vous allez utiliser pour analyser le journal. Consultez [afficher ou analyser les données collectées avec la recherche de journaux Azure Monitor](../azure-monitor/learn/tutorial-viewdata.md) pour plus d’informations sur l’utilisation d’Azure Monitor enregistre.
     
   * Spécifiez si vous souhaitez obtenir des journaux d’audit ou des journaux de demande ou les deux.
   * Spécifiez le nombre de jours pendant lesquels les données doivent être conservées. La rétention ne s’applique que si vous utilisez un compte de stockage Azure pour archiver les données du journal.
   * Cliquez sur **Enregistrer**.

Une fois que vous avez activé les paramètres de diagnostic, vous pouvez consulter les journaux dans l’onglet **Journaux de diagnostic** .

## <a name="view-diagnostic-logs-for-your-data-lake-storage-gen1-account"></a>Afficher les journaux de diagnostic de votre compte Azure Data Lake Storage Gen1
Il existe deux manières d’afficher les données de journal de votre compte Data Lake Storage Gen1.

* À partir de la vue des paramètres du compte Data Lake Storage Gen1
* À partir du compte de Stockage Azure dans lequel les données sont stockées

### <a name="using-the-data-lake-storage-gen1-settings-view"></a>Utilisation de la vue des paramètres Azure Data Lake Storage Gen1
1. Dans le panneau **Paramètres** de votre compte Azure Data Lake Storage Gen1, cliquez sur **Journaux de diagnostic**.
   
    ![Afficher les journaux de diagnostic](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Afficher les journaux de diagnostic") 
2. Dans le panneau **Journaux de diagnostic**, vous devez voir les journaux classés par **journaux d’audit** et **journaux de requête**.
   
   * Les journaux de requête capturent chaque demande d’API effectuée sur le compte Azure Data Lake Storage Gen1.
   * Les journaux d’audit sont similaires aux journaux de requête, mais ils fournissent une analyse beaucoup plus détaillée des opérations effectuées sur le compte Data Lake Storage Gen1. Par exemple, un simple appel d’API de chargement dans les journaux de demande peut entraîner plusieurs opérations « Ajouter » dans les journaux d’audit.
3. Pour télécharger les journaux, cliquez sur le lien **Télécharger** en regard de chaque entrée de journal.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>À partir du compte de Stockage Azure qui contient des données de journal
1. Ouvrez le panneau du compte de Stockage Azure associé au Data Lake Storage Gen1 pour la journalisation, puis cliquez sur Objets blob. Le panneau **Service Blob** répertorie deux conteneurs.
   
    ![Afficher la journalisation des diagnostics](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Afficher les journaux de diagnostic")
   
   * Le conteneur **insights-logs-audit** contient les journaux d’audit.
   * Le conteneur **insights-logs-requests** contient les journaux de demande.
2. Les journaux sont stockés dans ces conteneurs selon la structure suivante.
   
    ![Afficher la journalisation des diagnostics](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Afficher les journaux de diagnostic")
   
    Par exemple, le chemin d’accès complet à un journal d’audit peut être `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    De même, le chemin complet d’un journal de requête peut être `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=14/m=00/PT1H.json`.

## <a name="understand-the-structure-of-the-log-data"></a>Comprendre la structure des données de journal
Les journaux d’audit et de demande sont au format JSON. Dans cette section, nous examinons la structure JSON des journaux de demande et d’audit.

### <a name="request-logs"></a>journaux de demande
Voici un exemple d’entrée dans le journal de demande au format JSON. Chaque objet blob a un objet racine appelé **enregistrements** qui contient un tableau d’objets du journal.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schéma du journal de requête
| Nom | type | Description |
| --- | --- | --- |
| time |Chaîne |L’horodatage (heure UTC) du journal. |
| ResourceId |Chaîne |L’ID de la ressource sur laquelle l’opération a eu lieu. |
| category |Chaîne |La catégorie du journal. Par exemple, **Demandes**. |
| operationName |Chaîne |Le nom de l’opération qui est journalisée. Par exemple, getfilestatus. |
| resultType |Chaîne |L’état de l’opération. Par exemple, 200. |
| callerIpAddress |Chaîne |L’adresse IP du client qui a effectué la demande. |
| correlationId |Chaîne |L’ID du journal qui peut être utilisé pour regrouper un ensemble d’entrées de journal associées |
| identité |Object |L’identité qui a généré le journal. |
| properties |JSON |Voir les détails ci-dessous. |

#### <a name="request-log-properties-schema"></a>Schéma des propriétés de journal de demande
| Nom | type | Description |
| --- | --- | --- |
| HttpMethod |Chaîne |La méthode HTTP utilisée pour l’opération. Par exemple, GET. |
| path |Chaîne |Le chemin d’accès vers l’emplacement où l’opération a eu lieu. |
| RequestContentLength |int |La longueur du contenu de la demande HTTP. |
| ClientRequestId |Chaîne |L’ID qui identifie de façon unique la demande |
| StartTime |Chaîne |L’heure à laquelle le serveur a reçu la demande. |
| EndTime |Chaîne |L’heure à laquelle le serveur a envoyé une réponse. |

### <a name="audit-logs"></a>Journaux d’audit
Voici un exemple d’entrée dans le journal d’audit au format JSON. Chaque objet blob a un objet racine appelé **records** qui contient un tableau d’objets du journal

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "resultSignature": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_storage_gen1_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schéma du journal d’audit
| Nom | type | Description |
| --- | --- | --- |
| time |Chaîne |L’horodatage (heure UTC) du journal. |
| ResourceId |Chaîne |L’ID de la ressource sur laquelle l’opération a eu lieu. |
| category |Chaîne |La catégorie du journal. Par exemple, **Audit**. |
| operationName |Chaîne |Le nom de l’opération qui est journalisée. Par exemple, getfilestatus. |
| resultType |Chaîne |L’état de l’opération. Par exemple, 200. |
| resultSignature |Chaîne |Détails supplémentaires sur l’opération. |
| correlationId |Chaîne |L’ID du journal qui peut être utilisé pour regrouper un ensemble d’entrées de journal associées |
| identité |Object |L’identité qui a généré le journal. |
| properties |JSON |Voir les détails ci-dessous. |

#### <a name="audit-log-properties-schema"></a>Schéma des propriétés de journal d’audit
| Nom | type | Description |
| --- | --- | --- |
| StreamName |Chaîne |Le chemin d’accès vers l’emplacement où l’opération a eu lieu. |

## <a name="samples-to-process-the-log-data"></a>Exemples de traitement des données de journal
Lors de l’envoi de journaux à partir d’Azure Data Lake Storage Gen1 aux journaux d’Azure Monitor (consultez [afficher ou analyser les données collectées avec la recherche de journaux Azure Monitor](../azure-monitor/learn/tutorial-viewdata.md) pour plus d’informations sur l’utilisation d’Azure Monitor enregistre), la requête suivante retourne une table qui contient un liste des utilisateurs affichent des noms, l’heure des événements et le nombre d’événements pour l’heure de l’événement, ainsi que d’un graphique. Vous pouvez facilement la modifier pour afficher le GUID de l’utilisateur ou d’autres attributs :

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Data Lake Storage Gen1 fournit un exemple de traitement et d’analyse des données de journal. Vous pouvez trouver l’exemple à l’adresse [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Voir aussi
* [Vue d’ensemble d’Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Sécuriser les données dans Data Lake Storage Gen1](data-lake-store-secure-data.md)

