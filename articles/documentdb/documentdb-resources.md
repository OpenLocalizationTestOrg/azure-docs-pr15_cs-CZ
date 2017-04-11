<properties 
    pageTitle="DocumentDB model hierarchické prostředků a pojmy | Microsoft Azure" 
    description="Informace o společnosti DocumentDB hierarchické modelu databáze kolekce, funkce definované uživatelem (UDF), dokumenty, oprávnění ke správě zdrojů a další."
    keywords="Hierarchický modelu, documentdb azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Model hierarchické prostředků DocumentDB a koncepty

Entity databáze, které spravuje DocumentDB se označují jako **zdroje**. Jednotlivé zdroje je jedinečně identifikovány logické URI. Můžete komunikovat se zdroji pomocí standardních operací protokolu HTTP, žádostí a odpovědí záhlaví a stavů. 

Načtením v tomto článku si budete moct odpovězte na následující otázky:

- Co je na DocumentDB zdroje model?
- Jaké jsou systémové definované zdroje na rozdíl od zdrojů definované uživatelem?
- Jak zadám adresy zdroje?
- Práce s kolekcemi?
- Práce s aktivačními událostmi uložené procedury a uživatelsky definované funkce (UDF)?

## <a name="hierarchical-resource-model"></a>Model hierarchické prostředků
Jak ukazuje následující obrázek hierarchické DocumentDB **modelu zdroje** se skládá z sady zdrojů v části účet databázi, každá s možností zadání prostřednictvím logické a stabilní URI. Nastavení zdroje označenou jako informačního **kanálu** v tomto článku. 

>[AZURE.NOTE] DocumentDB nabízí vysoce efektivní protokolu TCP, což je také RESTful v jeho komunikace model dostupné prostřednictvím [.NET klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).

![Model DocumentDB hierarchické prostředků][1]  
**Model hierarchické prostředků**   

Pokud chcete začít pracovat s prostředky, musíte [vytvořit účet DocumentDB databáze](documentdb-create-account.md) pomocí předplatného Azure. Účet databáze může být tvořen sadu **databází**, každý obsahující víc **kolekcí**, z nichž každá zase obsahovat **uložené procedury, spustí, funkce definované uživatelem, dokumenty** a související **přílohy** (funkce Náhled). Databáze také přidruženou **uživatelů**, oba objekty mají úroveň **oprávnění** pro přístup k kolekce, uložené procedury, aktivačních událostí, funkce definované uživatelem, dokumenty nebo přílohy ve formě. Zatímco databází, uživatele, oprávnění a kolekcí jsou definované systém zdroje s známý schématy, dokumenty a přílohy obsahují libovolné, JSON obsahu definované uživatelem.  

