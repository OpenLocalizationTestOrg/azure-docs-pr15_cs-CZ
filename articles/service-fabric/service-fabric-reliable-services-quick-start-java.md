<properties
   pageTitle="Začínáme s spolehlivé služby | Microsoft Azure"
   description="Úvod k vytvoření aplikace Microsoft Azure služby struktury s příslušnosti a stavové služby."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Začínáme s spolehlivé služby

> [AZURE.SELECTOR]
- [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
- [Java na Linux](service-fabric-reliable-services-quick-start-java.md)

Tento článek popisuje základy Azure struktury spolehlivé služby a provede vás provede vytvoření a nasazení jednoduché spolehlivé služby napsaného v Java.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, zkontrolujte, jestli že máte vývojové prostředí služby struktury nastavení počítače.
Pokud potřebujete nastavit, přejděte na [Začínáme na Macu](service-fabric-get-started-mac.md) nebo [Začínáme na Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Základní koncepty
Začít s spolehlivé služby, stačí pár základních principech:

 - **Typ služby**: Toto je vaší implementací služby. Je definován třídy jste napsali, která slouží k rozšíření `StatelessService` a jakékoli další kód nebo závislosti použité v něm společně s název a číslo verze.

 - **Pojmenovaná služby instance**: Chcete-li spuštění služby, vytvořte pojmenované instance typu služby mnohem, jako se vytvořit instance objektu typu třídy. Instance služby, které jsou ve skutečnosti objekt instancemi svojí třídě služby, který napíšete. 

 - **Hostitele služby**: instance pojmenované služby vytvoříte potřeba spustit uvnitř hostitele. Hostitele služby je právě proces kde mohlo by umožnit spuštění instancí služby.

 - **Registrace služby**: registrace spojují vše. Typ služby registrovaný u runtime modul služby struktury v hostitele služby umožňuje služby struktury vytváření instancí ho spusťte.  

## <a name="create-a-stateless-service"></a>Vytvoření příslušnosti služby

Začněte tak, že vytvoříte novou aplikaci služby struktury. Služby struktury Linux obsahuje Yeoman generátor poskytnout generování uživatelského rozhraní pro aplikaci služby struktury příslušnosti služby. Začněte tím, že spuštěné následující Yeoman příkaz:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů a vytvořte **Spolehlivé příslušnosti služby**. Tento kurz název aplikace "HelloWorldApplication" a "Hello World". Výsledek bude obsahovat adresářů `HelloWorldApplication` a `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementace služby

Otevřete **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Tato třída definuje typ služby a mohlo by umožnit spuštění jakýkoli kód. Rozhraní API služeb nabízí dvě vstupní body kódu:

 - Metody čárky otevřený položku s názvem `runAsync()`, kde můžete začít provádění všech úloh, včetně dlouho probíhajících výpočetním úloh.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Komunikace vstupní bod místo, kam můžete zapojíte do zásobníku komunikace podle výběru. Je to, kde můžete začít přijímá žádosti z uživatelů a dalších služeb.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

V tomto kurzu budeme se zaměřuje na `runAsync()` bod metody zadávání. Je to, kde můžete ihned začít spuštěním kódu.

### <a name="runasync"></a>RunAsync

Platformu volá tuto metodu po umístěné a je připraven k provedení instanci služby. Otevření nebo uzavření obrázku instanci služby může dojít opakovaně pokoušeli přes životnost službu jako celek. K tomu může dojít různých důvodů, včetně:

- Systém přesune instancí služby Vyrovnávání zdroje.
- K chybám dochází v kódu.
- Aplikace nebo systém upgradu.
- Základní hardware dojde výpadku.

Tento průběhu spravují pomocí služby struktury k synchronizaci služby vysoce dostupné a správně vyrovnané.

#### <a name="cancellation"></a>Zrušení

Je důležité, kód do `runAsync()` můžete zastavíte upozornění struktury služby. `CompletableFuture` Vrácených `runAsync()` se zruší služby struktury vyžaduje službu zastavíte. Následující příklad ukazuje, jak se mají zpracovat událost nastavit jako zrušení: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Registrace služby

Typy služeb registrovaný u modul runtime služby struktury. Podle typu služby `ServiceManifest.xml` a svojí třídě služby implementující `StatelessService`. Registrace služby provádí v hlavní vstupní bod obrázku. V tomto příkladu je proces hlavní vstupní bod `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Spusťte aplikaci

Yeoman vygenerovaných obsahuje gradle skript k vytvoření aplikace a flám skripty pro nasazení a zrušit – nasazení aplikace. Pokud chcete spustit aplikaci, nejprve vytvořit aplikaci gradle:

```bash
$ gradle
```

Zajistíte balíček aplikace služby struktury, které můžou být nasazené pomocí služby struktury Azure rozhraní příkazového řádku. Skript install.sh obsahuje požadované příkazy Azure rozhraní příkazového řádku pro nasazení balíčku aplikace. Spusťte install.sh skript pro nasazení:

```bask
$ ./install.sh
```
