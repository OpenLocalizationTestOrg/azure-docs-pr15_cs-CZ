<properties
    pageTitle="Funkce JSON databáze SQL Azure | Microsoft Azure"
    description="Databáze SQL Azure umožňuje analýze dotazu a formátování dat v JavaScript Object Notation (JSON) zápisu."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Začínáme s funkcemi JSON v databázi SQL Azure

Databáze SQL Azure umožňuje analyzovat data ve formátu JavaScript Object Notation [(JSON)](http://www.json.org/) dotazu a export relačních dat jako JSON text.

JSON je Oblíbené dat formát pro výměnu dat v moderní webu a mobilní aplikace. JSON se používá taky pro ukládání částečně strukturovaná data v protokoly nebo v databázích NoSQL jako [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Mnoho webovým službám ZBÝVAJÍCÍ vrácených výsledků formátovaná jako JSON text nebo přijímat data ve formátu JSON. Většina Azure služeb, jako je [Azure vyhledávání](https://azure.microsoft.com/services/search/), [Azure úložiště](https://azure.microsoft.com/services/storage/)a [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) mít ZBÝVAJÍCÍ koncové body, které vracejí nebo používání JSON.

Databáze SQL Azure umožňuje snadno pracovat s daty JSON a integrace databáze s moderní službami.

## <a name="overview"></a>Základní informace

Databáze SQL Azure poskytuje následující funkce pro práci s daty JSON:

![Funkce JSON](./media/sql-database-json-features/image_1.png)

Pokud máte JSON text, můžete extrahování dat z JSON nebo ověřte, jestli je JSON správně naformátovaný pomocí integrovaných funkcí [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx) [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)a [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). Funkce [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) umožňuje aktualizovat hodnotu uvnitř textu JSON. Pro rozšířené dotazy a analýzy, funkce [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) transformovat maticových JSON objektů do sady řádků z. Dotaz SQL lze spustit na sadu vrácených výsledků. Nakonec je klauzuli [Pro JSON](https://msdn.microsoft.com/library/dn921882.aspx) , které můžete formátovat data uložená v tabulkách relační formátu JSON textu.

## <a name="formatting-relational-data-in-json-format"></a>Formátování relačních dat ve formátu JSON
Pokud máte webové služby přijímá dat z databáze vrstvy a poskytuje odpověď JSON formátování, zda klientských JavaScript rámců nebo knihoven, které přijímají dat ve formátu JSON, můžete naformátovat obsah databáze jako JSON přímo v dotazu SQL. Už máte psát kód aplikace, který formáty výsledky z databáze SQL Azure jako JSON, nebo obsahují některé JSON serializace knihovny převést výsledky tabulkových dotazu a potom serializovat objekty ve formátu JSON. Místo toho můžete klauzule pro JSON formátu JSON v databázi SQL Azure SQL výsledků dotazu a použít ho přímo v aplikaci.

V následujícím příkladu řádků z tabulky Sales.Customer jsou ve formátu JSON pomocí klauzule JSON pro:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

Klauzule pro CESTU JSON formátu JSON text výsledků dotazu. Názvy sloupců jsou používána jako klávesy, zatímco hodnot buněk se vytvářejí jako JSON hodnoty:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Sada výsledků se automaticky naformátuje jako maticové JSON, kde je každý řádek formátované jako samostatný objekt JSON.

CESTA označuje, že výstupní formát JSON výsledek jde přizpůsobit pomocí tečkami v aliasů sloupců. Následující dotaz změní název "JménoZákazníka" klíče ve formátu JSON výstup a umístí telefon a faxové číslo dílčí objekt "Kontakt":

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Výstup dotazu vypadat takto:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

V tomto příkladu jsme vrácené na jeden objekt JSON místo maticových určující možnost [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . Pokud víte, že jsou vracela na jeden objekt jako výsledek dotazu, můžete použít tuto možnost.

Hlavní klauzule JSON pro hodnotu, umožňuje vrátit složité hierarchických dat z databáze ve formátu vnořené JSON objekty nebo matice. Následující příklad ukazuje, jak zahrnout objednávky, které patří zákazníkovi jako vnořené pole objednávky:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Místo odesílání oddělit dotazů zobrazíte data o zákaznících a pak načtení seznamu souvisejících objednávek, zobrazí se všechny potřebné dat pomocí jednoho dotazu, jak je uvedeno v následující ukázce výstupu:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Práce s daty JSON

Pokud nemáte sporná, i když strukturovaná data, pokud máte složité dílčí objekty, matice nebo hierarchická data nebo datové struktury v průběhu času vyvíjejí, formátu JSON můžete představuje libovolný komplexních dat strukturu.

JSON je textový formát, který slouží jako jiný typ řetězce v databázi SQL Azure. Budete moct posílat a obsahují JSON dat jako standardní NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

V tomto příkladě použita data JSON vyjádřena pomocí NVARCHAR(MAX) typu. JSON můžete vkládat do této tabulky ani zadané jako argumentu uložené procedury pomocí standardní syntaxe jazyce Transact-SQL, jak je ukázáno v následujícím příkladu:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Libovolný jazyk na straně klienta nebo knihovny, kde pracuje s daty řetězce v databázi SQL Azure fungovat taky s daty JSON. JSON mohou být uloženy libovolné tabulky, který podporuje NVARCHAR typu, jako jsou paměť optimalizována tabulku nebo tabulku verzí systému. JSON není Představte všechna omezení na straně klienta kód nebo vrstva databáze.

## <a name="querying-json-data"></a>Dotazy na JSON data

Pokud máte data ve formátu JSON uložená v tabulkách Azure SQL, funkce JSON umožní tato data použít v libovolné dotaz SQL zobrazený.

Funkce JSON, které jsou k dispozici v umožňují databáze Azure SQL nakládáte s údaji ve formátu JSON jako libovolným jiným datovým typem SQL. Můžete snadno extrahovat hodnoty u textu JSON a použití JSON dat v libovolné dotazu:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Funkce JSON_VALUE extrahuje hodnotu z textu JSON uložené ve sloupci Data. Tuto funkci používá JavaScript profesionálové cesta k odkazovat na hodnotu v textu JSON extrahovat. Extrahované hodnoty můžete použít v libovolné části dotazu SQL.

Funkce JSON_QUERY je podobná JSON_VALUE. Na rozdíl od JSON_VALUE tato funkce extrahuje složité dílčí objekt, například polí nebo objekty, které jsou umístěná v textu JSON.

Funkce JSON_MODIFY umožňuje zadat cestu hodnotu v JSON text, který by měly být aktualizovány, jakož i novou hodnotu, která bude přepsat původní. Tímto způsobem můžete snadno aktualizovat JSON text bez reparsing celé struktury.

Protože JSON uložený ve standardní text, jsou žádné záruky, že jsou hodnoty uložené ve sloupcích text správně formátované. Můžete ověřit, že text uložený ve sloupci JSON správně formátované pomocí standardní databázi SQL Azure zaškrtnutí omezení a funkci ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Pokud správně naformátovaná zadávání textu JSON, ISJSON vrátí hodnotu 1. Na každém vložit nebo aktualizace JSON sloupce toto omezení se zkontrolujte, že nové textové hodnoty není poškozený JSON.

## <a name="transforming-json-into-tabular-format"></a>Transformace JSON do formátu tabulky

Databáze SQL Azure vás také seznámí transformace kolekce JSON na tabulkovém formátu a načíst nebo dotaz JSON data.

OPENJSON je funkce hodnota tabulky, která analyzuje JSON text najde maticových JSON objektů, prochází prvky matice a vrátí jeden řádek ve výsledku výstup pro každý prvek pole.

![JSON tabulky](./media/sql-database-json-features/image_2.png)

V uvedeném příkladu můžeme určit umístění JSON pole, které by měl být otevřen (v rozevíracím seznamu $. Cesta objednávky), jaké sloupce má být vrácena, i výsledek, kde najdete JSON hodnoty, které budou vráceny jako buňky.

Jsme transformace JSON matice v @orders proměnné do sady řádků, analyzovat Tato sada výsledků dotazu nebo vložení řádků do standardní tabulky:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Shromažďování objednávek ve formátu JSON pole a podle parametr uložená procedura můžete analyzovat a vložený do tabulky objednávky.

## <a name="next-steps"></a>Další kroky

Informace o integraci JSON do aplikace, podívejte se na tyto materiály:

- [Web TechNet blogu](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [Si přečtěte následující dokumentaci MSDN](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanálu 9 videa](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Další informace o různých scénáře integrace JSON do aplikace, najdete v článku ukázky toto [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) nebo najít situace, která odpovídá případu použití v [příspěvků blogu JSON](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).
