<properties
   pageTitle="Azure terminologie katalog dat | Microsoft Azure"
   description="Tento článek obsahuje přehled koncepty a termíny používané v katalogu dat Azure si přečtěte následující dokumentaci."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure terminologie katalogu dat

## <a name="catalog"></a>Katalog

V katalogu dat Azure slouží jako úložiště metadat cloudové v data, která může být registrován zdrojů a dat prostředky. Katalog slouží jako centrální úložiště pro strukturální metadata ze zdroje dat a popisný metadat přidané uživateli.

## <a name="data-source"></a>Zdroj dat

Zdroj dat je systému nebo kontejneru, který má na starosti majetku data. Jako příklad lze uvést databáze systému SQL Server, databáze Oracle, databáze SQL Server Analysis Services (tabulkových nebo multidimenzionálních) a servery služby SQL Server Reporting Services.

## <a name="data-asset"></a>Data majetku

Datové prostředky jsou objekty obsažené v zdroje dat, které můžete registrovaný u katalogu. Jako příklad lze uvést tabulky SQL serveru a zobrazení, Oracle tabulek a zobrazení, SQL Server Analysis Services míry, dimenze a klíčových ukazatelů výkonu a sestavy služby SQL Server Reporting Services.

## <a name="data-asset-location"></a>Umístění majetku dat

Katalog ukládá umístění zdroje dat nebo dat materiálů, které lze použít pro připojení ke zdroji pomocí klientské aplikaci. Podrobnosti o umístění a formátem lišit v závislosti na typu zdroje dat. Tabulce SQL serveru můžete například poznají podle názvu čtyři část – název serveru, název databáze, schématu jména, objekt –, zatímco SQL Server sestavy služby Reporting Services můžete označenými jeho adresu URL.

## <a name="structural-metadata"></a>Konstrukce metadat

Konstrukce metadat je metadata ze zdroje dat, který popisuje strukturu dat materiálů. Jedná se o umístění majetku jeho název objektu a typ a další typ specifické vlastnosti. Například strukturální metadat pro tabulky a zobrazení obsahuje názvy a datové typy sloupců na objekt.

## <a name="descriptive-metadata"></a>Popisný metadat

Popisný metadat je metadata, která popisuje účel nebo záměr majetku data. Obvykle popisný metadat přibude uživateli katalogu na portálu katalog dat Azure, ale jej lze také extrahovat ze zdroje dat při registraci. Například registrační nástroj katalog dat Azure vydělí popisy od vlastnosti popis v SQL Server Analysis Services a služby SQL Server Reporting Services a od [ms_description rozšířené vlastnosti](https://technet.microsoft.com/library/ms190243.aspx) databáze systému SQL Server, pokud těchto vlastností byla vyplněné s hodnotami.

## <a name="request-access"></a>Vyžádání přístupu

Metadata popisný dat materiálů mohou obsahovat informace o tom, jak požádat o přístup ke zdroji dat nebo dat materiálů. Tyto informace jsou uvedeny s umístění majetku dat a mohou obsahovat jednu nebo více z následujících možností:

- E-mailovou adresu uživatele nebo týmu zodpovědný za udělení přístupu ke zdroji dat.
- Adresa URL dokumentovaného procesu, které uživatelé musí odpovídat k získání přístupu ke zdroji dat
- Adresa URL identit a přístup nástroje pro správu (například identita správce), mohou sloužit k získání přístupu ke zdroji dat
- Uvolnit textové položky, které popisuje, jak uživatelé mohou získat přístup ke zdroji dat.

## <a name="preview"></a>Náhled

Jeho náhled v katalogu dat Azure je snímek až 20 záznamy, které lze extrahovat ze zdroje dat při registraci a uložené v katalogu majetku metadaty data. Náhled pomáhá uživatelům, kteří Seznamte se s dat materiálů lépe porozumět tomu, jeho (funkce) a účel. Jinými slovy zobrazuje ukázková data můžou posloužit více než zobrazují jenom názvy sloupců a datových typů.
Náhledy podporují pouze tabulek a zobrazení a musí být explicitně uživatelem vybrané při registraci.

## <a name="data-profile"></a>Profil dat

Profil dat v katalogu dat Azure poskytuje zobrazení tabulky a sloupce úrovně metadat o majetku registrovaných dat, který lze extrahovat ze zdroje dat při registraci a uložené v katalogu s metadaty materiálů data. Data profilu můžou pomoct uživatelů, kteří Seznamte se s dat materiálů lépe porozumět tomu, jeho (funkce) a účel. Podobně jako náhledy dat profilů musí být explicitně zaškrtnuté uživatelem při registraci.

> [AZURE.NOTE] Extrahování dat profilu může být nákladná operace pro rozsáhlých tabulek a zobrazení a může výrazně zvýšit dobu potřebnou k registraci zdroje dat.

## <a name="user-perspective"></a>Pohledu uživatele

V katalogu dat Azure všichni uživatelé zadejte popisný metadat položka registrovaná data. Každý uživatel má odlišné pohledu na data a jeho použití. Například, správce zodpovědný za serveru může poskytovat podrobnosti o jeho smlouva o úrovni služeb (SLA) nebo zálohy systému windows. odkazy na si přečtěte následující dokumentaci pro obchodní procesy dat podporuje; může poskytnout správy hlavních dat a analytik můžete obdržet popis řečeno, které jsou pro ostatní analytici nejdůležitější a může být diplom pro nejužitečnějšího uživatelům, kteří potřebují zjišťovat a interpretaci dat.

Každý z těchto perspektiv se podstatě cenný a s katalogem dat Azure každý uživatel zadat informace, které bude dávat smysl, zatímco všem uživatelům můžete použít tyto informace k pochopení data a její účel.

## <a name="expert"></a>Odborník na prezentace

Odborník je uživatel, který byl zjištěn jako s informovaní "expert" perspektivy položka data. Všichni uživatelé můžete přidat sami nebo jiný uživatel jako odborník majetku. Vedená jako odborník není žádná další oprávnění v katalogu dat Azure; vyjádření umožňuje uživatelům snadno vyhledat těchto perspektivy, které se s největší pravděpodobností být užitečné při prohlížení materiálů popisný metadata.

## <a name="owner"></a>Vlastník

Některé vlastníky je uživatel, který má další oprávnění pro správu dat materiálů v katalogu dat Azure. Uživatele můžete využít vlastnictví aktiv registrovaná data a vlastníci můžete přidat další uživatelé jako spolu vlastníci. Další informace najdete v tématu [Správa aktiv dat](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Vlastnictví a správy jsou k dispozici pouze v standardní vydání z Azure katalogu dat.

## <a name="registration"></a>Registrace

Registrace je příznaky extrahování metadat materiálů data ze zdroje dat a kopírování ke službě katalog dat Azure. Datové prostředky, které jsou registrované můžete pak poznámkami a zjistil.

## <a name="see-also"></a>Viz taky

- [Co je katalog dat Azure?](data-catalog-what-is-data-catalog.md) – Tento článek obsahuje přehled služby Azure katalogu dat, hodnoty, které poskytuje a scénáře, které podporuje.

- [Začínáme s katalogem dat Azure](data-catalog-get-started.md) – Tento článek obsahuje začátku do konce kurzu, který znázorňuje použití katalogu dat Azure pro zjišťování zdroje dat.  
