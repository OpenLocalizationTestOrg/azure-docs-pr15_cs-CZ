<properties 
    pageTitle="Spuštění a ukončení virtuálních počítačích s Azure automatizaci - pracovního postupu prostředí PowerShell | Microsoft Azure"
    description="Grafické verzi Azure automatizaci scénář včetně runbooks spuštění a ukončení klasické virtuálních počítačích."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure automatizaci scénář – spuštění a ukončení virtuálních počítačích

Tento scénář Azure automatizaci obsahuje runbooks spuštění a ukončení klasické virtuálních počítačích.  Použijte tento scénář z některého z následujících akcí:  

- Použití runbooks beze změny ve vlastních prostředí. 
- Úprava runbooks provést vlastní funkce.  
- Zavolejte runbooks z jiného postupu runbook jako součást celkovém řešení. 
- Použijte runbooks jako výukové programy pro další postupu runbook vytváření koncepty. 

> [AZURE.SELECTOR]
- [Grafické](automation-solution-startstopvm-graphical.md)
- [Pracovní postup prostředí PowerShell](automation-solution-startstopvm-psworkflow.md)

Toto je verze postupu runbook pracovního postupu prostředí PowerShell tento scénář. Je také k dispozici pomocí [grafické runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Získání scénáře

Tento scénář se skládá ze dvou runbooks pracovního postupu Powershellu, které si můžete stáhnout z následujících odkazů.  Zobrazte [grafický verze](automation-solution-startstopvm-graphical.md) tento scénář odkazy na grafické runbooks.

| Postupu Runbook | Odkaz | Typ | Popis |
|:---|:---|:---|:---|
| Zahájení AzureVMs | [Zahájení Azure klasické VMs](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | Pracovní postup prostředí PowerShell | Spustí všechny klasické virtuálních počítačích Azure subscriptionor všechny virtuálních počítačích s názvem konkrétní službu. |
| Zastavit AzureVMs | [Ukončení Azure klasické VMs](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | Pracovní postup prostředí PowerShell | Ukončí všechny virtuálních počítačích v účtu automatizaci nebo všechny virtuálních počítačích s názvem konkrétní službu.  |


## <a name="installing-and-configuring-the-scenario"></a>Instalace a konfigurace scénáře

### <a name="1-install-the-runbooks"></a>1. runbooks instalace

Po stažení runbooks, můžete je importovat pomocí postupu import [postupu Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Seznamte se s popis a požadavky
Runbooks zahrnout text skrytými v komentářích nápovědy, která obsahuje popis a požadované prostředky.  Můžete taky dostanete stejné informace v tomto článku. 

### <a name="3-configure-assets"></a>3. konfigurace prostředky
Runbooks vyžadují následující prostředky, které musí vytvořit a naplnění s příslušnými hodnotami.

| Typ majetku | Název majetku | Popis |
|:---|:---|:---|:---|
| Přihlašovací údaje | AzureCredential | Obsahuje přihlašovacích údajů pro účet, který má oprávnění spustit a zastavit virtuálních počítačích v Azure předplatného.  Můžete taky můžete určit jiné materiálů přihlašovacích údajů v parametru **pověření** aktivity **Přidat AzureAccount** . |
| Proměnná | AzureSubscriptionId | ID předplatného předplatného Azure obsahuje. |

## <a name="using-the-scenario"></a>Použití scénáře

### <a name="parameters"></a>Parametry

Runbooks každý mít následujících parametrů.  Nutné zadat hodnoty všech parametrů, povinná a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na vašim požadavkům.

| Parametr | Typ | Povinné | Popis |
|:---|:---|:---|:---|
| Název_služby | řetězec | Ne | Pokud hodnota je k dispozici, a pak začít nebo ukončit všechny virtuálních počítačích s tímto názvem služby.  Pokud je zadána žádná hodnota, jsou všechny klasické virtuálních počítačích v Azure předplatného začít nebo ukončit. |
| AzureSubscriptionIdAssetName | řetězec | Ne | Obsahuje název [proměnné materiálů](#installing-and-configuring-the-scenario) , který obsahuje ID předplatného předplatného Azure.  Pokud nezadáte hodnoty, použije se *AzureSubscriptionId* .  |
| AzureCredentialAssetName | řetězec | Ne | Obsahuje název [pověření materiálů](#installing-and-configuring-the-scenario) , která obsahuje přihlašovací údaje pro postupu runbook používat.  Pokud nezadáte hodnoty, použije se *AzureCredential* .  |

### <a name="starting-the-runbooks"></a>Spuštění runbooks

Můžete metod při [spouštění postupu runbook v Azure automatizaci](automation-starting-a-runbook.md) buď runbooks v tomto scénáři začít.

Následující ukázkové příkazy pomocí prostředí Windows PowerShell spusťte **StartAzureVMs** spuštění všech virtuálních počítačích s názvem služby *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Výstup

Runbooks bude [výstupní zprávy](automation-runbook-output-and-messages.md) pro každý virtuální počítač označující též úspěšně odeslána pokyny k zahájení nebo ukončení.  Můžete vyhledávat určitý řetězec do výstupu rozhodnout, bude výsledek pro každou postupu runbook.  V následující tabulce jsou uvedeny řetězců možné výstupu.

| Postupu Runbook | Podmínky | Zpráva |
|:---|:---|:---|
| Zahájení AzureVMs | Virtuální počítač se systémem  | MyVM už je spuštěný. |
| Zahájení AzureVMs | Zahájení žádosti o úspěšném odeslání virtuálního počítače | Spustila MyVM |
| Zahájení AzureVMs | Žádost o spuštění virtuálního počítače se nezdařila.  | MyVM se nepodařilo spustit |
| Zastavit AzureVMs | Zastaven virtuálního počítače  | Zastaven MyVM |
| Zastavit AzureVMs | Ukončení žádost o úspěšném odeslání virtuálního počítače | Přestal MyVM |
| Zastavit AzureVMs | Zastavit žádost o virtuálního počítače se nezdařila.  | MyVM se nepodařilo zastavit |

Například následující fragment kódu z postupu runbook pokusu o spuštění všech virtuálních počítačích s názvem služby *MyServiceName*.  Pokud některý z žádosti o selhání start, můžete chyby akce považuje. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Podrobný rozpis

Následuje podrobný rozpis runbooks v tomto scénáři.  Tyto informace slouží k přizpůsobení runbooks nebo chci jen další z nich pro vytváření vlastních automatizaci scénáře.

### <a name="parameters"></a>Parametry

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Pracovní postup je spuštěn získáním hodnoty pro [vstupní parametry](#using-the-scenario).  Pokud nejsou k dispozici názvy datových zdrojů je použito výchozí názvy.

### <a name="output"></a>Výstup

    # Returns strings with status messages
    [OutputType([String])]

Tento řádek deklaruje, po které bude výstupní postupu runbook řetězec.  Není povinná, ale je doporučený postup při postupu runbook jako [postupu runbook podřízené](automation-child-runbooks.md) aby věděli, postupu runbook nadřazený typ výstupu očekávat.

### <a name="authentication"></a>Ověřování

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Další řádky nastavit [přihlašovací údaje](automation-configuring.md#configuring-authentication-to-azure-resources) a Azure předplatné, které se použije pro zbytek postupu runbook.
Nejdřív jsme pomocí **Get-AutomationPSCredential** materiálů, která obsahuje údaje s přístupem k spustit a zastavit virtuálních počítačích v Azure předplatného. **Přidat AzureAccount** pomocí tohoto majetku nastavit přihlašovací údaje.  Výstup přiřazen proměnnou formální tak, aby ho výstupu není zahrnut postupu runbook.  

Proměnná majetku s ID předplatného se načítá s **Get-AutomationVariable** a nastavit s **AzureSubscription vyberte**předplatné.

### <a name="get-vms"></a>Získání VMs

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** slouží k načtení virtuálních počítačích, které postupu runbook fungovat s.  Pokud hodnotu do proměnné vstupní **Název_služby** virtuálních počítačích s tímto názvem služby načtením.  Pokud **Název_služby** je prázdné, jsou načtená všechny virtuálních počítačích.

### <a name="startstop-virtual-machines-and-send-output"></a>Spuštění nebo zastavení virtuálních počítačích a odeslat výstup

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Řádky přijde prostřednictvím virtuální počítače.  Nejdřív **PowerState** virtuálního počítače je zaškrtnuté políčko vidět – Pokud už je spuštěný nebo přerušili podle postupu runbook.  Pokud už je spuštěný do cílového stavu, zprávy odeslaný do výstupu a končí postupu runbook.  V opačném případě použije **AzureVM spustit** nebo **Zastavit AzureVM** pokoušet zahájení nebo ukončení virtuálního počítače s výsledkem žádost uložené proměnné.  Zprávy se přemístí výstup určující, zda byla úspěšně odeslána žádost spustit nebo zastavit.


## <a name="next-steps"></a>Další kroky

- Další informace o práci s podřízené runbooks najdete v tématu [podřízené runbooks v Azure automatizaci](automation-child-runbooks.md) 
- Další informace o výstupní zprávy při spuštění postupu runbook a protokolování můžou pomoct odstranit, najdete v článku [postupu Runbook výstup a zprávy v Azure automatizaci](automation-runbook-output-and-messages.md)
