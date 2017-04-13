<properties
    pageTitle="Šifrování klientských s Python Microsoft Azure úložištěm | Microsoft Azure"
    description="Knihovnu Azure úložiště klienta pro Python podporuje šifrování klientských maximální cenného papíru pro aplikace Azure úložiště."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Šifrování klientských s Python úložišti Microsoft Azure

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Základní informace

[Azure úložiště klienta v knihovně Python](https://pypi.python.org/pypi/azure-storage) podporuje šifrování dat v klientských aplikacích před nahráním k základnímu úložišti Azure a dešifrování dat při stahování klientovi.

>[AZURE.NOTE] Knihovna Azure úložiště Python není ve verzi preview.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Šifrování a dešifrování prostřednictvím Obálka techniku
Procesy šifrování a dešifrování podle Obálka techniku.

### <a name="encryption-via-the-envelope-technique"></a>Šifrování pomocí Obálka techniku
Šifrování pomocí Obálka techniku funguje následujícím způsobem:

1.  Knihovna Azure úložiště klienta vygeneruje obsahu šifrovací klíč (CEK), která je pomocí jednoho úvazek symetrickou klávesy.

2.  Uživatelská data je šifrovaná pomocí tohoto CEK.

3.  CEK je pak omotané (šifrovaná) pomocí klíčových šifrovací klíč (KEK). KEK je určen identifikátoru klíče a může být asymetrické pár klíčů nebo symetrickou klávesy, která spravuje místně.
Samotné knihovny úložiště klienta nikdy má přístup k KEK. Knihovnu vyvolá algoritmus klíčové obtékání, který není uvedený KEK. Uživatelé mohou pro účely vlastních poskytovatelů klíčové obtékání/obtékání podle potřeby.

4.  Šifrovaná data se pak nahrané služby Azure úložiště. Klávesu zalomený spolu s některé další šifrování metadat je uložená jako metadata (na blob) nebo interpolované s šifrovaná data (zpráv a tabulky entity).

### <a name="decryption-via-the-envelope-technique"></a>Dešifrování prostřednictvím Obálka techniku
Dešifrování prostřednictvím Obálka techniku funguje následujícím způsobem:

1.  Knihovna klienta se předpokládá, že uživatel spravuje klíčové šifrovací klíč (KEK) místně. Uživatel nemusí znát určité klávesy, která byla použita pro šifrování. Místo toho klíčové překládání, které odstraňuje identifikátory klíčové klíčů, můžete nastavit a používat.

2.  Knihovnu klienta stahování zašifrovaných data spolu s jakýkoli šifrování materiál, který je uložený ve službě.

3.  Zalomený obsahu šifrovací klíč (CEK) je nebalených (dešifrovaný) pomocí klíče šifrovací klíč (KEK). Tady znovu knihovnu klienta neměl přístup k KEK. Jednoduše vyvolá rozbalení algoritmus vlastního poskytovatele.

4.  Obsahu šifrovací klíč (CEK) je pak použít dešifrovat šifrované uživatelská data.

## <a name="encryption-mechanism"></a>Šifrování mechanismus  
Knihovna úložiště klienta používá [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) k šifrování uživatelská data. Konkrétně [Šifrování blok zřetězení CBC](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režim AES. Jednotlivé trochu jinak služby funguje tak, aby se probereme tato témata každý z nich tady.

### <a name="blobs"></a>Objekty BLOB
Knihovnu klienta v současné době podporuje šifrování pouze celé objektů BLOB. Konkrétně šifrování je podporované, když uživatelé používat **vytvořit*** metody. Jsou podporovány, obě dokončení a soubory ke stažení oblast stahování a paralelního zpracování obou nahrávání a stahování je k dispozici.

Při šifrování bude knihovnu klienta Generovat náhodné inicializace vektorové IV 16 bajtů, spolu s náhodné obsahu šifrovací klíč (CEK) 32 bajtů a proveďte obálky šifrování objektů blob dat z těchto informací. Zalomený CEK a některá další šifrování metadata jsou uložena jako objektů blob metadat spolu s šifrované objektů blob na služby.

>[AZURE.WARNING] Pokud jsou úpravy nebo odesílání vlastní metadat pro objekt blob, musíte zajistit, aby se zachová tato metadata. Pokud odešlete nová metadata bez tato metadata zalomený CEK, IV a ostatních metadatech, které budou ztracena a objektů blob obsah nikdy bude zobrazitelné ve výsledcích vyhledávání znovu.

Stahování zašifrovaných objektů blob zahrnuje načtení obsahu celý objektů blob pomocí **získat*** vhodné metody. Zalomený CEK nebalený a používat společně s IV (uložené v tomto případě jako objektů blob metadata) se vrátíte dešifrovaná data uživatelům.

Stahování libovolného oblasti (**získat*** metody s parametry rozsahu předaný) v šifrované objektů blob zahrnuje úpravy rozsahu poskytovanou uživatelů, abyste mohli získávat malou část Další data, která mohou sloužit k úspěšně dešifrování požadovaný rozsah.

Objekty BLOB bloku a stránku objektů BLOB pouze může být zašifrovaných/dešifrování pomocí tohoto režimu. Je aktuálně žádnou podporu pro šifrování připojení objektů BLOB.

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

3.  Zalomený CEK a některá další šifrování metadata potom uložená jako dvě doplňující vlastnosti rezervovaná. První vyhrazená vlastnost (\_ClientEncryptionMetadata1) je vlastnost řetězec s informacemi o IV, verze a zalomený klíče. Druhý vyhrazená vlastnost (\_ClientEncryptionMetadata2) je binární vlastnost, která obsahuje informace o vlastnostech, které jsou šifrované. Informace v této druhý vlastnosti (\_ClientEncryptionMetadata2) je sám šifrovaná.

4.  Z důvodu těchto dalších rezervovaná vlastností potřebných pro šifrování uživatelé nyní může mít jenom 250 vlastní vlastnosti místo 252. Celková velikost entity musí být menší než 1MB.

    Všimněte si, že je možné zašifrovat pouze řetězec vlastnosti. Pokud jiné druhy vlastnosti jsou šifrovány, musí být převáděny na řetězce. Jsou zašifrované řetězce jsou uložené ve službě jako binární vlastnosti a jsou převáděny zpátky na řetězce (jako nezpracovaná řetězce, ne EntityProperties s typem EdmType.STRING) po dešifrování.

    U tabulek, kromě zásady šifrování uživatelé musí zadat vlastnosti šifrování. Tím se teď dá uložením některý z těchto vlastností v TableEntity objektů s typ nastavit na stav EdmType.STRING a šifrování nastavena na hodnotu true nebo nastavení encryption_resolver_function tableservice objektu. Překládání šifrování je funkci, která trvá klíč oddílu, klíče řádku a název vlastnosti a vrátí logickou hodnotu, která označuje, zda má být zašifrován této vlastnosti. Při šifrování, knihovnu klienta se tyto informace použít k rozhodnout, jestli má být při psaní lince zašifrován vlastnost. Delegát taky poskytuje možnost použití logických operátorů kolem jak jsou šifrované vlastnosti. (Například Pokud argumenty X, pak šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že není nutné zadávat zadejte tyto informace při čtení nebo dotazování entity.

### <a name="batch-operations"></a>Dávkové operace
Jedna zásada šifrování platí pro všechny řádky listu. Knihovnu klienta interně vygeneruje nové náhodné IV a náhodné CEK na řádku na listu. Uživatelé mohou také k šifrování různé vlastnosti pro každé operaci dávku tím, že definujete toto chování v překládání šifrování.
Pokud vytvoření dávky jako místní odpovědi pomocí metody batch() tableservice zásady šifrování tableservice automaticky použije se s dávkou. Pokud explicitně vytvoření dávky tak, že zavoláte konstruktoru musí být zásady šifrování předané jako parametr a vlevo smíte bez jakýchkoli úprav dobu trvání dávky.
Všimněte si, jak se mají vkládat do dávky pomocí zásady šifrování dávky (entity šifrované není v době potvrzení dávky pomocí zásady šifrování tableservice) jsou šifrované entity.

### <a name="queries"></a>Dotazy
K provádění operací, dotazu, je nutné zadat klíčové překládání, který je vyřešit všechny klíče v sadě výsledků. Pokud entita obsažené ve výsledku dotazu nelze převést na poskytovatele, knihovnu klienta vyvolá chybu. Pro každý dotaz, které provede průzkumy straně serveru knihovnu klienta přidá vlastnosti metadat zvláštní šifrování (\_ClientEncryptionMetadata1 a \_ClientEncryptionMetadata2) ve výchozím nastavení vybrané sloupce.

>[AZURE.IMPORTANT] Mějte na paměti následující důležité bodů při použití šifrování na straně klienta:
>
>- Při čtení nebo zápisu šifrované objektů blob, použijte celou objektů blob upload a oblasti nebo celé objektů blob stažení příkazy. Zamezení zapsání do šifrované objektů blob pomocí protokolu operací, jako je vložit blok, umístění seznamu blokovaných adres, zápisu nebo stránky vymazat; v opačném případě může poškozený šifrované objektů blob a naformátovat ji čitelný.
>
>- U tabulek podobně jako omezení existuje. Dávejte Neaktualizovat šifrované vlastnosti bez aktualizace metadat šifrování.
>
>- Pokud jste nastavili metadat na šifrované objektů blob, může přepsat metadat souvisejících šifrování potřebných pro dešifrování, protože není přidávat nastavení metadata. To platí také k snímky; Vyhněte se určující metadat při vytváření snímek šifrované objektů blob. Pokud se nastaví metadata, je potřeba zavolat **get_blob_metadata** metodu nejprve získat aktuální metadata šifrování a vyhnout souběžné zápisy při nastaví metadata.
>
>- Povolte příznak **require_encryption** objektu služby pro uživatele, kteří mají pracovat jenom s šifrovaná data. Další informace najdete níže.

Knihovně úložiště klienta očekává ujednaných KEK a klíče překládání provádět následující rozhraní. Podpora [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/) správy Python KEK čeká na vyřízení a bude do této knihovny po dokončení.


## <a name="client-api--interface"></a>Klientského rozhraní API / rozhraní
Po vytvoření objekt úložiště služby (tedy blockblobservice) uživatel může přiřadit hodnoty polí, která představují zásad šifrování: key_encryption_key, key_resolver_function a require_encryption. Uživatelé můžou zadat jenom KEK pouze překládání nebo obojí. key_encryption_key je základní typ klíče, které jsou označeny pomocí klíčových identifikátoru a poskytující logiky pro obtékání/obtékání. key_resolver_function slouží k řešení klíč během procesu dešifrování. Vrátí platné KEK zadaných identifikátoru klíče. To umožňuje uživatelům vybrat mezi několika kláves, které jsou spravovány do několika umístění.

KEK musí implementovat následujících metod k úspěšně šifrování dat:
- wrap_key(cek): zalamuje zadaný CEK (bajtů) pomocí algoritmus výběru uživatele. Vrátí zalomený klíče.
- get_key_wrap_algorithm(): vrátí algoritmus použitý zalomení klíče.
- get_kid(): vrátí id řetězce klíčů pro tento KEK.
KEK musí implementovat následujících metod k úspěšně dešifrování dat:
- unwrap_key (cek, algoritmus): vrátí nebalených formulář zadaný CEK pomocí algoritmu zadán řetězec.
- get_kid(): vrátí id řetězce klíčů pro tento KEK.

Klíčové překládání musí aspoň implementovat metodu, která zadaných klíčové id vrátí odpovídající KEK implementující rozhraní výše. Tento způsob je přidělovat vlastnost key_resolver_function objektu Služba.

- Šifrování používá se vždy a nepřítomnosti klíče bude výsledkem chyba.
- Pro dešifrování:
    - Pokud je zadaná získat klávesu vyvolání klíčové překládání. Pokud překladač není zadán, ale nemá mapování pro identifikátor klíče, vyvolá se chyba.
    - Pokud není zadán překládání, ale není zadán klíč, používá se shoduje s identifikátoru požadované identifikátoru klíče. Pokud identifikátor neodpovídá, vyvolá se chyba.

      Příklady šifrování v azure.storage.samples <fix URL>ukazují podrobnější začátku do konce situace pro objekty BLOB, dotazů a tabulek.
        Ukázka implementace KEK a klíče překládání jsou uvedeny v ukázkové soubory jako KeyWrapper a KeyResolver v tomto pořadí.

### <a name="requireencryption-mode"></a>Režim RequireEncryption
Uživatele můžete volitelně povolit režim operace, které musí být všechny nahrávání a stahování zašifrován. V tomto režimu pokusy data bez zásad šifrování nahrávání nebo stahování data, která není zašifrovaných na službě selžou na straně klienta. Příznak **require_encryption** objektu Služba mezery nastavuje toto chování.

### <a name="blob-service-encryption"></a>Šifrování služby objektů BLOB
Nastavení zásad polí šifrování blockblobservice objektu. Všechny ostatní bude zpracována knihovnu klienta interně.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Šifrování služby fronty
Nastavení zásad polí šifrování queueservice objektu. Všechny ostatní bude zpracována knihovnu klienta interně.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Tabulku služba šifrování
Kromě vytváření zásad šifrování a nastavíte na žádost o možnostech, musíte zadat **encryption_resolver_function** na **tableservice**nebo nastavit atribut zašifrovat EntityProperty.

### <a name="using-the-resolver"></a>Použití překladač

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Pomocí atributů
Výše uvedené, může vlastnost označena šifrování ukládání do objektu EntityProperty a nastavením pole šifrování.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Šifrování a výkon
Všimněte si, že šifrování výsledky datový úložiště v nároky na další výkon. Je třeba vygenerovat obsahu klíče a IV třeba zašifrovat obsah a další metadata musí být naformátovaná a odeslat. Tento režijních budou lišit podle toho množství dat šifrovány. Doporučujeme, aby zákazníci testujte žádostí o výkonu během vývoje.

## <a name="next-steps"></a>Další kroky

- Stáhněte si [Azure úložiště klienta knihovny Java PyPi balíčku](https://pypi.python.org/pypi/azure-storage)
- Stáhněte si [Azure úložiště klienta v knihovně Python zdrojového kódu z GitHub](https://github.com/Azure/azure-storage-python)
