<properties
    pageTitle="Analýza výkonu okraje v Azure CDN | Microsoft Azure"
    description="Analýza výkonu uzel okraje v Microsoft Azure CDN. Analýza výkonu okraj obsahuje podrobné informace o použití přenosy a šířky pásma pro zprávy CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analýza výkonu uzel okraje v Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Základní informace
Analýza výkonu okraj obsahuje podrobné informace o použití přenosy a šířky pásma pro zprávy CDN. Tyto informace potom lze vytvořit trendu statistiku, které umožní získat informace o jak svém majetku probíhá v mezipaměti a doručila ji do klienty. Zároveň umožňuje provádět formulář strategii o tom, jak optimalizovat doručování obsahu a určit, co by měl být problémy řešit lepší účinek CDN. Nejen jako výsledek, který budou moct zvýšit výkon doručení datové, ale taky moct snížit náklady CDN.

> [AZURE.NOTE] Všechny sestavy pomocí zápisu UTC/GMT při určování typu datum a čas.

## <a name="reports-and-log-collection"></a>Sestavy a kolekce protokolů

CDN aktivity musí být údaje modulem analýzy výkonu okraje před mohou generovat sestavy na něm. Tento proces kolekce nastane jednou za den a zahrnuje aktivity, ke kterým došlo během na předchozí den. To znamená představující sestavy statistiky výběru s jednou variantou na den v době jeho zpracování a ne nutně obsahovat úplnou sadu dat pro aktuální den. Primární funkce tyto sestavy je vyhodnotit výkon. Není vhodné používat pro účely účtování nebo přesné číselné statistiky.

> [AZURE.NOTE] Výchozí informace shromážděné ze které strany výkonu analytické sestavy jsou neexistuje aspoň 90 dní.

## <a name="dashboard"></a>Řídicí panel

Řídicí panel okraj výkonu analýzy sleduje aktuální a historických CDN přenosů na graf a statistiky. Pomocí tohoto řídicího panelu lze zjistit poslední a dlouhodobé trendy na výkon přenosy CDN pro váš účet.

Tento řídicích panelů se skládá z:

* Interaktivní graf, který umožňuje vizualizace klíčové metriky a trendy.
* Časová osa představu o dlouhodobou vzorků pro klíčové metriky a trendů
* Klíčové metriky a statistické informace o tom, naší sítě CDN zlepšuje webu přenosy měřeno pomocí celkový výkon, použití a efektivity.

### <a name="accessing-the-edge-performance-dashboard"></a>Přístup k řídicím panelu okraj výkonu

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-edge-performance/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **Okraj prováděcí analýzy** .  Klikněte na **řídicí panel**.

    Zobrazí se na řídicím panelu okraj uzel analýzy.

### <a name="chart"></a>Graf

Řídicí panel obsahuje graf, který sleduje metriky časové období vybrané v časové osy, která se objeví přímo pod ním.  Časové osy grafy nahoru na poslední dva roky CDN činnosti se zobrazí přímo pod grafu.

#### <a name="using-the-chart"></a>Pomocí grafu

* Ve výchozím nastavení bude zobrazena míra účinnosti mezipaměti pro posledních 30 dní.
* Vygeneruje se tento graf z dat kompletovaně denně.
* Přejdete myší na den v spojnicový graf se označuje datum a hodnotu metriky na toto datum.
* Klikněte na zvýraznit víkendy Přepnout překrytí světle šedé svislé čáry, které představují víkendy do grafu. Tento typ překryvném je užitečný pro identifikaci vzorů přenosy přes víkendy.
* Klikněte na zobrazení jeden rok před Přepnout překrytí předchozí rok aktivity stejný časové období do grafu. Tento typ porovnání obsahuje přehled o dlouhodobé CDN využití vyhledávání. V pravém horním rohu grafu obsahuje legendu označující kód barvy pro každý spojnicový graf.

#### <a name="updating-the-chart"></a>Aktualizace grafu

* Časový rozsah: Proveďte jednu z následujících akcí:
    * Vyberte požadovanou oblast na časové ose. Graf se aktualizuje s daty, která odpovídá vybrané časové období.
    * Poklikejte na požadovaný graf zobrazení všech dostupných historických dat maximální hodnota dva roky.
