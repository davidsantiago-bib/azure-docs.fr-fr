---
title: Limites de requête - API de traduction de texte Translator Text
titleSuffix: Azure Cognitive Services
description: Cet article liste les limites de requête pour l’API de traduction de texte Translator Text. Des frais sont facturés sur la base du nombre de caractères, et non pas de la fréquence des requêtes, avec une limite de 5 000 caractères par requêtes. Les limites de caractères sont fonction de l’abonnement, les abonnements F0 étant limités à 2 millions de caractères par heure.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 97b0b6256b7aaf7b42565fe9453fb87a0c414569
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57861568"
---
# <a name="request-limits-for-translator-text"></a>Limites de requête pour la traduction de texte Translator Text

Cet article fournit des seuils de limitation de requêtes pour l’API de traduction de texte Translator Text. Les services incluent la traduction, la translittération, la détection de longueur de phrase, la détection de la langue et les traductions alternatives.

## <a name="character-limits-per-request"></a>Limites de caractères par requête

Chaque requête est limitée à 5 000 caractères. Vous êtes facturé au caractère, et non pas au nombre de requêtes. Il est recommandé d’envoyer des requêtes plus courtes et d’avoir des requêtes en attente à tout moment.

Il n’existe aucune limite sur le nombre de requêtes en attente pour l’API de traduction de texte Translator Text.

## <a name="character-limits-per-hour"></a>Limites de caractères par heure

Votre limite de caractère par heure est basée sur votre niveau d’abonnement Translator Text. Si vous atteindrez ou dépassez cette limite, vous recevrez probablement une réponse de dépassement de quota :

| Niveau | Limite de caractères |
|------|-----------------|
| F0 | 2 millions de caractères par heure |
| S1 | 40 millions de caractères par heure |
| S2 | 40 millions de caractères par heure |
| S3 | 120 millions de caractères par heure |
| S4 | 200 millions de caractères par heure |

Ces limites s’appliquent uniquement aux systèmes génériques de Microsoft. Les systèmes de traduction personnalisés qui utilisent Translator Hub de Microsoft sont limités à 1 800 caractères par seconde.

## <a name="latency"></a>Latence

L’API Translator Text a une latence maximale de 15 secondes à l’aide de modèles standards. Traduction à l’aide de modèles personnalisés a une latence maximale de 25 secondes. Dans ce délai, vous recevez un résultat ou une réponse de délai dépassé. En général, les réponses sont retournées dans un délai de 150 à 300 millisecondes. Temps de réponse peut varier en fonction de la taille de la paire de demande et la langue. Si vous ne recevez pas une traduction ou une [réponse d’erreur](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) pendant ce laps de temps, vous devez vérifier votre connexion réseau et réessayez.

## <a name="sentence-length-limits"></a>Limites de longueur de phrase

Quand vous utilisez la fonction [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence), la longueur des phrases est limitée à 275 caractères. Il existe des exceptions pour les langues suivantes :

| Langage | Code | Limite de caractères |
|----------|------|-----------------|
| Chinois | zh | 132 |
| Allemand | de | 290 |
| Italien | it | 280 |
| Japonais | ja | 150 |
| Portugais | pt | 290 |
| Espagnol | es | 280 |
| Italien | it | 280 |
| Thaï | th | 258 |

> [!NOTE]
> Cette limite ne s’applique pas aux traductions.

## <a name="next-steps"></a>Étapes suivantes

* [Tarification](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Disponibilité régionale](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [Référence de l’API de traduction de texte Translator Text v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
