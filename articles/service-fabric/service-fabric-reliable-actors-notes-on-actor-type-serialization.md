<properties
   pageTitle="Spolehlivé objekty actor poznámky actor zadejte serializace | Microsoft Azure"
   description="Tento článek popisuje základní požadavky pro definování serializovatelný tříd, které lze použít k určení služeb struktury spolehlivé účastníky státy a rozhraní"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Poznámky: Služba struktury spolehlivé účastníky zadejte serializace


Argumenty všechny metody, typy výsledků úkolů vrácené obou metod v rozhraní actor a objektů uložených v objektu actor stavu Správce musí být [Dat smlouvy serializovatelný](https://msdn.microsoft.com/library/ms731923.aspx). To se týká rovněž do argumentů různých metod podle [actor události rozhraní](service-fabric-reliable-actors-events.md#actor-events). (Actor události rozhraní metody vždy vrátí void.)

## <a name="custom-data-types"></a>Vlastní datové typy

V tomto příkladu rozhraní následující actor definuje metodu, která vrací vlastním typem dat s názvem `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Rozhraní je impelemented tak, že actor využívající správci stavu pro ukládání `VoicemailBox` objektu:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

V tomto příkladu `VoicemailBox` serializovat objekt při:
 - Objekt jsou přenášena mezi instancí actor a volající.
 - Správce stavu, kde je zachován na disk a replikovat na jiné uzly uložený na objekt.
 
Spolehlivé Actor rámec používá DataContract serializace. Proto vlastní datové objekty a jejich členové musí být označena atributy **DataContract** a **DataMember** , respektive

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Další kroky
 - [Životní cyklus a uvolnění kolekce actor](service-fabric-reliable-actors-lifecycle.md)
 - [Objekt actor časovače a připomenutí](service-fabric-reliable-actors-timers-reminders.md)
 - [Objekt actor události](service-fabric-reliable-actors-events.md)
 - [Vícenásobný actor](service-fabric-reliable-actors-reentrancy.md)
 - [Objekt actor polymorfismus a návrhu objektově orientovaného vzorky](service-fabric-reliable-actors-polymorphism.md)
 - [Objekt actor diagnostiky a sledování výkonu](service-fabric-reliable-actors-diagnostics.md)
