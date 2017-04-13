<properties 
   pageTitle="Jak nastavit statické IP interní soukromé"
   description="Principy statické interní IP adresy (poklesu) a jak je lze spravovat"
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
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Jak nastavit statická interní soukromých IP adres pomocí prostředí PowerShell (klasický)
Ve většině případů nebudete muset zadejte statickou interní IP adresu počítače virtuální. VMs v síti virtuální se automaticky obnoví interní IP adresu z oblasti, které zadáte. Ale v některých případech určující statickou IP adresu pro konkrétní OM smysl. Například, pokud vaše OM přejdete na spustit DNS nebo budou řadiče domény. Statická interní IP adresa zůstane s OM i prostřednictvím stavu zastavit/identitách. 

> [AZURE.IMPORTANT] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce zdrojů a klasické](../resource-manager-deployment-model.md). Tento článek se věnuje pomocí klasické nasazení modelu. Microsoft doporučuje, že většina nových nasazení pomocí [Správce prostředků nasazení modelu](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Ověření, pokud je k dispozici zvláštní IP adresa
Abyste ověřili, pokud je k dispozici v vnet s názvem *TestVnet*IP adresu *10.0.0.7* , spusťte tento příkaz prostředí PowerShell a ověřte hodnotu pro *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Pokud chcete testovat příkazu nad v bezpečí prostředí postupujte podle pokynů v tématu [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-portal.md) vytvořit vnet s názvem *TestVnet* a zajistěte, aby že používala *10.0.0.0/8* adresní prostor.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Určení statické IP interní při vytváření virtuálního počítače
Skript Powershellu dole vytvoří nový Cloudová služba s názvem *TestService*, potom načte obrázek z Azure, pak vytvoří OM s názvem *TestVM* v nové cloudovou službu načtená obrázek, nastaví OM být podsítě s názvem *podsítě-1*a slouží k nastavení *10.0.0.7* jako statické IP interní OM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Jak získat interní IP statickým pro virtuálního počítače
Statická interní IP informace o OM vytvořená pomocí skriptu výše zobrazíte spuštěním následujícího příkazu Powershellu a sledovat hodnoty pro *adresa IP*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Jak odebrat statické IP interní z virtuálního počítače
Odebrání statické interní IP přidána do OM ve výše uvedené skriptu, spusťte tento příkaz Powershellu:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Jak přidat statické IP interní do existující OM
Chcete-li přidat vnitřní statickou IP bude v angličtině pomocí vytvořit skript nad o má následující příkaz:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Další kroky

[User](virtual-networks-reserved-public-ip.md)

[Úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
