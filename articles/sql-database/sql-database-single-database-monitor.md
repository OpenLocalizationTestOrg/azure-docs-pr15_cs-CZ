<properties
    pageTitle="Sledování výkonu databáze v databázi SQL Azure | Microsoft Azure"
    description="Informace o možnostech sledování databáze pomocí nástroje pro Azure a Správa dynamických zobrazení."
    keywords="databáze sledování výkonu databáze cloudu"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Sledování výkonu databáze v databázi SQL Azure
Sledování výkonu databázi SQL Azure začíná sledování využití prostředků relativní úrovně výkonu databáze, které zvolíte. Sledování se zanalyzovat databáze má nadbytečné kapacitu zda jsou potíže, proto, že zdroje jsou maxed a potom rozhodněte, jestli je potřeba upravit úroveň výkonu a [vrstvy služeb](sql-database-service-tiers.md) databáze. Můžete sledovat databázi pomocí nástroje pro grafické [Azure portál](https://portal.azure.com) nebo [Správa dynamických zobrazení](https://msdn.microsoft.com/library/ms188754.aspx)SQL.

## <a name="monitor-databases-using-the-azure-portal"></a>Sledování databáze pomocí portálu Azure

[Azure portál](https://portal.azure.com/)můžete sledovat jednu databázi využití výběrem databáze a kliknutím na graf **Sledování** . Tím se vyvolá **míru** okno, ve kterém můžete změnit kliknutím na tlačítko **Upravit graf** . Přidejte následující metriky:

- Procento využití procesoru
- Procento DTU
- Procento vstupu a výstupu dat
- Velikost databáze v procentech

Po přidání těchto metriky, můžete dál tyto informace zobrazit v grafu **Sledování** s další informace o okně **míru** . Všechny čtyři metriky zobrazí procento využití průměr relativní **DTU** databáze. Naleznete v článku [– úrovně služeb](sql-database-service-tiers.md) podrobnosti o DTUs.

![Služba osy sledování výkonu databáze.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Můžete taky nakonfigurovat upozornění metrice výkonu. Klikněte na tlačítko **Přidat upozornění** v okně **metrické** . Postupujte podle pokynů Průvodce pro nastavení vaší upozornění. Máte možnost upozornění Pokud metriky překročí určitou prahovou hodnotu nebo metriky spadá pod určitou prahovou hodnotu.

Pokud se myslíte, že pracovní zátěž na databázi rozšířit, je možné nakonfigurovat e-mailovém upozornění vždy, když databázi dosáhne 80 % na kterékoli z metriky výkonu. Můžete to jako nejdříve možné upozornění k určení prodejce, budete muset přepnout na vyšší úrovni vyšší výkon.

Metriky výkonu také vám můžou pomoct zjistit, zda je možné přejít nižší výkon. Předpokládejme používáte standardní S2 databázi a měřítka všechny zobrazit, že databázi průměrně nepoužívejte víc než 10 % v daném okamžiku. Je pravděpodobné, že databázi budou fungovat i v standardní S1. Ale znát úloh, které dosahuje nebo kolísání před rozhodování přejdete k nižší výkon.

## <a name="monitor-databases-using-dmvs"></a>Sledování databáze využívající DMVs

Stejné metriky, která jsou na portálu jsou k dispozici prostřednictvím zobrazení systém také: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) logické **hlavní** databáze serveru a [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) v databázi uživatelů. Pokud potřebujete ke sledování méně podrobného dat přes delší dobu, použijte **sys.resource_stats** . Pokud potřebujete ke sledování podrobnější data v rámci menší časové rozmezí použijte **sys.dm_db_resource_stats** . Další informace najdete v tématu [Pokyny výkonu databáze SQL Azure](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **Sys.dm_db_resource_stats** vrátí prázdný výsledek nastavit při použití v Web a Business edition databází, které jsou už nepoužívá.

Pro fondy pružná databáze můžete sledovat jednotlivé databází ve fondu s postupy popsaná v této části. Ale taky můžete sledovat fondu jako celek. Informace najdete v tématu [sledování a správa fondu pružná databáze](sql-database-elastic-pool-manage-portal.md).
