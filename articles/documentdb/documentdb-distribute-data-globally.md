<properties
   pageTitle="Distribuovat data globálně s DocumentDB | Microsoft Azure"
   description="Informace o obnovení geo replikace, převzetí a data planet měřítko globální databází z Azure DocumentDB plně spravované NoSQL databáze služby."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Distribuovat data globálně s DocumentDB

> [AZURE.NOTE] Globální rozdělení databáze DocumentDB je všeobecně dostupná a automaticky povoleno pro všechny nově vytvořený DocumentDB účty. Pracujeme na povolení globálního distribuce pro všechny existující účty, ale mezitím požadovaná globální distribuční povolili na váš účet, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a jsme budete povolte ho můžete nyní.

Azure DocumentDB slouží potřebám IoT aplikací tvořené miliony globálně distribuované zařízení a aplikace internet měřítko, které uživatelům poskytovat vysoce citlivé prostředí po celém světě. Tyto databáze systémy obličej ověřovací kód dosáhnout s nízkou latencí přístup k datům aplikace z více zeměpisné oblasti s záruky konzistenci a dostupnosti dobře definovaný data. Jako globálně distribuované databáze systému DocumentDB nabízející plně spravovaných a více oblast databáze účty, které představují vymazat poměry mezi konzistence, dostupnost a výkonu, všechna odpovídající záruky zjednodušuje globální distribuční data. Účty databázi DocumentDB nabídnuta s vysokou dostupnost, čekacích jedna číslice ms dob, více [úrovní dobře definovaný konzistence] [consistency], průhledné místní převzetí s více homing rozhraní API a možnost elastically měřítko výkonu a úložiště po celém světě. 

  
## <a name="configuring-multi-region-accounts"></a>Konfigurace účtů více oblastí

Konfigurace účtu DocumentDB zobrazit po celém světě se teď dá za méně než jednu minutu přes [Azure portálu](documentdb-portal-global-replication.md). Je třeba udělat stačí vyberte úroveň správné konzistence mezi několik úrovní podporované dobře definovaný konzistenci a přiřadit libovolný počet Azure oblastí pomocí svého účtu databáze. Úrovně konzistence DocumentDB představují vymazat poměry mezi konkrétní konzistence záruky a výkonu. 

![DocumentDB nabízí víc, dobře definované (naleznete) konzistence modely můžete vybírat][1]

DocumentDB nabízí víc, dobře definované (naleznete) konzistence modely můžete vybírat.

Výběr správné konzistence úrovně závisí na datech konzistence zaručit potřebami aplikace. DocumentDB automaticky zreplikuje dat přes všechny zadané regiony a zaručuje konzistenci, kterou jste vybrali pro váš účet databáze. 


## <a name="using-multi-region-failover"></a>Použití více oblastí překlopení 

Azure DocumentDB nebude účtům transparentně převzetí databáze napříč několika Azure oblastí – nové [více homing rozhraní API] [ developingwithmultipleregions] jistotu, že aplikace můžete dál používat logické koncového bodu a nepřetržité tak, že záložní. Převzetí řídí můžete, poskytující flexibilitu přesunout databázi na účtu v případě jednotlivých rozsah možnou chybu podmínek dojít k, včetně aplikace, infrastruktura, služby a místní neúspěšné (nebo skutečných simulovaný). V případě místní selhání DocumentDB službu selže transparentně nad vaším účtem databáze a aplikace pokračuje pro přístup k datům a nepřijít dostupnosti. Během DocumentDB nabízí [99,99 % dostupnost rozsahu][sla], můžete otestovat aplikace end vlastnosti dostupnost zakončení pomocí obou [programově] simulace selhání místní[ arm] stejně jako portálu Azure.


## <a name="scaling-across-the-planet"></a>Změna měřítka přes planety
DocumentDB umožňuje nezávisle na sobě zřízení výkon a používání úložiště pro každou kolekci DocumentDB ve všech velkém měřítku globálně napříč všech oblastí přidruženého k vašemu účtu databáze. Kolekce DocumentDB je automaticky distributed globálně a spravovat ve všech oblastech přidruženého k vašemu účtu databáze. Kolekce v rámci vašeho účtu databáze může být rozvržena jakoukoli z Azure oblastí, ve kterém [je k dispozici DocumentDB služba][serviceregions]. 

Výkon koupili a spotřebované množství pro každou kolekci DocumentDB úložiště je automaticky k dispozici ve všech oblastech rovnoměrně z vnější. Díky aplikaci Bezproblémová zobrazit přes glóbus [platební pouze výkon a úložiště, které používáte v rámci každou hodinu][pricing]. Například pokud máte zřízení 2 miliony RUs kolekce DocumentDB, pak každé z oblastí přidruženého k vašemu účtu databáze získá RUs 2 miliony pro tuto kolekci. To je znázorněno níže.

![Změny měřítka výkon kolekce DocumentDB ve čtyřech oblastech][2]

DocumentDB zaručuje < 10 ms čtení a zápis < 15 ms čekacích dob na P99. Přečtěte si požadavky nikdy rozsahu datacentra hranice zajistit [soulad požadavky, které jste vybrali][consistency]. Zápisy jsou vždy kvora potvrzeného místně před jsou potvrzeno klientům. Každý databáze účet nakonfigurovaný zápisu oblast priorita. Oblast určenou s nejvyšší prioritou bude sloužit jako aktuální oblasti zápisu pro účet. Všechny SDK transparentně směrujte databáze účtu zápisy aktuální oblasti zápisu. 

Nakonec protože DocumentDB je úplně [bez ohledu na schéma] [ vldb] -ukládání nemusíte vůbec starat starat o správě/aktualizaci schémat nebo vedlejší indexy napříč několika datacentrech. Svoje [dotazy SQL] [ sqlqueries] dál pracovat, zatímco aplikace a datové modely se nadále vyvíjejí. 


## <a name="enabling-global-distribution"></a>Povolení globálního rozdělení. 

Můžete se rozhodnout, aby se data místně nebo globálně distributed přidružením buď jeden nebo více Azure oblasti s DocumentDB databáze účtu. Můžete přidat nebo odebrat oblastí ke svému účtu databáze kdykoli. Povolení globálního distribuce pomocí portálu, najdete v článku [Jak provést replikace databáze globální DocumentDB pomocí portálu Azure](documentdb-portal-global-replication.md). Povolení globálního distribuce programově, najdete v článku [rozvojových s účty DocumentDB více oblastí](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Další kroky

Další informace o distribuce data globálně s DocumentDB v těchto článcích:

* [Zřízení výkonu a úložiště pro kolekci] [throughputandstorage]
* [Více homing API prostřednictvím ZBÝVAJÍCÍ. .NET, Java, Python a uzel SDK] [developingwithmultipleregions]
* [Soulad úrovní v DocumentDB] [consistency]
* [Dostupnost rozsahu] [sla]
* [Správa databáze účtu] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

