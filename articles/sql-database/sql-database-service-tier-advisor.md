<properties 
   pageTitle="Ceny osy doporučení pro databázi SQL Azure" 
   description="Při změně ceny úrovní Azure portálu ceny osy doporučení jsou za předpokladu, že doporučená osy, který je nejlepší vhodný pro spuštění pracovního vytížení existující Azure SQL databáze společnosti. Ceny úrovní popisují služby osy a výkonu úrovně databázi SQL." 
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
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>Databáze SQL ceny osy doporučení

 Ceny doporučení osy byste služby osy a výkonu úroveň, která je nejvhodnější pro spuštění pracovního vytížení existující databáze Azure SQL.

> [AZURE.NOTE] Ceny doporučení osy se jenom k dispozici pro Web a Business databází a pružná databáze fondů – k dispozici pouze v [Azure portálu](https://portal.azure.com/).


Získání ceny osy doporučení během tyto věci:

- [Změna služby osy a výkonu úrovně (cena za osy) databáze SQL](sql-database-scale-up.md)
- [Upgrade server Azure SQL V12](sql-database-upgrade-server-portal.md)
- Přejděte na serveru V12. V tématu [databáze SQL ceny doporučení osy](sql-database-service-tier-advisor.md).
- [Vytvoření fondu pružná databáze](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Základní informace

Služba SQL databáze analyzuje aktuální výkon a funkce požadavky podle hodnocení používání historických zdrojů pro databázi SQL. Kromě toho vrstvy minimální přijatelné služeb určí se v závislosti na velikosti databáze a povolené [nepřerušený](sql-database-business-continuity.md) funkcí. 

Analyzovat tyto informace a služby osy a výkonu úroveň, která je nejlepší vhodná pro spuštění databáze typické pracovního vytížení a zachovat, že je aktuální sadu funkcí se doporučuje.

- Služba kontroluje předchozí 15 až 30 dní historických dat (využití prostředků velikost databáze a aktivita v databázi) a provede srovnání množství prostředků spotřebované množství a skutečné omezení úrovní momentálně neexistuje služby a výkonu úrovně.
- Analyzovat data v 15 druhý intervalů a každý interval sadě zařadit do existující vrstvy služeb a úroveň výkonu, je nejlepší vhodné pro zpracování pracovního vytížení této sadě.
- Tyto 15 druhé ukázky potom seskupená do větších analýzy 15-30 den a doporučuje se service osy a výkonu úroveň můžete efektivně zpracovat 95 % historických pracovní zátěž.

### <a name="recommendations"></a>Doporučení

Na základě vaší databáze použití, existují aktuálně 2 kategorie doporučení, která může dojít:


| Doporučení | Popis |
| :--- | :--- |
| Upgrade | Upgrade na nové osy. |
| Není k dispozici | Databáze vyžaduje minimální pracovní zátěž nebo přibližně 35 dní aktivity. Není dost data zadání platná doporučení. |

## <a name="getting-pricing-tier-recommendations"></a>Získání ceny osy doporučení

Získání ceny osy doporučení tak, že vyberete existující Web nebo Business databáze, **všechna nastavení**klepněte **ceny osy (měřítko DTUs)**. (Ceny doporučení vrstvy jsou dostupné taky kdy můžete [upgradovat Azure SQL server a V12](sql-database-upgrade-server-portal.md).)

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet** > **SQL databáze**.
4. V **SQL databáze** zásuvné klikněte na databázi, která chcete zobrazit doporučení pro:

    ![Vyberte databázi][1]

5. Na zásuvné databáze vyberte **všechna nastavení** a vyberte **ceny osy (měřítko DTUs)**.


7. **Doporučené ceny úrovní** otevřete kde můžete klikněte na navrhovaná osy a potom klikněte na tlačítko **Vybrat** přejdete k této osy.

    ![Registrace k náhledu][4]

8. Pokud chcete klikněte na **zobrazení podrobností o použití** otevřete zásuvné **Ceny osy doporučení podrobnosti** , kde si můžete pustit doporučené vrstvy pro databázi, porovnání funkcí mezi aktuálním a doporučené úrovní a graf analýzy využití historických zdroje.

    ![Registrace k náhledu][5]



## <a name="summary"></a>Souhrn

Ceny doporučení osy zajistíte automatické možnosti pro shromažďování telemetrickými daty pro každou databázi SQL a doporučující nejlepší služby osy/výkonu úrovně kombinace podle potřeby skutečný výkon a funkce požadavky do databáze. Na zásuvné nastavení kliknutím zobrazíte ceny osy doporučení pro Web a Business databáze **ceny osy (měřítko DTUs)** .



## <a name="next-steps"></a>Další kroky

V závislosti na podrobné informace o konkrétní databázi obvykle provádějící upgrade nebo přechodu nedojde z toho důvodu okamžitě. Na portálu poskytne oznámení jako přechody databáze do nové osy nebo stavu upgradu můžete sledovat pomocí dotazu [sys.dm_operation_status (databáze SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx) zobrazení předlohy databáze SQL serveru databáze.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
