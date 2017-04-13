<properties
    pageTitle="Začínáme se synchronizací dat databáze SQL"
    description="Tento kurz vám pomůže začít s Azure SQL Data Sync (verze Preview)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Začínáme se synchronizací dat Azure SQL (verze Preview)
V tomto kurzu se naučíte základy synchronizace dat Azure SQL pomocí portálu klasické Azure.

Tento kurz předpokládá minimální zkušenosti s SQL Server a databázi SQL Azure. V tomto kurzu vytvoříte skupinu synchronizace hybridní (SQL Server a instance databáze SQL) plně nakonfigurované a synchronizace podle plánu, které můžete nastavit.

> [AZURE.NOTE] Úplná technické dokumentace nastavení synchronizace dat SQL Azure, dřív najdete na webu MSDN, je k dispozici jako PDF. Stáhněte si ho [tady](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Krok 1: Připojení k databázi Azure SQL

1. Přihlaste se k [portálu klasické](http://manage.windowsazure.com).

2. V levém podokně klikněte na **SQL databáze** .

3. Klikněte na **SYNCHRONIZOVAT** v dolní části stránky. Když kliknete na tlačítko SYNCHRONIZOVAT, se zobrazí seznam zobrazující věcí, které můžete přidat – **Novou skupinu synchronizace** a **Nový Agent synchronizace**.

4. Spuštění Průvodce nový Agent synchronizovat Data SQL, klikněte na **Nový Agent synchronizace**.

5. Pokud jste ještě nepřidali agent dřív, **klikněte na stáhnout tady**.

    ![Obrazek1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Krok 2: Přidání agenta klienta
Tento krok je vyžadován jenom v případě, že budete mít k databázi SQL Server místní součástí skupiny synchronizace. Pokud skupiny synchronizace obsahuje pouze databáze SQL instance, přeskočte na krok 4.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Krok 2: instalace příslušný software požadovaný
Ujistěte se, jestli máte nainstalovat na počítač, kde nainstalujete Client Agent následující.

- **.NET framework 4.0**

 Nainstalujte .NET Framework 4.0 z [tady](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 CLR typů systémů (x86)**

 Instalace Microsoft SQL Server 2008 R2 SP1 systém typy CLR (x86) z [tady](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Správa objekty (x86) sdílených Microsoft SQL Server 2008 R2 SP1**

 Nainstalujte Microsoft SQL Server 2008 R2 SP1 sdílené Správa objektů (x86) [zde](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Krok 2b: instalace nového klienta agenta

Postupujte podle pokynů v [Instalace agenta klienta (SQL Data Sync)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) a nainstalovat agenta.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Krok 2c: dokončení Průvodce nový Agent synchronizovat Data SQL

1.  Vraťte se do Průvodce nový Agent synchronizovat Data SQL.
2.  Agent smysluplně pojmenujte.
3.  Z rozevírací nabídky vyberte požadovanou **oblast** (datacentrem) hostovat Tento agent.
4.  Z rozevírací nabídky vyberte **PŘEDPLATNÉ** hostovat Tento agent.
5.  Klikněte na šipku doprava.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Krok 3: Registrace databáze SQL serveru s agentem klienta

Po instalaci Client Agent zaregistrujte každé místní databázi SQL serveru, kterou chcete přidat do skupiny synchronizace s agentem.
Zaregistrujte databáze s agentem, postupujte podle pokynů na [zaregistrovat databázi SQL serveru s agentem klienta](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Krok 4: Vytvoření skupiny synchronizace


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Krok 4a: spuštění Průvodce novou skupinu synchronizace

1.  Vraťte se do [klasického portálu](http://manage.windowsazure.com).
2.  Klikněte na **SQL databáze**.
3.  Klikněte na **Přidat SYNCHRONIZACE** v dolní části stránky a pak vyberte Nová skupina synchronizace z okraj.

    ![Obrazek2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Krok 4b: Zadejte základní nastavení


1.  Zadejte vhodný název pro skupinu synchronizace.
2.  Z rozevírací nabídky vyberte požadovanou **oblast** (Datacentrem) hostovat tuto skupinu synchronizace.
3. Klikněte na šipku doprava.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Krok 4c: definování rozbočovači synchronizace

1. Z rozevírací nabídky vyberte instanci systému SQL databáze sloužící jako centrální skupiny synchronizace.
2. Zadejte přihlašovací údaje pro tuto instanci databáze SQL – **Centrální uživatelské jméno** a **Heslo centrální**.
3. Počkejte SQL Data Sync potvrďte uživatelské jméno a heslo. Zobrazí se zelenou značku zaškrtnutí zobrazí napravo od heslo při potvrzeno pověření.
4. Z rozevírací nabídky vyberte zásadu **Vyřešení KONFLIKTŮ** .

 **Služba Wins rozbočovači** - jakákoli změna došlo k zápisu databáze rozbočovači k databázím odkaz zápisu přepisování změny ve stejném odkazovat záznam v databázi. Funkčně znamená to, že první změna došlo k centru zápisu rozšíří do jiné databáze.


 **Klient Wins** - změny došlo k centru zápisu jsou přepíšou změny v databázích odkaz. Funkčně to znamená, že poslední změnu došlo k centru zápisu je k dispozici a rozšíří do jiné databáze.

5.  Klikněte na šipku doprava.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Krok 4d: Přidání odkazu databáze


Opakujte tento krok pro každý další databáze, kterou chcete přidat do skupiny synchronizace.

1. V rozevíracím seznamu vyberte databáze, kterou chcete přidat.

    Databáze v rozevíracím seznamu obsahují obou registrovaných s agent a instance databáze SQL databáze systému SQL Server.
2.  Zadejte přihlašovací údaje pro tuto databázi – **uživatelské jméno** a **heslo**.
3.  Z rozevírací nabídky vyberte **SYNCHRONIZOVAT SMĚR** pro tuto databázi.

    **Obousměrné** - změny v databázi odkaz jsou aby došlo k zápisu centrální databáze a změny v centrální databázi jsou aby došlo k zápisu referenční databáze.

    **Synchronizace z centra** - databázi získávání aktualizací od centru. Neodesílá změny k rozbočovači.

    **Synchronizace k rozbočovači** - databázi odešle aktualizace centru. Změnami rozbočovači nejsou napsali k této databázi.

4.  Dokončete vytváření skupině synchronizace, zaškrtněte políčko v pravém dolním rohu průvodce. Počkejte synchronizovat Data SQL potvrďte pověření. Zelené zaškrtnutí označuje, že jsou pole Potvrzeno pověření.

5.  Klikněte na značku zaškrtnutí ještě jednou. Návrat na stránku **SYNCHRONIZACE** klikněte v části SQL databáze. Tuto skupinu synchronizovat teď společně s vaší synchronizace skupiny a agentů.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Krok 5: Definovat data, která chcete synchronizovat

Synchronizace dat Azure SQL umožňuje výběr tabulek a sloupců tak, aby synchronizovat. Pokud chcete filtrovat sloupec tak, že pouze řádky s určitých hodnot (například stáří > = 65) synchronizovat, pomocí portálu synchronizovat Data SQL v Azure a následující dokumentaci pro vyberte tabulek, sloupců a řádků, které chcete synchronizovat definovat data synchronizovat.

1.  Vraťte se do [klasického portálu](http://manage.windowsazure.com).
2.  Klikněte na **SQL databáze**.
3.  Klikněte na kartu **SYNCHRONIZACE** .
4.  Klikněte na název skupiny synchronizace.
5.  Klikněte na kartu **SYNCHRONIZACE pravidla** .
6.  Vyberte databázi chcete zadat schématu synchronizace skupiny.
7.  Klikněte na šipku doprava.
8.  Klikněte na tlačítko **Aktualizovat schéma**.
9.  Pro každou tabulku v databázi vyberte sloupce, které chcete zahrnout do synchronizace.
    - Nejde vybrat sloupce s nepodporovaných datových typů.
    - Pokud nejsou vybrány žádné sloupce v tabulce, není v tabulce součástí skupiny synchronizace.
    - Chcete-li vybrat nebo zrušit výběr všech tabulkách a klikněte v dolní části obrazovky.
10. Klikněte na tlačítko **Uložit**a potom čekat skupině synchronizace dokončete vytváření.
11. Pokud chcete vrátit na cílovou stránku synchronizace dat, klikněte na tlačítko Zpět v levém horním rohu obrazovky (nad název skupiny synchronizace).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Krok 6: Konfigurace synchronizace skupiny

Kliknutím na tlačítko SYNCHRONIZOVAT v dolní části cílovou stránku synchronizace dat můžete synchronizovat vždy skupinu synchronizace.
Synchronizovat v plánu, nakonfigurujete skupině synchronizace.

1.  Vraťte se do [klasického portálu](http://manage.windowsazure.com).
2.  Klikněte na **SQL databáze**.
3.  Klikněte na kartu **SYNCHRONIZACE** .
4.  Klikněte na název skupiny synchronizace.
5.  Klikněte na kartu **KONFIGUROVAT** .
6.  **AUTOMATICKÉ SYNCHRONIZACE**
    - Konfigurace synchronizace skupiny synchronizovat na frekvenci sadu, klikněte na **zapnuto**. Můžete dál synchronizovat na vyžádání po kliknutí na SYNCHRONIZOVAT.
    - Klepněte na **Vypnuto, pokud** konfigurace synchronizace skupiny synchronizovat pouze v případě, klikněte na SYNCHRONIZOVAT.
7.  **SYNCHRONIZACE ČETNOSTI**
    - Pokud je na automatické SYNCHRONIZACE, nastavte četnost synchronizace. Četnost musí být mezi 5 minut a 1 měsíc.
8.  Klikněte na **Uložit**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Blahopřejeme. Vytvořili jste skupinu synchronizace, která zahrnuje instance SQL databáze a databáze SQL serveru.

## <a name="next-steps"></a>Další kroky
Další informace o databáze SQL a synchronizace dat SQL najdete v článku:

* [Stažení dokončení technické dokumentace synchronizovat Data SQL](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [Přehled databáze SQL](sql-database-technical-overview.md)
* [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
