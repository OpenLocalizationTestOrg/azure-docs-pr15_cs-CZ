<properties 
    pageTitle="Příklady NoSQL Python pro DocumentDB | Microsoft Azure" 
    description="Najdete NoSQL Python příklady github pro běžné úkoly v DocumentDB, včetně operace CRUD pro JSON dokumenty v databázích NoSQL." 
    keywords="Příklady Python"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Příklady DocumentDB Python

> [AZURE.SELECTOR]
- [Příklady .NET](documentdb-dotnet-samples.md)
- [Příklady Node.js](documentdb-nodejs-samples.md)
- [Příklady Python](documentdb-python-samples.md)
- [Galerie ukázek kódu Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Ukázka řešení, která umožňuje provádět operace CRUD a dalších běžné operací Azure DocumentDB zdroje jsou součástí úložiště GitHub [azure documentdb python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) . Tento článek obsahuje:

- Propojení úkolů v jednotlivých souborů Python příklad projektu. 
- Odkazy na související rozhraní API odkazovat na obsah.

**Zjistit předpoklady pro**

1. Musíte mít účet Azure použít tyto příklady Python:
    - Můžete [Otevřít účet Azure zdarma](https://azure.microsoft.com/pricing/free-trial/): získání přeplatky můžete vyzkoušet placené služby Azure a i po byli zvyklí až můžete ponechat účet a použití uvolnit Azure služby, jako je weby. Platební kartou nikdy strhne příslušná, pokud explicitně změňte nastavení a požádat o platit.
   - Je možné [Aktivovat výhod odběratele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): vaše aplikace Visual Studio předplatné vám přeplatky každý měsíc, využívající služby placené Azure.
2. Budete potřebovat [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Každý vzorek je samostatná, nastaví samotné a vyčistí po samotné. Jako takové vzorky zadejte několik volání [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Pokaždé, když provedete předplatného se vám nebudou účtovat poplatky pro 1 hodinu využití za osy výkonu kolekce vzniku. 

## <a name="database-examples"></a>Příklady databáze

Soubor [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) projektu [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) ukazuje, jak provádět následující úkoly.

Úkol | Rozhraní API odkaz
--- | ---
[Vytvoření databáze](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Dotaz účtu pro danou databázi](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Přečtěte si databáze podle Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Seznam databází pro účet](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Odstranění databáze](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Příklady kolekce 

Soubor [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) projektu [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) ukazuje, jak provádět následující úkoly.

Úkol | Rozhraní API odkaz
--- | ---
[Vytvoření kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Přečtěte si seznam všech kolekcí v databázi](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Získání kolekce podle Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Získání výkonu osy kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Změna úrovně výkonu kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Odstranění kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
