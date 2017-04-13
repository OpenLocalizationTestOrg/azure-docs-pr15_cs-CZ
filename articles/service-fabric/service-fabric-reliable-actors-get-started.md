<properties
   pageTitle="Začínáme s účastníky spolehlivé struktury služby | Microsoft Azure"
   description="Tento kurz vás provede jednotlivými kroky vytvoření ladění a nasazení jednoduché služeb na základě actor pomocí služby struktury spolehlivé účastníky."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Začínáme s spolehlivé účastníky

> [AZURE.SELECTOR]
- [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
- [Java na Linux](service-fabric-reliable-actors-get-started-java.md)

Tento článek popisuje základy Azure služby struktury spolehlivé účastníky a provede vás vytváření, ladění a nasazení aplikace jednoduché spolehlivé Actor ve Visual Studiu.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, zajištění struktury služby vývojové prostředí nastavení počítače.
Pokud potřebujete nastavit najdete v článku podrobné informace o [tom, jak nastavit vývojové prostředí](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Základní koncepty
Začít s spolehlivé účastníky, stačí pár základních principech:

 * **Objekt actor služby**. Spolehlivé účastníky tvoří spolehlivé služby, které můžou být nasazené v infrastruktury služby struktury. Objekt actor instance jsou aktivní v instanci pojmenované služby.
 
 * **Registrace actor**. Jako spolehlivé služby služby spolehlivé Actor musí registrovaný u modul runtime služby struktury. Kromě toho, typ actor musí registrovaný u Actor runtime.
 
 * **Rozhraní actor**. Rozhraní actor slouží k definování silného typu veřejné rozhraní objektu actor. V modelu terminologii spolehlivé Actor rozhraní actor definuje typy zpráv, které objekt actor srozumitelný a proces. Rozhraní actor slouží s jinými účastníky a klientské aplikace "odešlete" (asynchronní) zprávy actor. Spolehlivé účastníky můžete používat více rozhraní.
 
 * **ActorProxy předmětu**. Třídy ActorProxy slouží v klientských aplikacích vyvolat metody vystaven prostřednictvím rozhraní actor. Třídy ActorProxy nabízí dvě důležité funkcí:
    * Překlad: je možné vyhledejte objekt actor v obrázku (najít uzel clusteru, kde je hostovaný).
    * Chyba při zpracování: můžete opakovat metoda vyvolání a znovu najít umístění actor po, například se nepovede, které si žádá actor mají být přemístěny jiné uzel clusteru.

Následující pravidla, které se týkají actor rozhraní stojí zmínění:

- Nesmí být přetížené metody rozhraní actor.
- Objekt actor rozhraní, které se nesmí mít metody ref a volitelné parametry.
- Obecná rozhraní nejsou podporované.

## <a name="create-a-new-project-in-visual-studio"></a>Vytvoření nového projektu v aplikaci Visual Studio
Po instalaci nástroje služby struktury for Visual Studio můžete vytvořit nový projekt typy. Nové typy projektů jsou ve skupinovém rámečku kategorie **cloudu** dialogového okna **Nový projekt** .


![Nástroje služby struktury for Visual Studio - nového projektu][1]

V dialogovém okně Další můžete vybrat typ projektu, který chcete vytvořit.

![Šablony projektů struktury služby][5]

Projekt Hello World použijeme službu služby struktury spolehlivé účastníky.

Po vytvoření řešení, měli byste vidět následující strukturu:

![Struktura služby struktury projektu][2]

## <a name="reliable-actors-basic-building-blocks"></a>Spolehlivé objekty actor základní stavební bloky

Typické spolehlivé účastníky řešení se skládá ze tří projektů:

* **Projekt aplikace (MyActorApplication)**. Toto je projektu, který balíčky všechny společné služby pro nasazení. Obsahuje skripty *ApplicationManifest.xml* a prostředí PowerShell pro správu aplikace.

* **Rozhraní projektu (MyActor.Interfaces)**. Toto je projekt, který obsahuje definici rozhraní pro objekt actor. V aplikaci project MyActor.Interfaces můžete definovat rozhraní, které se použije účastníky řešení. Objekt actor rozhraní možné definovat ve jakýkoli projekt s názvem, ale rozhraní definuje actor smlouvy, který je sdílen implementací actor a klienty, volající objekt actor, takže obvykle dává smysl určíte v sestavení, které je nezávislý implementaci actor a smí zobrazovat více projektů.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Projekt služby actor (MyActor)**. Toto je projektu slouží k definování služby struktury služba, která bude hostovat actor. Obsahuje provádění actor. Implementace actor je třída, která je odvozena z základní typ `Actor` a implementuje rozhraní, které jsou definovány v aplikaci project MyActor.Interfaces. Třídu actor musíte také implementovat konstruktor, který přijme `ActorService` instance a `ActorId` a předá při daném základu `Actor` předmětu. Tato funkce umožňuje konstruktor závislost vkládání závislostí platformu.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Služba actor registrovaný u typu služby v modulu runtime služby struktury. Aby služba Actor ke spuštění actor instancí typ actor taky registrovaný u službu Actor. `ActorRuntime` Způsob registrace provede tuto akci pro objekty actor.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

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

Pokud začnete z nového projektu ve Visual Studiu a máte jenom jednu definici actor, registraci je standardně součástí kód generovaný Visual Studio. Když definujete ostatní účastníky ve službě, potřebujete přidat registrace actor pomocí:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Modul runtime služby struktury účastníky posílá některé [událostí a výkonnosti související s actor metody](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Jsou užitečné v diagnostických nástrojů a sledování výkonu.


## <a name="debugging"></a>Ladění

Nástroje služby struktury for Visual Studio domovské stránce podpory ladění na místním počítači. Spuštění relace s ladění zasažení klávesu F5. Visual Studio vytvoří (v případě potřeby) balíčky. Také nasadí aplikace místní clusteru služby struktury a připojí ladění.

Během procesu nasazení uvidíte průběhu v okně **výstupu** .

![Ladění okně výstupu struktury služby][3]


## <a name="next-steps"></a>Další kroky
 - [Použití služby struktury platformy spolehlivé účastníky](service-fabric-reliable-actors-platform.md)
 - [Správa stavu actor](service-fabric-reliable-actors-state-management.md)
 - [Životní cyklus a uvolnění kolekce actor](service-fabric-reliable-actors-lifecycle.md)
 - [Objekt actor rozhraní API dokumentaci](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ukázkový kód](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
