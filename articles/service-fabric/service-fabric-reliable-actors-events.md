<properties
   pageTitle="Spolehlivé objekty actor události | Microsoft Azure"
   description="Úvod do událostí pro službu struktury spolehlivé účastníky."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Objekt actor události
Objekt actor události lze o neodeslání oznámení nejlepší dostupné z objektu actor pro klienty. Objekt actor události jsou sice pro komunikace actor klienta a neměly používat ke komunikaci actor actor.

Následující fragmenty kódu předvedení použití actor události v aplikaci.

Definování rozhraní, které popisuje události zveřejněné actor. Rozhraní musí pocházet z `IActorEvents` rozhraní. Argumenty metody musí být [dat smlouvy serializovatelný](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Metody musí vrátit void, jako událost oznámení jedním ze způsobů a plánování řízené úsilí nejlepší.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Deklarujte události zveřejněné actor v rozhraní actor.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Na straně klienta implementujte obslužná rutina události.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

V klientském počítači vytvořte proxy actor, který publikuje události a přihlášení k odběru svých událostí.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

V případě převzetí služeb při selhání objekt actor nemusí nad jiné zpracování nebo uzel. Actor proxy spravuje aktivní předplatné a automaticky je znovu přihlásí. Můžete určit interval obnovení předplatného prostřednictvím `ActorProxyEventExtensions.SubscribeAsync<TEvent>` rozhraní API. Odhlášení odběru, můžete `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` rozhraní API.

Na objekt actor jednoduše zveřejnit události probíhají. Pokud jsou předplatitele na událost, objekty actor runtime odešle je oznámení.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Další kroky
 - [Vícenásobný actor](service-fabric-reliable-actors-reentrancy.md)
 - [Objekt actor diagnostiky a sledování výkonu](service-fabric-reliable-actors-diagnostics.md)
 - [Objekt actor rozhraní API dokumentaci](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ukázkový kód](https://github.com/Azure/servicefabric-samples)
