<properties
    pageTitle="Šifrování klientských s Java Microsoft Azure úložištěm | Microsoft Azure"
    description="Knihovna Azure úložiště klienta jazyka Java podporuje šifrování na straně klienta a integrace se službou Azure klíč trezoru maximální cenného papíru pro aplikace Azure úložiště."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-java-for-microsoft-azure-storage"></a>Šifrování klientských s Java úložišti Microsoft Azure   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Základní informace  

[Azure úložiště klienta knihovny jazyka Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) podporuje šifrování dat v klientských aplikacích před nahráním k základnímu úložišti Azure a dešifrování dat při stahování klientovi. Knihovny taky podporuje integrace se službou [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/) pro správu klíčů účtu úložiště.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Šifrování a dešifrování prostřednictvím Obálka techniku    
Procesy šifrování a dešifrování podle Obálka techniku.  

### <a name="encryption-via-the-envelope-technique"></a>Šifrování pomocí Obálka techniku  
Šifrování pomocí Obálka techniku funguje následujícím způsobem:  

1.  Knihovna Azure úložiště klienta vygeneruje obsahu šifrovací klíč (CEK), která je pomocí jednoho úvazek symetrickou klávesy.  

2.  Uživatelská data je šifrovaná pomocí tohoto CEK.   

3.  CEK je pak omotané (šifrovaná) pomocí klíčových šifrovací klíč (KEK). KEK je určen identifikátoru klíče a mohou být asymetrické pár klíčů nebo symetrickou klávesou a můžete být spravované místně nebo uloženy v Azure klíč trezorů.  
Samotné knihovny úložiště klienta nikdy má přístup k KEK. Knihovnu vyvolá algoritmus klíčové obtékání, který poskytuje společnost klíč trezoru. Uživatelé mohou pro účely vlastních poskytovatelů klíčové obtékání/obtékání podle potřeby.  

4.  Šifrovaná data se pak nahrané služby Azure úložiště. Klávesu zalomený spolu s některé další šifrování metadat je uložená jako metadata (na blob) nebo interpolované s šifrovaná data (zpráv a tabulky entity).

### <a name="decryption-via-the-envelope-technique"></a>Dešifrování prostřednictvím Obálka techniku  
Dešifrování prostřednictvím Obálka techniku funguje následujícím způsobem:  

1.  Knihovnu klienta se předpokládá, že je uživatel místně nebo v Azure klíč trezorů Správa klíčových šifrovacího klíče (KEK). Uživatel nemusí znát určité klávesy, která byla použita pro šifrování. Místo toho klíčové překládání, které odstraňuje identifikátory klíčové klávesy můžete nastavit a používat.  

2.  Knihovnu klienta stahování zašifrovaných data spolu s jakýkoli šifrování materiál, který je uložený ve službě.  

3.  Zalomený obsahu šifrovací klíč (CEK) je nebalených (dešifrovaný) pomocí klíče šifrovací klíč (KEK). Tady znovu knihovnu klienta neměl přístup k KEK. Jednoduše vyvolá vlastní nebo rozbalení algoritmus klíč trezoru poskytovatele.  

4.  Obsahu šifrovací klíč (CEK) je pak použít dešifrovat šifrované uživatelská data.

