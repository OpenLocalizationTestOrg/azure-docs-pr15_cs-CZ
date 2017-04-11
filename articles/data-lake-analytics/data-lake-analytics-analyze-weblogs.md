<properties
   pageTitle="Analýza protokoly webu pomocí analýzy jezera dat Azure | Azure"
   description="Zjistěte, jak analyzovat protokoly webu pomocí analýzy dat jezera. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Kurz: Analýza protokoly webu pomocí analýzy jezera dat Azure

Zjistěte, jak analyzovat protokoly webu pomocí technologie pro analýzu dat jezera, zejména v hledání, které doporučující osoby narazila chyby při pokusu o na webu.

>[AZURE.NOTE] Pokud chcete zobrazit aplikace práce, ušetříte čas projít [interaktivní výukové programy pro použití Azure dat jezera analýzy](data-lake-analytics-use-interactive-tutorials.md). Tento kurz se podle stejný scénář a téhož kódu. Tomto kurzu účel dát vývojáři prostředí vytvoření a spuštění aplikace jezera analýzy dat z konce.

## <a name="prerequisites"></a>Požadavky:

- **Visual Studio 2015, Visual Studio 2013 aktualizovat 4, nebo Visual Studio 2012 s nainstalované Visual C++**.
- **Microsoft Azure SDK pro .NET verze 2,5 nebo vyšší**.  Instalace pomocí [webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Jezera data Tools for Visual Studio](http://aka.ms/adltoolsvs)**.

    Po instalaci používat datové nástroje jezera for Visual Studio, zobrazí se nabídka **Jezera dat** ve Visual Studiu:

    ![Nabídka U SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Základní znalost jezera analýzy dat a datové nástroje jezera for Visual Studio**. Začít pracovat, najdete tady:

    - [Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](data-lake-analytics-get-started-portal.md).
    - [Pomocí nástrojů jezera datového Visual Studio skript vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).

- **Analýzy dat jezera účet.**  V tématu [Vytvoření účet Azure dat jezera analýzy](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Datové nástroje jezera nepodporuje vytváření analýzy dat jezera účty.  Takže budete muset vytvořit pomocí portálu Azure, Azure Powershellu, .NET SDK nebo Azure rozhraní příkazového řádku.
- **Nahrajte ukázkových dat do účtu dat jezera analýzy.** V tématu [Nahrání SearchLog.tsv výchozí účet jezera úložný prostor](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Spuštění úlohy jezera analýzy dat, musíte se některá data. Přestože datové nástroje jezera podporuje nahrávání data, použijete odešlete vzorová data usnadnit postupujte podle kurzu, na portálu.

## <a name="connect-to-azure"></a>Připojení k Azure

Před můžete vytvářet a testovat libovolný skripty U SQL, musíte nejdřív připojit k Azure.

**Připojení k jezera analýzy dat**

1. Otevřete aplikaci Visual Studia.
2. V nabídce **Jezera dat** klikněte na **Nastavení a možnosti**.
4. Pokud někdo má přihlášení klikněte na **Sign In**nebo **Změna uživatele** a postupujte podle pokynů.
5. Klikněte na **OK** zavřete dialogové okno možností a nastavení.

**Umožňuje přecházet účtů jezera analýzy dat**

1. Z aplikace Visual Studio otevřete **Průzkumníka serveru** stiskněte **Kombinaci kláves CTRL + ALT + S**.
2. V systému Windows **Server Explorer**rozbalte **Azure**a potom rozbalte **Dat jezera analýzy**. Musí se zobrazí seznam účtů analýzy dat jezera Pokud jsou k dispozici. Nemůžete vytvořit účty jezera analýzy dat z studio. Vytvořit účet, najdete v tématu [Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](data-lake-analytics-get-started-portal.md) nebo [Začít pracovat s Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Můžete vyvíjet aplikace U SQL

U jazyka SQL aplikace je většinou skript U SQL. Další informace o U SQL, najdete v článku [Začínáme s U-SQL](data-lake-analytics-u-sql-get-started.md).

Přidání uživatelem definovaných operátory můžete přidat aplikaci.  Další informace najdete v tématu [operátory pro analýzy dat jezera úlohy definované uživatelem vyvíjet U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Vytvoření a odeslání úlohy jezera analýzy dat**

1. V nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.
2. Vyberte typ projektu U-SQL.

    ![Nový projekt Visual Studio U SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klikněte na **OK**. Visual studio vytvoří řešení se souborem Script.usql.
4. Zadejte tento skript do souboru Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U jazyka SQL, najdete v tématu [Začínáme s jazykem analýzy jezera dat U-SQL](data-lake-analytics-u-sql-get-started.md).    

5. Přidání nového skript U SQL do projektu a zadejte tyto údaje:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Přepnout zpátky na první skript U SQL a vedle tlačítko **Odeslat** , zadejte svůj účet analýzy.
7. V **Okně Průzkumník** **Script.usql**klikněte pravým tlačítkem a pak klikněte na **Vytvořit index**. Zkontrolujte výsledky v podokně výstupu.
8. V **Okně Průzkumník** **Script.usql**klikněte pravým tlačítkem a pak klikněte na **Odeslat index**.
9. Ověřte, zda **Analýzy účet** je místo, kam chcete spuštění úlohy a pak klikněte na **Odeslat**. Odkaz úlohy a odeslání výsledků jsou k dispozici v nástrojích jezera datového Visual Studio výsledky okna po dokončení podávání.
10. Počkejte, až po úspěšném dokončení projektu.  Pokud úlohy selže, chybí pravděpodobně ve zdrojovém souboru.  Získáte základní části tohoto kurzu. Další informace o odstraňování potíží najdete v článku [sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Po dokončení projektu, zobrazí se na následující obrazovce:

    ![analýzy dat jezera analýzu blogy webu protokolů](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Teď opakujte kroky 10 7 – pro **Script1.usql**.

>[AZURE.NOTE]Nelze číst nebo zapisovat do tabulky U SQL, který byl vytvořili nebo změnili ve stejném skriptu.  Proto použití pomocí dvou skriptů v tomto příkladu.

**Chcete-li zobrazit výstup projektu**

1. V systému Windows **Server Explorer**rozbalte **Azure**, rozbalte **Jezera analýzy dat**, rozbalte účtu jezera analýzy dat, rozbalte **Úložiště účtů**, klikněte pravým tlačítkem na výchozí účet jezera ukládání dat a klepněte na položku **Průzkumník**.
2.  Poklikejte na **vzorky** otevřít složku a potom poklepejte **výstupů**.
3.  Poklikejte na **UnsuccessfulResponsees.log**.
4.  Poklikejte na položku ve výstupním souboru v zobrazení Diagram projektu, pokud chcete přejít přímo do výstupu.

## <a name="see-also"></a>Viz taky

Začínáme s jezera analýzy dat pomocí různých nástrojů, najdete tady:

- [Začínáme s jezera analýzy dat pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s jezera analýzy dat pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Začínáme s jezera analýzy dat pomocí .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Pokud chcete zobrazit další témata vývoj:

- [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)
- [Můžete vyvíjet operátory definované uživatelem U jazyka SQL pro úlohy jezera analýzy dat](data-lake-analytics-u-sql-develop-user-defined-operators.md)
