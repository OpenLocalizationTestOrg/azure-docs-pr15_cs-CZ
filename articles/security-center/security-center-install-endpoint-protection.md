<properties
   pageTitle="Instalace Endpoint Protection v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Instalace Endpoint Protection**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Instalace Endpoint Protection v Centru zabezpečení Azure

Centrum zabezpečení Azure doporučí, pokud již není povolené antimalware zřízení aplikaci antimalware na Azure virtuálních počítačích (VMs). Toto doporučení platí jenom pro Windows VMs.

> [AZURE.NOTE] Tento dokument zavádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **Nainstalovat Endpoint Protection**.
![Vyberte instalovat Endpoint Protection][1]

2. **Instalace Endpoint Protection** zásuvné otevře s přehledem VMs bez antimalware povolené. Vyberte ze seznamu, které chcete nainstalovat antimalware a klikněte na tlačítko **instalovat na VMs**VMs.
![Vyberte VMs nainstalovat antimalware na][2]

3. **Vyberte Endpoint Protection** zásuvné otevře umožňuje vybrat antimalware řešení, které chcete použít. V tomto příkladu vybereme **Microsoft Antimalware**.
![Vyberte Endpoint Protection][3]

4. Zobrazí se další informace o řešení antimalware. Vyberte možnost **vytvořit**.
![Vytvoření řešení antimalware][4]

5. Zadejte požadovanou konfiguraci nastavení na zásuvné **Přidat linky** a pak vyberte **OK**. Další informace o nastavení najdete v tématu [výchozí a vlastní Antimalware konfigurace](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) je nyní aktivní na vybrané VMs.

## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Instalace Endpoint Protection." Další informace o povolení aplikace antimalware v Azure, najdete v těchto článcích:

- [Microsoft Antimalware pro cloudovými službami a virtuálních počítačích](../azure-security-antimalware.md) – zjistěte, jak pro nasazení Microsoft antimalware.

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