## <a name="encryption-mechanism"></a>Šifrování mechanismus  
Knihovna úložiště klienta používá [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) k šifrování uživatelská data. Konkrétně [Šifrování blok zřetězení CBC](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režim AES. Jednotlivé trochu jinak služby funguje tak, aby se probereme tato témata každý z nich tady.

### <a name="blobs"></a>Objekty BLOB  
Knihovnu klienta v současné době podporuje šifrování pouze celé objektů BLOB. Konkrétně šifrování je podporované, když uživatelé používat **Nahrát** * metody nebo * *openOutputStream** metody. Jsou podporované, obě dokončení a soubory ke stažení oblast stahování.  

Při šifrování bude knihovnu klienta Generovat náhodné inicializace vektorové IV 16 bajtů, spolu s náhodné obsahu šifrovací klíč (CEK) 32 bajtů a proveďte obálky šifrování objektů blob dat z těchto informací. Zalomený CEK a některá další šifrování metadata jsou uložena jako objektů blob metadat spolu s šifrované objektů blob na služby.

>[AZURE.WARNING] Pokud jsou úpravy nebo odesílání vlastní metadat pro objekt blob, musíte zajistit, aby se zachová tato metadata. Pokud odešlete nová metadata bez tato metadata zalomený CEK, IV a ostatních metadatech, které budou ztracena a objektů blob obsah nikdy bude zobrazitelné ve výsledcích vyhledávání znovu.

Stahování zašifrovaných objektů blob zahrnuje načtení obsahu celý objektů blob pomocí * *Stáhnout*/openInputStream** vhodné metody. Zalomený CEK nebalený a používat společně s IV (uložené v tomto případě jako objektů blob metadata) se vrátíte dešifrovaná data uživatelům.

Stahování libovolného oblasti (**downloadRange*** metody) v šifrované objektů blob zahrnuje úpravy rozsahu poskytovanou uživatelů, abyste mohli získávat malou část Další data, která mohou sloužit k úspěšně dešifrování požadovaný rozsah.  

Všechny typy objektů blob (blokovat objektů BLOB stránky objektů BLOB a připojit objektů BLOB) lze zašifrovaných/dešifrovat pomocí tohoto režimu.

### <a name="queues"></a>Fronty  
Protože zpráv mohou být libovolného formátu, knihovnu klienta definuje vlastní formát, který obsahuje vektorové inicializace (IV) a šifrované obsahu šifrovací klíč (CEK) v textu zprávy.  

Při šifrování knihovnu klienta vygeneruje náhodné IV 16 bajtů spolu s náhodné CEK 32 bajtů a provede obálky šifrování textu zprávy fronty z těchto informací. Zalomený CEK a některá další šifrování metadata pak přidají šifrované fronty zprávu. Tato změněné zpráva (viz níže) je uložený ve službě.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Při dešifrování klávesu zalomený extrahovaných z fronty zprávy a nebalený. IV je také extrahovaných z fronty zprávy a použít společně s klávesu nebalených dešifrovat data fronty zprávy. Všimněte si, že metadata šifrování small (v části 500 bajtů), zatímco nepočítá směrem k 64KB limit fronty zprávu, dopad by měl být spravovatelnosti.

### <a name="tables"></a>Tabulky  
Knihovna klienta podporuje šifrování entity vlastností pro vložení operace a nahradit.

>[AZURE.NOTE] Sloučení není aktuálně podporován. Protože podmnožinu vlastnosti může mít zašifrovaný dříve pomocí různých klíče, jednoduše sloučení nové vlastnosti a aktualizace metadata dojde ke ztrátě dat. Sloučení buď vyžaduje volání navíc služby číst stávajících entity ze služby nebo pomocí nové klíč podle vlastností, které nejsou vhodné důvodů výkonu.

Šifrování dat tabulky funguje takto:  

1.  Uživatelé vlastnosti šifrování.  

2.  Knihovna klienta vygeneruje náhodné inicializace vektorové IV 16 bajtů spolu náhodné obsahu šifrovací klíč (CEK) 32 bajtů pro každou entitu a provede obálky šifrování jednotlivých vlastností šifrované odvozené nová IV na vlastnost. Vlastnost šifrované uložená jako binárními daty.  

3.  Zalomený CEK a některá další šifrování metadata potom uložená jako dvě doplňující vlastnosti rezervovaná. Vlastnost první rezervovaná (_ClientEncryptionMetadata1) je vlastnost řetězec s informacemi o IV, verze a zalomený klíče. Vlastnost druhé rezervovaná (_ClientEncryptionMetadata2) je binární vlastnost, která obsahuje informace o vlastnostech, které jsou šifrované. Informace v této druhý vlastnosti (_ClientEncryptionMetadata2) je šifrovaná.  

4.  Z důvodu těchto dalších rezervovaná vlastností potřebných pro šifrování uživatelé nyní může mít jenom 250 vlastní vlastnosti místo 252. Celková velikost entity musí být menší než 1MB.  

    Všimněte si, že je možné zašifrovat pouze řetězec vlastnosti. Pokud jiné druhy vlastnosti jsou šifrovány, musí být převáděny na řetězce. Jsou zašifrované řetězce jsou uložené ve službě jako binární vlastnosti a jsou převáděny zpátky na řetězce po dešifrování.

    U tabulek, kromě zásady šifrování uživatelé musí zadat vlastnosti šifrování. Tím se teď dá zadáním buď [zašifrovat] atribut (POCO entity, které jsou odvozeny od TableEntity) nebo šifrování překládání v žádosti o možnostech. Překládání šifrování je delegáta, který má klíč oddílu, klíče řádku a název vlastnosti a vrátí logickou hodnotu, která označuje, zda má být zašifrován této vlastnosti. Při šifrování, knihovnu klienta se tyto informace použít k rozhodnout, jestli má být při psaní lince zašifrován vlastnost. Delegát taky poskytuje možnost použití logických operátorů kolem jak jsou šifrované vlastnosti. (Například Pokud argumenty X, pak šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že není nutné zadávat zadejte tyto informace při čtení nebo dotazování entity.

### <a name="batch-operations"></a>Dávkové operace  
V dávkové operace stejné KEK se použije ve všech řádcích v operaci dávku protože knihovnu klienta umožňuje pouze jeden objekt možnosti (a tedy jeden zásad/KEK) na dávku operaci. Však knihovnu klienta interně vygeneruje nové náhodné IV a náhodné CEK na řádku na listu. Uživatelé mohou také k šifrování různé vlastnosti pro každé operaci dávku tím, že definujete toto chování v překládání šifrování.

### <a name="queries"></a>Dotazy  
K provádění operací, dotazu, je nutné zadat klíčové překládání, který je vyřešit všechny klíče v sadě výsledků. Pokud entita obsažené ve výsledku dotazu nelze převést na poskytovatele, knihovnu klienta vyvolá chybu. Pro dotaz, který provádí průzkumy straně serveru přidá knihovnu klienta vlastnosti metadat zvláštní šifrování (_ClientEncryptionMetadata1 a _ClientEncryptionMetadata2) ve výchozím nastavení vybrané sloupce.

## <a name="azure-key-vault"></a>Azure klíčové trezoru  
Azure trezoru klíč pomáhá chránit klíče a tajemství používá cloudové aplikací a služeb. Pomocí klávesy trezoru Azure uživatelé mohou šifrovat klíče a tajemství (například ověřovací klíče, klíče účtu úložiště, klíče šifrování dat. Soubory PFX a hesla) pomocí kláves, které jsou chráněny hardwarové zabezpečení moduly (moduly hardwarového zabezpečení). Další informace najdete v tématu [Co je Azure klíč trezoru?](../key-vault/key-vault-whatis.md).

Knihovně úložiště klienta používá knihovnu základní klíč trezoru za účelem zajištění společný rámec přes Azure pro správu klíčů. Uživatelé otevřete taky Další výhodou používání knihovna rozšíření klíč trezoru. Knihovna rozšíření poskytuje užitečné funkce kolem jednoduchý a hladké Symmetric/RSA místní a klíče zprostředkovatelů cloudu i s agregace a ukládání do mezipaměti.

### <a name="interface-and-dependencies"></a>Rozhraní a závislosti  
Existují tři balíčků trezoru klíč:  

- Azure keyvault core obsahuje IKey a IKeyResolver. Je menší balíčku s žádné závislosti. V knihovně úložiště klienta Java definuje ji jako závislostí.
- Azure keyvault obsahuje ZBÝVAJÍCÍ trezoru klíč klienta.  
- Azure keyvault rozšíření obsahuje rozšíření kód, který obsahuje implementace algoritmy šifrování a RSAKey a SymmetricKey. Závisí na základní a KeyVault obory názvů a poskytuje funkce definovat agregační překládání (když uživatelé chtějí používat několik klíčových zprostředkovatele) a ukládání do mezipaměti překládání klíče. Ačkoli knihovně úložiště klienta nezávisí přímo na balíček, pokud chcete použít trezoru klíč Azure, ukládání legendou nebo rozšíření klíč trezoru umožňuje využívat místní i cloudových cryptographic zprostředkovatelů uživatelů, bude nutné balíček.  

  Klíč trezoru je určený pro vysoké hodnoty hlavní klíče a omezení limity za klíč trezoru jsou navrženy s tímto na paměti. Při provádění klientských šifrování s trezoru klíč, upřednostňované modelu je klávesami symetrickou předlohy uložených jako tajemství klíč trezoru a cached místně. Uživatelé musí takto:  

1.  Vytvořte tajná offline a uložte do klíč trezoru.  

2.  Použijte základní identifikátor tajná jako parametr k řešení aktuální verzi tajná šifrování a místně tyto informace do mezipaměti. Použití CachingKeyResolver pro ukládání do mezipaměti; Uživatelé nejsou podle očekávání měly být implementovat vlastní ukládání do mezipaměti použití logických operátorů.  

3.  Použití mezipaměti překládání jako vstup při vytváření zásady šifrování.
Další informace o použití klávesy trezoru najdete ve vzorcích kódu šifrování. <fix URL>  

## <a name="best-practices"></a>Doporučené postupy  
Podpora šifrování je k dispozici pouze v knihovně úložiště klienta jazyka Java.

>[AZURE.IMPORTANT] Mějte na paměti následující důležité bodů při použití šifrování na straně klienta:
>  
>- Při čtení nebo zápisu šifrované objektů blob, použijte celou objektů blob upload a oblasti nebo celé objektů blob stažení příkazy. Zamezení zapsání do šifrované objektů blob pomocí protokolu operací, jako je vložit blok, umístění seznamu blokovaných adres, napište stránky, vymazat stránky nebo Přidat blok; v opačném případě může poškozený šifrované objektů blob a naformátovat ji čitelný.
>
>- U tabulek podobně jako omezení existuje. Dávejte Neaktualizovat šifrované vlastnosti bez aktualizace metadat šifrování.  
>
>- Pokud jste nastavili metadat na šifrované objektů blob, může přepsat metadat souvisejících šifrování potřebných pro dešifrování, protože není přidávat nastavení metadata. To platí také k snímky; Vyhněte se určující metadat při vytváření snímek šifrované objektů blob. Pokud se nastaví metadata, je potřeba zavolat **downloadAttributes** metodu nejprve získat aktuální metadata šifrování a vyhnout souběžné zápisy při nastaví metadata.  
>
>- Povolte **requireEncryption** příznaku v žádosti o výchozí možnosti pro uživatele, kteří mají pracovat jenom s šifrovaná data. Další informace najdete níže.  

## <a name="client-api--interface"></a>Klientského rozhraní API / rozhraní  
Při vytváření objektu EncryptionPolicy, můžete uživatelům poskytnout pouze klíč (provádění IKey), pouze překladač (provádění IKeyResolver) nebo obojí. IKey je základní typ klíče, které jsou označeny pomocí klíčových identifikátoru a poskytující logiky pro obtékání/obtékání. IKeyResolver slouží k řešení klíč během procesu dešifrování. Definuje ResolveKey metodu, která vrací IKey zadaných identifikátoru klíče. To umožňuje uživatelům vybrat mezi několika kláves, které je spravováno do několika umístění.

- Šifrování používá se vždy a nepřítomnosti klíče bude výsledkem chyba.  
- Pro dešifrování:  
    - Pokud je zadaná získat klávesu vyvolání klíčové překládání. Pokud překladač není zadán, ale nemá mapování identifikátoru klíče, vyvolá se chyba.  
    - Pokud není zadán překládání, ale není zadán klíč, používá se shoduje s identifikátoru požadované identifikátoru klíče. Pokud identifikátor neodpovídá, vyvolá se chyba.  

      [Šifrování vzorky](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>podrobnější scénář začátku do konce pro objekty BLOB, dotazů a tabulek, je popsán spolu s integrací klíč trezoru.

### <a name="requireencryption-mode"></a>Režim RequireEncryption  
Uživatele můžete volitelně povolit režim operace, které musí být všechny nahrávání a stahování zašifrován. V tomto režimu pokusy data bez zásad šifrování nahrávání nebo stahování data, která není zašifrovaných na službě selžou na straně klienta. Příznak **requireEncryption** objektu žádosti možnosti mezery nastavuje toto chování. Pokud aplikace bude šifrování všech objektů uložených v úložišti Azure, můžete nastavit vlastnost **requireEncryption** na výchozí možnosti žádosti o služby klienta objektu.   

Například umožňuje **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** šifrování pro všechny operace objektů blob prováděné pomocí tohoto klienta objektu.

### <a name="blob-service-encryption"></a>Šifrování služby objektů BLOB  
Vytvoření **BlobEncryptionPolicy** objektu a jeho nastavení v možnostech žádost (za rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všechny ostatní bude zpracována knihovnu klienta interně.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);

    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    blob.download(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Šifrování služby fronty  
Vytvoření **QueueEncryptionPolicy** objektu a jeho nastavení v možnostech žádost (za rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všechny ostatní bude zpracována knihovnu klienta interně.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);

    queue.addMessage(message, 0, 0, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### <a name="table-service-encryption"></a>Tabulku služba šifrování  
Kromě vytváření zásad šifrování a nastavíte na žádost o možnostech, musíte zadat **EncryptionResolver** v **TableRequestOptions**nebo nastavit atribut [zašifrovat] u příjemce je entita a nastavení.

### <a name="using-the-resolver"></a>Použití překladač  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });

    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    retrieveOptions.setEncryptionPolicy(policy);

    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### <a name="using-attributes"></a>Pomocí atributů  
Výše uvedené, pokud entitu implementuje TableEntity, pak vlastnosti příjemce a nastavení mohou být ozdobenou s atributem [zašifrovat] místo určení **EncryptionResolver**.

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }

    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## <a name="encryption-and-performance"></a>Šifrování a výkon  
Všimněte si, že šifrování výsledky datový úložiště v nároky na další výkon. Je třeba vygenerovat obsahu klíče a IV třeba zašifrovat obsah a další metadata musí být naformátovaná a odeslat. Tento režijních budou lišit podle toho množství dat šifrovány. Doporučujeme, aby zákazníci testujte žádostí o výkonu během vývoje.

## <a name="next-steps"></a>Další kroky  

- Stáhněte si [Azure úložiště klienta knihovny Java Maven balíčku](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
- Stáhněte si [Azure úložiště klienta knihovny zdrojového kódu jazyka Java z GitHub](https://github.com/Azure/azure-storage-java)   
- Stáhněte knihovnu Azure klíč trezoru Maven pro Java Maven balíčků:
    - [Základní](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) balíček
    - Balíček [klienta](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault)
- Navštivte [Azure klíč trezoru si přečtěte následující dokumentaci](../key-vault/key-vault-whatis.md)  
