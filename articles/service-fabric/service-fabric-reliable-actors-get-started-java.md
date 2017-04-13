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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Začínáme s spolehlivé účastníky

> [AZURE.SELECTOR]
- [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
- [Java na Linux](service-fabric-reliable-actors-get-started-java.md)

Tento článek popisuje základy Azure služby struktury spolehlivé účastníky a provede vás vytvoření a nasazení aplikace spolehlivé Actor jednoduché Java.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, zkontrolujte, jestli že máte vývojové prostředí služby struktury nastavení počítače.
Pokud potřebujete nastavit, přejděte na [Začínáme na Macu](service-fabric-get-started-mac.md) nebo [Začínáme na Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Základní koncepty
Začít s spolehlivé účastníky, stačí pár základních principech:

 * **Objekt actor služby**. Spolehlivé účastníky tvoří spolehlivé služby, které můžou být nasazené v infrastruktury služby struktury. Objekt actor instance jsou aktivní v instanci pojmenované služby.
 
 * **Registrace actor**. Jako spolehlivé služby služby spolehlivé Actor musí registrovaný u modul runtime služby struktury. Kromě toho, typ actor musí registrovaný u Actor runtime.
 
 * **Rozhraní actor**. Rozhraní actor slouží k definování důrazně zadaný veřejné rozhraní objektu actor. V modelu terminologii spolehlivé Actor rozhraní actor definuje typy zpráv, které objekt actor srozumitelný a proces. Rozhraní actor slouží s jinými účastníky a klientské aplikace "odešlete" (asynchronní) zprávy actor. Spolehlivé účastníky můžete používat více rozhraní.
 
 * **ActorProxy předmětu**. Třídy ActorProxy slouží v klientských aplikacích vyvolat metody vystaven prostřednictvím rozhraní actor. Třídy ActorProxy nabízí dvě důležité funkcí:
    * Překlad: je možné vyhledejte objekt actor v obrázku (najít uzel clusteru, kde je hostovaný).
    * Chyba při zpracování: můžete opakovat metoda vyvolání a znovu najít umístění actor po, například selhání vyžadovaného actor mají být přemístěny jiné uzel clusteru.

Následující pravidla, které se týkají actor rozhraní stojí zmínění:

- Nesmí být přetížené metody rozhraní actor.
- Objekt actor rozhraní, které se nesmí mít metody ref a volitelné parametry.
- Obecná rozhraní nejsou podporované.

## <a name="create-an-actor-service"></a>Vytvoření služby actor
Začněte tak, že vytvoříte novou aplikaci služby struktury. Služby struktury Linux obsahuje Yeoman generátor poskytnout generování uživatelského rozhraní pro aplikaci služby struktury příslušnosti služby. Začněte tím, že spuštěné následující Yeoman příkaz:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů a vytvořte **Spolehlivé Actor služby**. Tento kurz název aplikace "HelloWorldActorApplication" a actor "HelloWorldActor." Vytvoří následující generování uživatelského rozhraní:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Spolehlivé objekty actor základní stavební bloky

Základní pojmy popsané výše přeložit spolehlivé Actor služby základní stavební bloky.

### <a name="actor-interface"></a>Objekt actor rozhraní

Tato stránka obsahuje definici rozhraní pro objekt actor. Rozhraní definuje actor smlouvy, který je sdílen implementaci actor a klienty, volající actor, takže obvykle smysl určíte v na místě, které je nezávislý implementaci actor a smí zobrazovat více jiných služeb nebo klientské aplikace.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Objekt actor služby 
Tato stránka obsahuje actor implementaci a actor registračního kódu. Třída actor používá rozhraní actor. Toto je místo, kam má vaše actor svou práci.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Registrace actor

Služba actor registrovaný u typu služby v modulu runtime služby struktury. Aby služba Actor ke spuštění actor instancí typ actor taky registrovaný u službu Actor. `ActorRuntime` Způsob registrace provede tuto akci pro objekty actor.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testovací klient

Toto je jednoduchý test klientské aplikaci z aplikace služby struktury testování actor služby spuštěním samostatně. Toto je příklad použití ActorProxy abyste mohli aktivovat a komunikovat s actor instance. Ho není nenasadí s vašimi službami.

### <a name="the-application"></a>Aplikace 

Nakonec balíčky aplikace službu actor a jiných služeb, které byste mohli přidat v budoucnu společná pro nasazení. Jsou v ní na *ApplicationManifest.xml* a blokování majitele balíčku služby actor.

## <a name="run-the-application"></a>Spusťte aplikaci

Yeoman vygenerovaných obsahuje gradle skript k vytvoření aplikace a flám skripty pro nasazení a zrušit – nasazení aplikace. Pokud chcete spustit aplikaci, nejprve vytvořit aplikaci gradle:

```bash
$ gradle
```

Zajistíte balíček aplikace služby struktury, které můžou být nasazené pomocí služby struktury Azure rozhraní příkazového řádku. Skript install.sh obsahuje požadované příkazy Azure rozhraní příkazového řádku pro nasazení balíčku aplikace. Spusťte install.sh skript pro nasazení:

```bask
$ ./install.sh
```
