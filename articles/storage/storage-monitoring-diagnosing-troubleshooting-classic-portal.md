<properties
    pageTitle="Sledování a Diagnostika a odstraňování potíží úložiště | Microsoft Azure"
    description="Pomocí funkcí jako technologie pro analýzu úložiště, protokolování na straně klienta a dalších nástrojů jiných výrobců zobrazíte Diagnostika a řešení potíží s problémy související s Azure úložiště."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Sledování a Diagnostika a odstraňování potíží úložišti tabulek Microsoft Azure

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Základní informace

Diagnostikování a řešení problémů s v aplikace distribuované použitý ve prostředí cloudu může být složitější než v tradiční prostředí. Aplikace můžete nasazenou v infrastrukturu PaaS nebo IaaS místně na mobilním zařízení nebo na některou z kombinací obou těchto. Obvykle aplikace v síti může procházet veřejné a privátní sítě a aplikace může používat víc úložiště technologiemi usnadnění, například tabulek Microsoft Azure úložiště objektů BLOB, fronty nebo soubory kromě jiná data ukládá tak jako relační a dokumentace databáze.

Spravovat tyto aplikace úspěšně sledujte včasným a pochopit, jak Diagnostika a řešení potíží s všechny aspekty je a jejich závislé technologie. Jako uživatel služby Azure úložiště by měl nepřetržitě sledovat úložiště služby, které aplikace používá pro změny neočekávané chování (například nižší než obvykle odezvy) a použijte protokolování k shromažďovat data o podrobnější a analyzovat problém podrobně. Diagnostické informace, které jste získali od sledování i protokolování vám pomůže zjistit příčinu kořenové aplikace zjistil problém. Pak můžete při řešení potíží a určit příslušné kroky, kterými můžete k nápravě ho. Azure úložiště je Azure služby základní a je důležitou součástí většinou řešení, které zákazníci nasadit Azure infrastruktury. Azure úložiště přináší funkce zjednodušit sledování diagnostikování a řešení potíží v aplikace cloudového úložiště.

> [AZURE.NOTE] Účty úložiště k typu replikační části Zone nadbytečné úložiště (ZRS) nemají metriky nebo funkce protokolování povoleno v současné době. 

Praktická průvodce do začátku do konce řešení potíží s přihlášením úložišti Azure aplikací najdete v tématu [Začátku do konce Poradce při potížích s použitím metriky úložišť Azure protokolování, AzCopy a Analyzer zprávy](storage-e2e-troubleshooting.md).

+ [Úvod]
    + [Uspořádání této příručce]
+ [Sledování úložiště služby]
    + [Sledování stavu služeb]
    + [Sledování kapacity]
    + [Sledování dostupnosti]
    + [Sledování výkonu]
+ [Diagnostika problémů s úložiště]
    + [Service zdravotní problémy]
    + [Problémy s výkonem]
    + [Diagnostikování chyby]
    + [Problémy emulátoru úložiště]
    + [Nástroje protokolování úložiště]
    + [Pomocí nástroje protokolování sítě]
+ [Trasování začátku do konce]
    + [Srovnávací dat protokolu]
    + [Žádost o ID klienta]
    + [ID požadavku serveru]
    + [Časová razítka]
+ [Pokyny pro řešení potíží]
    + [Metriky jsou zobrazeny vysoké AverageE2ELatency zhoršeným AverageServerLatency]
    + [Metriky zobrazit zhoršeným AverageE2ELatency a nízké AverageServerLatency ale klienta dochází vysokou latencí]
    + [Metriky zobrazit vysoké AverageServerLatency]
    + [Jste zaznamenali neočekávané zpožděním při doručení zprávy ve frontě]
    + [Metriky zobrazit zvýšení v PercentThrottlingError]
    + [Metriky zobrazit zvýšení v PercentTimeoutError]
    + [Metriky zobrazit zvýšení v PercentNetworkError]
    + [Klient získává 403 HTTP (zakázáno) zprávy]
    + [Klient získává HTTP 404 (nebyl nalezen) zprávy]
    + [Klient získává 409 HTTP (konflikt) zprávy]
    + [Metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace se stavem ClientOtherErrors]
    + [Kapacita metriky zobrazit neočekávané zvýšení využití kapacitu úložiště]
    + [Jste zaznamenali neočekávané restartování virtuálních počítačích, které mají velkého počtu připojených VHD]
    + [Problém nastane pomocí emulátoru úložiště pro vývoj nebo test]
    + [Dochází k potíže s instalací Azure SDK pro .NET]
    + [Máte jiný problém se službou úložiště]
+ [Dodatky]
    + [Dodatek 1: Použití Fiddler k zaznamenání přenosy protokolu HTTP a HTTPS]
    + [Dodatek 2: Použití Wireshark k zaznamenání v síti]
    + [Dodatek 3: Pomocí analyzátoru zprávy Microsoft k zaznamenání v síti]
    + [Dodatek 4: Pomocí aplikace Excel a zobrazovat metriky protokolování údajů]
    + [Dodatek 5: Sledování pomocí aplikace přehledy pro týmovou Visual Studio]

## <a name="introduction"></a>Úvod

Tato příručka, uvidíte, jak používat funkce, jako jsou Azure úložiště analýzy klienta protokolování v knihovně Azure úložiště klienta a dalších nástrojů jiných výrobců zjistit, Diagnostika a řešení potíží s Azure úložiště související s jejich stavem.

![][1]

*Obrázek 1 sledování diagnostiky a řešení potíží*

Tato příručka je určena zobrazíte primárně vývojáři online služeb, které používají úložiště služby Azure a v oboru IT zodpovědné za správu těchto online služeb. Cíle tato příručka je:

- Které usnadňují zdraví a výkonu účtů v úložišti Azure.
- Abyste měli potřebné procesy a nástroje pro snazší rozhodnutí, pokud problému nebo potíží v aplikaci se vztahuje k základnímu úložišti Azure.
- Abyste měli akce pokyny pro řešení problémů související s Azure úložiště.

### <a name="how-this-guide-is-organized"></a>Uspořádání této příručce

V části "[sledování úložiště služby]" popisuje, jak sledovat výkon služby Azure úložiště pomocí technologie pro analýzu metriky úložišť Azure (metriky úložišť) a stavu.

V části "[diagnostikování úložiště problémy]" popisuje Diagnostika problémů s použitím Azure úložiště analýzy protokolování (úložiště protokolování). Také popisuje, jak povolit protokolování na straně klienta pomocí možností v jednom z knihoven klienta například národní knihovna úložiště klienta pro .NET nebo SDK Azure jazyka Java.

V části "[začátku do konce trasování]" popisuje, jak můžete porovnat informace obsažené v různých souborů protokolu a metriky data.

V části "[pokyny pro odstraňování potíží]" poskytuje Poradce při potížích některých běžných problémech souvisejících s úložištěm, ke kterým může dojít.

"[Dodatky]" obsahují informace o použití jiných nástrojů, jako jsou Wireshark a programu Netmon pro analýzu dat sítě paketu Fiddler pro analýzu protokolu HTTP/HTTPS zprávy a Microsoft zprávy Analyzer pro korelace dat protokolu.


## <a name="monitoring-your-storage-service"></a>Sledování úložiště služby

Pokud máte zkušenosti s sledování výkonu systému Windows, si můžete představit metriky úložišť jako úložišti Azure ekvivalent sledování výkonu systému Windows. V metriky úložišť najdete komplexní sady metriky (čítače v aplikaci výkon terminologii) například služba dostupnosti, celkový počet žádosti o služby nebo procentech úspěšné žádosti o službu (Úplný seznam dostupných metriky naleznete v tématu <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Úložiště analýzy metriky tabulky schéma</a> na webu MSDN). Můžete určit, jestli chcete, aby služba úložiště můžete shromažďovat a agregovat metriky každou hodinu nebo každou minutu. Další informace o tom, jak povolit metriky a sledovat úložiště účty najdete v článku <a href="http://go.microsoft.com/fwlink/?LinkId=510865" target="_blank">Povolení metriky úložišť</a> na webu MSDN.

Můžete zvolit, které hodinové metriky chcete zobrazit na portálu klasické Azure a konfigurace pravidla, která upozornit správci e-mailu kdykoli hodinové míru překročí určitou prahovou hodnotu (Další informace najdete na stránce <a href="http://msdn.microsoft.com/library/azure/dn306638.aspx" target="_blank">jak: dostávat oznámení a výstrahy pravidla v Azure</a>). Služba úložiště shromažďuje metriky pomocí nejlepší úsilí, ale nelze zaznamenat všechny operace úložiště.

Obrázek 2 pod znázorňuje stránce sledování na portálu klasické Azure, kde můžete zobrazit metriky například dostupnost, celkový počet požadavků a čísel průměrnou latenci účtu úložiště. Pravidlo oznámení je také nastavené upozornit správce při dostupnost poklesu pod určitou úroveň. Samostatné zobrazování tato data, je možné oblastí pro výzkum tabulku služba úspěch zmenšováním procentní hodnoty jsou nižší než 100 % (Další informace naleznete v části "[metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace s stav ClientOtherErrors]").

![][2]

*Obrázek 2 zobrazení metriky úložišť klasické portálu Azure*

Neustále byste měli sledovat Azure aplikacím jistotu, že jsou správný a provést běžným způsobem tak, že:

- Vytvoření některé metriky podle směrného plánu pro aplikaci, která vám umožní porovnat aktuální data a identifikovat významné změny v chování Azure úložiště a aplikace. V mnoha případech bude hodnot podle směrného plánu metriky specifické pro aplikaci a můžete zavede je po výkonu testování aplikace.
- Nahrávání minute metriky a používání sledování aktivně neočekávané chyby a odchylky například vrcholy pole zobrazuje chyba spočítá nebo žádost o sazby.
- Nahrávání hodinové metriky a používání sledování průměry jako průměrná počty chyby a požádejte sazby.
- Zkoumání potenciálních problémů pomocí nástroje diagnostiky, jak je popsáno dále v části "[problémy úložiště diagnostikování]."

Graf s prostorovým obrázek 3 dole ilustrují, jak výpočet průměru, který bude proveden pro každou hodinu metriky můžete skrýt vrcholy pole v aktivitě. Každou hodinu metriky zobrazí stabilní rychlost požadavků, při minutu metriky zobrazte kolísání, jejichž dokončení trvá opravdu místo.

![][3]

Zbývající Tato část popisuje co byste měli sledovat metriky a proč.

### <a name="monitoring-service-health"></a>Sledování stavu služeb

