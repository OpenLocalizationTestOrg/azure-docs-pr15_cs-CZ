<properties
   pageTitle="Úvod k Centru zabezpečení Azure | Microsoft Azure"
   description="Informace o Centrum zabezpečení Azure klíčové funkce, a jak to funguje."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Úvod k Centru zabezpečení Azure

Informace o Centrum zabezpečení Azure klíčové funkce, a jak to funguje.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad.

## <a name="what-is-azure-security-center"></a>Co je Centrum zabezpečení Azure?
 Centrum zabezpečení pomáhá zabránit, zjišťování a odpovědět hrozeb lepší viditelnost do a publikum nemůže ovládat zabezpečení Azure prostředků. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

##  <a name="key-capabilities"></a>Klíčové funkce
 Centrum zabezpečení nabízí snadno použitelné a efektivní hrozbou prevence, zjišťování a odpověď funkcí, které jsou součástí Azure. Klíčové funkce jsou:

| | |
|----- |-----|
| Zabránit | Sledování stavu zabezpečení Azure prostředků. |
| | Definuje zásad pro Azure předplatná a zdroje skupiny zabezpečení požadavkům vaší společnosti, typy aplikace, které používáte a utajení dat |
| | Použití zásad řízené úsilím zabezpečení doporučení Průvodce vlastníků služby procesem implementace potřeby ovládacích prvků |
| | Rychle nasadí služby zabezpečení a zařízení společnostmi Microsoft a partnery |
| Zjišťování |Automaticky shromažďuje a analyzuje zabezpečení dat z Azure zdrojů, sítě a partnerských řešení jako antimalware programy a bránách firewall |
| | Globální zneužívá, ochrany proti novým ohrožením intelligence z produkty společnosti Microsoft a služby, Jednotková služby Microsoft digitální zločinů (DCU), Microsoft zabezpečení odpověď Center (Nejčastější) a externí kanálů RSS |
| | Slouží k použití pokročilé technologie pro analýzu, včetně výukové počítače a chování analýzy |
| Odpovědět | Poskytuje uspořádaný zabezpečení incidentem/upozornění |
| | Nabízí podstatu zdroji útok a dotčeném zdrojů |
| | Navrhuje způsoby, jak zastavit aktuální útok a ochrana před útoky typu budoucí |

