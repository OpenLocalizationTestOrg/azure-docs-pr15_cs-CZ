<properties
   pageTitle="Katalog dat Azure často kladené otázky | Microsoft Azure"
   description="Časté otázky k katalog dat Azure, včetně možností pro zjišťování zdroje dat, poznámky a správu."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Nejčastější dotazy Azure katalogu dat

Tento článek obsahuje nejčastější dotazy týkající se ke službě Microsoft **Azure katalog dat** odpovědi.

## <a name="q-what-is-azure-data-catalog"></a>Otázka: Jaký je katalog dat Azure?

Odpověď: katalog dat Microsoft Azure je plně spravované služby hostované v cloudu společnosti Microsoft Azure, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. Azure katalog dat obsahuje funkcí, které povolit kterýmkoli uživatelem – z analytici dat vědeckých pro vývojáře – zaregistrovat, seznamte se s principy a používání zdrojů dat.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>Otázka: jaké zákazníků přijal se pokusí znamená katalog dat Azure vyřešit?

Azure katalog dat adresy na výzvu zjišťování zdroje dat a "tmavě data" tím, že uživatelé zjišťovat a interpretaci zdroje dat organizace.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>Otázka: kdo jsou cílových skupin pro katalog dat Azure?

Katalog dat Azure poskytuje možnosti pro technické a netechnických uživatelů, včetně:

- Vývojáři dat, BI a technologie pro analýzu profesionály: kdo je zodpovědný za vytváření dat a analýzy obsahu pro jiné uživatele k používání
-   Osoby zodpovědné za rámci dat: Uživatelům, kteří knowledge o data, co to znamená: a jak je určená pro použití a pro konkrétní účel
- Uživatelé data: Uživatelům, kteří mají mít možnost snadno zjišťovat, pochopit a připojení k datům potřebné k dělat svou práci pomocí nástroje zvoleném
- Centrální IT: ty, kteří potřebují zpřístupnění konfigurací stovky zdroje dat pro firemní uživatele a potřebují udržovat dohled přes využití dat a kým

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>Otázka: Jaký je oblast dostupnost katalog dat Azure?

Služby Azure katalog dat jsou aktuálně dostupné v těchto datacentrech:

- Západ USA
- Východní USA
- Západní Evropě
- Severní Evropě
- Austrálie východ
- Jihovýchodní Asie

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>Otázka: Jaká jsou omezení počtu prostředky dat v katalogu dat Azure?

Bezplatné Edition z Azure katalogu dat se omezí na 5 000 registrovaná data prostředky.

Standardní vydání z Azure katalogu dat podporuje až 100 000 prostředky registrovaná data.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>Otázka: jaké podporované datové typy zdrojových a materiálů?

U seznamu současnosti podporované zdroje dat získáte [DSR katalogu dat](data-catalog-dsr.md) .

## <a name="q-how-do-i-request-support-for-another-data-source"></a>Otázka: Jak můžu žádost o podporu pro jiný zdroj dat?