* Metriky: Klikněte na ikonu grafu, která se zobrazí vedle požadovaného metriky. V grafu a na časovou osu aktualizujete s daty odpovídající metriky.


### <a name="key-metrics-and-statistics"></a>Klíčové metriky a statistiky

#### <a name="efficiency-metrics"></a>Metriky efektivity

Má tyto metriky účel zobrazíte, zda lze zvýšit efektivitu mezipaměti. Hlavní výhody odvozeno z mezipaměti efektivity jsou:

* Snížení zatížení zdrojový server, což může vést k:
    * Lepší výkon webového serveru.
    * Snížení provozní nákladů.
* Vylepšené data doručení akcelerace od více požadavků bude podávané přímo ze zprávy CDN.

Pole | Popis
------|------------
Efektivity mezipaměti | Určuje procento přenesených dat, který byl doručen z mezipaměti. Tento metrických opatření při režim cached verzi požadovaný obsah byl doručen přímo z CDN (servery edge) pro žadatele (například webový prohlížeč)
Úspěšnost | Určuje procento požadavky, které byly podávané množství z mezipaměti. Tento metrických opatření při režim cached verzi požadovaný obsah byl doručen přímo z CDN (servery edge) pro žadatele (například webový prohlížeč).
% Vzdálené bajtů – bez konfigurace mezipaměti | Určuje procento přenos, který byl doručen z origin servery k CDN (servery edge), nebude cached důsledku funkce přemostění mezipaměti (stroj pravidel HTTP).
% Vzdálené bajtů – vypršela platnost mezipaměti | Určuje procento přenos, který byl doručen z origin servery k CDN (servery edge) důsledku zastaralé obsahu Revalidace.

#### <a name="usage-metrics"></a>Použití metriky

Má tyto metriky účel poskytnout přehled o následující opatření vyjmutí nákladů:

* Minimalizace provozní náklady prostřednictvím CDN.
* Snížení CDN výdaje prostřednictvím efektivity mezipaměti a komprese.

> [AZURE.NOTE] Přenosy hlasitost čísla představují přenos, který byl použit výpočtů poměrů a procent a může zobrazit pouze část celkové přenosy pro zákazníky vysoká hlasitost.

Pole | Popis
------|------------
Uložit bajty | Označuje průměrný počet bajtů přenesených pro každý požadavek zobrazovaná CDN (servery edge) žadateli (například webový prohlížeč).
Žádné rychlost Config bajtu mezipaměti | Určuje procento přenosy podávané množství od CDN (servery edge) žadateli (například webový prohlížeč), který nebude do mezipaměti kvůli funkce přemostění mezipaměti.
Komprimovaná bajtu sazba | Určuje procento přenosů z CDN (servery edge) žadatele (například webový prohlížeč) ve formátu mu tuhle zkomprimovanou.
Bajty | Označuje množství dat, v bajtech dodaných z CDN (servery edge) žadateli (například webový prohlížeč).  
Bajtů | Označuje, množství dat, v bajtech z žadatele (například webový prohlížeč) k CDN (okraj servery).
Vzdálené bajtů | Označuje množství dat, v bajtech odeslal CDN a zákazník servery origin CDN (okraj servery).

#### <a name="performance-metrics"></a>Měřítka

Má tyto metriky účel ke sledování celkový výkon CDN pro vaše data.

Pole | Popis
------|------------
Přenos sazba | Označuje průměrná rychlost obsah byl přenosu ze zprávy CDN žadateli.
Doba trvání | Udává průměrnou čas v milisekundách trvala předvádění aktivum žadateli (například webový prohlížeč).
Komprimovaná žádost sazba | Ve formátu mu tuhle zkomprimovanou označuje Procento přístupů dodaných z CDN (servery edge) žadateli (například webový prohlížeč).
Míra 4xx chyb | Určuje procento přístupů generovaných 4xx stavový kód.
Míra 5xx chyb | Určuje procento přístupů generovaných 5xx stavový kód.
Přístupy | Označuje počet požadavky na CDN obsah.

