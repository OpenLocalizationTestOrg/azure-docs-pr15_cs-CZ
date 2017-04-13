<properties
    pageTitle="Replikace VMware virtuálních počítačích na Azure pomocí obnovení webu Azure automatizaci DSC | Microsoft Azure"
    description="Popisuje, jak používat Azure automatizaci DSC automaticky nasadit služby Azure webu obnovení nastavení mobilních zařízení a Azure agent pro virtuální/fyzické stroje Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Replikace VMware virtuálních počítačích na Azure pomocí DSC Azure automatické obnovení webu

Správa sady operace vám poskytneme komplexní zálohování a havárie obnovení řešení, které můžete použít v rámci vašeho plánu kontinuitu business.

Jsme zahájené tento cesty společně s Hyper-V použití Hyper-V otevřené. Ale jsme rozbalená podporuje nesourodými nastavení protože zákazníci více hypervisory a platformách v jejich mračnech.

Pokud používáte VMware pracovního vytížení a/nebo pole fyzicky servery dnes, na serveru správy všechny prvky obnovení webu Azure běží ve vašem prostředí dělat s replikace komunikaci a dat s Azure, když Azure je vaše cíle.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Nasazení službu mobilita využití webu pomocí automatizaci DSC
Začněme provedením rychlý přehled jaký dopad tento server pro správu.

Server pro správu spustí několik role serveru. Jednu z těchto rolích je *Konfigurace*, která souřadnic komunikace a spravuje dat replikace a obnovení procesů.

Kromě toho roli *proces* se chová jako brána replikace. Tato role přijímá replikace data z počítače chráněného zdroj, optimalizuje ukládání do mezipaměti, komprese a šifrování a potom ji pošle účet Azure úložiště. Jedna z funkcí pro roli proces je také nabízená instalace služby mobilita na zamknutém počítače a provést automatické zjišťování VMware VMs.

Pokud existuje navrácení z Azure, *předlohy cílovou* roli replikace data jako součást operace zpracuje.

Pro chráněné stroje jsme spolehnout *mobilita služby*. Tato součást nasazených na všechny počítače (VMware OM nebo pole fyzicky server), které chcete replikovat Azure. Zaznamenává zápis dat v počítači a předá ho server pro správu (role obrázku).

Když pracujete s nepřerušený, je důležité pochopit svého pracovního vytížení, infrastrukturu a souvisejících součásti. Můžete pak splňovat kritéria pro cíle časového využití (RTO) a obnovení čárky cíle (operace RPO). V tomto kontextu službu mobilita je klíčová k zajištění chránit vaše pracovního vytížení tak, jak byste chtěli.

A jak můžete, optimalizované způsobem zajištění spolehlivé nastavení chráněné pomocí některé součásti operace správy sadu?

Tento článek obsahuje příklady použití Azure automatické vyplňování stavu konfigurace (DSC), spolu s obnovení webu zajistit, aby:

- Služba nastavení mobilních zařízení a Azure OM agent jsou nasazeny Windows stroje, které chcete zamknout.
- Nastavení mobilních zařízení běží a služba Azure OM agent vždy po Azure cíl replikace.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Úložiště pro ukládání vyžadovaného nastavení
- Úložiště pro ukládání požadované heslo k registraci server pro správu

 > [AZURE.NOTE] Vygeneruje se jedinečný heslo pro každý server pro správu. Pokud přecházíte nasazení více servery správy, je nutné zajistit, že je správné heslo uložené v souboru passphrase.txt.

