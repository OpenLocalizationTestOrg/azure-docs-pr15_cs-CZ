<properties
   pageTitle="Polymorfismus v rámci spolehlivé účastníky | Microsoft Azure"
   description="Vytvoření hierarchie .NET rozhraní a typů v rámci spolehlivé objekty actor pro opakované použití funkcí a rozhraní API definice."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfismus v rámci spolehlivé účastníky

Spolehlivé účastníky framework umožňuje vytvářet účastníky pomocí mnohých z stejné postupy, které použijete v návrhu objektově orientovaného. Jedním z těchto postupů je polymorfismus, který umožňuje typy a rozhraní dědit z více generalized nadřazených. Dědičnost v rámci spolehlivé účastníky obecně spočívá v modelu .NET s několika další omezení.

## <a name="interfaces"></a>Rozhraní

Spolehlivé účastníky framework vyžaduje definovat alespoň jeden rozhraní provádět podle typu objektu actor. Rozhraní bude použito k vygenerování třídy proxy, kterou můžete použít klienty komunikovat s vaší účastníky. Rozhraní dědit z jiných rozhraní, dokud každé rozhraní, který provádí typu objektu actor a od všech nadřazených nakonec jsou odvozeny od IActor. IActor je definován platformy základní rozhraní pro objekty actor. Klasický polymorfismus příklad použití obrazců proto může vypadat nějak takhle:

![Hierarchie rozhraní pro objekty actor obrazce][shapes-interface-hierarchy]


## <a name="types"></a>Typy

Můžete taky vytvořit hierarchii actor typů, které jsou odvozeny od základní Actor třídy poskytované platformu. V případě obrazce, bude pravděpodobně nutné o základu `Shape` typ:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes z `Shape` můžete přepsat metody od základny.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Poznámka `ActorService` atribut na typ actor. Tenhle atribut říká rámcem spolehlivé Actor, aby měli automaticky vytvořit do služby pro hostování objekty actor tohoto typu. V některých případech můžete chtít vytvořit základní typ, který je určená pouze pro funkce nasdílet podtypy a bude použito nikdy pro vytvoření instance konkrétní účastníky. V těchto případech byste měli použít `abstract` klíčové slovo označíte, že se nikdy vytvoříte actor podle typu.


## <a name="next-steps"></a>Další kroky

- V tématu [jak framework spolehlivé účastníky využívá platformu struktury službu](service-fabric-reliable-actors-platform.md) poskytovat spolehlivost, škálovatelnost a konzistentní stav.
- Informace o životním [cyklu actor](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
