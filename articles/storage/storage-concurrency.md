<properties
    pageTitle="Správa souběžné v úložišti Microsoft Azure"
    description="Jak spravovat souběžné služby objektů Blob, fronty, tabulky a souborů"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Správa souběžné v úložišti Microsoft Azure

## <a name="overview"></a>Základní informace

Moderní aplikace Internet na základě obvykle uživatelé by měli více zobrazení a aktualizace dat současně. Při této akci musí vývojáři pečlivě přemýšlet o tom, jak zajistíte předvídatelná možnosti koncových uživatelů, zejména u scénáře, které více uživatelů můžete aktualizovat stejná data. Existují tři hlavní datové souběžné strategie vývojáři zvážíme obvykle:  


1.  Optimistická souběžné – provádění aplikace, které aktualizace v rámci jeho aktualizace ověří, když se data změní od aplikace posledního čtení tato data. Například-li dva uživatelé zobrazení stránky wikiwebu aktualizaci na stejnou stránku potom platformu wikiwebu musíte se ujistit druhé aktualizace nepřepíše první aktualizace – a obě uživatele jasné, zda byla jejich aktualizace úspěšná. Tato strategie se používá nejčastěji ve webových aplikacích.
2.  Pesimistická souběžné – aplikaci chcete provést aktualizaci bude trvat zámek na objekt brání aktualizaci dat, dokud nebude vydána zámek ostatním uživatelům. Například ve scénáři replikace předlohy/podřízený dat místo, kam se jenom na předlohu provádění aktualizací předlohy se obvykle podržte výhradní zámek delší dobu časového úseku na data, která chcete zajistit, aby neměl že nikdo jiný můžete aktualizovat.
3.  Zápis poslední – postup, který umožňuje všechny operace aktualizace pokračujte bez ověření Pokud jakékoli jiné aplikace data od aktualizoval aplikace nejdřív přečíst data. Tento strategie (nebo chybějící formální strategie) se obvykle používá kde oddíly data tak, že je pravděpodobnost, že více uživatelé stejná data. Ho může být také užitečné, kde jsou zpracovávání krátkodobý datových proudů.  

Tento článek obsahuje přehled jak platformu úložišti Azure zjednodušuje vývoj poskytnutím prvotřídní podpory pro všechny tři tyto souběžné strategie.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure úložiště – zjednodušuje vývoj cloudu
Služba Azure úložiště podporuje všechny tři strategie, ačkoli je charakteristické možnosti podporují celou optimistické a pesimistické souběžné protože byla určena podpořit silných konzistence modelu, které zaručuje, že když služba úložiště potvrzení vložení dat nebo operace aktualizace všechny další přistupuje pro která data se zobrazí v nejnovějších aktualizacích a v oblasti. Úložiště platformy, které používají modelu případné konzistence mít prodlevy mezi při provádění zápisu jedním uživatelem a aktualizovaná data uvidí ostatní uživatelé, čímž je ztížena vývoje klientských aplikací nekonzistence, aby se z došlo k ovlivnění koncoví uživatelé.  

Kromě výběru efektivní strategii odpovídající souběžné vývojáři by měla být vědom jak úložiště platformy izoluje změny – zejména změny u jednoho objektu přes transakce. Povolit operace čtení stát souběžně zápisu v jednom oddílu využívá služba Azure úložiště izolace snímek. Na rozdíl od jiných úrovně izolace snímek izolace zaručuje, že všechny operace čtení najdete v článku konzistentní snímek dat, i když se vyskytnou aktualizací – v podstatě vrácením poslední potvrzené hodnoty při aktualizaci transakce zpracovávání.  

## <a name="managing-concurrency-in-blob-storage"></a>Správa souběžné v úložišti objektů Blob
Můžete se rozhodnout pro řízení přístupu k objektů BLOB a kontejnery ve službě objektů blob pomocí buď optimistické nebo pesimistické souběžné modely. Pokud nezadáte explicitně strategie poslední wins zápisy je výchozí hodnota.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistická souběžné pro objekty BLOB a kontejnery  