- Management Framework WMF (Windows) 5.0 nainstalovaný v počítačích, které chcete povolit ochranu (pro automatizaci DSC povinné)

 > [AZURE.NOTE] Pokud chcete použít strojů DSC pro systém Windows, které mají WMF 4.0 nainstalovaný, najdete v části [Použití DSC v odpojeno prostředí](#Use DSC in disconnected environments).

Nastavení mobilních zařízení služba je možné nainstalovat prostřednictvím příkazovém řádku a několika argumenty. Proto budete muset mít binární soubory (po extrahování je z nastavení) a byly ukládány v místě, kde je můžete obnovit pomocí nástroje Konfigurace DSC.

## <a name="step-1-extract-binaries"></a>Krok 1: Extrahovat binární

1. Extrahujte soubory, které potřebujete pro toto nastavení, přejděte do složky na serveru správy:

    **Recovery\home\svsystems\pushinstallsvc\repository \Microsoft azure webu**

    V této složce měli byste vidět souboru MSI s názvem:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Pomocí následujícího příkazu extrahovat instalační program:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Vyberte všechny soubory a poslat mu tuhle zkomprimovanou složku (ZIP).

Nyní máte binární soubory, které potřebujete k automatizaci instalace služby mobilita pomocí DSC automatizaci.

### <a name="passphrase"></a>Heslo

Pak budete muset určete, kde má být tento ZIP složku umístit. Účet Azure úložiště, můžete použít, jak je znázorněno na pozdější uložit heslo, které potřebujete pro nastavení. Agent bude zaregistrujte se serverem správy jako součást procesu.

Heslo, které jste získali při nasazení serveru správy můžete uložit do textového souboru jako passphrase.txt.

Umístěte v kontejneru vyhrazené v okně účet Azure úložiště složky ZIP a heslo.

![Umístění složky](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Pokud chcete zachovat tyto soubory ve sdílené složce ve vaší síti, můžete to udělat. Potřebujete zajistit, aby DSC prostředek, který použijete později má přístup a dostali instalační program a heslo.

## <a name="step-2-create-the-dsc-configuration"></a>Krok 2: Vytvoření konfigurace DSC

Nastavení závisí na WMF 5.0. Na počítači úspěšně použít konfiguraci prostřednictvím automatizaci DSC WMF 5.0 potřebujete prezentovat.

Prostředí používá následující příklad DSC konfigurace:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Konfigurace bude takto:

- Proměnné se dozvíte konfiguraci získání binární soubory pro službu nastavení mobilních zařízení a agenta OM Azure získání heslo a místo pro ukládání výstupu.
- Konfigurace naimportuje zdroje xPSDesiredStateConfiguration DSC tak, aby bylo možné použít `xRemoteFile` ke stažení souborů v úložišti.
- Konfigurace vytvoří v adresáři, ve které chcete uložit binární.
- Zdroje archivu extrahujte soubory ze složky ZIP.
- Službu mobilita nainstaluje balíček zdroj instalace z UNIFIEDAGENT. Instalační program EXE s konkrétních argumentů. (Proměnné, které vytvářet argumenty muset změnit tak, aby odrážela prostředí.)
- Agent OM Azure, což se doporučuje na každé OM, které se spouští v Azure nainstaluje balíček AzureAgent zdroje. Agent Azure OM také umožňuje přidat rozšíření bude v angličtině po překlopení.
- Služba zdrojů nebo zdrojů zajistí vždy spuštěna související mobilita služby a služby Azure.

Konfigurace uložte jako **ASRMobilityService**.

>[AZURE.NOTE] Upozorňujeme, že chcete nahradit CSIP v konfiguraci tak, aby odrážely skutečné management server tak, aby agent správně spojeny a použijete správné heslo.

## <a name="step-3-upload-to-automation-dsc"></a>Krok 3: Odeslání automatizace DSC

Protože DSC konfigurace, které jste udělali naimportuje vyžaduje modul zdroje DSC (xPSDesiredStateConfiguration), budete muset před nahrajete konfigurace DSC importovat tento modul v automatizaci.

Přihlaste se ke svému účtu automatizaci, přejděte do **prostředky** > **modulů kontroly**a klikněte na **Procházet Galerie**.

Tady můžete hledat modulu a importujte ho ke svému účtu.

![Import modul](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Až budete hotoví, přejděte na vašem počítači kterém máte moduly správce prostředků Azure nainstalovaný a přejděte na import konfigurace nově vytvořený DSC.

### <a name="import-cmdlets"></a>Rutiny pro import

V prostředí PowerShell Přihlaste se k předplatnému Azure. Změna rutiny, aby odrážela prostředí a zachycení informací o účtu automatizaci do proměnné:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Nahrání konfigurace k automatizaci DSC pomocí následující rutinu:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Konfigurace ve DSC automatické shrnutí

Pak budete muset kompilaci konfigurace ve automatizaci DSC tak, že můžete začít registrovat uzly k němu. Dosažení, spusťte následující rutinu:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Protože v podstatě nasazujete konfigurace pro službu hostované vyžádané DSC to může trvat několik minut.

Po kompilaci konfigurace můžete načítat informace úlohy pomocí prostředí PowerShell (Get-AzureRmAutomationDscCompilationJob) nebo pomocí [Azure portálu](https://portal.azure.com/).

![Načtení projektu](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Máte teď úspěšně publikovat a nahráli konfiguraci DSC DSC automatizaci.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Krok 4: Integrovaný počítače pro automatizaci DSC
>[AZURE.NOTE] Jednou z požadavcích na dokončení tento scénář je, že počítačích Windows se aktualizují na nejnovější verzi WMF. Můžete stáhnout a nainstalovat správné verze pro svoji platformu z [Webu služby Stažení softwaru](https://www.microsoft.com/download/details.aspx?id=50395).

Teď vytvoříte metaconfig pro DSC, které bude použito u uzly. K úspěšnému s tím, musíte získat adresu URL koncového bodu a primární klíč účtu automatizaci vybrané v Azure. Můžete najít tyto hodnoty **klíčů** na zásuvné **všechna nastavení** pro automatizaci účet.

![Hodnoty klíče](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

V tomto příkladu máte fyzické serveru Windows Server 2012 R2, který chcete chránit pomocí obnovení webu.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Vyhledat všechny čekající operaci přejmenování souboru v registru

Než začnete přidružit koncový bod automatizaci DSC serveru, doporučujeme, abyste ověřili všechny čekající operaci přejmenování souboru v registru. Dokončení vzhledem k restartování počítače se může zakázat nastavení.

Spusťte následující rutinu ověřte, že je bez restartování počítače na serveru:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Pokud se zobrazí prázdné, jsou na tlačítko OK. Pokud ne, je třeba zabývat tak, že restartování serveru během údržby.

Chcete-li použít konfiguraci na serveru, spusťte prostředí integrovaném skriptování prostředí PowerShell (ISE) a spuštěním následujícího skriptu. To je v podstatě DSC místní konfigurace, který nasměruje modul Správce konfigurace místní k registraci ke službě automatizaci DSC a načtení konkrétní konfigurace (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Konfigurace způsobí modul Správce konfigurace místní k registraci s DSC automatizaci. Také bude určovat, jak by se měly používat modul, co to udělat, když je konfigurace posun (ApplyAndAutoCorrect) a jak by měla postupovat s konfigurací Pokud je potřeba restartovat počítač.

Po spuštění tento skript by měly začít uzel k registraci DSC automatizaci.

![Registrace uzel probíhá](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Pokud se vrátíte k portálu Azure, uvidíte, že nově registrovaných uzel má nyní se nezobrazují na portálu.

![Registrovaná uzel na portálu](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Na serveru můžete spustit následující rutinu Powershellu pro ověření, že na uzel registrované správně:

```powershell
Get-DscLocalConfigurationManager
```

Po konfiguraci byl doplněné a použití na serveru, ověřit to můžete spusťte následující rutinu:

```powershell
Get-DscConfigurationStatus
```

Výstup ukazuje, že server úspěšně doplněné konfigurace:

![Výstup](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Nastavení služby mobilita navíc vlastní protokol, který najdete na *systémová_jednotka*\ProgramData\ASRSetupLogs.

To je vše. Máte teď úspěšně nasazeném a registrován služby nastavení mobilních zařízení v počítači, který chcete chránit pomocí obnovení webu. DSC zajistí, že jsou vždy spuštěny požadované služby.

![Úspěšné nasazení](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Po server pro správu zjistí úspěšné nasazení, můžete konfigurovat ochranu a povolení replikace v počítači pomocí obnovení webu.

## <a name="use-dsc-in-disconnected-environments"></a>Používání DSC v odpojeno prostředích

Pokud počítače nejste připojení k Internetu, můžete pořád spolehnout na DSC nasazení a konfiguraci služby mobilita úloh, které chcete zamknout.

Můžete vytvořit vlastní DSC vyžádané serveru instanci ve vašem prostředí v podstatě poskytnout stejné možnosti, které dostanete z DSC automatizaci. To znamená klienty získávat konfigurace (poté, co je registrovaná) na koncový bod DSC. Další možností je ručně nabízená konfigurace DSC do počítače, místní nebo vzdálené.

Všimněte si, že v tomto příkladu je přidaný parametr pro název počítače. Vzdálené soubory, které jsou teď umístěny vzdálené sdílené složky, která má být přístupné stroje, které chcete zamknout. Na konec skript představuje konfiguraci a potom začne použít konfigurace DSC do cílového počítače.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Ujistěte se, že je nainstalovaný modul xPSDesiredStateConfiguration Powershellu. U nainstalovanou WMF 5.0 počítačů s Windows můžete nainstalovat modul xPSDesiredStateConfiguration spusťte následující rutinu na cílových počítačů:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Taky můžete stáhnout a uložte modul v případě, že budete muset distribuce Windows počítačů, které mají WMF 4.0. Spusťte tuto rutinu na počítač kde PowerShellGet (WMF 5.0) neobsahuje data:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Také WMF 4.0 zajistit nainstalované [Windows 8.1 aktualizovat KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) počítačích.

Následující konfigurace můžete posune Windows počítačů, které mají WMF 4.0 a WMF 5.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Pokud chcete vytvořit vlastní DSC vyžádané serveru ve vaší podnikové síti napodobit funkcí, které se dá dostat z DSC automatizaci, najdete v článku [Nastavení serveru DSC webových vložit](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Volitelné: Nasazení konfigurace DSC pomocí šablony pro správce prostředků Azure

Tento článek se zaměřuje na jak je možné vytvořit vlastní DSC konfiguraci automaticky nasadit služby mobilita a Agent OM Azure – a zajistit, které jsou spuštěné v počítačích, které chcete zamknout. Máme také nasazení této konfiguraci DSC účet Azure automatizaci nové nebo existující šablony aplikace Správce prostředků Azure. Šablona bude použita vstupních parametrů pro vytvoření automatizaci materiálů, které budou obsahovat proměnné ve vašem prostředí.

Po nasazení šablony můžete jednoduše odkázat na krok 4 v této příručce integrovaný počítače.

Šablona bude takto:

1. Použití existujícího účtu automatizaci nebo vytvořte nový účet
2. Vytváření vstupních parametrů pro:
    - ASRRemoteFile – umístění pro uložení nastavení služby mobilita
    - ASRPassphrase – umístění pro uložení souboru passphrase.txt
    - ASRCSEndpoint – IP adresu serveru správy
3. Import modulu xPSDesiredStateConfiguration PowerShell
4. Vytvoření a sestavíte DSC konfigurace

Předchozích kroků dojde ve správném pořadí, aby mohli začít rychlého připojení počítače pro ochranu.

Šablona s pokyny pro nasazení se nachází na [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Nasazení šablony pomocí prostředí PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Další kroky

Po nasazení služby agentů mobilitu, můžete [Povolit replikace](site-recovery-vmware-to-azure.md#step-6-replicate-applications) pro virtuálních počítačích.
