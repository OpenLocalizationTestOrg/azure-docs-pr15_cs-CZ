<properties 
    pageTitle="Programování DocumentDB: uložené procedury, databáze a funkce definované uživatelem | Microsoft Azure" 
    description="Naučte se DocumentDB můžete začít psát pomocí funkce definované uživatelem (UDF), uložené procedury a databáze v JavaScriptu. Získejte tipy programing databáze." 
    keywords="Aktivace databáze uložená procedura, uložené procedury, databázovým programem, sproc, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Programování serverovou DocumentDB: uložené procedury, databáze a funkce definované uživatelem

Zjistěte, jak Azure DocumentDB jazyk integrované, transakční provádění JavaScript umožňuje vývojářům nativně psaní **uložené procedury** **aktivačních událostí** a **funkce definované uživatelem** v JavaScriptu. Umožňuje provádět napsat databáze program aplikace logiku, kterou lze vyřízeny a spouštět přímo na oddíly úložiště databáze 

Doporučujeme Začínáme se můžete podívat v následujícím videu, kde Andrew Liu obsahuje stručný úvod k programovacím modelu DocumentDB na straně serveru databáze. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Vraťte se na tento článek kde se dozvíte odpovědi na následující otázky:  

- Jak psát uložené procedury, aktivační událost nebo UDF sešity pomocí Javascriptového?
- Jak DocumentDB zaručit kyseliny?
- Jak fungují transakce v DocumentDB?
- Co jsou předem spustí a po aktivaci a jak jednu psát?
- Jak zaregistrovat a spuštění uložené procedury, aktivační událost nebo UDF RESTful způsobem pomocí HTTP?
- Co jsou dostupné k vytváření a spouštění DocumentDB SDK uložené procedury, aktivačních událostí a funkce definované uživatelem?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Úvod k programování UDF a uložená procedura

Tento přístup *"JavaScript jako moderní den T-SQL"* uvolní vývojáře aplikací z složitostí překlepy systému a technologií objekt relační mapování. Je také počet vnitřní výhody, které můžete využít k vytváření bohaté aplikací:  

-   **Postupu použití logických operátorů:** JavaScript jako vysokou úroveň programovacího jazyka, poskytuje rozhraní propracovaných a známé vyjádřit obchodní logiky. Můžete provádět složité sekvence operací blíž k datům.

-   **Jednotlivé transakce:** DocumentDB zaručuje, že provádět operace databáze do jednoho uložené procedury nebo aktivační událost jsou atomová. Díky aplikaci kombinovat související operace v jedné dávce tak, aby všem poznámkám úspěšné nebo žádný z existujících úspěšné. 

-   **Výkonu:** K tomu, že JSON vnitřně namapovala systému typ jazyka Javascript a je také základní jednotka úložiště na DocumentDB umožňuje celá řada optimalizace jako pustí pobírala JSON dokumentů ve fondu vyrovnávací paměť a aby šlo je k dispozici okamžitou kód. Existují další výhody výkonu přidružené dodávky obchodní logiky pro databázi:
    -   Dávkové – vývojáři můžete seskupit operací, jako je vloží a odešlete je hromadně. Latence sítě přenosy nákladů a nároky úložiště vytvořit samostatné transakce jsou menší výrazně. 
    -   Před kompilace – DocumentDB předkompiluje aktivačních událostí uložené procedury a funkce definované uživatelem (UDF), abyste se vyhnuli JavaScript kompilace náklady pro každou vyvolání. Režijních sestavování bajt kód Procedurální logika amortizovaný minimální hodnotu.
    -   Řazení – mnoho operace potřebujete straně efekt ("aktivační událost"), které potenciálně zahrnuje jeden i více provádění operací sekundárním úložiště. Kromě nedělitelnost je to další performant při přesunutí na server. 
-   **Zapouzdření:** Uložené procedury mohou sloužit k seskupení obchodní logiky na jednom místě. To má dvě výhody:
    -   Přidá vrstvu odběru nad původní data, což umožňuje dat tvůrci vyvíjet žádostí o nezávisle data. Při je to zejména výhodné data jsou schématu bez, kvůli křehká předpokladů, které může být nutné peče do aplikace, pokud mají pro práci s daty přímo.  
    -   Tento odběru umožňuje podniky zabezpečit svá data tak, že zjednodušení přístupu z skriptů.  

Vytvoření a spuštění databáze, uložené procedury a operátorů vlastní dotaz je podporované prostřednictvím [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)a [klienta SDK](documentdb-sdk-dotnet.md) v mnoha platformách včetně .NET, Node.js a JavaScript.

