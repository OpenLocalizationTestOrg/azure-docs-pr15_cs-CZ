<properties
  pageTitle="Připojení k vlastní doménu do cloudové služby | Microsoft Azure"
  description="Zjistěte, jak připojit webové či pracovníka role do vlastní AD domény pomocí prostředí PowerShell a přípona AD domény"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Připojení rolí služby Azure cloudu vlastní AD domény řadiče domény hostované v Azure

V Azure jsme bude nejdřív nastavit virtuální sítě (VNet). Potom přidáme Active Directory řadiče domény (hostované na Azure virtuální počítač) do VNet. Dále jsme přidáte role stávajících služby cloudu předem vytvořené VNet a následně připojení k řadiče domény.

Před můžeme začít pracovat, pár věcí, které mějte na paměti:

1.  Tento kurz pomocí prostředí PowerShell, proto je nutné nastavit, že máte nainstalovaný prostředí PowerShell Azure a připravená k odeslání. Potřebujete pomoct s nastavení prostředí PowerShell Azure, zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).

2.  AD domény řadiče domény a Web/pracovníka roli instance musí být v VNet.

Postupujte podle podrobných pokynů a pokud narazíte na problémy, napište nám komentář dole. Někdo se vrátit k můžete (Ano, jsme čtení komentářů).

3. Sítě, ke které odkazují cloudové služby <mark>, musíte být</mark> **klasické virtuální sítě**.

## <a name="create-a-virtual-network"></a>Vytvořit virtuální sítě

Virtuální sítě můžete vytvořit v Azure pomocí Azure klasické portál nebo Powershellu. Pro účely tohoto návodu použijeme Powershellu. Pokud chcete vytvořit virtuální síť na portálu Azure klasické, najdete v článku [Vytvořit virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Jakmile jste dokončili nastavení virtuální sítě, je potřeba vytvořit řadiče domény AD. Pro účely tohoto návodu jsme budou nastavovat AD domény řadiče na Azure virtuálního počítače.

K tomuto účelu vytvoření virtuálního počítače pomocí prostředí PowerShell pomocí následujících příkazů:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Zvýšení úrovně virtuálního počítače do jiného řadiče domény
Konfigurace virtuálního počítače jako řadiče domény AD, musíte se a přihlaste se k OM ji nakonfigurovat.

K přihlášení do OM, můžete získat soubor RDP prostřednictvím Powershellu, použijte následující příkazy.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Jakmile jste přihlášeni OM, instalační program virtuálního počítače jako řadiče domény AD pomocí následujících podrobného průvodce o [tom, jak nastavit zákazníkovi AD domény řadiče domény](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Přidání cloudové služby virtuální sítě

Pak budete muset přidat nasazení služby cloudu VNet jste právě vytvořili. K tomuto účelu upravte vaše cloudové služby cscfg přidáním příslušných částí do svého cscfg pomocí aplikace Visual Studio nebo editoru podle svého výběru.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Další Vytvoření projektu cloud services a nasazení do Azure. Potřebujete pomoct s nasazováním ve vašem balení cloudové služby Azure, přečtěte si, [jak vytvořit a nasazení do cloudové služby](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Připojení webové či pracovníka role do domény

Po nasazení projektu cloudové služby na Azure připojte role instance na vlastní doménu AD s příponou AD domény. Přidání domény služby AD rozšíření do stávajícího nasazení služby cloudu a připojovat se vlastní doménu, provést následující příkazy v prostředí PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

A to je vše.

Cloudové služby by měl být se připojili k vaší vlastní domény řadiče domény. Pokud chcete další informace o různých možností pro konfiguraci přípona AD domény, použijte nápovědu prostředí PowerShell jak je ukázáno v následujícím příkladu.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
