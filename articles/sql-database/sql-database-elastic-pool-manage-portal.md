<properties
    pageTitle="Sledování a správa fondu pružná databáze pomocí portálu Azure | Microsoft Azure"
    description="Zjistěte, jak používat portál Azure a předdefinované intelligence databáze SQL spravovat, sledovat a vpravo velikost fond scalable pružná databáze můžete optimalizovat výkon databáze a spravovat náklady."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Sledování a správa fondu pružná databáze pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-manage-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Portál Azure slouží ke sledování a správa fondu pružná databází a databází ve fondu. Z portálu Microsoft můžete sledovat využití pružná fondu a databází v rámci tohoto fondu. Můžete taky tak, abyste sadu změny pružná fondu a odeslat všechny změny ve stejnou dobu. Tyto změny zahrnují přidáním nebo odebráním databází, změnou nastavení pružná fondu nebo změnou nastavení databáze.

Následující obrázek znázorňuje pružná fondu příklad. Zobrazení zahrnuje:

*  Grafy pro sledování využití prostředků fondu pružná a databází obsažených ve fondu.
*  Tlačítko **Konfigurace** fondu ke změnám fondu pružná.
*  Tlačítko **vytvořit databázi** , která vytvoří novou databázi a přidá ji do fondu aktuální pružná.
*  Pružná úlohy, které vám pomůžou Správa velkého množství databází spuštěním Transact SQL skriptů všechny databází v seznamu.

![Fond zobrazení][2]

