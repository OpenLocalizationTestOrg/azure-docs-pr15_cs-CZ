<properties
    pageTitle="Určení databází a tabulek pro databázi roztáhnout spuštěním Poradce roztáhnout databáze pro | Microsoft Azure"
    description="Zjistěte, jak identifikovat databází a tabulek, které jsou kandidáty pro roztáhnout databázi."
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
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Určení databází a tabulek pro databázi roztáhnout spuštěním Poradce roztáhnout databáze pro

K identifikaci databází a tabulek, které jsou kandidáty pro databázi roztáhnout, stáhněte SQL serveru 2016 Poradce pro Upgrade a spusťte Poradce roztáhnout databáze. Roztažení Advisor databáze také identifikuje blokování problémy.

## <a name="download-and-install-upgrade-advisor"></a>Stažení a instalace Poradce pro Upgrade
Stáhněte a nainstalujte Poradce pro Upgrade z [tady](http://go.microsoft.com/fwlink/?LinkID=613421). Tento nástroj není součástí systému SQL Server instalačního média.

## <a name="run-the-stretch-database-advisor"></a>Spuštění Poradce roztáhnout databáze

1.  Spusťte Poradce pro Upgrade.

2.  Vyberte **scénáře**a pak vyberte **Spustit ADVISOR ROZTÁHNOUT databáze**.

3.  Na zásuvné **Spustit Advisor databáze roztáhnout** klikněte na **Výběr DATABÁZÍCH analýzu**.

4.  Na zásuvné **Výběr databázích** zadejte nebo vyberte název serveru a informace o ověřování. Klikněte na **Připojit**.

5.  Zobrazí se seznam databází ve vybraném serveru. Vyberte databází, které chcete analyzovat. Klikněte na **Výběr**.

6.  Na zásuvné **Spustit Advisor databáze roztáhnout** klikněte na **Spustit**.  Spuštění analýzy.

## <a name="review-the-results"></a>Prohlédněte si výsledky

1.  Až budete hotovi analýzy, na zásuvné **Analyzed databáze** vyberte jednu z databáze, které analyzovat zobrazíte zásuvné **výsledky analýzy** .

    **Výsledky analýzy** zásuvné seznamy doporučené tabulek ve vybrané databázi, které splňují daná kritéria doporučení výchozí.

2.  V seznamu tabulek na zásuvné **výsledky analýzy** vyberte jednu z doporučených tabulky, které chcete zobrazit **tabulku výsledků** zásuvné.

    Pokud jsou blokuje problémy, zásuvné **tabulku výsledky** jsou uvedeny blokování problémy pro vybranou tabulku. Informace o blokování problémech zjištěná Advisor databáze roztáhnout najdete v článku [omezení roztáhnout databáze](sql-server-stretch-database-limitations.md).

3.  V seznamu blokování problémy na zásuvné **tabulku výsledky** , vyberte jednu z problémy zobrazíte další informace o problému s vybranou a navrhuje omezení rizik kroky. Pokud chcete konfigurovat vybranou tabulku pro databázi Roztáhnout: implementujte kroky navrhované řešení.

## <a name="next-step"></a>Další krok
Povolte roztáhnout databáze.

-   Povolení roztáhnout databáze v **databázi**, najdete v článku [Povolení roztáhnout databáze pro danou databázi](sql-server-stretch-database-enable-database.md).

-   Aby roztáhnout databáze v jiné **tabulce**, je-li roztáhnout už povolena databázi, najdete v článku [Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Viz taky

[Omezení pro roztáhnout databáze](sql-server-stretch-database-limitations.md)

[Povolení roztáhnout databáze pro danou databázi](sql-server-stretch-database-enable-database.md)

[Povolení roztáhnout databáze pro tabulku](sql-server-stretch-database-enable-table.md)

[Všech témat služby Azure SQL Server roztáhnout databáze](sql-server-stretch-database-index-all-articles.md)