Služba úložiště přiřadí identifikátor každý objekt uložený. Tento identifikátor se aktualizuje pokaždé, když se provádí operace aktualizace na objekt. Identifikátor vráceny klientovi v rámci odpověď HTTP GET pomocí záhlaví ETag (značka entity), která je definovaná v protokolu HTTP. Uživatel aktualizaci provádění k objektu můžete odeslat v původní ETag spolu s podmíněným záhlaví zajistit, že aktualizací jenom dojde, pokud splnění určité podmínky – v tomto případě je podmínka "If POZVYHLEDAT" záhlaví, která vyžaduje službu úložiště a zkontrolujte, že hodnota ETag podle žádost o aktualizaci stejný jako uložené ve službě úložiště.  

Osnovu tohoto procesu je následující:  

1.  Načtení objektů blob z úložiště služby, odpověď obsahuje záhlaví ETag HTTP hodnotu, která určuje aktuální verzi objektu ve službě úložiště.
2.  Při aktualizaci objektů blob obsahovat hodnotu ETag, který jste dostali v kroku 1 v záhlaví podmíněné **POZVYHLEDAT v případě** žádosti odesílané do služby.
3.  Služba porovnává hodnota ETag v pozvánce aktuální hodnotou ETag objektů blob.
4.  Pokud aktuální ETag objektů blob hodnotu v jiné verzi než ETag v záhlaví podmíněné **If POZVYHLEDAT** v žádosti o, službu vrátí chybu 412 klientovi. Tento údaj označuje klientovi jiným procesem aktualizoval objektů blob od klienta načtené ho.
5.  Pokud aktuální ETag objektů blob hodnotu ve stejné verzi jako ETag v záhlaví podmíněné **If POZVYHLEDAT** v žádosti o, služba provede požadované operace a aktualizuje aktuální hodnotu ETag objektů blob zobrazíte, aby byla vytvořena nová verze.  

Následující C# úryvek (pomocí knihovně úložiště klienta 4.2.0) zobrazuje jednoduchý příklad toho, jak vytvářet **If POZVYHLEDAT AccessCondition** na základě ETag hodnoty dostupný z vlastnosti objektů blob, která byla dříve načíst nebo vložit. Potom používá objekt **AccessCondition** při jeho aktualizace objektů blob: objekt **AccessCondition** přidá záhlaví **If POZVYHLEDAT** k žádosti o. Pokud jiným procesem aktualizoval objektů blob, služba objektů blob vrátí stavová zpráva 412 HTTP (předpoklad.). Můžete si stáhnout celkového vzorku: [Správa souběžné pomocí Azure úložiště](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Služba úložiště podporuje také další podmíněné záhlaví, jako **Když-změny a od**, **Když-smíte bez jakýchkoli úprav a od** a **Když-žádné časově POZVYHLEDAT** i jejich kombinace. Další informace najdete na webu MSDN [Určující podmíněné záhlaví pro operace objektů Blob služby](http://msdn.microsoft.com/library/azure/dd179371.aspx) .  

Následující tabulka shrnuje kontejneru operací, přijměte podmíněné záhlaví například **Když POZVYHLEDAT** v žádosti o a, který vrací hodnotu ETag v odpovědi.  

| Operace                | Vrátí hodnotu ETag kontejneru | Přijme podmíněné záhlaví |
|:-------------------------|:-----------------------------|:----------------------------|
| Vytvoření kontejneru         | Ano                          | Ne                          |
| Získání kontejneru vlastnosti | Ano                          | Ne                          |
| Získání kontejneru metadat   | Ano                          | Ne                          |
| Nastavit kontejneru Metadata   | Ano                          | Ano                         |
| Získání kontejneru ACL        | Ano                          | Ne                          |
| Nastavení kontejneru ACL        | Ano                          | Ano (*)                     |
| Odstranění kontejneru         | Ne                           | Ano                         |
| Kontejner zapůjčení          | Ano                          | Ano                         |
| Seznam objekty BLOB               | Ne                           | Ne                          |

