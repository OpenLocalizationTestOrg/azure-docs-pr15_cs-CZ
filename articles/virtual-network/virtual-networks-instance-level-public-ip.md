<properties 
   pageTitle="Instance úroveň veřejnou IP (ILPIP) | Microsoft Azure"
   description="Principy ILPIP (PIP) a jak je lze spravovat"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Přehled IP úrovně veřejné instanci
Úrovně veřejnou IP (ILPIP) je instance veřejnou IP adresy, kterou můžete přiřadit přímo k instanci aplikace OM nebo roli a ne cloudové služby, který OM nebo role instance uložena v. Toto není nahrazovat VIP (virtuální IP) přiřazeného ke cloudové službě. Místo toho je další IP adresu, která umožňuje připojit přímo k instanci OM nebo roli.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](virtual-network-ip-addresses-overview-arm.md). 

Zkontrolujte, jestli že víte, jak fungují [IP adres](virtual-network-ip-addresses-overview-classic.md) v Azure.

>[AZURE.NOTE] V minulosti byla ILPIP označovány jako PIP, který zastupuje veřejnou IP. 

![Rozdíl mezi ILPIP a VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Jak vidíte na obrázku 1, cloudové služby je získat přístup pomocí VIP, zatímco jednotlivé VMs jsou obvykle otevřít prostřednictvím virtuální:&lt;číslo portu&gt;. Přiřazením ILPIP konkrétní OM, zda OM přístupná přímo pomocí této adresy IP.

Při vytváření do cloudové služby v Azure odpovídající záznamy DNS A jsou vytvářeny automaticky povolit přístup ke službě prostřednictvím plně kvalifikovaný název domény (FQDN) místo použití skutečné VIP. Stejný postup pro ILPIP, který umožňuje přístup k instanci OM nebo role tak, že plně kvalifikovaný název domény místo ILPIP neděje Pokud vytvoříte do cloudové služby s názvem *contosoadservice*a konfigurace roli webu s názvem *contosoweb* s dvě instance, například bude Azure registrovat následující záznamy o pro instance:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Jedinou ILPIP pro každou roli nebo OM instanci můžete přiřadit. Můžete používat až na 5 ILPIP na jedno předplatné. V současné době není podporována ILPIP pro více NIC VMs.

## <a name="why-should-i-request-an-ilpip"></a>Proč by měl žádat ILPIP?
Pokud budete chtít se připojit k instanci aplikace OM nebo role IP adresu přiřazenou přímo, ne pomocí cloudu služby VIP:&lt;číslo portu&gt;, požádat o ILPIP pro vaše OM nebo instanci rolí.
- **Trpný FTP** – tím, že ILPIP na OM, můžete přijímat přenosy na téměř jakéhokoli portu, nebudete muset otevřít koncový bod pro příjem vysílání. Díky scénáře jako pasivní FTP, kde jsou porty dynamicky zvolené.
- **Odchozích IP** - odchozí přenosy pocházející z OM patří ILPIP jako zdroj a to jednoznačně identifikuje OM na externí entity.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Jak požádat o ILPIP při vytváření OM
Skript Powershellu dole vytvoří nový Cloudová služba s názvem *FTPService*, a pak načte obrázek z Azure, vytvoří OM s názvem *FTPInstance* načtená obrázek, nastaví OM používat ILPIP a přidá OM do nové služby:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Jak získat ILPIP informace pro virtuálního počítače
Informace o ILPIP OM vytvořená pomocí skriptu výše zobrazíte spuštěním následujícího příkazu Powershellu a sledovat hodnoty pro *PublicIPAddress* a *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Jak odebrat ILPIP z virtuálního počítače
Odebrat ILPIP přidána do OM ve výše uvedené skriptu, spusťte tento příkaz Powershellu:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Přidání ILPIP do existující OM
Pokud chcete přidat ILPIP OM vytvořené pomocí skriptu pro výše uvedené, spusťte tento příkaz:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Jak můžete přidružit ILPIP do virtuálního počítače pomocí konfiguračního souboru služby
Můžete taky přiřadíte ILPIP do virtuálního počítače pomocí souboru konfigurace (CSCFG) služby. Následující ukázkové xml ukazuje, jak nakonfigurovat do cloudové služby používat ILPIP s názvem *MyPublicIP* role instance: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Další kroky

- Pochopit, jak [IP adres](virtual-network-ip-addresses-overview-classic.md) funguje v modelu klasické nasazení.

- Informace o [vyhrazené IP adresy](virtual-networks-reserved-public-ip.md).
 