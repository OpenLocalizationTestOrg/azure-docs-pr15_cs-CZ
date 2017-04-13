<properties
   pageTitle="Povolení OM agenta v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Povolit Agent OM**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Povolení OM agenta v Centru zabezpečení Azure

Agent OM musí být nainstalovaný na virtuálních počítačích (VMs) za účelem [Povolení shromažďování dat](security-center-enable-data-collection.md).  Centrum zabezpečení Azure vám umožní zobrazit které VMs vyžadují agenta OM a doporučí povolení agenta OM v těchto VMs.

Agent OM nainstalovaný ve výchozím nastavení pro VMs, které jsou nasazeny z Azure Marketplace. V článku [rozšíření – část 2 a OM Agent](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) obsahuje informace o tom, jak nainstalovat agenta OM.


> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad. Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V **zásuvné doporučení**vyberte **Povolit Agent OM**.
![Povolení OM agenta][1]

2. Otevře se zásuvné **OM Agent je chybějící nebo neodpovídá**. Tento zásuvné seznamy, které vyžadují agenta OM VMs. Podle pokynů na zásuvné instalace agenta OM.
![Chybí OM Agent][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
