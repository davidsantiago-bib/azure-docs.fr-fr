---
title: Événements dans les acteurs Azure Service Fabric basés sur un acteur | Microsoft Docs
description: Présentation des événements pour Reliable Actors Service Fabric
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: 9075fc8391e8afa21e3963c1eff6a630c586d647
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58659386"
---
# <a name="actor-events"></a>Événements d’acteurs
Les événements d’acteur sont un moyen d’envoyer des notifications Meilleur effort de l’acteur aux clients. Les événements d’acteur sont conçus pour la communication acteur-client et ne doivent pas être utilisés pour la communication acteur-acteur.

L’extrait de code suivant montre comment utiliser les événements d’acteur dans votre application.

Définissez une interface qui décrit les événements publiés par l'acteur. Cette interface doit être dérivée de l'interface `IActorEvents` . Les arguments des méthodes doivent être [sérialisables en contrat de données](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Les méthodes doivent retourner une valeur nulle, car les notifications d’événement sont unidirectionnelles et Meilleur effort.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
Déclarez les événements publiés par l'acteur dans l'interface d'acteur.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
Côté client, implémentez le gestionnaire d'événements.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

Sur le client, créez un proxy pour l'acteur qui publie l'événement et s'abonne à ses événements.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

En cas de basculement, l’acteur peut basculer sur un nœud ou processus différent. Le proxy de l’acteur gère les abonnements actifs et s’y réabonne automatiquement. Vous pouvez contrôler l’intervalle de réabonnement via l’API `ActorProxyEventExtensions.SubscribeAsync<TEvent>` . Pour vous désabonner, utilisez l’API `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` .

Sur l’acteur, publiez les événements à mesure qu’ils se produisent. Le runtime Actors envoie la notification aux abonnés à l’événement, le cas échéant.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Étapes suivantes
* [Réentrance des acteurs](service-fabric-reliable-actors-reentrancy.md)
* [Diagnostics et surveillance des performances d’acteur](service-fabric-reliable-actors-diagnostics.md)
* [Documentation de référence de l’API d’acteur](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Exemple de code C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Exemple de code C# .NET Core](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Exemple de code Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)
