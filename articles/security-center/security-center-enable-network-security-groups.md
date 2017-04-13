<properties
   pageTitle="Povolení sítě skupiny zabezpečení v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Povolit skupiny zabezpečení sítě**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Povolení sítě skupiny zabezpečení v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí povolení skupinu zabezpečení sítí (NSG), pokud již není povolené. NSGs obsahovat seznamu pravidel seznam řízení přístupu (ACL), která povolit nebo odepřít v síti na OM instance v síti virtuální. NSGs můžou být přidružené podsítí nebo jednotlivé instance OM v rámci této podsítě. Po přidružené k podsítě NSG ACL pravidla platí pro všechny instance OM v této podsítě. Umožnění datových přenosů do jednotlivých OM navíc může být omezený další přidružením NSG přímo do této OM. Další informace najdete v článku Další [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)

Pokud nemáte NSGs povoleno, Centrum zabezpečení zobrazeného dvou doporučení pro vás: povolení sítě skupinám zabezpečení podsítí a povolení sítě skupinám zabezpečení virtuálních počítačích. Vyberte úroveň, podsítě nebo OM, chcete-li použít NSGs.


> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **Povolit skupiny zabezpečení sítě** podsítí nebo na virtuálních počítačích.
![Povolení skupiny zabezpečení sítě][1]

2. Otevře se zásuvné **Konfigurace chybějící skupiny zabezpečení sítě** pro podsítí nebo virtuálních počítačích, podle toho, doporučení, který jste vybrali. Vyberte podsítě virtuálního počítače pro konfiguraci NSG.

  ![Konfigurace NSG podsítě][2]

  ![Konfigurace NSG pro OM][3]
3. Na zásuvné **skupiny zabezpečení síť zvolte** vyberte existující NSG nebo vytvořit nové NSG.

  ![Zvolte skupinu zabezpečení sítě][4]

Pokud vytvoříte nový NSG, postupujte podle pokynů v článku [Správa NSGs pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) vytvořit NSG a nastavit pravidla zabezpečení.

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Povolit sítě skupiny zabezpečení" podsítí nebo virtuálních počítačích. Další informace o povolení NSGs, najdete v těchto článcích:

- [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Jak spravovat NSGs pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- [Zabezpečení azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
