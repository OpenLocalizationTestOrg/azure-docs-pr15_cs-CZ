<properties
    pageTitle="Analýza vzorce použití Azure CDN | Microsoft Azure"
    description="Můžete zobrazit vzorce použití pro vaše CDN pomocí následující sestavy: šířky pásma, přenášet Data, přístupy, stavy mezipaměti, poměr přístupů mezipaměti, přenášet Data IPV4/IPV6."
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

# <a name="analyze-azure-cdn-usage-patterns"></a>Analýza vzorce použití Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Použití vzorce můžete zobrazit pro vaše CDN pomocí následující sestavy:

- Šířka pásma
- Převedení dat
- Přístupy
- Zobrazit stavy mezipaměti
- Poměr přístupů do mezipaměti
- Převedení dat IPV4 a IPV6

## <a name="accessing-advanced-http-reports"></a>Přístup k rozšířené HTTP sestavy

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-reports/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **Základní sestavy** .  Klikněte na požadovanou zprávu v nabídce.

    ![Portálu Správa CDN – nabídka základní sestavy](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Šířka pásma

Sestava šířky pásma se skládá z grafu a dat tabulky označující využití šířky pásma pro HTTP a protokol HTTPS v průběhu určitého časového období. Využití šířky pásma můžete zobrazit přes POP všechny CDN nebo konkrétní POP. Umožňuje prohlížet vrcholy pole přenosy a distribuce CDN POP v MB /.

- Vyberte všechny okraj uzlů v tématu přenos ze všech uzlů nebo vybrat konkrétní oblasti/uzel v rozevíracím seznamu.
- Vyberte rozsah kalendářních dat zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data a potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.
- Můžete exportovat a stáhněte si data kliknutím na ikonu list aplikace excel umístěny vedle "Přejít".

Sestava se aktualizuje každých 5 minut.

![Sestava šířky pásma](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Převedení dat

Tato zpráva se skládá z grafu a dat tabulky označující použití přenosy protokolu HTTP a protokol HTTPS v průběhu určitého časového období. Použití přenosy můžete zobrazit přes POP všechny CDN nebo konkrétní POP. Umožňuje prohlížet vrcholy pole přenosy a distribuce CDN POP v GB.

- Vyberte všechny okraj uzlů v tématu přenos ze všech poznámek nebo vybrat konkrétní oblasti/uzel v rozevíracím seznamu.
- Vyberte rozsah kalendářních dat zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data a potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.
- Můžete exportovat a stáhněte si data kliknutím na ikonu list aplikace excel umístěny vedle "Přejít".

Sestava se aktualizuje každých 5 minut.

![Přenášet data sestavy](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Přístupy (stavů)

Tuhle sestavu popisuje rozdělení žádost o stavu kódy pro obsah. Každý požadavek na obsah vygeneruje stavový kód HTTP. Stavový kód popisuje, jak okraj POP zpracovány žádosti. Například 2xx stavů označuje, že žádosti byla úspěšně podávané množství klientovi, zatímco stavový kód 4xx označuje, že došlo k chybě. Podrobné informace o HTTP stavový kód najdete v článku [kódy stavu](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Vyberte rozsah kalendářních dat zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data a potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.
- Můžete exportovat a stáhněte si data po kliknutí na listu aplikace excel umístěny vedle "Přejít".

![Přístupy sestavy](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Zobrazit stavy mezipaměti

Tuhle sestavu popisuje výskyt přístupy do mezipaměti a Neúspěšné přístupy do mezipaměti pro požadavek klienta. Od nejrychlejší výkon pochází z přístupy do mezipaměti, abyste optimalizovali rychlosti doručení dat minimalizace Neúspěšné přístupy do mezipaměti a vypršela platnost mezipaměti přístupů. Neúspěšné přístupy do mezipaměti můžete sníží nakonfigurováním serveru origin Chcete-li předejít přiřazení "bez ukládání do mezipaměti" odpověď záhlaví vyloučením řetězce dotazu ukládání do mezipaměti kromě v případě potřeby sporná, i když a vyloučením kódů – vytvářených odpovědí. Vypršela platnost mezipaměti přístupy můžete vyhnout tak, že aktivum je maximální počet stáří tak dlouho minimalizovat počet požadavků na zdrojový server.

![Sestava stavy mezipaměti](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Hlavní mezipaměti stavy patří:

- TCP_HIT: Podávané množství od okraje. Objekt byl v mezipaměti a nebyla Překročená jeho stáří max.
- TCP_MISS: Podávané množství od počátku. Objekt není v mezipaměti a odpovědi se zpátky do origin.
- TCP_EXPIRED _MISS: podávané množství od počátku po Revalidace s origin. Objekt byl v mezipaměti, ale překročil jeho stáří max. Revalidace s origin výsledkem je někdo jiný nahradil tak, že nové odpovědi z původního objektu mezipaměti.
- TCP_EXPIRED _HIT: podávané množství od okraje po Revalidace s origin. Objekt byl v mezipaměti, ale překročil jeho stáří max. Revalidace je zdrojový server výsledkem objektu mezipaměti právě smíte bez jakýchkoli úprav.

- Vyberte rozsah kalendářních dat zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data a potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.
- Můžete exportovat a stáhněte si data kliknutím na ikonu list aplikace excel umístěny vedle "Přejít".

### <a name="full-list-of-cache-statuses"></a>Úplný seznam stavů mezipaměti

- TCP_HIT - tento stav se zobrazí při žádost pochází přímo z POP klientovi. Aktivum okamžitě zobrazovaná POP až do mezipaměti na nejblíže k desktopovému klientovi POP a má platný, time-to-live nebo TTL. Hodnota TTL, je určený podle následujících hlavičky odpovědi:

    - Mezipaměti ovládacího prvku: s maxage
    - Mezipaměti ovládacího prvku: max stáří
    - Vypršení platnosti

- TCP_MISS - Tenhle stav označuje, že režim cached verzi požadované materiálů nebyla nalezena na nejbližší klientovi POP. Aktiva budete požádáni z zdrojovému serveru nebo zdrojovému serveru znaku. Vrátí-li zdrojový server nebo zdrojový server znaku aktivum, bude podávané množství klientovi a cached na klienta a edge serveru. V opačném-200 stavový kód (například 403 Zakázáno, 404 Nenalezeno, atd.), bude vrácena.

- TCP_EXPIRED _HIT - tento stav se zobrazí při žádost o cílové aktivum s ukončenou platností TTL, například kdy majetku max stáří vypršení platnosti, byl doručen přímo z POP klientovi.

    Žádost o konverzaci vypršela platnost obvykle vytváří požadavek revalidace zdrojový server. Aby TCP_EXPIRED _HIT problémy zdrojový server musí být uvedeno, že na novější verzi majetku neexistuje. Tento typ situace obvykle aktualizuje mezipaměti řízení a platnost vyprší záhlaví, které materiálů.

- TCP_EXPIRED _MISS - tento stav se zobrazí při na novější verzi vypršela platnost režim cached materiály ke klientovi pochází z POP. V takovém po vypršení TTL režim cached položka (například prošlý max věk) a zdrojový server vrátí na novější verzi tohoto materiálů. Tato nová verze majetku bude podávané klientovi místo v mezipaměti. Navíc ji do mezipaměti budou edge serveru a klienta.

- CONFIG_NOCACHE - tento stav označuje, že konfigurace specifické pro zákazníka na naše okraji POP zabráněno majetku do mezipaměti.

- ŽÁDNÁ - tento stav znamená, že zaškrtnutí aktuálnost obsahu mezipaměti nebyla provedena.

- TCP_ CLIENT_REFRESH _MISS - tento stav se zobrazí při klientovi HTTP (například prohlížeč) vynutí hranu POP k načtení novou verzi zastaralé materiálů ze zdrojového serveru.

    Ve výchozím nastavení naše servery zabránit v klientovi HTTP vynucení naše serverech a získat novou verzi majetku ze serveru origin.

- TCP_ PARTIAL_HIT - tento stav je uveden při žádost oblast bajt výsledkem ke shodě částečně režim cached položka. Požadovaný rozsah bajtů je okamžitě zobrazovaná POP klientovi.

- UNCACHEABLE - tento stav se zobrazí při majetku mezipaměti řízení a platnost vyprší záhlaví označuje, že ho neměli ukládat do mezipaměti POP nebo klientem HTTP. Tyto typy požadavků je zobrazovaná zdrojový server

## <a name="cache-hit-ratio"></a>Poměr přístupů do mezipaměti

Tato zpráva ukazuje procento režim cached požadavků, které byly podávané množství přímo z mezipaměti.

Sestava poskytuje následující údaje:

- Požadovaný obsah byl mezipaměti na nejbližší žadateli POP.
- Žádost byl doručen přímo od okraje naší síti.
- Žádost not požadovat prodloužení doby je zdrojový server.

Sestava neobsahuje:

- Požadavky, které jsou odepřen kvůli země možnosti filtrování.
- Požadavky na prostředky, jejichž záhlaví označuje, že by neměli mezipaměti. Například mezipaměti ovládacích prvků: private, mezipaměti ovládacích: bez mezipaměti nebo Pragma: mezipaměti bez záhlaví zabrání aktivum do mezipaměti.
- Bajt oblast žádosti o obsah částečně v mezipaměti.

Vzorec: (záznam NALEZEN na TCP_ / (TCP_ PŘÍSTUPŮ + TCP_MISS)) * 100

- Vyberte rozsah kalendářních dat zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data a potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.
- Můžete exportovat a stáhněte si data kliknutím na ikonu list aplikace excel umístěny vedle "Přejít".


![Sestavu poměru přístupů do mezipaměti](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Převedení dat IPV4 a IPV6

Tato zpráva zobrazuje distribuci použití přenosy v IPV4 a IPV6.

![Převedení dat IPV4 a IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Vyberte rozsah kalendářních dat a zobrazení dat pro aktuální/tento týden/tento měsíc, atd. nebo zadejte vlastní data.
- Potom klikněte na "Komu" Zkontrolujte, jestli že se aktualizuje vašeho výběru.


## <a name="considerations"></a>Co byste měli zvážit

Sestavy můžete generovat pouze během posledních 18 měsíců.
