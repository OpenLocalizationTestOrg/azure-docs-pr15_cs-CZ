<properties 
    pageTitle="Služba Bus asynchronní zpráv | Microsoft Azure"
    description="Popis služby Bus asynchronní zasílání zpráv."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchronní zpráv vzorů a dostupnost

Asynchronní zpráv lze provést mnoha různými způsoby. Díky fronty, témata a předplatné podporuje Bus služby Azure asynchrony přes store a přeposílání mechanismus. V normálním (synchronní) operaci posílat zprávy do fronty a témata a přijímání zpráv z fronty a předplatná. Aplikace, kterou jste napsali, závisí na těchto entit vždy je k dispozici. Při změně stavu entity kvůli z různých důvodů, třeba způsob, jak poskytnout omezenou možnost osoba, která můžete splňovat většina potřeb.

Aplikace obvykle asynchronní zpráv vzorků povolit pomocí počet scénáře komunikace. Je možné vytvářet aplikace, ve kterých klientů můžete posílat zprávy služby, i když není spuštěná služba. Aplikace, které prostředí, které můžou pomoct roztržení komunikace, fronty vyrovnání načíst zadáním místo vyrovnávací paměť komunikace. Nakonec se zobrazí při vyrovnávání zatížení jednoduché, ale působivé k distribuci zpráv ve více počítačích.

Pokud chcete zachovat dostupnost některou z těchto položek, zvažte několika různými způsoby, ve kterém můžete zobrazit tyto entity není k dispozici pro trvalé zasílání zpráv systému. Obecně vidíme entity nedostupná aplikace, kterou jsme napsali následující různými způsoby:

- Nejde posílat zprávy.

- Nelze přijímat zprávy.

- Nejde spravovat entity (vytvoření, načíst, aktualizace nebo odstranění entity).

- Nelze kontaktovat službu.

U každé z těchto chyb existují různé selhání režimy podporující nástroj aplikace pokračujte k provedení práce některé úrovni omezenou možnost. Například systému, který můžete odesílat zprávy, ale nikoli je přijímat můžete pořád se objevuje objednávky zákazníků, ale nedají zpracovat těch objednávky. Toto téma popisuje potenciální problémy, které se mohou vyskytnout, a jak zmírnit těchto problémů. Služba Bus má zavádí několik mitigations, které se musí určovat, jestli se do a toto téma popisuje také pravidla pro používání mitigations tyto určovat, jestli se změnami.

## <a name="reliability-in-service-bus"></a>Spolehlivost v Bus služby

Existuje několik způsobů, jak řešit problémy s zprávy a entita a jsou zásady vhodné použití těchto mitigations. Abyste pochopili pokyny, musíte nejprve porozumět, co může selhat v Bus služby. Vzhledem k navrhování Azure systémů všechny tyto problémy jsou obvykle krátkodobý. Na vysoké úrovni různých příčiny nedostupnost vypadat takto:

-   Omezení z externího systému na kterém závisí Bus služby. Omezení dojde na interakce s úložiště a využití prostředků.

-   Problém systému, na kterém závisí Bus služby. Například určitá část úložiště můžete používání narazíte na problémy.

-   Chyba služby Bus v jedné podsystému. V takovém případě výpočetní uzly dostali do nekonzistentní stavu a po restartování sebe sama příčinou všechny entity, které slouží k načtení zůstatek do jiných uzlů. Zase může dojít k krátce zpracování pomalé zprávy.

-   Chyba při Bus služby v rámci Azure datacentra. Toto je "katastrofické selhání" během níž systému nedostupná několik hodin a počet minut.

> [AZURE.NOTE] **Úložiště** termínů můžete znamenají Azure úložiště a SQL Azure.

Služba Bus obsahuje celá řada Zklidňující těchto problémů. Následující části se zabývají jednotlivé položky a jejich odpovídajících mitigations.

### <a name="throttling"></a>Omezení

Pomocí služby Bus omezení umožňuje správu sazba spolupráce zprávy. Každý jednotlivé uzel služby Bus máte entity. Všech entit zajišťuje požadavky na systém z hlediska procesoru, paměti, ukládání a pořádáním. Když některý z těchto fasetami zjistí použití, která překračuje definovaný mezní hodnoty, služby Bus odepřít daný požadavek. Volající obdrží [ServerBusyException][] a opakování po 10 sekund.

Jako omezení rizik musí kód číst chybu a zastavit všechny opakování zprávy aspoň 10 sekund. Vzhledem k tomu, že chyba se může stát přes použitelné části aplikace zákazníky, očekává se, že každý úsek nezávisle na sobě provede logickou opakovat. Kód můžete zmenšit pravděpodobnost omezena povolením oddílů na fronty nebo tématu.

### <a name="issue-for-an-azure-dependency"></a>Problém Azure závislost

