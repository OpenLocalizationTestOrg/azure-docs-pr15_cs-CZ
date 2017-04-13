<properties
    pageTitle="Srovnávací přehled Azure SQL databáze"
    description="Toto téma popisuje srovnávacích databáze SQL Azure měření výkonu databáze SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL databázi srovnávací přehled

## <a name="overview"></a>Základní informace
Databázi Microsoft Azure SQL nabízí tři [úrovně služby](sql-database-service-tiers.md) s více úrovněmi výkonu. Každou úroveň výkonu obsahuje rostoucí řadu zdrojů nebo "power" navrženy pro stále vyšší výkon.

Je důležité mít možnost podpořte jak rostoucí power jednotlivých úrovní výkonu převede na výkon lepší databáze. Dělat toto Microsoft vyvíjí srovnávací databáze SQL Azure (ASDB). Testu vykonává kombinaci základních operací součástí všechny úlohami OLTP. Jsme změřte výkon dosáhnout pro databáze systém v jednotlivých úrovní výkonu.

Zdroje a mocniny jednotlivých úrovní osy a výkonu služby jsou vyjádřena pomocí [Jednotek databázové transakce (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs lze popsat relativní kapacita úroveň výkonu na základě prolnutí rozměru procesoru, paměti a čtení a zápis sazby nabízená jednotlivých úrovní výkonu. Velikosti DTU hodnocení databáze rovná velikosti power databáze. Testu umožňuje posuďte jejich dopad na výkon databáze zvýšení výkonu nabízená jednotlivých úrovní výkonu uplatněním skutečné databáze operace při měřítka velikost databáze, počet uživatelů a sazby transakce podle zdroje uvedené v databázi.

Vyjádření výkon osy služby základní pomocí transakce hodinové vrstvy standardní služeb pomocí transakce za minutu a vrstvy služeb Premium pomocí transakce rychlostí, umožňuje jednodušší rychle výkon potenciální jednotlivé vrstvy služeb požadavky aplikace.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Srovnávací výsledky reálné databáze výkonu
Je důležité pochopit, že ASDB, jako jsou všechny ukazatele je zástupce a orientační pouze. Kurzy transakce dosáhnout s aplikací srovnávacích nebudou stejné jako ty, které může dosáhnout s jinými aplikacemi. Testu zahrnuje skupinu různých transakce, které typy kontrolovat schéma obsahující oblasti tabulkách a datových typů. Během testu vykonává stejné základní operace, které jsou společná pro všechny úlohami OLTP, nepředstavuje konkrétní třídy databáze nebo aplikace. Cílem testu je poskytovat vodítko rozumné relativní výkon databázi, která lze očekávat při změně velikosti nahoru nebo dolů mezi úrovněmi výkonu. Ve skutečnosti databáze jsou různé velikosti a složitosti, dojde k jiné mix pracovního vytížení a odpoví různými způsoby. Například aplikace vstupu a výstupu vyžadující značnou může přístupů mezní hodnoty vstupu a výstupu dříve nebo aplikace procesoru vyžadující značnou může přístupů procesoru limity dříve. Není nijak zaručené, který konkrétní databázi bude přizpůsobit stejným způsobem jako srovnávacích klikněte v části zvětšení načíst.

Testu a jeho metodiky se podrobněji píše níže.

## <a name="benchmark-summary"></a>Srovnávací přehled
ASDB výkonů kombinaci operace základní databáze započteny modus-nejčastěji v online transakce zpracování pracovního vytížení (OLTP). Přestože testu slouží s cloudu výpočetních v paměti, schéma databáze, naplnění dat a vytvářeli transakce být obecně představující nejčastěji používané v OLTP úloh základní prvky.

## <a name="schema"></a>Schéma
Schéma slouží k máte dost různých a složitosti podporuje širokou škálu operace. Testu spustí databáze skládá šest tabulek. Tabulky rozdělit do tří kategorií: pevnou velikostí, změnu měřítka a Kvetoucí. Existují dvě tabulky pevnou velikostí; tři změny velikosti tabulky; a jednu rostoucí tabulku. Pevnou velikostí tabulky mají pevného počtu řádků. Změny velikosti tabulky mají mohutnosti, která je poměrné databáze výkon, ale nezmění během testu. Velikost rostoucí tabulky je nastavena jako změny velikosti tabulky na počáteční zatížení, ale pak změny mohutnosti při spuštění testu řádky jsou vložené a odstraněné.

Schéma zahrnuje kombinaci datových typů, včetně celé číslo, číselné, znak a datum a čas. Schéma zahrnuje primární a sekundární klíče, ale ne všechny cizích klíčů – to znamená, jsou bez omezení referenční integrity mezi tabulkami.

Analytický nástroj Generátor sady dat generuje údaje o počáteční databáze. Celé číslo a číselná data je vytvořený pomocí různých strategie. V některých případech hodnoty se v průběhu náhodně oblasti. V ostatních případech množiny hodnot je náhodně permutovanou funkci zajistit zachování konkrétní rozdělení. Textová pole se vytvářejí váženého seznamu slova, která chcete plodiny reálné vypadající data.

Databázi je velikosti podle "měřítko." Měřítko (zkracuje SF) určuje mohutnosti měřítko a Kvetoucí tabulek. Viz dále v části Uživatelé a Pacing, velikost databáze, počet uživatelů a maximální výkon všech měřítko podle sebe.

## <a name="transactions"></a>Transakce
Pracovní zátěž se skládá z devět transakce typů, jak je uvedeno v následující tabulce. Každou transakci slouží ke zvýraznění konkrétní sadu vlastností systému v databázi engine a systémem hardwaru, s vysokým kontrastem z jiných transakce. Tento přístup usnadňuje posuďte, jaký vliv mají na jiné součásti celkový výkon. Například "Čtení hodně" transakce vytvoří významné počet operací čtení z disku.

| Typ transakce | Popis |
|---|---|
| Přečtěte si Lite | VÝBĚR; v paměti. jen pro čtení |
| Střední pro čtení | VÝBĚR; převážně v paměti; jen pro čtení |
| Těžké pro čtení | VÝBĚR; převážně není v paměti; jen pro čtení |
| Aktualizace Lite | AKTUALIZOVAT; v paměti. čtení a zápis |
| Aktualizace těžké | AKTUALIZOVAT; převážně není v paměti; čtení a zápis |
| Vložení Lite | VLOŽIT. v paměti. čtení a zápis |
| Vložení těžké | VLOŽIT. převážně není v paměti; čtení a zápis |
| Odstranění | ODSTRANĚNÍ; kombinaci v paměti a ne v paměti; čtení a zápis |
| Procesor těžké | VÝBĚR; v paměti. relativně velkém zatížení procesoru; jen pro čtení |

## <a name="workload-mix"></a>Pracovní zátěž mix
Transakce jsou vybrány náhodně z váženého rozdělení následující celkové kombinaci. Celková kombinaci má pro čtení i zápis poměr přibližně 2:1.

| Typ transakce | % Mix |
|---|---|
| Přečtěte si Lite | 35 |
| Střední pro čtení | 20 |
| Těžké pro čtení | 5 |
| Aktualizace Lite | 20 |
| Aktualizace těžké | 3 |
| Vložení Lite | 3 |
| Vložení těžké | 2 |
| Odstranění | 2 |
| Procesor těžké | 10 |

## <a name="users-and-pacing"></a>Uživatelé a pacing
Pracovní zátěž srovnávacích úsilím z nástroje, které odesílá transakce přes sadu připojení simulovat většího počtu uživatelů současně. Sice počítače generovaného všechna připojení a transakce pro zjednodušení označovány tato připojení jako "users". Ačkoli každý uživatel pracuje nezávisle na ostatních uživatelů, proveďte všichni uživatelé stejného obrázku kroky ukázáno v následujícím příkladu:

1. Vytvoření připojení k databázi.
2. Opakujte, dokud signál ukončíte:
    - Vyberte transakce (z náhodně váženého rozdělení).
    - Provedení vybraného transakce a měření doby odezvy.
    - Počkejte pacing zpoždění.
3. Zavřete připojení k databázi.
4. Ukončení.

Náhodně vybrané pacing zpoždění (v kroku 2c), ale s rozdělením, které obsahuje průměr 1.0 druhé. Každý uživatel můžete, tedy průměrně generovat maximálně jednu transakci sekundu.

## <a name="scaling-rules"></a>Změna měřítka pravidel
Počet uživatelů, kteří je určený podle velikost databáze (v jednotkách – měřítko). Je jeden uživatel pro každý pět jednotky – měřítko. Z důvodu pacing zpoždění můžete jeden uživatel generovat maximálně jednu transakci sekundu, průměrně.

Například-násobku 500 (SF = 500) databáze bude obsahovat 100 uživatelů a můžete dosáhnout maximální rychlosti 100 TPS. Jednotka vyšší TPS kurz vyžaduje více uživatelů a větší databáze.

Následující tabulka ukazuje počet uživatelů ve skutečnosti tím pro každou úroveň osy a výkonu služby.

| Vrstvy služeb (úroveň výkonnosti) | Uživatelé | Velikost databáze |
|---|---|---|
| Základní | 5 | 720 MB |
| Standardní (S0) | 10 | 1 GB |
| Standardní (S1) | 20 | 2.1 GB |
| Standardní (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Doba trvání měrných jednotek
Platné výkonnosti vyžaduje konstantní stavu měření dobu trvání alespoň jednu hodinu.

## <a name="metrics"></a>Metriky
Klíčové metriky v testu jsou výkon a dobu odezvy.

- Výkon je míra základní výkonu v testu. Výkon je uveden ve transakcí za jednotku předčasné, počítání všechny typy transakcí.
- Doba odezvy je míra předvídatelnost výkonu. Omezení času odpověď se liší podle předmětu službou vyšší třídy službu s přísnější odpověď čas požadavek, jak je ukázáno v následujícím příkladu.

| Třída služby  | Míra výkon | Požadavky na dobu odezvy |
|---|---|---|
| Premium | Transakce za sekundu | 95. percentil na 0,5 sekund |
| Standardní | Transakce za minutu | 90 percentilu v 1.0 sekund |
| Základní | Transakce za hodinu | 80. percentil na 2.0 sekund |

## <a name="conclusion"></a>Uzavření
Srovnávací databáze SQL Azure výkonů relativní databáze SQL Azure spuštěný přes oblast úrovní dostupnými službami a výkonu úrovně. Testu vykonává kombinaci operace základní databáze započteny modus-nejčastěji v online transakce zpracování pracovního vytížení (OLTP). Měřením skutečný výkon testu poskytuje smysluplnější posoudit dopad na výkon změny na úrovni výkonu, než je možné jenom seznam zdrojů poskytnutých jednotlivých úrovní například rychlosti procesoru, velikosti paměti a procesorů. V budoucnu budeme dál vyvíjejí srovnávacích rozšířit jeho obor a rozbalte data k dispozici.

## <a name="resources"></a>Zdroje informací
[Úvod k SQL databázi](sql-database-technical-overview.md)

[Služba úrovní a výkonu úrovně](sql-database-service-tiers.md)

[Pokyny pro výkonu pro jednu databáze](sql-database-performance-guidance.md)
