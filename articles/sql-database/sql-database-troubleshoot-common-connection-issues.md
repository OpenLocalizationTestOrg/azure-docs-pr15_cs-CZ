<properties
    pageTitle="Poradce při potížích s běžné problémy s připojením k databázi SQL Azure"
    description="Postup při identifikovat a vyřešit běžné chyby při připojení k databázi SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Řešení potíží s připojením k databázi SQL Azure

Připojení k databázi SQL Azure se nezdaří, dostáváte [chybové zprávy](sql-database-develop-error-messages.md). Tento článek je centralizované téma, které pomáhají řešit problémy s připojením databáze SQL Azure. Uvádí [společné způsobuje](#cause) problémy s připojením, doporučuje [Nástroj řešení potíží](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , který vám pomůže identity problém a obsahuje kroky k řešení [přechodná chybám](#troubleshoot-transient-errors) a [trvalý nebo nepřechodná](#troubleshoot-the-persistent-errors). Nakonec zobrazí se seznam [všechny příslušné články pro problémy s připojením databáze SQL Azure](#all-topics-for-azure-sql-database-connection-problems).

Pokud narazíte na problémy s připojením, zkuste Poradce při potížích s kroky, které jsou popsané v tomto článku.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Příčina

Problémů s připojením může být způsobeno některým z následujících akcí:

- Chyba při použití osvědčených postupů a pokyny pro návrh během procesu návrhu aplikace.  V tématu [Přehled vývoje databáze SQL](sql-database-develop-overview.md) začít pracovat.
- Konfigurace databáze SQL Azure
- Nastavení brány firewall
- Časový limit připojení
- Nesprávný přihlašovací údaje
- Maximální limit dosažení na několik zdrojů informací databáze SQL Azure

Obecně vzato problémy s připojením k databázi SQL Azure můžete je možné rozdělit následujícím způsobem:

- [Přechodné chyby (krátkodobý nebo chybovému)](#troubleshoot-transient-errors)
- [Trvalý nebo nepřechodná chyby (chyby, které se pravidelně opakují)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Zkuste Poradce při potížích pro problémy s připojením databáze SQL Azure

Pokud dojde k chybě konkrétní připojení, zkuste [Tento nástroj](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), který pomůže vám rychle identit a řešení problémů.

## <a name="troubleshoot-transient-errors"></a>Poradce při potížích přechodná
Pokud aplikace je dochází k chybám přechodná, projděte si následující témata pro tipy, jak s řešeními problémů a snížit frekvenci tyto chyby:

- [Poradce při potížích s databáze &lt;x&gt; na serveru &lt;y&gt; není k dispozici (Chyba: 40613)](sql-database-troubleshoot-connection.md)
- [Poradce při potížích s Diagnostika a zabránit chybám připojení SQL a přechodná databáze SQL](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Poradce při potížích s trvalý chyby (chyby nepřechodná)

Pokud aplikaci trvale nepodaří připojit k databázi SQL Azure, obvykle označuje problém s některým z následujících akcí:

- Konfigurace brány firewall. Azure SQL databáze nebo klientských brána firewall blokuje připojení k databázi SQL Azure.
- Sítě konfigurace na straně klienta: můžete například novou IP adresu nebo proxy server.
- Chyba uživatele: například chybně parametry připojení, například název serveru v připojovacím řetězci.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Postup, jak vyřešit problémy s připojením trvalý

1.  Nastavení [pravidel pro brány firewall](sql-database-configure-firewall-settings.md) pro IP adresu povolit klienta.
2.  Na všechny brány firewall mezi klientem a na Internetu Zkontrolujte, zda je port 1433 otevřené pro odchozí připojení. Další ukazatele naleznete v tématu [Konfigurace brány Windows Firewall povolit přístup SQL serveru](https://msdn.microsoft.com/library/cc646023.aspx) .
3.  Ověřte připojovacího řetězce a další nastavení připojení. V části připojovací řetězec v [tématu připojení problémy](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Zkontrolujte stav služby na řídicím panelu. Pokud si myslíte, že je výpadku místní, najdete v článku [obnovení z výpadku](sql-database-disaster-recovery.md) postup obnovit novou oblast.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Všech témat pro problémy s připojením databáze SQL Azure

Následující tabulka obsahuje téma problém každé připojení, které platí přímo ve službě databáze SQL Azure.


| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 1 | [Řešení potíží s připojením k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Toto je cílová stránka pro řešení potíží s problémy s připojením databáze SQL Azure. Popisuje, jak identifikovat a vyřešit přechodná chybám a trvalý nebo nepřechodná v databázi SQL Azure. |
| 2 | [Poradce při potížích s Diagnostika a zabránit chybám připojení SQL a přechodná databáze SQL](sql-database-connectivity-issues.md) | Informace o řešení potíží s Diagnostika a zabránit SQL připojení chybu nebo přechodná chybu v databázi SQL Azure. |
| 3 | [Obecné pokyny pro přechodná poruch zpracování](best-practices-retry-general.md) | Obsahuje obecné pokyny pro přechodná poruch zpracování při připojení k databázi SQL Azure. |
| 4 | [Vyřešit problémy s připojením s databázi Microsoft Azure SQL](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Tento nástroj pomáhá identity váš problém vyřešit chyby připojení. |
| 5 | [Poradce při potížích s "databáze &lt;x&gt; na serveru &lt;y&gt; není momentálně neexistuje. Opakujte připojení později"chyby](sql-database-troubleshoot-connection.md) | Popisuje, jak identifikovat a vyřešit 40613 Chyba: "databáze &lt;x&gt; na serveru &lt;y&gt; není momentálně neexistuje. Opakujte připojení později." |
| 6 | [Kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů](sql-database-develop-error-messages.md) | Poskytuje informace o kódy chyb SQL databáze SQL klientských aplikacích, například běžných chyb připojení databáze, problémy kopii databáze a obecné chyby. |
| 7 | [Azure SQL databáze výkonu pokyny pro jeden databáze](sql-database-performance-guidance.md) | Obsahuje pokyny vám pomůže určit, které služby osy je nejlepší pro aplikaci. Také poskytuje doporučení pro optimalizaci aplikaci využívejte databázi SQL Azure. |
| 8 | [Přehled vývoje databáze SQL](sql-database-develop-overview.md) | Odkazy na ukázek kódu pro různé technologiemi, které vám pomohou připojit a práce s databáze SQL Azure. |
| 9 | Upgrade databáze SQL Azure v12 stránku ([Azure portál](sql-database-upgrade-server-portal.md), [Powershellu](sql-database-upgrade-server-powershell.md)) | Obsahuje pokyny pro upgradu existující V11 databáze SQL Azure servery a databází na V12 databáze SQL Azure pomocí Azure portál nebo Powershellu. |


## <a name="next-steps"></a>Další kroky

- [Databáze SQL Azure řešení problémů s výkonem](sql-database-troubleshoot-performance.md)
- [Poradce při potížích oprávnění databáze SQL Azure](sql-database-troubleshoot-permissions.md)
- [Všechny témata pro službu databáze SQL Azure](sql-database-index-all-articles.md)
- [Hledání v dokumentaci na Microsoft Azure](http://azure.microsoft.com/search/documentation/)
- [Zobrazení nejnovějších aktualizacích pro službu databáze SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Další zdroje informací

- [Přehled vývoje databáze SQL](sql-database-develop-overview.md)
- [Obecné pokyny pro přechodná poruch zpracování](../best-practices-retry-general.md)
- [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md)
- [Naučná stezka pro používání databáze SQL Azure](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Naučná stezka pro používání pružná databáze funkcí a nástrojů](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
