<properties
    pageTitle="Měřítko toku analýzy práce s funkcemi Azure počítače výukové | Microsoft Azure"
    description="Zjistěte, jak správně zobrazit úlohy toku analýzy (oddílů, SH množství a další) při použití funkce Azure počítače výuka."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Změna velikosti toku analýzy práce s funkcemi výukové počítače Azure

Je často snadno nastavení technologie pro analýzu toku úlohy a projít ukázkovými daty. Co můžeme udělat, když potřebujeme stejné úlohu s objemem vyšším dat? Vyžaduje nám vědět, jak konfigurovat úlohu toku analýzy tak, aby se měřítko. V tomto dokumentu abychom se zaměřuje na zvláštní aspekty měřítka toku analýzy úlohy funkcí výukové počítače. Informace o tom, jak měřítko toku analýzy úlohy obecně naleznete v článku [Měřítko úlohy](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Co je Azure počítače výukové funkce v toku analýzy?

Funkce učení počítače v toku analýzy mohou sloužit jako hovoru normální funkce v jazyce dotazu toku analýzy. Za scénu, jsou volání funkce skutečně žádosti o Azure počítače výukové webové služby. Počítač výukové webovým službám domovské stránce podpory "dávky" víc řádků, nazývaný zkrácené dávku ve stejném volání webové služby rozhraní API, zlepšit celkový výkon. Najdete v následujících článcích podrobnosti; [Funkce učení počítače Azure v toku analýzy](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) a [Azure počítače výukové webové služby](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurovat toku analýzy úlohu s funkcemi výukové počítače

Při konfiguraci počítače výukové funkci analýzy toku úlohy, se dvěma parametry vzít v úvahu, velikost dávky volání funkce učení počítače a datových proudů jednotky (SUs) k dispozici úlohy toku analýzy. Zjištění příslušné hodnoty pro tyto, nejdřív rozhodnutí třeba mezi latence a výkon, to znamená latence toku analýzy úlohy a výkon každý dílčí. SUs může vždy přidat úlohy zvýšit výkon a oddíly dotazu toku analýzy, i když další SUs zvyšuje náklady spuštění úlohy.

Proto je důležité zjistit *odchylku* latence úloha toku analýzy. Další latence spustila žádosti o služby Azure počítače výukové zvětšíte přirozeným s velikostí dávku, který bude složený latence úlohy toku analýzy. Na druhé straně zvětšení dávku umožňuje úlohy toku analýzy zpracuje *Další událostí pomocí *stejné číslo * počítače výukové web žádostí o službu. Zvýšení počítače výukové webové služby latence je často podřízeného lineární zvýšení velikost dávky, je důležité zvážit náklady nejefektivnější velikost dávky webové služby strojového výukové v dané situaci. Výchozí velikost dávky žádosti o služby web je 1000 a lze změnit pomocí [Toku analýzy REST API](https://msdn.microsoft.com/library/mt653706.aspx "Toku analýzy REST API") nebo [prostředí PowerShell klienta pro analýzy toku](stream-analytics-monitor-and-manage-jobs-use-powershell.md "prostředí PowerShell klienta pro analýzy proudu").

Po velikost dávky množství datových proudů jednotky (SUs) můžete určí, na základě počtu události, které je potřeba funkci zpracovat sekundu. Další informace o datových proudů jednotky naleznete v článku [toku analýzy měřítko úlohy](stream-analytics-scale-jobs.md#configuring-streaming-units).

Obecně existuje 20 souběžné připojení webové služby strojového výukové pro každý 6 SUs s tím rozdílem, že 1 dílčí projekty a 3 dílčí projekty pošle 20 souběžné připojení taky.  Například pokud sazba zadávání dat je 200 000 události sekundu a velikost dávky je výchozí hodnota 1 000 výsledné latence služby web s listem zkrácené události 1000 je 200 MS. To znamená, že každé připojení můžete 5 žádosti o webové službě výukové počítače do druhého. 20 připojení úlohy toku analýzy může zpracovávat 20 000 v 200 MS a tedy 100 000 události do druhého. Tak zpracuje 200 000 události sekundu, úlohy analýzy toku musí 40 souběžné připojení, kterými 12 SUs. Následující diagram znázorňuje požadavky z úlohy toku analýzy koncový bod počítače výukové webové služby – všechny SUs 6 má 20 souběžné připojení webové služby strojového výukové na max.

![Technologie pro analýzu toku měřítko s příkladem úlohy Machine výukové funkce 2] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Technologie pro analýzu toku měřítko s příkladem úlohy Machine výukové funkce 2")

Obecně ***B*** dávku velikost, ***L*** latence webové služby na velikost dávky B v milisekundách výkon toku analýzy úlohu s ***N*** SUs je:

![Změna velikosti toku analýzy pomocí počítače výukové vzorec funkce] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Změna velikosti toku analýzy pomocí počítače výukové vzorec funkce")

Další pozornost mohou být "maximální současné volání" na straně počítače výukové webové služby, doporučujeme nastavte možnost maximální hodnota (aktuálně 200).

Další informace o toto nastavení prosím přečtěte si [článek měřítko webové služby strojového výuka](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Příklad – myšlenkou analýzy

Následující příklad obsahuje toku analýzy úlohy pomocí analýzy myšlenkou funkce učení počítače popsanou v tomto [kurzu integrace toku analýzy počítače výukové](stream-analytics-machine-learning-integration-tutorial.md).

Dotaz je jednoduchý plně rozdělený dotazu následované funkci **myšlenkou** jak je ukázáno v následujícím příkladu:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Zvažte následující scénář; s výkon 10 000 tweety sekundu musí vytvořen projektu toku analýzy při analýze myšlenkou tweety (události). Použití 1 SH tuto úlohu toku analýzy by mohl zpracování přenosů? Použití výchozí velikost dávku 1000 by měla zachovávat vstupní projektu. Přidaná funkce učení počítače dál má víc než druhé latence, což je obecné generovat výchozí latence analýzy myšlenkou webová služba strojového výukové (s výchozí velikost dávky 1000). Latence **celkovou** nebo začátku do konce projektu toku analýzy budou obvykle trvat několik sekund, než. Prohlédněte si podrobnější do této toku analýzy úlohy *zejména* počítače výukové volání funkce. S velikost dávky jako 1000, výkon 10 000 události bude trvat asi 10 požadavky webové služby. Ani za 1 SH jsou dost souběžné připojení tak, aby zahrnoval tento vstupní přenos.

Ale co když sazba vstupní události zvyšuje 100 x a teď úlohy analýzy toku musí zpracuje 1,000,000 tweety sekundu? Dvěma způsoby:

1.  Zvětšení velikosti dávku nebo
2.  Oddíl vstupní proudu zpracuje události souběžně

S parametrem první úlohy **latence** zvětšíte.

S druhou možnost Další SUs by musel být zřízenou a tedy generovat více souběžné žádostí webové služby strojového výukové. To znamená, že zvětšíte projektu **náklady** .


Předpokládejme, že latence analýzy myšlenkou webová služba strojového výuka je 200 MS dávkách 1000 událost nebo pod, 250ms pro událost 5 000 listů, 300ms pro událost 10 000 listů nebo 500ms pro událost 25 000 listů.

1. Použití první možnosti (**Ne** zřizování další SUs) velikost dávky může zvýší **25 000**. To zase umožní úlohy zpracovat 1,000,000 událostí pomocí 20 souběžné připojení webové služby strojového výukové (latence 500ms za hovor). Další latence úlohy toku analýzy kvůli požadavky funkce myšlenkou proti žádostí webové služby strojového výukové by zvýší to z **200 MS** na **500ms**. Všimněte si, že dávka velikost **nemůže** být lepší nekonečně jako webových služeb strojového výuka vyžaduje velikost datové části žádost o 4 MB nebo menší webová služba žádosti vypršení časového limitu po 100 sekund operace.
2. Použití druhou možnost, velikost dávky ještě zbývá na 1 000 s 200 MS webové služby latence, každých 20 souběžné připojení k webové službě by mohli zpracování 1000 událostí *20* 5 = 100,000 sekundu. Zpracuje 1,000,000 události sekundu, tak by projektu potřebujete 60 SUs. Ve srovnání s první možnost analýzy toku úlohy by proveďte další web dávku žádosti o služby, zase generování vyšší náklady.

Tady je tabulka pro výkon úlohy toku Analytics pro různé SUs a dávku rozměrů (počet případů sekundu).

| velikost dávky (latence ML)  | 500 (200 MS) | 1 000 (200 MS) | 5 000 (250ms) | 10 000 (300ms) | 25 000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SH** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **3 SUs** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **6 SUs** | 2 500 | 5 000 | 20 000 | 30 000 | 50 000 |
| **12 SUs** | 5 000 | 10 000 | 40 000 | 60 000 Kč | 100,000 |
| **18 SUs** | 7,500 | 15 000 | 60 000 Kč | 90,000 | 150 000 |
| **24 SUs** | 10 000 | 20 000 | 80,000 | 120 000 Kč | 200 000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25 000 | 50 000 | 200 000 | než 300 000 | 500 000 |

Nyní máte už dobře porozumět fungování funkcí počítače výuka podle toku analýzy. Pravděpodobně také chápete toku analýzy úlohy "vyžádat" data ze zdroje dat a každý "Vložit" vrátí sadu událostí pro danou úlohu toku analýzy zpracuje. Jak tento vyžádané modelu dopad výuky počítače webové žádosti o služby

Za normálních okolností velikost dávky nastavený pro počítač výukové funkce nebudou přesně dělitelný počet vrácených jednotlivé úlohy toku analýzy "Vložit" událostí. Když k tomu dojde, webové služby strojového výukové se říká s "částečné" listy. Důvodem je ušetří režijních latence další úlohy v slučovací události z vyžádané vložit.

## <a name="new-function-related-monitoring-metrics"></a>Nové funkce související sledování metriky

V oblasti Monitor úlohy toku analýz byly přidány tři další metriky související funkce. Jak je znázorněno na následujícím obrázku jsou funkce požadavky, funkce událostí a ZAMÍTNUTÉ žádosti (funkce).

![Změna velikosti toku analýzy pomocí počítače výukové metriky funkcí] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Změna velikosti toku analýzy pomocí počítače výukové metriky funkcí")

Jsou definována následujícím způsobem:

**Funkce požadavky**: číslo požadavků na (funkce).

**Funkce události**: číslo událostí v žádosti (funkce).

**ZAMÍTNUTÉ funkce žádosti**: počet neúspěšných funkce požadavků.

## <a name="key-takeaways"></a>Klíčové Takeaways  

Shrnutí hlavní body, abyste mohli měřítko toku analýzy úlohu s funkcemi počítače výuka, musí být považovány za následující položky:

1.  Kurz vstupní události
2.  Povolenou latence spuštěná úloha toku analýzy (a tedy velikost dávky žádost o službu strojového výukové web)
3.  Zřizování SUs toku technologie pro analýzu a počet jejích žádosti o služby strojového výukové web (další související funkce náklady)

Plně rozdělený dotazu toku analýzy byl použit jako příklad. V případě potřeby složitější dotaz na [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) je skvělým zdrojem pro získání další nápovědy od týmu služeb toku analýzy.

## <a name="next-steps"></a>Další kroky

Další informace o toku analýzy, najdete tady:

- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
