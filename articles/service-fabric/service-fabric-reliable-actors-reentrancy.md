---
title: Réentrance dans les intervenants Azure Service Fabric | Microsoft Docs
description: Présentation de la réentrance pour Service Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 46682787bac2d60d188384a4078ca2fa1f46ae7a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669025"
---
# <a name="reliable-actors-reentrancy"></a>Réentrance Reliable Actors
Par défaut, le runtime Reliable Actors autorise la réentrance des appels logiques selon le contexte. Cela permet de réentrer des acteurs s'ils se trouvent dans la même chaîne de contexte d'appel. Par exemple, un acteur A envoie un message à un acteur B qui envoie le message à un acteur C. Dans le cadre du traitement du message, si l’acteur C appelle l’acteur A, le message est réentrant et donc autorisé. Tout autre message faisant partie d’un contexte d’appel différent est bloqué au niveau de l’acteur A jusqu’à ce qu’il termine le traitement.

Deux options disponibles pour la réentrance des acteurs sont définies dans l’enum `ActorReentrancyMode` :

* `LogicalCallContext` (comportement par défaut)
* `Disallowed` : désactive la réentrance

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Vous pouvez configurer la réentrance dans les paramètres d’un `ActorService`lors de l’inscription. Le paramètre s’applique à toutes les instances d’acteur créées dans le service d’acteur.

L’exemple suivant montre un service d’acteur qui affecte la valeur `ActorReentrancyMode.Disallowed`au mode de réentrance. Dans ce cas, si un acteur envoie un message réentrant à un autre acteur, une exception de type `FabricException` est levée.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Étapes suivantes
* En savoir plus sur la réentrance dans la [documentation de référence de l’API Actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)
