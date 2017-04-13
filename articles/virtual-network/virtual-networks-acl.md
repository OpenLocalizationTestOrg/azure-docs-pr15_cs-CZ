<properties
   pageTitle="Co je v seznamu pro řízení přístupu síti (ACL)?"
   description="Další informace o ACL"
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

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Co je koncový bod seznam řízení přístupu (ACL)?

Koncový bod seznam řízení přístupu (ACL) je zabezpečení rozšíření k dispozici pro nasazení služby Azure. ACL umožňuje selektivně povolit nebo zakázat přenosů koncového bodu virtuálního počítače. Tuto funkci filtrování paketů poskytuje další úroveň zabezpečení. Můžete zadat sítě ACL pro koncové body pouze. Nelze zadat ACL pro virtuální sítě nebo konkrétní podsítě obsažené v virtuální sítě.

> [AZURE.IMPORTANT] Použití sítě skupin zabezpečení (NSGs) se doporučuje namísto ACL kdykoli je to možné. Další informace o NSGs najdete v tématu [Co je skupina zabezpečení sítě?](virtual-networks-nsg.md).

ACL je možné konfigurovat pomocí prostředí PowerShell nebo portálu pro správu. Konfigurace sítě ACL pomocí prostředí PowerShell, najdete v článku [Správa seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md). Konfigurace sítě ACL pomocí portálu pro správu, najdete v článku [jak nastavit koncové body do virtuálního počítače](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Použití sítě ACL, můžete udělat toto:

- Selektivně povolit nebo zakázat příchozích podle vzdálené adres podsítí IPv4 adresu oblasti zadávání koncový bod virtuálního počítače.

- Zakázaných IP adres

- Vytvoření více pravidel každý koncový bod virtuálního počítače

- Určit až 50 pravidla ACL každý koncový bod virtuálního počítače

- Použití pravidel řazení zajistit správnou sadu pravidel, použijí se na daný virtuální počítač endpoint (nejnižší k nejvyšší)

- Určení ACL pro konkrétní vzdálené adres podsítí IPv4 adresu.

## <a name="how-acls-work"></a>Fungování ACL

ACL je objekt, který obsahuje seznam pravidel. Při vytváření ACL a použít ho na počítač virtuální endpoint filtrování paketů probíhá na uzel hostitele vaší OM. To znamená, že provoz z Vzdálená IP adresy filtrované podle uzel hostitele odpovídající ACL pravidla místo na vaše OM. To vaše OM zabrání výdaje cenné cykly CPU filtrování paketů.

Po vytvoření virtuálního počítače výchozí ACL se vloží na místě chcete blokovat všechny příchozí přenosy. Ale pokud koncový bod se vytvoří (port 3389), potom výchozí ACL upravit tak, aby přenosy všechny příchozí tohoto koncového bodu. Příchozí data ze vzdáleného podsítě potom moct tohoto koncového bodu a bez zřízení brány firewall je nutný. Všechny ostatní porty jsou blokovány u příchozích nevytváříte koncové body pro tyto porty. Ve výchozím nastavení jsou povolené odchozí komunikaci.

**Tabulka s příkladem – výchozí ACL**

| **Pravidlo #** | **Vzdálené podsítě** | **Koncový bod** | **Povolit nebo odepřít** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Povolení      |

## <a name="permit-and-deny"></a>Povolit a odepřít

Selektivně můžete povolit nebo zakázat pro zadávání koncový bod virtuálního počítače v síti tak, že vytvoříte pravidla, které určují "Povolit" nebo "Odepřít". Je důležité mít na paměti, že ve výchozím nastavení po vytvoření koncového bodu jsou povoleny všechny přenosy na koncový bod. Z tohoto důvodu je důležité pochopit, jak vytvořit povolit nebo odepřít pravidla a jejich ve správném pořadí priorit požadovaná podrobného publikum nemůže ovládat v síti, které chcete povolit přístup koncový bod virtuálního počítače.

Aspekty k zamyšlení:

1. **Žádné ACL –** Ve výchozím nastavení po vytvoření koncového bodu jsme povolit všechny pro koncový bod.

1. **Povolit-** Když přidáte jeden nebo více "Povolit" oblasti, jsou ve výchozím nastavení odepření jiné oblasti. Pouze pakety z v povoleném rozsahu IP budou moct komunikovat s koncový bod virtuálního počítače.

1. **Odepřít-** Když přidáte jeden nebo více "Odepřít" oblastí, dáváte všechny oblasti provoz ve výchozím nastavení.

1. **Kombinace povolit a odepřít-** Můžete kombinací "Povolit" a "Odepřít", když budete chtít doložka mimo určitý rozsah IP povoleno nebo odepřít.

## <a name="rules-and-rule-precedence"></a>Pravidla a priorit pravidel

Síť ACL můžete nastavit na konkrétní virtuální počítač koncové body. Například můžete zadat v síti ACL pro koncový bod RDP vytvořil virtuální počítač, který zámky dolů přístup pro určité IP adresy. Následující tabulka ukazuje, jak udělit přístup k veřejné virtuální adresy IP (adres VIP) některé oblasti k povolení přístupu pro RDP. Všechny ostatní vzdálené adresy odepřen. Jsme podle *nejnižší přednost* pořadí pravidel.

### <a name="multiple-rules"></a>Více pravidel

V následujícím příkladu Pokud chcete povolit přístup k koncový bod RDP jenom z dva veřejné IPv4 rozsahy adres (65.0.0.0/8 a 159.0.0.0/8) můžete dosáhnout to tím, že zadáte dvě *Povolit* pravidla. V tomto případě od RDP se vytvoří ve výchozím nastavení virtuálního počítače, můžete uzamknout přístup do portu RDP podle vzdálené podsítě. Následující příklad ukazuje způsob, jak udělit přístup k veřejné virtuální adresy IP (adres VIP) některé oblasti k povolení přístupu pro RDP. Všechny ostatní vzdálené adresy odepřen. To funguje, protože sítě ACL můžete nastavit pro konkrétní virtuální počítač koncového bodu a přístup odepřen ve výchozím nastavení.

**Příklad – více pravidel**

| **Pravidlo #** | **Vzdálené podsítě** | **Koncový bod** | **Povolit nebo odepřít** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Povolení      |
| 200    | 159.0.0.0/8   | 3389     | Povolení      |

### <a name="rule-order"></a>Pořadí pravidel

Protože více pravidel lze zadat pro koncový bod, musí být způsob, jak uspořádat pravidla s cílem určit pravidlo, které mají přednost. Pořadí pravidel určuje prioritu. Síť ACL podle *nejnižší přednost* pořadí pravidel. V následujícím příkladu je koncového bodu na portu 80 selektivně udělený přístup jenom některých rozsahy IP adres. Konfigurace to máme pravidlo Odepřít (pravidlo \# 100) pro adresy do pole 175.1.0.1/24. Druhé pravidlo je pak určené s prioritou 200, který umožňuje přístup k všechny adresy ve skupinovém rámečku 175.0.0.0/8.

**Příklad – priorit pravidel**

| **Pravidlo #** | **Vzdálené podsítě** | **Koncový bod** | **Povolit nebo odepřít** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Odepřít        |
| 200    | 175.0.0.0/8   | 80       | Povolení      |

## <a name="network-acls-and-load-balanced-sets"></a>Sítě ACL a načíst rovnováha sady

Síť ACL můžete zadanou v nastavení (LB nastavené) endpoint rozloženy. Je-li ACL pro sadu LB, ACL sítě se použije pro všechny virtuálních počítačích v této sadě LB. Například pokud LB nastavení se vytvoří pomocí "Port 80" a nastavit LB obsahuje 3 VMs, ACL sítě na vytvoření koncového bodu "Porty 80" jedné OM bude automaticky používat pro ostatní VMs.

![Sítě ACL a načíst rovnováha sady](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Další kroky

[Jak spravovat seznamy řízení přístupu (ACL) koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md)
