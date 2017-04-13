<properties 
   pageTitle="Jak přesunout instanci OM nebo role jiné podsítě"
   description="Naučte se přesouvat VMs a rolí instance jiné podsítě"
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

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Jak přesunout instanci OM nebo role jiné podsítě

Prostředí PowerShell slouží k přesunutí vaší VMs z jedné podsítě na druhý ve stejné síti virtuální (VNet). Role instance přesouvat úpravy CSCFG, spíše než pomocí Powershellu.

>[AZURE.NOTE] Tento článek obsahuje informace, které jsou relativní Azure pouze klasické nasazení.

Proč VMs přesunout do jiné podsítě? Migrace podsítě je užitečná, když starší podsítě je moc malý a nejde rozbalené z důvodu existujících pracovního VMs v této podsítě. V takovém případě můžete vytvořit nové, větší podsítě a migrace VMs do nového podsítě a po dokončení migrace odstraníte původní prázdné podsítě.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Jak přesunout virtuálního počítače do jiného podsítě

Přesunout OM, spouštět rutiny prostředí PowerShell Set-AzureSubnet v příkladu níže jako šablonu. V následujícím příkladu jsme přesouváte TestVM z jeho prezentovat podsítě podsítě-2. Ujistěte se, chcete-li upravit příklad tak, aby odrážely prostředí. Všimněte si, že při každém spuštění rutinu aktualizace AzureVM jako součást postupu restartuje vaší OM jako součást procesu aktualizace.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Pokud jste zadali statické IP interní soukromé pro vaše OM, budete muset před OM můžete přesunout do nového podsítě zrušte toto nastavení. V takovém případě následujícím způsobem:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Chcete-li přesunout instanci rolí do jiné podsítě

Přesunout instanci rolí, upravte soubor CSCFG. V následujícím příkladu jsme přesouváte "Role0" v virtuální sítě *VNETName* z prezentace podsítě *podsítě -*2. Protože nasazené už instanci rolí, budete jednoduše změňte název podsítě = podsítě-2. Ujistěte se, chcete-li upravit příklad tak, aby odrážely prostředí.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
