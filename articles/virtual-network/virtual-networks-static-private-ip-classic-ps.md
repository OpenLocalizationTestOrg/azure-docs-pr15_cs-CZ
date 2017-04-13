<properties 
   pageTitle="Jak nastavit statické IP soukromé klasický režim pomocí prostředí PowerShell | Microsoft Azure"
   description="Principy statické soukromé IP adresy (poklesu) a jak mají ovládat v klasického režimu a prostředí PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Jak nastavit statická soukromé IP adresu (klasické) v prostředí PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete taky [Spravovat statické soukromé IP adresu v modelu nasazení Správce prostředků](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ukázkové příkazy Powershellu dole očekávat jednoduché prostředí vytvořen. Pokud chcete spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovacím prostředí popsaná v tématu [Vytvoření VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Ověření, pokud je k dispozici zvláštní IP adresa
Abyste ověřili, pokud je k dispozici v VNet s názvem *TestVNet*IP adresu *192.168.1.101* , spusťte tento příkaz prostředí PowerShell a ověřte hodnotu pro *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Očekávaný výstup:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Určení statické IP adresu soukromé při vytváření virtuálního počítače
Skript Powershellu dole vytvoří nový Cloudová služba s názvem *TestService*, a pak načte obrázek z Azure, vytvoří OM s názvem *DNS01* v nové cloudovou službu načtená obrázek, nastaví OM být podsítě s názvem *FrontEnd*a sady *192.168.1.7* jako statické soukromých IP adres pro OM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Očekávaný výstup:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak získat statické soukromé informace o IP adrese pro virtuálního počítače
Statická soukromé informace o IP adresu pro OM vytvořená pomocí skriptu výše uvedených zobrazíte spuštěním následujícího příkazu Powershellu a sledovat hodnoty pro *adresa IP*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Očekávaný výstup:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak odebrat soukromé statickou IP adresu z virtuálního počítače
Chcete-li odebrat soukromé statickou IP adresu přidají do OM ve výše uvedená skriptu spuštěním následujícího příkazu Powershellu:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Očekávaný výstup:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak přidat statické soukromé IP adresu do existující OM
Chcete-li přidat statické soukromé IP adresu OM vytvořené pomocí skriptu nad o má následující příkaz:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Očekávaný výstup:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Další kroky

- Další informace o [Rezervovaná veřejnou IP](virtual-networks-reserved-public-ip.md) adresách.
- Další informace o adresách [úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Použijte [User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