Projděte kroky v tomto článku, musíte SQL server na Azure s nejméně jednu databázi a pružná fondu. Pokud nemáte pružná fondu, přečtěte si článek [Vytvoření fondu](sql-database-elastic-pool-create-portal.md); Pokud nemáte databáze, přečtěte si článek [Kurz databáze SQL](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Sledování pružná fondu

Můžete přejít na konkrétní fondu zobrazíte její využití prostředků. Ve výchozím nastavení fondu nakonfigurovaný tak, aby zobrazit využití úložiště a eDTU za poslední hodinu. Zobrazit různé metriky přes windows různé čas je možné konfigurovat grafu.

1. Vyberte fond pro práci s.
2. V části **Sledování pružná fondu** je graf s popisky **využití prostředků**. Klikněte na graf.

    ![Sledování pružná fondu][3]

    Otevře se zásuvné **metrické** a zobrazující podrobný přehled zadaný metriky přes určeného časového intervalu.   

    ![Metriky zásuvné][9]

### <a name="to-customize-the-chart-display"></a>Chcete-li přizpůsobit zobrazení grafu

Můžete upravovat grafu a metrických zásuvné zobrazíte jiné metriky například procesoru procento procento vstupu a výstupu dat a procento vstupu a výstupu protokolu použité.

2. V metrických zásuvné klikněte na **Upravit**.

    ![Klikněte na Upravit][6]

- V zásuvné **Upravit graf** vyberte novou oblast čas (za hodinu, dnes, nebo za týden) nebo klikněte na **vlastní** vyberte všechny rozsah kalendářních dat během posledních dvou týdnů. Vyberte typ grafu (pruhový nebo spojnicový) a potom vyberte zdroje, chcete-li sledovat.

> Poznámka: Pouze metriky s stejnou měrnou jednotku lze zobrazit v grafu ve stejnou dobu. Třeba při výběru možnosti "eDTU procentuální hodnotu" potom pouze bude možné vybrat jiné metriky s procentuální hodnotu jako měrné jednotky.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klikněte na **OK**.


## <a name="elastic-database-monitoring"></a>Sledování pružná databáze

Jednotlivé databáze můžete sledovat taky pro potenciální problémy.

1. V části **Sledování pružná databáze**je graf zobrazující metriky pro pět databáze. Ve výchozím nastavení grafu zobrazuje horní 5 databází ve fondu tak, že průměru eDTU použití za poslední hodinu. Klikněte na graf.

    ![Sledování pružná fondu][4]

2. Zobrazí se zásuvné **Využití prostředků databáze** . To obsahuje podrobný přehled použití databáze ve fondu. Pomocí mřížka v dolní části zásuvné, můžete vybrat libovolné databází ve fondu zobrazíte její použití v diagramu (až na 5 databází). Můžete taky přizpůsobit okna metriky a čas v grafu zobrazit kliknutím na **Upravit graf**.

    ![Databáze zásuvné využití prostředků][8]

### <a name="to-customize-the-view"></a>Chcete-li přizpůsobit zobrazení

1. V zásuvné **využití prostředků databáze** klikněte na **Upravit graf**.

    ![Klikněte na upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. V grafu zásuvné **Úpravy** vyberte nový časový rozsah (za hodinu nebo po 24 hodinách) nebo klikněte na **vlastní** vyberte jiný den během posledních 2 týdnů zobrazíte.

    ![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klikněte na rozevírací seznam **porovnání databází tak, že** chcete vybrat jiné metrické používat, když porovnání databází.

    ![Úprava grafu](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Chcete-li vybrat databází ke sledování

V seznamu databáze zásuvné **Využití prostředků databáze** můžete najít konkrétní databází Hledat na stránkách v seznamu nebo zadejte název databáze. Pomocí zaškrtávacího políčka vyberte databázi.

![Hledání pro databáze ke sledování][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Přidání upozornění do fondu zdrojů

Přidání pravidel na zdroje, které odesílají e-maily lidi nebo upozornění řetězců pro koncové body URL při využití prahovou hodnotu, která nastavíte narazí zdroje.

> [AZURE.IMPORTANT]Využití prostředků pro pružná fondů sledování má prodlevy nejméně 20 minut. Nastavení upozornění menší než 30 minut pro pružná fondů není aktuálně podporován. Upozornění na žádné nastavení pro pružná fondů tečkou (parametr s názvem "-velikost_okna" v prostředí PowerShell API) nemusí být menší než 30 minut spouštěný. Ujistěte se, že všechna oznámení, které definujete pro pružná fondy používat uplynutí (velikost_okna) zabere nejméně 30 minut.

**Chcete-li přidat upozornění pro všechny zdroje:**

1. Klikněte na **využití prostředků** graf otevřete zásuvné **míru** , klikněte na **Přidat upozornění**a potom vyplňte informace o v **Přidat pravidlo výstrahy** zásuvné (**zdroje** je automaticky nastavením fondu, se kterou pracujete s).
2. Zadejte **název** a **Popis** , který identifikuje upozornění pro všechny příjemce.
3. Zvolte **míru** , který chcete upozornit ze seznamu.

    Graf zobrazuje dynamické využití prostředků pro tuto míru snadněji zvolíte prahovou hodnotu.

4. Vyberte **podmínku** (větší než menší než atd) a **mezní hodnota**.
5. Klikněte na **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Přesunutí databáze do fondu pružná

Můžete přidat nebo odebrat existující fond databází. Databáze může být v fondy. Však možné přidávat jen databází, které jsou na stejném logické serveru.

1. V zásuvné fondu klikněte v části **pružná databází** **Konfigurovat fondu**.

    ![Klikněte na konfigurovat fondu][1]

2. V zásuvné **fondu konfigurovat** klikněte na **Přidat do fondu**.

    ![Klikněte na Přidat do fondu](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. V **databázích přidat** zásuvné vyberte databázi nebo databáze přidáte do fondu. Klikněte na **Vybrat**.

    ![Výběr databázích přidat](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Konfigurovat fondu** zásuvné teď seznamy databázi, kterou jste vybrali přidají se jeho stav nastaveno **na zjištění stavu úkolů**.

    ![Pole Čekání na fondu doplňky](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. V **zásuvné fondu konfigurovat**klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu pružná

1. V zásuvné **fondu konfigurace** vyberte databázi nebo databáze odebrat.

    ![zobrazení databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klikněte na **Odebrat z fondu**.

    ![zobrazení databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Konfigurovat fondu** zásuvné teď seznamy databázi, kterou jste vybrali odeberou se jeho stav nastaveno **na zjištění stavu úkolů**.

    ![Náhled databáze přidání a odebrání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. V **zásuvné fondu konfigurovat**klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Změna nastavení výkonu fondu

Jak sledovat využití zdrojů fondu můžete zjistit, že jsou třeba některé úpravy. Možná fondu musí změnit v rámci výkonu a úložiště. Možná budete chtít změnit nastavení databáze ve fondu. Můžete změnit nastavení fondu kdykoli získat nejlepší zůstatek výkon a náklady. V tématu [Při fondu pružná databáze bude použito?](sql-database-elastic-pool-guidance.md) Další informace.

**Chcete-li změnit limity eDTUs nebo úložiště na fondu a eDTUs na databázi:**

1. Otevřete zásuvné **fondu konfigurovat** .

    V části **Nastavení fondu pružná databázi**změnit nastavení fondu pomocí některého z posuvníku.

    ![Využití pružná fondu zdrojů](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Při změně nastavení jsou zobrazeny odhadovaných měsíční nákladů změnit.

    ![Aktualizace fondu a nové náklady na měsíční](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Vytváření a správa pružná úlohy

Pružná úlohy umožňují kontrolovat libovolný počet databází ve fondu skriptů Transact-SQL. Můžete vytvářet nové úlohy nebo spravovat existující úlohy na portálu.

![Vytváření a správa pružná úlohy][5]


Před použitím úlohy instalace součásti pružná úlohy a zadejte svoje přihlašovací údaje. Další informace najdete v tématu [Přehled úloh pružná databáze](sql-database-elastic-jobs-overview.md).

V tématu [měřítko, s databáze SQL Azure](sql-database-elastic-scale-introduction.md): umožňuje škálování, přesuňte data, pružná databázové nástroje dotazu nebo vytvořit transakce.



## <a name="additional-resources"></a>Další zdroje informací

- [Pružná fondu databáze SQL](sql-database-elastic-pool.md)
- [Vytvoření fondu pružná databáze pomocí portálu](sql-database-elastic-pool-create-csharp.md)
- [Vytvoření fondu pružná databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Vytvoření fondu pružná databáze s C#](sql-database-elastic-pool-create-csharp.md)
- [Ceny a výkonu aspektech pro fondy pružná databáze](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
