<properties
   pageTitle="Omezit přístup prostřednictvím internetového koncové body v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **omezit přístup přes Internet vystaveného koncového bodu**."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Omezit přístup prostřednictvím internetového koncové body v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí omezit přístup prostřednictvím internetového koncové body, pokud má některá příchozí pravidla, které umožňují přístup z "všechny" Zdrojová IP adresa některé ze svých skupin zabezpečení síti (NSGs). Otevření přístup k"" může povolit útočníků pro přístup k zdroje. Centrum zabezpečení doporučí upravit tyto příchozí pravidla pro omezení přístupu ke zdroji IP adresy, které skutečně potřebují mít přístup.

Vygeneruje se toto doporučení pro jakýkoli – web, který obsahuje "" jako zdroj.

> [AZURE.NOTE] Tento dokument zavádí službu pomocí nasazení příklad. Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V **zásuvné doporučení**vyberte **omezit přístup přes Internet vystaveného koncového bodu**.
![Omezit přístup prostřednictvím Internetu koncový bod][1]

2. Otevře se zásuvné **omezit přístup přes Internet vystaveného koncového bodu**. Tento zásuvné seznamy virtuálních počítačích (VMs) se příchozí pravidla, které vytvářejí potenciální problém se zabezpečením. Vyberte virtuálního počítače.
![Vyberte virtuálního počítače][2]

3. Zásuvné **NSG** zobrazí skupiny zabezpečení sítě informace související příchozí pravidla a související OM. Zvolte **Upravit příchozí pravidla** na pokračovat v úpravách pravidlo pro příchozí připojení.
![Skupina zabezpečení sítě zásuvné][3]

4. Na zásuvné **pravidla příchozí zabezpečení** vyberte příchozí pravidla na Upravit. V tomto příkladu vybereme **AllowWeb**.
![Příchozí pravidla zabezpečení][4]

  Poznámka: můžete také vybrat **výchozí pravidla** zobrazíte sadu pravidel výchozí obsažených ve všech NSGs. Výchozí pravidla nelze odstranit, ale, protože kterému jsou přiřazené nižší prioritu, můžete pomocí pravidel, která vytvoříte přepsat. Další informace o [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).
![Výchozí pravidla][5]

5. Na zásuvné **AllowWeb** úpravy vlastností příchozí pravidla tak, aby je **zdroj** IP adresy nebo blokování IP adres. Další informace o vlastnostech příchozí pravidla najdete v tématu [NSG pravidla](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Úprava příchozí pravidla][6]

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Omezit přístup přes Internet vystaveného koncového bodu." Další informace o povolení NSGs a pravidla, najdete v těchto článcích:

- [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Jak spravovat NSGs pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md)– zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure](security-center-recommendations.md)– zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md)– zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md)– zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md)– najít často kladené otázky týkající se pomocí služby.
- [Zabezpečení azure blog](http://blogs.msdn.com/b/azuresecurity/)– získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
