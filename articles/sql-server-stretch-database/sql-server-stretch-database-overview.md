<properties
    pageTitle="Roztažení databázi přehled | Microsoft Azure"
    description="Zjistěte, jak roztáhnout databáze migraci studenou dat transparentně a bezpečné do cloudu společnosti Microsoft Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Roztažení databázi přehled

Roztažení databáze migruje studenou dat transparentně a bezpečné Microsoft Azure cloudu.

Pokud chcete začít s databází roztáhnout, najdete v článku [Začínáme databáze povolit roztáhnout Průvodce spuštěním](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Jaké jsou přínosy roztáhnout databáze?
Roztažení databáze poskytuje následující výhody:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Poskytuje náklady\-efektivní dostupnost pro studenou data
Roztažení teplé a studenou transakční data dynamicky ze serveru SQL Server Microsoft Azure s roztáhnout databáze systému SQL Server. Na rozdíl od typické studenou úložný prostor data jsou vždy online a ostatní vás dotazu. Můžete zadat delší časové osy uchovávání dat bez přerušení banky velkým tabulkám jako historie pořadí zákazníka. Využívat zhoršeným nákladů Azure nikoli měřítka drahé na\-prostory úložiště. Zvolte ceny osy a nakonfigurujte nastavení portálu Azure zachování kontroly nad náklady. Podle potřeby změňte velikost nahoru nebo dolů. Navštivte web [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) podrobnosti.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Nevyžaduje změny dotazy nebo aplikace
Přístup k datům serveru SQL Server Bezproblémová ohledu na to, jestli se jedná\-prostory nebo roztažen do cloudu.  Nastavení zásad, která určuje, kde jsou uložena data, a SQL Server zpracovává přesun dat na pozadí. Celou tabulku je vždy online a jako dotazovatelné. A, roztáhnout databáze nevyžaduje žádné změny stávajících dotazy nebo aplikací – dat umístěný úplně průhledné aplikaci.

### <a name="streamlines-on-premises-data-maintenance"></a>Zjednodušuje na\-prostory Údržba dat
Omezit na\-prostory údržbu a úložiště pro vaše data. Zálohování pro vaše na\-místní data běžet rychleji a dokončení v okně údržbu. Zálohování v cloudu část dat spustí automaticky. Vaše na\-požadavky na úložiště místní výrazně jsou menší. Azure úložiště může být 80 % levnější než přidání zapnuto\-prostory SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Zachová data zabezpečené i během migrace
Líbit míru paměti při roztažení aplikace nejdůležitější bezpečně do cloudu. SQL Server vždy zašifrovaných poskytuje šifrování pro vaše data v pohybu. Řádek úroveň zabezpečení (RLS) a další funkce zabezpečení systému SQL Server taky spolupracovat s databází roztáhnout chránit vaše data.

## <a name="what-does-stretch-database-do"></a>Co dělá roztáhnout databáze?
Po povolení roztáhnout databáze pro instanci systému SQL Server, databáze a alespoň jednu tabulku, nebude zahájen tiše studenou data migrovat do Azure roztáhnout databáze.

-   Pokud ukládáte studenou dat do tabulky, můžete migrovat celou tabulku.

-   Pokud tabulka obsahuje teplé a studené data, můžete zadat funkci Filtr vyberte řádky, které chcete migrovat.

**Nemusíte změnit existující dotazy a klientské aplikace.** Budete mít dál bezproblémové přístup k datům místních i vzdálený i během migrace dat. Existuje malou část latence pro vzdálené dotazy, ale jenom dojde k této latence dotazu studenou data.

**Že roztáhnout databáze zajistí žádná data ztratí** , pokud dojde k selhání v průběhu migrace. Je také opakovat logiky pro zpracování problémy s připojením, ke kterým může dojít během migrace. Správa dynamických zobrazení poskytuje stav migrace.

**Zastavte ukazatel myši migraci dat** k řešení problémů s místním serverem nebo maximalizovat dostupné šířka pásma.

![Roztažení databázi přehled][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Roztáhnout databáze je pro vás?
Pokud uděláte následující příkazy, roztáhnout databáze může pomoci, odpovídají vašim požadavkům a vyřešení problémů.

|Pokud jste rozhodnutí maker|Pokud nejste databáze|
|------------------------------|-------------------|
|Musím uchovávat transakční data poměrně dlouho.|Velikost tabulek budete posílat z ovládacího prvku.|
|Někdy mám studenou data dotazu.|Svoje uživatele Řekněme, že mají přístup k datům studenou, ale používají jen zřídka.|
|Aplikace, včetně starší aplikace, které se nemají aktualizovat mám.|Musím zachovat nákupem a přidání další úložiště.|
|Chci najít způsob, jak ušetřit úložný prostor.|Můžu zálohovat nebo obnovit takové rozsáhlých tabulek v SLA.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Jaký druh databází a tabulek jsou kandidáty pro roztáhnout databázi?
Roztažení cílů transakční databáze s velkým množstvím studenou data, obvykle uložená v malým počtem tabulek. Tyto tabulky může obsahovat více než miliard řádky.

Pokud používáte funkci časový tabulky SQL serveru 2016, použijte k migraci část nebo celý tabulky přidružené historie nákladů roztáhnout databáze\-efektivní úložiště v Azure. Další informace najdete v tématu [Správa uchovávání informací z historických dat verzí systému časový tabulek](https://msdn.microsoft.com/library/mt637341.aspx).

Použijte Poradce pro databázi roztáhnout, funkce SQL serveru 2016 Upgrade Advisor, k identifikaci databází a tabulek pro databázi roztáhnout. Další informace najdete v tématu [Představení databází a tabulek pro databázi roztáhnout](sql-server-stretch-database-identify-databases.md). Další informace o možných problémech blokování, najdete v článku [omezení roztáhnout databáze](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Test disku roztáhnout databáze
**Test disku roztáhnout databázi ukázkové databázi AdventureWorks.** Ukázkové databázi AdventureWorks stáhnete aspoň souboru databáze a soubor ukázky a skripty z [tady](https://www.microsoft.com/download/details.aspx?id=49502). Po obnovení databáze ukázkové instanci systému SQL Server 2016 unzip soubor ukázek a otevřete soubor roztáhnout DB ukázek ze složky roztáhnout DB. V tomto souboru a kontrola místo, které používají data před a po povolení roztáhnout databáze, můžete sledovat průběh migraci dat a potvrďte, že můžete pokračovat v dotazu existujících dat a vkládání nových dat během i po migraci dat, spusťte skripty.

## <a name="next-step"></a>Další krok
**Zjistěte, databází a tabulek, které jsou kandidáty pro roztáhnout databázi.** Stažení SQL serveru 2016 Poradce pro Upgrade a spusťte Poradce roztáhnout databáze k identifikaci databází a tabulek, které jsou kandidáty pro roztáhnout databázi. Roztažení Advisor databáze také identifikuje blokování problémy. Další informace najdete v tématu [Představení databází a tabulek pro databázi roztáhnout](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