Jiné součásti v Azure může občas můžete mít problémy se službou. Třeba při upgradu systému, který používá Bus služby systému můžete dočasně zaznamenat nižší možnosti. Informace k alternativním řešením tyto typy potíží, služby Bus pravidelně kontroluje a implementuje mitigations. Vedle projevů těchto mitigations nezobrazují. Služba Bus používá zpracovat přechodná problémy s úložištěm, systému, který umožňuje operace Odeslat zprávu konzistentně pracovat. Z důvodu přírodní omezení rizik odeslané zprávy může trvat až 15 minut objevit v problémového fronty nebo předplatné a provozu pro operace přijmout. Většina entit nebude obecně zaznamenat tento problém. Však počet entit podle Bus služby v rámci Azure, toto omezení je někdy potřeba pro malou skupinu Bus služeb zákazníkům.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Chyba služby Bus v jedné podsystému

U jakékoli aplikace mohou způsobit okolností interní součástí služby Bus osvobozením od nekonzistentní. Když služby Bus zjistí to, shromažďuje údaje o aplikaci na podporu diagnostikovat příčinách. Jakmile se shromažďují data, po restartování aplikace v pokus o se vraťte do konzistentního stavu. Tento proces proběhne velmi rychle a jsou mnohem kratší výsledky entity zobrazená jako není k dispozici pro až několik minut, když je typické dolů časy.

V těchto případech vygeneruje klientské aplikaci [System.TimeoutException][] nebo [MessagingException][] výjimku. Služba Bus obsahuje omezení rizik pro tento problém ve formě Logika opakování automatické klienta. Jakmile opakovat lhůta a není zprávu doručit, můžete prozkoumat pomocí dalších funkcí jako je [párových obory názvů][]. Párových obory názvů mají ostatní upozornění popisované v tomto článku.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Chyba služby Bus v Azure datacentru

Většina spočítána důvod Chyba instalace v Azure datacentra je selhání nasazení upgradu služeb Bus nebo závislá systému. Jak platformu vyzrávaly, co pravděpodobnost tohoto typu chyby. Selhání datacentra můžete taky nastat z důvodů, které patří:

-   Elektrické výpadku (napájení a generování power zmizí).
-   Připojení (internet konec mezi klienty a Azure).

V obou případech selhání fyzická nebo umělých způsobily tento problém. Pokud chcete tento problém vyřešit a ujistěte se, že vám pořád posílat zprávy, můžete [párových obory názvů][] zpráv v druhém umístění a primární umístění je nastavená jako správný znovu povolit. Další informace najdete v tématu [Doporučené postupy pro izolační aplikace proti služby Bus výpadků a havárie][].

## <a name="paired-namespaces"></a>Párových obory názvů

Funkce [párových obory názvů][] podporuje scénáře ve kterém služby Bus entity nebo nasazení v datovém centru nebude k dispozici. Během této události málo, distribuované systémy pořád musí požadovat nejhorší případech. Obvykle tato událost způsobeno některým prvkům na kterém závisí služby Bus dochází problémy s krátkou platností. Údržbu dostupnosti aplikace během výpadku, služby Bus mohou uživatelé dvěma samostatné obory nejlépe v samostatném datacentrech hostovat zpráv entity. Zbytek tento oddíl je definována následujícím terminologie:

-   Primární názvů: názvů, se kterým aplikace spolupracuje pro odesílání a přijímání operace.

-   Sekundární názvů: obor názvů, která funguje jako zálohu primární názvů. Použití logických operátorů aplikace není komunikovat s Tento obor názvů.

-   Převzetí intervalu: dobu přijmout běžné chyby, aby aplikace kombinace kláves vymění z primární názvů do vedlejší názvů.

Párovaný obory názvů podpory *Poslat dostupnosti*. Odesláno: dostupnost zachová možnost posílání zpráv. Můžete odeslat dostupnosti aplikace musí splňovat následující kritéria:

1.  Zprávy jsou doručeny pouze z primární názvů.

2.  Zprávy odeslané do dané fronty nebo tématu může dorazí mimo pořadí.

3.  Pokud vaše aplikace používá relace, může se ukládají zpráv v rámci relace mimo pořadí. Toto je konec oddílu z normální funkce relací. To znamená, že vaše aplikace používá relací pro logické seskupení zpráv. Stav relace trvá jenom na primární názvů.

4.  Může se ukládají nefunguje zprávy v rámci relace. Toto je konec oddílu z normální funkce relací. To znamená, že vaše aplikace používá relací pro logické seskupení zpráv.

5.  Stav relace trvá jenom na primární názvů.

6.  Primární fronty můžete přechodu do online a začněte přijímání zpráv, než sekundární fronty ukládá všechny zprávy do primární fronty.

V následujících částech jsou uvedeny rozhraní API, jak jsou implementovaná rozhraní API a zobrazuje ukázka kód, který používá tuto funkci. Všimněte si, že jsou fakturační důsledky související s touto funkcí.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync rozhraní API

