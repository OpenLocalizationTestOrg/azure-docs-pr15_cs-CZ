<properties 
   pageTitle="Jak migrovat ze skupin spřažení do místního virtuální sítě (VNet)"
   description="Zjistěte, jak migrovat z spřažení skupiny do místního vnets"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Jak migrovat ze skupin spřažení do místního virtuální sítě (VNet)

Skupinu spřažení umožňuje zajistit, že zdroje vytvořené ve stejné skupině spřažení fyzicky hostovaných servery, které jsou těsně, povolení tyto materiály ke komunikaci zrychlit. V minulosti byly spřažení skupiny povinné pro vytváření virtuálních sítí (VNets). V té době může služba správce sítě, která spravuje VNets funkční pouze v rámci sady fyzické servery nebo jednotku měřítko. Architektonické vylepšení zvýšily obor Správa sítě do oblasti.

Důsledku tyto architektonické vylepšení spřažení skupiny jsou už doporučené nebo povinné virtuálních sítí. Použití spřažení skupin pro VNets je nahrazen oblastí. VNets, které jsou přidružené k oblasti se označují jako místní VNets.

Doporučujeme kromě toho, že skupin spřažení nepoužíváte Obecné. Kromě požadovaného VNet spřažení skupiny důležitosti taky můžete zajistit, aby byly zdroje, jako je třeba výpočetním a úložiště, umístěny vedle sebe. S aktuální Azure síťové architektury tyto požadavky umístění jsou však už není potřeba. Několik zbývající určitých případech, kde je vhodné použít skupinu spřažení naleznete v tématu [spřažení skupiny a VMs](#Affinity-groups-and-VMs) .

## <a name="creating-and-migrating-to-regional-vnets"></a>Vytvoření a migrace pro místní VNets

Chvíle, při vytváření nové VNets, použijte *oblast*. Zobrazí se tato jako jednu z možností v portálu pro správu. Všimněte si, že v souboru konfigurace sítě se zobrazí jako *umístění*.

>[AZURE.IMPORTANT] Ačkoli je pořád doslova lze vytvořit virtuální sítě, které je přidružené ke skupině spřažení, není atraktivní důvod k tomu nevyzve. Mnoho nových funkcí, například skupiny zabezpečení sítě jsou dostupné, jenom Pokud používáte místní VNet a nejsou k dispozici pro virtuální sítě, které jsou přidružené k spřažení skupiny.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Informace o VNets přidružené k spřažení skupiny

VNets přidružené aktuálně spřažení skupiny jsou povoleny pro migraci do místního VNets. Migrace do místního VNet, postupujte takto:

1. Exportujte do souboru konfigurace sítě. Můžete pomocí prostředí PowerShell nebo portálu pro správu. Na portálu Správa najdete v článku [konfigurace vaší VNet pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md).

1. Upravte soubor konfigurace sítě, nahraďte původní hodnoty nové hodnoty. 

    > [AZURE.NOTE] **Umístění** je oblast, kterou jste zadali spřažení skupiny, který je přidružený k vaší VNet. Například pokud vaše VNet je přidružené ke skupině spřažení, který je umístěn v západní USA při migraci, vaší polohy musí odkazovat západní USA. 
    
    Upravte následující řádky v souboru konfigurace sítě nahradit hodnoty vlastní: 

    **Stará hodnota:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Nová hodnota:** \<VirtualNetworkSitename = "VNetUSWest" umístění = "Západ USA"\>

1. Uložte změny a [importovat](virtual-networks-using-network-configuration-file.md) konfiguraci sítě Azure.

>[AZURE.NOTE] Tento migrace nezpůsobí všechny prostoj svých služeb.

## <a name="affinity-groups-and-vms"></a>Spřažení skupiny a VMs

Jak jsme zmínili dříve, spřažení skupiny jsou vám doporučené už obecně VMs. Používejte pro skupinu spřažení pouze sadu VMs musí být absolutní nejnižší sítě latenci mezi VMs. VMs ve skupině spřažení tak, že umístíte VMs budou všechny umístěny ve stejné výpočetním clusteru nebo měřítko jednotky.

Je důležité mít na paměti, že pomocí spřažení skupiny můžete mít dva, případně záporné číslo, důsledky:

- Sadě velikostí OM bude omezena na sadě velikostí OM nabízená jednotku výpočetním měřítko.

- Je vyšší pravděpodobnost nebude možné přidělit nové OM. Tím se stane, když jednotku konkrétní měřítko pro skupinu spřažení je mimo kapacity.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Co dělat, když máte virtuálního počítače ve skupině spřažení

VMs, které jsou v současnosti ve skupině spřažení nemusí být odebrat ze skupiny spřažení.

Po zavedení virtuálního počítače je používaný výběru jednoho měřítko. Spřažení skupiny můžou omezit sadě velikostí dostupné OM nové nasazení OM, ale už je omezen na sadě velikostí OM k dispozici v jednotce měřítko, ve kterém je nasazený OM existující OM, který nasazení. Z toho důvodu odebrání virtuálního počítače ze skupiny spřažení bude mít žádný vliv.
 
