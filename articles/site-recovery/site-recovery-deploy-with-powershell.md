<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech pomocí prostředí PowerShell a obnovení webu Azure | Microsoft Azure"
    description="Zjistěte, jak můžete automatizovat replikace Hyper-V virtuálních počítačích v VMM mračnech pomocí obnovení webu a Powershellu."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Replikovat Hyper-V virtuálních počítačích v VMM mračnech Azure pomocí prostředí Powershell – klasické

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-azure.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasický portálu](site-recovery-vmm-to-azure-classic.md)
- [Prostředí PowerShell – klasické](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Základní informace

Obnovení Azure webu přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích několika způsobech nasazení. Úplný seznam scénářů nasazení najdete v článku [obnovení webu Azure přehled](site-recovery-overview.md).

V tomto článku se dozvíte, jak pomocí Powershellu Automatizace běžných úkolů, které potřebujete provést, když nastavíte obnovení webu Azure replikovat Hyper-V virtuálních počítačích v System Center VMM mračnech k základnímu úložišti Azure.

Tento článek obsahuje požadavky pro scénář a uvidíte, jak nastavit trezoru obnovení webu, instalace zprostředkovatele obnovení webů Azure na zdrojový server VMM, registrace serveru v trezoru, přidejte účet Azure úložiště, instalace služby Azure Recovery agent na serverech hostitele Hyper-V, nastavení ochrany VMM shluky, které se použije na všechny chráněné virtuálních počítačích a pak ji povolit ochranu pro tyto virtuálních počítačích. Dokončete nastavení testováním převezme zkontrolujte, jestli že všechno funguje očekávaným způsobem.

Pokud narazíte na problémy, nastavte si tento scénář, pošlete svoje otázky na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce zdrojů a klasické](../resource-manager-deployment-model.md). Tento článek se věnuje pomocí klasického nasazení modelu.



## <a name="before-you-start"></a>Než začnete

Zkontrolujte, jestli že budete mít tyto požadavky na místě:

### <a name="azure-prerequisites"></a>Požadavky na Azure

- Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- Musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Účet musí geo replikace povolené. Ho by měl být ve stejné oblasti jako trezoru obnovení webu Azure a být přidružené stejném předplatném. [Další informace o Azure úložiště](../storage/storage-introduction.md).
- Musíte, abyste měli jistotu, že virtuálních počítačích, které chcete zamknout v souladu s [požadavky na Azure virtuálního počítače](site-recovery-best-practices.md#virtual-machines).

### <a name="vmm-prerequisites"></a>Požadavky na VMM
- Budete potřebovat VMM serveru na System Center 2012 R2.
- Musíte mít alespoň jeden cloudu na VMM serveru, který chcete zamknout. By měl obsahovat cloudu:
    - Nejméně jedna VMM skupiny hostitelů.
    - Jeden nebo několik Hyper-V hostitelské servery a clusterů v každé skupiny Host (hostitel).
    - Nejméně jedna virtuálních počítačích na zdrojovém Hyper-V serveru.

### <a name="hyper-v-prerequisites"></a>Zjistit předpoklady pro Hyper-V

- Hostitelských Hyper-V serverech musí běžet aspoň **Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a nainstalovali nejnovější aktualizace.
- Pokud používáte Hyper-V v poznámce clusteru této clusteru zprostředkovatele není automaticky vytvoří Pokud máte statickou IP adresu clusteru založený. Musíte ruční konfiguraci zprostředkovatele obrázku. Uděláte to takto: ve Správci serveru > správce, připojte se k němu, klikněte na **Konfigurovat rolí** a vyberte **Hyper-V otevřené zprostředkovatele** na obrazovce **Vyberte Role** Průvodce dostupnost.
- Hyper-V serveru nebo obrázku, u kterého chcete spravovat ochranu musí být součástí VMM cloudu.

### <a name="network-mapping-prerequisites"></a>Předpoklady mapování sítě
Když uložíte chránit virtuálních počítačích v mapách Azure sítě mapování mezi OM sítí na zdrojovém VMM serveru a obrázku Azure sítí povolit následující:

- Všechny počítače, které převzetí ve stejné síti můžete připojit k sobě, bez ohledu na to, jaký plán obnovení jsou.
- Pokud brána sítě je nastavena na cílovém Azure sítě, virtuálních počítačích můžete připojit k jiné místní virtuálních počítačích.
- Pokud není konfigurace sítě mapování pouze virtuálních počítačích, které selhání v plánu stejného obnovení bude moct připojit k sobě po přepnutí Azure.

Pokud chcete nasadit mapování sítě budete potřebovat takto:

- Virtuálních počítačích, které chcete zamknout na zdrojovém VMM serveru by měl být připojení k síti OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
- Azure sítě, ke kterému se můžete připojit replikovanou virtuálních počítačích po převzetí. V době převzetí vyberete této sítě. Ve stejné oblasti jako předplatné obnovení webu Azure by měl být v síti.
- [Další informace](site-recovery-network-mapping.md) o mapování sítě:

###<a name="powershell-prerequisites"></a>Zjistit předpoklady pro PowerShell
Zkontrolujte, jestli že máte Azure PowerShell připravená k odeslání. Pokud už používáte Powershellu, musíte k upgradu na verzi 0.8.10 nebo novější. Informace o nastavení Powershellu najdete v tématu [jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Po nastavení a konfigurace prostředí PowerShell, můžete zobrazit všechny dostupné rutiny pro službu [tady](https://msdn.microsoft.com/library/dn850420.aspx).

Další informace o tipů, které vám mohou pomoci, že pomocí rutin, například hodnoty parametrů, vstupů a výstupů obvykle zpracování v Azure Powershellu najdete v článku [Začínáme s Azure rutiny](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Krok 1: Nastavení předplatného

V prostředí PowerShell spusťte tyto rutiny:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Nahraďte prvky "< >" se specifickými informacemi.

## <a name="step-2-create-a-site-recovery-vault"></a>Krok 2: Vytvoření trezoru obnovení webu

V prostředí PowerShell nahraďte prvky "< >" se specifickými informacemi a spusťte tyto příkazy:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Krok 3: Generovat trezoru registračního klíče

Generování registračního klíče v trezoru. Po stažení poskytovatel obnovení webu Azure a nainstalujte ji na VMM server použijete tento klíč k registraci VMM serveru v trezoru.

1.  Ke stažení souboru trezoru nastavení a nastavení kontextu:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Nastavení kontextu trezoru spuštěním následující příkazy:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalace zprostředkovatele obnovení Azure webů

1.  V počítači VMM vytvořte adresář spuštěním tento příkaz:

    ```

    pushd C:\ASR\

    ```

2.  Extrahujte soubory stažené poskytovatele pomocí tohoto příkazu

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Instalace zprostředkovatele použitím následujících příkazů:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Počkejte na dokončení instalace.

4.  Registrace serveru v trezoru pomocí následujícího příkazu:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Vytvořte účet Azure úložiště

Pokud nemáte účet Azure úložiště, vytvořte účet povolené geo replikace spuštěním následujícího příkazu:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Všimněte si, že účet úložiště musí být ve stejné oblasti jako služba obnovení webu Azure a být přidružené stejném předplatném.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Instalace agenta služby Azure obnovení

Z portálu Microsoft Azure nainstalujte agenta služby Azure Recovery na každý server hostitele Hyper-V umístěn v mračnech VMM, které chcete zamknout.

Na všem hostitelé VMM spusťte tento příkaz:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurace cloudu nastavení ochrany

1.  Vytvoření profilu ochranu shluk Azure pomocí tohoto příkazu:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Získání kontejneru ochranu spuštěním následující příkazy:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Přidružení kontejneru ochranu začněte cloudu:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Po dokončení projektu, spusťte tento příkaz:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Po dokončení zpracování úlohy, spusťte tento příkaz:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Konfigurace mapování sítě
Než začnete mapování sítě ověřte, že virtuálních počítačích na zdrojovém VMM serveru jsou připojené k síti OM. Kromě toho vytvořte jednu nebo víc sítí Azure virtuální. Všimněte si, že víc sítí OM možné namapovat jedné sítě Azure.

Poznámka: Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě dnem nachází virtuálního počítače zdroje a pak otevřené virtuální počítač bude připojen k této cílové podsítě po překlopení. Pokud není cílové podsítě s odpovídajícím názvem, virtuální počítač připojen k první podsítě v síti.

Příkaz první získá servery pro aktuální obnovení webu Azure trezoru. Příkaz Uložit servery obnovení webu Microsoft Azure $Servers pole proměnné.

    $Servers = Get-AzureSiteRecoveryServer


Druhý příkaz získá sítě obnovení webů pro první server v poli $Servers. Příkaz Uložit sítí $Networks proměnné.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Příkaz třetí získá předplatné Azure pomocí rutiny Get-AzureSubscription a uložíte stejnou hodnotu do proměnné $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Čtvrtý příkaz získá Azure virtuální sítě pomocí rutiny Get-AzureVNetSite a pak tuto hodnotu do proměnné $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



Konečný rutina vytvoří mapování mezi primární síť a sítí Azure virtuálního počítače. Rutiny určuje primární síť jako první prvek $Networks. Rutiny určuje síti virtuálního počítače jako první prvek $AzureVmNetworks pomocí jeho ID. Příkaz obsahuje vaše ID Azure předplatného.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Povolte ochranu virtuálních počítačích

Po dokončení serverů, mračnech a sítí konfigurace správně, můžete povolit ochranu virtuálních počítačích v cloudu. Poznámka:

[Azure virtuálního počítače požadavky](site-recovery-best-practices.md#virtual-machines)musí splňovat virtuálních počítačích.

Povolení ochrany operačního systému a operační systém musí být vlastnosti disku nastavená pro virtuální počítač. Při vytváření virtuálních počítačů v VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost. Na kartě **Obecné** a **Konfigurace hardwaru** vlastnosti virtuálního počítače můžete nastavit taky těchto vlastností pro existující virtuálních počítačích. Pokud nenastavíte těchto vlastností v VMM si budete moct při konfiguraci v portálu obnovení webu Azure.



1.  Povolení ochrany, spusťte tento příkaz získat ochranu kontejner:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Získání entity ochranu (OM) spuštěním následujícího příkazu:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Povolení DR OM spuštěním následujícího příkazu:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo vytvořit plán obnovení tvořené několika virtuálních počítačích a spusťte test přepojení plánu. Převzetí test napodobuje si převzetí a obnovení mechanismus izolovaná síť. Všimněte si, že:

- Pokud chcete připojit k počítači virtuální v Azure pomocí vzdálené plochy po záložní, povolte připojení ke vzdálené ploše počítače virtuální před spuštěním testovací překlopení.
- Po selhání použijete veřejnou IP adresu pro připojení k virtuálního počítače v Azure pomocí vzdálené plochy. Pokud chcete, můžete to udělat, zajistěte, aby že se vám nezobrazuje žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.

Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).

### <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

1. Vytvoření souboru XML jako šablonu pro váš plán obnovení pomocí níže uvedených dat a pak ho uložte jako "C:\RPTemplatePath.xml".
2. Změna RecoveryPlan uzel Id, název, PrimaryServerId a SecondaryServerId.
3. Změna uzel ProtectionEntity PrimaryProtectionEntityId (vmid z VMM).
4. Můžete přidat další VMs přidáním více ProtectionEntity uzlů.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Vyplňte údaje v šabloně:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Vytvoření RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Spusťte test selhání

1.  Získání objektu RecoveryPlan spuštěním následujícího příkazu:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Spusťte test převzetí tohoto příkazu:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Sledování aktivity

Pomocí následujících příkazů ke sledování aktivity. Všimněte si, že budete muset počkat mezi úlohy pro zpracování na Dokončit.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Další kroky

[Přečtěte si další](https://msdn.microsoft.com/library/dn850420.aspx) informace o rutinách Powershellu obnovení webu Azure. </a>.