(*) Oprávnění definované SetContainerACL ukládány a aktualizacím tato oprávnění trvat 30 sekund, než se rozšíří během nichž aktualizace nemusí být konzistentní.  

Následující tabulka shrnuje objektů blob operace, přijměte podmíněné záhlaví například **Když POZVYHLEDAT** v žádosti o a, který vrací hodnotu ETag v odpovědi.

| Operace           | Vrátí hodnotu ETag | Přijme podmíněné záhlaví           |
|:--------------------|:-------------------|:--------------------------------------|
| Umístění objektů Blob            | Ano                | Ano                                   |
| Získání objektů Blob            | Ano                | Ano                                   |
| Získání objektů Blob vlastnosti | Ano                | Ano                                   |
| Nastavení vlastností objektů Blob | Ano                | Ano                                   |
| Získat objektů Blob Metadata   | Ano                | Ano                                   |
| Nastavit objektů Blob Metadata   | Ano                | Ano                                   |
| Kulatý zapůjčení (*)      | Ano                | Ano                                   |
| Snímek Blob       | Ano                | Ano                                   |
| Kopírování objektů Blob           | Ano                | Ano (zdrojové a cílové blob) |
| Přerušení objektů Blob kopie     | Ne                 | Ne                                    |
| Odstranění objektů Blob         | Ne                 | Ano                                   |
| Vložit blok           | Ne                 | Ne                                    |
| Vložit – seznam blokování      | Ano                | Ano                                   |
| Získání seznamu blokovaných adres      | Ano                | Ne                                    |
| Vložit stránku            | Ano                | Ano                                   |
| Získání rozsahů     | Ano                | Ano                                   |


