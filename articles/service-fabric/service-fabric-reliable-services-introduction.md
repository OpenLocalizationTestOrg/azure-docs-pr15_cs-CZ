<properties
   pageTitle="Základní informace o programovací model služby spolehlivé struktury | Microsoft Azure"
   description="Další informace o programovací model služby struktury spolehlivé služby a začněte psát vlastní služby."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Spolehlivé služby základní informace
Azure struktury služby zjednodušuje psaní a správa příslušnosti a stavové spolehlivé služby. Tohoto dokumentu bude si o:

- Model programování spolehlivé služby příslušnosti a stavové služby.
- Možnosti, které máte, aby se při psaní spolehlivé služby.
- Některé scénáře a příklady použití spolehlivé služby a jak se zapsat.

Spolehlivé služby patří mezi modely programování k dispozici na struktury služby. Další informace o programovací model spolehlivé účastníky najdete v článku [Úvod do služby struktury spolehlivé účastníky](service-fabric-reliable-actors-introduction.md).

V služby struktury službu se skládá z konfigurační kód aplikace a (volitelně) uveďte.

Služba struktury spravuje životnost služeb, z zřizování a nasazení prostřednictvím upgradu a odstranění prostřednictvím [struktury služby Správa aplikací](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Jaké jsou spolehlivých služeb?
Spolehlivé služby poskytuje jednoduché, výkonné a nejvyšší úrovně programovací model vám express to podstatné aplikaci. Spolehlivé služeb programování modelu dostanete:

- Stavové státní model programování spolehlivé služby umožňuje konzistentní a problémy se spolehlivým ukládat stavu vpravo do služby pomocí spolehlivé kolekcí. Toto je jednoduchý sadu vysoce dostupné kolekce tříd, které budou znát každý, kdo má používané C# kolekce. Obvykle služby potřebné externí systémy pro správu spolehlivé stavu. S spolehlivé kolekce se mohou být uloženy váš stav vedle svého výpočetním stejné dostupnost a spolehlivost, které jste nenajdete očekávat z důrazně dostupné externí obchody a s přesností další latence umožňující spoluvytváření umístění pro využití a stavu.

- Jednoduchého modelu pro spuštění vlastního kódu, který vypadá jako programování modely, které jste zvyklí. Kód má dobře definovaný vstupní bod a snadno spravovaných životního cyklu.

- Model připojitelné komunikace. Použití transport podle svého výběru, například HTTP se [Rozhraní API webových](service-fabric-reliable-services-communication-webapi.md)WebSockets, vlastní protokoly TCP, atd. Spolehlivé služby obsahují některé skvělé možnosti mimo pole můžete použít, nebo můžete zadat vlastní.

## <a name="what-makes-reliable-services-different"></a>Co dělá různých spolehlivých služeb?
Spolehlivé služby v služby struktury se liší od služby, které možná jste napsali před. Služba struktury poskytuje spolehlivost, dostupnost, konzistenci a škálovatelnost.  

- **Spolehlivost**– služby ve zůstanou, tak i nedůvěryhodných prostředí, kde může počítače nepovede nebo přístupů k problémům se sítí.

- **Dostupnost**– služby bude dostupný a neodpovídá. (Není to, že nesmí obsahovat služby, které nelze najít nebo přejít z mimo.)

- **Škálovatelnost**– Services jsou oddělené z konkrétní hardwaru a můžete zvětšit nebo zmenšit podle potřeby prostřednictvím přidávání nebo odebírání hardwaru nebo virtuální zdrojů. Služby se snadno rozdělit na oddíly (zejména v případě stavové) ověřit, že můžete nezávislé částí služby měřítko a reagovat na selhání nezávisle na sobě. Nakonec služby struktury podporuje služeb lightweight tím tisíce služby zřízení v rámci jednoho obrázku, spíše než vyžadující nebo přidělíte celé instance OS jedna instance konkrétní úlohu.

- **Konzistence**– žádné informace uložené v této službě může být zaručena konzistentní (Toto nastavení se projeví až stavové služby – Další informace o to později)

## <a name="service-lifecycle"></a>Poskytování služeb
Jestli je vaše služba stavové nebo příslušnosti, spolehlivé služby poskytují jednoduchý životního cyklu, které můžete rychle zapojte kódu a můžete začít.  Způsoby jenom jednu nebo dvě, které je potřeba implementace zobrazíte služby nahoru a spuštěné.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – to je místo, kam službu definuje zásobníku komunikace, který chce používat. Do zásobníku komunikace, třeba [Rozhraní API webových](service-fabric-reliable-services-communication-webapi.md)je co definuje naslouchají koncový bod nebo koncové body pro službu (jak klientů dosáhne ho). Definuje také jak zprávy, které se zobrazí skončí interakce s ostatními kód služby.

- **RunAsync** – to je místo, kam služby spustí obchodní logiky. Zrušení token, který je k dispozici je signál pro při měli zastavit, které fungují. Pokud máte službu, kterou je potřeba neustále přetáhněte zpráv mimo spolehlivé fronta a zpracovat, to je například místo, kam chcete stát, které fungují.

### <a name="service-startup"></a>Spuštění služby

Hlavní události v životním cyklu spolehlivé služby, které jsou:

1. Objekt služby (věc, která je odvozena z příslušnosti služby nebo stavová služba) je vytvořen.

2. `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` Názvem metody poskytující služby umožňující vrátit jeden nebo více posluchače komunikace podle svého výběru.
  - Všimněte si, že toto je nepovinný krok, i když většinou služeb vystavit některé koncový bod přímo.

3. Po vytvoření posluchače komunikace otevřením.
  - Komunikace posluchače mít metodu s názvem `OpenAsync()`, která se nazývá v tomto okamžiku a vracející naslouchají adresa pro službu. Pokud spolehlivé služby musí mít některou z předdefinovaných ICommunicationListeners, to má na starosti za vás.

4. Po otevření, posluchače komunikace `RunAsync()` s názvem metody na hlavní služby.
  - Všimněte si, že `RunAsync()` je nepovinný krok. Pokud službu všechny své práce přímo v odpovědi uživateli pouze hovory, není potřeba ho implementovat `RunAsync()`.

### <a name="service-shutdown"></a>Vypnutí služby

Při vypnutí službu (Pokud chcete odstranit, modernizovaných nebo přesunutý) se zobrazuje zrcadlově pořadí volání: nejdřív token zrušení držitelem `RunAsync()` se zruší; potom `CloseAsync()` je místo toho na posluchače komunikace.

Poznámka o vypnutí stavové služby několik důležitých věcí:

- Služba struktury nebude podporovat jiné otevřené služby primární stav do `CloseAsync` a `RunAsync` vrátili. Pokud používáte posluchače předdefinované komunikace `CloseAsync` způsob zpracování za vás.

- Při žádné časový limit zapnutém vracení z těchto postupů okamžitě ztratili možnost zápisu spolehlivé kolekce a tedy nelze dokončit skutečná práce. Doporučujeme vrátit se co nejrychleji obdrží žádost o zrušení.

## <a name="example-services"></a>Příklad služby
Znalost programovací model, podívejme rychlý přehled dva různé služby zobrazíte, jak tyto použitelné spolupracují.

### <a name="stateless-reliable-services"></a>Příslušnosti spolehlivé služby
Příslušnosti služby je jeden tam, kde je doslova žádný stav zachovány v rámci služby a stav, který je k dispozici je úplně jednorázové a nevyžaduje synchronizace, replikace, trvalé nebo dostupnost.

Zvažte například kalkulačce, který nemá žádnou paměť volání a přijímání všechny podmínky a operace k provádění najednou.

RunAsync() službu v tomto případě může být prázdné, protože bez pozadí-zpracování úkolů, které služba je potřeba udělat. Po vytvoření službu kalkulačky vrátí ICommunicationListener (například [Rozhraní API webových](service-fabric-reliable-services-communication-webapi.md)), které se objeví naslouchají koncového bodu na některé portu. Tento naslouchají koncový bod se připojit k různé metody (Příklad: "Přidat (n1, n2)"), která definovat Kalkulačka na veřejném rozhraní API.

Při volání je volání z klienta, metodu odpovídající vyvolat a kalkulačky služby provádí operace na kartě data k dispozici a vrátí výsledek. Všech států, které nejsou uložena.

Není ukládání vnitřního stavu je tento příklad kalkulačce velmi snadné. Ale nejsou skutečně příslušnosti většinou služeb. Místo toho že externalize svůj stav na některé další úložiště. (Například web appu, která závisí na uchovávání stav relace v obchodě zálohování nebo mezipaměti není úplně příslušnosti.)

Běžné příklad způsob příslušnosti služeb se používají v struktury služby je jako front-end, poskytuje rozhraní API veřejné webové aplikace. Službu front-end potom mluví stavové služeb dokončete žádost uživatele. V tomto případě volání klientů přesměrováni známé portu, například 80, kde je služba příslušnosti přijímá. Tuto službu příslušnosti přijme hovor a určuje, zda je volání z důvěryhodného strany, jako dobře mezi službami, které je určená pro.  Potom službu příslušnosti přepošle volání správný oddíl stavová služba a čeká na odpověď. Pokud službu příslušnosti obdrží odpověď, odpovědi zpátky do původního klienta.

### <a name="stateful-reliable-services"></a>Stavová spolehlivé služby
Stavová služba je taková, která musí mít nějaká část stav uchovává konzistentní a prezentovat v pořadí pro službu (funkce). Zvažte služba neustále vypočítá postupné průměr nějakou hodnotu podle aktualizace, které obdrží. K tomuto účelu musí mít aktuální sadu příchozí žádosti nutnou proces, jakož i aktuální průměr. Služba, která načte, zpracuje a informacemi o v externí úložiště (například Azure objektů blob nebo tabulce úložiště dnes) se stavové. Zachová jenom stavu v úložišti externí stavu.

Většinou služeb dnes obsahují stavu externě, protože externí úložiště je co stanoví spolehlivost, dostupnost, škálovatelnost a konzistence státu. V služby struktury nejsou stavová služba potřebných pro uložení stavu externě; Služba struktury má na starosti tyto požadavky pro kód služby a stav služby.

Řekněme, že chcete napsat služba, která přijímá žádosti o řadu převodů, které je potřeba provést na obrázek a obrázek, který je třeba převést.  Pro tuto službu ho vrátí komunikace posluchače (Předpokládejme, že rozhraní API webových), který se objeví komunikační port a umožňuje odeslání prostřednictvím rozhraní API jako `ConvertImage(Image i, IList<Conversion> conversions)`. V tomto rozhraní API službu může informace žádost uložena ve frontě spolehlivý a vraťte se některé token klientovi tak, aby ho mohl mít přehled o žádosti (protože žádosti může trvat několik minut).

V této službě může být RunAsync složitější. Služba může mít smyčky uvnitř jeho RunAsync použije žádosti o mimo IReliableQueue, provede převodů uvedené, který ukládá výsledky IReliableDictionary, tak, aby při klienta vracejí, dostali převedené obrázky. Zajistit, že i když se něco nepovede obrázek se neztratí, tuto službu spolehlivé by přetáhněte mimo fronty, provádět převody a výsledek uložte do transakce. V tomto případě zprávy je skutečně odebrali pouze fronty a výsledky jsou uložené ve slovníku výsledek po dokončení převody. Když něco se nepovede uprostřed (například počítači spuštěné v této instanci kódu), zůstane žádost ve frontě čeká na zpracování znovu.

Poznámka o této službě je vypadá stejně jako normální .NET služby. Pouze rozdíl je, že podle struktury služby datové struktury, který se používá (IReliableQueue a IReliableDictionary) a proto jsou prováděné důrazně spolehlivé, k dispozici a konzistentní.

## <a name="when-to-use-reliable-services-apis"></a>Kdy použít spolehlivé rozhraní API služeb
Pokud něco z tohoto určení potřeb aplikace služby, měli byste zvážit spolehlivé rozhraní API služeb:

- Budete muset zadat chování aplikace napříč několika jednotek stavu (například objednávek a objednávky řádky).

- Stav vaše aplikace můžete přirozeným modelovat jako spolehlivé slovníky a fronty.

- Váš stav je třeba vysoce dostupné s nízkou latence přístup.

- Aplikace potřebuje pro správu souběžné nebo granularity zpracovaných operací přes jednu nebo více spolehlivé kolekcí.

- Chcete-li spravovat sdělení či ovládacích prvků schématu rozdělení oddílů službě.

- Kód musí prostředí podprocesy runtime.

- Aplikace potřebuje dynamicky vytvořit nebo odstranit spolehlivé slovníky nebo fronty za běhu.

- Budete muset programově určit služby struktury-za předpokladu, že zálohování a obnovení funkcí pro vaše služba stavu *.

- Aplikace potřebuje udržovat historie změn pro jeho měrných jednotek stavu *.

- Chcete-li vyvíjet nebo používat vlastní třetích stran vyvinuté stavu poskytovatelů *.

> [AZURE.NOTE] * Funkce jsou dostupné na webu SDK všeobecně dostupná.


## <a name="next-steps"></a>Další kroky
+ [Spolehlivé Představení aplikace služby](service-fabric-reliable-services-quick-start.md)
+ [Spolehlivé služby rozšířené možnosti použití](service-fabric-reliable-services-advanced-usage.md)
+ [Model programování spolehlivé účastníky](service-fabric-reliable-actors-introduction.md)