|Zdroje   |Popis
|-----------|-----------
|Účet databázi   |Účet databáze je přidružený sadu databází a pevný počet úložiště objektů blob pro přílohy (funkce Náhled). Můžete vytvořit jednu nebo více účtů databáze pomocí předplatného Azure. Další informace získáte v naší [ceny stránky](https://azure.microsoft.com/pricing/details/documentdb/).
|Databáze   |Databázi je kontejner logických ukládání dokumentů na oddíly v kolekcích. Je taky kontejneru uživatelů.
|Uživatel   |Logické obor názvů definování oprávnění. 
|Oprávnění |Token se tak mohli ověřovat spojené s uživatelem pro přístup k určitému zdroji.
|Kolekce |Kolekce je kontejner JSON dokumentů a přidružené aplikace logickou JavaScript. Kolekce je fakturaci entitu, kde je určen [náklady](documentdb-performance-levels.md) úroveň výkonu související s odběrem. Kolekce může zahrnovat jeden nebo více oddílů/servery a lze přizpůsobit zpracovat prakticky neomezené množství úložiště nebo výkon.
|Uložená procedura   |Logika aplikace napsané v JavaScript, které je registrovaný u kolekce a využití transakce spouštět v rámci databázový stroj.
|Aktivační událost    |Logika aplikace napsaná v JavaScriptu spouštět před nebo za buď vložit, nahrazení nebo odstranění operace.
|UDF    |Logika aplikace napsané v JavaScriptu. Funkce definované uživatelem umožňují modelovat operátor vlastní dotaz a tím rozšířit základní DocumentDB dotazovací jazyk.
|Dokument   |Definované uživatelem (libovolného) JSON obsah. Ve výchozím nastavení žádné schéma musí být definované ani sekundárním indexy potřebují být k dispozici pro všechny dokumenty přidané do kolekce.
|(Verze preview) Přílohy   |Přílohy je speciální dokument obsahující odkazy a přidružená metadata pro externí objektů blob/média. Vývojář můžete mít objektů blob spravuje DocumentDB nebo uložit u poskytovatele služeb externí objektů blob jako je OneDrive, Dropbox atd. 


## <a name="system-vs-user-defined-resources"></a>Systém porovnání zdroje definované uživatelem
Zdroje, jako jsou databáze účty, databází, kolekcí, uživatele, oprávnění, uložené procedury, aktivačních událostí a funkce definované uživatelem – mít pevné schématu a nazývají systémové prostředky. Zdroje, jako jsou dokumenty a příloh naopak máte bez omezení na schéma a příkladů uživatelem definované zdroje. V DocumentDB systém a uživatelem definované zdrojů představující a spravovat jako standardní požadavkům JSON. Všechny zdroje, systému nebo uživatelem definované, mají následující vlastnosti.

> [AZURE.NOTE] Poznámka: aby všechny vygenerovaný vlastnosti ve zdroji jsou předponou podtržítka (_) v jejich reprezentaci JSON.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Vlastnost</strong></p></td>
            <td valign="top"><p><strong>Nastavit uživatele nebo generovat systém?</strong></p></td>
            <td valign="top"><p><strong>Účel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Generovat systém</p></td>
            <td valign="top"><p>Generování systémem, jedinečný a hierarchických identifikátor zdroje</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Generovat systém</p></td>
            <td valign="top"><p>etag vyžadovaná pro ovládací prvek optimistické souběžné</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Generovat systém</p></td>
            <td valign="top"><p>Poslední aktualizované časové razítko ze zdroje</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Generovat systém</p></td>
            <td valign="top"><p>Jedinečný identifikátor URI s možností zadání zdroje</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Generovat systém</p></td>
            <td valign="top"><p>Uživatelem definované jedinečný název zdroje (se stejnou hodnotou oddíl klíče). Pokud uživatel není zadán id, id budou generovat systém</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Znázornění drátěný zdrojů
DocumentDB není pověřit vlastní rozšíření JSON standardní nebo jinak kódování; funguje s standardní kompatibilní se standardem JSON dokumenty.  
 
### <a name="addressing-a-resource"></a>Adresování zdroje
Všechny zdroje jsou URI s možností zadání. Hodnota vlastnosti **_self** zdroje představuje relativní identifikátor URI zdroje. Formát identifikátor URI se skládá z /\<kanálu\>/ segmenty cesty {_rid}:  

|Hodnota _self |Popis
|-------------------|-----------
|/DBS   |Informační kanál databází pod účtem databáze
|/DBS/ {dbName}  |Databáze s kódem párování hodnotu {dbName}
|/colls/ /DBS/ {dbName}   |Informační kanál kolekcí v části databáze
|/colls/ /DBS/ {dbName} {collName} |Kolekce s kódem párování hodnotu {collName}
|/colls/ /DBS/ {dbName} {collName} / dokumenty    |Informační kanál dokumentů v části kolekce
|/docs/ /colls/ {collName} /DBS/ {dbName} {docId}    |Dokument s kódem párování hodnotu {dokumentu}
|/users/ /DBS/ {dbName}   |Informační kanál uživatelů v části databáze
|/users/ /DBS/ {dbName} {ID}   |Uživatel s kódem párování hodnotu {uživatele}
|/users/ /DBS/ {dbName} {ID} / oprávnění   |Informační kanál oprávnění v části uživatele
|/permissions/ /users/ {ID} /DBS/ {dbName} {permissionId}    |Oprávnění s kódem párování hodnotu {oprávnění}
  
Jednotlivé zdroje obsahuje název jedinečný uživatelem definované přístupné pomocí vlastnosti id. Poznámka: u dokumentů, pokud uživatel není zadat id, náš podporované SDK automaticky vygeneruje jedinečné id dokumentu. Id je řetězec definované uživatelem, až 256 znaků, které je jedinečný v rámci konkrétní nadřazené zdroje. 

Jednotlivé zdroje také generovat systém hierarchické zdroje identifikátor je (označuje také jako server relativních ID), která je k dispozici prostřednictvím _rid vlastnosti. Serveru relativních ID kódování celou hierarchii daný zdroj a je vhodné vnitřní podoba použit k jejímu vynucení referenční integrity distribuované způsobem. Serveru relativních ID je jedinečný v rámci účet databáze a ho interně používá DocumentDB pro efektivní směrování bez nutnosti křížového oddíl vyhledávání. Hodnoty _self a _rid vlastnosti jsou formátovaná jako alternativní a kanonický zdroje. 

Rozhraní REST API DocumentDB podporují adresování zdrojů a směrování žádostí o podle id a vlastnosti _rid.

## <a name="database-accounts"></a>Účty databáze
Můžete vytvořit jednu nebo více DocumentDB databáze účty pomocí předplatného Azure.

Můžete [vytvořit a spravovat účty databáze DocumentDB](documentdb-create-account.md) prostřednictvím portálu Microsoft Azure na [http://portal.azure.com/](https://portal.azure.com/). Vytváření a Správa účtu databáze vyžaduje přístup pro správu a lze provést pouze v rámci předplatného Azure. 

### <a name="database-account-properties"></a>Vlastnosti účtu databáze
Jako součást vytváření a Správa účtu databáze můžete nakonfigurovat a přečtěte si následující vlastnosti:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Název vlastnosti</strong></p></td>
            <td valign="top"><p><strong>Popis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Soulad zásad</p></td>
            <td valign="top"><p>Nastavte tuto vlastnost na konfigurovat výchozí úroveň konzistence pro všechny kolekce pod svým účtem databáze. Je možné přepsat úroveň konzistence na základě na žádost o pomocí hlavičky [x ms konzistence úrovně]. <p><p>Všimněte si, že tato vlastnost platí jenom pro <i>zdroje definované uživatelem</i>. Všechny definovaný konfigurace podporuje čtení/vyhledávání systému s silných konzistence.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Povolení klíče</p></td>
            <td valign="top"><p>Toto jsou primárních a sekundárních předlohy a jen pro čtení klávesy, pomocí kterých poskytnout přístup pro správu u všech zdrojů v části účet databázi.</p></td>
        </tr>
    </tbody>
</table>

Všimněte si, že kromě zřizování, konfigurace a Správa účtu databáze z portálu Microsoft Azure, můžete také programově vytvářet a spravovat účty DocumentDB databáze pomocí [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) , stejně jako [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Databáze
Databáze DocumentDB je kontejner logické jednu nebo více kolekcí a uživatelům, jak je znázorněno na následujícím obrázku. Můžete vytvořit libovolný počet databází pod účtem DocumentDB databáze vyměřené poplatky za jeho omezení nabídky.  

![Účet a kolekcí hierarchické modelu databáze][2]  
**Databázi je kontejner logické uživatelů a kolekce**

Databáze může obsahovat úložiště neomezený dokumentů oddíly kolekce, které tvoří domény transakce pro dokumenty obsažené v nich. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Pružná měřítka DocumentDB databáze
Databáze DocumentDB je pružná ve výchozím nastavení – od několika GB petabytes SSD záložní ukládání dokumentů a zřizování výkon. 

Na rozdíl od databáze v tradiční RDBMS databáze v DocumentDB není obor vymezený na jednom počítači. Pomocí DocumentDB vaše aplikace měřítko potřeb rozšířit, vytvoříte více kolekcí nebo databáze. Nakonec různé první strany aplikace v Microsoft jste používali DocumentDB ve velkém měřítku spotř vytvořením velmi velké databáze DocumentDB každý obsahující tisíce kolekce s TB ukládání dokumentů. Můžete zvětšit nebo zmenšit databáze přidáním nebo odebráním kolekce požadavkům měřítko vaše aplikace. 

Můžete vytvořit libovolný počet kolekcí v databázi vyměřené poplatky za jeho nabídky. Jednotlivé kolekce má SSD záložní úložiště a výkon k dispozici v závislosti na vybrané výkonu osy.

Databáze DocumentDB je také kontejneru uživatelů. Uživatel může v zapnout, je logické obor názvů pro sadu oprávnění, která poskytuje jemně odstupňovaná oprávnění a přístup k kolekce, dokumenty a přílohy.  
 
Stejně jako u dalších zdrojů v modelu zdroje DocumentDB databáze se dají vytvářet, nahradit, odstranit, číst nebo vytvořit výčet snadno pomocí [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo některou z [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB zaručuje silných konzistenci pro čtení nebo dotazování metadat zdroj databáze. Odstranění databáze automaticky zaručuje, že nebudete mít přístup kolekce nebo uživatelů obsažené v něm obsažené.   

## <a name="collections"></a>Kolekce
Kolekce DocumentDB je kontejner pro svoje dokumenty JSON. Kolekce je také jednotka měřítko pro transakce a dotazů. 

### <a name="elastic-ssd-backed-document-storage"></a>Pružná SSD záložní ukládání dokumentů
Kolekce vnitřně pružná - automaticky roste a zmenší se jenom při přidání nebo odebrání dokumentů. Kolekce je logické zdrojů a může zahrnovat fyzické oddíly nebo servery. Počet oddílů v rámci kolekce je určený podle DocumentDB na základě velikosti úložiště a zřizování výkon kolekce webů. Každý oddíl DocumentDB má pevný počet záložní SSD úložiště přidružená a replikovat vysoké dostupnosti. Oddíl Správa plně spravuje Azure DocumentDB a nemáte složité kódu nebo spravovat oddílů. Kolekce DocumentDB jsou **prakticky neomezený** z hlediska úložiště a výkon. 

### <a name="automatic-indexing-of-collections"></a>Automatické indexování kolekce
DocumentDB je systém true uvolnit schématu databáze. Nemá předpokládá nebo vyžadovat libovolné schéma pro dokumenty JSON. Při přidávání dokumentů do kolekce DocumentDB automaticky indexy a jsou k dispozici pro vás k vytvoření dotazu. Automatické indexování dokumenty bez nutnosti schématu nebo vedlejší indexy je klíčové funkce DocumentDB a povolit tak, že index optimalizované zapsat, uvolněte lock a strukturovanými protokolu údržbu technik. DocumentDB podporuje trvalý objemu velmi rychlý zápisy při pořád poskytování konzistentní dotazů. Ukládání dokumentů a index slouží k výpočtu úložiště využívané každé kolekce. Můžete určit, ukládání a výkonu střídání přidružené indexování pomocí konfigurace indexování zásad pro kolekci. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Konfigurace indexování zásad kolekce
Indexování zásad každé kolekce webů je možné provádět výkon a úložiště střídání přidružené indexování. Jsou dostupné jako součást indexování konfigurace následující možnosti:  

-   Zvolte, jestli kolekci automaticky indexy všechny dokumenty nebo ne. Ve výchozím nastavení jsou všechny dokumenty automaticky indexovat. Můžete vypnout automatické indexování a selektivním přidat jenom určité dokumenty do indexu. Naopak můžete selektivním chcete vyloučit jenom určité dokumenty. Můžete dosáhnout nastavení automatické vlastnost na hodnotu true nebo false na indexingPolicy kolekce a použití hlavičky [x-ms-indexingdirective] při vkládání, nahrazení nebo odstranění dokumentu.  
-   Vyberte, jestli chcete zahrnout nebo vyloučit konkrétní cesty nebo vzorků v dokumentech z indexu. Můžete dosáhnout toto nastavení includedPaths a excludedPaths na indexingPolicy kolekce v tomto pořadí. Můžete taky nakonfigurovat úložiště a výkonu střídání pro oblast a hash dotazů pro určitou cestu vzorků. 
-   Volba mezi synchronní (konzistentní) a aktualizací asynchronní index (pustí). Index je ve výchozím nastavení synchronní aktuální informace o jednotlivých vložení, nahrazení nebo odstranění dokumentu v kolekci. Díky dotazů přijmout stejné úrovni konzistence jako čtení dokumentu. Během DocumentDB je zapsat optimalizované a podporuje trvalý svazky o zápisů dokument společně s údržbu synchronní index a poskytování konzistentní dotazů, můžete nakonfigurovat některé kolekce pomalu aktualizovat jejich index. Pustí indexování zvyšuje výkon zápisu dál a je ideální pro hromadné požití scénáře Collections primárně těžké pro čtení.

Indexování zásad bude možné měnit spuštěním umístění v kolekci. Toho můžete dosáhnout buď přes [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx) [Portál Azure](https://portal.azure.com) nebo [Azure DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Dotazování kolekce
Dokumenty v rámci kolekce můžete mít libovolného schémata a dotaz dokumenty v rámci kolekce bez zadání schématu nebo vedlejší indexy předem. Můžete dotaz kolekci pomocí [syntaxe jazyka SQL DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)poskytující RTF hierarchické, relační a prostorové operátory a rozšíření prostřednictvím na základě JavaScript funkce definované uživatelem. JSON gramatiky umožňuje modelování JSON dokumentů jako stromy s popisky jako uzly stromu. To je zneužil jak DocumentDB jeho automatické indexování technik, jakož i dialektu DocumentDB na SQL. Dotazovací jazyk DocumentDB tvoří tři hlavní aspekty:   

1.  Kratších seznamů operací dotazu, které mají přirozený namapovat stromové struktuře včetně hierarchické dotazů a prognózy. 
2.  Podmnožinu relační operace včetně složení filtr, průzkumy, agregace a vlastní spojení. 
3.  Čistý JavaScriptu na základě funkce definované uživatelem, které spolupracují s (1) a (2).  

Model dotazu DocumentDB pokusí narazila poměr funkce, efektivity a zjednodušení. Databázový stroj DocumentDB nativně kompilaci a spustí dotaz příkazy SQL. V kolekci pomocí rozhraní [REST API DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo některou z [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)můžete dotaz. .NET SDK získáváte LINQ poskytovatele.

> [AZURE.TIP] Můžete vyzkoušet DocumentDB a spouštění dotazů SQL týkající se naší sady dat v [Dotazu hřišť](https://www.documentdb.com/sql/demo).

### <a name="multi-document-transactions"></a>Transakce složené z více dokumentů
Databázové transakce poskytnout bezpečných a předvídatelná programovací model týkající se souběžné změny data. Tradiční způsob psát obchodní logiky v RDBMS, je psaní **uložené procedury** a/nebo **aktivačních událostí** a dodávat k databázovému serveru transakční spuštění. V RDBMS je nutné programátor aplikace zacházet s dvěma různými jazyky: 

- (Transakční) aplikace programovací jazyk (například JavaScript Python, C#, Java, atd.)
- T-SQL, transakční programovací jazyk, který je nativně spouštět v databázi

Na základě hloubkové zavázala JavaScript a JSON přímo v rámci databázový stroj DocumentDB poskytuje intuitivní programovací model pro provádění logických aplikace JavaScriptu na základě přímo na kolekce z hlediska uložené procedury a aktivace. Tato funkce umožňuje obě následující akce:

- Efektivní provádění souběžné řídit, obnovení automatické indexování grafů objektu JSON přímo v databázový stroj
- Přirozeným vyjádření řízení toku, proměnné definování rozsahu, přiřazení a integrace zpracování základní s databázové transakce přímo z hlediska programovacího jazyka JavaScript výjimek

Logika JavaScript registrovaná na úrovni kolekce můžete vydá s dokumenty v dané kolekci operace databáze. DocumentDB implicitně zalamuje JavaScriptu na základě uložené procedury a aktivačních událostí v rámci okolního kyseliny transakcí s snímek izolace mezi dokumenty v rámci kolekce. V průběhu spuštění Pokud JavaScript výjimku, potom přerušení celé transakce. Výsledný programovacího modelu je velmi jednoduché výkonné. Vývojáři JavaScript získání "trvalá" programovací model při stále používáte jejich přeložený konstrukce a základní knihovny.   

Možnost Spustit JavaScript přímo v rámci databázový stroj do pole Adresa jako fondu vyrovnávací paměť umožňuje performant a transakční provádění operací databázi proti dokumenty kolekce. Kromě toho DocumentDB databázový stroj zajišťuje hloubkové závazku ve formátu JSON a JavaScript eliminuje jakékoli omezení neshodu mezi systémy typ aplikace a databáze.   

Po vytvoření kolekce, můžete s kolekcí pomocí rozhraní [REST API DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo některou z [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)zaregistrovat aktivačních událostí uložené procedury a funkce definované uživatelem. Po registraci můžete odkázat a spouštět je. Zvažte následující uložená procedura úplně psaných JavaScript, následující kód se zadávají dva argumenty (název knihy a jméno autora) a vytvoří nový dokument, dotazy pro dokument a potom aktualizuje ho – vše v rámci implicitní kyseliny transakce. Kdykoli během provádění Pokud je k výjimce JavaScript, celý transakce přeruší.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Klient můžete "dodat" nahoře logickou JavaScript k databázi transakční spuštění prostřednictvím protokolu HTTP příspěvek. Další informace o používání HTTP metody najdete v článku [RESTful interakce s DocumentDB zdroje](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Všimněte si, protože databázi nativně rozumí JSON a JavaScript, není žádný typ systému neshodu, "OR mapování" nebo kód generování magické povinné.   

Uložené procedury a aktivace komunikovat s kolekce a dokumentům v kolekci prostřednictvím dobře definovaný objektový model, která poskytuje aktuální kontext kolekce.  

Kolekcí v DocumentDB lze vytvořit, odstranit, přečtěte si nebo výčtu snadno pomocí rozhraní [REST API DocumentDB Azure](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo některou z [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB vždy poskytuje silných konzistence pro čtení nebo dotazování metadata kolekce. Odstranění kolekce automaticky zajistí, že nebudete mít přístup na dokumenty přílohy, uložené procedury, aktivačních událostí a funkce definované uživatelem obsažené v něm obsažené.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Uložené procedury, aktivačními událostmi a uživatel definované funkce (UDF)
Podle popisu v předchozí části, můžete napsat aplikace logiku spuštění přímo v rámci transakce uvnitř databázový stroj. Logika aplikace může být psaných úplně JavaScript a můžete modelovat jako uložené procedury, aktivační událost nebo uživatelem definovanou FUNKCI. Kód JavaScript v rámci uložené procedury nebo aktivační můžete vložit, nahradit, odstranění, dokumentů pro čtení nebo dotazu v rámci kolekce. Na druhé straně JavaScript v rámci uživatelem definovanou FUNKCI nelze vkládat, nahradit nebo odstraňovat dokumenty. Funkce definované uživatelem umožňuje zobrazit výčet dokumenty sadu výsledků dotazu a nezpůsoboval tak jinou sadu výsledků. Pro víceklientská vynutí DocumentDB zásady správného řízení zdrojů striktně rezervaci na základě. Každý uložené procedury, aktivační událost nebo uživatelem definovanou FUNKCI získá pevných kvanta operační systém prostředků svou práci. Kromě toho uložené procedury, aktivace nebo funkce definované uživatelem nelze vytvořit propojení proti externí JavaScript knihoven a jsou zakázány pokud překročí rozpočty zdroje přidělit je. Můžete zaregistrovat, unregister uložené procedury, aktivace nebo funkce definované uživatelem s kolekcí pomocí rozhraní REST API.  Při registraci je uložená procedura, aktivační událost nebo uživatelem definovanou FUNKCI předem kompilovaný a uložená jako bajt kód, který získá provést později. V následující části ukazují, jak můžete používat DocumentDB JavaScript SDK registrovat, spustit a unregister uživatelem definovanou FUNKCI, uložené procedury a aktivační událost. Jednoduchý obal na JavaScript SDK překročení [DocumentDB REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Registrace uložená procedura
Registrace uložené procedury vytvoříte nový zdroj uložená procedura kolekce prostřednictvím protokolu HTTP příspěvek.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Spuštění uložené procedury
Spuštění uložené procedury vystavila společnost vydání HTTP POST proti existující zdroj uložená procedura předáním parametrů pomocí postupu v žádosti o textu.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Registrace uložená procedura
Registrace uložená procedura jednoduše vystavila společnost vydání HTTP odstranění se před existující uložená procedura zdroj.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registrace před aktivační událost
Registrace aktivační vystavila společnost vytvoření nového prostředku aktivační události pro kolekci prostřednictvím protokolu HTTP příspěvek. Můžete zadat Pokud aktivační událost, je pre nebo aktivační příspěvku a typ operace může být přidružené k (například vytvořit, nahradit, odstranit nebo všechny).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Spuštění před aktivační událost
Provádění aktivační vystavila společnost určující název aktivační události pro existující v době vystavení příspěvku, umístění nebo odstranění požadavku zdroje dokumentů pomocí hlavičky.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Registrace před aktivační událost
Registrace aktivační jednoduše probíhá prostřednictvím vydání HTTP odstranění se před existující zdroj aktivační událost.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registrace uživatelem definovanou FUNKCI
Registrace uživatelem definovanou FUNKCI vystavila společnost vytvoření nového prostředku UDF kolekce prostřednictvím protokolu HTTP příspěvek.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Spuštění UDF jako součást dotazu
Uživatelem definovanou FUNKCI může být zadán jako součást dotaz SQL zobrazený a slouží jako způsob, jak rozšířit základní [dotazu jazyka SQL DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Registrace uživatelem definovanou FUNKCI 
Registrace uživatelem definovanou FUNKCI jednoduše vystavila společnost vydání HTTP odstranění se před existující UDF zdroj.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

I když fragmenty výše ukázal registrace (publikovat), zrušení registrace (Vložit), čtení/seznamu (GET) a spuštění (POST) prostřednictvím [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js), můžete také rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) nebo jiného [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumenty
Můžete vložit, nahradit, odstranit, přečtěte si, umožňuje zobrazit výčet a dotaz libovolného JSON dokumenty v kolekci. DocumentDB není pověřit libovolné schéma a nevyžaduje sekundární indexy pro podporu dotazování na dokumenty v kolekci.   

Jsou služby opravdu otevřenou databázi, DocumentDB není zásob žádné speciální datových typů (například datum čas) nebo konkrétní kódování JSON dokumentů. Všimněte si, že DocumentDB nevyžaduje žádné zvláštní konvence JSON k kodifikovat vztahy mezi různé dokumenty; Syntaxe jazyka SQL DocumentDB obsahuje hodně výkonná hierarchické a relační dotaz operátorů k dokumentům dotazu a projektu bez zvláštní poznámky nebo třeba kodifikovat vztahy mezi dokumentů pomocí rozlišit vlastnosti.  
 
Jako s dalších zdrojů dokumenty lze vytvářet nahrazený odstraněné, přečtěte si, výčtu a dotazovaném snadno pomocí rozhraní REST API nebo některou z [klienta SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). Odstranění dokumentu okamžitě uvolní kvóty odpovídající u všech vnořených příloh. Úrovně čtení konzistence dokumentů následuje zásady konzistence na účet databázi. Na základě požadavků v závislosti na požadavky konzistence dat aplikace mohou přepsat tuto zásadu. Při dotazování dokumentů, čtení konzistence sleduje indexování režimu nastavení v kolekci. Pro "konzistentní" to následuje zásady konzistence účtu. 

## <a name="attachments-and-media"></a>Přílohy a média
>[AZURE.NOTE] Přílohy a média zdroje jsou funkce.
 
DocumentDB umožňuje ukládat binární objektů BLOB/médium s DocumentDB nebo k úložišti vzdáleného média. Také umožňuje představují metadata multimediálních z hlediska zvláštní dokument s názvem příloh. Přílohy v DocumentDB je speciální dokument (JSON), který odkazuje média a objektů blob uložený jinde. Přílohu je jednoduše zvláštní dokumentem, který popisuje metadata (například umístění, Autor atd.) multimediálních uložené v úložišti vzdáleného média. 

Zvažte sociální čtení aplikace, která používá DocumentDB k uložení rukopisné poznámky a metadat včetně komentáře, zvýrazní, záložky, hodnocení, lajky/nelíbí přidružené k e knihu daného uživatele atd.   

-   Obsah v knize samotné uložené v úložišti médií buď jsou k dispozici jako součást účet databázi DocumentDB nebo médium vzdáleného úložiště. 
-   Aplikace může ukládat metadata každého uživatele jako různých dokument – například Honzův metadata pro Sešit1 jsou uložena v dokumentu odkazováno /colls/joe/docs/book1. 
-   Přílohy přejdete na stránky obsahu dané knihy uživatele jsou uložené ve skupinovém rámečku odpovídající dokument například /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 atd. 

Všimněte si, že výše uvedené příklady použít ke sdělení hierarchii zdroje popisný ID. Zdroje jsou dostupné prostřednictvím REST API prostřednictvím jedinečné ID. 

Pro média, která spravuje DocumentDB vlastnost _media přílohy odkazují multimédia tak, že jeho URI. DocumentDB zajistí, aby uvolnění shromažďuje médií při všech zbývajících odkazy se nezobrazí. DocumentDB automaticky vygeneruje přílohy při odesílání nových médií a vyplní _media tak, aby ukazovaly na nově přidaný média. Pokud se rozhodnete uložit médií v úložišti objektů blob vzdálené spravuje můžete (například OneDrive, úložišti Azure DropBox atd), můžete dál používat přílohy neodkazuje médií. V tomto případě sami vytvoříte na přílohu a naplnění vlastností _media.   

Stejně jako u všech ostatních prostředků přílohy se dají vytvářet, nahradit, odstranit, číst nebo vytvořit výčet snadno pomocí rozhraní REST API nebo některou z klienta SDK. Stejně jako u dokumentů, čtení konzistence úroveň přílohy následuje zásady konzistence na účet databázi. Na základě požadavků v závislosti na požadavky konzistence dat aplikace mohou přepsat tuto zásadu. Při dotazování na přílohy, sleduje čtení konzistence indexování režimu nastavení v kolekci. Pro "konzistentní" to následuje zásady konzistence účtu. 
 
## <a name="users"></a>Uživatelé
Uživatel DocumentDB představuje logické názvů pro seskupení oprávnění. DocumentDB uživatel může odpovídají uživatele v systému správy identit nebo předdefinované aplikace roli. DocumentDB uživatel jednoduše představuje odběru zařadit do skupiny sadu oprávnění v části databáze.   

K provádění víceklientská v aplikaci, uživatelé se dají vytvářet v DocumentDB, která odpovídá skutečné uživatelům nebo klientů aplikace. Můžete pak vytvořit oprávnění pro daného uživatele, které odpovídají řízení přístupu přes různé kolekce dokumenty, přílohy, atd.   

Aplikace se potřebujete měřítko růstu uživatele, můžete přijmout různé způsoby shard vaše data. Můžete modelovat všem uživatelům následujícím způsobem:   

-   Každý uživatel mapy k databázi.
-   Každý uživatel mapuje kolekce. 
-   Dokumenty, které odpovídají více uživatelů přejděte do kolekce vyhrazené. 
-   Dokumenty, které odpovídají více uživatelů přejděte k části Nastavení kolekcí.   

Bez ohledu na to, které zvolíte konkrétní sharding strategie můžete modelovat skutečné uživatele jako uživatele v databázi DocumentDB a přidružení správně odstupňovaná oprávnění jednotlivým uživatelům.  

![Kolekce uživatele][3]  
**Sharding strategie a modelování uživatele**

Jako všechny ostatní zdroje uživatelů v DocumentDB lze vytvořit, nahradit, odstranit, číst nebo vytvořit výčet snadno pomocí rozhraní REST API nebo některou z klienta SDK. DocumentDB vždy poskytuje silných konzistence pro čtení nebo dotazování metadata uživatele zdroje. Je vhodné poukázání na odstraněním uživatele automaticky zaručuje, že nebudete mít přístup oprávnění obsažené v něm obsažené. I když DocumentDB získá kvóty oprávnění jako součást odstraněnými uživatelskými na pozadí, odstraněné oprávnění neexistuje okamžitě znovu použít.  

## <a name="permissions"></a>Oprávnění
Z pohledu ovládací prvek aplikace přístup, zdroje, jako jsou databáze účty databází, uživatelé a oprávnění jsou považovány za prostředky *pro správu* od vyžadují oprávnění správce. Na druhé straně materiálů, včetně kolekce, dokumenty, přílohy, uložené procedury, aktivačních událostí a funkce definované uživatelem rozsah v části danou databázi se považuje za *prostředky aplikace*. Odpovídající dva typy zdrojů a role, které jsou k nim (hlavně správce a uživatele), model se tak mohli ověřovat definuje dva typy *přístupových kláves z verze*: *hlavní* a *zdroje klíče*. Hlavní klíč je součástí databáze účtu a je k dispozici karta Vývojář v (nebo správce) kdo je zřízení účtu databáze. Tento klíč předlohy má správce sémantiku v tom, že jej lze udělit oprávnění k přístupu k prostředkům pro správu a aplikace. Klíč zdroje je naopak podrobného přístupový kód, který umožňuje přístup k *určitému* zdroji aplikace. Proto zaznamenává vztah mezi uživateli databáze a oprávnění, která má uživatel určitého zdroje (například kolekce, dokumentu, přílohy, uložené procedury, aktivační událost nebo UDF).   

Jediný způsob, jak získat klíč zdroje je tak, že vytvoříte zdroj oprávnění v části daného uživatele. Všimněte si, že k vytvoření nebo získat oprávnění, musí být hlavní klíč prezentovány v záhlaví se tak mohli ověřovat. Prostředek oprávnění spojuje zdroje, přístup a uživatel. Po vytvoření zdroje oprávnění uživatele pouze musí prezentovat klávesu přidružené zdroje k získání přístupu k příslušný zdroj. Klíč prostředku proto lze zobrazit jako logické a kompaktní znázornění zdroje oprávnění.  

Stejně jako u všech ostatních prostředků oprávnění v DocumentDB se dají vytvářet, nahradit, odstranit, číst nebo vytvořit výčet snadno pomocí rozhraní REST API nebo některou z klienta SDK. DocumentDB vždy poskytuje silných konzistence pro čtení nebo dotazování metadat oprávnění. 

## <a name="next-steps"></a>Další kroky
Další informace o práci s zdrojů pomocí příkazů HTTP v [RESTful interakce s DocumentDB zdroje](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

