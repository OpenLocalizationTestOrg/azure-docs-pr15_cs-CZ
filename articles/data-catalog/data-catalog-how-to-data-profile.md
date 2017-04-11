<properties
    pageTitle="Jak se Data zdroje dat."
    description="Článek s postupy zvýraznění jak vložit profily dat úrovni tabulky a sloupce při registraci zdroje dat v katalogu dat Azure a použití dat profilů pochopit zdroje dat"
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Data zdroje dat.

## <a name="introduction"></a>Úvod

**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. **Katalog dat Azure** jinými slovy, je celé o tom pomáhá lidé zjišťovat, pochopit a získání další hodnoty z existujících dat pomocí zdrojů dat a nápovědu organizace. Při registraci zdroje dat s **Katalogem dat Azure**jeho metadata zkopírovány a indexované službou, ale není tam ukončit textu.

**Použití dat profilů** funkcí katalogu **dat Azure** zkontroluje data z podporovaných zdrojů dat ve vašem katalogu a shromažďuje informace o těchto datech a statistiky. Není těžké si zahrnout profilu majetku data. Při registraci majetku data zvolte registrace Nástroje pro zdroj dat **Zahrnout Data profil** .

## <a name="what-is-data-profiling"></a>Co je to vytváření profilů dat

Použití dat profilů kontroluje data ve zdroji dat registrovaná a shromažďuje Statistika a informace o tato data. Při zjišťování zdroje dat tyto statistiky vám pomůže zjistit vhodnosti data, která chcete své obchodní problém vyřešit.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

V následujících zdrojích dat dál nebude podporovat vytváření profilů dat:

- Zobrazení a tabulky SQL serveru (včetně databáze SQL Azure SQL Azure datový sklad)
- Oracle tabulek a zobrazení
- Teradata tabulek a zobrazení
- Podregistru tabulek

Včetně dat profilů při registraci dat prostředky pomáhá uživatelům odpovědi na otázky ohledně zdroje dat, včetně:

-   Ho slouží k řešení problému business?
-   Data dodržuje konkrétní standardy nebo vzorků?
-   Jaké jsou některé nesrovnalosti zdroje dat?
-   Co je možné výzvy integrace tato data do aplikace?

> [AZURE.NOTE] Můžete také přidat si přečtěte následující dokumentaci pro aktivum k tomu, jak možné integrace dat do aplikace. Zjistěte, [jak k dokumentu zdroje dat](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Jak vložit profil dat při registraci zdroje dat

Není těžké si zahrnout profil zdroje dat. Při registraci zdroj dat v panelu **objekty zaregistrovat** registrace Nástroje pro zdroj dat, dialogové okno zvolte **Profil zahrnout Data**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Další informace o tom, jak zaregistrovat zdroje dat najdete v tématu [jak si zaregistrovat zdroje dat](data-catalog-how-to-register.md) a [začít pracovat s katalogem dat Azure](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrování dat prostředky, které obsahují data profilů
Seznamte se s dat prostředky, které obsahují data profilu, můžete zadat `has:tableDataProfiles` nebo `has:columnsDataProfiles` jako jednu z hledaných slov.

> [AZURE.NOTE] Výběr **Zahrnout Data profilu** v registrační nástroj zdroj dat obsahuje tabulky a informace o úrovni sloupce profilu. Rozhraní API katalogu dat však umožňuje dat prostředky k registrovaný u pouze jednu sadu zahrnuté informace o profilu.

## <a name="viewing-data-profile-information"></a>Zobrazení informací o profilu dat

Jakmile najdete vhodné datové zdroje s profilem, můžete zobrazit podrobností profilu data. Pokud chcete zobrazit profil dat, vyberte data materiálů a dialogové okno zvolit **Profil dat** v okně portálu katalog dat.

![](media\data-catalog-data-profile\data-catalog-view.png)

Profil dat v **Katalogu dat Azure** zobrazí tabulky a sloupce profilu informace včetně:

### <a name="object-data-profile"></a>Objekt dat profilu

-   Počet řádků
-   Velikost tabulky
-   Poslední aktualizace objektu

### <a name="column-data-profile"></a>Sloupec dat profilu

- Datový typ sloupce
- Počet různých hodnot
- Počet řádků s hodnoty NULL
- Minimum, maximum, průměr a směrodatnou odchylku pro hodnoty sloupců

## <a name="summary"></a>Souhrn
Data vytváření profilů obsahuje statistiky a informace o registrovaná data prostředky vám pomohou zjistit vhodnosti data, která chcete obchodní problémy vyřešit. Přidání poznámek a dokumentaci zdroje dat, profily dat můžete uživatelům poskytnout hlubší porozumění datům.


## <a name="see-also"></a>Viz taky
-   [Jak si zaregistrovat zdroje dat](data-catalog-how-to-register.md)
-   [Začínáme s katalogem dat Azure](data-catalog-get-started.md)
