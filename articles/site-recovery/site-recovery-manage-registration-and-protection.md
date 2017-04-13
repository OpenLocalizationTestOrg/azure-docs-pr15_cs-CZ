<properties
    pageTitle="Odebrání servery a zakažte ochranu | Microsoft Azure" 
    description="Tento článek popisuje, jak unregister servery z trezoru obnovení webu a zakázání ochranu virtuálních počítačích a fyzické servery." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Odebrání servery a zákaz ochrany

Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak unregister servery z trezoru obnovení webu a jak zakázat ochranu virtuálních počítačích chráněny obnovení webu. 

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="unregister-a-vmm-server"></a>Zrušení registrace VMM serveru

Odstraněním serveru, na kartě **servery** na portálu obnovení webu Azure unregister serveru VMM z trezoru. Všimněte si, že:

-  **Připojení VMM serveru**: doporučujeme unregister serveru VMM při připojení k Azure. Zajistíte tím, že se správně vyčistí nastavení na serveru VMM místních a servery VMM přidružen (VMM servery, které obsahují shluky, které jsou namapované mračnech na serveru, který chcete odstranit). Doporučujeme, aby že server nepřipojené odeberete pouze při trvalé problém s připojením.
- **Nepřipojené VMM server**: VMM server není připojený po jeho odstranění musíte spustit skript ručně můžete provést vymazání. Skript je dostupná v [Galerii Microsoft](http://aka.ms/asr-cleanup-script-vmm). Poznamenejte si ID VMM serveru k dokončení procesu Ruční vymazání.
- **Server VMM na obrázku**: když potřebujete unregister VMM serveru, který je nasazený v clusteru, postupujte takto:

    - Pokud server je připojené, odstraňte připojený server VMM na kartě **servery** . Chcete-li odinstalovat poskytovatele na serveru, přihlášení na každé clusteru ho v Ovládacích panelech. Spusťte skript vyčištění odkazuje v předchozí části ve všech pasivní uzlech clusteru odstranit položky registru.
    - Pokud není připojený serveru musíte spustit skript vyčištění ve všech uzlech.

### <a name="unregister-an-unconnected-vmm-server"></a>Unregister nepřipojené VMM serveru

Na serveru VMM chcete odebrat:

1. Zrušení registrace serveru VMM z portálu Microsoft Azure.
2. Na serveru VMM Stáhněte skript vyčištění.
3. Otevřete PowerShell s spustit jako správce z možností spuštění zásady pro výchozí (LocalMachine) obor změnit.
4. Postupujte podle pokynů uvedených v článku skript. 

Na serverech VMM, které mají shluky, které jsou spárované s mračnech na serveru, který se chystáte odebrat:

1. Skript vyčištění spustit a postupujte podle kroků 2 až 4.
2. Zadejte ID VMM VMM serveru, na kterém byla zrušena registrace. 
3. Tento skript odeberete registračních údajů VMM serveru a cloudu párování informace.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Zrušení registrace pro Hyper-V serveru na webu pro Hyper-V

Po obnovení webu Azure nasazení chránit virtuálních počítačích umístěný na Hyper-V serveru na webu pro Hyper-V (s bez VMM serveru), můžete zrušit registraci Hyper-V server z trezoru následujícím způsobem:

1. Zakážete ochranu virtuálních počítačích nachází na serveru Hyper-V.
2. Na kartě **servery** na portálu obnovení webu Azure vyberte server > odstranit. Server nemusí být připojeni k Azure, když to uděláte.
3. Spuštěním následujícího skriptu vyčistit nastavení na serveru a unregister z trezoru. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Ukončení ochrana virtuálního počítače Hyper-V

Pokud už nechcete ochrana virtuálního počítače Hyper-V musíte ochranu ho odebrat. V závislosti na tom, jak odebrat zámek můžete potřebovat vyčištění nastavení ochrany ručně v počítači. 

### <a name="remove-protection"></a>Odebrání ochrany

1. Na kartě **virtuálních počítačích** cloudu vlastnosti vyberte virtuálního počítače > **Odebrat**.
2. Na stránce **potvrďte odebrání virtuálního počítače** máte několik možností:

    - **Zakázání ochrany**– Pokud povolíte a tato možnost uložit virtuální počítač bude už chráněny obnovení webu. Nastavení ochrany pro virtuální počítač bude mít automaticky vyčistí.
    - **Odebrat z trezoru**– Pokud vyberete tuto možnost virtuální počítač odeberou pouze z trezoru obnovení webu. Nastavení ochrany v místním počítači virtuální nebude to mít vliv na. Musíte vyčištění ručně nastavení a odebrání nastavení ochrany virtuálního počítače odebrat Azure předplatné a odebrání nastavení zámku bude potřeba je vyčistit ručně pomocí následujícího postupu.

Pokud vyberete odstranit virtuálního počítače a jeho pevných discích budou odebrány z cílové umístění.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Vyčistit nastavení ochrany ručně (mezi VMM weby)

Pokud jste vybrali možnost **Ukončit správu virtuální počítač**, vyčistěte ruční nastavení:

1. Na primárním serveru spusťte tento skript z konzoly VMM vyčištění nastavení primárního virtuálního počítače. V konzole VMM kliknutím na tlačítko prostředí PowerShell spusťte konzolu VMM Powershellu. SQLVM1 nahraďte názvem virtuálního počítače.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Na sekundárním serveru VMM spusťte tento skript vyčištění nastavení sekundární virtuálního počítače:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Na sekundárním serveru VMM po spuštění skript aktualizace virtuálních počítačích na hostitelském serveru Hyper-V tak, aby sekundární virtuální počítač získá znovu zjištěnou konzole VMM.
4. Výše uvedené kroky vymaže server pouze VMM replikace nastavení. Pokud chcete odebrat virtuální počítač replikace virtuálního počítače. Budete muset udělat následující postup, jak primární a sekundární virtuálních počítačích. Spustit pod skriptu pro odebrání replikace a SQLVM1 nahraďte názvem virtuálního počítače.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Vyčistit nastavení ochrany ručně (mezi místním VMM weby a Azure)

1. Na zdrojovém VMM serveru spusťte tento skript vyčištění nastavení primárního virtuálního počítače:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Výše uvedené kroky vymaže server pouze VMM replikace nastavení. Po odebrání replikace ze serveru VMM předstihu odebrat replikace virtuálního počítače spuštěných na hostitelském serveru Hyper-V pomocí tohoto skriptu. Nahraďte názvem virtuální počítač a host01.contoso.com podle názvu serveru pro Hyper-V SQLVM1.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Vyčistit nastavení ochrany ručně (až weby pro Hyper-V Azure)

1. Na hostitelském serveru zdroj Hyper-V odebrat replikace virtuálního počítače pomocí tohoto skriptu. SQLVM1 nahraďte názvem virtuálního počítače.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Ukončení ochrana VMware virtuálního počítače nebo pole fyzicky serveru

Pokud už nechcete ochrana VMware virtuálního počítače nebo pole fyzicky serveru musíte ochranu ho odebrat. V závislosti na tom, jak odebrat zámek můžete potřebovat vyčištění nastavení ochrany ručně v počítači. 

### <a name="remove-protection"></a>Odebrání ochrany

1. Na kartě **virtuálních počítačích** cloudu vlastnosti vyberte virtuálního počítače > **Odebrat**.
2. Na **Odebrat počítač virtuální** vyberte jednu z možností:

    - **Zakázání ochrany (slouží k obnovení velikosti přechodu na podrobnosti a hlasitost)**– budete pouze zobrazování projektu a povolit tuto možnost, pokud jste:
        - **Resized hlasitost virtuálního počítače**– při změně velikosti hlasitost virtuální počítač přejde do stavu kritických. Tuto možnost vyberte, pokud k tomu dojde. Zakáže ochranu a ponechá obnovení bodů v Azure. Pokud povolíte ochrany počítače dojde k Azure převedení dat pro změněnou velikostí hlasitost.
        - **Spuštění selhání**– po otestování prostředí spuštěním přepojení z místního VMware virtuálních počítačích nebo pole fyzicky servery Azure vyberte tuto možnost a začněte znova chránit vaše místní virtuálních počítačích. Tato možnost vypne virtuální počítače a pak budete muset znovu povolit ochranu pro ně. Všimněte si, že:
            - Zakázání virtuálního počítače s tímto nastavením nemá vliv na otevřené virtuálního počítače v Azure.
            - Služba mobilita nesmí odinstalovat virtuální počítač.
    
    - **Zakázání ochrany**– Pokud povolíte a tato možnost uložit do počítače se už chráněny obnovení webu. Nastavení ochrany počítače budou se automaticky vyčistí.
    - **Odebrat z trezoru**– Pokud vyberete tuto možnost počítači odeberou pouze z trezoru obnovení webu. Nastavení ochrany místního počítače nebude to mít vliv na. Odebrání nastavení počítače a virtuálního počítače odeberete Azure předplatné a budete muset vyčistit nastavení odinstalováním službu nastavení mobilních zařízení.
    
        ![Odebrání možností](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