Poslat žádost o funkci a jiné názory můžete odeslat v [katalogu dat Azure fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>Otázka: Jak můžu začít s katalogem dat Azure?

Nejlepší místem začít je podle pokynů v části [Začínáme s katalogem dat](data-catalog-get-started.md). Tento článek je přehled funkcí ve službě začátku do konce.

## <a name="q-how-do-i-register-my-data"></a>Otázka: jak zaregistrovat Moje data?

Zaregistrujte datům v katalogu dat Azure, spusťte nástroj katalog dat Azure registrace z oblasti "Publikovat" portálu katalog dat Azure. V katalogu dat Azure publikování aplikací Přihlaste se pomocí stejné přihlašovací údaje, který používáte pro přístup k portálu katalog dat Azure a pak vyberte zdroj dat a konkrétní prostředky, které se chcete zaregistrovat.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>Otázka: jaké vlastnosti extrahování pro data prostředky, které jsou registrované?

Specifické vlastnosti se liší od zdroje dat ke zdroji dat, ale obecně služby publikování katalog dat Azure vydělí následující informace:

- Název majetku
- Typ majetku
- Popis materiálů
- Názvy atribut/sloupců
- Atribut či sloupec datové typy
- Atribut či sloupec popisu

> [AZURE.IMPORTANT] Registrace prostředky dat pomocí katalogu dat Azure přesunutí nebo zkopírování dat do cloudu. Registrace prostředky ze zdroje dat zkopíruje datových zdrojů metadat Azure, ale data zůstanou v existující umístění zdroje dat. Jedinou výjimkou tohoto pravidla je, když uživatel vybere nahrát náhled záznamů nebo profilu dat při registraci prostředky. Kdy včetně náhledu až 20 záznamů se zkopírují z každé materiálů a uložená jako snímek v katalogu dat Azure. Při vkládání dat profilu, bude souhrnné informace (třeba velikost tabulky, procentuální hodnoty null ve sloupci a minimální, maximální a průměr hodnot pro sloupce) počítaného a součástí metadata uložená v katalogu.

<br/>

> [AZURE.NOTE] V případě zdrojů dat třeba SQL Server Analysis Services, máte prvotřídní vlastnosti **Popis** v katalogu dat Azure publikování aplikace vydělí hodnotu. Pro relační databáze systému SQL Server, které nemají prvotřídní vlastnosti **Popis** , publikování aplikace katalog dat Azure vydělí hodnotu z ms_description rozšířená vlastnost pro objekty a sloupce. Další informace najdete v tématu TechNet [Pomocí rozšířené vlastnosti na databázové objekty](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>Otázka: jak dlouho má trvá nově registrovaných prostředky zobrazit v katalogu dat Azure?

Po registraci majetku s katalogem dat Azure doména může obsahovat tečku 5 až 10 sekund, než se zobrazí v portálu katalog dat Azure.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Otázka: jak pořizovat poznámky a obohacení metadata majetku Moje registrované dat?

Nejjednodušší způsob, jak poskytnout metadat pro registrovaných prostředky je vyberte majetek na portálu katalog dat Azure a potom zadat hodnoty metadat v okně vlastností nebo schématu pro vybraný objekt.

Některé metadat, třeba odborníky a značky, můžete zadat také během procesu registrace. Zadané hodnoty v katalogu dat Azure službu bude platit pro všechny majetek registrovaná v té době. Objekty naposledy registrovaná na portálu pro další poznámky zobrazíte klikněte na tlačítko **Portál zobrazení** na poslední obrazovce publikování aplikace katalog dat Azure.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>Otázka: jak se odstraňuje Moje registrované datové objekty?

Odstranění objektu z katalogu dat Azure vyberete objekt v portálu a kliknutím na tlačítko **Odstranit** . Odebere metadata objektu z katalogu dat Azure ale neovlivní skutečné podkladovém zdroji dat.

## <a name="q-what-is-an-expert"></a>Otázka: Jaký je odborník?

Odborník je osoba, která má informovaní perspektivy o objekt data. Objekt může obsahovat více odborníkům. Odborník nemusí být "vlastníka" pro konkrétní objekt; Poradce je jednoduše někoho, kdo věděli, jak můžete data a bude použito.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>Otázka: jak se sdílejí informace s týmem katalog dat Azure Pokud vyskytnou problémy?

Použití katalogu dat Azure fórum hlášení problémů, sdílení informací a položte otázky. Najdete na fóru na http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>Otázka: Azure katalog dat funguje s Tento jiného zdroje dat, kterou jsem zájem o?
Aktivně pracujeme na přidání další zdroje dat do katalogu dat Azure. Pokud je zdroj dat, který byste chtěli najdete v článku podporované, zadejte ho navrhne (nebo hlasovou svoji podporu, pokud již byly doporučovány) ve [fóru katalog dat Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Otázka: jak katalog dat Azure souvisí v katalogu dat v Power BI pro Office 365?

Katalog dat Azure si můžete představit jako vývoji katalog dat. Katalog dat Azure poskytuje podobné funkce pro publikování zdroje dat a zjišťování, ale je soustředit na širší scénářů a nezávislé na Office 365. Brzy po v katalogu dat Azure obecně zpřístupní dvou katalogy bude sloučit do jedné služby.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>Otázka: jaká oprávnění musí uživatel přístup k registraci majetku s katalogem dat Azure?

Spuštění nástroje pro registraci katalog dat Azure uživatel musí oprávnění ve zdroji dat, která vám umožní mu číst metadata ze zdroje. Pokud uživatel slouží také k výběru, zobrazil náhled, klikněte uživatele taky může mít oprávnění povolujících ho zobrazíte v řádcích dat z objektů registrovaná.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Otázka: Azure katalog dat budou dostupné taky v místním nasazení?

Azure katalog dat je do cloudové služby, které můžete pracovat s cloudu a místní zdroje dat, zjišťování řešení hybridní dat zdroje. Neexistují žádné plány pro verze služby Azure katalogu dat, který se spustí místní.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>Otázka: můžeme extrahovat další / bohatší metadata ze zdroje dat, které jsme zaregistrovat?

Pracujeme aktivně rozbalte funkcí katalogu dat Azure. Pokud tam je další metadata, která se mají zobrazit extrahované ze zdroje dat při registraci, přejděte prosím byste (nebo je-li již byly doporučovány hlasování) ve [fóru katalog dat Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). V budoucnu můžeme vám umožní jinými výrobci přidat nové datové typy zdrojových prostřednictvím rozšiřitelnost rozhraní API.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Otázka: jak se tak, aby je mohli zjišťovat pouze určitým uživatelům omezit viditelnost prostředky registrovaná data?

Odpověď: Vyberte prostředky dat v katalogu dat Azure a klikněte na tlačítko "Převzít vlastnictví". Vlastníci datových zdrojů dat v katalogu dat Azure můžete změnit nastavení viditelnosti pro buď všem uživatelům katalogu zjistit vlastněná prostředky nebo omezit viditelnost pro konkrétní uživatele.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Otázka: jak aktualizovat registrace pro majetku dat do, nezměnilo ve zdroji dat se projeví v katalogu?

A: když Pokud chcete aktualizovat metadat pro data prostředky, které jsou již registrována v katalogu, stačí znovu zaregistrujte zdroji dat, která obsahuje majetek. Změny ve zdroji dat jako sloupce přidávání nebo odebírání z tabulky nebo zobrazení, aktualizují v katalogu, ale žádné poznámky poskytovanou uživatelé budou zachovány.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>Otázka: Můj byste nenašli odpověď otázku tady – Co mám dělat?

Hlavy na myší na [Fórum komunity katalog dat Azure](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Otázky tam bude hledat jejich způsobem.
