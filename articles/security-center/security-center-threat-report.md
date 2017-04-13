<properties
   pageTitle="Centrum zabezpečení Azure hrozbou, že Intelligence sestavy | Microsoft Azure"
   description="Tento dokument můžete použít Azure zabezpečení Centrum hrozbou, že inteligentní sestavy průběhu vyšetřování zobrazíte další informace týkající se upozornění zabezpečení."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Centrum zabezpečení Azure hrozbou, že analytických nástrojů sestavy
Tento dokument vysvětluje, jak Azure zabezpečení Centrum hrozbou, že inteligentní sestavy vám mohou pomoci další informace o hrozbou, které vygenerovalo oznámení na zabezpečení.

## <a name="what-is-a-threat-intelligence-report"></a>Co je sestava intelligence hrozbou, že?
Centrum zabezpečení zjišťování hrozbou, že funguje tak, že sledování informací o zabezpečení z Azure prostředků, sítě a připojené partnerských řešení. Analyzuje tyto informace, často korelace informace z různých zdrojů, k identifikaci hrozeb. Tento postup je součástí Centra zabezpečení [funkcím zjišťování](security-center-detection-capabilities.md). 

Když Centrum zabezpečení identifikuje nebezpečí, spustí k [Upozornění zabezpečení](security-center-managing-and-responding-alerts.md), která obsahuje podrobné informace týkající se zvláštní události, včetně návrhů remediation. Pomáhat incidentu týmy prozkoumat a napravovat hrozeb Centrum zabezpečení obsahuje hrozbou, že intelligence sestavu, která obsahuje informace o hrozbou, že byl zjištěn, včetně informací, jako: 

- Přidružení (pokud tyto informace jsou k dispozici) nebo identity uživatele
- Útočníků cíle
- Útok aktuálních a historických kampaní (pokud tyto informace jsou k dispozici)
- Útočníků taktik nástroje a postupy
- Přidružená indikátory narušení zabezpečení (IoC) například adresy URL a soubor hash
- Victimology, což je odvětví a zeměpisné rozšíření při zjištění, zda jsou Azure zdroje na rizika
- Informace o omezení rizik a remediation

>[AZURE.NOTE] Množství informací v každé určité zprávy se budou lišit; úrovně podrobností o je na základě malware aktivity a rozšíření.

Centrum zabezpečení má tři typy hrozbou, že sestav, které se může lišit podle útok. Jsou k dispozici sestavy:

- **Sestava skupiny činností**: poskytuje hloubkové dives do útočníků svých cílů a taktik.
- **Sestava kampaně**: se zaměřuje na stránce Podrobnosti konkrétní útok kampaní. 
- **Sestava Souhrn hrozbou**: zahrnuje všechny položky v předchozích dvou sestav.

Tento typ informací je velmi užitečné během [incidentu odpověď](security-center-incident-response.md) , kde není probíhajícího vyšetřování pochopit zdroj útok navštívili podnětů a akce zmírnit tento problém chovaly. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Jak získat přístup k sestavě intelligence hrozbou, že?

Zkontrolujte nastavení upozornění v aktuálním pohledem na dlaždici **výstrah zabezpečení** . Otevřete portál Azure a postupujte podle návodu zobrazíte další informace o jednotlivých upozornění:

1. Na řídicím panelu Centra zabezpečení zobrazí se dlaždici **výstrah zabezpečení** .

2. Klikněte na dlaždici otevřete zásuvné **výstrah zabezpečení** , která obsahuje další podrobnosti o oznámení a pak ve výstraze zabezpečení, které chcete získat další informace o.

    ![Upozornění zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. V tomto případě zásuvné **podezřelé proces spouštět** zobrazí podrobnosti o oznámení jak je znázorněno na následujícím obrázku:

    ![Detais upozornění zabezpečení](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Množství informací k dispozici pro každý výstrahy zabezpečení se liší podle typu upozornění. V poli **sestavy** obsahovat odkaz na hrozbou, že analytických nástrojů sestavy. Klikněte na ni a jiné okno prohlížeče se zobrazuje na PDF soubor.

    ![Výběr úložiště](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Tady můžete stáhnout soubor PDF tato zpráva a přečtěte si další informace o zabezpečením, který byl zjištěn a akcím podle informací, které.

## <a name="see-also"></a>Viz taky

V tomto dokumentu jste se naučili, jak můžou pomoct Azure zabezpečení Centrum hrozbou, že inteligentní sestavy během vyšetřování o výstrah zabezpečení. Další informace o Centru zabezpečení Azure, najdete v těchto článcích:

- [Časté otázky k Centru zabezpečení azure](security-center-faq.md). Najdete odpovědi na nejčastější dotazy týkající se používání služby.
- [Využívání Centrum zabezpečení Azure incidentu odpověď](security-center-incident-response.md)
- [Funkce rozpoznávání Centrum zabezpečení Azure](security-center-detection-capabilities.md)
- [Centrum zabezpečení azure plánování a operace Průvodce](security-center-planning-and-operations-guide.md). Informace o plánování a návrh Principy přijmout Azure Centrum zabezpečení.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md). Naučte se spravovat e-maily a odpovídat na výstrahy zabezpečení.
- [Zpracování incidentu zabezpečení v Centru zabezpečení Azure](security-center-incident.md)
- [Blog azure zabezpečení](http://blogs.msdn.com/b/azuresecurity/). Stát, že příspěvky Azure zabezpečení a dodržování předpisů.
