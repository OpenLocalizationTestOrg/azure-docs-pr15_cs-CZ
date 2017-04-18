<properties
    pageTitle="Pomocí serveru SQL Server Azure Premium úložiště | Microsoft Azure"
    description="Tento článek využívá zdroje vytvořená pomocí klasické nasazení modelu a nabízí návod úložišti Premium Azure pomocí serveru SQL Server na virtuálních počítačích Azure spuštěné."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Použití úložiště Azure Premium se serverem SQL Server na virtuálních počítačích


## <a name="overview"></a>Základní informace

[Azure Premium úložiště](../storage/storage-premium-storage.md) je další generování úložiště, která obsahuje zhoršeným latence a s vysokou vstupů/výstupů. Je ideální pro klíčové vstupu a výstupu náročné úloh, třeba SQL Server ve [virtuálních počítačích](https://azure.microsoft.com/services/virtual-machines/)IaaS funguje.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tento článek obsahuje plánování a pokyny pro migraci do virtuálního počítače se systémem SQL serveru můžete Premium úložiště. Platí to i Azure infrastruktury (sociální sítě, ukládání) a hosta Windows OM kroky. Příklad v [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) ukazuje úplné komplexní migrace konce jak přesunout větší VMs umožní využít výhod vylepšené místní úložiště SSD pomocí prostředí PowerShell.

Je třeba porozumět procesu začátku do konce použití úložiště Premium Azure se serverem SQL Server na IAAS VMs. Tato volba zahrnuje:

- Identifikace předpoklady použití Premium úložiště.
- Příklady nasazení serveru SQL Server na IaaS k základnímu úložišti Premium nové nasazení.
- Příklady migrace stávajícího nasazení najdete samostatné servery a nasazení pomocí SQL vždy na dostupnost skupiny.
- Možné migrace přístupů.
- Příklad úplného začátku do konce zobrazující Azure, Windows a SQL Server kroky migrace stávající implementace vždy na.

Podrobnější informace o serveru SQL Server ve virtuálních počítačích Azure najdete v článku [SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** ADAM Sol **technické recenzentů:** Leoš Roman Vargas sleďů Sanjay Mishra, Pravin Mital, Juergen Kutěj, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Požadavky na úložiště Premium

Existuje několik požadavcích na používání Premium úložiště.

### <a name="machine-size"></a>Velikost počítače

Použití úložiště Premium musíte používat řadu DS virtuálních počítačích (OM). Pokud nepoužijete počítačích DS řady v cloudové službě před, musíte odstranit existující OM, zachovat připojených disků a vytvořte nové Cloudová služba před znovu vytvářeli OM jako DS * role velikost. Další informace o velikosti virtuálního počítače najdete v článku [virtuální počítač a velikosti cloudové služby Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Cloud services

Vytvoření nového cloudové služby, můžete použít jenom DS * VMs s úložištěm Premium. Pokud používáte SQL Server vždy na v Azure, vždy na posluchače bude odkazovat na adresu Azure interní a externí IP Vyrovnávání zatížení, který je spojený s do cloudové služby. Tento článek se zaměřuje na tom, jak migrovat zachování dostupnosti v tomto scénáři.

> [AZURE.NOTE] Série DS * musí být první OM, které je nasazené nové cloudové služby.

### <a name="regional-vnets"></a>Místní VNETS

Pro DS * VMs musíte nakonfigurovat virtuální síti (VNET) hostingu VMs jako místní. To "rozšiřuje" VNET umožňují větší VMs v jiných clusterů zřízení a povolit komunikaci mezi nimi. Následující snímek obrazovky zobrazuje zvýrazněný umístění místní VNETs, že první výsledek zobrazuje "úzký" VNET.

![RegionalVNET][1]

Můžete zvýšit Microsoft požadavek podpory můžete migrovat do místního VNET, Microsoft provádění změn na a pak dokončete migrace pro místní VNETs změnit vlastnost AffinityGroup v konfiguraci sítě. Nejdřív export konfigurace sítě v prostředí PowerShell a nahradit vlastnost **AffinityGroup** v elementu **VirtualNetworkSite** vlastnost **umístění** . Určení `Location = XXXX` kde `XXXX` je Azure oblast. Importujte nové konfigurace.

Rozhodování, například následující VNET konfigurace:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Přesunout to místní VNET v Evropě západní, změňte konfiguraci následujícím způsobem:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Účty úložiště

Bude potřeba vytvořit nový účet úložiště, který je nakonfigurovaný pro Premium úložiště. Všimněte si, že využívání Premium úložiště je nastavený na účtu úložiště na jednotlivé VHD, ale při použití řady OM DS * virtuálního pevného disku je můžete připojit z účtů Premium a standardní úložiště. Může to zvažte Pokud nebudete chtít umístěte virtuální pevný disk OS k účtu úložiště Premium.

Příkaz **Nový AzureStorageAccountPowerShell** s "Premium_LRS" **Typ** vytvoří účet úložiště Premium:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Nastavení mezipaměti VHD

Hlavní rozdíl mezi vytváření disků, které jsou součástí účet Premium úložiště je nastavení diskové mezipaměti. SQL Server Data hlasitost disků se doporučuje se doporučuje používat "**Ukládání do mezipaměti pro čtení**". Pro transakční protokol svazky nastavení diskové mezipaměti by měl nastavit "**žádný**". Tím se liší od doporučení pro účty standardní úložiště.

Po připojení VHD nelze změnit nastavení mezipaměti. Musíte odpojit a znovu virtuální pevný disk s nastavením aktualizované mezipaměti.

### <a name="windows-storage-spaces"></a>Prostor úložiště systému Windows

Můžete použít [Windows úložiště Spaces](https://technet.microsoft.com/library/hh831739.aspx) stejně jako u předchozí standardní úložiště, bude možné migrace OM, který je již využití úložiště mezery. [Dodatek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (krok 9 a přesměrování) ukazuje kód prostředí Powershell extrahovat a import OM s více připojených VHD.

Úložiště skupin používaly s účtem úložiště standardní Azure zlepšit výkon a zmenšení latence. Hodnota můžete najít ve testování úložiště fondů s úložištěm Premium nové nasazení, ale přidat další složitost s nastavením úložiště.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Jak najít které virtuálních disků Azure mapování a fondu úložiště

Jsou jiné mezipaměti nastavení doporučení pro připojený VHD rozhodnout zkopírovat VHD k účtu úložiště Premium. Když je znovu připojit nová Pošta řada OM, můžete zkusit změnit nastavení mezipaměti. Je jednodušší použít úložiště Premium doporučuje nastavení mezipaměti, když máte samostatné VHD pro SQL datové soubory a soubory protokolu (spíše než jednoho virtuálního pevného disku, který obsahuje obojí).

> [AZURE.NOTE] Pokud máte SQL Server data a souborů protokolu ve stejném svazku, možnost ukládání do mezipaměti, jaká jste závisí na vstupu a výstupu vzorků přístup pro databázi úloh. Pouze testování prokázat, kterou možnost je nejvhodnější pro tento scénář.

Pokud používáte Windows úložiště mezery, které se skládají z více VHD budete muset podívejte se na původní skriptů k identifikaci níž připojena VHD jsou však do jaké konkrétní fondu, takže pak můžete nastavit nastavení mezipaměti příslušným způsobem pro každý disk.

Pokud nemáte původní skript k dispozici pro vidíte, které VHD mapování do fondu úložiště, můžete určit mapování fondu/úložištích podle těchto kroků.

Pro každý disku použijte podle těchto kroků:

1. Zobrazit seznam disků připojené k OM pomocí příkazu **Get-AzureVM** :

    Get-AzureVM - ServiceName <servicename> – název <vmname> | Get-AzureDataDisk

1. Poznámka: Diskname a logické jednotky.

    ![DisknameAndLUN][2]

1. Vzdálená plocha do OM. Přejděte na **Správa počítače** | **Správce zařízení** | **diskových jednotek**. Podívejte se na vlastnosti každého "Microsoft virtuálních disků"

    ![VirtualDiskProperties][3]

1. Číslo logické jednotky tady je odkaz na číslo logickou jednotku, které zadáte při připojování virtuální pevný disk bude v angličtině.
1. Za "Microsoft virtuální Disk", přejděte na kartu **Podrobnosti** , klikněte v seznamu **vlastností** přejděte **Ovladač**s klávesou CTRL. Do pole **hodnota**Všimněte si, **Odsazení**, což je 0002 následující snímek obrazovky. 0002 označuje PhysicalDisk2, který odkazuje fondu úložiště.

    ![VirtualDiskPropertyDetails][4]

2. Pro každý fondu úložiště výpis se přidružené disků:

    Get-StoragePool - FriendlyName AMS1pooldata | Get fyzický

    ![GetStoragePool][5]

Teď můžete použít tyto informace zobrazily přidružení VHD přiřazená discích do úložiště skupin.

Jakmile jste změnili VHD discích do úložiště skupin můžete pak odpojení a ke zkopírování přes účet Premium úložiště a potom připojení s nastavením správné mezipaměti. Prosím podívejte se na příklad v [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)kroky 8 až 12. Tento postup ukazují, jak extrahovat konfigurace disku připojené OM virtuální pevný disk do souboru CSV, zkopírujte VHD, změnit nastavení mezipaměti konfigurace disku a nakonec přeinstalujte OM jako DS řada OM s připojeným discích.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Šířka pásma OM úložiště a výkon úložiště virtuální pevný disk

Velikost úložiště výkonu závisí na zadanou DS * OM velikost a velikosti virtuální pevný disk. VMs k dispozici různé příspěvky pro počet VHD, které může být připojen a maximální šířky pásma podporují (MB/ne). Čísla konkrétní šířky pásma najdete v článku [virtuální počítač a velikosti cloudové služby Azure](virtual-machines-linux-sizes.md).

Lepší procesorů dosáhne se větších disku. Byste měli zvážit při úvahách o cestu migrace. Další informace [najdete v tabulce procesorů a typy disku](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Nakonec zvažte, VMs máte jiný maximální disk šířek pásma, který bude podporovat všech disků připojené. Velkém zatížení může nasytit dostupné pro danou velikost role OM šířky pásma maximální disku. Například Standard_DS14 podporují nahoru o velikosti 512 MB/s; Proto se tři P30 disků může nasytit disku šířku pásma OM. Ale v tomto příkladu by mohl být limit výkon překročen v závislosti na kombinaci pro čtení a zápis IOs.

## <a name="new-deployments"></a>Nové nasazení

Následující dva oddíly ukazují, jak můžete nasadit VMs SQL serveru k základnímu úložišti Premium. Jak je uvedeno před nepotřebujete nutně umístit disku OS na Premium úložiště. Je možné provést, pokud jsou úmyslu všechny náročné pracovního vytížení vstupu a výstupu včasné OS virtuální pevný disk.

První příklad ukazuje využití existujících Azure Galerie obrázků. Druhý příklad ukazuje, jak používat vlastní obrázek OM, který jste ještě do existujícího účtu standardní úložiště.

> [AZURE.NOTE] V příkladech se předpokládá, že jste již vytvořili místní VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Vytvoření nového OM s úložištěm Premium s obrázkem po posunutí Galerie

V příkladu níže ukazuje, jak umístěte OS virtuální pevný disk na premium úložiště a připojte VHD úložiště Premium. Můžete však také umístit disku OS účet standardní úložiště a připojte k ní VHD, které se nacházejí v účtu úložiště Premium. Obou případech je znázorněn.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Krok 1: Vytvoření účtu úložiště Premium


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Krok 2: Vytvoření nového cloudové služby

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Krok 3: Rezervujte VIP cloudové služby, (volitelné)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Krok 4: Vytvoření kontejneru OM
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Krok 5: Umístění s operačním systémem virtuální pevný disk na standardní nebo Premium úložiště
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Krok 6: Vytváření OM
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Vytvoření nového OM Premium úložiště pomocí vlastní obrázek

Tento scénář ukazuje, kde máte existující vlastní obrázky umístěné ve standardní ukládání do účtu. Jak je uvedeno, pokud chcete umístit virtuální pevný disk OS úložný prostor Premium musíte zkopírovat obrázek, který neexistuje účet standardní úložiště a přenést do úložiště Premium před použitím. Pokud máte obrázek místního nasazení, můžete také tato metoda, zkopírujte přímo do účtu úložiště Premium.

#### <a name="step-1-create-storage-account"></a>Krok 1: Vytvoření účtu úložiště
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Krok 2 vytvořte cloudové služby
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Krok 3: Použití existující obrázek
Můžete použít existující obrázek. Nebo můžete [udělat obrázek existující počítače](virtual-machines-windows-classic-capture-image.md). Všimněte si do počítače, můžete obrázek nemusí být DS* počítače. Až budete mít obrázku, podle těchto kroků ukazují, jak ho zkopírujte do úložiště Premium účet u * *Start AzureStorageBlobCopy** prostředí PowerShell..

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Krok 4: Kopírování objektů Blob mezi účty úložiště
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Krok 5: Zkontrolujte pravidelně Kopírovat stav:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Krok 6: Přidání obrázek disku Azure disk úložiště v předplatného
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Můžete zjistit, i když je stav hlásí úspěch, může pořád vám chyba zapůjčení disku. V tomto případě počkejte asi 10 minut.

#### <a name="step-7--build-the-vm"></a>Krok 7: Sestavení OM
Tady jsou vytváříte OM z obrázku a připojení dvou VHD úložiště Premium:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Existující nasazení, které nepoužívají vždy na skupiny dostupnosti

> [AZURE.NOTE] Existující nasazení nejdřív naleznete v části [požadavky](#prerequisites-for-premium-storage) v tomto tématu.

Existují různé aspekty nasazení serveru SQL Server, které nepoužívají vždy na skupiny dostupnost a ty, které se. Pokud nepoužíváte vždy na a máte existující samostatného serveru SQL Server, můžete pomocí nového účtu služby a úložiště cloudu upgradovat k základnímu úložišti Premium. Zvažte následující možnosti:

- **Vytvoření nového OM SQL serveru**. Můžete vytvořit nové OM serveru SQL, který používá účet Premium úložiště, jak je uvedeno v nové nasazení. Potom zálohování a obnovení databáze SQL serveru konfigurace a uživatele. Aplikace muset aktualizovat neodkazuje nové SQL serveru, pokud ho přistupuje interně nebo externě. Musíte zkopírujte všechny objekty "mimo db", jako kdybyste jste dělali migrovat SQL Server (souběžně sdílená) vedle sebe. Jedná se o objekty, jako jsou přihlášení, certifikáty a propojené servery.
- **Migrace stávajícího OM Server SQL**. To vyžaduje přijímat OM Server SQL v režimu offline a pak přenos ji do nového cloudové služby, včetně všech svých připojených VHD kopírování k tomuto účtu úložiště Premium. OM vycházejí online, aplikace odkazovat název serveru jako před. Mějte na paměti, že velikost existující disk ovlivní ukazatele výkonu. Například disk 400 GB získá zaokrouhlený P20. Pokud znáte nevyžadují výkon tohoto disku, může obnovil OM jako OM řady Pošta a připojit Premium úložiště VHD specifikace velikost a výkon, kterou požadujete. Pak můžete odebrat a znovu soubory databáze SQL.

> [AZURE.NOTE] Při kopírování disků virtuální pevný disk byste měli znát velikosti, v závislosti na velikosti bude znamenat, jaký typ Premium úložiště disku spadají, tato možnost určuje specifikace výkonu disku. Azure bude zaokrouhlení nahoru na nejbližší disku velikost, takže máte disk 400 GB to bude možné zaokrouhlený P20. V závislosti na vaší stávající vstupu a výstupu požadavky OS virtuálního pevného disku může se stát potřebujete migrovat to k účtu úložiště Premium.

Pokud váš Server SQL přistupuje externě, se změní VIP služby cloudu. Budete taky muset aktualizace koncové body, ACL a DNS nastavení.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Existující nasazení, které používají vždy na skupiny dostupnosti

> [AZURE.NOTE] Existující nasazení nejdřív naleznete v části [požadavky](#prerequisites-for-premium-storage) v tomto tématu.

Původně v této části se podíváme na vždy o interakci s Azure sítě. Budeme se pak rozdělí migrací ve dvou situacích: migrace místo, kam můžete přípustné některé výpadek služeb a migrace kde musí dosažení minimální prostoje.

Místní SQL Server vždy na dostupnost skupiny pomocí posluchače místní, které zaregistruje virtuální název DNS spolu s IP adresu, která se budou sdílet mezi více serverů SQL. Při připojení klientů jsou směrovány IP posluchače primární SQL Server. Jedná se o server, který vlastní prostředek vždy na IP v té době.

![DeploymentsUseAlways na][6]

V aplikaci Microsoft Azure může mít jenom jeden IP adresa přiřazená NIC na OM tak k dosažení stejné vrstvě odběru jako místní, Azure využívá IP adresy, kterou je přiřazen k vyrovnávání zatížení interní/externí (ILB/ELB). IP prostředek, který se sdílí mezi servery je nastavena na stejné IP jako ILB/ELB. To je publikovaných v galerii DNS a provoz klientů projde ILB/ELB otevřené primární SQL Server. ILB/ELB ví, tedy SQL Server primární od zdrojů vždy na IP probe pomocí sond. V předchozím příkladu sond jednotlivých uzlech, který má koncový bod odkazováno ELB/ILB, podle toho, které odpoví je primární SQL Server.

> [AZURE.NOTE] ILB a ELB obě přiřazené určité Azure cloudové služby, proto všechny migrace cloudu v Azure nejspíš znamená, že se změní IP Vyrovnávání zatížení.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrace vždy o nasazení povolujících některé výpadek služeb

Existují dva strategie migrace vždy o nasazení, které umožňují některé výpadku:

1. **Přidat další sekundární repliky do existujícího vždy na clusteru**
1. **Migrace do nového vždy na clusteru**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. přidáte další sekundární repliky do existujícího vždy na clusteru

Jedna strategie je přidat další druhotné vždy na dostupnost skupiny. Budete muset přidat do nového Cloudová služba a aktualizovat posluchače nové IP Vyrovnávání zatížení.

##### <a name="points-of-downtime"></a>Aspekty výpadek služeb:

- Ověření obrázku.
- Testování vždy o převzetí služeb při selhání pro nové druhotné.

Pokud používáte Windows úložiště fondů v OM pro vyšší výkon vstupu a výstupu, potom tyto budou do režimu offline během úplné ověřování obrázku. Test ověření je potřeba při přidání uzly clusteru. Časové náročnosti testu se může lišit, takže je vhodné otestovat ve vašem prostředí zástupce test získat Přibližná doba jak dlouho bude trvat.

Zřizujete čas, kde můžete provést ruční převzetí a chaos testování nově přidaný uzlech, aby vždy na dostupnost funkcí podle očekávání.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Byste měli zastavit všechny instance serveru SQL Server použití fondu úložiště před spuštěním ověření.
##### <a name="high-level-steps"></a>Podrobný postup

1. Vytvořte dva nové servery SQL v nové cloudové službě s připojených prémií úložiště.
1. Zkopírujte CELOU zálohování a obnovení s **NORECOVERY**.
1. Zkopírovat "mimo uživatelem DB" závislé objekty, například přihlášení atd.
1. Vytvořit nový na nový vyrovnávání pro interní zatížení (ILB) nebo použijte externí zatížení vyrovnávání (ELB) a pak nastavení koncové body Vyrovnávání zatížení na obou nové uzly.
> [AZURE.NOTE] Zkontrolujte, že všechny uzly správné konfiguraci Endpoint před pokračováním

1. Ukončení přístupu uživatele/aplikace k serveru SQL Server (Pokud úložiště fondů).
1. Zastavte modul služby SQL Server ve všech uzlech (Pokud úložiště fondů).
1. Přidání nových uzlů clusteru a spouštění úplné ověřování.
1. Po úspěšném ověření spuštění všechny služby SQL Server.
1. Protokoly transakcí zálohování a obnovení uživatelských databází.
1. Přidání nových uzlů do vždy na dostupnost skupiny a replikace do **synchronní**.
1. Přidání zdroje IP adresu z nové cloudové služby ILB/ELB prostřednictvím Powershellu pro vždy na založené na více webů příklad v [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage). V systému Windows clusterů nastavte na nové uzly staré **možné vlastníky** **IP adresu** zdroje. V části "Přidání IP adresu zdroje na stejné podsítě" [Dodatek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Přepnutí do jednu z nových uzlů.
1. Vytvořit nové uzly automatického převzetí partnery a otestujte převzetí služeb při selhání.
1. Odebrání skupiny dostupnosti původní uzlů.

##### <a name="advantages"></a>Výhody

- Nové servery SQL lze otestovat (SQL Server a aplikace) předtím, než se přidají do vždy na.
- Můžete změnit velikost OM a přizpůsobení ukládání vašich přesné požadavků. Však je výhodné zachovat všechny cesty soubor SQL stejné.
- Můžete určit, zahájení sekundární replikám přenosu zálohování databáze. Tím se liší od PowerShell Azure **Start AzureStorageBlobCopy** kopírování pomocí VHD, protože to je asynchronní kopírovat.

##### <a name="disadvantages"></a>Nevýhody
- Pokud používáte Windows úložiště skupin, se při úplné clusteru ověřování pro nové další uzly prostoje obrázku.
- V závislosti na verzi serveru SQL a existující číslo sekundární repliky nebudete moct přidat další sekundární repliky bez odebrání existující druhotné.
- Může to být SQL dat přenos dlouho při nastavování druhotné.
- Je další náklady v průběhu migrace když máte novou počítače se systémem souběžně.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. migrovat do nového vždy na clusteru

Další strategie je vytvořit úplně nový vždy na Cluster s ještě nepracovali uzlů v nové cloudové službě a potom přesměrování klientů používat.

##### <a name="points-of-downtime"></a>Ukazatel výpadek

Při přenosu aplikace a uživatele do nové posluchače vždy na je výpadek služeb. Prostoje závisí na:

- Doba obnovit konečný transakce protokolu zálohy databáze na nové servery.
- Doba aktualizovat klientské aplikace použít nové vždy na posluchače.

##### <a name="advantages"></a>Výhody

- Můžete otestovat skutečné provozním prostředí serveru SQL Server a operačního systému sestavíte změny.
- Máte možnost upravit úložiště a potenciálně zmenšit velikost OM. To může vést k snížení nákladů.
- Během tohoto procesu můžete aktualizovat sestavení SQL serveru nebo verzi. Můžete taky upgradovat operační systém.
- Předchozí vždy na obrázku můžete fungují jako cíle plné vrácení zpět.

##### <a name="disadvantages"></a>Nevýhody

- Potřebujete změnit název DNS posluchače, pokud chcete obou vždy na clusterů spuštění současně. Správa nároky během migrace se přidá jako klientské aplikace řetězce musí obsahovat nový název posluchače.
- Je nutné implementovat mechanismus synchronizace mezi dvěma prostředími ponechat stále co nejtěsněji minimalizovat požadavky na poslední synchronizace před migrací.
- Během migrace se se přidá náklady během máte nové prostředí spuštěný.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrace vždy o nasazení minimální výpadku

Existují dva strategie pro nasazení migrace vždy na minimální výpadku:

1. **Využití existujícího sekundární: jeden webu**
1. **Využití existujícího sekundární Replica(s): více webů**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. využít stávající sekundární: jeden webu

Jedna strategie minimálního výpadku je trvat sekundární existující cloudu a odeberte ho z aktuální cloudové služby. Potom zkopírujte VHD do nového účtu úložiště Premium a vytvořte OM v nové cloudové službě. Aktualizujte posluchače v clusterů a překlopení.

##### <a name="points-of-downtime"></a>Ukazatel výpadek

- Když aktualizujete poslední uzel s koncovým Vyrovnávání zatížení je výpadek služeb.
- Opětovné připojení klientů může zpoždění v závislosti na konfiguraci klientských/DNS.
- Pokud se rozhodnete provádět vždy na obrázku skupiny offline zaměnit IP adres není další prostoje. Můžete předejít pomocí závislost nebo a možné vlastníky přidané IP adresu zdroje. V části "Přidání IP adresu zdroje na stejné podsítě" [Dodatek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Pokud nechcete uzel přidané do zúčastnit se v jako vždy o převzetí partnera, budete muset přidat koncového Azure s odkazem na načíst rovnováha sadu. Po spuštění příkazu **Přidat AzureEndpoint** k tomuto aktuální připojení zůstat otevřít, ale nové připojení k posluchače nebude možné stanovit dokud Vyrovnávání zatížení aktualizoval. Při testování to byl jinak na poslední 90 120seconds, to měli otestovat.

##### <a name="advantages"></a>Výhody

- Není extra náklady vzniklé v průběhu migrace.
- Přímé migrace.
- Jednodušší.
- Umožňuje lepší procesorů z úložiště Premium skladové jednotky. Když disků oddělit od OM a zkopírují do nového cloudové služby, 3rd narozeninovou párty s motivy nástroj mohou sloužit k zvětšení virtuálního pevného disku, který poskytuje vyšší propustnosti. Zvětšení velikosti virtuálního pevného disku, najdete v článku této [diskusní fóra](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nevýhody

- Během migrace se dočasná ztráta HA a DR.
- Jak jde migrovat 1:1, budete muset používat minimální velikost OM podporující vaše číslo VHD, takže nebudete moci downsize vaše VMs.
- Tento scénář byste použili PowerShell Azure **Start AzureStorageBlobCopy** , tedy asynchronní. Po dokončení kopie je žádné SLA. Čas kopie se liší během to záleží na čekání ve frontě, které se také závisí na množství data, která chcete převést. Čas kopírovat zvyšuje Pokud přenosu přejdete na jiný Azure datovém centru, které podporuje Premium úložiště v jiné oblasti. Máte jenom 2 uzly, zvažte možná řešení v případě, že výtisku trvá déle než při testování. Může se jednat o následující nápady.
    - Přidejte dočasné 3 uzel SQL serveru pro HA před migrací s schválené prostoje.
    - Spusťte migraci mimo Azure plánovanou údržbu.
    - Zkontrolujte, zda že jste nakonfigurovali kvora clusteru správně.  

##### <a name="high-level-steps"></a>Podrobný postup

Tento dokument neuvádí dokončení příkladu konce však [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) obsahuje podrobné informace, které můžete využít k provedení této.

![MinimalDowntime][8]

- Shromáždění konfigurace disku a odebrání uzel (neodstraňujte připojených VHD).
- Vytvoření účtu úložiště Premium a zkopírujte VHD z účtu standardní úložiště
- Vytvoření nového Cloudová služba a přeinstalujte OM SQL2 v této cloudové službě. Vytvoření OM pomocí zkopírovaný původní OS virtuálního pevného disku a připojení zkopírovaný VHD.
- Konfigurace ILB / ELB a přidejte koncové body.
- Můžete buď aktualizujte posluchače:
    - Přepnutí do offline režimu vždy o skupině a aktualizace vždy na posluchače s novou ILB / ELB IP adres.
    - Nebo jeho přidáním IP adresu z nového cloudové služby ILB/ELB pomocí prostředí PowerShell do clusterů systému Windows. Potom nastavte možných vlastníků prostředku IP adresu migrované uzel SQL2 a to jako závislostí nebo do pole název sítě. V části "Přidání IP adresu zdroje na stejné podsítě" [Dodatek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Kontrola konfigurace DNS/odesílatel klientům.
- Migrace SQL1 OM a projděte si kroky 2 až 4.
- Pokud používáte 5ii kroky, přidejte SQL1 jako vlastníkem přidané prostředek IP adresy
- Otestujte převzetí služeb při selhání.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. využít stávající sekundární replica(s): více webů

Pokud máte uzly ve víc než jednom datacentru Azure (Datacentrum) nebo pokud máte hybridního prostředí, můžete pomocí konfigurace vždy na v tomto prostředí minimalizovat výpadek služeb.

Přístup je změnit synchronizace vždy na synchronní místní nebo vedlejší Datacentrum Azure, a potom klepněte na převzetí myši na tento Server SQL. Pak zkopírujte VHD k účtu úložiště Premium a přeinstalujte počítači do nového cloudové služby. Aktualizujte posluchače a potom navrácení.

##### <a name="points-of-downtime"></a>Ukazatel výpadek

Prostoje se skládá z doby přepnutí na alternativní Datacentrum a zpět. Dále závisí na konfigurace DNS/klienta a opětovné připojení klientů může zpoždění.
Příklad hybridní vždy na konfigurace:

![MultiSite1][9]

##### <a name="advantages"></a>Výhody

- Můžete využít stávající infrastrukturu.
- Máte možnost před upgradem Azure úložiště na web DR Azure Datacentrum nejdřív.
- Úložiště DR Azure Datacentrum lze překonfigurovat.
- Během migrace, s výjimkou test převzetí služeb při selhání je nejméně dvě převzetí služeb při selhání.
- Přesunutí SQL Server dat pomocí zálohování a obnovení nepotřebujete.

##### <a name="disadvantages"></a>Nevýhody

- V závislosti na klientský přístup k serveru SQL Server může být lepší latence když SQL Server běží alternativní Datacentrum aplikaci.
- Kopírovat čas VHD k základnímu úložišti Premium může být dlouhé. Může to mít vliv vaše rozhodnutí, zda chcete-li zachovat uzel ve skupině dostupnosti. Zvažte možnost pro protokolu náročné práce během migrace se systémem zatížení při Povinní, protože primární uzel bude mít nereplikovaných transakce byste měli mít na jeho transakční protokol. Proto to může zvětšit výrazně.
- Tento scénář byste použili PowerShell Azure **Start AzureStorageBlobCopy** , tedy asynchronní. Po dokončení je žádné SLA. Čas kopie se liší během to záleží na čekání ve frontě, bude taky závisí na množství data, která chcete převést. Proto máte jenom jednu uzel v 2 datovém centru, by měl provedete omezení rizik kroky v případě, že výtisku trvá déle než při testování. Může se jednat o následující nápady.
    - Přidejte dočasné 2 uzel SQL pro HA před migrací s schválené prostoje.
    - Spusťte migraci mimo Azure plánovanou údržbu.
    - Zkontrolujte, zda že jste nakonfigurovali kvora clusteru správně.

Tento scénář předpokládá, že jsou popsány instalací a vědět, jak namapovala úložiště před uskutečněním změny pro optimální diskové mezipaměti.

##### <a name="high-level-steps"></a>Podrobný postup
![Multisite2][10]

- Volání místní / alternativní Datacentrum Azure SQL Server primární a usnadňují jiných automatického převzetí partnera (AFP).
- Shromážděte informace o konfiguraci disků ze SQL2 a odebrání uzel (neodstraňujte připojených VHD).
- Vytvoření účtu úložiště Premium a zkopírujte VHD z účtu standardní úložiště.
- Vytvořit nové Cloudová služba a OM SQL2 s jeho disků prémií úložiště připojené.
- Konfigurace ILB / ELB a přidejte koncové body.
- Aktualizace vždy na posluchače u nových ILB / ELB IP adresa a otestujte překlopení.
- Kontrola konfigurace DNS.
- Přejděte AFP SQL2, migrace SQL1 a projděte si kroky 2 až 5.
- Otestujte převzetí služeb při selhání.
- Přejděte AFP zpátky do SQL1 a SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Dodatek: Migrace více pracovišť vždy na obrázku na Premium úložiště

Zbývající část tohoto tématu poskytuje podrobné příklad o převedení clusteru vždy na více webů na Premium úložiště. Převede také posluchače pomocí vyrovnávání zatížení externí (ELB) na Vyrovnávání zatížení vnitřní (ILB).

### <a name="environment"></a>Prostředí

- Windows 2k 12 / SQL 2k 12
- 1 DB souborů na SP
- 2 x fondů úložiště na uzel

![Appendix1][11]

### <a name="vm"></a>OM:

V tomto příkladu budeme ukazují přesouvající se z ELB k ILB. ELB bylo k dispozici před ILB, takže jak můžete přejít na to během migrace se zobrazí.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Před kroky: Připojení k předplatnému

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Krok 1: Vytvoření nového účtu úložiště i cloudových služeb
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Krok 2: Zvýšení povolené selhání na zdroje<Optional>
Na některé prostředky, které patří do vždy na dostupnost skupiny jsou omezení na kolik chyby, ke kterým může dojít v období, kde se pokusí službu clusteru restartujte skupina zdroje. Je vhodné že můžete zvětšit zatímco jste jsou rozbor tento postup od Pokud není ručně převzetí signálu a aktivační signál převzetí služeb při selhání tak, že vypnutí počítačích získané zavřít toto omezení.

Je vhodné poklikejte na příspěvek selhání, můžete to udělat v správce, přejděte na vlastnosti vždy na pole Skupina zdroje:

![Appendix3][13]

Změňte maximální selhání až 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Krok 3: Přidání IP adresu zdroje pro skupinu clusterů<Optional>

Pokud máte jenom jednu IP adresu skupiny clusterů a toto zarovnání podsítě cloudu, mějte na paměti, pokud se omylem převést do offline všech uzlech v cloudu v této síti pak zdroje IP clusteru a clusteru Network Name nebudou moct přechodu do online. V případě to zabrání aktualizace na jiné zdroje obrázku.

#### <a name="step-4-dns-configuration"></a>Krok 4: Konfigurace DNS

Implementace hladký přechod závisí na tom, jak DNS probíhá využít a aktualizovat.
Vždy je nainstalovaná, vytvoří skupinu Windows clusteru zdrojů, pokud jste otevřeli správce, zobrazí se, že ho bude mít minimálně tři zdroje, jsou dva, které odkazuje dokumentu:

- Virtuální Network Name (VNN) – to je název DNS klienta připojení chtějí připojit k serverům SQL pomocí vždy zapnuto.
- IP adresa – to je zdroj IP adresu, kterou spojená VNN, může mít víc než jeden a v konfiguraci více pracovišť bude mít k IP adrese na webu/podsítě.

Při připojení k serveru SQL Server SQL Server Client ovladač načítat záznamy DNS související s posluchače a pokusíte se připojit ke každé vždy na přidružené IP adresu, pod probereme tato některé faktory, které může ovlivnit to.

Počet současně záznamy DNS, které jsou přidružené k názvu posluchače závisí nejen na počet IP adresy přidružené, ale "RegisterAllIpProviders'setting v převzetí clusterů vždy o VNN zdroje.

Když nasadíte vždy na v Azure existují různé kroky při tvorbě posluchače a IP adresy, budete muset ručně nakonfigurovat 'RegisterAllIpProviders' 1, nejedná o místní nasazení vždy na mají ji už nastavenou na 1.

Je-li "RegisterAllIpProviders" 0, zobrazí se pouze jeden DNS záznam DNS přidružené posluchače:

![Appendix4][14]

Je-li 'RegisterAllIpProviders' 1:

![Appendix5][15]

Následující kód bude výpis VNN nastavení a nastavení za vás, zadejte osobní zprávu, aby se změna projevila budou muset VNN jako offline a zapnutí zpět online Tento záznam v režimu offline posluchače příčinou přerušení připojení klientů.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

V byste potřebovali aktualizovat aktualizované IP adresu, který bude odkazovat Vyrovnávání zatížení posluchače vždy na pozdější migračních kroků to bude zahrnovat IP adresu zdroje odstranění a přidání. Po aktualizaci IP je třeba zajistit novou IP adresu byl aktualizován v zóně DNS a že klienti aktualizujete jejich místní mezipaměti DNS.

Pokud svoje klienty uložena v jiné síti segmentu a odkazují na jiný DNS server, byste měli myslet, co se stane, o přenosu zóny DNS během migrace, jak připojit aplikace čas bude omezena alespoň na úrovni zóny přepojit čas všechny nové IP adresy pro posluchače. Pokud jste v části čas omezení tady, by měl diskutovat o a otestujte vynucení přenos přírůstková zóny s týmy Windows a záznam hostitele DNS také umístit do malá písmena TTL Time To Live (), takže klienti aktualizovat. Další informace najdete v tématu [Přírůstková přenosy zón](https://technet.microsoft.com/library/cc958973.aspx) a [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Ve výchozím nastavení TTL záznamu DNS, který souvisí s posluchače in vždy v Azure je 1200 sekund. Můžete chtít to snížit, pokud jste v části doba omezení při migraci zajistit klienty aktualizovat jejich DNS aktualizace IP adresy pro posluchače. Můžete zobrazit a upravit konfigurace tak, že rozpětí se konfigurace VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Dejte pozor, malá písmena "HostRecordTTL", dojde k vyšší množství přenosů DNS.

##### <a name="client-application-settings"></a>Nastavení aplikace klienta

Pokud SQL klientské aplikace podporuje .net 4.5 SQLClient a pak můžete použít "MULTISUBNETFAILOVER = TRUE" klíčové slovo, které tato doporučuje se mohou být použity jako umožňuje rychlejší připojení k SQL vždy na dostupnost skupiny při selhání. Výčet prostřednictvím všechny adresy IP přidružené k posluchače vždy na paralelně a provede další agresivní rychlost opakování připojení TCP během přepojení.

Další informace týkající se nastavení výše najdete v článku [MultiSubnetFailover klíčových slov a související funkce](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Viz také [SqlClient podpora dostupnost havárie obnovení](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Krok 5: Nastavení kvora obrázku

Jak budete trvá se aspoň jeden SQL Server dolů po jednom, upravte nastavení kvora clusteru používáte-li soubor sdílet monitorovací (FSW) s 2 uzly, byste měli nastavit kvora povolit Většina uzlů a využívání dynamické hlasování a to je umožnit načítání zůstat postavení.


    Set-ClusterQuorum -NodeMajority  

Další informace o správě a konfiguraci kvora clusteru najdete v článku [Konfigurovat a spravovat kvora v systému Windows Server 2012 převzetí clusteru](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Krok 6: Extrahuje existující koncové body a ACL
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Uložte do textového souboru.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Krok 7: Změna partnery převzetí a replikace režimy

Pokud máte víc než 2 servery SQL, by měl změňte převzetí jiné sekundární v jiném Datacentrum nebo na místní "Synchronní" a usnadňují automatické převzetí partnera (AFP), tak udržovat HA při provádění změn. Lze provést pomocí TSQL z upravit přes SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Krok 8: Odebrání sekundárních OM z cloudové služby

By měl můžete naplánovat migraci do cloudu sekundárního uzlu nejdřív, jestli je aktuálně primární, by měl zahájit ruční přepojení.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 9: Změna nastavení v souboru CSV mezipaměti disku a uložení

Pro objemů dat tyto by měl nastavit jen pro čtení.

Pro TLOG svazky tyto stanovit na žádný.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Krok 10: Kopie VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Můžete zkontrolovat stav kopie VHD k tomuto účtu úložiště Premium:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Počkejte, dokud všechny tyto informace jsou zaznamenány jako úspěšnosti.

Informace pro jednotlivé objekty BLOB:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Krok 11: Registrace s operačním systémem disku

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Krok 12: Naimportujte sekundární nové cloudové služby

Následující kód tady taky používá přidané možnost můžete importovat do počítače a používat retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Krok 13: Vytvoření ILB na nové cloudu Svc, přidejte zatížení rovnováha koncové body a ACL
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Krok 14: Aktualizace vždy na
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nyní odeberte původní cloudovou službu IP adresu.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Krok 15: Kontrola aktualizací DNS

Teď zkontrolujte servery DNS v síti klienta SQL serveru a ujistěte se, že clusterů přidal záznam navíc hostitele přidané IP adresu. Pokud tyto servery DNS nebyly aktualizováno, zvažte vynucení přenos zóny DNS a zkontrolujte, že je klienty v nějaké podsítě budou moct překládal obě vždy na IP adresy, jedná, aby se nemusí čekat automatické replikace DNS.

#### <a name="step-16-reconfigure-always-on"></a>Krok 16: Překonfigurovat vždy na

V tomto okamžiku čekáte sekundární uzel migrovaná plně synchronizovat s místním uzel a přepněte do uzlu synchronní replikace a změňte ji AFP.  

#### <a name="step-17-migrate-second-node"></a>Krok 17: Migrace druhý uzel
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Krok 18: Změna nastavení v souboru CSV mezipaměti disku a uložení

Pro objemů dat tyto by měl nastavit jen pro čtení.

Pro TLOG svazky tyto stanovit na žádný.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Krok 19: Vytvoření nového účtu nezávislé úložiště pro sekundární uzel
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Krok 20: Kopie VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Kopírovat stav virtuální pevný disk můžete vyhledat všechny VHD: ForEach ($disk v $diskobjects) {$lun = $disk. Logické jednotky $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Jmenovka_disku $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Počkejte, dokud všechny tyto informace jsou zaznamenány jako úspěch.

Informace pro jednotlivé objekty BLOB: #Check induvidual objektů blob stav Get AzureStorageBlobCopyState-objektů Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd"-kontejneru $containerName-kontextu $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Krok 21: Registrace s operačním systémem disku
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Krok 22: Přidání zatížení rovnováha koncové body a ACL
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Krok 23: Převzetí Test

Teď je třeba povolit migrované uzel synchronizace s místním vždy na uzel, umístěte ji v režimu synchronní replikace a počkejte, až se synchronizuje. Potom přepnutí z místní první uzel poštovním, která je AFP. Jakmile, pracoval, změňte poslední migrované uzel AFP.

By měl testování převzetí služeb při selhání mezi všemi uzly a spusťte když chaos testů zajistit převzetí služeb při selhání práce jako očekávání a ve včasných manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Krok 24: Vrátit nastavení kvóra clusteru / DNS TTL / převzetí Pntrs / nastavení synchronizace
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Přidání zdroje IP adresu na stejné podsítě

Pokud máte jenom 2 servery SQL a chcete migrovat do nového cloudové služby, ale chcete zachovat stejnou podsítě, vyhnete přijímat posluchače offline odstranit původní vždy na IP adresu a přidat novou IP adresu. Při migraci VMs do jiné podsítě nebudete potřebovat k tomu, jak bude síť další obrázku, který bude odkazovat této podsítě.

Jakmile aktualizovány migrované sekundární a přidána do nového prostředku IP adresu pro nové cloudovou službu před převzetí existující primární by měl proveďte Tyhle kroky v rámci vedoucímu překlopení obrázku:

Pokud chcete přidat do pole IP adresa, najdete v článku [dodatku](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)kroku 14.

1. Aktuální zdroje IP adresu změňte možné vlastníka "Existující primární SQL Server", v následujícím příkladu, "dansqlams4":

    ![Appendix13][23]

1. Nový prostředek IP adresu Změna vlastníka možné "Migrated sekundární SQL Server", v následujícím příkladu, "dansqlams5":

    ![Appendix14][24]

1. Jakmile toto nastavení můžete převzetí a při migraci na poslední uzel možné vlastníky by měl být úpravou uzel se přidá ve formě vlastníkem:

    ![Appendix15][25]

## <a name="additional-resources"></a>Další zdroje informací
- [Azure Premium úložiště](../storage/storage-premium-storage.md)
- [Virtuálních počítačích](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
