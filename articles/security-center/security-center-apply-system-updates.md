<properties
   pageTitle="Použití aktualizací systému v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **použití aktualizací systému** a **restartujte po aktualizace systému**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Použití aktualizací systému v Centru zabezpečení Azure

Centrum zabezpečení Azure sleduje denně Windows a Linux virtuálních počítačích (VMs) chybějících aktualizace operačního systému. Centrum zabezpečení načte seznam dostupných zabezpečení a důležité aktualizace z Windows Update nebo Windows Server služby WSUS (Update), podle toho, které je služba nakonfigurovaný na OM Windows.  Centrum zabezpečení nejnovější aktualizace taky hledá v systémech Linux. Pokud vaše OM chybí aktualizaci systému, bude Centrum zabezpečení doporučujeme použít aktualizace systému

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **použít systém aktualizace**.
![Použití aktualizací systému][1]

2. **Aktualizace systému použít** zásuvné otevře s přehledem VMs chybějící aktualizace systému. Vyberte virtuálního počítače.
![Vyberte virtuálního počítače][2]

3. Zásuvné otevře zobrazení seznamu chybějící aktualizace pro tento OM. Vyberte aktualizace systému. V tomto příkladu vybereme KB3156016.
![Chybějící aktualizace zabezpečení][3]

4. Postupujte podle kroků v tématu **Aktualizace zabezpečení** zásuvné použít chybějící aktualizaci.
![Aktualizace zabezpečení][4]

## <a name="reboot-after-system-updates"></a>Restartujte po aktualizace systému

5. Vraťte se do zásuvné **doporučení** . Nová položka vygenerované po použití aktualizací systému s názvem **restartujte po aktualizace systému**. Tento údaj značí, že bude potřeba restartovat OM dokončete proces instalace aktualizací systému.
![Restartujte po aktualizace systému][5]

6. Vyberte, **restartujte po aktualizace systému**. Otevře se **že je na zjištění stavu úkolů k dokončení aktualizací systému** zásuvné s přehledem VMs, které je potřeba restartovat dokončete proces aktualizace systém použít.
![Restartujte na zjištění stavu úkolů][6]

Restartujte OM z Azure dokončete proces.

## <a name="see-also"></a>Viz taky

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
