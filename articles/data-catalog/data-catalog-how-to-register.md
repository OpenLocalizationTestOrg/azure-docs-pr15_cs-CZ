<properties
   pageTitle="Jak si zaregistrovat zdroje dat | Microsoft Azure"
   description="Článek s postupy zvýraznění jak si zaregistrovat zdroje dat s katalogem dat Azure, včetně pole metadat extrahovaných při registraci."
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


# <a name="how-to-register-data-sources"></a>Jak si zaregistrovat zdroje dat

## <a name="introduction"></a>Úvod
**Katalog dat Microsoft Azure** je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování pro zdroje dat organizace. **Katalog dat Azure** jinými slovy, je celé o tom pomáhá lidé zjišťovat, pochopit a získání další hodnoty z existujících dat pomocí zdrojů dat a nápovědu organizace. A prvním krokem při vytváření zjistitelný pomocí **Katalogu dat Azure** zdroje dat registrovat tomuto zdroji dat.
## <a name="registering-data-sources"></a>Registrace zdroje dat
Registrace je proces extrahování metadat ze zdroje dat a kopírování tato data do **Katalogu dat Azure** služby. Data zůstanou uložena ho aktuálně umístěn kde zůstane v části ovládací prvek správců a zásady aktuální systému.

Zaregistrovat zdroj dat, jednoduše spuštění registrační nástroj **Katalog dat Azure** datový zdroj z portálu Microsoft **Azure katalogu dat** . Protokol pomocí svého pracovního nebo školního účtu (stejné Azure Active Directory přihlašovací údaje slouží k přihlášení k portálu) a pak vyberte zdroj dat, který chcete zaregistrovat.
V tématu [Začínáme s katalogem dat Azure](data-catalog-get-started.md) kurz další podrobné informace.

Jakmile zaregistroval zdroje dat katalogu sleduje jeho umístění a indexy jeho metadata tak, aby uživatelé mohli hledání, procházet a seznamte se s zdroje dat a připojení k ní pomocí aplikace nebo nástroje zvoleném pomocí jeho umístění.

## <a name="sources-supported"></a>Podporované zdroje
U seznamu současnosti podporované zdroje dat získáte [DSR katalogu dat](data-catalog-dsr.md) .
<br/>


## <a name="structural-metadata"></a>Konstrukce metadat
Po zaregistrování zdroje dat registrační nástroj bude extrahovat informace o struktuře objekty, které vyberete – to je uvedená jako strukturální metadata.

U všech objektů zahrne tato strukturální metadata umístění na objekt, tak, aby uživatelé, kteří Seznamte se s data můžete pomocí těchto informací pro připojení k objektu v klientských nástrojích podle svého výběru. Další strukturální metadata obsahuje název objektu a typ a název atributu/sloupce dat zadejte.

## <a name="descriptive-metadata"></a>Popisný metadat
Kromě základní strukturální metadata ze zdroje dat registrační nástroj zdroj dat vydělí taky i popisný metadata. SQL Server Analysis Services a služby SQL Server Reporting Services se považuje z popis vlastností zveřejněné příslušným těchto služeb. Pro systém SQL Server bude extrahovat hodnoty zadané pomocí ms_description rozšířená vlastnost. Databáze Oracle registrační nástroj zdroj dat vydělí sloupci KOMENTÁŘE v zobrazení ALL_TAB_COMMENTS.

Kromě popisná metadata extrahuje ze zdroje dat můžete uživatelům také zadat popisný metadat pomocí nástroje k registraci zdroje dat. Uživatele můžete přidat značky a zjistit, které odborníci pro objekty registrovaná. Všechny tato popisný metadata zkopírována ke službě **Katalog dat Azure** spolu s strukturální metadata.

## <a name="including-previews"></a>Včetně náhledy

Ve výchozím nastavení pouze metadata ze zdroje dat a zkopírovala do služby **Azure katalogu dat** , ale principy, které zdroje dat je často jednodušší veřejné ukázková data v ní.

Nástroj **Katalog dat Azure** zdroj dat registrace umožňuje uživatelům, zobrazil náhled snímku dat v každé tabulce a zobrazení, které máte zaregistrované. Pokud uživatel rozhodne se změnami zahrnout náhledy při registraci, nástroj registrace bude obsahovat až 20 záznamy z jednotlivých tabulek a zobrazení. Tento snímek potom zkopírovány do katalogu spolu s strukturální a popisný metadata.


> [AZURE.NOTE]  Široký tabulky s velkým počtem sloupců může mít méně než 20 záznamů součástí jejich náhled.


## <a name="including-data-profiles"></a>Včetně dat profilů

Stejně jako včetně náhledy můžete kontext cenné pro uživatele hledání zdrojů dat v **Katalogu dat Azure**, včetně profil dat můžete taky snadněji pochopit zjištěnou daty.

Nástroj **Katalog dat Azure** zdroj dat registrace umožňuje uživatelům zahrnout profil dat pro každou tabulku a zobrazení, které máte zaregistrované. Pokud uživatel vybere zahrnout data profil při registraci, nástroj registrace bude obsahovat souhrnných statistických údajů o data v každé tabulce a zobrazení, včetně:

* Počet řádků a velikost dat v objektu
* Datum poslední aktualizace dat a schématu objektu
* Počet null záznamů a odlišné hodnoty pro sloupce
* Hodnoty minimum, maximum, průměr a směrodatnou odchylku sloupce

Tyto statistiky zkopírovány do katalogu spolu s strukturální a popisný metadata.

> [AZURE.NOTE]  Sloupce text a datum nezahrnuje průměr či směrodatná odchylka statistiky ve svých dat profilu.

## <a name="updating-registrations"></a>Aktualizace registrace

Registrace zdroje dat, bude zjistitelný v **Katalogu dat Azure** pomocí metadat a volitelné náhled extrahovaných při registraci. Pokud zdroj dat je potřeba aktualizovat v katalogu (například pokud schématu objektu změnila, nebo tabulky původně vyloučené by měly být součástí nebo chce uživatel aktualizovat data součástí náhledy) registrační nástroj zdroj dat můžete znovu spustit.

Znovu registrace zdroj dat již registrována provádí operace sloučení "upsert": existující objekty se aktualizují, když se vytvoří nové objekty. Metadatům poskytovanou uživatelů prostřednictvím portálu **Katalog dat Azure** bude zachován.

## <a name="summary"></a>Souhrn
Registrace zdroje dat s **Katalogem dat Azure** snazší daného zdroje dat zjišťovat a interpretaci, kopírování strukturální a popisný metadata ze zdroje dat do služby katalogu. Jakmile registrované zdroj dat, ho můžete pak být poznámkami spravované a zjištěnou na portálu **Katalog dat Azure** .

## <a name="see-also"></a>Viz taky
- [Začínáme s katalogem dat Azure](data-catalog-get-started.md) kurz podrobné informace o tom, jak zaregistrovat zdroje dat
