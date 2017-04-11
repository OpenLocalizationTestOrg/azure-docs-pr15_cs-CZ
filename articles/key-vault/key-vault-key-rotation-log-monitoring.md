<properties
    pageTitle="Jak nastavit klíč trezoru s konce střídání a auditování | Microsoft Azure"
    description="Pomocí této postupy vám pomohou nastavení s střídání a sledování klíčových trezoru protokolů"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Jak nastavit klíč trezoru s konce střídání a auditování

##<a name="introduction"></a>Úvod

Po vytvoření trezoru klíč Azure, budou moct uživatelé zahajovat využití tohoto trezoru ukládání klíče a tajemství. Používání aplikací už potřebovat uchovávat klíčů nebo tajemství, ale nechcete bude požadovat je z klíčové trezoru podle potřeby. Umožňuje aktualizovat klíče a tajemství bez vlivu na chování aplikace, které se objeví široké možnosti kolem kód a tajné řízení chování.

Tento článek provede příklad využívání Azure trezoru klíč pro ukládání tajné, v tomto případě účtu úložiště Azure klávesy, která je používána v aplikaci. Bude taky ukazují provádění plánované otočení tento klíč účtu úložiště. Nakonec ho provede jednotlivými ukázku ke sledování protokolů auditování klíčové trezoru a vysokoškolskou upozornění, když neočekávané žádosti.

