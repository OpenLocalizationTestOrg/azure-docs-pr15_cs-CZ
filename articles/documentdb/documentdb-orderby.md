<properties 
    pageTitle="Řazení dat DocumentDB pomocí ORDER | Microsoft Azure" 
    description="Zjistěte, jak používat Order v dotazech DocumentDB LINQ a SQL a určení indexování zásad pro Order dotazy." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Řazení dat DocumentDB pomocí Order By
Microsoft Azure DocumentDB podporuje zjišťování dokumenty pomocí jazyka SQL JSON dokumenty. Výsledky dotazu můžete objednali pomocí klauzule ORDER BY v příkazech dotaz SQL.

Po přečtení v tomto článku si budete moct odpovězte na následující otázky: 

- Jak dotazu s ORDER?
- Jak nakonfigurovat indexování zásad pro Order By
- Co je tu další?

[Ukázky](#samples) a [Nejčastější dotazy týkající se](#faq) také k dispozici.

Kompletní odkaz na dotazy SQL najdete v tématu [kurz DocumentDB dotazu](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Jak k vytvoření dotazu s Order By
Stejně jako v ANSI SQL, můžete teď Zahrnout volitelné klauzule ORDER v příkazech SQL při dotazování DocumentDB. Klauzule mohou obsahovat volitelný argument ASC/DESC určete pořadí, ve které musí výsledky získávány. 

### <a name="ordering-using-sql"></a>Řazení pomocí jazyka SQL
Tady je příklad dotazu k načtení knihy prvních 10 sestupně podle jejich názvů. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Řazení pomocí jazyka SQL s filtrování
Můžete řadit pomocí vnořené vlastnosti v dokumentech jako Books.ShippingDetails.Weight a další filtry můžete zadat v klauzuli WHERE v kombinaci s Order jako v tomto příkladu:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Řazení pomocí zprostředkovatele LINQ pro .NET
Použití .NET SDK verze 1.2.0 a vyšší, můžete taky použít klauzuli OrderBy() nebo OrderByDescending() v rámci LINQ dotazy podobně jako v tomto příkladu:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB podporuje řazení jednoho číselné, řetězec nebo logická vlastnost na dotaz, s typy další dotazů brzy k dispozici. Najdete v tématu [Co je tu další](#Whats_coming_next) pro další podrobnosti.

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfigurace indexování zásad pro Order By

Odvolat, že DocumentDB podporuje dvěma druhy indexy (Hash a oblast), které se dá nastavit pro konkrétní cesty/vlastnosti datových typů (řetězce/čísla) a na různých přesnost hodnoty (Maximální přesnost nebo pevnou přesností hodnoty). Protože DocumentDB používá Hash indexování jako výchozí, musí vytvoříte novou kolekci s vlastní indexování zásadu kombinací rozsah na čísla, řetězce nebo obojí mohli použít řadit podle. 

>[AZURE.NOTE] Řetězec oblast indexy byly vydané na 7 červenec 2015 pomocí rozhraní REST API verze 2015 06 03. Chcete-li vytvořit zásady pro Order proti řetězce, je nutné použít SDK verze 1.2.0 .NET SDK nebo verze 1.1.0 Python, Node.js nebo Java SDK.
>
>Před rozhraní REST API verze 2015 06 03 byl výchozí zásady indexování kolekce Hash řetězce a čísel. To byla změněna na Hash pro řetězce a oblasti pro čísla. 

Další informace najdete v článku [DocumentDB indexování zásady](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indexování Order proti všech vlastností
Tady je způsob můžete vytvořit kolekci s "Všechny oblasti" indexování Order proti libovolného a všechny číselné nebo řetězec vlastnosti, které se zobrazí v dokumentech JSON v něm obsažené. Tady jsme přepsat výchozí typ indexu u textových hodnot na oblast a v maximální přesnost (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Všimněte si, že Order jenom výsledky vrátí datových typů (řetězce a číslo) indexovaných s RangeIndex. Například pokud máte jako výchozí indexování zásad, která pouze RangeIndex na čísla, Order proti cestu s hodnotami typu řetězec vrátí nejsou žádné dokumenty.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indexování Order pro jednu vlastnost
Tady je, jak vytvořit kolekci s indexování Order proti jenom vlastnost název, který je řetězec. Existují dvě cesty, jedno pro vlastnost název ("/ název /?") s oblast indexování a druhý pro každou další vlastnost s indexování schéma výchozí, což je Hash pro řetězce a oblasti pro čísla.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Vzorky
Podívejte se na tento [Github vzorky projektu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , který demonstruje použití order, včetně vytváření indexování zásady a stránkování pomocí pořadí. Vzorky jsou otevřít zdroj a doporučujeme odesílat žádosti o vyžádané s příspěvky, které by mohly využít ostatním DocumentDB vývojářům. Získáte [příspěvek pokyny](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) pro informace o tom, jak přispívat.  

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

**Co je očekávaná spotřeba požádat o jednotky (RU) Order dotazů?**

Od Order využívá DocumentDB indexu vyhledávání, bude vypadat podobně jako odpovídající dotazy bez Order počet jednotek žádost o využívané Order dotazů. Stejně jako jakékoli jiné operace na DocumentDB počet jednotek, žádost o závisí na velikosti/obrazci dokumentů a také složitost dotazu. 


**Co je očekávané indexování nároky ORDER?**

Indexování nároky úložiště bude poměrné počty vlastnosti. V Scénář Nejhorší varianta bude režijních index 100 % data. Žádné mezi je rozdíl v výkon (požádat o jednotky) režijních oblast/Order indexování a indexování Hash výchozí.

**Jak dotazu Moje existujících dat v DocumentDB pomocí pořadí?**

Pokud chcete řadit výsledky dotazu pomocí pořadí, musí změnit indexování zásady kolekci použití typu oblasti index proti vlastnost se nepoužívá. V tématu [Úprava indexování zásad](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Co jsou aktuální omezení ORDER?**

ORDER lze zadat jenom proti vlastnost buď číselné nebo řetězec je oblast indexovat s maximální přesnost (-1).

Provedením následujících akcí:
 
- ORDER s vlastnostmi interní řetězec jako id, _rid a _self (už brzo).
- ORDER s vlastnostmi odvozeno z výsledků na spojení uvnitř dokumentu (už brzo).
- Řadit podle víc vlastnosti (už brzo).
- ORDER s dotazy na databáze, kolekcí, uživatele, oprávnění nebo přílohy ve formě (už brzo).
- Řadit podle s počítanou vlastnostmi jako výsledek výrazu nebo UDF/vestavěné funkci.

ORDER nepodporuje aktuálně pro více oddílů dotazů při použití Průzkumníka dotazu na portálu Azure.

## <a name="troubleshooting"></a>Řešení potíží

Pokud se zobrazí chyba, že Order není podporovaný, zkontrolujte, že používáte verzi [SDK](documentdb-sdk-dotnet.md) , která podporuje řadit podle. 

## <a name="next-steps"></a>Další kroky

Rozvětvené [Github vzorky projektu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) a začít s řazením vaše data. 

## <a name="references"></a>Odkazy
* [Odkaz na DocumentDB dotaz](documentdb-sql-query.md)
* [Odkaz na zásad indexování DocumentDB](documentdb-indexing-policies.md)
* [Reference k DocumentDB SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order vzorky](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

