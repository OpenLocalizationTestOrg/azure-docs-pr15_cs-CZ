<properties
    pageTitle="Replikace Hyper-V virtuálních počítačích v VMM mračnech na vedlejší VMM webu pomocí prostředí PowerShell (Správce zdrojů) | Microsoft Azure"
    description="Popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení Hyper-V VMs v VMM mračnech sekundární VMM web pomocí prostředí PowerShell (Správce zdrojů)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replikace Hyper-V virtuálních počítačích v VMM mračnech na vedlejší VMM webu pomocí prostředí PowerShell (Správce zdrojů)

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-vmm.md)
- [Klasický portálu](site-recovery-vmm-to-vmm-classic.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Vítá vás obnovení Azure webu! Použití tohoto článku, pokud chcete replikovat místní Hyper-V virtuálních počítačích spravovaný v mračnech System Center virtuálního počítače Manager (VMM) na vedlejší Web. 

V tomto článku se dozvíte, jak pomocí Powershellu Automatizace běžných úkolů, které potřebujete provést, když nastavíte obnovení webu Azure replikovat Hyper-V virtuálních počítačích v System Center VMM mračnech System Center VMM mračnech sekundární webu.

Tento článek obsahuje požadavky pro scénář a se dozvíte 

- Jak nastavit trezoru služby obnovení
- Instalace zprostředkovatele obnovení webů Azure na zdrojový server VMM a cílový VMM server
- Registrace VMM servery v trezoru
- Nastavení zásad replikace pro VMM cloudu. Nastavení replikace v zásady se použije na všechny chráněné virtuálních počítačích 
- Povolte ochranu virtuálních počítačích. 
- Otestujte převzetí VMs jednotlivě nebo jako součást obnovení plán, aby zkontrolovala, jestli že všechno funguje očekávaným způsobem.
- Provádějte plánované nebo neplánované převzetí VMs jednotlivě nebo jako součást obnovení plán, aby zkontrolovala, jestli že všechno funguje očekávaným způsobem.

Pokud narazíte na problémy, nastavte si tento scénář, pošlete svoje otázky na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení. Tento článek popisuje nasazení modelu správce prostředků.



## <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

Tady je, co budete muset na webech primárních a sekundárních místního nasazení situaci:

**Zjistit předpoklady pro** | **Podrobnosti** 
--- | ---
**VMM** | Doporučujeme že nasazení serveru VMM primární webu a serveru VMM sekundární webu.<br/><br/> Můžete také [mezi mračnech na jednom VMM serveru](site-recovery-single-vmm.md). Můžete to udělat musíte mít aspoň dva mračnech nakonfigurovaný na serveru VMM.<br/><br/> Servery VMM by měla běžet aspoň System Center 2012 SP1 s nejnovějšími aktualizacemi.<br/><br/> Každý VMM server musí mít na jednom nebo více mračnech nakonfigurované a všechny shluky musí mít profilu kapacity Hyper-V nastavení. <br/><br/>Mračnech musí obsahovat jednu nebo více skupin VMM Host (hostitel).<br/><br/>Další informace o nastavení VMM mračnech při [konfiguraci struktury cloudu VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Servery VMM měli přístup k Internetu. 
**Hyper-V** | Pro Hyper-V serverech musí běžet aspoň Windows Server 2012 s rolí Hyper-V a nainstalovali nejnovější aktualizace.<br/><br/> Server pro Hyper-V smí obsahovat jeden nebo více VMs.<br/><br/>  Hostitelské Hyper-V servery musí být umístěny v hostitele skupin v VMM mračnech primárních a sekundárních.<br/><br/> Pokud používáte Hyper-V clusteru v systému Windows Server 2012 R2 byste si měli nainstalovat [aktualizace 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud používáte Hyper-V clusteru v systému Windows Server 2012 poznámce této clusteru zprostředkovatele není automaticky vytvoří Pokud máte statické IP adresy clusteru založený. Musíte ruční konfiguraci zprostředkovatele obrázku. [Přečtěte si další](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Zprostředkovatel** | Během obnovení webu nasazení instalace zprostředkovatele obnovení webů Azure na serverech VMM. Zprostředkovatel informuje uživatele o s obnovení webu přes HTTPS 443 za účelem organizovat replikace. Data replikace mezi servery Hyper-V primárních a sekundárních přes síť LAN nebo připojení VPN.<br/><br/> Zprostředkovatel na serveru VMM potřebuje přístup k tyto adresy URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Kromě toho komunikovat bránu firewall od serverů VMM [Azure datacentra rozsahy IP adres](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolte protokol HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Předpoklady mapování sítě
Mapy sítí mapování mezi VMM OM sítí na serverech VMM primárních a sekundárních a:

- Optimální včasné otevřené VMs sekundární hosts Hyper-V po překlopení.
- Otevřené VMs připojte k odpovídající OM sítě.
- Pokud není konfigurace sítě mapování otevřené VMs nesmí být připojeni k jakákoli síť po překlopení.
- Pokud chcete nastavit síť mapování během obnovení webu nasazení tady je co budete potřebovat:

    - Ujistěte se, že jsou VMs na hostitelském serveru Hyper-V zdroj připojení k síti VMM OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
    - Ověřte, zda sekundární cloudu, který budete chtít použít pro obnovení síti odpovídající OM nakonfigurované. Síť OM by měl být propojené s logické sítě, který máte přidružený k sekundární cloudu.


Další informace o konfiguraci VMM sítě pod články

- [Postup při konfiguraci logických sítí v VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Postup při konfiguraci OM sítí a bran v VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Další informace](site-recovery-network-mapping.md) o tom, jak funguje mapování sítě.

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

1. Vytvoření skupiny zdroje správce prostředků Azure, pokud už nemáte

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Vytvoření nové služby Recovery trezoru a uložte vytvořený objekt trezoru automatickým obnovením systému do proměnné (se použijí později). Můžete také zadat pomocí rutiny Get-AzureRMRecoveryServicesVault vytvoření příspěvek objektu automatickým obnovením systému trezoru:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavení kontextu trezoru služby obnovení

1.  Pokud jste už vytvořili trezoru spustit pod příkaz přejděte trezoru.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Nastavení kontextu trezoru spuštěním pod příkaz.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Krok 5: Vytvoření a přiřazení zásady replikace

1.  Vytvoření zásad replikace Hyper-V 2012 R2 spuštěním následujícího příkazu:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] VMM cloud může obsahovat Hyper-V hosts s různými verzemi systému Windows Server (jak je uvedeno v požadavcích pro Hyper-V), ale zásady replikace je operační systém verze určitých. Pokud máte jiný hosts spuštěných pro různé verze operačního systému, vytvořte samostatné replikace zásad pro každý typ operační systém verze. Pro např: Pokud máte pět hosts se systémem Windows servery 2012 a tři v systému Windows Server 2012 R2, vytvořit dva zásady replikace – jeden pro každý typ verze operačního systému.

2.  Získání primární ochranu kontejner (primární VMM Cloud) a obnovení ochrany kontejner (obnovení VMM cloudu) spuštěním následující příkazy:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Načtení zásadu, kterou jste vytvořili v kroku 1 pomocí popisný název zásady

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Přidružení kontejneru ochranu (VMM cloudu) můžete začněte s zásady replikace:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Počkejte na dokončení úlohy přidružení zásad. Můžete zkontrolovat, že pokud dokončení úlohy pomocí následující úryvek Powershellu.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Po dokončení zpracování úlohy, spusťte tento příkaz:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurace mapování sítě

1. Příkaz první získá servery pro aktuální obnovení webu Azure trezoru. Příkaz Uložit servery obnovení webu Microsoft Azure $Servers pole proměnné.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Pod příkazy získat sítě obnovení webů pro zdrojový server VMM a cílový VMM server.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Zdrojový server VMM může být první z nich nebo druhá matice servery. Zaškrtněte políčko názvy serverů VMM a řádně podporovat získat sítí


4. Konečný rutina vytvoří mapování mezi primární síť a obnovení sítě. Rutiny určuje primární síť jako první prvek $PrimaryNetworks a obnovení síťové jako první prvek $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Krok 6: Konfigurace mapování úložiště

1. Pod příkazem získá seznam úložiště klasifikace do $storageclassifications proměnné.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Pod příkazy získat zdroj zařazení do $SourceClassificaion proměnné a cílové zařazení do $TargetClassification proměnné. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Klasifikace zdrojové a cílové může být všechny prvky v poli. Podívejte se do výstupu pod příkaz k určení prodejce index zdrojovou a cílovou klasifikace $storageclassifications matice. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Vyberte objekt – vlastnosti FriendlyName, Id | Formátovat tabulku


3. Pod rutina vytvoří mapování mezi klasifikace zdrojové a cílové klasifikace. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolte ochranu virtuálních počítačích

Po dokončení serverů, mračnech a sítí konfigurace správně, můžete povolit ochranu virtuálních počítačích v cloudu. 


  1. Povolení ochrany, spusťte tento příkaz získat ochranu kontejner:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Získání entity ochranu (OM) spuštěním následujícího příkazu:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Povolte pro OM spuštěním následujícího příkazu:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo vytvořit plán obnovení tvořené několika virtuálních počítačích a spusťte test přepojení plánu. Převzetí test napodobuje si převzetí a obnovení mechanismus izolovaná síť. 

> [AZURE.NOTE] Můžete vytvořit plán pro obnovení aplikace Azure portálu.

Zkontrolovat dokončení operace, postupujte podle pokynů v [Monitoru aktivity](#monitor).


### <a name="run-a-test-failover"></a>Spusťte test selhání

1.  Spustit pod rutiny get OM sítě, ke kterému chcete testovat převzetí VMs k.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Proveďte test selhání virtuálního počítače následujícím způsobem:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Provedení selhání testovací plán obnovy následujícím způsobem:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Spuštění plánované selhání

1. Provedení plánované selhání virtuálního počítače následujícím způsobem:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Provedení plánované selhání plán obnovy následujícím způsobem:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Spuštění neplánované překlopení

1. Provedení neplánované převzetí virtuálního počítače následujícím způsobem:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. proveďte neplánované převzetí plán obnovy následujícím způsobem:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