[Klasický portál Azure](https://manage.windowsazure.com) slouží k zobrazení stavu služby úložiště (a další služby Azure) ve všech oblastech Azure ve světě. To vám umožní zobrazit okamžitě Pokud problém mimo ovládací prvek má vliv na služby úložiště na oblast, kterou používáte pro aplikace.

Klasický portálu Azure můžete taky poskytnout oznámení událostí, které ovlivňují různé služby Azure.
Poznámka: Tyto informace bylo dřív k dispozici spolu s historickými dat na řídicím panelu služby Azure na <a href="http://status.azure.com" target="_blank">http://status.azure.com</a>.

Během klasické portálu Azure shromažďuje informace o stavu z v Azure datacentrech (zevnitř sledování), zvažte také přijímání přístupu k mimo v vytvořte syntetické transakce, kteří pravidelně přístup hostované Azure webové aplikace z více míst. Služby nabízených <a href="http://www.keynote.com/solutions/monitoring/web-monitoring" target="_blank">hlavní</a> <a href="https://www.gomeznetworks.com/?g=1" target="_blank">Gomez</a>a interpretaci aplikace Visual Studio týmu služeb jsou příklady tento mimo přístup. Další informace o přehledech aplikace Visual Studio týmovou naleznete v dodatku "[Dodatek 5: sledování s přehledy aplikace Visual Studio týmu služeb]."

### <a name="monitoring-capacity"></a>Sledování kapacity

Metriky úložišť uloží kapacita metriky pro službu objektů blob protože objektů BLOB obvykle největší část uložená data (při psaní není možné použít metriky úložišť sledování kapacita tabulek a fronty). Pokud jste povolili sledování pro službu objektů Blob, můžete najít tato data v tabulce **$MetricsCapacityBlob** . Metriky úložišť zaznamenává tato data jednou za den a přínosu **RowKey** vám pomohou zjistit, zda řádek obsahuje entitu, která se vztahuje k uživatelských dat (hodnoty **data**) nebo technologie pro analýzu dat (hodnoty **analýzy**). Každá uložené entita obsahuje informace o úložiště (**Kapacita** ve měří se v bajtech) a aktuální počet kontejnerů (**ContainerCount**) a objekty BLOB (**ObjectCount**) používá účtu úložiště. Další informace o metriky kapacita uložené v tabulce **$MetricsCapacityBlob** najdete na webu MSDN <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Schématu úložiště analýzy metriky tabulky</a> .

> [AZURE.NOTE] Byste měli sledovat tyto hodnoty pro nejdříve možné upozornění, že se blížíte omezení kapacitu úložiště účtu. Na portálu klasické Azure na stránce **Sledování** účtu úložiště můžete přidat pravidla výstrah upozorníte, pokud agregační úložiště používá větší než nebo pod mezní hodnoty, které zadáte.

Pokud potřebujete pomoc odhad velikost jednotlivé objekty úložiště, jako jsou objekty BLOB najdete v blogovém příspěvku <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx" target="_blank">Principy Azure úložiště fakturace – šířky pásma, transakce a kapacity</a>.

### <a name="monitoring-availability"></a>Sledování dostupnosti

Měli byste sledovat dostupnost úložiště služby ve vašem účtu úložiště sledováním hodnotu ve sloupci **dostupnost** v tabulkách hodinové nebo minute metriky – **$MetricsHourPrimaryTransactionsBlob** **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Dostupnost** sloupec obsahuje procentuální hodnotu, která označuje dostupnost se služby nebo operaci rozhraní API představované řádku ( **RowKey** zobrazuje obsahuje-li řádek metriky pro službu jako celek nebo pro konkrétní operaci rozhraní API).

Všechny hodnoty a menší než 100 % označuje, že se nedaří některé požadavky úložiště. Zobrazí se, proč se nedaří porovnáním ostatních sloupců metriky data, která zobrazit čísla požadavky s typy různých chyb například **ServerTimeoutError**. Můžete očekávat **dostupnost** spadají dočasně nižší než 100 % důvodů například časové limity přechodná serveru během službu přesune oddíly lepší vyrovnávání zatížení žádost; Logika opakování v klientské aplikaci zacházet s chybovému podmínek. Na stránce <a href="http://msdn.microsoft.com/library/azure/hh343260.aspx" target="_blank"></a> jsou uvedeny typy transakce metriky úložišť zahrnutých do výpočtu jeho **dostupnost** .

Na portálu klasické Azure na stránce **Sledování** účtu úložiště můžete přidat pravidla výstrah upozorníte, pokud **dostupnost** pro službu spadá pod mezní hodnoty, které zadáte.

"[Pokyny pro odstraňování potíží]" sekce Tato příručka popisuje některé běžné úložiště problémy se službou související s dostupnosti.

### <a name="monitoring-performance"></a>Sledování výkonu

Ke sledování výkonu služby úložiště, můžete použít následující metriky z tabulek hodinové a minute metriky.

- Hodnoty v polích **AverageE2ELatency** a **AverageServerLatency** zobrazení Průměrná doba služba úložiště nebo typ operace rozhraní API trvá zpracovávat žádosti o. **AverageE2ELatency** je míra latence začátku do konce, která obsahuje i čas věnovat čtení žádosti a odesílat odpovědi kromě doba zpracovat žádosti (tedy obsahuje latence sítě, jakmile žádost dosáhne služba úložiště); **AverageServerLatency** je míra jenom čas zpracování a tedy nezahrnuje všechny sítě latence související s komunikaci s klientem. Najdete v části "[metriky zobrazit vysoké AverageE2ELatency a nízké AverageServerLatency]" v této příručce pro diskusi důvod, proč může být významný rozdíl mezi těmito dvěma hodnotami.
- Hodnoty ve sloupcích **TotalIngress** a **TotalEgress** zobrazuje celkové množství dat, v bajtech přichází do a přejdete z úložiště služby nebo prostřednictvím určitý typ operace rozhraní API.
- Hodnoty ve sloupci **TotalRequests** zobrazit celkový počet požadavky, které služba úložiště rozhraní API operace dostává. **TotalRequests** je celkový počet požadavky, které služba úložiště přijímá.

Budete obvykle sledovat neočekávané změny v některém z těchto hodnot jako indikátor se problém nepovedlo odstranit, které si žádá vyšetřování.

Na portálu klasické Azure na stránce **Sledování** účtu úložiště můžete přidat pravidla výstrah upozorníte, pokud měřítka pro tuto službu spadají pod nebo být větší než mezní hodnoty, které zadáte.

"[Pokyny pro odstraňování potíží]" sekce Tato příručka popisuje některé běžné úložiště problémy se službou souvisejících s výkonem.


## <a name="diagnosing-storage-issues"></a>Diagnostika problémů s úložiště

Existuje několik způsobů, že jste pravděpodobně se dozvěděli o problém v aplikaci, patří:

- Hlavní selhání, který způsobí aktivaci aplikace chybu nebo přestanou fungovat.
- Významná změn od hodnot podle směrného plánu ve metriky sledujete podle popisu v předchozí části "[sledování úložiště služby]".
- Sestavy před uživateli aplikace, které některé speciální operaci neproběhla podle očekávání nebo že některé funkce nefunguje.
- Generování v aplikaci, která se v protokoly nebo jiné metody, oznámení.

Otázky týkající se služby Azure úložiště obvykle spadají do jedné ze čtyř kategorií:

- Aplikace má problémy s výkonem, vykázaného můžete uživatelům nebo zjištěné při změnami metriky výkonu.
- Došlo k potížím s infrastruktury Azure úložiště na jednu nebo více oblastí.
- Aplikace je dochází k chybě vykázaného můžete uživatelům nebo zjištěné při zvýšení v jednom z metriky počet chyb, které sledujete.
- V průběhu vývoje a test může pomocí místní úložiště emulátoru; můžete narazit některé problémy, které se týkají konkrétně použití emulátoru úložiště.

V následujících částech vytvoření přehledu postupujte podle pokynů a Diagnostika problémů v žádném z těchto čtyř kategorií. V části "[pokyny pro odstraňování potíží]" v této příručce podrobnostmi pro některé běžné problémy, ke kterým může dojít.

### <a name="service-health-issues"></a>Service zdravotní problémy

Problémy se službou stavu jsou obvykle mimo ovládacího prvku. Klasický portálu Azure obsahuje informace o všech problémů, probíhající služby Azure včetně úložiště služby. Pokud jste pro přístup ke čtení Geo nadbytečné úložiště při vytvoření účtu úložiště, pak v případě dat jsou k dispozici v primární umístění aplikace může dočasně přepnout do kopie jen pro čtení v sekundární umístění. K tomuto účelu musí aplikace jít přepnout mezi používáním primárních a sekundárních úložišť a mohli pracovat v režimu snížené funkčnosti daty jen pro čtení. Knihoven Azure úložiště klienta umožňují definování čitelného z sekundárním úložiště v případě, že pro čtení z primární úložiště selže zásad opakovat. Aplikace taky potřebuje mějte na paměti, že je data v sekundární umístění postupně konzistentní. Další informace najdete v tématu v blogovém příspěvku <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/04/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx" target="_blank">Možnosti redundance úložiště Azure a nadbytečné úložiště Geo přístup pro čtení</a>.

### <a name="performance-issues"></a>Problémy s výkonem

Výkon aplikace může být subjektivní, hlavně z pohledu uživatele. Proto, je důležité, abyste měli metriky podle směrného plánu k dispozici pro vám pomůže identifikovat, kde může být problém s výkonem. Spousta faktorů může ovlivnit výkon služby Azure úložiště z pohledu aplikace klienta. Následujících skutečností mohou pracovat ve službě úložiště, klienta nebo síťovou infrastrukturu; Proto je důležité, abyste měli strategie pro identifikaci origin problému s výkonem.

Teď, když jste určili pravděpodobně umístění příčinu problému s výkonem z metriky, můžete soubory protokolu najdete podrobné informace a diagnostikovat potíže dál.

Oddílu, který "[pokyny pro odstraňování potíží]" v této příručce obsahuje další informace o některých běžných výkonu související při potížích můžete narazit.

### <a name="diagnosing-errors"></a>Diagnostikování chyby

Uživatelé aplikace může vás upozorní na chyb hlášených klientské aplikaci. Metriky úložišť také záznamů počty typy různých chyb z úložiště služby například **NetworkError**, **ClientTimeoutError**nebo **AuthorizationError**. Během metriky úložišť pouze záznamy počty typy různých chyb, můžete získat další podrobné informace o jednotlivých požadavků porovnáním serverovou straně klienta a síťové protokoly. Obvykle stavový kód HTTP vrácené služba úložiště vám umožní údajem o proč žádost se nezdařila.

> [AZURE.NOTE] Mějte na paměti, že by měl nejspíš uvidím chybovému chybám: například chyby kvůli přechodná síťové podmínky, nebo aplikací.

V následujících zdrojích na webu MSDN jsou vhodné k principy souvisejících s úložištěm stav a chyby kódy:

- <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">Běžné REST API chyb</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179439.aspx" target="_blank">Kódy chyb služby objektů BLOB</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179446.aspx" target="_blank">Kódy chyb služby fronty</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179438.aspx" target="_blank">Kódy chyb tabulku služba</a>

### <a name="storage-emulator-issues"></a>Problémy emulátoru úložiště

Azure SDK obsahuje úložiště emulátoru spuštění na vývoj workstation. Tento emulátoru napodobuje většina chování služby Azure úložiště a je užitečné při vývoj a testování, což umožňuje spuštění aplikace, které využívají služby Azure úložiště bez nutnosti Azure předplatné a účet Azure úložiště.

"[Pokyny pro odstraňování potíží]" sekce Tato příručka popisuje některé běžné problémy narazili pomocí emulátoru úložiště.

### <a name="storage-logging-tools"></a>Nástroje protokolování úložiště

Protokolování úložiště poskytuje protokolování na straně serveru požadavků úložiště ve vašem účtu Azure úložiště. Další informace o tom, jak povolit protokolování na straně serveru a přístup dat protokolu najdete v článku <a href="http://go.microsoft.com/fwlink/?LinkId=510867" target="_blank">protokolování na straně serveru použití</a> na webu MSDN.

Knihovně úložiště klienta pro .NET umožňuje shromáždit klientských protokolu data, která se vztahuje k úložiště prováděných aplikace. Další informace o tom, jak povolit protokolování na straně klienta a přístup dat protokolu najdete v článku <a href="http://go.microsoft.com/fwlink/?LinkId=510868" target="_blank">protokolování na straně klienta pomocí úložiště klienta knihovny</a> na webu MSDN.

> [AZURE.NOTE] V některých případech (například přidružení zabezpečení se tak mohli ověřovat selhání) uživatel zprávu chybu, které můžete najít žádná data žádost v protokolech na straně serveru úložiště. Můžete použít možnosti protokolování knihovně úložiště klienta zjistit, zda je příčinou problémů v klientském počítači nebo pomocí nástroje pro sledování sítě prošetřit sítě.

### <a name="using-network-logging-tools"></a>Pomocí nástroje protokolování sítě

Můžete zachytit přenos mezi klienta a serveru obsahuje podrobné informace o data, která výměny klienta a serveru a základní stav sítě. Užitečné síťové protokolování nástroje patří:

- Fiddler (<a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>) je bezplatná web ladění proxy server, který umožňuje zkoumat záhlaví a data datové HTTP a HTTPS žádostí a odpovědí zpráv. Další informace najdete v tématu "[dodatku 1: použití Fiddler k zaznamenání přenosy protokolu HTTP a HTTPS]".
- Sledování sítě (Netmon) (<a href="http://www.microsoft.com/download/details.aspx?id=4865" target="_blank">http://www.microsoft.com/download/details.aspx?id=4865</a>) a ve Wiresharku (<a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>) jsou analyzátory protokol bezplatné sítě, které vám umožní zobrazit paketu podrobné informace o širokou škálu síťové protokoly. Další informace o programu Wireshark najdete v tématu "[dodatku 2: použití Wireshark k zaznamenání v síti]".
- Microsoft zprávy Analyzer je nástroj od Microsoftu, který nahrazuje programu Netmon a kromě sběr dat paketu sítě, můžete k prohlížení a analýze dat protokolu nezaznamenávají z dalších nástrojů. Další informace najdete v tématu "[dodatku 3: pomocí analyzátoru zprávy Microsoft k zaznamenání v síti]".
- Pokud chcete provádět test základní připojení ke kontrole, klientského počítače můžete připojit ke službě Azure úložiště v síti, nemůžete dělat toto pomocí nástroje standardní **ping** na straně klienta. Nástroj **tcping** však může použít ke kontrole připojení. **Tcping** je k dispozici ke stažení na <a href="http://www.elifulkerson.com/projects/tcping.php" target="_blank">http://www.elifulkerson.com/projects/tcping.php</a>.

V mnoha případech protokolu data z úložiště protokolování a knihovně úložiště klienta bude postačovat diagnostikovat problém, ale v některých případech může být nutné podrobnější informace, které může poskytnout tyto síťové nástroje protokolování. Příklad použití Fiddler k zobrazení zprávy HTTP a HTTPS umožňuje zobrazit data záhlaví a datové odeslané z úložiště služby, které vám umožní zkoumat jak klientské aplikaci opakování skladování a. Protokol analyzátory například Wireshark pracovat na úrovni paketů umožňuje zobrazit TCP data, která vám umožní Poradce při potížích s ztracených paketů a problémy s připojením. Analýza zprávy můžete pracovat na HTTP a TCP vrstvy.

## <a name="end-to-end-tracing"></a>Trasování začátku do konce

Sledování začátku do konce pomocí různých souborů protokolu je užitečné postup vyšetřování potenciálních problémů. Datum a čas informace z dat metriky slouží jako indikaci kde začít hledat souborů protokolu podrobné informace, které vám pomohou při řešení potíží se změnami.

### <a name="correlating-log-data"></a>Srovnávací dat protokolu

Při prohlížení protokoly v klientských aplikacích, sleduje sítě a úložiště serverovou protokolování, které je důležité, abyste mohli sladit žádosti přes různé protokoly. Soubory protokolu obsahuje mnoho různých polí, které lze použít jako identifikátory korelace. Id žádosti klienta je nejužitečnější pole použít ke koordinaci položek v různých protokoly. Ale v některých případech může být užitečné použít id požadavku serveru nebo časová razítka. Další informace o těchto možnostech naleznete v následujících oddílech.

### <a name="client-request-id"></a>Žádost o ID klienta

Knihovně úložiště klienta automaticky vygeneruje jedinečný klientské žádost o id pro všechny žádosti o.

- V na straně klienta protokol, který vytvoří knihovně úložiště klienta id žádosti klienta v poli se zobrazí **Kód žádosti klienta** o každém zadání protokolu vztahující se k žádosti o.
- V trasování sítě například jednu zachycených Fiddler id žádosti klienta se zobrazuje v žádosti o zprávy jako hodnota záhlaví **x-ms--žádost – id klienta** HTTP.
- V protokolu úložiště protokolování serverovou id žádosti klienta zobrazí ve sloupci ID žádosti klienta.

> [AZURE.NOTE]Je možné pro více požadavků pro sdílení stejného id žádosti klienta, protože klienta můžete přiřadit tuto hodnotu (i když knihovně úložiště klienta automaticky přiřadí novou hodnotu). V případě opakování z klienta všechny pokusy sdílet stejnou id žádosti klienta. V případě dávku odesílaná z klienta má dávku id žádost o jedním klientem.


### <a name="server-request-id"></a>ID požadavku serveru

Služba úložiště automaticky vygeneruje ID žádost o serveru.

- V protokolu úložiště protokolování serverovou id žádosti serveru se zobrazí **žádost o ID záhlaví** sloupce.
- V trasování sítě například jednu zachycených Fiddler id žádosti serveru se zobrazí v odpovědi na zprávy jako hodnotu **x ms žádost o id** HTTP záhlaví.
- V na straně klienta protokol, který vytvoří knihovně úložiště klienta id žádosti o serveru ve sloupci se zobrazí **Text operace** pro záznam s podrobnostmi odpovědi na serveru.

> [AZURE.NOTE] Služba úložiště vždy přiřadí jedinečné serveru id požadavku každé žádost přijme, tak, aby každý pokus o opakovat z klienta a všechny operace zahrnuté v jedné dávce má žádost o id jedinečné serveru.

Pokud knihovna úložiště klienta vyvolá **StorageException** v klientovi, vlastnost **RequestInformation** obsahuje **RequestResult** objekt, který zahrnuje **ServiceRequestID** vlastnost. Získat přístup k objektu **RequestResult** z **OperationContext** instance.

Ukázku kód ukazuje, jak nastavit vlastní hodnoty **ClientRequestId** připojením objektu **OperationContext** žádosti ke službě úložiště. Také ukazuje, jak načíst hodnotu **ServerRequestId** z zprávu s odpovědí.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Časová razítka

Můžete taky časová razítka k vyhledání položek souvisejících protokol, ale dávejte všechny hodiny zkosení mezi klienta a serveru, který může existovat. Vyhledávejte plus a mínus 15 minut, než odpovídající serverovou položky podle časového razítka na straně klienta. Mějte na paměti, že objektů blob metadat pro objekty BLOB obsahující metriky označuje časového rozsahu metriky uložené v objektů blob; To je užitečné, pokud máte hodně objektů BLOB metriky pro stejný minutu nebo hodinu.

## <a name="troubleshooting-guidance"></a>Pokyny pro řešení potíží

V této části vám pomůže s diagnostiky a řešení problémů s některých běžných problémech aplikace může dojít při používání služby Azure úložiště. Vyhledání informací pro určitý problém důležité pomocí následujícího seznamu.

**Poradce při potížích s rozhodovacího stromu**

----------

Souvisí problém s výkonem mezi službami úložiště?

- [Metriky jsou zobrazeny vysoké AverageE2ELatency zhoršeným AverageServerLatency]
- [Metriky zobrazit zhoršeným AverageE2ELatency a nízké AverageServerLatency ale klienta dochází vysokou latencí]
- [Metriky zobrazit vysoké AverageServerLatency]
- [Jste zaznamenali neočekávané zpožděním při doručení zprávy ve frontě]

----------

Souvisí s dostupností jednoho z úložiště služby problém?

- [Metriky zobrazit zvýšení v PercentThrottlingError]
- [Metriky zobrazit zvýšení v PercentTimeoutError]
- [Metriky zobrazit zvýšení v PercentNetworkError]

----------

Klientská aplikace dostává informace o odpověď HTTP 4XX (například 404) z úložiště služby?

- [Klient získává 403 HTTP (zakázáno) zprávy]
- [Klient získává HTTP 404 (nebyl nalezen) zprávy]
- [Klient získává 409 HTTP (konflikt) zprávy]

----------

[Metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace se stavem ClientOtherErrors]

----------

[Kapacita metriky zobrazit neočekávané zvýšení využití kapacitu úložiště]

----------

[Jste zaznamenali neočekávané restartování virtuálních počítačích, které mají velkého počtu připojených VHD]

----------

[Problém nastane pomocí emulátoru úložiště pro vývoj nebo test]

----------

[Dochází k potíže s instalací Azure SDK pro .NET]

----------

[Máte jiný problém se službou úložiště]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriky jsou zobrazeny vysoké AverageE2ELatency zhoršeným AverageServerLatency

Vypálení obrázku z nástroje pro sledování klasické portál Azure příklad se výrazně vyšší než **AverageServerLatency** **AverageE2ELatency** .

![][4]

Všimněte si, že služba úložiště pouze vypočítá míru **AverageE2ELatency** pro úspěšné požadavky a, na rozdíl od **AverageServerLatency**zahrnuje doby trvání klienta data odesílat a přijímat potvrzení z úložiště služby. Rozdíl mezi **AverageE2ELatency** a **AverageServerLatency** proto může být kvůli klientské aplikace je pomalé reagovat nebo kvůli podmínek v síti.

> [AZURE.NOTE] Můžete taky zobrazit **E2ELatency** a **ServerLatency** pro jednotlivé úložiště operace v protokolu datový úložiště protokolování.

#### <a name="investigating-client-performance-issues"></a>Zkoumání problémy s výkonem klienta

Možné příčiny klienta reagovat pomalu zahrnout mají omezený počet dostupných připojení nebo vlákna. Možná budete moct tento problém vyřešit, změnou kód klienta efektivnější (například pomocí asynchronní volání úložiště služby), nebo pomocí větší virtuálního počítače (a další jádra víc paměti).

Pro tabulky a fronty služby algoritmu Nagle může docházet vysoké **AverageE2ELatency** ve srovnání s **AverageServerLatency**: Další informace najdete v tématu publikování <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx" target="_blank">společnosti Nagle algoritmus není popisný směrem malé požadavky</a> na Microsoft Azure úložiště týmového blogu. Algoritmus Nagle v kódu můžete zakázat pomocí třídy **Třída ServicePointManager** v oboru **System.Net** . Je vhodné provést před všechny volat do tabulky nebo fronty služby v aplikaci, protože to neovlivní připojení, které jsou již otevřít. Následující příklad pochází z metody **Application_Start** v roli kolegy.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Měli byste protokoly na straně klienta zobrazíte, kolik žádosti o odeslání klientské aplikace a vyhledat obecné .NET související kritické ve vašem klientovi, jako je využití procesoru, .NET uvolnění paměti, využití sítě nebo paměti (jako výchozí bod pro řešení potíží s klientské aplikace .NET najdete <a href="http://msdn.microsoft.com/library/7fe0dd2y(v=vs.110).aspx" target="_blank">ladění trasování a vytváření profilů</a> na webu MSDN).

#### <a name="investigating-network-latency-issues"></a>Zjišťování problémů latence sítě

Vysokou latencí začátku do konce způsobená síti bývá kvůli přechodná podmínky. Prozkoumejte obou problémům se sítí přechodná a trvalý například zamítnuté pakety pomocí nástrojů, jako jsou Wireshark nebo Microsoft zprávy Analyzer.

Další informace o používání Wireshark problémů se sítí, najdete v článku "[Dodatek 2: použití Wireshark k zaznamenání v síti]."

Další informace o používání Microsoft zprávy Analyzer problémů se sítí, najdete v článku "[Dodatek 3: použití Průvodce analýzou Microsoft Message k zaznamenání v síti]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriky zobrazit zhoršeným AverageE2ELatency a nízké AverageServerLatency ale klienta dochází vysokou latencí

V tomto scénáři je většinou to bývá zpoždění v žádosti o úložiště sahající služba úložiště. Měli byste zkontrolovat, proč požadavky z klienta nejsou aby je šlo prostřednictvím služby objektů blob.

Možné zpoždění odesláním žádosti klienta příčiny mají omezený počet dostupných připojení nebo vlákna. By měl zaškrtněte, pokud klienta provádí více opakování a prošetřit příčinu případě. Můžete udělat toto programově tak, že vyhledává v objektu **OperationContext** přidružený k žádosti a načítání **ServerRequestId** hodnoty. Další informace najdete v tématu ukázka kódu v části "[ID požadavku serveru]."

Pokud v klientovi neexistují problémy, měli byste zkontrolovat potenciální problémy se sítí například ztráty paketu. Pomocí nástrojů Wireshark ATP Microsoft zprávy Analyzer můžete prozkoumat problémům se sítí.

Další informace o používání Wireshark problémů se sítí, najdete v článku "[Dodatek 2: použití Wireshark k zaznamenání v síti]."

Další informace o používání Microsoft zprávy Analyzer problémů se sítí, najdete v článku "[Dodatek 3: použití Průvodce analýzou Microsoft Message k zaznamenání v síti]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metriky zobrazit vysoké AverageServerLatency

V případě vysoké **AverageServerLatency** požadavky na stažení objektů blob byste měli použít protokoly úložiště protokolování zda jsou opakované požadavky na stejné objektů blob (nebo sady objektů BLOB). Kulatý nahrát požadavky, měli byste zkontrolovat, jaké blok velikost klienta používá (například blokuje menší než 64 kB můžete mít za následek režijních nákladů, pokud nejste čtení taky v menší než 64 kB bloky), a pokud více klientů nahrávají bloky stejné objektů blob souběžně. Měli byste taky zkontrolovat metriky za minutu pro vrcholy pole v části požadavky, jejichž výsledkem je větší než počet za druhý cílů škálovatelnost: informace v tématu "[metriky zobrazit zvýšení PercentTimeoutError]."

Pokud se vám zobrazuje vysoké **AverageServerLatency** pro objektů blob, která stáhnout žádosti o při opakované požadavků stejný objektů blob nebo sadu objektů BLOB, měli zvážit, ukládání do mezipaměti těchto objektů BLOB Azure mezipaměti nebo Network služby Azure obsahu doručení (CDN). Pro odeslání žádosti o můžete zlepšit výkon pomocí větší velikost bloku. Pro dotazy na tabulky je také možné provádět v klientech, která provádí operace stejný dotaz a pokud se data nezmění často použití mezipaměti klienta.

Nejvyšší hodnoty **AverageServerLatency** lze také příznakem špatně navržený tabulek nebo dotazů, výsledek vzorce vytvořeného v prohledávání nebo že musíte připojit a připojte ochrany proti vzorku. Další informace najdete v tématu "[metriky zobrazit zvýšení PercentThrottlingError]".

> [AZURE.NOTE] Můžete najít úplný kontrolního seznamu výkonu kontrolního seznamu tady: [Microsoft Azure úložiště výkon a škálovatelnost kontrolního seznamu](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Jste zaznamenali neočekávané zpožděním při doručení zprávy ve frontě

Pokud jste zaznamenali zpoždění mezi aplikace přidá zprávu do fronty a časem, který bude k dispozici pro čtení ve frontě by měl proveďte následující kroky diagnostikovat potíže:

- Ověření, že aplikace úspěšně přidává zprávy do fronty. Zkontrolujte, že aplikace není opakování metodu **AddMessage** několikrát před následných. V knihovně úložiště klienta protokolech jsou uvedené opakované pokusy operací úložiště.
- Ověřte je skew mezi role pracovníka, která přidá zprávu do fronty žádné hodiny a roli pracovníka, která bude číst zprávy ve frontě, díky kterému je zobrazit, jako kdybyste je při zpracování zpoždění.
- Zaškrtněte, pokud je chybné roli pracovníka, která bude číst zprávy ve frontě. Pokud klienta fronty volá metodu **GetMessage** ale přestane odpovídat s odpověď na odeslanou zprávu, zůstane zpráva neviditelné ve frontě až do doby **invisibilityTimeout** vypršení platnosti. V tomto okamžiku zprávy bude k dispozici pro zpracování znovu.
- Zaškrtněte, pokud délka fronty roste určitou dobu. K tomu může dojít, pokud nemáte dostatečný pracovníků k dispozici pro všechny zprávy, které ostatní zaměstnanci jsou uvedení na fronty zpracovat. Také byste měli zkontrolovat, že metriky zobrazíte-li odstranit žádosti selhání a počtu dequeue u zpráv, které může to znamenat opakují neúspěšné pokusy o odstraníte zprávu.
- Prohlédněte si protokolování úložiště protokoly pro všechny operace fronty, které mají vyšší než očekávané hodnoty **E2ELatency** a **ServerLatency** průběhu delšího období počet minut, než obvykle.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metriky zobrazit zvýšení v PercentThrottlingError

Omezení chyby při překročit škálovatelnost cílů služba úložiště. Služba úložiště dělá zajistit, že žádný jednoho klienta nebo klienta může používat službu na ostatní. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure úložiště škálovatelnost a výkonu cílů</a> podrobnosti o cílů rozšiřitelnost pro účty úložiště a výkonu cílů pro oddíly v rámci úložiště účtů.

Pokud míru **PercentThrottlingError** zobrazit zvýšení procento požadavky, které se nedaří se omezení chyba, budete muset prošetřit jednu ze dvou situacích:

- [Přechodné zvýšení PercentThrottlingError]
- [Trvalé zvýšení PercentThrottlingError chyby]

Zvýšení **PercentThrottlingError** výskytů ve stejnou dobu jako zvýšení počtu požadavků na úložiště nebo po počátečním načtení testování aplikace. To může taky projevit v klientovi jako "stav zaneprázdněn 503 Server" nebo "časový limit 500 operace" HTTP zprávy o stavu operacích, které úložiště.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Přechodné zvýšení PercentThrottlingError

Pokud se vám zobrazuje vrcholy pole hodnoty **PercentThrottlingError** , který se shoduje s období vysoké aktivity aplikace, měli byste exponenciální (ne lineární) regrese strategie pro opakované pokusy ve vašem klientovi: tím snížení okamžitého oddílu a Nápověda hladce, vrcholy pole provozu v aplikaci. Další informace o tom, jak implementovat opakovat zásady použití knihovně úložiště klienta najdete na webu MSDN <a href="http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx" target="_blank">Microsoft.WindowsAzure.Storage.RetryPolicies Namespace</a> .

> [AZURE.NOTE] Můžete taky vidět vrcholy pole hodnoty **PercentThrottlingError** , které se neshodují s období vysoké aktivity aplikace: většinou to bývá tady je služba úložiště přesun oddílů zlepšit vyrovnávání zatížení.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Trvalé zvýšení PercentThrottlingError chyby

Pokud se vám zobrazuje neustále vysoké hodnotu **PercentThrottlingError** následující trvalé zvýšení objemu transakce nebo provádíte počáteční zatížení zkoušek aplikace a pak budete muset zjistit aplikace je využití úložiště oddíly a jestli se blíží cílů škálovatelnost účtu úložiště. Například pokud se vám zobrazuje omezení chyby na fronty (který se počítá jako jeden oddíl), potom měli byste zvážit pomocí další fronty rozšířit transakce mezi více oddílů. Pokud se vám zobrazuje omezení chyb v tabulce, budete muset zvažte možnost rozšířit své transakce mezi více oddílů pomocí může používat větší okruh hodnoty klíče oddíl jiné schéma rozdělení. Jednou z běžných příčin tohoto problému je prepend/připojení ochrany proti vzorek kde vyberte datum jako klíč oddílu a potom všechna data na určitý den se jeden oddíl: zatížení, může být výsledkem kritický zápisu. Si zvažte různých oddílů návrh nebo rozhodněte, zda pomocí úložiště objektů blob může být lepší řešení. Měli byste taky zaškrtněte, pokud omezení probíhá důsledku vrcholy pole v přenosy pro vaši a prošetřit možností, jak nástroj vyrovnání vzorku žádostí o.

Pokud distribuovat své transakce přes více oddílů, musíte pořád být informace o limitech škálovatelnost nastavení účtu úložiště. Například pokud jste použili deseti fronty každý zpracování maximálně 2 000 zpráv 1KB za sekundu, bude na celkové maximálně 20 000 zpráv za sekundu účtu úložiště. Pokud potřebujete zpracuje víc než 20 000 entity sekundu, zvažte použití více účtů úložiště. Také byste měli mít na paměti, že velikost vašich požadavků a entity má vliv na kdy služba úložiště omezení klienty: Pokud máte větší požadavky a osoby, se může sníží dříve.

Návrh neefektivní dotazu může docházet k přístupů limity rozšiřitelnost pro oddíly tabulky. Dotaz se filtru, který pouze vybere jedno procento entit v oddílu, ale, kontroluje všechny entity v oddílu bude nutné získat přístup k jednotlivé entity. Každý podnik číst spočítá směrem celkový počet transakce v tomto oddílu; Proto můžete snadno dostanete škálovatelnost cílů.

> [AZURE.NOTE] Testování výkonu by měly odhalit žádné návrhy neefektivní dotazu v aplikaci.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metriky zobrazit zvýšení v PercentTimeoutError

Vaše metriky zobrazit zvýšení v **PercentTimeoutError** pro jeden z úložiště služby. Ve stejnou dobu klient nedostane velkého množství zprávy o stavu "časový limit 500 operace" HTTP operacích, které úložiště.

> [AZURE.NOTE] Může se zobrazit chyby vypršení časového limitu dočasně jako služba úložiště načíst zůstatků požadavky můžete přesunout oddíl k novému serveru.

Metriky **PercentTimeoutError** je agregaci následující metriky: **ClientTimeoutError** **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**a **SASServerTimeoutError**.

Vypršení časového limitu serveru příčinou chybu na serveru. Časové limity klienta příčiny operaci na serveru překročila vypršení časového limitu nastavil klienta. například klienta pomocí knihovně úložiště klienta můžete nastavil časový limit pro operaci pomocí vlastnost **ServerTimeout** třídy **QueueRequestOptions** .

Vypršení časového limitu serveru označovat potíže se službou úložiště, který vyžaduje další vyšetřování. Můžete metriky zobrazíte, pokud jsou zasažení limity rozšiřitelnost pro službu a k identifikaci všechny vrcholy pole v přenos, který může způsobovat potíže. Pokud k potížím dochází přerušovaně, může být kvůli Vyrovnávání zatížení aktivitu v rámci služby. Pokud problém je trvalý a není způsobená aplikace zasažení škálovatelnost omezení služby, vyvoláte by měl případ pro podporu. Pro časové limity klienta musí nastavte, jestli časový limit je nastavena na hodnotu odpovídající v klientovi a buď změnit nastavte hodnotu časového limitu v klientovi nebo zjistěte, jak můžete zlepšit výkon operace ve službě úložiště například optimalizace tabulky dotazů nebo zmenšit velikost zprávy.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metriky zobrazit zvýšení v PercentNetworkError

Vaše metriky zobrazit zvýšení v **PercentNetworkError** pro jeden z úložiště služby. Metriky **PercentNetworkError** je agregaci následující metriky: **NetworkError** **AnonymousNetworkError**a **SASNetworkError**. Tyto vznikají, když službu úložiště rozpozná chybu v síti žádá úložiště klienta.

Nejčastější příčiny tato chyba je klient odpojení před vypršením platnosti časový limit úložiště služby. Měli byste zkontrolovat kód ve vašem klientovi pochopit, proč a kdy se klient odpojí z úložiště služby. Můžete také Wireshark, Microsoft zprávy Analyzer nebo Tcping prozkoumat problémy s připojením k síti z klienta. Tyto nástroje jsou popsány v [dodatky].

### <a name="the-client-is-receiving-403-messages"></a>Klient získává 403 HTTP (zakázáno) zprávy

Pokud klientské aplikace je vyvolání chyb 403 HTTP (zakázáno), to bývá příliš klienta používá vypršela platnost sdílené přístup podpisu (přidružení zabezpečení) odešle žádost o úložiště (i když další možné příčiny hodiny skew neplatné klíče a prázdné záhlaví). Pokud platnost klíč přidružení zabezpečení způsobuje, neuvidíte všechny položky v straně serveru protokolování úložiště dat protokolu. Následující tabulka zobrazuje výběru protokolu klientských generovaného knihovnou úložiště klienta, který znázorňuje tento problém výskytu:

Zdroje|Podrobností|Podrobností|Žádost o id klienta|Operace textu
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informace|3|85d077ab-...|Spuštění operace s umístěním primární umístění režim PrimaryOnly podle.
Microsoft.WindowsAzure.Storage|Informace|3|85d077ab-...|Spuštění synchronní žádost o https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;si = mypolicy&amp;podpis = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm 2FyhNYZEmJNQ % 3D&amp;verze rozhraní api = 2014-02-14.
Microsoft.WindowsAzure.Storage|Informace|3|85d077ab-...|Čekání na odpověď.
Microsoft.WindowsAzure.Storage|Upozornění|2|85d077ab-...|Výjimce čekání na odpověď: vzdálený server vrácena chyba: (403) zakázán.
Microsoft.WindowsAzure.Storage|Informace|3|85d077ab-...|Odpověď. Stavový kód = 403 požádat o ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, obsah MD5 = ETag =.
Microsoft.WindowsAzure.Storage|Upozornění|2|85d077ab-...|Během operace výjimce: vzdálený server vrácena chyba: (403) zakázán.
Microsoft.WindowsAzure.Storage|Informace|3 |85d077ab-...|Kontrola, pokud mají operaci opakovat. Počet opakování = 0, stavový kód HTTP = 403 výjimce = vzdálený server vrácena chyba: (403) zakázán.
Microsoft.WindowsAzure.Storage|Informace|3|85d077ab-...|Další umístění byl nastaven do primární, na základě umístění režimu.
Microsoft.WindowsAzure.Storage|Chyba|1|85d077ab-...|Opakovat zásad neumožňuje pro opakování. Došlo k selhání s vzdálený server vrácena chyba: (403) zakázán.

V tomto scénáři měli byste zkontrolovat proč token přidružení zabezpečení blíží se konec platnosti před klienta odešle tokenu serveru:

- Obvykle byste neměli nastavte počáteční čas při vytváření přidružení klienta k použití okamžitě. Pokud existují malé hodiny rozdíly mezi hostiteli generování přidružení zabezpečení pomocí aktuální čas a služba úložiště je možné pro službu úložiště pro příjem přidružení zabezpečení, který ještě není platný.
- Nastavíte neměli čas krátké vypršení přidružení zabezpečení. Znovu malé hodiny rozdíly mezi hostiteli generování přidružení zabezpečení a služba úložiště může vést k přidružení zabezpečení očividně před vypršením platnosti dříve, než očekávat.
- Znamená parametr verze v klíči přidružení zabezpečení (například **SVP = 2012 02 12**) odpovídající verzi knihovně úložiště klienta používáte. Používejte vždycky nejnovější verzi knihovně úložiště klienta. Další informace o tokenu správy verzí přidružení zabezpečení najdete v článku [Co je nového pro úložišti tabulek Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/14/what-s-new-for-microsoft-azure-storage-at-teched-2014.aspx).
- 
- Pokud obnovit vašeho úložiště přístupové klávesy (klikněte na **Správa přístupových kláves** na kterékoli stránce ve vašem účtu úložiště na portálu klasické Azure) to platnost všechny existující tokeny přidružení zabezpečení. Pokud generovat tokeny přidružení zabezpečení s časem dlouhé vypršení platnosti pro klientské aplikace do mezipaměti to může být problém.

Pokud používáte knihovně úložiště klienta pro vytvoření přidružení zabezpečení tokenů, je snadno vytvářet token platný. Ale pokud jste pomocí rozhraní REST API úložiště a od ruky stavba tokeny přidružení zabezpečení pečlivě přečtěte si téma <a href="http://msdn.microsoft.com/library/azure/ee395415.aspx" target="_blank">Delegování přístupu podpisem sdílené aplikace Access</a> na webu MSDN.

### <a name="the-client-is-receiving-404-messages"></a>Klient získává HTTP 404 (nebyl nalezen) zprávy
Pokud klientské aplikaci zobrazuje zpráva 404 HTTP (nebyl nalezen) ze serveru, to znamená, že na objekt, který klienta při pokusu o (jako je třeba entity, tabulky, objektů blob, kontejner nebo fronty) neexistuje ve službě úložiště. Existuje několik důvodů, třeba:

- [Klient nebo jiným procesem odstraněnou objektu]
- [Problém se tak mohli ověřovat s sdílených přidružení (zabezpečení aplikace Access podpis)]
- [Kód v JavaScriptu klientských nemá oprávnění pro přístup k objektu]
- [Selhání sítě]

#### <a name="client-previously-deleted-the-object"></a>Klient nebo jiným procesem odstraněnou objektu
V situacích, kdy klienta pokouší číst, aktualizovat a odstraňovat dat v úložišti služby je obvykle snadno identifikovat v protokolech na straně serveru předchozí operace, odstranili z úložiště služby požadovaný objekt. Často dat protokolu se zobrazí, jinému uživateli nebo proces odstranil objektu. V protokolu úložiště protokolování serverovou typ operace a požadované klíče objektu sloupce se zobrazí při odstranění klienta objektu.

Scénář, kde je klienta pokusíte vložit objekt nemusí být ihned zřejmé proč výsledkem odpověď HTTP 404 (nebyl nalezen), že klienta je vytvoření nového objektu. Ale pokud klienta vytváří objektů blob ho musíte mít k vyhledání kontejneru objektů blob klienta vytváření zprávy, že musíte mít vyhledání fronty a klienta je přidání řádku musí být nalézt v tabulce.

Protokol klientských z knihovny úložiště klienta umožňuje získat podrobnější vysvětlení z klienta odesílá specifických požadavků na služba úložiště.

Protokol následující klientských generovaných knihovně úložiště klienta ukazuje potíže při klienta nenašli kontejneru pro objektů blob, který je vytvořit. Tento protokol obsahuje podrobnosti o tyto operace úložiště:

Žádost o ID|Operace
---|---
07b26a5d-...|Metoda **DeleteIfExists** odstranění objektů blob kontejner. Všimněte si, že tuto operaci obsahuje žádost **vedoucího** se mají zjišťovat existenci kontejner.
e2d06d78...|Metoda **CreateIfNotExists** vytvořit kontejner objektů blob. Všimněte si, že tuto operaci obsahuje žádost **vedoucí** , která vyhledává existenci kontejner. **Hlavy** chybovou zprávu 404, ale stále.
de8b1c3c-...|Metoda **UploadFromStream** vytvořit objektů blob. Požadavek na **umístění** úspěšný 404 zprávou

Protokol položky:

Žádost o ID |  Operace textu
---|---
07b26a5d-...|Počáteční synchronní žádosti o https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014 června 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Čekání na odpověď.
07b26a5d-... | Odpověď. Stavový kód = 200 požádat o ID = eeead849-... Obsah MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Záhlaví odpovědí byly úspěšně zpracována, pokračuje zbytek operace.
07b26a5d-... | Stahování odpověď textu.
07b26a5d-... | Operace byla úspěšně dokončena.
07b26a5d-... | Počáteční synchronní žádosti o https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014 června 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Čekání na odpověď.
07b26a5d-... | Odpověď. Stavový kód = 202, požádat o ID = 6ab2a4cf-..., obsah MD5 = ETag =.
07b26a5d-... | Záhlaví odpovědí byly úspěšně zpracována, pokračuje zbytek operace.
07b26a5d-... | Stahování odpověď textu.
07b26a5d-... | Operace byla úspěšně dokončena.
e2d06d78-... | Počáteční asynchronní žádosti o https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 2014 června 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Čekání na odpověď.
de8b1c3c-... | Počáteční synchronní žádosti o https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... |  StringToSign = umístění... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE 03 2014 června 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Příprava k zápisu dat žádost.
e2d06d78-... | Výjimce čekání na odpověď: vzdálený server vrácena chyba: (404) nebyl nalezen.
e2d06d78-... | Odpověď. Stavový kód = 404 požádat o ID = 353ae3bc-..., obsah MD5 = ETag =.
e2d06d78-... | Záhlaví odpovědí byly úspěšně zpracována, pokračuje zbytek operace.
e2d06d78-... | Stahování odpověď textu.
e2d06d78-... | Operace byla úspěšně dokončena.
e2d06d78-... | Počáteční asynchronní žádosti o https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
e2d06d78-...|StringToSign = umístění... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE 03 2014 června 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Čekání na odpověď.
de8b1c3c-... | Psaní žádost o data.
de8b1c3c-... | Čekání na odpověď.
e2d06d78-... | Výjimce čekání na odpověď: vzdálený server vrácena chyba: (409) konfliktu.
e2d06d78-... | Odpověď. Stavový kód = 409 požádat o ID = c27da20e-..., obsah MD5 = ETag =.
e2d06d78-... | Stahování chyby odpověď textu.
de8b1c3c-... | Výjimce čekání na odpověď: vzdálený server vrácena chyba: (404) nebyl nalezen.
de8b1c3c-... | Odpověď. Stavový kód = 404 požádat o ID = 0eaeab3e-..., obsah MD5 = ETag =.
de8b1c3c-...| Během operace výjimce: vzdálený server vrácena chyba: (404) nebyl nalezen.
de8b1c3c-... | Opakovat zásad neumožňuje pro opakování. Došlo k selhání s vzdálený server vrácena chyba: (404) nebyl nalezen.
e2d06d78-... | Opakovat zásad neumožňuje pro opakování. Došlo k selhání s vzdálený server vrácena chyba: (409) konfliktu.

V tomto příkladu protokol ukazuje, že je klient prokládání žádosti o metodě **CreateIfNotExists** (žádost o id e2d06d78...) s požadavky na metodu **UploadFromStream** (de8b1c3c-...); To je to, že verze klientské aplikace je asynchronní vyvolání těchto postupů. Změňte kód asynchronní v klientovi zajistit předtím vytvořil kontejneru před pokusem o odeslání všechna data do objektů blob v tomto kontejneru. V ideálním případě by měl vytvoříte všechny kontejnery předem.

#### <a name="SAS-authorization-issue"></a>Problém se tak mohli ověřovat s sdílených přidružení (zabezpečení aplikace Access podpis)

Pokud klientské aplikace se pokusí pomocí klávesy přidružení zabezpečení, který neobsahuje potřebná oprávnění pro operaci, služba úložiště chybovou zprávu HTTP 404 (nebyl nalezen) klientovi. Ve stejnou dobu zobrazí se také nenulová hodnota pro **SASAuthorizationError** v metriky.

Následující tabulka zobrazuje Ukázka zprávy serverovou protokolu ze souboru protokolu úložiště protokolování:

<table>
  <tr>
    <td>Žádost o počáteční čas</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Typ operace</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Žádost o stavu</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>Nastavit informace HTTP stavový kód</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Typ ověřování</td>
    <td>Přidružení zabezpečení</td>
  </tr>
  <tr>
    <td>Typ služby</td>
    <td>Objektů BLOB</td>
  </tr>
  <tr>
    <td>Adresa URL požadavku</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; si = mypolicy&amp;amp; podpis = XXXXX&amp;amp; verze rozhraní api = 2014-02-14&amp;konverzace</td>
  </tr>
  <tr>
    <td>Žádost o záhlaví id</td>
    <td>a1f348d5-8032-4912-93EF-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Žádost o ID klienta</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Měli byste zkontrolovat, proč klientské aplikace pokouší provedení operace, které ho nebyla udělená oprávnění.

#### <a name="JavaScript-code-does-not-have-permission"></a>Kód v JavaScriptu klientských nemá oprávnění pro přístup k objektu

Pokud používáte JavaScript klienta a služba úložiště je vracející HTTP 404, abyste si ověřili následujících chyb JavaScript v prohlížeči:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Nástroje pro vývojáře F12 v Internet Exploreru slouží ke sledování zpráv vyměňovat mezi prohlížečem a služba úložiště při řešení problémů JavaScript straně klienta.

K těmto chybám protože webový prohlížeč implementuje omezení <a href="http://www.w3.org/Security/wiki/Same_Origin_Policy" target="_blank">stejné origin zásady</a> zabezpečení, které zabrání volání rozhraní API v jiné doméně než doménu, kterou stránku pochází z webové stránky.

Informace k alternativním řešením problém JavaScript, můžete nakonfigurovat křížové Origin zdroje sdílení (CORS) úložiště služby, kterou přístupu klienta. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/dn535601.aspx" target="_blank">Sdílení zdrojů mezi Origin (CORS) Podpora služby Azure úložiště</a> na webu MSDN.

Následující příklad kódu ukazuje, jak nakonfigurovat službu objektů blob Povolit JavaScript spuštěné v doméně Contoso pro přístup k objektů blob služby úložiště objektů blob:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Selhání sítě

V některých případech může ztraceny síťové pakety vést k služba úložiště vracející HTTP 404 klientovi. Třeba při klientské aplikace je odstraňování entita z tabulky služby se zobrazí klienta vyvolat úložiště výjimce vykazování "HTTP 404 (nebyl nalezen)" zpráva o stavu z tabulky služby. Při zkoumání tabulky v tabulku služba úložiště uvidíte, že služba odstranit na vyžádání.

Podrobnosti o výjimce v klientovi zahrnout žádost o id (7e84f12d...) přiřazené tabulku služba žádosti: můžete použít tyto informace zjistit podrobnosti o požadavku v protokolech na straně serveru úložiště hledáním ve sloupci **žádost o záhlaví id** v souboru protokolu. Zjistit, jestli selhání například to provést a vyhledejte protokoly na základě času metriky zaznamenané tato chyba se může taky použitím metriky. Tento záznam zobrazuje odstranit skončil stavová zpráva "HTTP (404) klienta jiná chyba". Stejný záznam obsahuje taky id žádosti generovaných klientovi ve sloupci **id žádosti klienta** (813ea74f...).

Protokol serverovou obsahuje taky jiné položky se stejnou hodnotou **id žádosti klienta** (813ea74f...) pro úspěšné operaci pro stejný subjekt a od stejného klienta. Úspěšné odstranění uskutečnila krátce před selhalo odstranění žádosti o.

Většinou to bývá tento scénář je klient odeslání žádost o odstranění entity ke službě tabulky, které bylo úspěšné, ale nebyla přijata odpověď na odeslanou zprávu ze serveru (z nějakého důvodu problém dočasné sítě). Klient potom automaticky opakované operace (pomocí stejné **id žádosti klienta**) a tento opakovat nezdařilo entitu už byl odstraněn.

Pokud tento problém objevuje často, měli byste zkontrolovat, proč se nedaří klienta provést dostanete potvrzení od tabulku služba. Pokud k potížím dochází přerušovaně, byla by měl zachycení chyba "Nebyl nalezen HTTP (404)" a přihlášení klienta, ale povolit klientům pokračovat.

### <a name="the-client-is-receiving-409-messages"></a>Klient získává 409 HTTP (konflikt) zprávy

V následující tabulce jsou uvedeny výpis z protokolu serverovou dvě operace na straně klienta: **DeleteIfExists** následují **CreateIfNotExists** se stejným názvem objektů blob kontejner. Všimněte si, že každý klient operace za následek dva požadavky na server nejdřív **GetContainerProperties** žádost neposlali ke kontrole, pokud existuje kontejneru, následovaný **DeleteContainer** nebo **CreateContainer** žádost o.

Časové razítko|Operace|Výsledek|Název kontejneru|Žádost o id klienta
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Kód v klientské aplikaci odstranit a potom okamžitě znovu vytvořit kontejner objektů blob se stejným názvem: metodu **CreateIfNotExists** (klienta požadavek ID bc881924 –...) se nezdaří s chybou 409 HTTP (konflikt). Když klienta odstraní objektů blob kontejnery, tabulek nebo dotazů je stručný období před názvem znovu k dispozici.

Klientská aplikace používejte jedinečné kontejneru názvy pokaždé, když se vytvoří nový kontejnery při běžných vzorek odstranit/znovu vytvořit.

### <a name="metrics-show-low-percent-success"></a>Metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace se stavem ClientOtherErrors

Metriky **PercentSuccess** zaznamenává procent operacích, které byly úspěšně podle jejich HTTP stavový kód. Operace s stavů 2XX počítání jako úspěšná, že operace s stavů v oblasti 3XX, 4XX a 5XX jsou započteny jako neúspěšné a snížit hodnotu metriky **PercentSucess** . Protokoly straně serveru úložiště jsou tyto operace zaznamenané se stavem transakce **ClientOtherErrors**.

Je důležité mít na paměti, že tyto operace úspěšně dokončené a proto nemají vliv na jiné metriky například dostupnost. Mezi některé příklady operace, úspěšně spustit, ale výsledkem může být úspěšný HTTP stavů patří:
- **ResourceNotFound** (Nebyla nalezena 404), například z požadavek GET objektů blob, který neexistuje.
- **ResouceAlreadyExists** (Konflikt 409), například z **CreateIfNotExist** operace, které už existuje zdroje.
- **ConditionNotMet** (Nezměněné 304), například z podmíněné operace například pokud klient odešle hodnotu **ETag** a HTTP **If-žádné časově POZVYHLEDAT** záhlaví požádat o obrázek jenom v případě, že byl aktualizovaný od posledního operace.

Seznam kódů běžné rozhraní REST API chyb, jejichž výsledkem je úložiště služby na stránce <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">Nejčastější kódy chyb REST API</a>můžete najít.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapacita metriky zobrazit neočekávané zvýšení využití kapacitu úložiště


Pokud se zobrazí náhlé, neočekávané změny kapacita použití ve vašem účtu úložiště, můžete prozkoumat důvody nejprve kontrolou dostupnost metriky; například nárůst počet neúspěšných odstranění žádosti o může vést k nárůst velikosti úložiště objektů blob, které používáte při aplikace konkrétní čištění, které se mohou mít měly být uvolnění místa nepracuje podle očekávání (například z toho důvodu tokeny přidružení zabezpečení používané uvolnit místo vypršela platnost).

### <a name="you-are-experiencing-unexpected-reboots"></a>Jste zaznamenali neočekávané restartováním aplikace virtuálních počítačích Azure, které mají velkého počtu připojených VHD

Pokud Azure virtuálního počítače (OM) obsahuje velké množství připojených VHD, které jsou ve stejném účtu úložiště, překročíte cílů rozšiřitelnost pro jednotlivé úložiště účet příčinou OM selhání. Měli byste zkontrolovat minute metriky úložišť účtu (**TotalRequests**/**TotalIngress**/**TotalEgress**) pro vrcholy pole, které překročení cílů škálovatelnost účtu úložiště. Najdete v části "[že metriky zobrazit zvýšení PercentThrottlingError]" Požádejte o pomoc při určování, zda omezení došlo ke svému účtu úložiště.

Obecně se převádí každý jednotlivé vstup nebo výstup operace virtuálního pevného disku od virtuálního počítače k **Získání** nebo **Umístění stránku** operace v podkladovém objektů blob stránky. Proto můžete odhadovaná procesorů ve vašem prostředí ladění kolik VHD můžete mít v účtu jednoho úložiště na základě konkrétních chování aplikace. Nedoporučujeme s více než 40 disků v účtu jednoho úložiště. Podrobnosti získáte v <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure úložiště škálovatelnost a výkonu cílů</a> aktuální cílů rozšiřitelnost pro účty úložiště, zejména celkové žádost sazby a celkové šířky pásma pro typ účtu úložiště, který používáte.
Pokud jsou vyšší než cílů škálovatelnost účtu úložiště, měli byste vaší VHD umístit do několika různých úložiště účtů zmenšit aktivity v jednotlivé účty.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Problém nastane pomocí emulátoru úložiště pro vývoj nebo test

Obvykle použití emulátoru úložiště během vývoje a otestujte Chcete-li předejít požadavku na účet Azure úložiště. Běžné problémy, které může dojít, pokud používáte emulátoru úložiště jsou:

- [Funkce "X" nefunguje v emulátoru úložiště]
- [Chyba "hodnotu pro jedno ze záhlaví HTTP není ve správném formátu" při použití emulátoru úložiště]
- [Spuštění emulátoru úložiště vyžaduje oprávnění správce]

#### <a name="feature-X-is-not-working"></a>Funkce "X" nefunguje v emulátoru úložiště

Úložiště emulátoru nepodporuje všechny funkce služby Azure úložiště například služba souborů. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/gg433135.aspx" target="_blank">rozdíly mezi emulace úložiště a služby Azure úložiště</a> na webu MSDN.

Pro tyto funkce, které emulátoru úložiště nepodporuje pomocí služby Azure úložiště v cloudu.

#### <a name="error-HTTP-header-not-correct-format"></a>Chyba "hodnotu pro jedno ze záhlaví HTTP není ve správném formátu" při použití emulátoru úložiště

Testování aplikace využívající knihovně úložiště klienta proti emulátoru místní úložiště a metoda volá jako **CreateIfNotExists** nezdaří s chybovou zprávou "hodnotu pro jedno ze záhlaví HTTP není ve správném formátu." Tento údaj označuje, že verze emulátoru úložiště, který používáte nepodporuje verzi knihovně úložiště klienta, který používáte. Knihovně úložiště klienta přidá všechny žádosti o provedené záhlaví **x-ms verze** . Pokud emulátoru úložiště nerozpozná hodnotu v záhlaví **x-ms verze** , požadavek odmítne.

Protokoly úložiště klienta knihovny můžete použít zobrazíte hodnotu **záhlaví x-ms verze** odesílaných. Taky uvidíte hodnotu **záhlaví x-ms verze** použijete Fiddler sledovat požadavky na klientské aplikace.

Tento scénář obvykle dojde, když je nainstalovat a používat nejnovější verzi knihovně úložiště klienta bez aktualizace emulátoru úložiště. Si nainstalovat nejnovější verzi emulátoru úložiště, nebo použijte cloudového úložiště místo emulátor pro vývoj a testování.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Spuštění emulátoru úložiště vyžaduje oprávnění správce

Zobrazí se výzva k zadání přihlašovacích údajů správce při spuštění emulátoru úložiště. Pouze k tomu dojde, když jsou inicializace emulátoru úložiště poprvé. Poté, co jste inicializovali emulátoru úložiště, nemusí administrativním oprávněním znovu ho spusťte.

Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/gg433132.aspx" target="_blank">Inicializace emulátoru úložiště pomocí nástroje příkazového řádku</a> na webu MSDN (můžete taky inicializace emulátoru úložiště ve Visual Studiu také vyžaduje oprávnění správce).

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Dochází k potíže s instalací Azure SDK pro .NET

Když se pokusíte nainstalovat sadu SDK, dojde k chybě pokusu o instalaci emulátoru úložiště na místním počítači. Protokol instalace obsahuje jednu z následujících zpráv:

- CAQuietExec: Chyb: Nelze získat přístup k instance serveru SQL
- CAQuietExec: Chyb: Nelze vytvořit databázi

Příčinou je problém s existující LocalDB instalace. Ve výchozím nastavení používá emulátoru úložiště LocalDB uchovávat data po napodobuje služby Azure úložiště. Obnovit LocalDB instance spuštěním následující příkazy v okně příkazového řádku před pokusu o instalaci SDK.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Příkaz **Odstranit** odebere všechny staré soubory databáze z předchozí instalace emulátoru úložiště.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Máte jiný problém se službou úložiště

Pokud předchozí Poradce při potížích oddíly neobsahují problém, který máte se službou úložiště, můžete přijmout následující postup diagnostiky a Poradce při potížích s problém.

- Zaškrtněte políčko metriky vidět – Pokud dojde ke změně z vaší očekávaná base řádku. Z metriky budete moci určit, jestli řešení tohoto problému je přechodná nebo trvalé nebo které skladování problém má vliv na.
- Metriky informace slouží k můžete hledat data protokolu serverovou podrobnější informace o chybách, ke kterým dochází. Tyto informace můžete odstraňování a řešení problému.
- Pokud informace v protokolech na straně serveru nestačí úspěšně řešení problému, můžete v protokolech na straně klienta úložiště klienta knihovny prošetřit chování klientské aplikace a nástrojů, jako jsou Fiddler Wireshark a Microsoft zprávy Analyzer prozkoumat sítě.

Další informace o používání Fiddler najdete v tématu "[Dodatek 1: použití Fiddler k zaznamenání přenosy protokolu HTTP a HTTPS]."

Další informace o používání Wireshark najdete v tématu "[Dodatek 2: použití Wireshark k zaznamenání v síti]."

Další informace o používání Microsoft Analyzer zprávy, najdete v článku "[Dodatek 3: použití Průvodce analýzou Microsoft Message k zaznamenání v síti]."

## <a name="appendices"></a>Dodatky

Dodatky popisují několik nástrojů, které můžete narazit na užitečné, když jste diagnostikování a řešení problémů s Azure úložiště (a další služby). Tyto nástroje nejsou součástí Azure úložiště a jiné produkty jiných výrobců. Jako takové nástroje popisované v těchto příloh se nevztahuje všechny podpory smlouvy, který může mít s Microsoft Azure nebo Azure úložiště a proto jako součást vašeho zkušebního měli prozkoumat licencování a podporu možností z zprostředkovatelé těchto nástrojů.

### <a name="appendix-1"></a>Dodatek 1: Použití Fiddler k zaznamenání přenosy protokolu HTTP a HTTPS

Fiddler je užitečným nástrojem pro analýzu HTTP a HTTPS provoz mezi vaší klientské aplikace a služby Azure úložiště, kterou používáte. Fiddler si můžete stáhnout z <a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>.

> [AZURE.NOTE] Fiddler můžete dekódovat HTTPS přenosy; Přečtěte si Fiddler si přečtěte následující dokumentaci pečlivě pochopit, jak se to dělá a abychom pochopili důsledky zabezpečení.

Tento dodatek obsahuje stručný návod pro nastavení Fiddler k zaznamenání komunikace mezi místním počítači nainstalovanou Fiddler a služby Azure úložiště.

Po spuštění Fiddler se spustí zachycení přenosy protokolu HTTP a HTTPS v místním počítači. Následující seznam uvádí některé užitečné příkazy pro řízení Fiddler:

- Zastavení a spuštění zachytávání přenosy. V hlavní nabídce přejděte na **soubor** a klepněte na tlačítko **Zachytit přenosy** přepnete na zachycení zapínat a vypínat.
- Uložení dat zachycený přenosy. V hlavní nabídce přejděte na **soubor**, klikněte na **Uložit**a potom klikněte na **Všechny relace**: umožňuje uložit přenos souboru archivu relace. Nové načtení relace archivu později pro analýzu nebo jeho odeslání v případě žádosti ze podpory společnosti Microsoft.

Pokud chcete omezit množství dat, který popisuje Fiddler, můžete použít filtry, které můžete konfigurovat na kartě **filtry** . Následující obrázek ukazuje filtru, který popisuje pouze přenosů na koncový bod úložiště **contosoemaildist.table.core.windows.net** :

![][5]

### <a name="appendix-2"></a>Dodatek 2: Použití Wireshark k zaznamenání v síti

Wireshark je analyzátoru protokolu sítě, který umožňuje zobrazit paketu podrobné informace o širokou škálu síťové protokoly. Wireshark si můžete stáhnout z <a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>.

Následující postup ukazuje, jak zachycení informací podrobné paketů pro přenos z místního počítače místo, kam jste si nainstalovali Wireshark ke službě tabulky ve vašem účtu Azure úložiště.

1.  Spuštění programu Wireshark na místním počítači.
2.  V části **začít** vyberte místní síti rozhraní rozhraní připojených k Internetu.
3.  Klikněte na tlačítko **zachytit možnosti**.
4.  Přidáte do textového pole **Zachytit filtr** filtr. **Host (hostitel) contosoemaildist.table.core.windows.net** například nastaví její konfiguraci a Wireshark k zaznamenání pouze pakety odeslané do nebo z účtu úložiště **contosoemaildist** koncový bod služby tabulky. Úplný seznam zachytit filtry najdete v článku <a href="http://wiki.wireshark.org/CaptureFilters" target="_blank">http://wiki.wireshark.org/CaptureFilters</a>.

    ![][6]

5.  Klikněte na tlačítko **Start**. Wireshark teď zachytit všechny odeslané pakety z koncový bod služby tabulky nebo při použití klientské aplikace na místním počítači.
6.  Po dokončení získáte v hlavní nabídce **zachycení** a pak **zastavení**.
7.  Uložte zachycený data v programu Wireshark zachytit souboru v hlavní nabídce klikněte na **soubor** a potom na **Uložit**.

WireShark zvýrazní všechny chyby, které jsou v okně **packetlist** . Můžete taky použít okno **Expert informace** (klikněte na **analyzovat**a pak **Expert informace**) Pokud chcete zobrazit souhrn chyb a upozornění.

![][7]

Můžete taky zvolíte zobrazení dat TCP vrstvě aplikací ho neviděl pravým tlačítkem myši na TCP data a výběrem **postupujte TCP proudu**. Toto je užitečné, pokud nezaznamenávají výpis bez filtru snímku. Najdete <a href="http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html" target="_blank">tady</a> Další informace.

![][8]

> [AZURE.NOTE] Další informace o používání Wireshark v <a href="http://www.wireshark.org/docs/wsug_html_chunked/" target="_blank">Programu Wireshark uživatelské příručky</a>.

### <a name="appendix-3"></a>Dodatek 3: Pomocí analyzátoru zprávy Microsoft k zaznamenání v síti

Zachycení přenosy protokolu HTTP a HTTPS podobným způsobem Fiddler pomocí analyzátoru zprávy Microsoft a zachytit v síti podobným způsobem Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurace webové trasování relace pomocí analyzátoru zprávy Microsoft

Abyste mohli nakonfigurovat webovou trasování relaci pro HTTP a HTTPS přenosu pomocí analyzátoru zprávy Microsoft, spuštění aplikace Microsoft zprávy Analyzer a v nabídce **soubor** klikněte na **Snímek a sledování**. V seznamu dostupné sledování scénáře vyberte **Web Proxy**. V panelu **Sledování scénář konfigurace** **HostnameFilter** do textového pole, přidejte názvy koncových bodů úložiště (můžete vyhledat tyto názvy klasické portálu Azure). Například **contosodata**při název účtu úložiště Azure bychom přidat následující do textového pole **HostnameFilter** :

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Znak mezery odděluje názvy hostitelů.

Až budete připraveni začít shromažďování dat trasování, klikněte na tlačítko **Start s** .

Další informace o sledování Microsoft zprávy Analyzer **Proxy serveru webové** naleznete na webu TechNet <a href="http://technet.microsoft.com/library/jj674814.aspx" target="_blank">PEF WebProxy poskytovatele</a> .

Integrované sledování **Proxy serveru webové** v Microsoft zprávy Analyzer je založená na Fiddler; ho můžete zaznamenávat přenosy HTTPS straně klienta a zobrazit nešifrovaném zprávy HTTPS. Sledování **Proxy serveru webové** funguje nakonfigurováním místní proxy pro všechny přenosy protokolu HTTP a HTTPS, které poskytuje přístup k nešifrovaném zprávy.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnostika pomocí analyzátoru zprávy Microsoft problémům se sítí

Kromě použití sledování Microsoft zprávy Analyzer **Proxy serveru webové** k zaznamenání podrobnosti o přenosy protokolu HTTP/HTTPs mezi klientské aplikace a služba úložiště, můžete také vestavěné sledování **Místní vrstvy odkaz** k zaznamenání údajů o paketu síti. To umožňuje k zaznamenání dat, které můžou shromažďovat s Wireshark a Diagnostika podobný síti problémy jako vyřazené pakety.

Následující obrázek ukazuje příklad sledování **Místní vrstva propojení** s některé **informační** zprávy ve sloupci **DiagnosisTypes** . Po kliknutí na ikonu ve sloupci **DiagnosisTypes** zobrazuje podrobnosti o zprávu. V tomto příkladu serveru opakovaně odeslané zprávy #305 protože ji neobdržel odpověď na odeslanou zprávu z klienta:

![][9]

Při vytváření relace trasování v Microsoft zprávy Analyzer můžete určit filtry ke snížení množství hluku trasování. Na stránce **zachytit / trasování** kde definovat sledování klikněte na odkaz **Konfigurovat** vedle **Microsoft Windows NDIS PacketCapture**. Následující obrázek ukazuje konfigurace, který filtruje TCP návštěvníci IP adresy tři úložiště služby:

![][10]

Další informace o sledování Microsoft zprávy Analyzer místní odkaz vrstvy najdete v článku <a href="http://technet.microsoft.com/library/jj659264.aspx" target="_blank">PEF NDIS PacketCapture poskytovatele</a> na TechNetu.

### <a name="appendix-4"></a>Dodatek 4: Pomocí aplikace Excel a zobrazovat metriky protokolování údajů

Celou řadu nástrojů umožňují stažení metriky úložišť dat z úložiště tabulek Azure ve formátu s oddělovači, který usnadňuje data načítáte do Excelu pro prohlížení a analýza. Úložiště protokolování data z úložiště objektů blob Azure již ve formátu s oddělovači mohou načítáte do Excelu. Však bude potřebujete přidat nadpisy odpovídající sloupec na základě informací na <a href="http://msdn.microsoft.com/library/azure/hh343259.aspx" target="_blank">Formát protokolu úložiště technologie pro analýzu</a> a <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Schématu tabulky metriky analýzy úložiště</a>.

Protokolování úložiště dat do Excelu mohli importovat po stažení z úložiště objektů blob:

- V nabídce **Data** klikněte na **Z textu**.
- Přejděte k souboru protokolu, který chcete zobrazit a klikněte na **importovat**.
- V kroku 1 **Průvodce importem textu**vyberte **Oddělovač**.

V kroku 1 **Průvodce importem textu**, vyberte **středník** jako pouze oddělovač a zvolte dvojité nabídky jako **textový kvalifikátor**. Klikněte na tlačítko **Dokončit** a zvolte, kam chcete umístit data v sešitu.

### <a name="appendix-5"></a>Dodatek 5: Sledování pomocí aplikace přehledy pro týmovou Visual Studio

Můžete taky použít funkci přehledy aplikace pro týmovou Visual Studio jako součást výkon a sledování dostupnost. Tento nástroj tyto možnosti:

- Zkontrolujte, že webová služba je k dispozici a citlivé. Jestli je aplikace na web nebo zařízení aplikaci používající webové služby, ji otestujte svoji adresu URL každých několik minut z umístění světě a kancelářskou sponkou, pokud došlo k potížím.
- Rychle diagnostikovat potíže s výkonem nebo výjimky ve webové služby. Zjistěte, roztažení procesoru nebo jiných zdrojů je právě, optimálnímu zásobníku trasování výjimky a snadno prohledejte prostřednictvím protokolu trasování. Pokud v aplikaci výkon vynechává pod přijatelné omezení, doporučujeme vám mohou posílat e-mailu. Můžete sledovat .NET a Java webové služby.

AT je čas psaní přehledy aplikace ve verzi preview. Další informace najdete v <a href="http://msdn.microsoft.com/library/azure/dn481095.aspx" target="_blank">Aplikaci přehledy služby Visual Studio týmu na webu MSDN</a>.


<!--Anchors-->
[Úvod]: #introduction
[Uspořádání této příručce]: #how-this-guide-is-organized

[Sledování úložiště služby]: #monitoring-your-storage-service
[Sledování stavu služeb]: #monitoring-service-health
[Sledování kapacity]: #monitoring-capacity
[Sledování dostupnosti]: #monitoring-availability
[Sledování výkonu]: #monitoring-performance

[Diagnostika problémů s úložiště]: #diagnosing-storage-issues
[Service zdravotní problémy]: #service-health-issues
[Problémy s výkonem]: #performance-issues
[Diagnostikování chyby]: #diagnosing-errors
[Problémy emulátoru úložiště]: #storage-emulator-issues
[Nástroje protokolování úložiště]: #storage-logging-tools
[Pomocí nástroje protokolování sítě]: #using-network-logging-tools

[Trasování začátku do konce]: #end-to-end-tracing
[Srovnávací dat protokolu]: #correlating-log-data
[Žádost o ID klienta]: #client-request-id
[ID požadavku serveru]: #server-request-id
[Časová razítka]: #timestamps

[Pokyny pro řešení potíží]: #troubleshooting-guidance
[Metriky jsou zobrazeny vysoké AverageE2ELatency zhoršeným AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metriky zobrazit zhoršeným AverageE2ELatency a nízké AverageServerLatency ale klienta dochází vysokou latencí]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metriky zobrazit vysoké AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Jste zaznamenali neočekávané zpožděním při doručení zprávy ve frontě]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Metriky zobrazit zvýšení v PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Přechodné zvýšení PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Trvalé zvýšení PercentThrottlingError chyby]: #permanent-increase-in-PercentThrottlingError
[Metriky zobrazit zvýšení v PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metriky zobrazit zvýšení v PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Klient získává 403 HTTP (zakázáno) zprávy]: #the-client-is-receiving-403-messages
[Klient získává HTTP 404 (nebyl nalezen) zprávy]: #the-client-is-receiving-404-messages
[Klient nebo jiným procesem odstraněnou objektu]: #client-previously-deleted-the-object
[Problém se tak mohli ověřovat s sdílených přidružení (zabezpečení aplikace Access podpis)]: #SAS-authorization-issue
[Kód v JavaScriptu klientských nemá oprávnění pro přístup k objektu]: #JavaScript-code-does-not-have-permission
[Selhání sítě]: #network-failure
[Klient získává 409 HTTP (konflikt) zprávy]: #the-client-is-receiving-409-messages

[Metriky zobrazení zhoršeným PercentSuccess nebo analýzy protokolu položky mají operace se stavem ClientOtherErrors]: #metrics-show-low-percent-success
[Kapacita metriky zobrazit neočekávané zvýšení využití kapacitu úložiště]: #capacity-metrics-show-an-unexpected-increase
[Jste zaznamenali neočekávané restartování virtuálních počítačích, které mají velkého počtu připojených VHD]: #you-are-experiencing-unexpected-reboots
[Problém nastane pomocí emulátoru úložiště pro vývoj nebo test]: #your-issue-arises-from-using-the-storage-emulator
[Funkce "X" nefunguje v emulátoru úložiště]: #feature-X-is-not-working
[Chyba "hodnotu pro jedno ze záhlaví HTTP není ve správném formátu" při použití emulátoru úložiště]: #error-HTTP-header-not-correct-format
[Spuštění emulátoru úložiště vyžaduje oprávnění správce]: #storage-emulator-requires-administrative-privileges
[Dochází k potíže s instalací Azure SDK pro .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Máte jiný problém se službou úložiště]: #you-have-a-different-issue-with-a-storage-service

[Dodatky]: #appendices
[Dodatek 1: Použití Fiddler k zaznamenání přenosy protokolu HTTP a HTTPS]: #appendix-1
[Dodatek 2: Použití Wireshark k zaznamenání v síti]: #appendix-2
[Dodatek 3: Pomocí analyzátoru zprávy Microsoft k zaznamenání v síti]: #appendix-3
[Dodatek 4: Pomocí aplikace Excel a zobrazovat metriky protokolování údajů]: #appendix-4
[Dodatek 5: Sledování pomocí aplikace přehledy pro týmovou Visual Studio]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/overview.png
[2]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/portal-screenshot.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-2.png
