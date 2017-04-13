<properties
   pageTitle="Centrum zabezpečení Azure – úvodní příručka | Microsoft Azure"
   description="Tento článek vám pomůže začít rychle pomocí Centra zabezpečení Azure provede vás součásti správy sledování a zásady zabezpečení a propojením k dalším krokům."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Centrum zabezpečení Azure – úvodní příručka

Tento článek vám pomůže rychle začít s Azure Centrum zabezpečení a provede vás zabezpečení sledování a zásady správy součástí Centra zabezpečení.

> [AZURE.NOTE] Tento článek uvádí službu pomocí nasazení příklad. Tento článek není podrobného průvodce.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Začít s Centrum zabezpečení, musíte mít předplatné Microsoft Azure. Pokud nemáte předplatné, můžete registraci [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Bezplatné osy Centrum zabezpečení je automaticky zapnutá k vašemu předplatnému a poskytuje přehled o stavu zabezpečení Azure prostředků. Poskytuje Správa zásad základní zabezpečení doporučení týkající se zabezpečení a integrace s zabezpečení produkty a služby Azure partnerů.

Máte přístup k Centru zabezpečení z [Azure portálu](https://azure.microsoft.com/features/azure-portal/). Další informace o portálu Azure, najdete v článku [si přečtěte následující dokumentaci portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Shromažďování dat

Centrum zabezpečení shromažďuje údaje o virtuálních počítačích (VMs) posuďte stavu zabezpečení, poskytnutí doporučení týkající se zabezpečení a upozornění na rizika. Při prvním přístup k Centru zabezpečení, shromažďování dat povolený na všechny VMs ve vašem předplatném. Shromažďování dat je vhodné, ale můžete se rozhodnout vypnutím shromažďování dat v Centru zabezpečení zásadách.

Následující kroky popisují, jak zobrazit a použít součásti webové části Centrum zabezpečení. V těchto krocích jsme ukazují, jak vypnout shromažďování dat, pokud zvolíte možnost odhlásit.

## <a name="access-security-center"></a>Centrum zabezpečení aplikace Access

Na portálu takto pro přístup k Centru zabezpečení:

1. V nabídce **Microsoft Azure** vyberte **Centrum zabezpečení**.
![Nabídka Azure][1]

2. Pokud pracujete s Centrum zabezpečení poprvé, otevře **úvodní** zásuvné. Vyberte možnost Ano **! Budu chtít Centrum zabezpečení Azure spuštění** otevřete zásuvné **Centrum zabezpečení** a povolte shromažďování dat.
![Úvodní obrazovka][10]

3. Po spuštění Centrum zabezpečení z úvodní zásuvné nebo v nabídce Microsoft Azure vyberte Centrum zabezpečení, otevře se zásuvné **Centrum zabezpečení** . Pro snadný přístup k zásuvné **Centrum zabezpečení** v budoucnu přepínač **zásuvné připnout na řídicí panel** (vpravo nahoře).
![Připnutí zásuvné možnost řídicích panelů][2]

## <a name="use-security-center"></a>Pomocí Centra zabezpečení

Můžete nakonfigurovat zásady zabezpečení pro Azure předplatná a skupiny zdrojů. Pojďme konfigurace zásad zabezpečení pro vaše předplatné:

1. Na zásuvné **Centrum zabezpečení** vyberte dlaždici **zásad** .
![Zásady zabezpečení][3]

2. Na zásuvné **zásady zabezpečení – definování zásad za předplatné nebo skupiny prostředků** vyberte předplatné.
3. Na zásuvné **zásady zabezpečení** je povoleno **shromažďování dat** automaticky shromáždit protokoly. Sledování rozšíření máte k dispozici na všech aktuální tak i u nových VMs v předplatného. (Můžete vyjádření výslovného nesouhlasu shromažďování dat nastavením **shromažďování dat** na **Vypnuto**, ale to zabrání Centrum zabezpečení pojmenování výstrah zabezpečení a doporučení.)
4. Na zásuvné **zásady zabezpečení** možnost **Zvolit účet úložiště jednotlivých oblastech**. Pro každou oblast, ve kterém máte VMs spuštěná zvolte úložiště účet, kde je uložena data shromážděná z těchto VMs. Pokud nevyberete účet úložiště pro každou oblast, vytvoří se za vás. Data, která se shromažďují je logicky izolovaný od ostatních zákazníků data z bezpečnostních důvodů.

     > [AZURE.NOTE] Doporučujeme povolit shromažďování dat a vybrat účet úložiště na úrovni předplatné jméno. Nastavit zásady zabezpečení na úrovni Azure předplatné a úrovně seskupení zdrojů dochází ke konfiguraci shromažďování dat a účtu úložiště na úrovni předplatného.

5. Na zásuvné **zásady zabezpečení** vyberte **zásad pro prevenci**. Otevře se zásuvné **zásad pro prevenci** .
![Do zásad pro prevenci][4]

6. Na zásuvné **zásad pro prevenci** zapněte doporučení, která chcete zobrazit jako součást svoje zásady zabezpečení. Příklady:

 - Nastavení **aktualizací systému** **na** kontroluje všechny podporované virtuálních počítačích chybějících OS aktualizace.
 - Nastavení **chyby OS** **na** kontroluje všechny podporované virtuálních počítačích k identifikaci konfigurace OS, které mohou virtuální počítač útoku.

### <a name="view-recommendations"></a>Zobrazení doporučení

1. Vraťte se do **Centra zabezpečení** zásuvné a vyberte dlaždici **doporučení** . Centrum zabezpečení pravidelně analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, zobrazuje doporučení na zásuvné **doporučení** .
![Doporučení v Centru zabezpečení Azure][5]

2.  Vyberte doporučení na zásuvné **doporučení** zobrazíte další informace a/nebo provádět akce tento problém vyřešit.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Zobrazení stavu zdraví a zabezpečení prostředků.

1.  Vraťte se do zásuvné **Centrum zabezpečení** . Na dlaždici **prostředky zabezpečení stavu** obsahuje indikátory stavu zabezpečení pro virtuálních počítačích, sítí, dat a aplikace.
2.  Vyberte **virtuálních počítačích** zobrazíte další informace. Zásuvné **virtuálních počítačích** spustí a zobrazí stav souhrn antimalware programy, aktualizace systému, restartování a chyby operační systém vašeho VMs.
![Na dlaždici prostředky stavu v Centru zabezpečení Azure][6]

3.  Vyberte doporučení v části **VIRTUÁLNÍHO počítače doporučení** pro zobrazení dalších informací a/nebo Akce konfigurace nutné ovládací prvky.
4.  Vyberte OM v části **virtuálních počítačích** zobrazíte další informace.

### <a name="view-security-alerts"></a>Zobrazení upozornění zabezpečení

1.  Vraťte se do **Centra zabezpečení** zásuvné a vyberte dlaždici **výstrah zabezpečení** . Zásuvné **výstrahy zabezpečení** se otevře a zobrazí seznam upozornění. Centrum zabezpečení analýzy protokoly zabezpečení a činnosti sítě vygeneruje tyto upozornění. Upozornění od integrované partnerských řešení jsou však započítávány.
![Upozornění zabezpečení v Centru zabezpečení Azure][7]

    > [AZURE.NOTE] Upozornění zabezpečení jsou dostupné, jenom Pokud je povoleno standardní osy Centrum zabezpečení. 90 dnů bezplatnou zkušební verzi standardní osy je k dispozici. Informace o tom, jak získat standardní osy naleznete v tématu [Další kroky](#next-steps) .

2.  Vyberte upozornění při zobrazení dalších informací. V tomto příkladu vybereme **zjistil změněno systém binární**. Otevře se listy, které obsahují další podrobnosti o oznámení.
![Podrobnosti výstrahy zabezpečení v Centru zabezpečení Azure][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Zobrazení stavu partnerských řešení

1. Vraťte se do zásuvné **Centrum zabezpečení** . Dlaždice **partnerských řešení** umožňuje na první pohled, sledování stavu vaší partnerských řešení integrovaný s Azure předplatné.
2. Vyberte dlaždici **partnerských řešení** . Zásuvné spustí a zobrazí seznam vaší partnerských řešení připojené k Centru zabezpečení.
![Partnerských řešení][9]

3. Vyberte partnerských řešení. V tomto příkladu vybereme řešení **F5 WAF** .  Zásuvné otevře a uvidíte stav partnerských řešení a související materiály na řešení. Vyberte možnost **konzoly řešení** otevřete možnostem správy partnera pro toto řešení.

## <a name="next-steps"></a>Další kroky
Tento článek směřující k zabezpečení sledování a zásady správy součástí Centra zabezpečení. Teď, když znáte Centrum zabezpečení, vyzkoušejte následující kroky:

- Konfigurace zásad zabezpečení u předplatného Azure. Další informace najdete v tématu [nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md).
- Pomocí doporučení v Centru zabezpečení můžete chránit vaše Azure zdroje. Další informace najdete v tématu [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md).
- Kontrola a správa aktuální výstrah zabezpečení. Další informace najdete v tématu [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md).
- Další informace o [Pokročilé funkce rozpoznávání hrozbou](security-center-detection-capabilities.md) , které jsou součástí [Standardní osy](security-center-pricing.md) v Centru zabezpečení. 90 dnů bezplatnou zkušební verzi standardní osy je k dispozici.
- Pokud máte dotazy k používání Centra zabezpečení, přečtěte si článek [Azure časté otázky k Centru zabezpečení](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
