<properties
   pageTitle="Nápravě chyby operačního systému v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **nápravě OS chyby**."
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

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Nápravě chyby operačního systému v Centru zabezpečení Azure

Centrum zabezpečení Azure analyzuje denně virtuálního počítače (OM) operačního systému (OS) konfigurace, které by mohly volání OM útoku a doporučuje změny konfigurace při řešení těchto chyb. Další informace o konkrétních konfigurace sledován najdete v [seznamu doporučená konfigurace pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centrum zabezpečení doporučuje řešení chyb při konfiguraci operační systém vašeho OM neodpovídá doporučená konfigurace pravidel.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **nápravě OS chyby**.
![Nápravě chyby OS][1]

    **Chyby nápravě OS** zásuvné otevřete a zobrazíte seznam VMs s konfigurací OS, které se neshodují doporučená konfigurace pravidel.  U každé OM zásuvné identifikuje:

   - **Pravidla se nezdařilo** – počet pravidla, která OM OS konfigurace se nezdařila.
   - **Čas POSLEDNÍHO SKENOVÁNÍ** - datum a čas, Centrum zabezpečení poslední naskenovaný konfigurace OS OM.
   - **Stav** – aktuální stav chybu:

        - Otevřít: Chybu nebyla byla určena ještě
        - V průběhu: Je právě použita chybu, kterou je potřeba žádná akce
        - Vyřešit: Chybu byl vyplněnou adresou (Pokud problém vyřeší, položka se zobrazuje šedě)
  - **ZÁVAŽNOSTI** – všechny chyby jsou nastavené závažnosti minimum, což znamená chybu by měl být adresovaná ale nevyžaduje okamžitou pozornost.

Vyberte virtuálního počítače. Zásuvné pro tuto OM spustí a zobrazí pravidla, která se nezdařil.
   ![Konfigurace pravidel neúspěšných][2]

Vyberte pravidlo. V tomto příkladu umožňuje vybrat **heslo musí splňovat složitost**. Zásuvné otevře popisující selhalo pravidlo a jejich dopad. Zkontrolujte podrobnosti a zvažte použití konfigurace operačního systému.
  ![Popis selhalo pravidla][3]

  Centrum zabezpečení obsahuje běžné konfigurace výčet (CCE), která přiřazuje identifikátorů konfigurace pravidel. V tomto zásuvné je k dispozici následující informace:

  - NÁZEV – Název pravidla
  - ZÁVAŽNOSTI – CCE závažnosti přínosu kritické důležité nebo upozornění
  - CCIED – Jedinečný identifikátor CCE pravidla
  - Popis – Popis pravidla
  - CHYBY – Vysvětlení chybu nebo rizika, pokud nejsou použita pravidla
  - VLIV – Obchodních výsledků při použití pravidla
  - – OČEKÁVANÁ hodnoty očekávat při konfiguraci OM OS pravidlu analyzuje Centrum zabezpečení
  - OPERACE pravidel – Pravidla operace použije Centrum zabezpečení při analýze konfigurace OM OS pravidlu
  - SKUTEČNÉ hodnoty – Vrácena hodnota po analýzu konfigurace OM OS pravidlu
  - Výsledek vyhodnocení – – výsledek analýzy: předat, selhalo:


## <a name="see-also"></a>Viz taky

Tento článek ukázal, jak implementovat Centrum zabezpečení doporučení "Remediate OS chyby." Můžete zkontrolovat sady pravidel konfigurace [tady](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centrum zabezpečení obsahuje CCE (běžné konfigurace výčet), která přiřazuje identifikátorů konfigurace pravidel. Navštivte web [CCE](http://cce.mitre.org) Další informace.

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