Funkce párových obory názvů obsahuje metodu [PairNamespaceAsync][] ve třídě [Microsoft.ServiceBus.Messaging.MessagingFactory][] :

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Po dokončení úkolu názvů párování je také dokončení a chtít použít pro všechny [MessageReceiver][], [QueueClient][]nebo [TopicClient][] vytvořené pomocí [MessagingFactory][] instance. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] je základní třídy pro různé typy spojení, které jsou dostupné s objektem [MessagingFactory][] . Je v současné době, pouze odvozené třídy jeden pojmenované [SendAvailabilityPairedNamespaceOptions][], které dostupnost požadavky na Odeslat. [SendAvailabilityPairedNamespaceOptions][] obsahuje sadu konstruktory, které vytvářet na sobě. Prohlížíte konstruktoru se většina parametry, můžete pochopit chování další konstruktory.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Tyto parametry mají následující význam:

-   *secondaryNamespaceManager*: spuštění instance [NamespaceManager][] pro obor sekundární [PairNamespaceAsync][] metoda slouží k nastavení oboru sekundární. Správce názvů slouží k získání seznamu fronty v oboru a aby zkontrolovala existenci fronty požadované rezervu. Pokud tyto fronty neexistuje, se vytvářejí. [NamespaceManager][] vyžaduje možnost vytvořit token s deklaraci **Spravovat** .

-   *messagingFactory*: [MessagingFactory][] instanci sekundární názvů. Objekt [MessagingFactory][] slouží k odesílání a pokud je vlastnost [EnableSyphon][] nastavena na **hodnotu true**, přijímání zpráv ve frontách rezervu.

-   *backlogQueueCount*: počet rezervu fronty vytvořit. Tato hodnota musí být alespoň na úrovni 1. Při odesílání zprávy rezervu, jednu z těchto fronty náhodně zvolili. Nastavte hodnotu na 1, pak jedinou fronty někdy lze nastavit. Když fronty jeden rezervu vygeneruje chyby k tomu dojde, klienta je nemohli zkusit jiné rezervu fronty a odešlete zprávu se nemusí podařit. Doporučujeme nastavení této hodnoty na některé větší hodnotu a výchozí hodnotu 10. Můžete změnit na hodnotu vyšších a nižších v závislosti na množství zpracovávaných dat aplikace odešle po dnech. Jednotlivé rezervu fronty můžou obsahovat až 5 GB zpráv.

-   *failoverInterval*: dobu, po kterou přijímá selhání na obor primární před jedna entita přepnutí na vedlejší názvů. Převzetí služeb při selhání dojít na základě entity entity. Entity v jedné názvů často live v jiné uzlů v rámci služby Bus. Chyba instalace v jedné entity neznamená selhání v jiném. Nastavit tuto hodnotu k [System.TimeSpan.Zero][] k přepnutí sekundární bezprostředně po první, nepřechodná selhání. Chyb, které se aktivují časovače převzetí jsou všechny [MessagingException][] , ve kterém je vlastnost [IsTransient][] NEPRAVDA nebo [System.TimeoutException][]. Další výjimky, například [UnauthorizedAccessException][] nezpůsobí překlopení, protože označují, že klienta chybně nakonfigurované. [ServerBusyException][] nezpůsobí převzetí protože správné vzorek je až 10 sekund, pak zprávu odeslat znovu.

-   *enableSyphon*: označuje, že tento konkrétní párování by měl taky syphon zprávy od oboru sekundárního zpět do primární názvů. Obecně nastavit tuto hodnotu **Nepravda**; aplikace, které posílání zpráv aplikace, které zprávy je nastavit tuto hodnotu **true**. Důvodem je, že často, existují méně příjemce zprávy než zprávu odesílatele. V závislosti na počtu příjemců můžete mít instanci jedné aplikace zpracování úkolů Trativod. Použití mnoha příjemců má fakturační důsledky pro jednotlivé rezervu fronty.

Použití kódu, vytvořte primární instance [MessagingFactory][] , sekundární instance [MessagingFactory][] , sekundární instance [NamespaceManager][] a instanci [SendAvailabilityPairedNamespaceOptions][] . Volání můžou skládat například takto:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Až se dokončí Úloha vrácená metodu [PairNamespaceAsync][] všechno, co je nastaveno a připravená k použití. Před úkol je vrácen, nemusí dokončení všech nutné pro párování pracovat přímo pozadí práce. V důsledku toho neměli zahájení odesílání zpráv, dokud úkol vrátí. Pokud došlo k, například chybná pověření nebo Chyba při vytvoření fronty rezervu, budou tyto výjimky vyvolání po dokončení úkolu. Jakmile úkol vrátí, ověřte, zda byly frontách nalezena nebo vytvořili porovnáním vlastnost [BacklogQueueCount][] [SendAvailabilityPairedNamespaceOptions][] instance. Pro předchozí kód, který operace vypadat takto:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy asynchronní zasílání zpráv v Bus služby, přečtěte si další informace o [párových obory názvů][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Doporučené postupy pro izolační proti služby Bus výpadků a havárie aplikací]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [párových obory názvů]: service-bus-paired-namespaces.md