**Že tento kurz používá [Node.js SDK s sliby otázky](http://azure.github.io/azure-documentdb-node-q/) ** ke znázornění syntaxe a použití te000126961 uložené procedury a funkce definované uživatelem aktivačních událostí.   

## <a name="stored-procedures"></a>Uložené procedury

### <a name="example-write-a-simple-stored-procedure"></a>Příklad: Psaní simple uložená procedura 
Začněme s jednoduché uložené procedury, která vrací odpovědi na "Ahoj světe".

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Uložené procedury registrovaných na kolekci a můžete pracovat na dokumentu a přílohy prezentovat v této kolekci. Následující úryvek ukazuje, jak si zaregistrovat Hello World uložené procedury s kolekcí. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Po registraci uložená procedura můžeme spouštět oproti kolekci a číst výsledků zpět na straně klienta. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Objekt kontextu poskytuje přístup k všechny operace, které lze provádět úložný prostor DocumentDB i přístupu k objektům žádostí a odpovědí. V tomto případě jsme použili objektu odpovědi nastavení textu, která byla odeslána klientovi odpověď. Další informace najdete v příručce [DocumentDB JavaScript serveru si přečtěte následující dokumentaci SDK](http://azure.github.io/azure-documentdb-js-server/).  

Dejte nám rozbalte v tomto příkladu, přidejte že další databáze související funkce uložené procedury. Uložené procedury můžete vytvářet, aktualizovat, přečtěte si, dotazu a odstraňovat dokumenty a příloh v kolekci.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Příklad: Psaní uložené procedury k vytvoření dokumentu 
Další fragment kódu ukazuje, jak objekt kontextu slouží k interakci s DocumentDB zdroje.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Uložená procedura používá jako vstupní documentToCreate textu dokument vytvořený v aktuální kolekci. Tyto operace jsou asynchronní a závisí na zpětná JavaScript (funkce). Funkce zpětné má dvěma parametry, jeden objektu chyb v případě, kdy se nezdaří a druhý pro vytvořený objekt. Uvnitř zpětné uživatelé mohou zpracovat výjimku nebo vyvolá chybu. V případě zpětné není k dispozici a dojde k chybě, modul runtime DocumentDB vyvolá chybu.   

Ve výše uvedeném příkladu zpětné vyvolá chybu, pokud nezdařila. V opačném nastaví id vytvořeného dokumentu v těle zprávy odpovědi klientovi. Tady je, jak spustit tento uložená procedura s vstupní parametry.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Všimněte si, že tato uložená procedura můžete upravit tak, aby udělat matice orgánů dokumentu předávat na vstupu a byla vytvořená na vše v rámci uložená procedura místo víc sítí požadavky na každý z nich vytvoření jednotlivě. Mohou sloužit k provedení efektivně hromadné import pro DocumentDB (popsáno dále v tomto kurzu).   

Příklad popsané prokázáno, jak používat uložené procedury. Budeme se zabývat těmito oblastmi aktivačními událostmi a funkce definované uživatelem (UDF) dál v tomto kurzu.

## <a name="database-program-transactions"></a>Databázové transakce programu
Transakce v databázi typické lze definovat jako posloupnost operací jako jednu jednotku logické práce. Každou transakci poskytuje **kyseliny záruky**. Kyselina je známý zkratka, která čtyři vlastnosti - nedělitelnost, konzistenci, izolace a životnost.  

Stručně řečeno, nedělitelnost zaručuje, že všechny práci uvnitř transakce zpracován jako jednu jednotku kde buď všechno velmi dbá nebo žádný. Soulad zajišťuje, aby data vždy její stav dobrý interní přes transakce. Izolace zaručuje, že žádný dvě transakce navzájem rušit – obecně, většina komerčních systémy poskytují víc úrovně izolace, které lze použít podle potřeb aplikace. Životnost zaručuje, že všechny změny, které vyvíjí v databázi vždy bude k dispozici.   

V DocumentDB JavaScript použitý ve stejném paměť jako databáze. Proto žádosti v rámci uložené procedury a aktivace spustit ve stejném oboru relace databáze. Díky DocumentDB zajistit kyseliny pro všechny operace, které jsou součástí jednu uložené procedury a aktivační událost. Zvažte následující definice uložené procedury:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Uložená procedura používá transakce v aplikaci hry a obchodních položek mezi dvěma přehrávače jedné operace. Uložená procedura pokusí přečíst dva dokumenty, které odpovídající jednotlivým ID player předaný jako argument. Pokud nástroj nalezne oba dokumenty Playeru, pak uložená procedura aktualizuje dokumenty výměna své položky. Jestliže se tím všechny chyby, ho výjimku JavaScript, implicitně přeruší transakci.

Pokud kolekce uložené procedury registrovaných proti je kolekce jedním oddílem a pak transakce má obor vymezený na všechny docuemnts v rámci kolekce. Pokud kolekci oddíly, uložené procedury spouštět v rozsahu transakce jeden oddíl klíče. Každý uložené spuštění procedury musí obsahovat potom oddíl klíčové hodnotu odpovídající obor transakce spuštěním ve skupinovém rámečku. Další podrobnosti najdete v tématu [Rozdělení DocumentDB](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Potvrzení a vrácení zpět
Transakce je příliš a nativně integrováno společnosti DocumentDB JavaScript programovací model. Uvnitř funkce JavaScript jsou všechny operace automaticky omotané v rámci jedné transakce. Jestliže JavaScript dokončí bez všech výjimek, jsou potvrzené operace k databázi. "Začít transakce" a "POTVRDIT transakce" příkazy v relační databáze jsou v podstatě implicitní DocumentDB.  
 
Pokud existuje všech výjimek, které rozšíří z skript, je DocumentDB JavaScript runtime vrátí celá transakce. Jak je vidět v předchozím příkladu, vyvolání výjimky odpovídá účinně "Transakce vrácení" v DocumentDB.
 
### <a name="data-consistency"></a>Konzistence dat
Uložené procedury a aktivace vždy zpracují na primární otevřené kolekci DocumentDB. Díky uložení čtení z uvnitř silných konzistence postupy nabídky. Dotazy, které používají uživatelsky definované funkce můžete provádět na primární a sekundární otevřené, ale můžeme zajistit podle úrovně požadovaný konzistence výběrem příslušných otevřené.

## <a name="bounded-execution"></a>Ohraničenou spuštění
Všechny operace DocumentDB muset v zadaném serveru požádat o vypršení časového limitu doby trvání. Toto omezení platí funkcí JavaScript (uložené procedury a aktivace funkcí definovaných uživatelem.). Pokud operaci nedokončí s této časový limit, transakce je vrátit zpět. Funkce JavaScript musí dokončení v rámci časový limit nebo provádět pokračování na základě modelu dávku/životopisu spuštění.  

Zjednodušení vývoj uložené procedury a aktivační události pro zpracování časové limity všechny funkce v části kolekce objektu (vytvoření, čtení, nahradit a odstranit dokumenty a příloh) vrátí logickou hodnotu, která představuje zda operaci dokončí. Pokud je tato hodnota false, je označení, že časový limit se blíží konec platnosti a postup musí být uveden nahoru spuštění.  Operace ve frontě před prvním operace nepřijatá úložiště je zaručena dokončete Pokud uložená procedura dokončení v čase a ve frontě dalších požadavků.  

Funkce JavaScript jsou také ohraničená na využití prostředků. DocumentDB si vyhrazuje výkon kolekce v závislosti na zřizování velikosti účtu databáze. Výkon vyjádřena pomocí celku normalizovanou procesoru, paměti a spotřeby vstupu a výstupu s názvem RUs nebo žádost o jednotek. Funkce JavaScript můžete potenciálně vyčerpat velkého počtu RUs během chvilky a může se zobrazit omezené sazba Pokud v kolekci limitu. Nároky uložené procedury zdroje může v karanténě také k zajištění dostupnosti operací základní databáze.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Příklad: Hromadného importu dat do databáze aplikace
Níže je příklad uložené procedury, která je aby došlo k zápisu hromadného importu dokumenty do kolekce. Všimněte si, jak uložená procedura zacházet s ohraničenou spuštění kontroly logická hodnota vrácená createDocument a potom pomocí počet dokumentů vložit do každého vyvolání uložené procedury sledovat a obnovení průběh mezi listy.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Aktivace databáze
### <a name="database-pre-triggers"></a>Před aktivační události databáze
DocumentDB obsahuje aktivační události, které jsou spouštět nebo spouštěný kliknutím operaci v dokumentu. Například určením před aktivační události při vytváření dokumentu – tato před aktivační událost se spustí, než bude vytvořena dokumentu. Následující obrázek je příkladem použití před aktivační události pro ověřování vlastnosti dokumentu, který se vytváří:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


A odpovídající kód klientský registrací Node.js aktivační události:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Před aktivace nemůže obsahovat žádné vstupní parametry. Objekt request mohou sloužit k manipulaci s nimi žádosti o zpráva související s operací. Tady, před aktivační událost je spuštěn při vytváření dokumentu a textu zprávy žádost obsahuje dokument, který chcete vytvořit ve formátu JSON.   

Když jsou registrované aktivačních událostí, uživatelé mohou zadat operace, které můžete spustit. Tato aktivační událost byl vytvořený pomocí TriggerOperation.Create, což znamená, že následující není povoleno.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Aktivace po databáze
Po aktivační události, jako je před aktivačních událostí souvisí s operaci dokumentu a si prohlédněte vstupních parametrů. Spustit **Po** prováděny operaci a mít přístup k zprávu s odpovědí odesílaný velkému klienta.   

Následující příklad ukazuje po aktivačních událostí v akci:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Aktivační událost můžete registrované, jak je vidět v následujícím příkladu.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Tato aktivační událost dotazy pro dokument metadat a aktualizuje s podrobnostmi o nově vytvořený dokument.  

Jednoho sloupce, který je důležité mít na paměti je **transakční** spuštění aktivačních událostí v DocumentDB. Po aktivační události spustí v rámci stejné transakce jako vytvoření původního dokumentu. Proto doporučujeme výjimku ze po aktivaci (například pokud nelze aktualizovat dokument metadata), celá transakce nebude a vrátit zpět. Vytvoří se žádný dokument a výjimky, bude vrácena.  

##<a id="udf"></a>Funkce definované uživatelem
Funkce definované uživatelem (UDF) slouží k rozšíření DocumentDB SQL dotazu jazyka gramatické a implementovat vlastní obchodní logiky. Může být s názvem pouze z uvnitř dotazů. Nemáte přístup k objektu kontext a jsou určeny má být použit jako jen pro využití JavaScript. Funkce definované uživatelem proto možné spouštět na vedlejší kopie službu DocumentDB.  
 
V následujícím příkladu vytvoří UDF vypočítat daň příjmů podle sazby pro různé závorky příjem a potom pomocí uvnitř dotazu zobrazíte všech lidí, kteří zaplacený víc než 20 000 Kč v daně.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


UDF následně lze v dotazech podobně jako v následujícím příkladu:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>Dotaz integrovaný jazyka JavaScript rozhraní API
Kromě vydání dotazy, které používají jeho DocumentDB SQL gramatiky, SDK serverovou vám umožní provést optimalizované dotazů pomocí rozhraní fluent JavaScript bez znalosti jazyka SQL. JavaScript dotaz, který rozhraní API umožňuje programově sestavování dotazů předáním predikátu funkce do chainable funkce spojí se syntaxí známé do pole built-ins a Oblíbené knihoven JavaScript jako lodash ECMAScript5 společnosti. Dotazy jsou analyzovat tak, že modul runtime JavaScript ke spuštění účinně používá jeho DocumentDB indexy.

> [AZURE.NOTE]`__` (dvojité podtržítko) je alias `getContext().getCollection()`.
> <br/>
> Jinými slovy, můžete použít `__` nebo `getContext().getCollection()` pro přístup k rozhraní API JavaScript dotazu.

Podporované funkce patří:
<ul>
<li>
<b>chain().... hodnota ([zpětné] [, možnosti])</b>
<ul>
<li>
Začíná value() zřetězené volání, které musí být ukončen.
</li>
</ul>
</li>
<li>
<b>Filtr (predicateFunction [, možnosti] [, zpětné])</b>
<ul>
<li>
Filtruje vstupní pomocí predikátu funkce, která vrací hodnotu true nebo false za účelem filtrování nebo rezervace vstupní dokumenty do výsledné sady. Tím se chová podobně jako klauzuli WHERE v jazyce SQL.
</li>
</ul>
</li>
<li>
<b>Mapa (transformationFunction [, možnosti] [, zpětné])</b>
<ul>
<li>
Slouží k použití projekci zadaných transformace funkci, která mapy položky vstupní JavaScript object nebo hodnotu. Tím se chová podobně jako klauzule SELECT v jazyce SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([položka název vlastnosti] [, možnosti] [, zpětné])</b>
<ul>
<li>
Toto je zástupcem mapu, který extrahuje ze položky vstupní hodnotu jedné vlastnosti.
</li>
</ul>
</li>
<li>
<b>Sloučit ([isShallow] [, možnosti] [, zpětné])</b>
<ul>
<li>
Sloučí a sloučí polí z jednotlivých zadávání položek v matici. Tím se chová podobně jako označit více v LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predikát] [, možnosti] [, zpětné])</b>
<ul>
<li>
Vytvořit novou sadu dokumentů seřazením dokumentů v toku vstupní dokument ve vzestupném pořadí pomocí zadaného predikát. Tím se chová podobně jako klauzule ORDER BY v SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predikát] [, možnosti] [, zpětné])</b>
<ul>
<li>
Vytvořit novou sadu dokumentů seřazením dokumentů v toku vstupní dokument v sestupném pořadí pomocí zadaného predikát. Tím se chová podobně jako klauzule ORDER BY x DESC v SQL.
</li>
</ul>
</li>
</ul>


