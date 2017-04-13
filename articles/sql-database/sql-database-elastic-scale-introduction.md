<properties
    pageTitle="Rozšiřování s databáze SQL Azure | Microsoft Azure"
    description="Service (SaaS) vývojáři softwaru, mohli snadno vytvářet pružná, scalable databází v cloudu pomocí těchto nástrojích"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Rozšiřování s databáze SQL Azure

Můžete snadno rozšiřování databáze Azure SQL pomocí nástrojů **Pružná databáze** . Tyto nástroje a funkce umožňují prostředky neomezený databáze databáze **SQL Azure** použít k vytvoření řešení pro transakční pracovního vytížení a Software zejména jako aplikací služby (SaaS). Pružná databázové funkce se skládá z následujících akcí:

* [Knihovna klienta pružná databáze](sql-database-elastic-database-client-library.md): Knihovna klienta je funkce, která umožňuje vytvářet a spravovat sharded databází.  V tématu [Začínáme s pružná databázové nástroje](sql-database-elastic-scale-get-started.md).
* [Nástroj rozdělit sloučení pružná databáze](sql-database-elastic-scale-overview-split-and-merge.md): Přesun dat mezi sharded databází. To je užitečné pro přesunutí dat z databáze více klienta do jednoho tenanta databáze (nebo naopak). Viz [pružná databáze kurz nástrojů korespondence rozdělení](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Pružná databáze projektů](sql-database-elastic-jobs-overview.md) (verze preview): Správa velkého množství databáze Azure SQL pomocí úloh. Snadno provádí operace pro správu ATP změny schématu, Správa přihlašovacích údajů, aktualizaci dat odkazu, shromažďování dat výkonu kolekci telemetrie klienta (zákazník) pomocí úloh.
* [Pružná databázových dotazů](sql-database-elastic-query-overview.md) (verze preview): umožňuje spuštění dotazu jazyce Transact-SQL, který přesahuje více databází. Díky připojení k nástroje pro vytváření sestav třeba Excel PowerBI, Tableau, atd.
* [Pružná transakce](sql-database-elastic-transactions-overview.md): Tato funkce umožňuje spuštění transakce přecházející několik databází v databázi SQL Azure. Pružná databázové transakce jsou k dispozici pro .NET aplikace pomocí ADO .NET a integrace s známé programovací prostředí pomocí [System.Transaction třídy](https://msdn.microsoft.com/library/system.transactions.aspx).

Následující obrázek znázorňuje architektura, která obsahuje **pružná databázové funkce** vzhledem k kolekce databází.

V tomto obrázku databáze se zobrazují barvy schémat. Databáze se stejnou barvu sdílet na stejné schéma.

1. Nastavení **databáze Azure SQL** hostitelem Azure pomocí sharding architektury.
2. **Knihovna klienta pružná databáze** slouží ke správě shard sadu.
3. Podmnožinu databáze se budou zobrazovat do **fondu pružná databáze**. (Viz [Co je fond?](sql-database-elastic-pool.md)).
4. **Pružná databáze úlohy** běží plánované nebo ad-hoc T-SQL skripty proti všechny databáze.
5. **Nástroj rozdělit sloučení** slouží k přesunutí dat z jedné shard.
6. **Pružná databázových dotazů** umožňuje vytvořit dotaz, který přesahuje všechny databáze v sadě shard.
7. **Pružná transakce** umožňuje spuštění transakce přecházející několik databází. 


![Pružná databázové nástroje][1]


## <a name="why-use-the-tools"></a>Proč používat nástroje?

Dosažení pružnost a měřítko pro cloudové aplikace byl jednoduchých VMs a úložiště objektů blob – jednoduše přidat odčítání časových nebo zvýšit výkon. Ale zůstal výzvu pro stavové zpracování data v relační databáze. Problémy při vedou těchto situací:

* Kvetoucí a zmenšením kapacitu pro relační databáze součástí vašeho pracovního vytížení.
* Správa aktivní oblasti, které může nastat došlo k ovlivnění dílčí sadu dat – například zejména zaneprázdněné konec zákazníkem (klient).

Obvykle scénáře tyto mít byla kanceláři řešili investici větší databázový server pro podporu aplikace. Tuto možnost je však omezený v cloudu, kde všechny zpracování se stane, na předdefinované komoditou hardwaru. Místo toho distribuci dat a zpracování přes mnoho stejně strukturou databáze (označované jako "sharding" vzorek škálování) obsahuje alternativy tradiční přístupy měřítko zdola nahoru, jak z hlediska nákladů a pružnost.

## <a name="horizontal-and-vertical-scaling"></a>Změna měřítka vodorovné a svislé

Následující obrázek ukazuje vodorovné a svislé rozměry měřítko, které jsou základní způsobů, jak můžete diagramů s měřítky pružná databází.

![Vodorovné a svislé Scaleout][2]

Změna měřítka vodorovné odkazuje na přidávání a odebírání databází upravit kapacitu nebo celkový výkon. To je také vyznačenými "měřítka". Sharding, ve kterém je oddíly dat k dokumentu mezi skupinu stejně strukturovaných databází, je běžným způsobem implementovat vodorovné měřítko.  

Změna měřítka svislé odkazuje na zvýšení nebo snížení úrovně výkonu jednotlivé databáze –: někdy označované jako "škálování."

Většina cloudu měřítko databázové aplikace použije kombinace těchto dvou strategií. Například Software jako aplikace služby může pomocí vodorovné měřítko zřízení nového konce zákazníci a měřítka svislé aby zvětšit nebo zmenšit zdrojů v případě potřeby tak, že pracovní zátěž databázi každého konec zákazníka.

* Změna měřítka vodorovné spravují pomocí [knihovny klienta pružná databáze](sql-database-elastic-database-client-library.md).

* Změna měřítka svislé lze provést pomocí rutin prostředí PowerShell Azure přejděte vrstva služby nebo tak, že zaškrtnete databází ve fondu pružná.

## <a name="sharding"></a>Sharding

*Sharding* je postup k distribuci velké objemy dat stejně jako strukturovaný přes počet nezávislých databází. Je zejména Oblíbené s cloudu vývojáři vytváření softwaru jako nabízené služby (SAAS) pro ukončení zákazníci nebo firmy. Tyto koncovým zákazníkům jsou někdy označovány jako "klienti". Sharding může být nutné pro libovolný počet důvodů:  

* Celková částka dat je příliš velký tak, aby odpovídaly omezení jednu databázi
* Výkon transakce celkové pracovní zátěž přesáhne možnosti jednu databázi
* Klienti může vyžadovat fyzické izolace nezávisle, takže samostatné databáze jsou potřebné pro každého klienta
* Různé části databáze může být nutné nacházejí v různých zeměpisných pro dodržování předpisů, výkon nebo geopolitické důvodů.

V ostatních případech, například požití dat z distribuované zařízení sharding mohou sloužit k vyplnění sadu databází, které jsou organizovány dočasně. Například samostatnou databázi můžete se snaží o každý den nebo týden. V takovém případě klávesu sharding může být celé číslo představující data (prezentovat ve všech řádcích sharded tabulkami) a dotazy získávání informací pro rozsah kalendářních dat musí být směrovány aplikací podsady databází překrývajícím konkrétní oblast.

Sharding funguje nejlépe, když každé transakce v aplikaci lze omezit na jednu hodnotu sharding klíče. Který zajišťuje, aby všechny transakce bude místní databáze.

## <a name="multi-tenant-and-single-tenant"></a>Více klienta a jednoho klienta

Některé aplikace používají Nejjednodušeji vytváření samostatnou databázi pro každého klienta. To je **jednoho tenanta sharding vzorku** , který poskytuje izolace, zálohování a obnovení možnost a zdroje měřítko na granularity klienta. Pomocí jednoho tenanta sharding každou databázi je přidružené ke konkrétní klienta hodnotu ID (nebo hodnoty klíče zákazníka), ale tento klíč nemusí být vždy účastní samotná data. Odpovídá aplikace pro směrování každý požadavek na příslušnou databázi – a knihovně klienta můžete zjednodušit takto.

![Jednoho klienta a více klienta][4]

Ostatní víc klientů scénáře společně pack do databáze, spíše než izolace do samostatných databází. Toto je typické **více klienta sharding vzorek** – a může být řízený úsilím tím, že aplikace spravuje velkého množství příliš malý klienti. Ve více klienta sharding řádky v tabulkách všechny slouží k provádění sharding klávesy nebo klíč, který identifikuje ID klienta. Znovu vrstvy aplikace je zodpovědný za účelem směrování ke klientovi požadavek na příslušnou databázi a to plánuje podpora klienta knihovnou pružná databáze. Navíc řádku úrovně zabezpečení mohou sloužit k filtrování řádků každého klienta dostanete – Další informace najdete v tématu [více klienta aplikací s pružná databázové nástroje a zabezpečení na úrovni řádku](sql-database-elastic-tools-multi-tenant-row-level-security.md). Redistribuci dat mezi databází můžou být potřeba vzorkem sharding více klienta a tím usnadnit nástrojem rozdělit sloučení pružná databáze. Další informace o provedeních SaaS aplikací pomocí pružná fondů, najdete v článku [Provedeních více klienta SaaS aplikací se databáze SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Přesuňte data z více nájmu jednou databází

Při vytváření aplikace SaaS, je typické nabízet potenciální zákazníci zkušební verzi softwaru. V tomto případě je efektivní používání databáze více klienta pro data. Jakmile potenciálního zákazníka zákazníka, databázi jednoho klienta je však lepší od poskytuje lepší výkon. Pokud zákazník měli vytvořili dat během zkušební období, použijte [nástroj rozdělit sloučit](sql-database-elastic-scale-overview-split-and-merge.md) přesuňte data z více klienta pro novou databázi jednoho klienta.

## <a name="next-steps"></a>Další kroky

Ukázka aplikace, které ukazuje knihovnu klienta najdete v článku [Začínáme s pružná Datababase nástroje](sql-database-elastic-scale-get-started.md).

Pokud chcete převést existující databáze pomocí nástrojů, najdete v článku [migrace existující databáze do škálování](sql-database-elastic-convert-to-use-elastic-tools.md).

Zobrazíte konkrétní fondu pružná databáze v tématu [aspektech cena a výkonu pro fond pružná databáze](sql-database-elastic-pool-guidance.md)nebo vytvoření nového fondu s [kurz](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