#### <a name="secure-traffic-metrics"></a>Přenosy zabezpečené metriky

Má tyto metriky účel ke sledování výkonu CDN pro přenos HTTPS.

Pole | Popis
------|------------
Zajistit mezipaměť efektivity | Určuje procento přenesených pro HTTPS požadavky, které byly podávané množství z mezipaměti dat. Tento metriky opatření při režim cached verzi požadovaný obsah byl doručen přímo z CDN (servery edge) žadatele (například webový prohlížeč) přes protokol HTTPS.
Bezpečný přenos sazba | Označuje průměrná rychlost obsah byl přenosu z CDN (servery edge) pro žadatele (například webových serverů) přes protokol HTTPS.
Průměrná zabezpečené doba trvání | Udává průměrnou čas v milisekundách trvala poskytování aktivum žadateli (například webový prohlížeč) přes HTTPS.
Zabezpečené přístupů | Označuje počet HTTPS požadavky na CDN obsah.
Zabezpečené bajty | Označuje množství přenosů HTTPS, v bajtech dodaných z CDN (servery edge) žadateli (například webový prohlížeč).

## <a name="reports"></a>Sestavy

Každá zpráva v tomto modulu obsahuje graf a statistických údajů o využití šířky pásma a přenosy pro různé typy metriky (například HTTP stavů, mezipaměti stavů, požádat o adresy URL, atd.). Tyto informace lze delve hlouběji do jak obsah je právě podávané množství pro klienty a optimalizovat chování CDN pro zvýšení výkonu data doručení.

### <a name="accessing-the-edge-performance-reports"></a>Přístup k sestav výkonu okraje

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-edge-performance/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **Okraj prováděcí analýzy** .  Klepněte na **HTTP velkého objektu**.

    Zobrazí se obrazovka okraj uzel analýzy sestavy.

