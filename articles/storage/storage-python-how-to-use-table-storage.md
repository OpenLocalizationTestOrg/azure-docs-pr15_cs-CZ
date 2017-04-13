<properties
    pageTitle="Použití úložiště tabulek z Python | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Použití úložiště tabulek z Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře pomocí služba úložiště tabulek Azure. Vzorky psaných Python a používat [Microsoft Azure úložiště SDK Python]. Uvedené scénáře zahrnují vytváření a odstraňování tabulce, kromě režimem vkládání a dotazování entity v tabulce.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Vytvoření tabulky

Objekt **TableService** umožňuje pracovat se službami tabulky. Následující kód vytvoří objekt **TableService** . Přidání kódu do horní všech souborů Python, ve kterém chcete programově přístup k úložišti Azure:

    from azure.storage.table import TableService, Entity

Následující kód vytvoří objekt **TableService** pomocí klíč účtu a název účtu úložiště.  Nahraďte "Můj účet" a "mykey" název účtu a klíče.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Přidat entitu, je nutné nejprve vytvořte slovníku či entitu, který definuje názvy vlastností entita a hodnoty. Všimněte si, že každé entity musíte zadat **PartitionKey** a **RowKey**. Toto jsou jedinečné identifikátory entity. Můžete dotaz pomocí těchto hodnot rychleji než dotazu další vlastnosti. Pomocí **PartitionKey** automaticky distribuce entity tabulky nad mnoho úložiště uzlů.
Entity, které mají stejné **PartitionKey** jsou uložené ve stejném uzlu. **RowKey** je jedinečné ID entity v oddílu, které patří.

Entita do tabulky přidáte předat objektu slovníku **Vložit\_entity** metody.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Můžete také předat instanci třídy **Entity** **Vložit\_entity** metody.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Aktualizace entita

Tento kód ukazuje, jak chcete nahradit starou verzi existující entitu aktualizovanou verzi.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Pokud osoba, která je aktualizován neexistuje, se nepovede operace aktualizace. Pokud chcete ukládat entita bez ohledu na to, zda je existoval, použijte **Vložit\_nebo\_replace_entity**.
V následujícím příkladu nahradí první volání existující entitu. Druhé volání vloží nový entity od žádná entita s zadaný **PartitionKey** a **RowKey** nachází v tabulce.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Změna skupiny entit

Někdy dává smysl můžou odeslat více operací pohromadě v jedné dávce zajistit atomová zpracování na serveru. Provádění, používaná **TableBatch** předmětu. Pokud chcete odeslat dávku, zavoláte **Potvrdit\_dávku**. Všimněte si, že všechny entity mohli, musíte být ve stejném oddílu bude možné měnit jako listu. Následující příklad sečte dva typy entit v listu.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Listy lze také pomocí syntaxe doplnit správce:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Dotaz entity

K vytvoření dotazu entita v tabulce, použít **získat\_entity** metoda předáním **PartitionKey** a **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Dotaz sadu entity

V tomto příkladu zjistí, že všechny úkoly v Seattlu podle **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Dotaz podmnožinu entity vlastností

Dotaz do tabulky můžete načíst pár vlastnosti z entity.
Tento postup s názvem *projekce*, snižuje šířky pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Použít parametr **Vyberte** a předejte názvy vlastnosti, které chcete převést klientovi.

Následující kód vracel pouze popisy entity v tabulce.

[AZURE.NOTE] Následující úryvek funguje jenom proti službě cloudového úložiště. To není podporováno emulátorem úložiště.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Odstranění entity

Entita můžete odstranit pomocí jeho oddílů a řádek klíče.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Odstranění tabulky

Následující kód odstraní tabulku z účtu úložiště.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy úložiště tabulek, tyto odkazy vedou na další informace.

- [Středisko pro vývojáře Python](/develop/python/)
- [Služby Azure úložiště rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog týmu Azure úložiště]
- [Microsoft Azure úložiště SDK Python]

[Azure úložiště týmového blogu]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure úložiště SDK Python]: https://github.com/Azure/azure-storage-python
