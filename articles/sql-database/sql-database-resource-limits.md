<properties
    pageTitle="Limity prostředků databáze Azure SQL"
    description="Tato stránka popisuje některé běžné omezení prostředků pro databázi SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Omezení zdrojů Azure SQL databáze

## <a name="overview"></a>Základní informace

Databáze SQL Azure spravuje prostředky k dispozici pro databázi pomocí dva různé mechanismy: **Zásady správného řízení zdrojů** a **Prosazování omezení**. Toto téma vysvětluje těchto dvou hlavních oblastí systému řízení zdrojů.

## <a name="resource-governance"></a>Zásady správného řízení zdrojů
Jednou z cíle návrhu úrovní služby základní, Standard a Premium je databáze SQL Azure chovat jako kdybyste databázi běží ve vlastním počítači úplně samostatný z jiné databáze. Zásady správného řízení zdrojů napodobuje toto chování. Pokud využití prostředků agregované dosáhne maximální využití procesoru k dispozici, pamětí, v/v protokolu a dat v/v zdroje přiřazené k databázi, zásady správného řízení zdrojů fronty dotazů v spuštění a přiřazení zdrojů k ve frontě dotazů, jak uvolnit tak.

Jako vyhrazené počítače využití všechny dostupné zdroje povede delší provádění aktuálně provádění dotazů, které můžou mít za následek časové limity příkaz v klientském počítači. S logika agresivní opakování a aplikacemi, které spouštění dotazů databáze s vysokou četnost se chybové zprávy při pokusu o spuštění nové dotazy dosáhne limit souběžně prováděné žádosti.

### <a name="recommendations"></a>Doporučení:
Sledujte využití prostředků, jakož i doby odezvy průměr dotazů při blíží maximální využití možností databáze. Při výskytu vyšší čekacích dob dotazu obecně máte tři možnosti:

1.  Zkraťte příchozí žádosti k databázi pro ochranu časový limit a relačních operátorů žádosti.

2.  Vyšší výkon přiřadíte databáze.

3.  Optimalizace dotazy ke snížení využití prostředků z obou dotazů. Další informace naleznete v části dotazu optimalizace/Hinting v článku pokyny výkonu databáze SQL Azure.

## <a name="enforcement-of-limits"></a>Vynucení omezení
Zdroje než procesoru, paměti, v/v protokolu a Data v/v vynucuje odepření nových žádostí o při dosažení mezní hodnoty. Klienti, zobrazí se [chybová zpráva](sql-database-develop-error-messages.md) podle toho, což překročení maximální počet.

Počet připojení k databázi SQL i počet souběžně prováděné žádosti, které lze zpracovat jsou například s omezeným přístupem. Databáze SQL umožňuje počet připojení k databázi být větší než počet souběžně prováděné žádosti na podporu fond připojení. Částka připojení, které jsou k dispozici můžete snadno řídil aplikace, množství souběžné žádosti je často časy těžší k odhadu a řídit. Zejména během načítání Špička prodloužit aplikace buď odešle příliš mnoho požadavků nebo databáze dosáhne jeho zdroje omezuje kdy a kdy začíná kumulování pracovních podprocesů kvůli delší pracovního dotazů, můžete být zjištěny chyby.

## <a name="service-tiers-and-performance-levels"></a>Služba úrovní a výkonu úrovně

Mívají – úrovně služeb úrovně výkonu pro obě samostatného databáze a ohebné fondů.

### <a name="standalone-databases"></a>Samostatný databází

Pro danou databázi samostatného limity databáze jsou definovány úroveň osy a výkonu databáze služby. Následující tabulka popisuje charakteristických vlastností základní standardní a Premium databázích různé úrovně výkonu.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Pružná fondů

[Pružná fondů](sql-database-elastic-pool.md) sdílet zdroje přes databází ve fondu. Následující tabulka popisuje charakteristických vlastností Basic, Standard a Premium fondů pružná databáze.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Rozbalené definice jednotlivé zdroje uvedené v předchozí tabulce v popisech v [služby osy funkce a omezení](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Základní informace ze služby úrovní najdete v článku [Azure SQL databáze služby úrovní a výkonu úrovně](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Ostatní limity SQL databáze

| Oblast | Limit | Popis |
|---|---|---|
| Databáze pomocí funkce automatického exportovat jedno předplatné | 10 | Automatické export umožňuje vytvářet vlastní plán pro zálohování databáze SQL. Další informace najdete v tématu [SQL databáze: podpora pro automatické exporty databáze SQL](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Databáze na serveru | Až 5 000 | Až 5000 databáze jsou povolených pro jednoho serveru na serverech V12. |  
| DTUs na server | 45000 | 45000 DTUs jsou dostupné na serveru na serverech V12 pružná fondů i dat sklady zřizovací databází. |



## <a name="resources"></a>Zdroje informací

[Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md)

[Úrovně služby Azure SQL databáze a výkonu úrovně](sql-database-service-tiers.md)

[Chybové zprávy pro databáze SQL klientských programů](sql-database-develop-error-messages.md)