(*) Kulatý zapůjčení nezmění ETag na objekt blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pesimistická souběžné pro objekty BLOB
Chcete-li uzamknout objektů blob využívat, můžete získat [zapůjčení](http://msdn.microsoft.com/library/azure/ee691972.aspx) na něj. Při získání zapůjčenou určíte, jak dlouho je třeba zapůjčení: může to být pro mezi 15 až 60 sekund nebo nekonečné které částek výhradní zámek. Můžete prodloužit konečných zapůjčení rozšíření a můžete uvolníte jakékoli zapůjčení po dokončení s ním. Službu objektů blob automaticky vydání konečných zapůjčení, když vyprší jejich platnost.  

Zapůjčení povolit strategie různých synchronizace se plánuje podpora, včetně výhradní zápis / sdílené číst, výhradním zápisu / exkluzivní číst a sdílené zápisu / číst výhradní přístup. Pokud existuje zapůjčenou služba úložiště vynucuje exkluzivní zápisy (umístění, nastavení a odstraňování) však zajištění výhradního práva pro operace čtení vyžaduje vývojář zajistit, že všechny klientské aplikace používat zapůjčení ID a jedinou klienta současně má platný zapůjčení ID. Přečtěte si operacích, které neobsahují výsledku ID zapůjčení ve sdílené čtení.  

Následující úryvek C# příklad získáním výhradním zapůjčení 30 sekund na objekt blob, aktualizace obsahu objektů blob a uvolněním zapůjčení. Pokud už je platný zapůjčení na objekt blob při pokusu o získat novou zápůjčku, služba objektů blob vrátí výsledek stav "Konflikt HTTP (409)". Fragment dole používá objektu **AccessCondition** zapouzdřit informací o zápůjčce když odešle žádost aktualizovat objektů blob ve službě úložiště.  Můžete si stáhnout celkového vzorku: [Správa souběžné pomocí Azure úložiště](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Pokud se pokusíte operaci zápisu na pevné objektů blob bez předejte zapůjčení ID, požadavek selže s 412 chyby. Všimněte si, že pokud vypršení před voláním metody **UploadText** , ale pořád předat zapůjčení ID, žádost také nefunguje s chybou **412** . Další informace o správě doby vypršení platnosti zapůjčení a zapůjčení ID najdete v dokumentaci ZBÝVAJÍCÍ [Objektů Blob zapůjčení](http://msdn.microsoft.com/library/azure/ee691972.aspx) .  

Tyto operace objektů blob slouží ke správě pesimistické souběžné zapůjčení:  


-   Umístění objektů Blob
-   Získání objektů Blob
-   Získání objektů Blob vlastnosti
-   Nastavení vlastností objektů Blob
-   Získat objektů Blob Metadata
-   Nastavit objektů Blob Metadata
-   Odstranění objektů Blob
-   Vložit blok
-   Vložit – seznam blokování
-   Získání seznamu blokovaných adres
-   Vložit stránku
-   Získání rozsahů
-   Snímek Blob – volitelné, pokud existuje zapůjčenou zapůjčení ID
-   Kopírování objektů Blob - zapůjčení ID povinné existenci zapůjčenou na objektů blob cíl
-   Přerušení objektů Blob kopírovat - poskytovat na leasing ID povinné Pokud nekonečné zapůjčení existuje na objektů blob cíl
-   Kulatý zapůjčení  

### <a name="pessimistic-concurrency-for-containers"></a>Pesimistická souběžné kontejnerů
Zapůjčení na kontejnery povolit stejné strategie synchronizace se plánuje podpora na objekty BLOB (exkluzivní psaní / sdílené číst, výhradním zápisu / exkluzivní číst a sdílené zápisu / exkluzivní číst) však na rozdíl od objektů BLOB služba úložiště pouze vynucuje výhradního práva na operace odstranění. Odstranit kontejneru s aktivní zápůjčku, musí obsahovat klienta ID aktivní zápůjčku s žádostí o odstranit. Všech dalších operací container úspěšné pevné container bez zahrnutí ID zapůjčení v takovém případě jsou sdílené operace. Pokud je požadována výhradního práva aktualizace (umístění nebo sady) nebo operace čtení potom vývojáři by měl zajistěte, aby že všechny klienty pomocí ID zapůjčení a jenom pro jednoho klienta současně má platný zapůjčení ID.  

Tyto operace kontejneru slouží ke správě pesimistické souběžné zapůjčení:  

-   Odstranění kontejneru
-   Získání kontejneru vlastnosti
-   Získání kontejneru metadat
-   Nastavit kontejneru Metadata
-   Získání kontejneru ACL
-   Nastavení kontejneru ACL
-   Kontejner zapůjčení  

Další informace najdete tady:  

- [Určení podmíněné záhlaví pro operace služeb objektů Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Kontejner zapůjčení](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Kulatý zapůjčení](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Správa souběžné ve službě tabulky
Využívá optimistické kontroly souběžné jako výchozí chování při práci s osobami, na rozdíl od objektů blob služby, kde je nutné provést kontrolu optimistické souběžné explicitně vybrat tabulku služba. Další rozdíl mezi službami tabulky a objektů blob se vypočítá následujícím vzhledem ke službě objektů blob můžete spravovat současnému kontejnerů a objektů BLOB můžete jenom spravovat souběžné chování entity.  

Použití optimistické souběžné a zkontrolovat, pokud jiný proces od načtená z úložiště služby Tabulka změněn entita, můžete hodnota ETag, které dostanete při tabulku služba vrátí entity. Osnovu tohoto procesu je následující:  

1.  Získat entita z úložiště služby tabulky, odpověď obsahuje ETag hodnotu, která určuje aktuálního identifikátoru přidružený k této entity ve službě úložiště.
2.  Při aktualizaci entitu obsahovat hodnotu ETag, který jste dostali v kroku 1 v záhlaví povinné **If shody** , které odesíláte ke službě žádosti o.
3.  Služba ve srovnání hodnota ETag v žádosti o s aktuální hodnota ETag entity.
4.  Pokud je aktuální hodnota ETag entity liší od ETag v záhlaví povinné **If POZVYHLEDAT** v žádosti o, službu vrátí chybu 412 klientovi. Tento údaj označuje klientovi jiným procesem aktualizoval entitu od klienta načtené ho.
5.  Pokud aktuální hodnota ETag entity je stejná jako ETag v záhlaví povinné **If POZVYHLEDAT** v žádosti o nebo **Když POZVYHLEDAT** záhlaví obsahuje zástupný znak (*), služba provede požadované operace a aktualizuje aktuální hodnota ETag entity zobrazíte, že byl aktualizovaný.  

Nezapomeňte, že na rozdíl od službu objektů blob vyžaduje tabulku služba klienta zahrnout záhlaví **If POZVYHLEDAT** požadavky na aktualizaci. Je však možné vynutit nepodmíněné aktualizovat (poslední Redaktor wins strategie) a nepoužívání souběžné kontroly, pokud klienta nastaví záhlaví **If POZVYHLEDAT** zástupný znak (*) v pozvánce.  

Následující úryvek C# ukazuje entitu Zákazník, která byla dříve vytvořena nebo načíst s jejich e-mailovou adresu aktualizovat. Počáteční vložíte načtení operace ukládá ETag hodnoty v objektu zákazníků nebo protože vzorku používá stejné instanci objektu, když provede operaci nahradit, automaticky pošle hodnota ETag zpět tabulku služba povolení služby kontrolovaní porušení souběžné. Pokud jiným procesem aktualizoval entitu v úložiště tabulek, vrátí službu stavová zpráva 412 HTTP (předpoklad.).  Můžete si stáhnout celkového vzorku: [Správa souběžné pomocí Azure úložiště](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Pro explicitně vypnutí kontroly souběžné, měli nastavit vlastnost **ETag** objekt **zaměstnance** "*" před spuštěním operaci nahradit.  

    customer.ETag = "*";  

Následující tabulka shrnuje, jak operace entity tabulky použít ETag hodnoty:

| Operace                | Vrátí hodnotu ETag | Vyžaduje POZVYHLEDAT v případě žádosti o záhlaví |
|:-------------------------|:-------------------|:---------------------------------|
| Entity dotazu           | Ano                | Ne                               |
| Vložení Entity            | Ano                | Ne                               |
| Aktualizace Entity            | Ano                | Ano                              |
| Sloučení Entity             | Ano                | Ano                              |
| Odstranění Entity            | Ne                 | Ano                              |
| Vložení či entitu nahradit | Ano                | Ne                               |
| Vložení či entitu hromadné korespondence   | Ano                | Ne                               |

Všimněte si, že se operace **Insert nebo nahrazení entita** a **vložení či entitu sloučit** *není* provést všechny kontroly souběžné, protože není posílají hodnota ETag tabulku služba.  

Obecně vývojáři pomocí tabulek by měl využívají optimistické souběžné při vytváření scalable aplikací. V případě potřeby pesimistické zamykání jedním ze způsobů vývojáři může trvat po otevření tabulky přiřaďte určenými objektů blob pro každou tabulku a pokuste umět zapůjčenou na objekt blob provozní v tabulce. Tento postup vyžaduje aplikaci zajistit všechny cesty dat aplikace access získat ji před provozní v tabulce. By měl navíc nezapomeňte minimální zápůjček se 15 sekund, které vyžadují pečlivě zvážit rozšiřitelnost.  

Další informace najdete tady:  

- [Operace na entity](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Správa souběžné ve službě fronty
Jeden scénář, ve které souběžné netýká ve službě přidávání do fronty je, kde jsou více klientů načítání zpráv z fronty. Když zprávy je načtená z kontingenčního seznamu fronty, obsahuje odpověď na zprávu a místním potvrzení hodnotu, která je potřebná k odstranění zprávy. Zpráva není ve frontě automaticky odstraní, ale po načtení ho není viditelné pro ostatní klienti pro zadaný parametrem visibilitytimeout časový interval. Očekává se, že klienta, který načte zprávu odstraní zprávu po byly zpracovány a před časovému TimeNextVisible prvek odpověď, která se vypočítá na základě hodnoty parametru visibilitytimeout. Hodnota visibilitytimeout přibude čas načtení zprávy hodnotu TimeNextVisible interpolací.  

Služba fronty nemá podpora souběžné optimistické nebo pesimistické a pro tento důvod, proč klientů zpracování zprávy načtené z fronty by měl zajistěte zpracování zpráv způsobem idempotent. Poslední strategie wins Redaktor slouží k aktualizaci operací, jako je SetQueueServiceProperties SetQueueMetaData, SetQueueACL a UpdateMessage.  

Další informace najdete tady:  

- [Fronty služba REST API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Doručení zpráv](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Správa souběžné ve službě soubor
Služba souborů můžete získat přístup pomocí dvou koncové body jiný protokol – SMB a PODRŽTE. Služba REST nemá podpora optimistické uzamčení nebo pesimistické zamykání a všechny aktualizace bude sledovat poslední strategie wins Redaktor. Plány SMB klientech připojit sdílené složky můžete využít soubor systému uzamčení mechanismy spravovat přístup ke sdílené soubory – včetně provádět pesimistické zamykání. Pokud klient SMB otevře soubor, určuje přístupu k souborům a sdílení režimu. Soubor je uzamčený klientem SMB, dokud nezavřete soubor povede nastavením možnosti přístupu k souborům "Napište" nebo "Pro čtení i zápis" spolu s režimu sdílené složky "Žádný". Pokud pokus o ZBÝVAJÍCÍ operaci na souboru, kde klientem SMB má soubor uzamčen služba REST vrátí stavový kód 409 (konflikt) s kódem chyby SharingViolation.  

Po otevření souboru pro odstranění klientem SMB označí soubor jako čeká na vyřízení odstranit až do jiným SMB klientem jsou uzavřené otevřít úchyty u tohoto souboru. Když soubor označen jako čekající na odstranit, všechny operace ZBÝVAJÍCÍ na tomto souboru vrátí stavový kód 409 (konflikt) s kódem chyby SMBDeletePending. Stavový kód 404 (nebyl nalezen) není vrátit, protože je možné klient SMB odebrat příznak Probíhá odstraňování před zavřením soubor. Když soubor odebrali jsme možnost jinými slovy, pouze očekávané stavový kód 404 (nebyl nalezen). Všimněte si, že když souboru je v SMB nevyřízená odstranit, ho nebudou zahrnuty ve výsledcích seznam souborů. Navíc nezapomeňte, že ZBÝVAJÍCÍ odstranění souborů a ZBÝVAJÍCÍ odstranit adresářů operace potvrzené atomicky a není výsledkem nevyřízená odstranit.  

Další informace najdete tady:  

- [Správa souborů uzamkne](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Souhrnné informace a další kroky
Podle potřeby nejčastěji složité online aplikace bez vynucení vývojářům ohrozit nebo přehodnotit hlavních předpoklady například souběžné a data konzistence, že pocházejí poskytnuté se byl navržen tak službu úložišti tabulek Microsoft Azure.  

Celá ukázková aplikace odkazuje v tomto blogu:  

- [Správa souběžné pomocí úložišti Azure - ukázková aplikace](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Další informace o úložišti Azure najdete v článku:  

- [Microsoft Azure úložiště domovskou stránku](https://azure.microsoft.com/services/storage/)
- [Úvod k základnímu úložišti Azure](storage-introduction.md)
- Úložiště Začínáme pro [objektů Blob](storage-dotnet-how-to-use-blobs.md), [tabulky](storage-dotnet-how-to-use-tables.md), [fronty](storage-dotnet-how-to-use-queues.md)a [souborů](storage-dotnet-how-to-use-files.md)
- Architektura úložiště – [Azure úložiště: službě vysoce dostupné cloudového úložiště s silných konzistence](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