Sestava | Popis
-------|------------
Denní souhrn | Umožňuje zobrazit denní trendů přenosy zadaný časové období. Každý pruh na tento graf znázorňuje určitého data. Velikost panelu označuje celkový počet přístupů, ke kterým došlo k tomuto dni.
Každou hodinu souhrn | Umožňuje zobrazit každou hodinu přenosy trendy v určeném časovém období. Každý pruh na tento graf znázorňuje jednu hodinu k určitému datu. Velikost panelu označuje celkový počet přístupů, ke kterým došlo během této hodinu.
Protokoly | Zobrazí rozdělení komunikaci mezi protokoly HTTP a HTTPS. Prstencový graf označuje Procento přístupů, ke kterým došlo u jednotlivých typů protokolu.
Nastavit informace HTTP metody | Vám umožní získat rychle přehled které HTTP metody použily požádat o vaše data. Obvykle jsou nejčastější metody žádost HTTP GET, vedoucí a příspěvek. Prstencový graf označuje Procento přístupů, ke kterým došlo u jednotlivých typů metoda žádost HTTP.
Adresy URL | Obsahuje graf, který se zobrazuje horní 10 požadované adresy URL. Na panelu se zobrazí pro každou adresu URL. Výška řádku označuje, kolik přístupů generovaných konkrétní adresy URL přes časového rozsahu vztahuje konsolidovaná sestavy. Statistiky 100 nejvyšších požadovaného že adresy URL zobrazují přímo pod tento graf.
Mají záznamy CNAME | Obsahuje graf, který se zobrazuje nahoře 10 mají záznamy CNAME používá požádat o prostředky čase rozsahu sestavy. Statistiky 100 nejvyšších požadovaného že mají záznamy CNAME zobrazují přímo pod tento graf.
Typů | Obsahuje graf, který se zobrazuje horní 10 CDN nebo zákazníka origin servery ze kterých bylo požadováno prostředky přes zadané období. Statistiky 100 nejvyšších požadovaného serverech origin CDN nebo zákazníka jsou zobrazeny přímo pod tento graf. Zákazník origin servery jsou označeny název definovaný v poli Název adresáře.
GEO POP | Zobrazuje, kolik přenosy pro vaši směrovány přes konkrétní bodu z přítomnost (POP). Římskou číslici představuje POP v naší sítě CDN.
Klienti | Obsahuje graf, který se zobrazuje začátek 10 klientech požadované prostředky přes zadané období. Pro účely tato zpráva se všechny požadavky, které pocházejí z jedné adresy IP jsou považovány za ze stejného klienta. Statistiky horních 100 klientů zobrazují přímo pod tento graf. Tuhle sestavu slouží k určení vzorků aktivity ke stažení pro začátek klienty.
Zobrazit stavy mezipaměti | Zobrazí podrobný rozpis chování mezipaměti, které může odhalit postupů pro vylepšení zkušenosti s koncovým uživatelem. Od nejrychlejší výkon pochází z přístupy do mezipaměti, abyste optimalizovali rychlosti doručení dat minimalizace Neúspěšné přístupy do mezipaměti a vypršela platnost mezipaměti přístupů.
ŽÁDNÁ podrobnosti | Obsahuje graf, který se zobrazuje horní 10 adres URL pro prostředky pro kterou nebyla aktuálnost obsahu mezipaměti zaškrtnuté přes zadané období. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o CONFIG_NOCACHE | Obsahuje graf, který se zobrazuje prvních 10 adres URL pro materiálů, které nebyly cached kvůli konfigurace CDN zákazníka. Tyto typy prostředky byly podávané množství přímo ze zdrojového serveru. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
UNCACHEABLE podrobnosti | Obsahuje graf, který se zobrazuje horní 10 adres URL pro materiálů, které nelze cached kvůli data záhlaví žádosti o. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o TCP_HIT | Obsahuje graf, který se zobrazuje horní 10 adres URL pro materiálů, které jsou doručeny okamžitě z mezipaměti. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o TCP_MISS | Obsahuje graf, který se zobrazuje horní 10 adres URL pro materiálů, které mají stav mezipaměti TCP_MISS. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o TCP_EXPIRED_HIT | Obsahuje graf, který se zobrazuje horní 10 adres URL pro zastaralé prostředky, které byly podávané množství přímo z POP. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o TCP_EXPIRED_MISS | Obsahuje graf, který se zobrazuje horní 10 adres URL pro zastaralé prostředky pro které musel být načtená z kontingenčního seznamu zdrojový server novou verzi. Statistiky horních 100 adresy URL pro tyto typy prostředky zobrazují přímo pod tento graf.
Podrobnosti o TCP_CLIENT_REFRESH_MISS | Obsahuje pruhového grafu zobrazující prvních 10 adresy URL pro prostředky byly načtená z kontingenčního seznamu zdrojovému serveru kvůli žádost bez mezipaměti z klienta. Statistiky horních 100 adresy URL pro tyto typy požadavky se zobrazují přímo pod tento graf.
Žádost o typy klientů | Označuje typ požadavky, které byly klienty HTTP (například prohlížeče). Tato sestava obsahuje prstencový graf, který poskytuje smysl tak, aby se zpracování žádostí o. Šířka pásma a provoz informace u jednotlivých typů žádost se zobrazují pod grafem.
Uživatelského agenta | Obsahuje pruhového grafu zobrazující horní 10 uživatelských agentů požádat o obsahu pomocí našich CDN. Uživatelského agenta bývá webového prohlížeče, media Playeru nebo mobilní telefon prohlížeče. Statistiky horních 100 uživatelských agentů se zobrazují přímo pod tento graf.
Doporučující osoby | Obsahuje pruhového grafu zobrazující Hlavní doporučující osoby 10 obsahu prostřednictvím naše CDN. Referrer bývá adresu URL webové stránky nebo zdroje, který bude odkazovat na obsah. Podrobné informace zajišťuje pod grafu 100 nejvyšších doporučující osoby.
Typy komprese | Obsahuje prstencový graf, který rozdělí požadované prostředky o tom, jestli byly komprimovány tak, že naše servery okraje. Procento mu tuhle zkomprimovanou prostředky je prezentovaná podle typu použité komprese. Podrobné informace jsou uvedeny pod grafem pro každý typ komprese a stav.
Typy souborů | Obsahuje pruhového grafu zobrazující horní typy 10 souborů, které byla požadována prostřednictvím naše CDN pro váš účet. Pro účely Tato sestava typ souboru je definován majetku přípona názvu souboru a zadejte internetových médií (například .html \[textu nebo html\], .htm \[textu nebo html\], aspx \[textu nebo html\]atd.). Podrobné informace jsou uvedeny pod grafem pro typy horních 100 souborů.
Jedinečný souborů | Obsahuje graf, který zobrazuje celkový počet jedinečných materiálů, které byly požadavku na určitý den přes zadané období.
Tokenu Auth souhrn | Obsahuje výsečový graf, který obsahuje rychlý přehled o tom, jestli byly požadované prostředky chráněn ověřování na základě Token. Chráněný prostředky se zobrazují v grafu podle výsledků pokus o ověření.
Tokenu Auth odepřít podrobnosti | Obsahuje sloupcový graf, který umožňuje zobrazit nejvyšší 10 požadavky, které byly odepřen kvůli ověřování na základě Token.
Kódy odpověď HTTP | Poskytuje přehled stavů HTTP (například 200 OK, 403 Zakázáno, 404 Nenalezeno, apod), které byly doručené klientům HTTP naše servery okraje. Výsečový graf můžete rychle zjistit, jak byly svém majetku podávané množství. Pro každý kód odpovědí pod grafem není uvedený podrobné statistických údajů.
Chyby 404 | Obsahuje sloupcový graf, který umožňuje zobrazit nejvyšší 10 požadavky, které vedly 404 Nenalezeno kódem odezvy.
403 chyby | Obsahuje sloupcový graf, který umožňuje zobrazit nejčastější 10 požadavky, které vedly 403 kód odpovědi zakázáno. 403 kód odpovědi zakázáno bude proveden poté žádost o byl odepřen tak, že zdrojový server zákazníků nebo edge serveru na naše POP.
4xx chyby | Obsahuje sloupcový graf, který umožňuje zobrazit nejvyšší 10 požadavky, jejichž výsledkem kódu odpovědí v oblasti 400. Tato zpráva se 403 404 kódy odpověď zakázáno a nebyl nalezen. Obvykle kód odpovědi 4xx dochází, pokud je žádost o odepřen důsledku chyba klienta.
504 chyby | Obsahuje sloupcový graf, který umožňuje zobrazit nejvyšší 10 požadavky, které vedly 504 kód odpovědi vypršel časový limit brány. 504 kód odpovědi vypršel časový limit brány bude proveden poté, časový limit nastavit informace HTTP proxy při komunikaci s jiným serverem. V případě náš CDN 504 kód odpovědi vypršel časový limit brány obvykle bude proveden poté edge serveru nelze vytvořit komunikaci se serverem origin zákazníka.
Chyby 502 | Obsahuje sloupcový graf, který umožňuje zobrazit nejčastější 10 požadavky, které vedly kódem odezvy 502 Chybná brána. Kódem odezvy 502 Chybná brána dochází, když dojde k chybě protokolu HTTP mezi serverem a nastavit informace HTTP proxy. V případě naše CDN kódem odezvy 502 Chybná brána obvykle bude proveden poté, zdrojový server zákazníka vrátí neplatnou odpověď na edge serveru. Odpověď je neplatný, pokud nelze analyzovat nebo je neúplné.
5xx chyby | Obsahuje sloupcový graf, který umožňuje zobrazit nejčastější 10 požadavky, jejichž výsledkem kódu odpovědí v oblasti 500.  Tato zpráva se 502 Chybná brána a 504 kódů odpovědí vypršel časový limit brány.

## <a name="see-also"></a>Viz taky
* [Přehled Azure CDN](cdn-overview.md)
* [Data v reálném stat v Microsoft Azure CDN](cdn-real-time-stats.md)
* [Přepíše výchozí nastavení HTTP pomocí modul pravidel](cdn-rules-engine.md)
* [Rozšířené HTTP sestavy](cdn-advanced-http-reports.md)
