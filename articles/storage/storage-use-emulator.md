<properties 
    pageTitle="Použití emulátoru Azure úložiště pro vývoj a testování | Microsoft Azure" 
    description="Azure úložiště emulátoru poskytuje zdarma místní vývojové prostředí pro vývoj a otestování Azure úložiště. Informace o emulátoru úložiště, včetně jak ověřit požadavky, jak se připojit k emulátor z aplikace a jak používat nástroj příkazového řádku." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Použití emulátoru Azure úložiště pro vývoj a testování

## <a name="overview"></a>Základní informace

Úložiště emulátoru Microsoft Azure poskytuje místním prostředí, která napodobí objektů Blob Azure, fronty a tabulky služby pro účely vývoj. Pomocí emulátoru úložiště, můžete otestovat aplikace proti úložiště služby místně, bez vytvoření předplatné Azure nebo by žádné náklady. Až budete spokojeni s jak funguje aplikace emulátor, můžete přejít pomocí účet Azure úložiště v cloudu.

> [AZURE.NOTE] Emulátoru úložiště je k dispozici jako součást [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Můžete taky nainstalovat emulátoru úložiště pomocí [samostatný instalační program](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409). Abyste mohli nakonfigurovat emulátoru úložiště, musíte mít oprávnění správce v počítači.
> 
> Úložiště emulátoru aktuálně spustí pouze v systému Windows.
>  
> Všimněte si, že nejsou data vytvořený v jedné verze emulátoru úložiště zaručena přístupných osobám s postižením při použití jiné verze. Pokud potřebujete uchovávat data dlouhodobým, je vhodné ukládat tato data v účet Azure úložiště a ne emulátoru úložiště.

## <a name="how-the-storage-emulator-works"></a>Jak funguje emulátoru úložiště
 
Úložiště emulátoru používá místní instanci aplikace Microsoft SQL Server a místním pevném disku Chcete-li emulovat služby Azure úložiště. Ve výchozím nastavení používá emulátoru úložiště databáze v aplikaci Microsoft SQL Server 2012 Express LocalDB.  Je možné konfigurovat emulátoru úložiště pro přístup k místní instanci systému SQL Server místo LocalDB instance. Další informace naleznete v tématu [spuštění a inicializace emulátoru úložiště](#start-and-initialize-the-storage-emulator) pod.

Můžete nainstalovat SQL Server Management Studio Express ke správě instalace LocalDB. Úložiště emulátoru se připojí k serveru SQL Server nebo LocalDB pomocí ověřování Windows. 

Některé funkce rozdíly mezi emulace úložiště a služby Azure úložiště. Další informace o těchto rozdílů přečtěte si článek [rozdíly mezi emulátoru úložiště a úložišti Azure](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Ověřování požadavky emulátoru úložiště

Stejně jako v úložišti Azure v cloudu, musí být ověřeny každý požadavek, díky kterým bude proti emulátoru úložiště, pokud je žádost o konverzaci anonymní. Může ověřovat žádosti o proti emulátoru úložiště pomocí ověřování sdílený klíč nebo s sdílených přístup podpisem (přidružení zabezpečení).

### <a name="authentication-with-shared-key-credentials"></a>Ověřování s sdílených klíč přihlašovací údaje

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Podrobné informace o připojení řetězce najdete v článku [Konfigurace Azure úložiště připojovací řetězec](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Ověřování s sdílených přístup podpisem 

Některé knihoven Azure úložiště klienta, například národní knihovna Xamarin pouze podporuje ověřování s sdílených přístupový token podpisu (přidružení zabezpečení). Bude potřeba vytvořit tento přidružení zabezpečení token nástrojem nebo aplikace, která podporuje ověřování sdílené klíče. Snadný způsob, jak získat token přidružení zabezpečení, spočívá ve využití Azure Powershellu:

1. Pokud jste to ještě neudělali, nainstalujte prostředí PowerShell Azure. Doporučujeme používat nejnovější verze rutiny prostředí PowerShell Azure. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md#Install) pokyny k instalaci.

2. Otevřete Azure PowerShell a následující příkazy. Nezapomeňte nahradit *ACCOUNT_NAME* a *ACCOUNT_KEY ==* pomocí vlastní přihlašovacích údajů. *CONTAINER_NAME* nahraďte názvem vašeho výběru.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Výsledný podpis sdílený přístup URI nové kontejneru by měl být stejný tento:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Sdílený přístup podpis vytvořený v tomto příkladu je platný pro jeden den. Podpis uděluje plný přístup (tedy pro čtení, zapsat, odstranit, seznam) na objekty BLOB v kontejneru.

Další informace o podpisech sdílený přístup najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Zahájení a inicializace emulátoru úložiště

Spuštění emulátoru Azure úložiště, klikněte na tlačítko Start a stiskněte klávesu Windows. Začněte psát **Azure úložiště emulátoru**a vyberte emulátor ze seznamu aplikací. 

Když emulátor pracuje, uvidíte ikonu v oznamovací oblasti hlavního panelu Windows.

Při spuštění emulátoru úložiště, zobrazí se okno příkazového řádku. Můžete použít toto okno příkazového řádku pro zahájení a ukončení emulátoru úložiště i vymazat data, aktuální stav a inicializace emulátor. Další informace najdete v článku Principy [úložiště emulátoru příkazového řádku nástroj](#storage-emulator-command-line-tool-reference).

Při zavření okna příkazového řádku emulátoru úložiště pracuje. Přepínač příkazového řádku vyvoláte znovu, výše uvedených kroků jako při spuštění emulátoru úložiště.

Při prvním spuštění emulátoru úložiště prostředí místní úložiště smyčky podle za vás. Inicializační proces vytvoří databázi v LocalDB a rezervuje porty protokolu HTTP pro každou službu místní úložiště. 

Ve výchozím nastavení k adresáři C:\Program Files (x86) \Microsoft SDKs\Azure\Storage emulátoru je nainstalovaný emulátoru úložiště. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Inicializace emulátoru úložiště použít jinou databázi SQL

Inicializace emulátoru úložiště tak, aby ukazovaly na instanci SQL databáze než výchozí instanci LocalDB můžete nástroj úložiště emulátoru příkazového řádku. Všimněte si, že musíte mít spuštěný nástroj příkazového řádku s oprávněními správce inicializace back-end databáze pro emulátoru úložiště:

1. Klikněte na tlačítko **Start** nebo stiskněte klávesu **Windows** . Začněte psát `Azure Storage Emulator` a vyberte ho, když se objeví vyvoláte nástroj úložiště emulátoru příkazového řádku.
2. V okně příkazového řádku zadejte tento příkaz, kde `<SQLServerInstance>` je název instance serveru SQL Server. Chcete-li použít LocalDb, zadejte `(localdb)\v11.0` jako instanci systému SQL Server.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Můžete taky pomocí následujícího příkazu, který směruje emulátoru použít výchozí instanci systému SQL Server:

        AzureStorageEmulator init /server .\\ 

    Nebo můžete použít příkaz, který znovu inicializuje databáze pro výchozí instanci LocalDB:

        AzureStorageEmulator init /forceCreate 

Další informace o příkazech najdete v článku Principy [úložiště emulátoru příkazového řádku nástroj](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Přiřazování prostředků v emulátoru úložiště

Koncové body služby pro ukládání emulátoru se liší od těch účet Azure úložiště. Rozdíl je tomu, že místního počítače neprovádí rozlišení název domény, aby koncové body emulátoru úložiště vyžadují místní adresa místo názvu domény.

Při adresování zdroj v účet Azure úložiště použijte následující schéma, kde název účtu zadejte část názvu host URI a je zdroj určeno část cesty URI:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Například následující identifikátor URI je platná adresa pro objektů blob Azure úložiště účtu:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

V emulátoru úložiště protože místního počítače neprovádí překlad názvů domén název účtu je součástí cestu URI místo názvu hostitele. Použijte následující schéma zdroje aplikaci emulátoru úložiště:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Následující adresu například může použít pro přístup k objektů blob v emulátoru úložiště:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Koncové body služby pro ukládání emulátoru jsou:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adresování sekundární s Vzdálená pomoc GRS účtu

Začínající podverzí 3.1 úložiště emulátoru účet podporuje geo nadbytečné replikace přístup pro čtení (Vzdálená pomoc GRS). Úložiště zdrojů v cloudu a místní emulátoru, můžete získat přístup k sekundární umístění o přidávání - sekundární na název účtu. Následující adresu například může použít pro přístup k objektů blob pomocí vedlejší jen pro čtení v emulátoru úložiště:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Programový přístup k sekundární pomocí emulátoru úložiště vyberte knihovnu úložiště klienta .NET verze 3,2 nebo novější. Naleznete na webu [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) podrobnosti.

## <a name="storage-emulator-command-line-tool-reference"></a>Úložiště emulátoru příkazového řádku nástroje odkazu

V aplikacích od verze 3.0, když spustíte emulátoru úložiště, zobrazí se okno příkazového řádku překryvné. Pomocí okna příkazového řádku na spustit a zastavit emulátoru taky tak, aby dotaz na stav a provádění dalších operací.

> [AZURE.NOTE] Pokud máte Microsoft Azure výpočet emulátoru nainstalovaný, ikonu na hlavním panelu se zobrazí při spuštění emulátoru úložiště. Klikněte pravým tlačítkem myši na ikonu nabídku, která nabízí grafické způsob, jak spustit a zastavit emulátoru úložiště.

### <a name="command-line-syntax"></a>Syntaxe příkazového řádku

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Možnosti

Chcete-li zobrazit seznam možností, zadejte `/help` na příkazovém řádku.

| Možnost | Popis                                                    | Příkaz                                                                                                 | Argumenty                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Zahájení**  | Spuštění systému emulátoru úložiště.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: zahájení emulátor v aktuálním procesu místo abyste vytvářeli nového obrázku.                          |
| **Stop**   | Ukončí emulátoru úložiště.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Stav** | Vytiskne stav emulátoru úložiště.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Vymazání**  | Odstraní data ve všech služeb zadanou v příkazového řádku. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *kulatý*: Vymaže objektů blob data. <br/>*Fronta*: Vymaže fronty data. <br/>*Tabulka*: Odstraní data v tabulce. <br/>*všechny*: Odstraní všechna data v všechny služby. |
| **Spuštění**   | Provede jednorázové inicializace nastavit emulátor.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-server serverName\instanceName*: Určuje hostingu instanci systému SQL server. <br/>*-sqlinstance Název_instance*: název instance serveru SQL se nemusí používat v instanci výchozího serveru. <br/>*-forcecreate*: Vynutí vytvoření databáze SQL, i když už existuje. <br/>*-inprocess*: provádí inicializace v aktuálním procesu místo vytváření nového obrázku. Aktuální proces se zvýšenými oprávněními k provedení inicializace musí Snadné spuštění.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Rozdíly mezi emulátoru úložiště a úložišti Azure

Protože emulátoru úložiště je emulované prostředí spuštěné v místním instance serveru SQL, jsou určité rozdíly ve funkci mezi emulátor a účet Azure úložiště v cloudu:

- Emulátoru úložiště podporuje pouze jeden pevné účet a dobře známé ověřování klíče.

- Úložiště emulátoru není scalable úložiště služby a nebude podporovat velký počet souběžné klientů.

- Podle popisu v [adresování zdrojů v emulátoru úložiště](#addressing-resources-in-the-storage-emulator), zdroje jsou řeší jinak emulátoru úložiště versus účet Azure úložiště. Odchylka je tomu, že řešení název domény je k dispozici v cloudu, ale ne v místním počítači.

- Začínající podverzí 3.1 úložiště emulátoru účet podporuje geo nadbytečné replikace přístup pro čtení (Vzdálená pomoc GRS). V emulátoru všechny účty mají povoleno Vzdálená pomoc GRS a replik primárních a sekundárních je nikdy jakékoli prodlevy. Operace získat stat služby objektů Blob získat stat fronty služby a získat tabulku služba stat jsou podporovány na vedlejší účtu a vždy vrátí hodnotu `LastSyncTime` odpověď elementu jako aktuální čas podle podkladových databáze SQL.

    Programový přístup k sekundární pomocí emulátoru úložiště vyberte knihovnu úložiště klienta .NET verze 3,2 nebo novější. Naleznete na webu [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) podrobnosti.

- Soubor služby a koncové body služby protokolu SMB nepodporují aktuálně emulátoru úložiště.

- Úložiště emulátoru vrátí chybu VersionNotSupportedByEmulator (HTTP stavový kód 400 - Chybný požádat o) používáte verze služby úložiště, který není podporován verzi emulátoru, který používáte.

### <a name="differences-for-blob-storage"></a>Rozdíly k úložišti objektů Blob 

Následující rozdíly použít k úložišti objektů Blob v emulátor:

- Kulatý pouze podporuje úložiště emulátoru velikosti až do velikosti 2 GB.

- Operace umístění objektů Blob opakujte proti objektů blob existuje v emulátoru úložiště, který obsahuje aktivní zápůjčku, i když ID zapůjčení nebyla zadána v rámci žádosti o. 

- Přidání objektů Blob operace nejsou podporovány emulátorem. Pokoušející operaci objektů blob přidávacího vrátí chybu FeatureNotSupportedByEmulator (HTTP stavový kód 400 - Chybný požádat o).

### <a name="differences-for-table-storage"></a>Rozdíly úložiště tabulek 

Následující rozdíly použít k úložišti tabulek v emulátor:

- Kalendářní data ve službě tabulky v emulátoru úložiště podporují pouze rozsah podporovaný SQL Server 2005 (*tedy*jsou musí být vyšší než 1. ledna 1753). Všechna kalendářní data před 1. ledna 1753 se změní na tuto hodnotu. Přesnost kalendářní data se omezí na přesnosti SQL Server 2005, což znamená, že data jsou přesné 1/300th sekundy.

- Úložiště emulátoru podporuje oddíl klíče a řádek klíčové nemovitostí s hodnotou nižší než 512 bajtů. Kromě toho celkovou velikost název účtu, název tabulky a názvy klíčové vlastností společném nesmí překročit 900 bajtů.

- Celkový počet řádků v tabulce v emulátoru úložiště se omezí na méně než 1 MB.

- Do úložiště emulátoru vlastnosti dat zadejte `Edm.Guid` nebo `Edm.Binary` podporují pouze `Equal (eq)` a `NotEqual (ne)` relační operátory v dotazu filtrovat řetězce.

### <a name="differences-for-queue-storage"></a>Rozdíly úložištěm fronty

Specifické pro fronty úložiště na emulátor nejsou žádné rozdíly.

## <a name="storage-emulator-release-notes"></a>Poznámky k verzi emulátoru úložiště

### <a name="version-45"></a>Verze 4.5

- Pevná chyba, která způsobila inicializace a instalace emulátoru úložiště dojde k chybě při přejmenovaná zálohování databáze.

### <a name="version-44"></a>Verze 4.4

- Úložiště emulátoru nyní podporuje 2015-12-11 verzi služby úložiště na koncové body Blob fronty a tabulky služby.

- Uvolnění paměti emulátoru úložiště objektů blob dat je teď efektivnější týkají s velkým počtem objektů BLOB.

- Pevná chyba, která způsobila kontejneru ACL XML z jak služba úložiště znamená ověření trochu jinak.

- Pevná chyby může někdy dojít maximální a minimální hodnota hodnoty data a času uvedená v nesprávné časové pásmo.

### <a name="version-43"></a>Verze 4.3

- Úložiště emulátoru nyní podporuje 2015 07 08 verzi úložiště služby na koncové body Blob fronty a tabulky služby.

### <a name="version-42"></a>Verze 4.2

- Úložiště emulátoru nyní podporuje 2015 04 05 verzi služby úložiště na koncové body Blob fronty a tabulky služby.

### <a name="version-41"></a>Verze 4.1

- Úložiště emulátoru nyní podporuje verzi 2015 02 21 stránky služby na úložiště objektů Blob, fronty a tabulku služba koncovém, s výjimkou nových funkcí připojení objektů Blob. 

- Emulátoru úložiště nyní vrátí smysluplné chybová zpráva, pokud používáte verzi úložiště služby, který není podporován tuto verzi emulátor. Doporučujeme používat nejnovější verze emulátoru. Pokud dojde k chybě VersionNotSupportedByEmulator (HTTP stavový kód 400 - Chybný požádat o), stáhněte si prosím nejnovější verzi emulátoru úložiště.

- Pevná chybu, ve kterém závodní podmínka způsobil entity data tabulky jako nesprávná během operace souběžné korespondence.

### <a name="version-40"></a>Verze 4.0

- Spustitelný úložiště emulátoru přejmenoval na *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Verze 3,2

- Úložiště emulátoru nyní podporuje 2014-02-14 verzi služby úložiště na koncové body Blob fronty a tabulky služby. Všimněte si, že koncové body služby souboru nepodporuje aktuálně emulátoru úložiště. Další informace o verzi 2014-02-14 najdete v článku [Správa verzí úložiště služby Azure](https://msdn.microsoft.com/library/azure/dd894041.aspx) .

### <a name="version-31"></a>Podverzí 3.1

- Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS) teď podporuje emulátoru úložiště. Získání stat služby objektů Blob, získat stat fronty služby a získat tabulku služba stat API podporují sekundární účtu a vždy vrátí hodnotu prvku odpověď LastSyncTime jako aktuální čas podle podkladových databáze SQL. Programový přístup k sekundární pomocí emulátoru úložiště vyberte knihovnu úložiště klienta .NET verze 3,2 nebo novější. Podrobnosti získáte v knihovně Microsoft Azure úložiště klienta pro referenci .NET.

### <a name="version-30"></a>Verze 3.0

- Emulátoru Azure úložiště je už odeslané ve stejném balíčku emulátoru výpočetním.

- Úložiště emulátoru grafické uživatelské rozhraní se nedá použít ve prospěch scriptable rozhraní příkazového řádku. Informace o rozhraní příkazového řádku najdete v článku Principy úložiště emulátoru příkazového řádku nástroj. Grafického rozhraní, zůstanou měly tvořit verze 3.0, ale jenom k němu emulátoru výpočet je nainstalovaná na ikony hlavním panelu kliknete pravým tlačítkem a výběrem uživatelského rozhraní emulátoru zobrazit úložiště.

- Teď je plně podporován verze 2013-08-15 služby Azure úložiště. (Dříve tato verze je podporována pouze emulátorem úložiště verze 2.2.1 náhledu.)
