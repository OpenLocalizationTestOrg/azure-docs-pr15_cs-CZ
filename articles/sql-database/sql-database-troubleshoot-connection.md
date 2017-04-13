<properties
    pageTitle="Databáze na serveru není momentálně neexistuje, připojte k SQL databázi | Microsoft Azure"
    description="Poradce při potížích s databáze na serveru není momentálně neexistuje chyby při aplikace připojení k databázi SQL."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="databáze na serveru není momentálně neexistuje, připojovat se k databázi sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Chyba "Databáze na serveru není momentálně neexistuje" při připojení k databázi sql
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Aplikace se připojí k databázi Azure SQL, zobrazí tato chybová zpráva:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Tato chybová zpráva se obvykle přechodná (krátkodobý).

K této chybě dochází, když Azure databáze, která má být přesunutý (nebo překonfigurovat) a aplikace ztratí připojení k databázi SQL. Konfigurace události v SQL databázi příčinou plánované události (například upgrade softwaru) nebo neplánovanou událost (třeba pád proces, nebo Vyrovnávání zatížení). Většina konfigurace událostí se obecně krátkodobý a by měl být vyplnit 60 sekund maximálně. Však tyto události může občas trvat déle, například kdy velké transakce způsobí obnovení dlouho probíhajících.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Postup, jak vyřešit problémy s připojením přechodná
1.  Zaškrtněte políčko [Řídicí panel služeb Microsoft Azure](https://azure.microsoft.com/status) pro známé výpadků, ke kterým došlo v době, kdy byly oznámeny chyby aplikací.
2. Aplikace, která se připojují do cloudové služby, jako jsou databáze SQL Azure mají očekávat pravidelných konfigurace událostí a implementace opakovat logiky pro zpracování tyto chyby místo povrchové jako chyby aplikací pro uživatele. Prohlédněte si v části [přechodové chyby](sql-database-connectivity-issues.md) a doporučené postupy a návrhu pokyny v [SQL databázi vývoj přehled](sql-database-develop-overview.md) Další informace a obecné opakovat strategie. Pak v tématu ukázky u [Knihovny připojení databáze SQL a SQL Server](sql-database-libraries.md) pro zvláštnosti.
3.  Jakmile databáze přiblíží omezení prostředků, ho můžete zdá problém přechodná připojení. V tématu [odstraňování potíží s výkonem](sql-database-troubleshoot-performance.md).
4.  Pokud problémy s připojením k pokračovat, doba trvání, pro kterou aplikace dojde k chybě větší než 60 sekund nebo Pokud naopak uvidíte několika opakováními chyby do schránky v daný den, souborů žádost Azure podpory tak, že vyberete **Získat podporu** na web [Podpory Azure](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Další kroky
- Pokud se zobrazí na různých chybu, jsou vyhodnoceny [chybová zpráva](sql-database-develop-error-messages.md) pro vodítek o možné příčině.
- Pokud problém přetrvává, navštěvujte blog o pokyny v [Poradce při potížích s běžné problémy s připojením k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md).
