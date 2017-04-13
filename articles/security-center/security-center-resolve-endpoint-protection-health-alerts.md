<properties
   pageTitle="Řešení koncový bod ochrany stavu upozornění v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **vyřešit Endpoint Protection stavu upozornění**."
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

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Řešení koncový bod ochrany stavu upozornění v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí vyřešit zjištěné endpoint protection stavu upozornění.  Centrum zabezpečení vám umožní zobrazit které virtuálních počítačích (VMs) k dispozici selhání ochranu koncového bodu a kolik k chybám.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad. Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V **zásuvné doporučení**vyberte **vyřešit Endpoint Protection stavu upozornění**.
![Řešení koncový bod ochrany stavu upozornění][1]

2. Otevře se zásuvné **Endpoint Protection selhání** , které je uvedené VMs s chybami a počet neúspěšných pokusů pro každou OM. Ze seznamu vyberte virtuálního počítače.
![Chyba při ochraně koncový bod][2]

3. **Selhání seznamu** zásuvné Otevře vybraný OM s přehledem k chybám. Vyberte selhání ze seznamu a další informace. Otevře se zásuvné s informacemi o chybě vybrané.
![Seznam selhání][3]
  ![selhání události][4]

## <a name="see-also"></a>Viz taky

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md)– zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure](security-center-recommendations.md)– zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md)– zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md)– zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md)– najít často kladené otázky týkající se pomocí služby.
- [Zabezpečení azure blog](http://blogs.msdn.com/b/azuresecurity/)– získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
