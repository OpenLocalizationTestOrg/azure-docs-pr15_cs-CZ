<properties
   pageTitle="Použití Centra zabezpečení Azure incidentu odpověď | Microsoft Azure"
   description="Tento dokument vysvětluje, jak pomocí Centra zabezpečení Azure scénář incidentu odpověď."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Použití Centra zabezpečení Azure incidentu odpověď
Mnoho organizací zjistěte, jak reagovat na události zabezpečení až po některou útok. Snížit náklady a poškození, je důležité, abyste měli odpověď incidentu plánování místě před útok uplatněním. Centrum zabezpečení Azure můžete použít v různých fází incidentu odpověď.

## <a name="incident-response-planning"></a>Plánování incidentu odpověď

Efektivní plán závisí na tři hlavní funkce: tom mít možnost zamknout, zjišťování a odpovídání na hrozeb. Ochrana je o předcházení incidentem, zjišťování se o zjišťování hrozeb nejdříve možné, a odpovědi týkající se jeho vyloučení mohl a obnovení systémy zmírnit důsledky o porušení.

Tento článek použije fází incidentu zabezpečení v článku [Microsoft Azure zabezpečení odpovědi v cloudu](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) , jak je znázorněno na následujícím obrázku:

![Vyřešení incidentu odpověď cyklus](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Pomocí Centra zabezpečení fázích rozpoznat, vyhodnotit a Diagnostika. Tady jsou příklady jak Centrum zabezpečení mohou být užitečné třech fázích počáteční incidentu odpověď:

- **Rozpoznat**: Zkontrolujte první označení vyšetřování události.
    - Příklad: revize počáteční ověření, že výstrahy zabezpečení vysokou prioritou aktivované v řídicím panelu Centra zabezpečení.
- **Vyhodnotit**: provedení zhodnocení získat další informace o podezřelé aktivity.
    - Příklad: Získejte další informace o upozornění zabezpečení.
- **Diagnostika**: provede technické vyšetřování a určení strategie pro omezení, omezení rizik a řešení.
    - Příklad: postupujte remediation označená Centrum zabezpečení v této konkrétní zabezpečení upozornění.

Scénář, který následuje ukazuje, jak můžete využít Centrum zabezpečení rozpoznat, vyhodnotit a diagnostikování a odpovědět na ni fázích incidentu zabezpečení. V Centru zabezpečení je [zabezpečení incidentu](security-center-incident.md) agregaci všechna upozornění zdroje, které zarovnání vzorky [Ukončit řetězce](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Dlaždice [výstrah zabezpečení](security-center-managing-and-responding-alerts.md) a zásuvné zobrazí incidentem. K incidentu zobrazíte seznam související upozornění, která umožňuje získat další informace o každém výskytu. Centrum zabezpečení také nabídne vám samostatného upozornění zabezpečení, které lze také vysledovat podezřelé aktivity.

## <a name="scenario"></a>Scénář

Contoso naposledy některé ze svých místních zdrojů do migrovat Azure, včetně některé na základě virtuálního počítače pracovního vytížení řádek firmy a databáze SQL. V současné době incidentu odpověď týmu (CSIRT) systému společnosti Contoso Core počítač zabezpečení došlo k potížím vyšetřování potíže se zabezpečením z důvodu intelligence zabezpečení nejsou integrovaný s jejich aktuální nástroje incidentu odpověď. Tento chybějící integrace se objevuje problém ve fázi rozpoznat (příliš mnoho falešně pozitivní) i fázích vyhodnotit a Diagnostika. Jako součást této migrace rozhodli vyjádření výslovného souhlasu pro Centrum zabezpečení pomůžou tento problém vyřešit.

První fázi této migrace dokončení po jejich onboarded všechny zdroje a všechny zabezpečení doporučení z centra zabezpečení. Contoso CSIRT je kontaktní místo týkající se zabezpečení počítač událostí. Tým se skládá ze skupiny tělesně povinnosti týkající se všechny události zabezpečení. Členové týmu mít jasně definované povinností zajistit, že ještě zbývá žádné oblasti odpověď nezakrytým.

Pro účely tento scénář chceme fokus na role následující osoby, které jsou součástí Contoso CSIRT:

![Vyřešení incidentu odpověď cyklus](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Petr se operace zabezpečení. Svůj povinnostem patřit:

- Sledování zpráv a reagování na rizika zabezpečení nepřetržitě.
- Escalating cloudu pracovní zátěž vlastník nebo zabezpečení, analytik podle potřeby.

Je správce zabezpečení účtů, analytik zabezpečení a jeho povinnostem patřit:

- Zkoumání útoky.
- Stav upozornění.
- Práce s pracovní zátěž vlastníci určit a použít mitigations.

Jak vidíte, Petry a Sam mají různé povinnosti a musí vzájemně ladily a sdílení informací v Centru zabezpečení.

## <a name="recommended-solution"></a>Doporučené řešení

Protože Petry a Sam mají různé role, budou budete používat různých oblastí v Centru zabezpečení získat příslušné informace o každodenní aktivity. Petr použije **výstrah zabezpečení** v rámci svého denní sledování.

![Upozornění zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Petr použije výstrah zabezpečení fázích rozpoznat a vyhodnotit. Po dokončení počáteční hodnocení Petry uživatel může ohlásit Sam Pokud požaduje další vyšetřování. V tomto okamžiku Sam použije informace, které poskytnul Centrum zabezpečení někdy ve spojení s jiných zdrojů dat, přejděte do oblasti Diagnostika.


## <a name="how-to-implement-this-solution"></a>Jak implementovat toto řešení

Zjistěte, jak by pomocí Centra zabezpečení Azure ve scénáři incidentu odpověď, jsme budete postupně rozpoznat a vyhodnotit postupujte podle kroků na Petry a potom najdete v článku Co znamená Sam diagnostikovat potíže.

### <a name="detect-and-assess-incident-response-stages"></a>Zjištění a posuďte incidentu fáze

Petr přihlášení k portálu Azure a pracuje v konzole Centrum zabezpečení. V rámci svého denní sledování aktivity mohla začít revizí výstrah zabezpečení vysokou prioritou provedením následujících kroků:

1. Klikněte na dlaždici **výstrah zabezpečení** a přístup k zásuvné **výstrah zabezpečení** .
    ![Zásuvné upozornění zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Pro účely tento scénář Petry bude provádět hodnocení upozornění aktivity škodlivým SQL, jak je vidět na předchozím obrázku.
2. Klikněte na upozornění na **škodlivým SQL aktivity** a zkontrolujte napaden zdrojů v zásuvné **škodlivým SQL aktivity** :  ![podrobnosti o události](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    V tomto zásuvné Petry poznámky můžete vytvářet týkající se napaden zdrojů, jak se opakovaně pokoušeli útok stalo a kdy byl zjištěn.
3. Klikněte **útoku zdroje** získat další informace o útok.

Po přečtení popis, je Petry přesvědčeny, že se nejedná o falešně pozitivní a, mohla by měl odešlete tento případ do Sam.

### <a name="diagnose-incident-response-stage"></a>Diagnostika incidentu fáze

SAM přijímání případ Petry a spustí revizí nápravné kroky, které doporučovány Centrum zabezpečení.

![Vyřešení incidentu odpověď cyklus](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Další zdroje informací

Vyřešení incidentu odpověď týmu taky využít [Power BI Centrum zabezpečení](security-center-powerbi.md) možnost zobrazit různé typy sestav. Tyto sestavy můžete pomůžou při další vyšetřování jejich vizualizace, analýza a filtrace doporučení a výstrahy zabezpečení. Organizacím, které používají jeho informací o zabezpečení a řešení pro správu (SIEM) události během procesu vyšetřování mohou také [začlenit Centrum zabezpečení s jejich řešení](security-center-integrating-alerts-with-log-integration.md). Můžete taky integrovat Azure auditování protokoly událostí zabezpečení virtuálního počítače (OM) pomocí [Nástroje pro integraci Azure protokolu](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). Pokud chcete zkontrolovat útok, můžete tyto informace ve spojení s informací, které poskytuje Centrum zabezpečení.


## <a name="conclusion"></a>Uzavření

Sestavení týmu před incident je velmi důležité pro vaši organizaci a jednoznačně ovlivnit způsob zpracování incidentem. Správným nástrojům sledování zdroje můžete pomocí výstižných tento tým udělat přesné kroky k nápravě incidentu zabezpečení. Centrum zabezpečení [funkcím zjišťování](security-center-detection-capabilities.md) pomůže IT můžete rychle odpovědět na incidentem zabezpečení a nápravě potíže se zabezpečením.
