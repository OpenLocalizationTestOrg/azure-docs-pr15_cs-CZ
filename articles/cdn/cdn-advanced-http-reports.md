<properties
    pageTitle="Azure CDN Upřesnit HTTP sestavy | Microsoft Azure"
    description="Rozšířené HTTP sestavy v aplikaci Microsoft Azure CDN. Tyto sestavy obsahují podrobné informace o aktivitě CDN."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Rozšířené HTTP sestavy v aplikaci Microsoft Azure CDN

## <a name="overview"></a>Základní informace

Tento dokument vysvětluje Upřesnit HTTP vykazování v Microsoft Azure CDN. Tyto sestavy poskytují podrobné informace o CDN aktivity.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Přístup k rozšířené HTTP sestavy

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **Upřesnit sestavy protokolu HTTP** .  Klikněte na **velké platformy HTTP**.

    ![Portálu Správa CDN – nabídka Upřesnit sestavy](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Zobrazí se možnosti sestavy.

## <a name="geography-reports-map-based"></a>Zeměpisná oblast sestavy (založených na mapě)

Existuje pět sestavy, které využít mapy k označení oblasti, ze kterých jsou požadovány obsahu. Tyto sestavy jsou mapy světa, Spojené státy mapu, Kanada mapy, mapa Evropy a Asii tichomořská mapy.

Každá zpráva založených na mapě řadí zeměpisné entity (tedy země USA a provincie) podle Procento přístupů, které pochází z této oblasti. Kromě toho mapy umožňují vizualizaci do umístění, ze kterých jsou požadovány obsahu. Je možné k tomu nevyzve barevné označení každou oblast podle počtu služba zkušených v dané oblasti. Zapalovače oblastí označuje nižší služba pro obsah, zatímco tmavší oblasti označují vyšší úrovně služba pro obsah.

Podrobné informace o přenosy a šířky pásma pro každou oblast není uvedený přímo pod mapy. Díky tomu budete moct zobrazit celkový počet přístupů, Procento přístupů, celkové množství dat převeden (GB) a procento dat převedou pro každou oblast. Zobrazení popisu pro každou z těchto metriky. Nakonec Když přejedete myší přes oblast (tedy země, Kraj nebo Province (Kraj)), název a Procento přístupů, ke kterým došlo v oblasti se zobrazí jako popisů ovládacích prvků.

Stručný popis je uveden níže u jednotlivých typů založených na mapě geography sestavy.

Název sestavy | Popis
------------|------------
Světová mapa | Tuhle sestavu můžete zobrazit Celosvětová dostupnost služba pro CDN obsah. Pro jednotlivé země je barevně označené na mapě světa vyznačení Procento přístupů, které pochází z této oblasti.
Spojené státy mapy | Tuhle sestavu můžete zobrazit služba pro obsah CDN ve Spojených státech. Tyto stavy je barevně označené na mapa označíte Procento přístupů, které pochází z této oblasti.
Kanada mapy | Tuhle sestavu můžete zobrazit služba pro obsah CDN v Kanadě. Každý Province (Kraj) je barevně označené na mapa označíte Procento přístupů, které pochází z této oblasti.
Mapa Evropy | Tuhle sestavu můžete zobrazit služba pro obsah CDN v Evropě. Pro jednotlivé země je barevně označené na mapa označíte Procento přístupů, které pochází z této oblasti.
Země Asie pacifického mapy | Tuhle sestavu můžete zobrazit služba pro obsah CDN v Asii. Pro jednotlivé země je barevně označené na mapa označíte Procento přístupů, které pochází z této oblasti.

## <a name="geography-reports-bar-charts"></a>Zeměpisná oblast sestavy (pruhových grafů)

Existují dva další sestavy, které poskytují statistické informace podle předpisů rozhraní zeměpisná oblast horní měst a od horní země. Tyto sestavy pořadí měst a země, v podle požadovanou velikost nalezených záznamů, které pochází z těchto oblastí. Při generování tento typ sestavy, bude označovat pruhového grafu horní 10 měst nebo země, které požadované obsah přes konkrétní platformu. Pruhový graf můžete rychle zjistit oblasti, které mohou generovat nejvyšší počet požadavků pro obsah.

Na levé straně grafu (y) označuje, kolik přístupů došlo v zadané oblasti. Přímo pod grafem osy (x) zjistíte popisek pro každou z oblasti horní 10.

### <a name="using-the-bar-charts"></a>Použití pruhových grafů

* Když najedete myší na pruh, název a celkový počet přístupů, ke kterým došlo v oblasti se zobrazí jako popisů ovládacích prvků.
* Popis tlačítka pro sestavu horní měst identifikuje města jeho jméno, státu/provincie a zkratka země.
* Pokud města nebo oblasti (tedy státu/provincie), z něhož pochází žádost o nelze určit, potom ho výskyt znamená, že pocházejí neznámý. Pokud je země neznámý a potom otazník dva (tedy??), se zobrazí.
* Sestavy můžou obsahovat metriky "Europe" nebo "Asijsko-tichomořské oblasti." Tyto položky nejsou určeny o poskytnutí statistických údajů na všechny adresy IP v těchto oblastech. Místo toho se použijí jenom u požadavky, které pocházejí z IP adresy, které jsou rozložené v Evropě nebo země Asie a Tichomoří místo na konkrétní město nebo země.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Zde najdete celkový počet přístupů, Procento přístupů, množství dat převeden (GB) a procento dat převedou oblastí horní 250. Zobrazení popisu pro každou z těchto metriky.

Pro oba typy sestav níže není uvedený stručný popis.

Název sestavy | Popis
------------|------------
Horní města | Tuhle sestavu řadí města podle požadovanou velikost nalezených záznamů, které pochází z této oblasti.
Začátek země | Tuhle sestavu řadí zemí podle požadovanou velikost nalezených záznamů, které pochází z této oblasti.

## <a name="daily-summary"></a>Denní souhrn

Sestava denní souhrn umožňuje zobrazit celkový počet přístupů a data přenášena konkrétní platformu denně. Tyto informace lze rychle ověřit pravost CDN aktivitu vzorce. Například v této sestavě vám můžou pomoct zjistit, které dny vyšší zkušených nebo nižší než očekávané přenos.

Při generování tento typ sestavy, poskytne pruhového grafu vizuální znázornění částka specifické pro platformu služba zkušených denně časové období vztahuje konsolidovaná sestavy. Udělá tak zobrazením pruh pro každý den v sestavě. Výběr časové období například s názvem "Minulý týden" vygeneruje pruhový graf s sedm pruhy. Každý pruh výskyt znamená celkový počet přístupů zkušených v daný den.

Na levé straně grafu (y) označuje, kolik přístupů došlo k na zadané datum. Přímo pod grafem osy (x), bude hledat popisek, který označuje datum (formát: YYYY-MM-DD) pro každý den zahrnout do sestavy.

> [AZURE.TIP] Když najedete myší na pruh, zobrazí se celkový počet přístupů, ke kterým došlo k tomuto dni jako popisů ovládacích prvků.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Pro každý den vztahuje konsolidovaná sestavu tam bude hledat celkový počet přístupů a množství dat převeden (GB).

## <a name="by-hour"></a>O hodinu

Sestava tak, že hodinu umožňuje zobrazit celkový počet přístupů a data přenášena konkrétní platformu na hodinu. Tyto informace lze rychle ověřit pravost CDN aktivitu vzorce. Například v této sestavě vám pomůže zjistit časová období během dne, která zaznamenat vyšší nižší než očekávané přenosy.

Při generování tento typ sestavy, poskytne pruhovém grafu vizuální znázornění částka specifické pro platformu služba praxí hodinu časové období vztahuje konsolidovaná sestavy. Udělá tak zobrazením pruh pro každou hodinu vztahuje konsolidovaná sestavy. Například výběr 24 hodin časové období vygeneruje pruhový graf s pruhy dvacet čtyři. Každý pruh výskyt znamená celkový počet přístupů došlo během této hodinu.

Na levé straně grafu (y) označuje, kolik přístupů došlo k zadané hodiny. Přímo pod grafem osy (x), bude hledat popisek, který označuje datum a čas (formát: YYYY-MM-DD hh: mm) pro každou hodinu zahrnout do sestavy. Čas hlášení 24hodinovém formátu a není zadán podle časového pásma UTC/GMT.

> [AZURE.TIP] Když najedete myší na pruh, zobrazí se celkový počet přístupů, ke kterým došlo během této hodinu jako popisů ovládacích prvků.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Tam najdete celkový počet přístupů a množství dat převeden (GB) pro každou hodinu vztahuje konsolidovaná sestavy.

## <a name="by-file"></a>U souboru

Sestava po souboru umožňuje můžete zobrazit služba a provozu vzniklé nad určitou platformu pro často požadovaných prostředky. Při generování tento typ sestavy, pruhovém grafu vygeneruje na začátek 10 často požadovaných prostředky zadaný časové období.

> [AZURE.NOTE] Pro účely Tato sestava okraj CNAME URL se převedou na adresy URL jejich odpovídající CDN. Díky přesné ověřovací pro celkový počet přístupů přidružené aktivum bez ohledu na CDN nebo okraj CNAME URL použité o něj požádat.

Na levé straně grafu (y) označuje počet požadavků pro každý majetek zadaný časové období. Přímo pod grafem osy (x), zjistíte popisek, který označuje název souboru pro každý horní 10 požadované majetek.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Zde najdete tyto informace pro jednotlivá pole začátek 250 požadované majetku: relativní cestu, celkový počet přístupů, Procento přístupů, množství dat převeden (GB) a převedou procentuální hodnotu data.

## <a name="by-file-detail"></a>Tak, že soubor podrobností

Sestava podrobností tak, že soubor umožňuje můžete zobrazit služba a provozu vzniklé nad určitou platformu pro konkrétní materiálů. Na začátek této zprávy je možnost soubor podrobnosti pro. Tuto možnost obsahuje seznam často požadovaných prostředky platformu vybrané. K vygenerovat sestavu tak, že soubor podrobností, budete potřebovat vyberte požadované materiálů ze souboru podrobnosti pro možnost. Po jejímž uplynutí se pruhovém grafu se znázornit množství denní služba, které vygenerovalo zadaný časové období.

Na levé straně grafu (y) označuje celkový počet požadavků, že aktivum došlo v určitý den. Přímo pod grafem osy (x), bude hledat popisek, který označuje datum (formát: YYYY-MM-DD) pro které CDN byl vykázaného služba majetku.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Pro každý den vztahuje konsolidovaná sestavu tam bude hledat celkový počet přístupů a množství dat převeden (GB).

## <a name="by-file-type"></a>Podle typu souboru

Sestava podle typu souboru umožňuje můžete zobrazit služba a provozu vzniklé typ souboru. Při generování tento typ sestavy, bude označovat prstencový graf Procento přístupů generovaných horní typy 10 souborů.

> [AZURE.TIP] Když najedete myší na výseč grafu prstencový, Internet typ média, že typ souboru se zobrazí jako popisů ovládacích prvků.

Data, která byla použita k získání prstencový graf můžete zobrazit pod ním. Zde najdete název souboru zadejte médií rozšíření/Internet, celkový počet přístupů, Procento přístupů, množství dat převeden (GB) a procento dat převedou pro každý typ souboru horní 250.

## <a name="by-directory"></a>Službou Active Directory

Sestava tak, že adresářů umožňuje můžete zobrazit služba a provozu vynaloženy na konkrétní platformu pro obsah z určitých adresáři. Při generování tento typ sestavy, bude označovat pruhového grafu celkový počet přístupů generovaných obsah v horním 10 adresáře.

### <a name="using-the-bar-chart"></a>Použití pruhového grafu

* Najeďte myší na pruh zobrazit relativní cestu k adresáři odpovídající.
* Obsah uložený v podsložce adresář nepočítá však při výpočtu služba službou Active directory. Výpočet využívá výhradně počet požadavků generovaných pro obsah uložený v adresáři skutečné.
* Pro účely Tato sestava okraj CNAME URL se převedou na adresy URL jejich odpovídající CDN. Díky přesné ověřovací pro všechny statistiky přidružené aktivum bez ohledu na CDN nebo okraj CNAME URL použité o něj požádat.

Na levé straně grafu (y) označuje celkový počet požadavků pro obsah uložený v adresářích prvních 10. Každý pruh v diagramu představuje adresář. Použití barevného kódování schématu podle nahoru pruh do adresáře uvedené v části horní 250 celou adresáře.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Zde najdete tyto informace pro každou z adresáře horní 250: relativní cestu, celkový počet přístupů, Procento přístupů, množství dat převeden (GB) a převedou procentuální hodnotu data.

## <a name="by-browser"></a>V prohlížeči

Sestava pomocí prohlížeče umožňuje zobrazení, které prohlížeče používaly požádat o obsahu. Při generování tento typ sestavy, bude označovat výsečového grafu s přechodem Procento požadavků uskutečněných jednotlivými nejoblíbenější 10 prohlížeče.

### <a name="using-the-pie-chart"></a>Použití výsečového grafu

* Najeďte myší na výsečí ve výsečovém grafu zobrazit název a verze v prohlížeči.
* Pro účely Tato sestava každé kombinace jedinečných prohlížeče se považuje za jiný prohlížeč.
* Výsečí s názvem "Jiné" označuje Procento požadavků uskutečněných jednotlivými všechny ostatní prohlížeče a verze.

Data, která byla použita k vytvoření grafu můžete zobrazit pod ním. Pro každou z prohlížeče horní 250 tam bude hledat číslo typ/verze prohlížeče, celkový počet přístupů a Procento přístupů.

## <a name="by-referrer"></a>Podle Referrer

Sestava Referrer tak, že umožňuje zobrazit Hlavní doporučující osoby k obsahu na vybrané platformě. Referrer označuje hostname, ze které byl vytvořen žádost. Při generování tento typ sestavy, bude pruhového grafu znázornit množství službu (například přístupů) generovaných Hlavní doporučující osoby 10.

Na levé straně grafu (y) označuje celkový počet požadavků, že došlo k aktiva pro každou referrer. Každý pruh v diagramu představuje odkazující server. Použití barevného kódování schématu podle nahoru pruhu referrer uvedené v části horní 250 Referrer.

Data, která byla použita k získání pruhovém grafu můžete zobrazit pod ním. Tam bude hledat adresu URL, celkový počet přístupů a Procento přístupů generovaných ze všech Hlavní doporučující osoby 250.

## <a name="by-download"></a>Stažením

Sestava tak, že stáhnout umožňuje analyzovat stažení vzorků pro většinu požadovaný obsah. Horní části sestavy obsahuje pruhového grafu porovnává pokus stahování s dokončení stahování pro začátek 10 požadované prostředky. Každý pruh je barevně označené podle toho, zda je pokus o stažení (modrý) nebo dokončené stažení (zelený).

> [AZURE.NOTE] Pro účely Tato sestava okraj CNAME URL se převedou na adresy URL jejich odpovídající CDN. Díky přesné ověřovací pro všechny statistiky přidružené aktivum bez ohledu na CDN nebo okraj CNAME URL použité o něj požádat.

Levou stranu grafu (y) označuje název souboru pro každou z horní 10 požadovaná prostředky. Přímo pod grafem osy (x) zjistíte popisky, které označují celkový počet pokus o/dokončení stahování.

Přímo pod pruhového grafu tyto informace se zobrazí pro začátek 250 požadované prostředky: relativní cestu (včetně názvu souboru), počtu opakování, které jste ho stáhli do dokončení, kolikrát, které bylo požadováno a Procento požadavků, jejichž výsledkem je dokončeno ke stažení.

> [AZURE.TIP] Náš CDN není informuje klientem HTTP (tedy prohlížeč) při aktivum úplném stažení. V důsledku toho máme výpočet, zda aktivum úplně stáhli podle stavů a rozsah bajtů žádosti. První věc, kterou jsme vyhledejte při výpočtu se, zda žádost má za následek 200 OK stavový kód. Pokud ano, pak podíváme na rozsah bajtů požadavky na zajistit, aby se týkají celý materiálů. Nakonec jsme porovnat množství dat převedeny na požadovanou velikost požadovaná materiálů. Pokud jsou data převedena rovna nebo větší velikost souboru a žádosti o rozsah bajtů jsou vhodné pro majetek, budou shodě počítat Dokončeno ke stažení.
>
>Z důvodu interpretive přírodní tuhle sestavu můžete by měl mějte na paměti následující body, které může dojít ke změně konzistence a přesnost této zprávy.
>
>* Vzorky provozu nemůžete přesně předpovídat při uživatelských agentů s odlišným chováním. To může způsobit dokončené stáhnout výsledky, které jsou větší než 100 %.
>* Prostředky, které využívají stažení postupné HTTP se nemusí přesně tak, že v této sestavě. Toto je kvůli uživatelům hledání na jiné místo ve videa.

## <a name="by-404-errors"></a>Podle chyby 404

Sestava tak, že chyby 404 umožňuje vystihují typ obsahu, který vytvoří největší počet 404 Nenalezeno stavů. Horní části sestavy obsahuje pruhového grafu pro prvních 10 prostředky, u kterých byla vrácena 404 stavový kód nebyl nalezen. Tento pruhového grafu porovnává celkový počet požadavků s žádostí o, jejichž výsledkem 404 Nenalezeno stav kód aktiva. Každý pruh je barevně označené. Žlutém panelu slouží k označení žádost výsledkem 404 stavový kód nebyl nalezen. Červený pruh slouží k označení celkový počet požadavků pro majetek.

> [AZURE.NOTE] Pro účely tohoto sestavy mějte na paměti:
>
>* Ke shodě představuje všechny žádosti o aktivum bez ohledu na stavový kód.
>* Adresy URL CNAME okraje se převedou na adresy URL jejich odpovídající CDN. Díky přesné ověřovací pro všechny statistiky přidružené aktivum bez ohledu na CDN nebo okraj CNAME URL použité o něj požádat.

Na levé straně grafu (y) označuje název souboru pro každý horní 10 požadovaná majetek, jejichž výsledkem 404 stavový kód nebyl nalezen. Přímo pod grafem osy (x) zjistíte popisky, které označují celkový počet požadavků a počet požadavky, které vedly 404 stavový kód nebyl nalezen.

Přímo pod pruhového grafu tyto informace se zobrazí pro začátek 250 požadované prostředky: relativní cestu (včetně názvu souboru), počet požadavky, které vedly 404 Nenalezeno stavový kód, celkový počet, které bylo požadováno majetek a procento požadavky, které vedly 404 stavový kód nebyl nalezen.

## <a name="see-also"></a>Viz taky
* [Přehled Azure CDN](cdn-overview.md)
* [Data v reálném stat v Microsoft Azure CDN](cdn-real-time-stats.md)
* [Přepíše výchozí nastavení HTTP pomocí modul pravidel](cdn-rules-engine.md)
* [Analýza výkonu okraje](cdn-edge-performance.md)