Pokud je součástí uvnitř predikát a/nebo výběr funkcí, následující konstrukty JavaScript získat automaticky optimalizováno pro spuštění přímo na DocumentDB indexy:

* Jednoduchý operátory: = +, -, * nebo % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literály, včetně literál typu objektu: {}
* var return

Následující konstrukty JavaScript získat nejsou optimalizované pro DocumentDB indexy:

* Řízení toku (například pokud, zatímco)
* Volání funkce

Další informace najdete v článku naší [JSDocs straně serveru](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Příklad: Napište proceduru uloženou pomocí rozhraní API dotazu JavaScript

Následující příklad kódu je příklad použití rozhraní API dotazu JavaScript v kontextu uložená procedura. Uložená procedura vloží dokumentu dán vstupní parametry a aktualizuje metadat dokumentu pomocí `__.filter()` metodou s minSize maxSize a totalSize založený na vlastnost vstupní dokumentu velikost.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>Abyste pomocný Javascript
V následující tabulce jsou uvedeny různých SQL dotazy a odpovídající JavaScript.

Jak s dotazy SQL, dokumentů vlastnost klíče (například `doc.id`) jsou malá a velká písmena.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>Rozhraní API JavaScript dotazu</th>
<th>Podrobnosti</th>
</tr>
<tr>
<td>
<pre>
Vyberte * z dokumentů
</pre>
</td>
<td>
<pre>
__.map(Function(DOC) {zpáteční dokumentu;});
</pre>
</td>
<td>Výsledky ve všech dokumentech (čísla stránek vložena s pokračování token), jako je.</td>
</tr>
<tr>
<td>
<pre>
Vyberte docs.id, docs.message jako msg docs.actions z dokumentů
</pre>
</td>
<td>
<pre>
__.map(Function(DOC) {vrátit {id: doc.id, zpráva: doc.message, akce: doc.actions};});
</pre>
</td>
<td>Projekty id, zprávy (alias pro msg) a akci z všechny dokumenty.</td>
</tr>
<tr>
<td>
<pre>
Vyberte * z dokumentů WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(DOC) {vrátit doc.id === "X998_Y998;"});
</pre>
</td>
<td>Dotazy týkající se dokumentů s predikát: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Vyberte * z dokumentů kde ARRAY_CONTAINS(docs. Značky, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {vrátit x.Tags & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Dotazy týkající se dokumentů, které mají vlastnost značky a značky je pole obsahující hodnotu 123.</td>
</tr>
<tr>
<td>
<pre>
Vyberte docs.id, docs.message jako dokumenty od msg WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {vrátit doc.id === "X998_Y998;"}) .map(function(doc) {vrátit {id: doc.id, zpráva: doc.message};}) .value();
</pre>
</td>
<td>Dotazy týkající se dokumentů s predikátem, id = "X998_Y998" a potom projekty id a zprávy (alias pro msg).</td>
</tr>
<tr>
<td>
<pre>
Značka vybrat hodnotu z dokumentů spojení značku v dokumenty. Značky Order docs._ts
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {zpáteční dokumentu. Značky & & Array.isArray (dokumentu. Značky); {return doc._ts;}}) .sortBy(function(doc)) .pluck("Tags") .flatten() .value()
</pre>
</td>
<td>Filtry pro dokumenty, které mají maticových vlastnosti, značky a seřadí výsledné dokumenty _ts časové razítko systém vlastnosti a pak projekty + sloučí pole značky.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Podpora modulu runtime
[Na straně serveru DocumentDB JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) podporuje nejvíce hlavní fázi technické funkcí jazyka JavaScript jako standardizovaným tak, že [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Zabezpečení
JavaScript uložené procedury a aktivace jsou v izolovaném prostoru tak, aby efekty jeden skript není proniknout k druhému bez opakovaného izolace transakce snímek na úrovni databáze. Prostředí runtime jsou ve fondu ale vyčistit kontextu po jednotlivých spustit. Proto jsou zaručené za bezpečné před nechtěným účinky straně od sebe.

### <a name="pre-compilation"></a>Před kompilace
Uložené procedury aktivačních událostí jsou a funkce definované uživatelem implicitně neaktivním do formátu bajt kód a zabránit tak kompilace náklady v době každý vyvolání skriptu. Díky vyvolání uložené procedury rychlá a mít nízké nároky.

## <a name="client-sdk-support"></a>Podpora klientů SDK
Kromě klientovi [Node.js](documentdb-sdk-node.md) DocumentDB podporuje [.NET](documentdb-sdk-dotnet.md) [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)a [Python SDK](documentdb-sdk-python.md). Uložené procedury, aktivačních událostí a funkce definované uživatelem se dají vytvářet a spouštět pomocí kterékoli z těchto SDK stejně. Následující příklad ukazuje, jak k vytváření a spouštění uložené procedury pomocí klienta .NET. Všimněte si, jak předaným uložená procedura jako JSON a číst typy .NET.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Tento příklad ukazuje, jak používat [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) k vytvoření před aktivační události a vytváření dokumentu pomocí aktivační události povolené. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


A následující příklad ukazuje, jak vytvořit funkce definované uživatelem (UDF) a použijte ji v [dotazu DocumentDB SQL](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>ROZHRANÍ REST API
Všechny operace DocumentDB lze provést RESTful způsobem. Uložené procedury a aktivace funkcí definovaných uživatelem můžete registrovány v části kolekce pomocí HTTP příspěvek. Následujícím obrázku je příklad toho, jak si zaregistrovat uložené procedury:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Uložené procedury registrovaných spuštění žádost příspěvek proti URI dbs/testdb/colls/testColl/sprocs pomocí textu obsahující uložené procedury k vytvoření. Aktivace a funkce definované uživatelem můžete podobně registrované zadáním do příspěvku proti/aktivační události a /udfs v tomto pořadí.
Uložená procedura se dají potom prováděnou vystavení příspěvku požadavku před jeho odkaz zdroje:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Tady předávat na vstupu uložená procedura předán v hlavním textu žádosti o. Všimněte si, že zadání předané jako maticové JSON vstupních parametrů. Uložená procedura používá první vstupní jako dokument, který je textu odpověď. Odpověď, kterou jsme přijímání vypadá takto:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Aktivační události na rozdíl od uložené procedury nelze provést přímo. Místo toho jsou spouštět jako součást operace dokumentu. Můžeme určit aktivační události pro práci s žádost o použití HTTP záhlaví. Následující obrázek je žádost o vytvoření dokumentu.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Tady je před aktivační události pro práci s žádosti podle x-ms-documentdb-pre-trigger-include záhlaví. Po aktivace odpovídajícím způsobem, jsou uvedeny v x-ms-documentdb-post-trigger-include záhlaví. Všimněte si, že pravopis před a po aktivace mohou být zadány pro daný požadavek.

## <a name="sample-code"></a>Ukázkový kód

Další příklady kódem na straně serveru (včetně [hromadného odstranění](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)a [Aktualizovat](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) můžete najít na naše [Github úložiště](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Chcete sdílet Super uloženou procedurou? Pošlete nám prosím žádost vyžádané! 

## <a name="next-steps"></a>Další kroky

Až budete mít nejméně jeden uložené procedury aktivačních událostí a uživatelem definované funkce vytvořené, můžete je načíst a tyto informace zobrazit v portálu Azure pomocí skriptu Průzkumníka. Další informace najdete v tématu [zobrazení uložené procedury, aktivačních událostí a funkcí definovaných uživatelem pomocí Průzkumníka skript DocumentDB](documentdb-view-scripts.md).

Také užitečné následující odkazy a materiály do pole Cesta zobrazíte další informace o DocumentDB serverovou programování:

- [Azure DocumentDB SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [262 JavaScript ECMA](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON typ systému](http://www.json.org/js.html) 
- [Rozšiřitelnost zabezpečené přenosných a stolních databáze](http://dl.acm.org/citation.cfm?id=276339) 
- [Služba určený architektura databáze](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Hostování modulu Runtime .NET v Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)
