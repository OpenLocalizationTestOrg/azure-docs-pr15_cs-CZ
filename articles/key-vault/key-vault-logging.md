<properties
    pageTitle="Protokolování Azure klíčové trezoru | Microsoft Azure"
    description="Pomocí tohoto kurzu vám pomůžou začít s Azure klíč trezoru protokolování."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure klíčové trezoru protokolování #
Azure trezoru klíč je k dispozici ve většině oblastech. Další informace najdete v tématu [klíč trezoru ceny stránky](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod  
Po vytvoření jeden nebo více kláves trezorů budete pravděpodobně chtít sledovat způsob a četnost klíčové trezorů se k nim přistupovat, kým. Můžete to udělat povolením protokolování pro trezoru klíč, který ukládá informace v Azure úložiště účtu, který zadáte. Nové **přehledy protokoly auditevent** kontejner se automaticky vytvoří pro váš účet zadaný úložiště a použijete tento stejný účet úložiště pro shromažďování protokoly pro více klíčové trezorů.

Se mají přístup k informacím o protokolování maximálně, 10 minut po klávesu trezoru operace. Ve většině případů bude rychlejší než to.  Je to na spravovat protokolů ve vašem účtu úložiště:

- Pomocí metody ovládací prvek standardní Azure přístupu zabezpečené protokolů omezením, kdo má přístup.
- Odstranění protokolů, které už nechcete mít ve vašem účtu úložiště.

Použití tohoto kurzu, který vám pomůžou začít pracovat s Azure klíč trezoru protokolování pro vytvoření účtu úložiště povolit protokolování a interpretovat informace zaznamenané do protokolu shromažďované.  


>[AZURE.NOTE]  Tento kurz nezahrnuje pokyny, jak vytvořit klíčové trezorů, klíče a tajemství. Tyto informace najdete v článku [Začínáme s Azure klíč trezoru](key-vault-get-started.md). Nebo rozhraní příkazového řádku různé platformy, najdete v článku [odpovídající kurzu](key-vault-manage-with-cli.md).
>
>V současné době nejde nakonfigurovat Azure klíč trezoru Azure portálu. Použijte tyto pokyny Azure Powershellu.

Protokoly, které se shromažďují můžete vizualizovat pomocí protokolu analýzy ze sady operace správy. Další informace najdete v tématu [řešení Azure klíč trezoru (verze Preview) v protokolu analýzy](../log-analytics/log-analytics-azure-key-vault.md).

Základní informace o trezoru klíč Azure, najdete v článku [Co je Azure klíč trezoru?](key-vault-whatis.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz musí mít takto:

- Existující klíčové trezoru, který jste používali.  
- **Minimální verze 1.0.1**Azure Powershellu. Instalace prostředí PowerShell Azure a přidružit Azure předplatné, najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Pokud jste již nainstalovali Azure PowerShell a nebudou vědět, požadovanou verzi z konzoly Azure Powershellu, zadejte `(Get-Module azure -ListAvailable).Version`.  
- Protokoly dostatečné úložiště na Azure trezoru klíče.


## <a id="connect"></a>Připojení k předplatného ##

Zahájit relaci Powershellu Azure a přihlaste se k účtu Azure pomocí následujícího příkazu:  

    Login-AzureRmAccount

V okně místní prohlížeče zadejte svůj účet Azure uživatelské jméno a heslo. Azure Powershellu pošle Všechna předplatná, které jsou přidružené k tomuto účtu a ve výchozím nastavení, použije první z nich.

Pokud máte víc předplatných, pravděpodobně zadat určité, která byla použita k vytvoření trezoru klíč Azure. Zadejte následující příkaz zobrazíte předplatná pro váš účet:

    Get-AzureRmSubscription

Chcete-li předplatné, které máte přidružený k trezoru klíčů, který bude protokolování, zadejte:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Další informace o konfiguraci Azure Powershellu najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).


## <a id="storage"></a>Vytvoření nového účtu úložiště pro vaše protokoly ##

I když používáte existujícího účtu úložiště pro vaše protokoly vytvoříte nový účet úložiště, který se snaží protokoly klíč trezoru. Pro usnadnění pro při máme to později určit jsme budete slouží k uložení podrobností do proměnné s názvem **správce systému**.

Pro další usnadnění správy taky použijeme stejnou skupina zdroje jako ten, který obsahuje naše klíčové trezoru. [Kurz Začínáme](key-vault-get-started.md)tato skupina zdroje je s názvem **ContosoResourceGroup** a jsme budete dál používat jihovýchodní Asie umístění. Nahraďte tyto hodnoty pro vlastní, případně:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Pokud se rozhodnete použít existující účet úložiště, musíte použít stejném předplatném jako trezoru klíče a musí používat nasazení modelu správce prostředků, nikoli nasazení modelu Klasický.

## <a id="identify"></a>Určení klíčové trezoru protokolů ##

V našem dosáhnout toho, aby Začínáme kurzu naše klíčové trezoru název byl **ContosoKeyVault**, abychom budete pokračovat pomocí tohoto názvu a uložení podrobností do proměnné s názvem **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Povolení protokolování ##

Chcete-li povolit protokolování pro trezoru klíč, použijeme rutinu Set-AzureRmDiagnosticSetting společně s proměnných, kterou jsme vytvořili našeho nového účtu úložiště a naše klíčové trezoru. Budete taky nastavit **-povolené** příznak **$true** a nastavení kategorie AuditEvent (pouze kategorii klíč trezoru protokolování):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Výstup pro tuto zahrnuje:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


To znamená, že je u klíčových trezoru ukládání informací ke svému účtu úložiště teď povoleno protokolování.

Volitelně můžete také nastavit zásady uchovávání informací u protokolů tak, aby se starším protokoly se automaticky odeberou. Třeba nastavit zásady uchovávání informací pomocí **- RetentionEnabled** příznak **$true** a nastavení parametrů **- RetentionInDays** **90** tak, aby protokoly starší než 90 dnů se automaticky odeberou.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Co je přihlášení k lyncu:

- Všechny ověřené rozhraní REST API žádosti o přihlášení k lyncu, což zahrnuje neúspěšných požadavků důsledku přístupová oprávnění, chyby nebo chybná žádosti.
- Operace na klávesu vault sebe sama zahrnující vytváření, odstraňování, zásady přístupu klíčové trezoru nastavení a aktualizace klíčové trezoru atributů jako značky.
- Operace na klíče a tajemství klíčové trezoru, které bude zahrnovat vytvoření, úprava nebo odstranění těchto kláves nebo tajemství; operací, jako je znaménko, ověřte, šifrování, dešifrování, zalomit a Zrušit zalomení klíče, tajemství, seznam klíče a tajemství a jejich verze.
- Neověřené požadavky, jejichž výsledkem je 401 odpověď. Například požadavky, které nemají nosný token nebo jsou chybně nebo vypršela platnost nebo mají neplatný token.  


## <a id="access"></a>Přístup k protokolů ##

Klíčové trezoru protokoly jsou uloženy v kontejneru **přehledy protokoly auditevent** v úložiště účet, který jste zadali. Chcete-li zobrazit všechny objekty BLOB v tomto kontejneru, zadejte:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Výstup bude vypadat nějak takto:

**Identifikátor Uri kontejner: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Jméno**

**----**

**resourceId = / PŘEDPLATNÁ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/poskytovatelů/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / PŘEDPLATNÁ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/poskytovatelů/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / PŘEDPLATNÁ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/poskytovatelů/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Jak je vidět v tomto výstup objekty BLOB postupujte pojmenování: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Hodnoty data a času pomocí UTC.

Protože stejný účet úložiště mohou sloužit k shromáždit protokoly pro více zdrojů, je velmi užitečné pro přístup k nebo stáhnout jenom objekty BLOB, které potřebujete číslo ID zdroje úplný název objektů blob. Ale předtím jsme ukazuje nejdřív podrobný postup stáhnout všechny objekty BLOB.

Nejprve vytvořte složku, do které stáhnout objekty BLOB. Příklad:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Pak se zobrazí seznam všech objektů blob:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Tento seznam prostřednictvím "Get-AzureStorageBlobContent" stažení objekty BLOB do naší cílovou složku kanálu:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Když spustíte tento příkaz druhý **/** oddělovač v názvech objektů blob vytvořte strukturu úplné složek v části cílovou složku a tuto strukturu se použijí k stáhnout a ukládat objekty BLOB jako soubory.

Stáhnout objektů BLOB, pomocí zástupných znaků. Příklad:

- Pokud máte víc klíčové trezorů a chcete stáhnout protokoly pro jediným klíčové trezoru s názvem CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Pokud máte víc skupiny zdrojů a chcete stáhnout protokoly pro skupinu jediným zdrojů, `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Pokud chcete stáhnout všechny protokoly pro měsíc leden 2016, použijte `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Teď jste připraveni začít prohlížíte Novinky v protokolech. Ale teprve pak přejděte na toho dva další parametry pro Get-AzureRmDiagnosticSetting, které můžete potřebovat vědět:

- K vytvoření dotazu stav diagnostiky nastavení klíčové trezoru zdroje:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Jak zakázat protokolování pro klíčové trezoru zdroje:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Interpretace klíč trezoru protokoly ##

Jednotlivé objekty BLOB uložená jako text ve formátu JSON objektů blob. Toto je položka protokolu příklad spustila `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


V následující tabulce jsou uvedeny názvy polí a popisy.


| Název pole        | Popis |
| ------------- |-------------|
| čas      | Datum a čas (UTC).|
| resourceId      | Číslo ID zdroje Azure správce prostředků. Klíč trezoru protokoly to je vždy ID klíč trezoru zdroje.|
| Název operace      | Název operace, jak je uvedeno v následující tabulce.|
| operationVersion      | Toto je verze rozhraní REST API požadované klientem.|
| kategorie      | Klíč trezoru protokolů AuditEvent hodnotu jedné, která je dostupná.|
| resultType      | Výsledek rozhraní REST API žádosti o.|
| resultSignature      | Stav HTTP.|
| resultDescription     | Další popis o výsledek, pokud jsou dostupné.|
| durationMs      | Doba trvání pro zpracování požadavku rozhraní REST API v milisekundách. Aby čase změřit na straně klienta může se stát odpovídaly tentokrát nezahrnuje latence sítě.|
| callerIpAddress      | IP adresa klienta, kdo zadal požadavek na.|
| correlationId      | Volitelné GUID, který klienta předat sladit protokoly na straně klienta se protokoly na straně služby (klíč trezoru).|
| identity      | Identita z tokenu, který byl prezentovat při vytváření žádosti o rozhraní REST API. Je to obvykle "uživatel", "objekt zabezpečení služeb" nebo "uživatele + ID aplikace" kombinace jako na velká první písmena žádost o výsledkem rutiny prostředí PowerShell Azure.|
| Vlastnosti      | Toto pole bude obsahovat různé jednotlivé informace na základě operace (název operace). Ve většině případů obsahuje informace o klientovi (useragent řetězec předaných klienta) přesné rozhraní REST API žádosti URI a HTTP stavový kód. Kromě toho když objektu je vrácena důsledku žádost (například KeyCreate nebo VaultGet) bude taky obsahovat klíč URI (jako "id"), trezoru URI nebo tajná URI.|




Hodnoty polí **název operace** jsou ve formátu ObjectVerb. Příklad:

- Všechny operace klíčové trezoru mají "trezoru`<action>`" naformátujete tak, jako například `VaultGet` a `VaultCreate`.

- Mají všechny klíčové operace "klávesy`<action>`" naformátujete tak, jako například `KeySign` a `KeyList`.

- Všechny operace tajné mají "tajná`<action>`" naformátujete tak, jako například `SecretGet` a `SecretListVersions`.

V následující tabulce jsou uvedeny název operace a odpovídající příkaz rozhraní REST API.

| Název operace        | Příkaz REST API |
| ------------- |-------------|
| Ověřování      | Prostřednictvím koncového bodu služby Azure Active Directory|
| VaultGet      | [Získat informace o klíčových trezoru](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Vytvoření nebo aktualizace klíčové trezoru](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Odstranění klíčové trezoru](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Aktualizace klíčové trezoru](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Seznam všech klíčové trezorů ve skupině zdroje](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Vytváření klíče](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Získat informace o klíči](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Naimportujte trezoru klíč](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Zálohování klíče](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Odstraňte klíč](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Obnovení klíče](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Přihlaste se pomocí klávesy](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Ověření pomocí klíče](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Zalomit klíč](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Zrušit zalomení klíč](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Zašifrovat pomocí klávesy](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Dešifrování klíčem](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Aktualizace klíče](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Seznam klíče trezoru](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Seznam verze klíče](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Vytvoření tajná](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Získání tajná](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Aktualizace tajná](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Odstranění tajná](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Seznam tajemství v trezoru](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Seznam verze tajemství](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Další kroky ##

Kurz používající Azure klíč trezoru ve webové aplikaci najdete v článku [Použití Azure klíč trezoru z webové aplikace](key-vault-use-from-web-application.md).

Programování odkazy, najdete v článku [Příručka pro vývojáře Azure klíč trezoru](key-vault-developers-guide.md).

Seznam rutiny prostředí PowerShell 1.0 Azure pro trezoru klíč Azure najdete v tématu [Rutiny trezoru klíč Azure](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Kurz na klíčové otočení v prostoru a protokolu auditování s Azure klíč trezoru najdete v článku [jak nastavení trezoru klíč s konce střídání a auditování](key-vault-key-rotation-log-monitoring.md).
