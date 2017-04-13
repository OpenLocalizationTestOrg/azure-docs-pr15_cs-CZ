<properties
   pageTitle="Vyhrazená IP | Microsoft Azure"
   description="Princip rezervovaná IP adresy a jak je lze spravovat"
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

# <a name="reserved-ip-overview"></a>Přehled User
IP adresy v Azure spadají do dvou kategorií: dynamické a rezervovaná. Dynamické ve výchozím nastavení nejsou veřejnou IP adresy spravuje Azure. Která znamená, že na IP adresu použít pro dané Cloudová služba (VIP) nebo pro přístup k roli nebo OM instanci přímo (ILPIP) můžete změnit se občas, když zdroje jsou vypnutí nebo odebrána.

Zabránit Změna adresy IP, můžete rezervovat k IP adrese. Rezervovaná IP adresy lze použít pouze jako VIP zajišťující, že IP adresa cloudovou službu bude stejná i při vypnutí nebo zdrojů odebrána. Kromě toho můžete převést existující dynamické IP adresy použité jako VIP rezervovaná IP adresu.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Naučte se rezervovat statické veřejné adresy IP pomocí [Správce prostředků nasazení modelu](virtual-network-ip-addresses-overview-arm.md).

Zkontrolujte, jestli že víte, jak fungují [IP adres](virtual-network-ip-addresses-overview-classic.md) v Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Kdy je potřeba rezervovaná IP?
- **Chcete-li zajistit, aby ve vašem předplatném je rezervován IP**. Pokud chcete rezervovat IP adresu, která neuvolní ze svého předplatného klikněte v části jakékoli okolnosti, používejte rezervovaná veřejnou IP.  
- **Chcete svoji IP dodržovat předpisy s cloudovou službou i přes přestal nebo zrušeny, takže stavu (VMs)**. Pokud chcete služby přístupný pomocí IP adresu, která se nezmění, i když VMs ve službě cloud jsou zastavit nebo zrušeny, takže.
- **Chcete-li zajistit, aby používala odchozí přenosy z Azure předvídatelná IP adresu**. Můžete mít vaše blokuje místní brána nakonfigurované tak, aby přenosy jenom z konkrétní IP adres. Podle rezervaci IP, budou vědět Zdrojová IP adresa a nebudou mít k aktualizaci brány firewall pravidel kvůli změně IP.

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
1. Můžete použít vyhrazené IP pro všechny služby Azure?  
  - Rezervovaná IP adresy lze použít pouze pro VMs a vystaven prostřednictvím VIP role instanci služby cloudu.
1. Kolik rezervovaná IP adresy můžou mít?  
  - V současné době Všechna předplatná Azure oprávnění používat 20 rezervovaná IP adresy. Však můžete požádat o další rezervovaná IP adresy. Najdete na stránce [předplatné a omezení služeb](../azure-subscription-service-limits.md) Další informace.
1. Je pro rezervovaná IP adresy náklady?
  - Naleznete v tématu [Rezervovaná IP adresu ceny podrobnosti o](http://go.microsoft.com/fwlink/?LinkID=398482) cenách informace.
1. Jak rezervovat IP adresu?
  - Pomocí prostředí PowerShell nebo [Azure správy REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) můžete rezervovat IP adresy v určité oblasti. Tato rezervovaná IP adresa je přidružený k předplatnému. IP adresy nelze rezervovat pomocí portálu pro správu.
1. Můžu používat to skupinou spřažení na základě VNets?
  - Místní VNets podporuje pouze rezervovaná IP adresy. Není podporovaná pro VNets, které jsou přidružené k spřažení skupiny. Další informace o přiřazení VNet s oblasti nebo skupinu spřažení najdete v článku [o místní VNets a spřažení skupiny](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Jak spravovat rezervovaná virtuální

Než budete moct použít vyhrazené IP adresy, musíte ho ke svému předplatnému přidat. Pokud chcete vytvořit rezervovaná IP z fondu veřejných adres IP k dispozici v *USA centrální* umístění, spusťte tento příkaz Powershellu:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Všimněte si, ale není možné zadat co IP probíhá vyhrazená. K čemu IP adresy rezervováno ve vašem předplatném zobrazíte spuštěním následujícího příkazu Powershellu a Všimněte si hodnoty *ReservedIPName* a *adresu*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Jakmile je rezervován IP, zůstane přidružený k předplatnému, dokud odstraňte ji. Odstranit rezervovaná IP nahoře, spusťte tento příkaz Powershellu:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Rezervování IP adresu existující cloudové služby

Rezervovat IP adresu existující cloudové služby přidáním parametr *- ServiceName* . Rezervujte IP adresu do cloudové služby *TestService* v *USA centrální* umístění, spusťte tento příkaz Powershellu:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Jak můžete přidružit rezervovaná IP nové cloudové služby
Skript dole vytvoří nový rezervovaná IP a potom přidruží jej do nového cloudové služby s názvem *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Když vytvoříte rezervovaná IP pomocí služby do cloudové služby, přesto musíte odkázat OM pomocí *VIP:&lt;číslo portu >* pro příchozí komunikaci. Rezervace IP neznamená, že se můžete připojit k OM přímo. Rezervovaná IP se přiřadí ke cloudové služby, které byl používaný OM. Pokud chcete připojit přímo k OM podle IP, budete muset konfigurace veřejnou IP úrovni instance. Veřejné IP úrovni instance je typ veřejné adresy IP (jen ILPIP) přiřazené přímo do vaší OM. Nelze rezervovat. Další informace najdete v tématu [Úrovni Instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Jak odebrat rezervovaná IP z pracovního nasazení
Odebrat rezervovaná IP přidané do nové služby vytvořené ve výše uvedené skriptu, spusťte tento příkaz Powershellu:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Odebrání pracovního nasazení rezervovaná IP neodebírat rezervaci ze svého předplatného. Uvolnění jednoduše IP pro použití v jiné zdroje předplatné.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Jak můžete přidružit rezervovaná IP pracovního nasazení
Skript dole vytvoří nový Cloudová služba s názvem *TestService2* s novou OM s názvem *TestVM2*a potom Přidruží existující rezervovaná IP s názvem *MyReservedIP* cloudové služby.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Jak můžete přidružit rezervovaná IP do cloudové služby pomocí souboru konfigurační služby
Můžete taky přiřadíte rezervovaná IP do cloudové služby pomocí souboru konfigurace (CSCFG) služby. Následující ukázkové xml ukazuje, jak nakonfigurovat do cloudové služby používat rezervovaná VIP s názvem *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Další kroky

- Pochopit, jak [IP adres](virtual-network-ip-addresses-overview-classic.md) funguje v modelu klasické nasazení.

- Informace o [Rezervovaná soukromé IP adres](virtual-networks-reserved-private-ip.md).

- Další informace o [adresách Instance úroveň veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md).
