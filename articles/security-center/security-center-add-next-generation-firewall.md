<properties
   pageTitle="Přidání dalšího brány firewall generování v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Přidat další generování Firewall** a **směrování traffice prostřednictvím NGFW pouze**."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Přidání dalšího brány Firewall generování v Centru zabezpečení Azure

Centrum zabezpečení Azure může doporučit přidat další brány firewall generování (NGFW) od partnera společnosti Microsoft pro zvýšení zabezpečení ochrany. Tento dokument provede vás provede příklad toho, jak můžete to udělat.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **Přidat další generování Firewall**.
![Přidání dalšího brány Firewall generování][1]

2. V zásuvné **Přidat další brány Firewall generování** vyberte koncový bod.
![Vyberte koncový bod][2]

3. Otevře se druhá zásuvné **Přidat další generování Firewall** . Můžete použít existující řešení, pokud je k dispozici nebo vytvořte novou. V tomto příkladu nejsou žádné existujících řešení k dispozici, vytvoříte nový NGFW.
![Vytvořit novou bránu Firewall další generování][3]

4. Pokud chcete vytvořit nové NGFW, vyberte ze seznamu integrované partnerů řešení. V tomto příkladu jsme vyberte **Zaškrtnutí bod**.
![Vyberte další brány Firewall generování řešení][4]

5. **Zaškrtnutí čárky** zásuvné otevře poskytuje informace o řešení partnerů. Výběrem možnosti **vytvořit** v zásuvné informace.
![Informace o zásuvné brána firewall][5]

6. Otevře se zásuvné **vytvořit virtuální počítač** . Tady můžete zadat informace potřebné k číselník přes virtuální počítač (OM), který se spustí NGFW. Postupujte podle pokynů a zadejte požadované informace NGFW. Vyberte OK použijete.
![Vytvoření virtuálního počítače ke spuštění NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Směrování přenosů NGFW pouze

Vraťte se do zásuvné **doporučení** . Nová položka vygenerované po přidání NGFW prostřednictvím centra zabezpečení volání **směrování přenosů NGFW pouze**. Toto doporučení se vytvoří, jenom v případě, že jste si nainstalovali NGFW prostřednictvím centra zabezpečení. Pokud máte internetového koncové body, Centrum zabezpečení doporučí konfigurace skupiny zabezpečení sítě pravidla, která vynutit příchozích OM prostřednictvím svého NGFW.

1. V **zásuvné doporučení**vyberte **směrování přenosů NGFW pouze**.
![Směrování přenosů NGFW pouze][7]

2. Otevře se zásuvné **směrovat přenosy v síti prostřednictvím NGFW pouze** které je uvedené VMs, které můžete směrovat přenosy v síti na. Ze seznamu vyberte virtuálního počítače.
![Vyberte virtuálního počítače][8]

3. Otevře se zásuvné pro vybraný OM zobrazující související příchozí pravidla. Popis poskytuje další informace o možných další kroky. Zvolte **Upravit příchozí pravidla** na pokračovat v úpravách pravidlo pro příchozí připojení. Očekává se, že **zdroje** není nastavená **všechny** pro koncové body internetového propojený s NGFW. Další informace o vlastnostech příchozí pravidla najdete v tématu [NSG pravidla](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Konfigurace pravidla pro omezení přístupu][9]
![upravit příchozí pravidla][10]

## <a name="see-also"></a>Viz taky

Tento dokument ukázal, jak implementovat Centrum zabezpečení doporučení "Přidat další brány Firewall generování." Další informace o NGFWs a řešení partnerů čárky zaškrtnutí, naleznete v následujících tématech:

- [Další generování Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Kontrola vSEC bodu](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
