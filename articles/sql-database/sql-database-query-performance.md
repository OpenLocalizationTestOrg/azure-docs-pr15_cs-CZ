<properties 
   pageTitle="Přehled výkonu dotazu databáze Azure SQL" 
   description="Sledování výkonu dotazu označuje většina jinými procesoru dotazů pro databázi SQL Azure." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Přehled výkonu dotazu databáze Azure SQL


Správa a ladění výkonu relační databáze je náročný úkol, který vyžaduje značné znalosti a investice čas. Přehled výkonu dotazu umožňuje méně času Poradce při potížích s výkon databáze zadáním následujících akcí:

- Podrobnější využití prostředků (DTU) databáze. 
- Nejčastější dotazy podle procesoru/doba trvání/spuštění počet, která můžete potenciálně optimalizovaných pro zvýšení výkonu.
- Umožňuje přecházet na podrobnější informace v dotazu, zobrazit jeho historie využití prostředků a text. 
- Optimalizace poznámky, které ukazují akce provádí [Advisor databáze SQL Azure](sql-database-advisor.md) výkonu  



## <a name="prerequisites"></a>Zjistit předpoklady pro

- Přehled výkonu dotazu je dostupná jenom s V12 databáze SQL Azure.
- Přehled výkonu dotazu vyžaduje, aby [Úložiště dotazu](https://msdn.microsoft.com/library/dn817826.aspx) v databázi aktivní. Pokud není spuštěný dotaz úložiště, na portálu vás vyzve k zapnutí.

 
## <a name="permissions"></a>Oprávnění

Následující oprávnění [řízení přístupu na základě rolí](../active-directory/role-based-access-control-configure.md) nutné použít přehled výkonu dotazu: 

- **Čtečky**, **vlastníka**, **Přispěvatel**, **Přispěvatelů databáze SQL**nebo **SQL Server přispěvatelů** oprávnění jsou požadovaná můžete zobrazit nejvyšší zdroje náročný dotazů a grafy. 
- Oprávnění **vlastníka**, **Přispěvatel**, **Přispěvatelů databáze SQL**nebo **SQL Server přispěvatelů** vyžadovaných zobrazíte text dotazu.



## <a name="using-query-performance-insight"></a>Použití přehled výkonu dotazu

Přehled výkonu dotazu se snadno používá:

- Otevřete [portál Azure](https://portal.azure.com/) a najděte databázi, která chcete zkontrolovat. 
  - V levé nabídce podporu a odstraňování potíží vyberte "Přehled výkonu dotazu".
- Na kartě první prohlédněte seznam nejčastější dotazy využívající zdroje.
- Výběr jednotlivých dotazu zobrazíte její podrobnosti.
- Otevřete [Advisor databáze SQL Azure](sql-database-advisor.md) a zkontrolujte, jestli jsou všechna doporučení k dispozici.
- Pomocí jezdců nebo přiblížení ikony pozorovanými interval změnit.

    ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Několik hodin dat potřebuje zachytit úložiště dotazu pro databáze SQL poskytnout přehledy výkonu dotazu. Pokud databázi používá neaktivní nebo dotazu úložiště nebyla aktivní během určitého časového období, bude grafy prázdné při zobrazení této časové období. Pokud není spuštěný, můžete kdykoli povolit úložiště dotazu.   



## <a name="review-top-cpu-consuming-queries"></a>Kontrola horní procesoru jinými dotazů

Na [portálu](http://portal.azure.com) postupujte takto:

1. Přejděte k SQL databázi a klikněte na **všechna nastavení** > **podpory + Poradce při potížích** > **Přehled výkonu dotazu**. 

    ![Přehled výkonu dotazu][1]

    Otevře se zobrazení nejčastější dotazy a nejčastější dotazy náročný procesoru jsou uvedeny.

1. Klikněte na kolem grafu podrobnosti.<br>Horní řádek zobrazuje celkové DTU % pro databázi, zatímco pruhy zobrazit využití procesoru % využívané vybrané dotazy během vybraného intervalu (například pokud je vybraný **minulého týdne** každý pruh znázorňuje jeden den).

    ![Nejčastější dotazy][2]

    Mřížka dolní představuje souhrnné informace viditelné dotazy.

  - Dotaz ID – jedinečný identifikátor dotazu v databázi.
  - Procesor na dotaz během pozorovatelným intervalu (závisí na tom, funkce agregace).
  - Doba trvání jednoho dotazu (závisí na tom, funkce agregace).
  - Celkový počet spuštění pro konkrétní dotazu.

    Zaškrtněte nebo zrušte jednotlivé dotazy zahrnout nebo vyloučit z grafu pomocí zaškrtávacích políček.

1. Pokud zastaralá data, klikněte na tlačítko **Aktualizovat** .
1. Pomocí jezdců a lupy tlačítka pro změnu pozorování interval a prošetřit vrcholy pole:  ![nastavení](./media/sql-database-query-performance/zoom.png)
1. Volitelně můžete podle potřeby jiné zobrazení můžete vybrat kartu **vlastní** a nastavení:
  
  - Metriky (procesor, doba trvání, počet spuštění)
  - Časový interval (poslední 24 hodin, za týden, minulý měsíc). 
  - Počet dotazů.
  - Funkce agregace.

    ![nastavení](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Zobrazení podrobností jednotlivé dotazy

Chcete-li zobrazit podrobnosti o dotazu:

1. Klikněte na libovolnou dotaz v seznamu Nejčastější dotazy.

    ![Podrobnosti](./media/sql-database-query-performance/details.png)

1. Zobrazení podrobností o otevře a počet dotazů procesoru spotřebu/doba trvání/spuštění prezentovaná v čase.
1. Klikněte na kolem grafu podrobnosti.
  - Začátek graf zobrazuje spojnicový graf se celkové databáze DTU % a pruhy jsou využití procesoru % využívané vybraný dotaz.
  - Druhý diagram znázorňuje celkovou dobou trvání za vybraný dotaz.
  - Dolní graf zobrazuje celkový počet spuštění tak, že vybraný dotaz.
    
    ![Podrobnosti o dotazu][3]

1. Volitelně můžete pomocí jezdců tlačítka lupy a klikněte na **Nastavení** můžete přizpůsobit způsob zobrazení dat dotazu a vyberte jiné časové období.

## <a name="review-top-queries-per-duration"></a>Revize nejčastější dotazy na doba trvání

V poslední aktualizaci přehled výkonu dotazu jsme zavádí dva nové metriky, které vám může pomoct zjistit potenciálních problémů: dobu trvání a spuštění count.<br>

Časově náročné dotazy mají největší potenciální blokování zdrojů delší blokování ostatní uživatelé a omezení škálovatelnost. Jsou taky nejlepší kandidáty pro optimalizaci.<br>

Určení dlouhodobé dotazů:

1. Otevřete kartu **vlastní** v přehled výkonu dotazu pro vybrané databáze
1. Změna metriky je **Doba trvání**
1. Vyberte počet dotazů a pozorování interval
1. Vyberte agregační funkce
  - **SUMA** Sečte všechna čas spuštění dotazu během celé pozorování intervalu.
  - **Max** najde dotazy, které spuštění čas maximální intervalech celé pozorování.
  - **Avg** najde průměr čas spuštění všech spuštění dotazu a zobrazit horní mimo tyto průměry. 

    ![Doba trvání dotazu][4]

## <a name="review-top-queries-per-execution-count"></a>Revize nejčastější dotazy na počet spuštění

Velké množství spuštění nemusí být došlo k ovlivnění samotné databáze může být nízké využití prostředků, ale může se zobrazit celkové aplikace pomalé.

V některých případech může vést počet velmi vysoké spuštění zvětšíte ze sítě výměny zpráv. Zaokrouhlit cest významně ovlivnit výkon. Jsou vyměřené poplatky za jeho latence sítě a podřízenými latence. 

Například mnoha založených na datech webů velkém přístup k databázi pro každý požadavek uživatele. Při připojení fond pomáhá, lepší v síti a zpracování zatížení na databázovém serveru mohou nepříznivě ovlivnit výkon.  Obecné pokyny má mít výměny zpráv na absolutní minimum.

Jak identifikovat často spouštět dotazy ("chatty") dotazů:

1. Otevřete kartu **vlastní** v přehled výkonu dotazu pro vybrané databáze
1. Změna metriky jako **počet spuštění**
1. Vyberte počet dotazů a pozorování interval

    ![počet spuštění dotazu][5]

## <a name="understanding-performance-tuning-annotations"></a>Principy poznámky ladění výkonu 

Při prohlížení vašeho pracovního vytížení v přehled výkonu dotazu, můžete narazit na ikony s svislou čáru horní části grafu.<br>

Tyto ikony jsou poznámky; představují výkonu došlo k ovlivnění akce provádí [Advisor databáze SQL Azure](sql-database-advisor.md). Ukázání poznámky můžete získat základní informace o akci:

![pořizování poznámek dotazu][6]

Pokud budete chtít vědět víc nebo použití advisor doporučení, klikněte na ikonu. Otevře se podrobnosti akce. Pokud je aktivní doporučení jejím použití zase přímo pryč pomocí příkazu.

![Podrobnosti o poznámku dotazu][7]

### <a name="multiple-annotations"></a>Víc poznámek. ###

Je možné, že z důvodu úroveň přiblížení bude získat poznámky, které se blíží vzájemně sbalit do jedné. To bude představované speciální ikonu, kliknutím ho otevřít nový zásuvné kde seznam seskupené poznámky se zobrazí.
Srovnávací dotazů a akce ladění výkonu umožňují lepší přehled o vaší pracovní zátěž. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Optimalizace konfigurace dotazu úložiště pro přehled výkonu dotazu

Při použití přehled výkonu dotazu můžete narazit na následující dotaz ukládání zpráv:

- "Store dotaz není správně nakonfigurovaný na tuto databázi. Kliknutím sem zobrazíte další".
- "Store dotaz není správně nakonfigurovaný na tuto databázi. Kliknutím sem můžete změnit nastavení." 

Tyto zprávy obvykle zobrazují při úložiště dotazu nedokáže shromažďovat nová data. 

První písmena se stane, když dotazu úložiště je jen pro čtení a parametrů jsou nastavené optimální. Problém můžete vyřešit zvětšení velikosti úložiště dotazu nebo zrušením zaškrtnutí úložiště dotazu.

![tlačítko qds][8]

Druhá případ se stane, když dotazu úložiště přihlašovacích údajů je normálně vypnuté nebo nejsou optimálně nastavit parametry. <br>Můžete změnit zásady uchovávání informací a zachycení a povolit úložiště dotazů spouštěním následujících příkazů nebo přímo z portálu:

![tlačítko qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Doporučené zásady uchovávání informací a snímek

Existují dva typy zásad uchovávání informací:

- Velikost – Pokud nastavit automatické ho vyčistí dat automaticky při poblíž maximální velikosti.
- Na základě času - ve výchozím nastavení, které jsme nastaví na 30 dní, což znamená, pokud dotaz úložiště bude docházet prostor, odstraní dotaz informace starší než 30 dní.

Zachycení zásady může být nastaven na:

- **Vše** – zaznamenává všechny dotazy.
- **Automatické** – časté dotazy a s dobou trvání nadbytečných kompilace a spuštění jsou ignorovány. Mezní hodnoty celou dobu spuštění počet, kompilace a modulu runtime interně jsou určeny. Toto je výchozí možnost.
- **Žádná** – úložiště dotazu se zastaví zachycení nové dotazy však stat runtime pro už zachycený dotazy se pořád odebírají.
    
Doporučujeme nastavení všech zásad automatického a vyčistit zásadu 30 dní:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Zvětšení velikosti úložiště dotazu. To může vykonat připojení k databázi a vydání následující dotaz:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Použití nastavení nakonec, aby dotaz úložiště získávání nových dotazech, ale pokud nechcete, aby se má čekat, můžete je vymazat úložiště dotazu. 
> [AZURE.NOTE] Provádění následujících dotazu odstraníte všechny aktuální informace v úložišti dotazu. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Souhrn

Přehled výkonu dotazu pomůže pochopit dopad pracovní zátěž dotazu a vztah k využití prostředků databáze. Pomocí této funkce informace o horní jinými dotazů a snadno identifikovat z nich oprava před bude se jednat o problém.




## <a name="next-steps"></a>Další kroky

Další doporučení týkající se zlepšení výkonu SQL databázi klikněte na [doporučení](sql-database-advisor.md) na zásuvné **Přehled výkonu dotazu** .

![Konference Advisor výkonu](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

