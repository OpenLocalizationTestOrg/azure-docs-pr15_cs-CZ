<properties 
   pageTitle="Rozhraní sítě | Microsoft Azure"
   description="Informace o rozhraní Azure sítě v Azure správce."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Rozhraní sítě

Síťové rozhraní (NIC) je propojení mezi virtuálního virtuálního počítače (počítače) a základní sítě software. Tento článek vysvětluje síťové rozhraní je a jak se používá v modelu nasazení Správce prostředků Azure.

Microsoft doporučuje nasazení nových zdrojů pomocí Správce prostředků nasazení modelu, ale taky nasazením VMs s připojení k síti v [klasické](virtual-network-ip-addresses-overview-classic.md) nasazení modelu. Pokud znáte klasické modelu, je důležité rozdíly ve OM sítě v modelu nasazení Správce prostředků. Další informace o rozdílech v článku [virtuálního počítače sítě – klasické](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) .

V Azure, rozhraní sítě:

1. Prostředek, který můžete vytvoření, odstranění a konfigurovat nastavení.
2. Musí být připojená ke jeden podsítě v jedné Azure virtuální síti (VNet) při vytvoření. Pokud znáte není VNets, další informace o nich v článku [Přehled virtuální sítě](virtual-networks-overview.md) . NIC musí být připojená ke VNet rozlohu ve stejném Azure [umístění](https://azure.microsoft.com/regions) a [předplatné](../azure-glossary-cloud-terminology.md#subscription) jako síťovou Po vytvoření NIC můžete změnit podsítě, ke kterému je připojen k, ale není možné změnit VNet je připojené k.
3. Je název mu přiřazeno, které nelze změnit po vytvoření NIC. Název musí být jedinečný v rámci Azure [pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md#resource-groups), ale nemusí být v rámci předplatného, Azure umístění, do kterého už je vytvořená v nebo VNet je připojené k jedinečný. Několik nic se obvykle vytvářejí Azure předplatného. Je vhodné navrhnout pojmenování, díky kterému Správa více nic snadnější než používat výchozí názvy. Naleznete v článku [Doporučené konvence pro Azure zdroje](../guidance/guidance-naming-conventions.md) pro návrhy.
4. Může být připojen virtuálního počítače, ale může být připojen pouze jeden OM rozlohu ve stejném umístění jako síťovou
5. Má adresu MAC, která je zachován s NIC pro zůstává připojené k virtuálního počítače. Adresa MAC je zachován, zda se nerestartuje OM (z v operačním systému) nebo ukončit (zrušen) a začít používat portál Azure, Azure PowerShell nebo rozhraní Azure příkazového řádku. Pokud se odpojit od virtuálního počítače a připojené k jiné OM, obdrží NIC jiná adresa MAC. Pokud NIC odstranili, adresu MAC přiřazená jiných nic.
6. Musí mít jednu primární **soukromé** *IPv4* statické nebo dynamické IP adresu přiřazenou.
8. Může být jeden zdroj veřejné adresy IP přidruženými k němu.
9. Podporuje zrychlit síť s jednoduchým kořenové vstupu a výstupu virtualizace (SR IOV) pro specifických velikostí jeho OM konkrétní verzí operačního systému Microsoft Windows Server. Další informace o této funkci Náhled najdete v článku [Accelerated sítě pro virtuální počítač](virtual-network-accelerated-networking-powershell.md) .
10. Můžete přijímat přenosy nejsou určeny soukromé adresy přiřazené k němu Pokud IP přesměrování aktivované řešení síťovou Pokud virtuálního počítače je třeba systém software brány firewall, směruje pakety nejsou určeny pro vlastní IP adres. OM musí pořád software pro směrování nebo jejich předávání dál přenosy fungují, ale k tomu, musí být povolené IP přesměrování na NIC.
11. Často vznikne ve stejné skupině zdroje jako OM je připojen k nebo stejné VNet, který je připojen k, když ho není třeba.

## <a name="vms-with-multiple-network-interfaces"></a>VMs s více síťových rozhraní

Více nic může být připojen angličtině, ale když tím, mějte na paměti toto:  

- Velikost OM podporoval více nic. Další informace o tom, které podporují velikostí OM více nic, přečtěte si články [systému Windows Server OM velikostí](../virtual-machines/virtual-machines-windows-sizes.md) nebo [velikosti OM Linux](../virtual-machines/virtual-machines-linux-sizes.md) .   
- OM musí vytvořit se aspoň dva nic. Pokud OM se vytvoří pomocí jedinou NIC, i když velikost OM podporuje více než jedno, nemůžete připojit další nic bude v angličtině po vytvoření otevřela. Dokud OM byl vytvořený pomocí aspoň dva nic, můžete připojit další nic bude v angličtině po vytvoření otevřela, dokud velikost OM podporuje více než dvě nic.  
- Sekundární nic (primární NIC nelze odpojit) můžete odpojit od virtuálního počítače, pokud OM obsahuje nejméně tři nic připojené k němu. Nic nelze odpojit, pokud OM má dvě nebo méně nic připojené k němu.  
- Pořadí nic z uvnitř OM náhodně a také můžete změnit přes Azure infrastruktury aktualizace. Však adresy IP adresy a odpovídající ethernet MAC, zůstane. Předpokládejme například, že operační systém identifikuje Azure NIC1 jako Eth1. Eth1 má adresou IP 10.1.0.100 a MAC 00-0D-3A-B0-39-0D. Po infrastruktury služby Azure aktualizovat a restartujte, operační systém teď stanovit Azure NIC1 jako Eth2, ale adres IP a MAC budou shodovat s stejně jako při operační systém identifikovat Azure NIC1 jako Eth1. Po restartování počítače se inicializovaný zákazníka směru NIC zůstane v operačním systému.  
- Pokud OM patří do skupiny [Nastavte dostupnost](../azure-glossary-cloud-terminology.md#availability-set), musí mít všechny VMs v rámci sady dostupnost NIC jednoho nebo více nic. Více nic máte VMs číslo každý mají není musí být stejná, dokud každý OM obsahuje aspoň dva nic.

## <a name="next-steps"></a>Další kroky

- Naučte se vytvářet virtuálního počítače s jednoho NIC přečíst v článku [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Naučte se vytvářet virtuálního počítače s více nic o [nasazení OM s více NIC](virtual-network-deploy-multinic-arm-ps.md) článek.
- Naučte se vytvářet NIC s více IP konfigurací přečíst v článku [více IP adres pro Azure virtuálních počítačích](virtual-network-multiple-ip-addresses-powershell.md) .
