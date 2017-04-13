<properties
    pageTitle="Začínáme spuštěním databáze povolit roztáhnout Průvodce | Microsoft Azure"
    description="Informace o konfiguraci databáze pro databázi roztáhnout spuštěním databáze povolit roztáhnout průvodce."
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
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Začínáme spuštěním databáze povolit roztáhnout Průvodce

Konfigurace databáze pro databázi roztáhnout, spusťte Průvodce roztáhnout databáze povolit.  Toto téma popisuje možnosti, které je potřeba nastavit v průvodci a informace, které musíte zadat.

Další informace o databáze roztáhnout, najdete v článku [Roztáhnout databáze](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Později Pokud zakážete roztáhnout databáze, mějte na paměti, že zakážete roztáhnout databáze pro tabulky nebo databáze se neodstraní vzdálený objekt. Pokud chcete odstranit vzdálené tabulky nebo vzdálené databázi, budete muset pusťte pomocí portálu pro správu Azure. Vzdálené objekty dál vynakládá Azure, dokud neodstraníte ručně. 

## <a name="launch-the-wizard"></a>Spuštění Průvodce

1.  V SQL Server Management Studio v Průzkumníku objekt vyberte databázi, na které chcete povolit roztáhnout.

2.  Vpravo\-klikněte na a vyberte **úkoly**a pak vyberte **Roztáhnout**a pak vyberte **Povolit** a spusťte průvodce.

## <a name="Intro"></a>Úvod
Prohlédněte si účel průvodce a požadavky.

Důležitých předpokladech patřit následující úkoly:

-   Musíte být správce chcete změnit nastavení databáze.
-   Musíte mít předplatné Microsoft Azure.
-   SQL Server je třeba komunikovat s vzdálený server Azure.

![Úvodní stránka průvodce roztáhnout databáze][StretchWizardImage1]

## <a name="Tables"></a>Výběr tabulky
Vyberte tabulky, které chcete povolit pro roztáhnout.

Tabulky se spoustou řádků se zobrazí v horní části seznamu seřazené. Před Průvodce zobrazí seznam tabulek, analyzuje je pro datové typy, které aktuálně nepodporuje se roztáhnout databáze.

![Vyberte tabulky stránka průvodce roztáhnout databáze][StretchWizardImage2]

|Sloupec|Popis|
|----------|---------------|
|(bez názvu)|Zaškrtněte políčko v tomto sloupci povolit vybranou tabulku pro roztáhnout.|
|**Jméno**|Určuje název sloupce v tabulce.|
|(bez názvu)|Symbol v tomto sloupci představují upozornění, které není\'t zabránit povolení vybranou tabulku roztáhnout. Mohou také představovat blokování problém, který brání povolení vybranou tabulku roztáhnout \- například vzhledem k tomu tabulce používá nepodporované datového typu. Najeďte myší na symbol zobrazíte další informace v popisů ovládacích prvků. Další informace najdete v článku [omezení roztáhnout databáze](sql-server-stretch-database-limitations.md).|
|**Roztažení**|Určuje, zda je v tabulce už povoleno pro roztáhnout.|
|**Migrace**|Můžete migrovat celou tabulku (**Celou tabulku**) nebo můžete použít filtr na existující sloupec v tabulce. Pokud chcete použít jiný filtr vyberte řádky, které chcete migrovat, spusťte příkaz ALTER TABLE chcete po ukončení průvodce zadat funkci filtr. Další informace o funkci Filtr najdete v článku [Vyberte řádky pomocí funkce filter migrovat](sql-server-stretch-database-predicate-function.md). Další informace o použití této funkci najdete v tématu [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md) nebo [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Řádky**|Určuje počet řádků v tabulce.|
|**Velikost (KB)**|Určuje velikost tabulky ve znalostní bázi Knowledge Base.|

## <a name="Filter"></a>Pokud chcete zadat filtr řádků

Pokud chcete zadat funkci Filtr vyberte řádky, které chcete migrovat, proveďte následující akce na stránce **Výběr tabulek** .

1.  V seznamu **Vyberte tabulky, které chcete roztáhněte** klikněte v řádku tabulky na **Celou tabulku** . Otevře se dialogové okno **Vyberte řádky roztáhnout** .

    ![Definování funkce filter][StretchWizardImage2a]

2.  V dialogovém okně **Vybrat řádky, které roztáhněte** vyberte **Vyberte řádky**.

3.  Do **pole Název**zadejte název pro funkci filtr.

4.  Klauzule **Where** vyberte sloupec z tabulky, vyberte operátor a zadejte hodnotu.

5. Klikněte na **Zkontrolovat** otestovat funkce. Pokud funkce vrátí výsledky z tabulky – to znamená, pokud nejsou řádky migrace, které odpovídají podmínce - test sestavy **Úspěch**.

    >   [AZURE.NOTE] Do textového pole, která se zobrazí dotaz filtru je jen pro čtení. Nemůžete upravovat dotaz do textového pole.

6.  Klepněte na Hotovo se vrátíte na stránce **Výběr tabulek** .

Funkce filter se vytvoří na serveru SQL Server pouze po dokončení průvodce. Do té doby chcete se vrátit na stránku **Vyberte tabulky, které** chcete změnit nebo přejmenování funkci filtr.

![Vyberte stránku tabulek nadefinovaná funkce filter][StretchWizardImage2b]

Pokud chcete použít jiný typ funkce Filtr vyberte řádky, které chcete migrovat, proveďte jeden z následujících možností:  

-   Ukončete průvodce a spusťte příkaz ALTER TABLE povolení roztáhnout tabulky a zadat funkci filtr. Další informace najdete v tématu [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md).  

-   Spusťte příkaz ALTER TABLE můžete určit funkce filter po ukončení průvodce. Kroky najdete v článku [Přidání funkce filter po spuštění Průvodce](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Konfigurace Azure nasazení

1.  Přihlaste se k Microsoft Azure pod svým účtem Microsoft.

    ![Přihlaste se k Azure – Průvodce roztáhnout databáze][StretchWizardImage3]

2.  Vyberte stávající Azure předplatné pro účely roztáhnout databáze.

3.  Vyberte Azure oblast.
    -   Pokud vytvoříte nový server, vytvoří se v této oblasti serveru.  
    -   Pokud budete mít stávající servery na vybranou oblast, Průvodce uvádí seznam je při volbě **existující serveru**.

    Aby byl minimalizován latence, vyberte Azure oblast, ve kterém se nachází SQL serveru. Další informace o oblastech najdete v článku [Azure oblastí](https://azure.microsoft.com/regions/).

4.  Určete, zda chcete použít existující server nebo vytvořit nový server Azure.

    Pokud služby Active Directory na serveru SQL Server je federované se službou Azure Active Directory, můžete volitelně používat účet federované služby pro systém SQL Server komunikovat s vzdálený server Azure. Další informace o požadavcích na tuto možnost najdete v článku [Změnit nastavení možnosti databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Vytvoření nového serveru**

        1.  Vytvoření přihlašovací jméno a heslo pro správce serveru.

        2.  Můžete také použijte účet federované služby pro systém SQL Server komunikovat s vzdálený server Azure.

        ![Vytvoření nového Azure serveru – Průvodce roztáhnout databáze][StretchWizardImage4]

    -   **Existující server**

        1.  Vyberte stávající server Azure.

        2.  Vyberte metodu ověření.

            -   Pokud vyberete **Ověřování serveru SQL Server**, zadejte správce přihlašovací jméno a heslo.

            -   Vyberte **Integrované ověřování služby Active Directory** používat účet federované služby pro systém SQL Server komunikovat s vzdálený server Azure. Pokud je vybraný server není integrovaný s Azure Active Directory, tato možnost nezobrazí.

        ![Vyberte stávající server Azure – Průvodce roztáhnout databáze][StretchWizardImage5]

## <a name="Credentials"></a>Zabezpečené přihlašovací údaje
Musíte mít hlavní klíč databáze zajistit přihlašovací údaje, které roztáhnout databázi používá pro připojení ke vzdálené databázi.  

Pokud hlavní klíč databáze již existuje, zadejte heslo pro něj.  

![Zabezpečená pověření stránka průvodce roztáhnout databáze][StretchWizardImage6b]

Pokud není databázi existující primární klíč, zadejte silné heslo vytvořit hlavní klíč databáze.  

![Zabezpečená pověření stránka průvodce roztáhnout databáze][StretchWizardImage6]

Další informace o klíči hlavní databáze najdete v článku [vytvoření hlavní klíč (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) a [vytvořit hlavní klíč databáze](https://msdn.microsoft.com/library/aa337551.aspx). Další informace o přihlašovacích údajů, které průvodcem najdete v článku [Vytvoření databáze omezené přihlašovacích údajů (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Vyberte IP adresa
Vytvořit pravidlo bránu firewall na Azure, která umožňuje serveru SQL Server komunikovat s vzdálený server Azure pomocí rozsah IP adres podsítí (doporučeno) nebo veřejnou IP adresu serveru SQL Server.

IP adresa nebo adresy, které jsou zdrojem na této stránce řekněte Azure serveru tak, aby příchozí data, dotazů a Správa operací zahájil: po serveru SQL Server přes Azure bránu firewall. Průvodce nezmění něco v nastavení brány firewall na serveru SQL Server.

![Vyberte IP adresu stránka průvodce roztáhnout databáze][StretchWizardImage7]

## <a name="Summary"></a>Souhrn
Zkontrolujte hodnoty, které jste zadali a možnosti, které jste vybrali v průvodci a odhad nákladů v Azure. Klikněte na povolit roztáhnout **Dokončit** .

![Souhrnné stránce průvodce roztáhnout databáze][StretchWizardImage8]

## <a name="Results"></a>Výsledky
Prohlédněte si výsledky.

Chcete-li sledovat stav migrace dat, najdete v článku [sledování a odstraňování problémů s migrací dat (roztáhnout databáze)](sql-server-stretch-database-monitor.md).

![Stránky výsledků v Průvodci roztáhnout databáze][StretchWizardImage9]

## <a name="KnownIssues"></a>Poradce při potížích s průvodci
**Průvodce roztáhnout databáze se nezdařila.**
Pokud roztáhnout databáze dosud není povolený na úrovni serveru a spusťte průvodce bez systému oprávnění správce k tomu, průvodce se nezdaří. Požádejte správce systému povolíte roztáhnout databáze u instance místního serveru a pak znova spusťte průvodce. Další informace najdete v tématu [Základní: oprávnění k povolení roztáhnout databáze na serveru](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Další kroky
Povolení dalších tabulek pro databázi roztáhnout. Sledovat migraci dat a spravovat roztáhnout\-povolené databází a tabulek.

-   [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md) povolit další tabulky.

-   [Sledování a odstraňování problémů s migrací dat](sql-server-stretch-database-monitor.md) se stav migrace dat.

-   [Pozastavit roztáhnout databáze](sql-server-stretch-database-pause.md)

-   [Správa a řešení potíží s roztáhnout databáze](sql-server-stretch-database-manage.md)

-   [Zálohování databáze roztáhnout s podporou](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Viz taky

[Povolení roztáhnout databáze pro danou databázi](sql-server-stretch-database-enable-database.md)

[Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
