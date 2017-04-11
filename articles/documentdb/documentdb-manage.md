<properties
    pageTitle="DocumentDB úložiště a výkonu | Microsoft Azure" 
    description="Informace o ukládání dat a ukládání dokumentů do DocumentDB a jak je možné přizpůsobit DocumentDB potřebám kapacita aplikace." 
    keywords="ukládání dokumentů"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Další informace o ukládání a předvídatelná výkonu zřizování v DocumentDB
Azure DocumentDB je plně spravované, scalable týkajících se dokumentů NoSQL databáze služby JSON dokumentů. S DocumentDB nemusíte pronajímat virtuálních počítačích, nasazení software nebo sledovat databází. DocumentDB je spravovaný a průběžně sledovány pracovníci společnosti Microsoft k předvádění prezentace třídy dostupnost, výkon a ochrana dat.  

Mohli rovnou začít s DocumentDB vytvořením [účet databáze](documentdb-create-account.md) a [databáze DocumentDB](documentdb-create-database.md) přes [Azure portálu](https://portal.azure.com/). Databáze DocumentDB nabídnuta v jednotkách jednotce (SSD) záložní úložiště a výkon. Tyto úložiště jednotky jsou zřízení [vytváření kolekcí databází](documentdb-create-collection.md) v rámci svého účtu databáze každé kolekce s vyhrazené výkon, který lze nastavit nahoru nebo dolů kdykoli požadavkům aplikace. 

Pokud aplikace překročí rezervovaná výkon pro jednu nebo více kolekcí, požadavky omezený na základě za kolekce. To znamená, že některé požadavky aplikace může být úspěšné, zatímco ostatní omezena.

Tento článek obsahuje přehled zdrojů a metriky k dispozici pro řízení kapacity a ukládání dat v plánu. 

## <a name="database-account"></a>Účet databázi
Jako Azure odběratelů můžete vytvořit jeden nebo více účtů databáze DocumentDB ke správě databáze prostředků. Každý předplatné je přidružený jednoho Azure předplatné. 

Účty DocumentDB lze vytvořit přes [Azure portál](documentdb-create-account.md)nebo pomocí [šablony ARM nebo Azure rozhraní příkazového řádku](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Databáze
Jeden DocumentDB databáze může obsahovat prakticky neomezené množství seskupené do kolekce ukládání dokumentů. Kolekce poskytují izolace výkonu: každé kolekce můžete spravovat pomocí výkon, který není nasdílel ostatních kolekcí ve stejné databázi nebo účet. Databáze DocumentDB je pružná ve skupinovém rámečku velikost od GB pro ukládání dokumentů TBs SSD záložní a zřizování výkon. Na rozdíl od databázi tradiční RDBMS databáze v DocumentDB není obor vymezený na jednom počítači a může zahrnovat více počítačů nebo clusterů.  

S DocumentDB jako je třeba změnit velikost aplikace, můžete vytvořit více kolekcí nebo databáze nebo obojí. Databáze se dají vytvářet prostřednictvím [Azure portál](documentdb-create-database.md) nebo některý ze [Sady DocumentDB SDK](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Kolekcí databází
Každý DocumentDB databáze může obsahovat minimálně jednu kolekci. Kolekce představovat oddíly vysoce dostupné dat pro ukládání dokumentů a zpracování. Jednotlivé kolekce mohou být uloženy dokumenty s nesourodými schéma. Automatické indexování DocumentDB společnosti a použití dotazů umožňují snadno filtrovat a načíst dokumenty. Kolekce obsahuje obor pro spuštění úložiště a dotaz dokumentu. Kolekce je taky doména transakce pro všechny dokumenty obsažené v něm obsažené. Kolekce přiřazených výkon založené na hodnotu nastavenou na portálu Azure nebo prostřednictvím SDK. 

Kolekce jsou automaticky rozdělit na jeden nebo více fyzické serverů tak, že DocumentDB. Když vytvoříte kolekci, můžete určit zřizování výkon z hlediska žádost o jednotky za sekundu a vlastnost klíče oddíl. Hodnota vlastnosti používá DocumentDB k distribuci dokumentů mezi oddíly a směrování požadavků jako dotazů. Hodnoty klíče oddílu taky slouží jako hranici transakce pro uložené procedury a aktivace. Jednotlivé kolekce má rezervovaný počtu výkon specifické pro tuto kolekci, které není nasdílel ostatních kolekcí ve stejném účtu. Proto rozšiřování aplikace jak z hlediska úložiště a výkon. 

Kolekce se dají vytvářet prostřednictvím [Azure portál](documentdb-create-collection.md) nebo některý ze [Sady DocumentDB SDK](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Žádost o jednotky a operace databáze

Při vytváření kolekce rezervovat výkon z hlediska [žádost o jednotky (RU)](documentdb-request-units.md) na druhé. Místo toho přemýšlíte o a správa prostředcích, si můžete představit **žádost o jednotku** jako jednu míru u zdrojů potřebná k provádění různých operacích databáze a žádosti o služby. Čtení dokumentu 1 KB využívá stejné 1 RU bez ohledu na počet položek uložených v kolekci nebo počet souběžně prováděné žádosti o spuštění současně. Všechny žádosti o proti DocumentDB, včetně složité operace třeba dotazy SQL have předvídatelná RU hodnota, která lze určit v době vývoje. Pokud znáte velikost dokumentů a četnost každé operace (čtení, zápisy a dotazy) na podporu aplikace, můžete vytvořit přesný počet výkon potřebám vaší aplikace a měřítko databázi nahoru a dolů výkon potřeb. 

Každou kolekci lze rezervovat s výkon v blocích 100 jednotek žádost za sekundu, stovky až miliony jednotek požadavek na druhé. Zřizování výkon lze upravit průběhu kolekce přizpůsobit potřebám změna zpracování a přístup k vzorků aplikace. Další informace najdete v tématu [DocumentDB výkonu úrovně](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Kolekce jsou fakturaci entity. Náklady, je určený podle zřizování výkon kolekci měří se v žádosti o jednotky sekundu spolu s celkové spotřebované úložiště v GB. 

Kolik jednotek žádost bude operace zejména jako vložení, odstranění, dotaz nebo spuštění uložené procedury využívat? Jednotka žádost o vyjadřuje normalizovanou vyžádání náklady. Čtení dokumentu 1 KB je 1 RU, ale žádost o vložení, nahrazení nebo odstranění do stejného dokumentu potřebuje více procesorů ze služby a tím další žádosti. Každá odpověď z služby obsahuje vlastní záhlaví (`x-ms-request-charge`), která sestavy jednotek žádost o použití žádosti. Toto záhlaví je také k dispozici až [SDK](documentdb-sdk-dotnet.md). V .NET SDK je [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) vlastnosti objektu [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) . Pokud chcete k odhadu vašim potřebám výkon než se pustíte do jednoho volání, můžete [kapacitu Plánovač](documentdb-request-units.md#estimating-throughput-needs) Nápověda k této odhad. 

>[AZURE.NOTE] Směrný plán 1 jednotku žádost o dokumentu 1 KB odpovídá jednoduché GET dokumentu s [Konzistence relace](documentdb-consistency-levels.md). 

Existuje několik faktorů, které mají vliv jednotek žádost o použití pro operaci proti účet DocumentDB databáze. Zahrnout následujících skutečností:

- Velikost dokumentu. Jak velikost dokumentu rozšiřte jednotky spotřebované množství načtení nebo zápisu dat, že data také zvětšíte.
- Počet vlastností. V případě výchozí indexování všech vlastností, jednotky spotřebované množství napsat dokument zvětšíte jako zvyšuje počet vlastnost.
- Soulad data. Pokud chcete použít data konzistence úrovně silné nebo ohraničených Staleness, další jednotky bude potřeba ke čtení dokumentů.
- Indexované vlastnosti. Zásady indexu u každé kolekce určuje vlastnosti, které jsou ve výchozím nastavení indexováno. Žádost o Jednotková spotřeba můžete zmenšit omezení počtu indexované vlastnosti. 
- Indexování dokumentu. Ve výchozím nastavení automaticky indexováno každý dokument bude využívat méně jednotky žádost o, pokud se rozhodnete indexovat některé dokumenty.

Další informace najdete v tématu [DocumentDB žádost o jednotky](documentdb-request-units.md). 

Například, tady je tabulka zobrazující počet jednotek žádost o poskytování v tři velikosti jiného dokumentu (1KB 4KB a 64KB) a na dvou úrovních různých výkon (500 čtení/druhé 100 zápisy/druhé a 500 čtení/druhé + 500 zápisy/druhé). Konzistence dat o konfiguraci relace a indexování zásada byla nastavena na žádný.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Velikost dokumentu</strong></p></td>
            <td valign="top"><p><strong>Čtení a druhé</strong></p></td>
            <td valign="top"><p><strong>Zápisy/druhé</strong></p></td>
            <td valign="top"><p><strong>Žádost o jednotky</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 ZNALOSTNÍ BÁZI KNOWLEDGE BASE</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 ZNALOSTNÍ BÁZI KNOWLEDGE BASE</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 ZNALOSTNÍ BÁZI KNOWLEDGE BASE</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 ZNALOSTNÍ BÁZI KNOWLEDGE BASE</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Dotazy, uložené procedury a aktivace zpracovávat žádosti o jednotky podle složitost provádí operace. Při přípravě aplikace zkontrolujte záhlaví poplatku za žádost a snadněji pochopit, jak každé operace spotřebovává žádost o jednotku doby.  


## <a name="choice-of-consistency-level-and-throughput"></a>Volba konzistence úrovní a výkon
Volba výchozí úroveň konzistence má dopad na výkon a latence. Můžete nastavit výchozí úroveň konzistence programově i portálu Azure. Je také možné přepsat úroveň konzistence na základě na žádost o. Ve výchozím nastavení je úroveň konzistence nastavena na **relace**, která poskytuje monotónní pro čtení a zápis a číst záruky zápisu. Relace konzistence jsou skvělé pro uživatele zaměřené na zpracování aplikací a poskytuje ideální poměr konzistenci a výkonu střídání.    

Další informace o změně úroveň konzistence na portálu Azure najdete v tématu [Správa účtu DocumentDB](documentdb-manage-account.md#consistency). Nebo Další informace o úrovních konzistence najdete v článku [použití konzistence úrovně](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Ukládání a index režijních zřizování dokumentu
DocumentDB podporuje vytváření kolekce jedním oddílem a oddíly. Každý oddíl DocumentDB podporuje až 10 GB úložiště SSD zálohy. 10GB úložiště dokumentů se skládá z dokumentů a úložiště pro index. Ve výchozím nastavení kolekce DocumentDB nakonfigurovaný tak, aby všechny dokumenty automaticky indexovat explicitně bez sekundární indexy nebo schéma. Je založená na aplikací pomocí DocumentDB, režijních typické index mezi 2-20 %. Indexování technologii použitou DocumentDB zaručuje, že bez ohledu na to hodnot vlastností režijních index nepřekročí vyšší než 80 % velikosti dokumentů s výchozím nastavením. 

Ve výchozím nastavení všechny dokumenty jsou automaticky indexovat tak, že DocumentDB. Ale pokud chcete optimalizovat režijních index, můžete odebrat některé dokumenty z indexování při vkládání nebo nahrazení dokumentu, podle popisu v [DocumentDB indexování zásady](documentdb-indexing-policies.md). Můžete nakonfigurovat kolekce DocumentDB vyloučit všechny dokumenty v rámci kolekce z indexování. Můžete taky nakonfigurovat kolekce DocumentDB selektivně indexovat jenom některé vlastnosti nebo cesty pomocí zástupných znaků JSON dokumentů, podle popisu v [Konfigurace indexování zásad kolekce](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). S výjimkou vlastnosti nebo dokumenty také zlepšuje výkon zápisu – což znamená, že bude využívat méně žádost o jednotky.   

## <a name="next-steps"></a>Další kroky

Přečtěte si víc o tom, jak DocumentDB funguje, najdete v článku [Partitioning a stejné měřítko v Azure DocumentDB](documentdb-partition-data.md).

Pokyny týkající se sledování výkonu úrovně na portálu Azure najdete v tématu [Sledování DocumentDB účtu](documentdb-monitor-accounts.md). Další informace o výběru výkonu úrovně Collections najdete v článku [výkon úrovně v DocumentDB](documentdb-performance-levels.md).
 
