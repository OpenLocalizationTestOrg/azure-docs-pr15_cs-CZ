<properties
    pageTitle="Ochrana serverů Azure pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure | Microsoft Azure"
    description="Automatizace ochranu serverů Azure s obnovení webu Azure pomocí prostředí PowerShell a správce prostředků Azure."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Pomocí prostředí PowerShell a správce prostředků Azure replikovat mezi místním Hyper-V virtuálních počítačích a Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-hyper-v-site-to-azure.md)
- [Prostředí PowerShell – správce](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klasický portálu](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Základní informace

Obnovení Azure webu přispívá k strategie firmy kontinuitu a havárie obnovení tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích několika způsobech nasazení. Úplný seznam scénáře nasazení najdete v tématu [obnovení webu Azure přehled](site-recovery-overview.md).

Azure Powershellu je moduly, které poskytuje rutiny pro správu Azure přes Windows PowerShell. Můžete pracovat s dva typy modulů: modul Azure profilu nebo modul Azure správce prostředků.

Rutiny prostředí PowerShell obnovení webů s Azure Powershellu pro správce prostředků Azure vám pomůže chránit a obnovit serverech v Azure.

Tento článek popisuje, jak používat Windows PowerShell, spolu s Azure správce prostředků, nasazení obnovení webu ke konfiguraci a organizovat ochrany server Azure. Příklad použitých v tomto článku se dozvíte, jak chránit převzít a obnovit virtuálních počítačích na Hyper-V hostiteli na Azure, pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure.

> [AZURE.NOTE] Rutiny prostředí PowerShell obnovení webů aktuálně umožňují konfigurovat následující položky: webů správce virtuálního počítače do jiného, virtuálního počítače správce webu na Azure a web pro Hyper-V Azure.

Nemusí být expert prostředí PowerShell můžete v tomto článku, ale je potřeba porozumět základní koncepty, například modulů kontroly, rutin a relace. Další informace o prostředí Windows PowerShell najdete v tématu [Začínáme používat Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Můžete taky Další informace o [Pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Partnery společnosti Microsoft, které jsou součástí aplikace cloudové řešení poskytovatele (CSP) můžete konfigurovat a spravovat ochranu svým zákazníkům serverů svým zákazníkům odpovídajících CSP předplatná (klienta předplatná).

## <a name="before-you-start"></a>Než začnete

Zkontrolujte, jestli že budete mít tyto požadavky na místě:

- Účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). Kromě toho si můžete přečíst o [obnovení správce webu Azure ceny](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Informace o této verzi a jak ji nainstalovat, najdete v článku [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly. Nejnovější verze moduly se dá dostat z [Galerie prostředí PowerShell](https://www.powershellgallery.com/)

Tento článek ukazuje, jak pomocí Powershellu Azure pomocí Správce prostředků Azure ke konfiguraci a správě ochrany serverech. Příklad použitých v tomto článku se dozvíte, jak chránit virtuálního počítače spuštěných pro Hyper-V hostitele a Azure. Následující požadavky se vztahují k v tomto příkladu. Sady komplexnější požadavků na různých scénářích obnovení webu naleznete v dokumentaci týkající se tento scénář.

- Hyper-V host spuštěný Windows Server 2012 R2 nebo Microsoft Hyper-V serveru 2012 R2 obsahující jeden nebo více virtuálních počítačích.
- Servery Hyper-V připojení k Internetu, přímo nebo prostřednictvím proxy serveru.
- S [požadavky na virtuální počítač](site-recovery-best-practices.md#virtual-machines)musí vyhovovat virtuálních počítačích, které chcete zamknout.


## <a name="step-1-sign-in-to-your-azure-account"></a>Krok 1: Přihlášení k účtu Azure


1. Spusťte konzolu prostředí PowerShell a zadejte tento příkaz se přihlásit ke svému účtu Azure. Rutiny vyvolá webovou stránku, která vás vyzve k svoje přihlašovací údaje účtu.

        Login-AzureRmAccount

    Případně můžete rovněž uvést svoje přihlašovací údaje účtu jako parametr `Login-AzureRmAccount` rutina pomocí `-Credential` parametr.

    Pokud jste CSP partnera práce jménem klienta, zadejte zákazníka ke klientovi na jeho název primární domény tenantID nebo klienta.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Účet mohou obsahovat více předplatných, takže byste neměli spojovat předplatné, které chcete použít s účtem.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Ověřte, že vaše předplatné registrované pro účely Azure poskytovatelů služby Recovery a obnovení webu pomocí následujících příkazů:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    Do výstupu tyto příkazy Pokud **RegistrationState** je nastavený na **Registrováno**, můžete přejít ke kroku 2. V opačném případě byste měli zaregistrovat chybějící poskytovatele předplatné.

    K registraci Azure poskytovatele obnovení webu a obnovení služeb, spusťte následující příkazy:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Ověřte, že zprostředkovatelé registrovány úspěšně pomocí následujících příkazů: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` a `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Krok 2: Nastavení služby Recovery trezoru

1. Vytvořte skupinu zdroje správce prostředků Azure, ve kterém budete vytvářet trezoru nebo použít existující skupinu zdroje. Vytvoření nové skupiny prostředků pomocí tento příkaz:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    kde proměnnou $ResourceGroupName obsahuje název zdroje skupinu, kterou chcete vytvořit a proměnná $Geo obsahuje Azure oblast, ve kterém chcete vytvořit skupina zdroje (například Brazílie, že hodnota jih").

    Seznam skupin zdrojů lze získat ve vašem předplatném pomocí `Get-AzureRmResourceGroup` rutiny.

2. Vytvoření nové služby Azure Recovery trezoru následujícím způsobem:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Načtení seznamu existující trezorů pomocí `Get-AzureRmRecoveryServicesVault` rutiny.

> [AZURE.NOTE] Pokud chcete provádět operace s trezorů obnovení webu vytvořené pomocí portálu klasické nebo modul Azure služby správy Powershellu, můžete načtěte seznam těchto trezorů pomocí `Get-AzureRmSiteRecoveryVault` rutiny. Vytvoříte nové služby Recovery trezoru pro všechny nové operace. Obnovení webu trezorů, které jste dříve vytvořili formát podporuje, ale nemáte nejnovější funkce.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavení služby Recovery trezoru kontextu

1.  Nastavte kontextu trezoru pomocí tohoto příkazu:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Krok 4: Vytvoření webu pro Hyper-V a generovat nové registračního klíče trezoru pro daný web.

1. Vytvoření nového webu Hyper-V následujícím způsobem:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Tato rutina spuštění úlohy obnovení webu vytvořit web a vrátí objektu úlohy obnovení webu. Počkejte úkoly k dokončení a ověřte, že úloha byla úspěšně dokončena.

    Načtení objektu úlohy a následné zkontrolovat aktuální stav úlohy pomocí rutiny Get-AzureRmSiteRecoveryJob.
2. Generovat a stáhnout registrační klíč pro konkrétní web následujícím způsobem:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Zkopírujte stažené klíč pro Hyper-V hostiteli. Potřebujete klávesu zaregistrovat hostiteli Hyper-V na web.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Krok 5: Instalace zprostředkovatele obnovení webu Azure a agenta služby Azure obnovení na hostitele Hyper-V

1. Stáhněte instalační program pro nejnovější verzi zprostředkovatele od [Microsoftu](https://aka.ms/downloaddra).

2. Spuštění instalačního programu na hostitele Hyper-V a na konci instalace pokračujte krokem registrace.

3. Po zobrazení výzvy zadejte stažený webu registračního klíče a dokončení registrace hostiteli Hyper-V na web.

4. Ověřte, že je hostiteli Hyper-V registrovaná na web pomocí následujícího příkazu:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Krok 6: Vytvoření zásad replikace a přidružit kontejneru ochrany

1. Vytvoření zásad replikace následujícím způsobem:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Zaškrtněte políčko vrácené úlohu, která zajistí, že se mu vytváření zásad replikace.

    >[AZURE.IMPORTANT] Účet úložiště zadaný by měl být stejný Azure oblasti jako trezoru služby Recovery a by měla být geo replikace povolené.
    >
    > - Pokud zadaný obnovení úložiště je typu Azure úložiště (klasický), převzetí chráněné strojů obnovit připojení počítače k Azure IaaS (klasický).
    > - Pokud zadané obnovení účtu úložiště je typu Azure úložiště (Azure správce zdrojů), převzetí chráněné strojů obnovit připojení počítače k Azure IaaS (Správce prostředků Azure).

2. Získání kontejneru ochranu odpovídající na web následujícím způsobem:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Začněte přidružení kontejneru ochranu zásady replikace následujícím způsobem:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Počkejte na dokončení úlohy přidružení a ujistěte se, že ho byla úspěšně dokončena.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolte ochranu virtuálních počítačích

1. Získání entity ochranu odpovídající OM, které chcete zamknout, následujícím způsobem:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Začněte chránit virtuálního počítače, následujícím způsobem:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Účet úložiště zadaný by měl být stejný Azure oblasti jako trezoru služby Recovery a by měla být geo replikace povolené.
    >
    > - Pokud zadaný obnovení úložiště je typu Azure úložiště (klasický), převzetí chráněné strojů obnovit připojení počítače k Azure IaaS (klasický).
    > - Pokud zadané obnovení účtu úložiště je typu Azure úložiště (Azure správce zdrojů), převzetí chráněné strojů obnovit připojení počítače k Azure IaaS (Správce prostředků Azure).

    > Pokud OM chráníte má víc než jeden disk připojená, určete pomocí parametru *OSDiskName* disk operačního systému.

3. Čekat na virtuálních počítačích kontaktovat přísnější po počáteční replikace. To může nějakou dobu trvat, podle různých faktorů, jako jsou množství dat replikovat a k dispozici nadřazeného šířku pásma pro Azure. Stav úlohy a StateDescription se automaticky aktualizují takto, po OM sahající přísnější.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Aktualizujte vlastnosti obnovení, jako je velikost role OM a Azure síťové připojení virtuálního počítače síťové karty na po překlopení.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Spusťte test selhání

1. Spusťte testovací převzetí úlohy, následujícím způsobem:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Ověřte, že test OM je v Azure. (Testovací převzetí úlohy se pozastaví, po vytvoření test OM v Azure. Dokončí úloha tak, že vyčistí vytvořené rušivé vlivy po obnovení úlohy, jak je ukázáno v následujícím kroku.)

3. Dokončení testu převzetí následujícím způsobem:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Další kroky

[Přečtěte si další](https://msdn.microsoft.com/library/azure/mt637930.aspx) informace o obnovení webu Azure pomocí rutin prostředí PowerShell Azure správce prostředků.
