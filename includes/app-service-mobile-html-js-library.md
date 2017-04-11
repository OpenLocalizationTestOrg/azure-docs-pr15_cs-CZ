##<a name="create-client"></a>Vytvoření připojení klientů

Vytvoření připojení klientů vytvořením `WindowsAzure.MobileServiceClient` objektu.  Nahrazení `appUrl` se adresa URL pro váš mobilní aplikace.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Práce s tabulkami

Access nebo aktualizace dat vytvořit odkaz na tabulku back-end. Nahrazení `tableName` s názvem tabulky

```
var table = client.getTable(tableName);
```

Až budete mít odkaz na tabulku, můžete pracovat pomocí tabulky:

* [Dotaz tabulky](#querying)
  * [Filtrování dat](#table-filter)
  * [Procházení dat](#table-paging)
  * [Řazení dat](#sorting-data)
* [Vložení dat](#inserting)
* [Úprava dat](#modifying)
* [Odstranění dat](#deleting)

###<a name="querying"></a>Postup: dotazu odkaz na tabulku

Až budete mít odkaz na tabulku, můžete ji použijete k vytvoření dotazu pro data na serveru.  Dotazy jsou určené v jazyce "LINQ like".
Pokud chcete vrátit všechna data z tabulky, použijte následující:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Funkce úspěch nazývá s výsledky.   Nepoužívejte `for (var i in results)` úspěchu pracovat, budou iteraci informace obsažené v seznamu výsledků při další funkce dotazu (například `.includeTotalCount()`) se používají.

Další informace o syntaxi dotazu najdete informace v nápovědě k [dotazu objekt].

####<a name="table-filter"></a>Filtrování dat na serveru

Můžete použít `where` klauzule na odkaz tabulky:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Můžete také použít funkci, která vyfiltruje objektu.  V tomto případě `this` proměnné přiřazen aktuální objekt filtrován.  Následující odpovídá funkčně předchozího příkladu:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Procházení dat

Využití metody take() a skip().  Pokud například chcete rozdělit záznamy 100 řádků v tabulce:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

`.includeTotalCount()` Metoda slouží k přidání pole do totalCount objekt výsledky.  Pole totalCount vyplněná celkový počet záznamy, které by být vrácena pokud žádná stránkování používá.

Pak můžete proměnné stránky a některé tlačítek v uživatelském rozhraní poskytují seznam stránek; použití loadPage() načíst nové záznamy pro každou stránku.  Měli byste určitý druh ukládání do mezipaměti rychlost přístup záznamy, které již byly načíst.


####<a name="sorting-data"></a>Postup: vrátí data seřazená

Použijte metodu .orderBy() nebo .orderByDescending() dotazu:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Další informace o objekt dotazu informace v nápovědě k [dotazu objekt].

###<a name="inserting"></a>Postup: vložení dat

Vytvoření objektu JavaScript s příslušné datum a volání table.insert() asynchronní:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Na úspěšné vložení je vložený položku vráceny další pole, které jsou potřeba pro operace synchronizace.  Měli byste aktualizovat vlastní mezipaměti se tyto informace pro pozdější úpravy.

Všimněte si, že Server SDK Azure mobilní aplikace Node.js podporuje dynamické schématu pro účely vývoj.
V případě dynamické schématu schématu tabulky aktualizována rychlé úpravy, což umožňuje přidat sloupce do tabulky jenom zadáním je v insert nebo operace aktualizace.  Doporučujeme vypnout dynamické schématu přejde aplikace do výroby.

###<a name="modifying"></a>Postup: změnit Data

Podobně jako metodu .insert() by měl vytvoříte objektu aktualizace a pak zavolejte .update().  Aktualizace objekt musí obsahovat Identifikátor záznamu mají být aktualizovány – to je získali při čtení záznam nebo při volání .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Postup: odstranění dat

Zavolejte metodu .del() pro odstranění záznamu.  Odkaz na objekt předáte ID:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
