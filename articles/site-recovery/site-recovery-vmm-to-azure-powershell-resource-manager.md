<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech pomocí prostředí PowerShell (Správce zdrojů) a obnovení webu Azure | Microsoft Azure"
    description="Replikovat Hyper-V virtuálních počítačích v VMM mračnech pomocí prostředí PowerShell a obnovení webu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Replikovat Hyper-V virtuálních počítačích v VMM mračnech Azure pomocí prostředí PowerShell a správce prostředků Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-azure.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasický portálu](site-recovery-vmm-to-azure-classic.md)
- [Prostředí PowerShell – klasické](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Základní informace

Obnovení Azure webu přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích několika způsobech nasazení. Úplný seznam scénářů nasazení najdete v článku [obnovení webu Azure přehled](site-recovery-overview.md).

V tomto článku se dozvíte, jak pomocí Powershellu Automatizace běžných úkolů, které potřebujete provést, když nastavíte obnovení webu Azure replikovat Hyper-V virtuálních počítačích v System Center VMM mračnech k základnímu úložišti Azure.

Tento článek obsahuje požadavky pro scénář a se dozvíte

- Jak nastavit trezoru služby obnovení
- Instalace zprostředkovatele obnovení webů Azure na zdrojovém VMM serveru
- Registrace serveru v trezoru, přidejte účet Azure úložiště
- Instalace služby Azure Recovery agent na serverech hostitele Hyper-V
- Konfigurace nastavení ochrany pro VMM Oblaka, které se použije na všechny chráněné virtuálních počítačích
- Povolte ochranu pro tyto virtuálních počítačích.
- Otestujte před selháním, aby zkontrolovala, jestli že všechno funguje očekávaným způsobem.

Pokud narazíte na problémy, nastavte si tento scénář, pošlete svoje otázky na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce zdrojů a klasické](../resource-manager-deployment-model.md). Tento článek se věnuje pomocí nasazení modelu správce prostředků.

## <a name="before-you-start"></a>Než začnete

Zkontrolujte, jestli že budete mít tyto požadavky na místě:

### <a name="azure-prerequisites"></a>Požadavky na Azure

- Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Pokud nemáte jeden, začněte s [bezplatný účet](https://azure.microsoft.com/free). Kromě toho si můžete přečíst o [obnovení správce webu Azure ceny](https://azure.microsoft.com/pricing/details/site-recovery/).
- Pokud zkoušíte replikace scénáře předplatné CSP budete potřebovat CSP předplatné. Další informace o programu CSP v [jak se zapsat v programu CSP](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Musíte mít účet Azure v2 úložiště (Správce zdrojů) pro ukládání dat replikovat na Azure. Účet musí geo replikace povolené. By měl být ve stejné oblasti jako služba obnovení webu Azure a být přidružené stejného předplatného nebo předplatného CSP. Další informace o nastavení Azure úložiště najdete v tématu [Úvod k úložišti tabulek Microsoft Azure](../storage/storage-introduction.md) kdykoliv při ruce.
- Musíte, abyste měli jistotu, že virtuálních počítačích, které chcete zamknout v souladu s [požadavky na Azure virtuálního počítače](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] V současné době pouze OM úrovně operace je možné prostřednictvím Powershellu. Podpora pro využití úrovně plán budou brzy bude k dispozici.  Nyní jsou omezené na provádění selhání overs pouze na granularity "chráněné OM" a ne na úroveň plánování obnovení.

### <a name="vmm-prerequisites"></a>Požadavky na VMM
- Budete potřebovat VMM serveru na System Center 2012 R2.
- Libovolný VMM server obsahující virtuálních počítačích, které chcete zamknout musí běžet poskytovatel obnovení webu Azure. To je nainstalovaný během obnovení webu Azure nasazení.
- Musíte mít alespoň jeden cloudu na VMM serveru, který chcete zamknout. By měl obsahovat cloudu:
    - Nejméně jedna VMM skupiny hostitelů.
    - Jeden nebo několik Hyper-V hostitelské servery a clusterů v každé skupiny Host (hostitel).
    - Nejméně jedna virtuálních počítačích na zdrojovém Hyper-V serveru.
- Další informace o nastavení VMM mračnech:
    - Přečtěte si další informace o soukromé VMM mračnech [Co je nového v cloudu soukromé s System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) a [VMM 2012 a shluky](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Zjistěte, jak [Konfigurovat struktury VMM cloudu](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Po elementů struktury cloudu na místě informace o vytvoření soukromého mračnech při [vytváření soukromé cloudu v VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) a v [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Zjistit předpoklady pro Hyper-V

- Hostitelských Hyper-V serverech musí běžet aspoň **Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a nainstalovali nejnovější aktualizace.
- Pokud používáte Hyper-V v poznámce clusteru této clusteru zprostředkovatele není automaticky vytvoří Pokud máte statické IP adresy clusteru založený. Musíte ruční konfiguraci zprostředkovatele obrázku. Pro
- Postupujte podle pokynů uvedených v tématu [Konfigurace Hyper-V otevřené zprostředkovatele](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Hyper-V serveru nebo obrázku, u kterého chcete spravovat ochranu musí být součástí VMM cloudu.

### <a name="network-mapping-prerequisites"></a>Předpoklady mapování sítě
Kdy ochrana virtuálních počítačích v Azure, mapy mapování sítí virtuální počítač sítí na zdrojovém VMM serveru a obrázku Azure sítí povolit takto:

- Všechny počítače, které převzetí ve stejné síti můžete připojit k sobě, bez ohledu na to, jaký plán obnovení jsou.
- Pokud brána sítě je nastavena na cílovém Azure sítě, virtuálních počítačích můžete připojit k jiné místní virtuálních počítačích.
- Pokud nemáte konfigurovat mapování sítě, budou moct připojit k sobě po před selháním na Azure pouze virtuálních počítačích, které selhání v plánu stejného obnovení.

Pokud chcete nasadit mapování sítě budete potřebovat takto:

- Virtuálních počítačích, které chcete zamknout na zdrojovém VMM serveru by měl být připojení k síti OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
- Azure sítě, ke kterému se můžete připojit replikovanou virtuálních počítačích po před selháním. V době před selháním vyberete této sítě. Ve stejné oblasti jako předplatné obnovení webu Azure by měl být v síti.

Další informace o mapování sítě

- [Postup při konfiguraci logických sítí v VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Postup při konfiguraci OM sítí a bran v VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Jak konfigurovat a sledovat virtuálních sítí v Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>Zjistit předpoklady pro PowerShell
Zkontrolujte, jestli že máte Azure PowerShell připravená k odeslání. Pokud už používáte Powershellu, musíte k upgradu na verzi 0.8.10 nebo novější. Informace o nastavení prostředí PowerShell najdete v tématu [Průvodce instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Po nastavení a konfigurace prostředí PowerShell, můžete zobrazit všechny dostupné rutiny pro službu [tady](https://msdn.microsoft.com/library/dn850420.aspx).

Další informace o tipů, které vám mohou pomoci, že pomocí rutin, například hodnoty parametrů, vstupů a výstupů obvykle zpracování v Azure Powershellu najdete v článku [Průvodce si nejdřív rutiny Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Krok 1: Nastavení předplatného

1. Z Azure powershellu, přihlaste se k účtu Azure: pomocí těchto rutin

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Dostaňte seznam předplatné. Tím se také vytvořit seznam subscriptionIDs pro každou z předplatných. Poznamenejte si subscriptionID předplatné, ve kterém chcete vytvořit trezoru služby obnovení

        Get-AzureRmSubscription

3. Nastavení ve kterém má být vytvořil zmínění ID předplatného trezoru služby obnovení předplatného

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2: Vytvoření trezoru obnovení služby

1. Pokud už nemáte vytvořit skupinu zdrojů v Azure správce

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Vytvoření nové služby Recovery trezoru a uložte vytvořený objekt trezoru automatickým obnovením systému do proměnné (se použijí později). Můžete také zadat pomocí rutiny Get-AzureRMRecoveryServicesVault vytvoření příspěvek objektu automatickým obnovením systému trezoru:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavení kontextu trezoru služby obnovení

1.  Nastavení kontextu trezoru spuštěním pod příkaz.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalace zprostředkovatele obnovení Azure webů

1.  V počítači VMM vytvořte adresář spuštěním tento příkaz:

        New-Item c:\ASR -type directory

2.  Extrahujte soubory stažené poskytovatele pomocí tohoto příkazu

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Instalace zprostředkovatele použitím následujících příkazů:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Počkejte na dokončení instalace.

4.  Registrace serveru v trezoru pomocí následujícího příkazu:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Vytvořte účet Azure úložiště

1. Pokud nemáte účet Azure úložiště, vytvořte účet povolené geo replikace ve stejném geo jako trezoru spuštěním následujícího příkazu:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Všimněte si, že účet úložiště musí být ve stejné oblasti jako služba obnovení webu Azure a být přidružené stejném předplatném.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Instalace agenta služby Azure obnovení

1. Agent služby Azure Recovery na [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) stáhnout a nainstalovat ho na všech serverech hostitele Hyper-V umístěn v mračnech VMM, které chcete zamknout.

2. Na všem hostitelé VMM spusťte tento příkaz:

    marsagentinstaller.exe/q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurace cloudu nastavení ochrany

1.  Vytvořte replikace zásadu Azure pomocí tohoto příkazu:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Získání kontejneru ochranu spuštěním následující příkazy:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Zobrazit podrobnosti zásady proměnné pomocí projektu, který byl vytvořený a zmínění název popisný zásady:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Přidružení kontejneru ochranu začněte zásady replikace:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Po dokončení projektu, spusťte tento příkaz:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Po dokončení zpracování úlohy, spusťte tento příkaz:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Konfigurace mapování sítě

Než začnete mapování sítě ověřte, že virtuálních počítačích na zdrojovém VMM serveru jsou připojené k síti OM. Kromě toho vytvořte jednu nebo víc sítí Azure virtuální.

Další informace o tom, jak vytvořit virtuální síť pomocí Správce prostředků Azure a Powershellu v [vytvořit virtuální sítě s připojení VPN k webu pomocí Správce prostředků Azure a prostředí PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Všimněte si, že víc sítí virtuálního počítače možné namapovat jedné sítě Azure. Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě nachází virtuálního počítače zdroje, bude možné virtuální počítač otevřené připojení k této cílové podsítě po před selháním. Pokud žádná cílové podsítě s odpovídajícím názvem virtuální počítač připojen k první podsítě v síti.

1. Příkaz první získá servery pro aktuální obnovení webu Azure trezoru. Příkaz Uložit servery obnovení webu Microsoft Azure $Servers pole proměnné.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Druhý příkaz získá sítě obnovení webů pro první server v poli $Servers. Příkaz Uložit sítí $Networks proměnné.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Třetí příkaz získá Azure virtuálních sítí a pak tuto hodnotu do proměnné $AzureVmNetworks.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Konečný rutina vytvoří mapování mezi primární síť a sítí Azure virtuálního počítače. Rutiny určuje primární síť jako první prvek $Networks. Rutiny určuje síti virtuálního počítače jako první prvek $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Povolte ochranu virtuálních počítačích

Po dokončení serverů, mračnech a sítí konfigurace správně, můžete povolit ochranu virtuálních počítačích v cloudu.

 Poznámka:

 - Azure požadavky musí splňovat virtuálních počítačích. Se změnami tyto [požadavky a podporu](site-recovery-best-practices.md) příručce k plánování.

 - Povolit ochranu, operačního systému a operační systém musí být vlastnosti disku nastavená pro virtuální počítač. Při vytváření virtuálních počítačů v VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost. Na kartě **Obecné** a **Konfigurace hardwaru** vlastnosti virtuálního počítače můžete nastavit taky těchto vlastností pro existující virtuálních počítačích. Pokud nenastavíte těchto vlastností v VMM si budete moct při konfiguraci v portálu obnovení webu Azure.


  1. Povolení ochrany, spusťte tento příkaz získat ochranu kontejner:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Získání entity ochranu (OM) spuštěním následujícího příkazu:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Povolení DR OM spuštěním následujícího příkazu:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Testování nasazení

Chcete-li otestovat nasazení můžete spusťte test před selháním pro jeden počítač virtuální nebo vytvořit plán obnovení tvořené několika virtuálních počítačích a spusťte test před selháním plánu. Test před selháním napodobuje si před selháním a obnovení mechanismus izolovaná síť. Všimněte si, že:

- Pokud chcete připojit k počítači virtuální v Azure pomocí vzdálené plochy po před selháním, povolte připojení ke vzdálené ploše počítače virtuální před spuštěním testovací překlopení.
- Po před selháním použijete veřejnou IP adresu pro připojení k virtuálního počítače v Azure pomocí vzdálené plochy. Pokud chcete, můžete to udělat, zajistěte, aby že se vám nezobrazuje žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.

Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).


### <a name="run-a-test-failover"></a>Spusťte test selhání

1.  Spusťte test převzetí tohoto příkazu:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Spuštění plánované selhání

1. Nejdřív plánované převzetí tohoto příkazu:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Spuštění neplánované překlopení

1. Zahájení neplánované převzetí spuštěním následujícího příkazu:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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

[Přečtěte si další](https://msdn.microsoft.com/library/azure/mt637930.aspx) informace o obnovení webu Azure pomocí rutin prostředí PowerShell Azure správce prostředků.
