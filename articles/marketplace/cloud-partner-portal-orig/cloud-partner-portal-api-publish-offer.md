---
title: Publier une offre | Microsoft Docs
description: API permettant de publier l’offre spécifiée.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: cb1293a771a137f4df7e36a2b412f68b384f16ef
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806729"
---
<a name="publish-an-offer"></a>Publication d’une offre
================

Démarre le processus de publication pour l’offre spécifiée. Cet appel est une opération de longue durée.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

<a name="uri-parameters"></a>Paramètres URI
--------------

|  **Nom**      |    **Description**                               |  **Type de données** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherid   | Identificateur du serveur de publication, par exemple `contoso`      |   Chaîne       |
|  offerId       | Identificateur de l’offre                                 |   Chaîne       |
|  api-version   | Version la plus récente de l’API                        |   Date         |
|  |  |


<a name="header"></a>En-tête
------

|  **Nom**        |    **Valeur**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Authorization   |  `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Exemple de corps
------------

### <a name="request"></a>Requête

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Propriétés du corps de la requête

|  **Nom**               |   **Description**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  e-mails de notification    | Liste d’adresses e-mail séparées par des virgules pour informer de la progression de l’opération de publication. |
|  |  |


### <a name="response"></a>response

   `Operation-Location: /api/operations/contoso$56615b67-2185-49fe-80d2-c4ddf77bb2e8$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>En-tête de réponse

|  **Nom**             |    **Valeur**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Operation-Location    | URL qui peut être interrogée pour déterminer l’état actuel de l’opération.    |
|  |  |


### <a name="response-status-codes"></a>Codes d’état de réponse

| **Code** |  **Description**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted` : la requête a été acceptée. La réponse contient un emplacement qui peut être utilisé pour suivre l’opération lancée. |
| 400   | `Bad/Malformed request` : le corps de la réponse d’erreur peut fournir plus d’informations.                                                               |
| 422   | `Un-processable entity` : indique que la validation de l’entité à publier a échoué.                                                        |
| 404   | `Not found` : l’entité spécifiée par le client n’existe pas.                                                                              |
|  |  |
