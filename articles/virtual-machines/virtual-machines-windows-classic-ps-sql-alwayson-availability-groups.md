<properties
    pageTitle="Konfigurace vždy na skupiny dostupnosti v Azure OM pomocí prostředí PowerShell"
    description="Tento kurz využívá zdroje vytvořená pomocí klasické nasazení modelu a vytvoření vždy na dostupnost skupiny v Azure pomocí prostředí PowerShell."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfigurace vždy na skupiny dostupnosti v Azure OM pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Správce prostředků: šablony](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Správce prostředků: ruční](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasický: uživatelské rozhraní](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasický: prostředí PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtuálních počítačích (VMs) umožňuje správcům databáze implementovat nižší náklady dostupnost systému SQL Server. Tento kurz se dozvíte, jak implementovat dostupnost skupiny pomocí SQL Server vždy na začátku do konce uvnitř prostředí Azure. Na konci kurzu SQL Server vždy v řešení v Azure bude obsahovat tyto prvky:

- Virtuální sítě obsahující více podsítí, včetně front-end a back-end podsítě

- Řadiče domény s doménou Active Directory (AD)

- Dva SQL Server VMs používaný podsítě back-end a připojení k doméně AD

- Shluk WSFC 3 uzel s modelem kvora Většina uzlů

- Skupiny dostupnosti s dvěma replikami synchronní potvrdit dostupnost databáze

Tento scénář je vybrán pro jeho zjednodušení, ne pro účinnost náklady nebo jiných faktorů na Azure. Například můžete minimalizovat počet VMs pro skupinu dvě otevřené dostupnost za účelem uložení ve výpočetním hodiny v Azure pomocí řadiče domény jako kvora soubor kopií v clusteru WSFC 2 uzlů. Tento způsob snižuje počet OM pomocí jednoho z výše uvedených konfigurace.

Tento kurz se má zobrazit kroky potřebné k nastavení vypracování na podrobnosti každé krok popsané řešení nad. Proto místo vám ukážou, kroky konfigurace grafického rozhraní, použije prostředí PowerShell skriptování se můžete rychle jednotlivými kroky. Předpokládá takto:

- Už máte účet Azure k předplatnému virtuálního počítače.

- Jste nainstalovali [rutiny prostředí PowerShell Azure](../powershell-install-configure.md).

- Už máte dobré porozumění skupin vždy na dostupnost pro místní řešení. Další informace najdete v tématu [Vždy na dostupnost skupiny (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Připojení k předplatnému Azure a vytvořit virtuální sítě

1. V okně prostředí PowerShell ve vašem počítači importujte modul Azure publikování nastavení soubor stáhnout do počítače a relaci Powershellu se připojit k předplatnému Azure pomocí import stažených nastavení publikování.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Příkaz **Get-AzurePublishgSettingsFile** automaticky vygeneruje Správa certifikátu s Azure stáhne do počítače. V prohlížeči se automaticky otevře a zobrazí výzva k zadání přihlašovací údaje účtu Microsoft Azure předplatného. Stažený **.publishsettings** soubor obsahuje všechny informace, které budete potřebovat pro správu předplatného Azure. Po uložení souboru do místního adresáře, importujte pomocí příkazu **Importovat AzurePublishSettingsFile** .

    >[AZURE.NOTE] Soubor publishsettings obsahuje vaše přihlašovací údaje (nekódovaný), které slouží ke správě Azure předplatná a služeb. Doporučený postup zabezpečení pro tento soubor je uložte jej dočasně mimo zdrojové adresáře (například ve složce Libraries\Documents) a potom ho odstraňte po dokončení importu. Uživateli se zlými úmysly získá přístup k souboru publishsettings můžete upravit, vytvořit a odstranit služby Azure.

1. Definování řadu proměnné, které se používají k vytvoření vaše cloudové infrastruktury.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Věnujte pozornost následující zajistit, že bude vaše příkazy později úspěšné:

    - Proměnné **$storageAccountName** a **$dcServiceName** musí být jedinečné, protože se používají k identifikaci cloudové úložiště účet a cloudu serveru, respektive na Internetu.

    - Názvy určené pro proměnné **$affinityGroupName** a **$virtualNetworkName** nakonfigurovaná v dokumentu konfigurace virtuální sítě, který bude dál používat.

    - **$sqlImageName** název aktualizované OM obrázku, který obsahuje SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Pro zjednodušení **Contoso! 000** je použít v celém celý kurz stejné heslo.

1. Vytvoření skupiny spřažení.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Vytvořte virtuální sítě importu souboru konfigurace.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Konfigurační soubor obsahuje následující dokument ve formátu XML. Stručně řečeno Určuje virtuální sítě s názvem **ContosoNET** ve skupině spřažení s názvem **ContosoAG**a má místo adresu **10.10.0.0/16** a dvě podsítí **10.10.1.0/24** a **10.10.2.0/24**, které jsou přední podsítě a zpět podsítě, respektive. Přední podsítě je umístíte klientských aplikací, jako je Microsoft SharePoint, kde zpět podsítě je třeba bude umístit VMs SQL serveru. Pokud změníte proměnné **$affinityGroupName** a **$virtualNetworkName** dříve, musíte změnit také odpovídající názvy dole.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Vytvoření účtu úložiště, který bude přidružený skupině spřažení vytvořili a jeho nastavení jako aktuální úložiště účet předplatného.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Vytvoření serveru Datacentrum v novou sadu cloudové služby a dostupnosti.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Tuto řadu vytvoření kanálu příkazy provádět následující akce:

    - **Nový AzureVMConfig** vytvoří OM konfiguraci.

    - **Přidat AzureProvisioningConfig** vám bude radit parametry konfigurace systému Windows server samostatně.

    - **Přidat AzureDataDisk** přidá dat disku, který budete používat k ukládání dat služby Active Directory s ukládání do mezipaměti možnost nastavený na žádný.

    - **Nový AzureVM** vytvoří nový Cloudová služba a vytvoří nový OM Azure v nové cloudové služby.

1. Počkejte nové OM plně zřízení a vzdálené plochy budou moct soubor stáhnout do adresáře vaší práce. Protože nové OM Azure trvá dlouho trvat, při opakovat nadále hlasování nové OM, dokud je připraven k použití.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Teď je úspěšně zřízení Datacentrum serveru. Pak nakonfigurujete domény Active Directory na tomto serveru řadiče domény. Ponechání otevřeného okna prostředí PowerShell ve vašem počítači. Ho později použijete k vytvoření dvou VMs SQL serveru.

## <a name="configure-the-domain-controller"></a>Konfigurace řadiče domény

1. Připojení k serveru Datacentrum spuštěním souboru vzdálené plochy. Použití AzureAdmin uživatelského jména a hesla správce počítače **Contoso! 000**, které jste zadali při vytváření nové OM.

1. Otevřete okno Powershellu v režimu správce.

1. Spusťte následující **DCPROMO. EXE** příkaz nastavit **corp.contoso.com** domény s adresářů dat v jednotce M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Po dokončení příkazu OM restartuje automaticky.

1. Znovu připojte k serveru Datacentrum spuštěním souboru vzdálené plochy. Tentokrát, přihlaste se jako **CORP\Administrator**.

1. Otevřete okno Powershellu v režimu správce a import modul Active Directory PowerShell tento příkaz:

        Import-Module ActiveDirectory

1. Spusťte následující příkazy pro přidání tří uživatelů na doménu.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** se používá ke konfiguraci všechno související s instance služby SQL Server, WSFC obrázku a skupiny dostupnosti. **CORP\SQLSvc1** a **CORP\SQLSvc2** slouží jako účty služby SQL Server pro dva VMs SQL serveru.

1. Další spusťte následující příkazy a udělte **CORP\Install** oprávnění k vytvoření objekty počítače v doméně.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Identifikátor GUID výše uvedené je identifikátor GUID pro typ objektu počítače. Účet **CORP\Install** vyžaduje oprávnění **Ke čtení všech vlastností** a **Vytváření objektů počítače** k vytvoření aktivní přímé objekty, abyste WSFC obrázku. Oprávnění ke **Čtení všech vlastností** už zůstane CORP\Install ve výchozím nastavení, takže není potřeba udělit explicitně. Další informace týkající se oprávnění potřebné k vytvoření obrázku WSFC najdete v tématu [překlopení obrázku podrobného průvodce: Konfigurace účtů ve službě Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Teď dokončení konfigurace služby Active Directory a objektů uživatele vytvoříte dva VMs SQL serveru a připojte k této domény.

## <a name="create-the-sql-server-vms"></a>Vytvoření VMs serveru SQL

1. Dál pomocí okna prostředí PowerShell, který je otevřen v místním počítači. Definování následující další proměnné:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP adresy **10.10.0.4** je obvykle přiřazen k první OM vytvoříte v podsítě **10.10.0.0/16** Azure virtuální sítě. Ověřte, zda se že jedná adresu konfiguračního serveru vašeho Datacentrum spuštěním **IPCONFIG**.

1. Spusťte následující vytvoření kanálu příkazy k vytvoření prvního OM clusteru WSFC s názvem **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Poznámka týkající se příkaz nahoře:

    - **Nový AzureVMConfig** vytvoří konfiguraci OM s názvem nastavení požadovaného dostupnosti. Pozdější VMs se vytvoří se stejným názvem dostupnost nastavit tak, že jsou spojeny do stejné sady dostupnosti.

    - **Přidat AzureProvisioningConfig** spojí OM do domény Active Directory, kterou jste vytvořili.

    - **Nastavení AzureSubnet** umístí OM podsítě zpět.

    - **Nový AzureVM** vytvoří nový Cloudová služba a vytvoří nový OM Azure v nové cloudové služby. Parametr **DnsSettings** Určuje, že server DNS servery v nové cloudové službě má IP adresu **10.10.0.4**, což je IP adresa serveru řadiče domény. Tento parametr není potřeba povolit nové VMs ve službě cloud ke spojení a domény Active Directory úspěšně. Bez tento parametr musí ručně nastavit nastavení IPv4 OM serverem Datacentrum jako primární server DNS po zřízení OM a připojte OM do domény Active Directory.

1. Spusťte následující vytvoření kanálu příkazy k vytvoření VMs SQL serveru, s názvem **ContosoSQL1** a **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Poznámka týkající se výše příkazy:

    - **Nový AzureVMConfig** používá stejný název sady dostupnost jako Datacentrum server a používá obrázky SQL Server 2012 Service Pack 1 Enterprise Edition v galerii virtuálního počítače. Také nastaví disk operačního systému čtení-pouze ukládání do mezipaměti (bez ukládání do mezipaměti). Je vhodné migrace souborů databáze na samostatný datový disk připojit OM a nakonfigurovat ke čtení nebo ukládání do mezipaměti. Skoro je ale odebrat ukládání do mezipaměti na disk operačního systému, protože nemůžete odebrat přečtená ukládání do mezipaměti na disk operačního systému.

    - **Přidat AzureProvisioningConfig** spojí OM do domény Active Directory, kterou jste vytvořili.

    - **Nastavení AzureSubnet** umístí OM podsítě zpět.

    - **Přidat AzureEndpoint** přidá koncové body přístup tak, aby klientské aplikace dostanete tyto instance služby SQL Server na Internetu. Různé porty jsou uvedeny ContosoSQL1 a ContosoSQL2.

    - **Nový AzureVM** vytvoří nový OM SQL Server ve službě cloud jako ContosoQuorum. Pokud budete chtít, aby se stejná skupina dostupnost, je nutné umístit VMs ve stejném cloudové služby.

1. Počkejte každý OM na plně zřízenou a stažení souboru jeho vzdálené plochy do adresáře vaší práce. Obraze cyklicky tři nové VMs a provede příkazy uvnitř nejvyšší úrovně složených závorek pro každý z nich.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL Server VMs jsou teď zřízenou a spuštění, ale instalují se serverem SQL Server s výchozími možnostmi.

## <a name="initialize-the-wsfc-cluster-vms"></a>Inicializace VMs WSFC obrázku

V této části budete muset změnit tři servery, které budete používat v clusteru WSFC a instalace serveru SQL Server. Konkrétně:

- (Všech serverech) Budete potřebovat k instalaci **Clusterů převzetí** funkce.

- (Všech serverech) Budete muset přidat **CORP\Install** jako počítače **Správce**.

- (ContosoSQL1 a ContosoSQL2 pouze) Budete muset přidat **CORP\Install** jako **členem** role ve výchozím nastavení databáze.

- (ContosoSQL1 a ContosoSQL2 pouze) Budete muset přidat **NT AUTHORITY\System** jako přihlášeni pomocí těchto oprávnění:

    - Změnit všechny skupiny dostupnosti

    - Připojení SQL

    - Zobrazení stavu serveru

- (ContosoSQL1 a ContosoSQL2 pouze) Protokolu **TCP** se už povolili na OM SQL serveru. Ale přesto musíte otevřít brány firewall pro vzdálený přístup k serveru SQL Server.

Teď jste připraveni začít. Začínající **ContosoQuorum**, postupujte následujícím způsobem:

1. Připojení k **ContosoQuorum** spuštěním soubory vzdálené plochy. Použití **AzureAdmin** uživatelského jména a hesla správce počítače **Contoso! 000**, které jste zadali při vytváření VMs.

1. Ověřte, že počítače byl úspěšně připojen k **corp.contoso.com**.

1. Počkejte na dokončení spuštění automatické inicializace úkoly před pokračováním instalace SQL serveru.

1. Otevřete okno Powershellu v režimu správce.

1. Nainstalujte funkci clusterů selhání systému Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Přidání **CORP\Install** jako místní správce.

        net localgroup administrators "CORP\Install" /Add

1. Odhlaste se od ContosoQuorum. Dokončení práce s Tento server nyní.

        logoff.exe

Inicializace pak **ContosoSQL1** a **ContosoSQL2**. Postupujte podle pokynů, které jsou stejné obou VMs SQL serveru.

1. Připojení k dvou VMs SQL Server spuštěním soubory vzdálené plochy. Použití **AzureAdmin** uživatelského jména a hesla správce počítače **Contoso! 000**, které jste zadali při vytváření VMs.

1. Ověřte, že počítače byl úspěšně připojen k **corp.contoso.com**.

1. Počkejte na dokončení spuštění automatické inicializace úkoly před pokračováním instalace SQL serveru.

1. Otevřete okno Powershellu v režimu správce.

1. Nainstalujte funkci clusterů selhání systému Windows.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Přidání **CORP\Install** jako místní správce

        net localgroup administrators "CORP\Install" /Add

1. Importujte zprostředkovatele prostředí PowerShell SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Přidejte **CORP\Install** jako členem role pro výchozí instanci systému SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Přidání **NT AUTHORITY\System** jako přihlášení se tři oprávnění jsme je popsali výše.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Otevřete brány firewall pro vzdálený přístup k serveru SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Odhlášení z obou VMs.

        logoff.exe

Nakonec jste připraveni Konfigurace dostupnosti skupiny. K provedení všech práce na **ContosoSQL1**bude použijte zprostředkovatele Powershellu SQL Server.

## <a name="configure-the-availability-group"></a>Konfigurace dostupnosti skupiny

1. Znovu připojte k **ContosoSQL1** spuštěním soubory vzdálené plochy. Místo přihlášení pomocí účtu počítače, přihlaste se pomocí **CORP\Install**.

1. Otevřete okno Powershellu v režimu správce.

1. Definování následující proměnné:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Import zprostředkovatele prostředí PowerShell SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Změna účtu služby SQL Server pro ContosoSQL1 CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Změna účtu služby SQL Server pro ContosoSQL2 CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Stáhněte si **CreateAzureFailoverCluster.ps1** z [Vytvořit WSFC obrázku pro vždy na dostupnost skupiny v Azure OM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) místního adresáře práce. Pomocí tohoto skriptu se vám pomůže s vytvářením funkční WSFC obrázku. Důležité informace o interakci WSFC s Azure sítě najdete v článku [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).

1. Přejděte do adresáře vaší pracovní a vytvořte clusteru WSFC se stažený skriptem.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Povolte vždy na skupiny dostupnost pro výchozí instance serveru SQL Server na **ContosoSQL1** a **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Vytvořte záložní adresář a udělení oprávnění pro účty služby SQL Server. Příprava databáze dostupnost na vedlejší otevřené použijete tento adresář.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Vytvoření databáze na **ContosoSQL1** s názvem **MyDB1**, prohlédněte úplné zálohování a zálohu protokolu a jejich obnovení na **ContosoSQL2** s parametrem **S NORECOVERY** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Dostupnost koncové body skupiny na vytvořit VMs SQL serveru a nastavit příslušná oprávnění pro koncové body.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Vytvoření repliky dostupnosti.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Nakonec vytvořit skupinu dostupnost a připojení ke skupině dostupnost sekundární otevřené.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Další kroky
Jste teď úspěšně implementovali SQL Server vždy na vytvořením skupiny dostupnosti v Azure. Abyste mohli nakonfigurovat posluchače pro tuto skupinu dostupnost, přečtěte si téma [Konfigurace posluchače ILB skupin vždy na dostupnost v Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Další informace o použití serveru SQL Server Azure najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