## <a name="introductory-walkthrough"></a>Úvodní informace
 Máte přístup k Centru zabezpečení z [Azure portálu](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se k portálu](https://portal.azure.com), vyberte **Procházet**, posuňte zobrazení na položku **Centrum zabezpečení** a vyberte dlaždici **Centrum zabezpečení** , které jste se dříve připnuté na portálu řídicí panel.

![Dlaždice zabezpečení Azure portálu][1]

V Centru zabezpečení můžete nastavit zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazení upozornění zabezpečení.

### <a name="security-policies"></a>Zásady zabezpečení

Můžete definovat zásad pro Azure předplatná a skupin zdrojů podle požadavkům na zabezpečení vaší společnosti. K těmto typům aplikací, které používáte nebo citlivosti dat můžete přizpůsobit v každého předplatného. Například prostředky pro vývoj nebo testování třetí strana pravděpodobně požadavky na zabezpečení jiné než pro výrobní aplikace. Podobně aplikací s regulovaném dat jako Umožňujících může vyžadovat do vyšší úrovně zabezpečení.

> [AZURE.NOTE] Změnit zásady zabezpečení na úrovni předplatné nebo úroveň seskupení zdrojů, musíte být vlastníkem předplatné nebo Přispěvatel k němu.

Na zásuvné **Centrum zabezpečení** vyberte dlaždici **zásad** seznam svoje předplatné a skupiny zdrojů.   

![Centrum zabezpečení zásuvné][2]

Na zásuvné **zásady zabezpečení** vyberte předplatné a zobrazit podrobnosti zásad.

![Předplatné zásuvné zásady zabezpečení][3]

**Shromažďování dat** (viz výše) umožňuje shromažďování dat pro zásady zabezpečení. Povolení umožňuje:

- Denní kontrola všech podporované virtuálních počítačích pro sledování zabezpečení a doporučení.
- Kolekce událostí zabezpečení pro analýzu a hrozbou, že zjišťování.

**Vyberte účet úložiště jednotlivých oblastech** (viz výše) můžete vybrat pro každou oblast, ve kterém máte virtuálních počítačích systém, účtu úložiště, kde je uložena data shromážděná z těchto virtuálních počítačích. Pokud nevyberete účet úložiště pro každou oblast, vytvoří se za vás. Data, která se shromažďují je logicky izolovaný od ostatních zákazníků data z bezpečnostních důvodů.

> [AZURE.NOTE] Shromažďování dat a kliknutím na účet úložiště jednotlivých oblastech nakonfigurovaný na úrovni předplatného.

Otevřete zásuvné **zásad pro prevenci** vyberte **zásad pro prevenci** (viz výše). **Zobrazit doporučení pro** můžete vybrat, zda ovládací prvky zabezpečení, které chcete sledovat a doporučujeme, abyste na základě zabezpečení potřeb zdrojů v rámci předplatného.

Potom vyberte skupinu zdrojů a zobrazit podrobnosti zásad.

![Skupina zdroje zásuvné zásady zabezpečení][4]

**Dědičnost** (viz výše) umožňuje definovat skupina zdroje jako:

- Zděděný (výchozí) která znamená, že všechny zásady zabezpečení pro tuto skupinu zdroje se dědí od úrovně předplatného.
- Jedinečný což znamená, skupina zdroje, bude mít vlastní zásady zabezpečení. Musíte se ke změnám ve skupinovém rámečku **Zobrazit doporučení pro**.

> [AZURE.NOTE] Pokud dojde ke konfliktu mezi úrovní zásad předplatné a úrovní zásad skupiny zdrojů, úrovní zásad skupiny zdrojů přednost.

### <a name="security-recommendations"></a>Doporučení týkající se zabezpečení

 Centrum zabezpečení analyzuje stav zabezpečení Azure prostředky k identifikaci možnými chybami zabezpečení. Seznam doporučení provede vás procesem konfigurace potřebných ovládacích prvků. Jako příklad lze uvést:

- Zřízení antimalware pomůže identifikovat a odebrání škodlivým softwarem
- Konfigurace skupin zabezpečení sítě a pravidla pro ovládací prvek přenosy na virtuálních počítačích
- Zřizování webové aplikace bránách firewall pro chránit před útoky cílené webových aplikací
- Nasazení chybějící aktualizace systému
- Adresování konfigurace OS, které neodpovídají doporučený směrných plánů

Klikněte na dlaždici **doporučení** seznam doporučení. Klikněte na každý doporučení zobrazíte další informace nebo akce tento problém vyřešit.

![Doporučení týkající se zabezpečení v Centru zabezpečení Azure][5]

### <a name="resource-health"></a>Stav zdroje

Dlaždice **zdroje zabezpečení stavu** zobrazuje celkové postoje zabezpečení prostředí podle typu zdroje, včetně virtuálních počítačích, webových aplikací a dalších zdrojů.   

Vyberte typ zdroje na dlaždici **zdroje zabezpečení stav** zobrazíte další informace, včetně jejich seznamu všechny potenciální chyby zabezpečení, které jste určili. (**Virtuálních počítačích** je vybrané v příkladu níže).

![Dlaždice stavu zdrojů][6]

### <a name="security-alerts"></a>Upozornění zabezpečení

 Centrum zabezpečení automaticky shromažďují analyzuje a integruje protokolu dat z Azure zdrojů, sítě a partnerských řešení jako antimalware programy a bránách firewall. Když nejsou nalezeny hrozeb, vytvoří se výstrahy zabezpečení. Jako příklad lze uvést zjišťování:

- Hostují virtuálních počítačích komunikaci s známé škodlivým IP adres
- Pokročilé malware zjistit pomocí zasílání zpráv o chybách systému Windows
- Přímým útokům proti virtuálních počítačích
- Výstrahy zabezpečení integrovaných antimalware programy a brány firewall

Kliknutí na dlaždici **výstrahy zabezpečení** se zobrazí seznam uspořádaný upozornění.

![Upozornění zabezpečení][7]

Výběr upozornění se zobrazí další informace o útok a návrhy na tom, jak ho nápravě.

![Podrobnosti výstrahy zabezpečení][8]

### <a name="partner-solutions"></a>Partnerských řešení

Dlaždice **partnerských řešení** umožňuje monitor – rychlý přehled stavu partnerských řešení integrovaný s Azure předplatné. Centrum zabezpečení zobrazí upozornění pocházejících z řešení.

Vyberte dlaždici **partnerských řešení** . Zásuvné spustí se zobrazí seznam všech propojených partnerských řešení.

![Partnerských řešení][9]

## <a name="get-started"></a>Začínáme
Začínáme s Centrum zabezpečení, je nutné předplatné Microsoft Azure. Centrum zabezpečení je povolený s předplatným Azure. Pokud nemáte předplatné, můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

 Máte přístup k Centru zabezpečení z [Azure portálu](https://azure.microsoft.com/features/azure-portal/). Získáte v [portálu si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/azure-portal/) Další informace.

[Začínáme s Centrum zabezpečení Azure](security-center-get-started.md) rychle povede vás provede sledování zabezpečení a Správa zásad součásti webové části Centrum zabezpečení.

## <a name="see-also"></a>Viz taky
V tomto dokumentu byly vydané a Centrum zabezpečení, klíčové funkce, jak začít. Další informace najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Zabezpečení Azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