> \[AZURE. Poznámka:\] tento kurz není určená k vysvětlit podrobně počáteční sadou nahoru z trezoru klíč Azure. Tyto informace najdete v článku [Začínáme s Azure klíč trezoru](key-vault-get-started.md). Nebo rozhraní příkazového řádku různé platformy, najdete v článku [odpovídající kurzu](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Nastavení KeyVault

Abyste mohli povolit aplikace k načtení tajná z trezoru klíč Azure, musíte nejdřív vytvořit tajná a ho nahrajte do trezoru. Můžete to provést snadno pomocí prostředí PowerShell jak je ukázáno v následujícím příkladu.

Zahájit relaci Powershellu Azure a přihlaste se k účtu Azure pomocí následujícího příkazu:

```powershell
Login-AzureRmAccount
```

V okně místní prohlížeče zadejte svůj účet Azure uživatelské jméno a heslo. Azure Powershellu pošle Všechna předplatná, které jsou přidružené k tomuto účtu a ve výchozím nastavení, použije první z nich.

Pokud máte víc předplatných, pravděpodobně zadat určité, která byla použita k vytvoření trezoru klíč Azure. Zadejte následující příkaz zobrazíte předplatná pro váš účet:

```powershell
Get-AzureRmSubscription
```

Chcete-li předplatné, které máte přidružený k trezoru klíčů, který bude protokolování, zadejte:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Jak tento článek ukazuje, ukládání klíč účtu úložiště jako tajná, musíte získat tento klíč účtu úložiště.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Po načtení skryté, v tomto případě svůj klíč účtu úložiště, musíte převést, zabezpečené řetězec a pak vytvořte tajná s touto hodnotou v trezoru klíče.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Pak můžete získat identifikátor URI tajná, který jste právě vytvořili. Tím se použijí později, když voláte trezoru klíč k načtení skryté. Spuštěním následujícího příkazu Powershellu a poznamenejte si hodnotu "Identifikátor", která je tajná URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Nastavení aplikace

Teď, když máte tajná uložené můžete získat této tajná a použijte ji z kódu. Existuje několik kroků potřebných k tomuto účelu první a nejdůležitější které je aplikace zaregistrovali Azure Active Directory a potom oznámením Azure klíč trezoru informace o aplikaci tak, aby ho povolit žádosti o aplikaci.

> \[AZURE. Poznámka:\] aplikace musí být ve stejném klientovi Azure Active Directory jako trezoru klíč vytvořen. 

Nejdřív otevřete kartu aplikací služby Azure Active Directory

![Otevření aplikace v Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Kliknutím na "přidat přidat novou aplikaci Azure AD

![Zvolte Přidat aplikaci](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Nechte typ aplikace jako "Webové aplikace a či nebo rozhraní API webových" a zadejte název aplikace.

![Název aplikace](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Udělte aplikace "URL přihlašování" "Identifikátor URI aplikace". Tyto může být všechno, co, který se má tato ukázka a bude možné měnit později v případě potřeby.

![Zadejte požadované URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Po přidání aplikace na Azure AD můžete přepnutím do aplikace na stránce. Klikněte na kartu "Konfigurovat" od tohoto okamžiku najděte a zkopírujte hodnotu klienta ID. Poznamenejte si ID klienta pro pozdější kroky.

Další byste potřebovali získat klíč pro aplikace může moct komunikovat s Azure AD. Při vytvoření této části "Klávesy" na kartě "Konfigurace". Poznamenejte si nově generovaného klíče z aplikace Azure AD pro použití v později.

![Azure AD zkratky aplikace](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Před vytvořením volání z aplikace do klíč trezoru byste potřebovali trezoru klíč sdělte aplikace a její "oprávnění. Následující příkaz trvá trezoru jméno a ID klienta z aplikace Azure AD a povolí "získat přístup k trezoru klíčové aplikace.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Teď jste připraveni začít vytvářet aplikace zavolat. V aplikaci se musíte nejdřív nainstalovat balíčků NuGet povinné chcete provést interakci s Azure klíč trezoru a Azure Active Directory. Z konzoly Visual Studio balíčku správce zadejte následující příkazy. Všimněte si, že při psaní tohoto článku aktuální verzi balíček Active Directory 3.10.305231913, tak můžete chtít potvrďte nejnovější verzi a příslušně aktualizovat.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

V kódu aplikace vytvořte class pro uložení metody ověřování služby Active Directory. V tomto příkladu, který třídy jmenuje "Utils". Bude pak potřebujete přidat následující pomocí.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Dále přidejte podle pokynů k načtení JWT token z Azure AD. Pro udržovatelnost můžete chtít přesunout pevné kódovaný řetězcových hodnot do konfiguraci web nebo aplikaci.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Nakonec můžete přidat kód potřebné volat klíč trezoru a získání skryté hodnoty. Nejdřív musíte přidat následující pomocí příkazu.

```csharp
using Microsoft.Azure.KeyVault;
```

Dále přidáte volání metody vyvolat klíč trezoru a načíst skryté. Tato metoda poskytnete tajná URI, kterou jste si uložili v předchozím kroku. Všimněte si, metody GetToken ze třídy Utils vytvořili výše.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Po spuštění aplikace se teď byste měli být ověřování pro službu Azure Active Directory a potom načítání skryté hodnoty z trezoru klíč Azure.

##<a name="key-rotation-using-azure-automation"></a>Otočení klávesu automatické Azure

Provádění otočení strategie pro hodnoty, které obsahují jako Azure klíč trezoru tajemství různými způsoby. Tajemství můžete otáčet jako součást ručního procesu, je možné programově otočením využitím volání rozhraní API nebo může být otočený prostřednictvím skriptu automatizaci. Pro účely tohoto článku jsme využívání Azure PowerShell s Azure automatizaci změnit přístupové klávesy Azure úložiště účtu a potom bude aktualizován klíčové trezoru tajná nový klíč. 

K umožňují automatizaci Azure nastavení tajná hodnot v trezoru klíčové budete potřebovat zobrazíte ID klienta pro připojení s názvem "AzureRunAsConnection", která je vytvořená, pokud jste vytvořili Azure automatizované instance. Toto ID můžete dosáhnout výběru "Aktiva" z Azure automatizované instance. Odtud zvolte "připojení a pak vyberte principu služby"AzureRunAsConnection". Budete chtít poznamenejte si ID aplikace. 

![ID klienta Azure automatizaci](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Zatímco se stále nacházejí v okně prostředky taky můžete zvolit "Moduly". Z modulů vyberte "Galerie" a vyhledejte a "Import" aktualizované verze každé z následujících modulů.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Poznámka:\] při psaní tohoto článku pouze výše uvedených možností poznamenat, moduly nutné aktualizovat skriptu sdílené níže. Pokud zjistíte, když nefunguje automatizaci práce, by měl potvrďte, že máte všechny potřebné moduly a jejich závislosti importovat.

Po načtení ID aplikace pro připojení k automatizaci Azure, musíte zjistit trezoru klíč Azure tuto aplikaci má přístup k aktualizaci tajemství v trezoru. Můžete to provést pomocí následujícího příkazu Powershellu.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Potom bude vyberte zdroj "Runbooks" ve sloupci Azure automatizaci instance a vyberte Přidat postupu Runbook. Vyberte možnost rychle vytvořit. Název vaší postupu runbook a vyberte "Powershellu" psaní postupu runbook. Pokud chcete přidáte popis. Nakonec klikněte na možnost vytvořit,".

![Vytvoření postupu Runbook](./media/keyvault-keyrotation/Create_Runbook.png)

V podokně editoru pro nové postupu runbook vložíte tento skript Powershellu.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

V podokně editoru máte na výběr zkušební podokno chcete otestovat. Po spuštění skript bez chyb můžete vybrat možnost 'Publikovat' a potom můžete použít plán pro postupu runbook zpět v podokně Konfigurace postupu runbook.

##<a name="key-vault-auditing-pipeline"></a>Sestavy auditování trezoru klíč kanálem k odesílání zpráv

Při nastavení trezoru klíč Azure můžete zapnout auditu umožňuje shromáždit protokoly na žádosti o přístup provedené trezoru klíče. Tyto protokoly uložené v určené účet Azure úložiště a můžete pak být extrahované, sledovat a analyzovat. Pod walks prostřednictvím scénář, který využívá Azure funkcí aplikace logiky Azure klíč trezoru auditu protokoly a vytvořte příležitost k odeslání e-mailu po tajemství z trezoru načtením tak, že aplikace, která odpovídají id aplikace ve web appu.

Nejdřív musíte povolit protokolování trezoru klíče. Lze provést pomocí následující příkazy Powershellu (úplné podrobnosti můžete vidět [tady](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Až to povolí, pak protokolů auditování začnou shromažďování zohledňovala určené úložiště. Tyto protokoly bude obsahovat události návod a jsou-li klíč trezorů stahovány a kým. 

> \[AZURE. Poznámka:\] se mají přístup k informacím o protokolování maximálně, 10 minut po klávesu trezoru operace. Ve většině případů bude rychlejší než to.

Dalším krokem je vytvoření [fronty Bus služby Azure](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). To bude, které odesílají protokolů auditování klíčové trezoru. Jednou ve frontě, aplikaci logiky bude zvedněte je a s nimi pracovat. Vytvoření Bus služby je poměrně vážný předávání dál a tady jsou základní kroky:

1. Vytvoření obor Bus službu (pokud ještě nemáte, který chcete použít pro tuto a pak přejděte ke kroku 2).
2. Vyhledejte Bus služby na portálu a vyberte obor názvů, které chcete vytvořit do fronty.
3. Vyberte nový a zvolte služby Bus -> fronty a zadejte požadované informace.
4. Informace o připojení služby Bus použít výběr obor názvů a klikněte na možnost _Informace o připojení_. Tyto informace budete potřebovat pro další části.

Potom se dozvíte, jak [vytvořit funkci Azure](../azure-functions/functions-create-first-azure-function.md) hlasování protokoly trezoru klíč účtu úložiště a vystopovat nové zvláštní události. To bude funkci, která se spustí při plánování.

Vytvoření funkci Azure (zvolte Nový -> funkce aplikace na portálu). Při vytváření můžete použít existující plán hostingu nebo vytvořte nový účet. Může taky rozhodnout pro dynamické hostingu. Najdete další informace o funkci hostingu možnosti [tady](../azure-functions/functions-scale.md).

Po vytvoření funkci Azure přejděte k němu a zvolte časovače (funkce) a C\# na úvodní obrazovce klikněte na **vytvořit** .

![Zahájení zásuvné Azure funkcí](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Na kartě _vývoje_ nahraďte kód run.csx takto:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Poznámka:\] zkontrolujte, že nahradíte proměnné kódu výše tak, aby ukazovaly ke svému účtu úložiště, kde jsou napsali protokoly klíč trezoru Bus služby jste dříve vytvořili a na konkrétní cestu k protokoly úložiště klíčové trezoru.

Funkce vyzvedne nejnovější soubor protokolu z účtu úložiště, kde napsali klíč trezoru protokoly, získá poslední události z tohoto souboru a posune je do fronty Bus služby. Protože tvořená jedním souborem může mít více událostí, například přes celou hodiny, pak vytvoříme _sync.txt_ soubor, který taky sleduje funkci určit časové razítko posledního události, která byla převzít. Zajistíte, že jsme není nabízená jednu událost tisknutím. Tento soubor _sync.txt_ jednoduše obsahuje časové razítko pro poslední došlo k události. Protokoly, po načtení mít je možné řadit podle časového razítka zajistit, že jsou uspořádány správně.

Pro tuto funkci jsme odkazovat několik dalších knihoven, které nejsou k dispozici mimo pole ve funkcích Azure. Zahrnout tyto, potřebujeme Azure funkcí je vyžádat pomocí nuget. Zvolte možnosti _Zobrazení souborů_ 

![Možnosti zobrazení souborů](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

a přidejte nový soubor s názvem _project.json_ s obsahem takto:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Po _uložení_ to bude spouštět Azure funkce ke stažení požadované binární soubory. 

Přejděte na kartu **Integrace** a parametr časovače smysluplně pojmenujte používat ve funkci. Ve výše uvedeného kódu očekává časovače volání _myTimer_. Zadat [výraz CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) následujícím způsobem: 0 \* \* \* \* \* pro časovače, který má za následek funkce, kterou chcete spustit jednou za minutu. 

Na kartě stejné **Integrace** přidáte vstupní, který bude typu _Úložišti objektů Blob Azure_. To bude ukazovat _sync.txt_ souboru, který obsahuje časové razítko poslední událost dívali funkcí. To bude k dispozici ve funkci tak, že název parametru. Ve výše uvedeného kódu vstupní úložišti objektů Blob Azure předpokládá, že název parametru je _inputBlob_. Zvolte místo, kam bude uložený soubor _sync.txt_ účet úložiště (může to být stejné nebo jiné úložiště účtu) a do pole Cesta zadejte cestu, kde je soubor umístěn, ve formátu {container-name}/path/to/sync.txt.

Přidejte výstupní, která budou s typem výstup _Úložišti objektů Blob Azure_ . To bude ukazovat _sync.txt_ soubor, který právě definované ve vstupním. To použije funkci psát časové razítko poslední událost dívali. Kód výše očekává parametr volání _outputBlob_.

Tato funkce je v tomto okamžiku připravení. Zkontrolujte, že přejdete zpět na kartu **vytvořit** a _Uložit_ kód. V okně výstupu chyby kompilace zkontrolujte a opravte můžou být příslušným způsobem. Pokud kompilaci, kód by nyní běžet a každý minuta v protokolech klíč trezoru a nabízená všechny nové zvláštní události na definovaný fronty Bus služby. Měli byste vidět informace zaznamenané zapisovat protokolu okno při každé aktivaci funkce.

###<a name="azure-logic-app"></a>Použití logických operátorů Azure aplikace

Další jsme potřeba vytvořit aplikaci logiky Azure, která vystopovat události, které funkce předání do fronty Bus služby, analyzovat obsah a posílání e-mailu na základě nějaké podmínky, které je spárované.

[Vytvoření aplikace použití logických operátorů](../app-service-logic/app-service-logic-create-a-logic-app.md) tak, že přejdete na nový -> logiky aplikace. 

Po vytvoření aplikaci logiky přejděte k němu a zvolte _Upravit_. V editoru aplikace použití logických operátorů zvolí _Bus frontě_ spravované rozhraní api a zadejte svoje přihlašovací údaje Bus služby pro připojení do fronty.

![Bus aplikaci služby Azure logiky](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Potom vyberte Přidat _podmínku_. Podmínky, přepněte do _Rozšířený editor_ a zadejte tyto údaje, nahrazení APP_ID skutečné APP_ID svojí webové aplikace:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Tento výraz v podstatě vrátí **hodnotu false** nejsou-li vlastnost **ID aplikace** z příchozí události (což je textu zprávy služby Bus) **ID aplikace** aplikace. 

Dále vytvořte akce u možnosti _Pokud ne, nic …_ .

![Azure logiky aplikace vyberte akce](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Akce vyberte _Office 365 – odeslání e-mailu_. Vyplňte pole vytvořit e-mailu k odeslání po definované podmínky vrátí hodnotu false. Pokud nemáte Office 365 může podívat na alternativy k dosažení stejné.

Nyní máte konce kanálu, který jednou za minutu vyhledá nové protokolů auditování klíč trezoru. Které najde nové protokoly, ho budou nabízená je do fronty Bus služby. Použití logických operátorů aplikace bude aktivován hned, jak novou zprávu výkopu ve frontě a ID aplikace v rámci události není podle id aplikace aplikace, volající nakonec odeslat e-mail. 
