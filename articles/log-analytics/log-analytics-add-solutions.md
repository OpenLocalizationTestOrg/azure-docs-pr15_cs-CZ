<properties
    pageTitle="Přidání protokolu analýzy řešení z Galerie řešení | Microsoft Azure"
    description="Řešení analýzy protokolu jsou že kolekce použití logických operátorů, vizualizace a získávání dat pravidel, které poskytují metriky sklopí okolo části určitého problému."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Přidání protokolu analýzy řešení z Galerie řešení

Řešení analýzy protokolu jsou kolekce **Použití logických operátorů**, **vizualizace** a **pravidel pro získání dat** , které jsou zdrojem metriky sklopí okolo části určitého problému. Tento článek seznamy řešení podporuje protokol analýzy a je uvedeno, jak přidávat a odebírat pomocí Galerie řešení.

Řešení umožňují hlubší přehledy pro:

- Prozkoumejte a řešení provozní problémy rychleji
- shromažďování a sladit různými typy dat počítače
- můžete být aktivní s činnostmi, jako je plánování kapacity, vykazování stavu opravy a auditování zabezpečení.


>[AZURE.NOTE] Protokol analýzy obsahuje funkci hledání protokolu není potřeba nainstalovat řešení od jiných ji povolit. Vizualizace dat, navrhovaná hledání a další informace můžete získat však přidáním řešení z Galerie řešení.

Po přidání řešení, data jsou odebrané servery v infrastrukturu a odeslané ke službě OMS. Zpracování OMS službě obvykle trvá, než několik minut za hodinu. Po službu zpracuje data, můžete je zobrazit v OMS.

Řešení můžete jednoduše odebrat, pokud již není potřeba. Když odeberete řešení, její data nejsou odesílány OMS, které můžete snížit množství dat používaných denní kvótu, pokud nemáte.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Řešení nepodporuje Microsoft Agent sledování

V současné době můžete servery, které jsou připojené k OMS pomocí Microsoft Agent sledování nejčastěji řešení k dispozici, včetně:

- Hodnocení služby Active Directory
- Upozornění správy (bez oznámení SCOM)
- Antimalware
- Sledování změn
- Zabezpečení
- Hodnocení SQL
- Aktualizace systému

Následující řešení jsou však *podporovány u Microsoft Agent sledování a vyžadovat agent System Center operace správce (SCOM)* .

- Upozornění správy (včetně SCOM upozornění)
- Správa kapacity
- Konfigurace hodnocení

Podívejte se do [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md) informace o připojení agenta SCOM protokolu analýzy.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Chcete-li přidat řešení pomocí Galerie řešení

1. Na stránce Přehled OMS klikněte na dlaždici **Galerie řešení** .    
    ![Galerie řešení](./media/log-analytics-add-solutions/sol-gallery.png)
2. Na stránce Galerie řešení OMS informace o všech dostupných řešení. Klikněte na název řešení, které chcete přidat do OMS.
3. Na stránce řešení, které jste se rozhodli zobrazí se podrobné informace o řešení. Klikněte na **Přidat**.
4. Nová dlaždice řešení, které jste přidali, zobrazí se na stránku v OMS a můžete začít s ním pracovat po službu OMS zpracuje dat přehledu.

## <a name="to-configure-solutions"></a>Konfigurace řešení
1. Musíte nakonfigurovat některé řešení. Například musíte nakonfigurovat automatické obnovení webu Azure a zálohování, než budete moct použít.
2. Některý z těchto řešení klikněte na její dlaždici na stránce Přehled.  
    ![konfigurace řešení](./media/log-analytics-add-solutions/configure-additional.png)
3. Potom konfigurovat řešení potřebné informace a potom klikněte na **Uložit**.  
    ![konfigurace řešení](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Chcete-li odebrat řešení pomocí Galerie řešení

1. Na stránce Přehled OMS klikněte na dlaždici **Nastavení** .
2. Na stránce nastavení klikněte na kartě řešení, klikněte na **Odebrat** řešení, které chcete odebrat.
3. V potvrzovacím dialogovém okně klikněte na **Ano** odebrat řešení.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Podrobnosti kolekce dat pro OMS funkce a řešení

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje pro OMS funkce a řešení. Přímé agentů a SCOM agentů jsou v podstatě stejně, však přímé agent obsahuje další funkce povolit připojení do prostoru OMS a směrujete přes na proxy server. Pokud používáte agenta SCOM, musíte určený jenom jako agent OMS komunikovat s OMS. SCOM agenti v této tabulce jsou OMS agentů připojených k SCOM. Informace o připojení existující prostředí SCOM OMS naleznete v tématu [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md) .

>[AZURE.NOTE] Typ agent, který používáte Určuje, jak odesílaly OMS, pomocí následujících podmínek:

- Můžete použít přímé agent nebo agenta připojené SCOM OMS.
- Po požadované SCOM SCOM agent řešení vždy odesílaly OMS pomocí SCOM Správa skupiny. Kromě toho po požadované SCOM pouze agent SCOM používanou řešení.
- Když v tabulce jsou uvedeny, že SCOM agent odesílaly OMS pomocí skupiny správy SCOM není povinný, pak SCOM agent vždy odesílaly OMS pomocí Správa skupin. Přímé agentů vyhnout skupiny Správa a odešlete data přímo OMS.
- Když SCOM agent dat není odesílá provedením řídicí skupiny, pak data odeslaný přímo do OMS – vynechání skupině správy.


|datový typ| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|---|
|AD hodnocení|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 dnů|
|Stav replikace AD|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 dní|
|Upozornění (Nagios)|Linux|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|Při příchodu|
|Upozornění (Zabbix)|Linux|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minuta|
|Upozornění (Operations Manager)|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minuty|
|Antimalware|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| každou hodinu|
|Správa kapacity|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| každou hodinu|
|Sledování změn|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| každou hodinu|
|Sledování změn|Linux|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|každou hodinu|
|Konfigurace hodnocení (starší verze Advisor)|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| dvakrát denně|
|TRASOVÁNÍ UDÁLOSTÍ PRO WINDOWS|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Protokoly služby IIS|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Klíč trezorů|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Sítě aplikace brány|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Skupiny zabezpečení sítě|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Office 365|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|oznámení o|
|Výkonnosti|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|podle plánu, minimální 10 sekund|
|Výkonnosti|Linux|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|podle plánu, minimální 10 sekund|
|Služba struktury|Windows|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Hodnocení SQL|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 dnů|
|SurfaceHub|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|Při příchodu|
|Syslog|Linux|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|z Azure úložiště: 10 minut; z agenta: při příchodu|
|Aktualizace systému|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| alespoň na úrovni 2 krát za den a 15 minut po instalaci aktualizace|
|Protokoly událostí Windows zabezpečení|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| Azure úložištěm: 10 minut; Agent: při příchodu|
|Protokoly brána firewall systému Windows|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| Při příchodu|
|Protokoly událostí systému Windows|Windows|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)| Azure úložištěm: 1 min. Agent: při příchodu|
|Drátěný dat|Windows (2012 R2 / 8.1 nebo novější)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ano](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| každou 1 minutu|

## <a name="log-analytics-preview-solutions-and-features"></a>Protokol analýzy náhled řešení a funkce

Spuštěním služby a postupy devops budou moct partnera se zákazníky se dají funkce a řešení.

Během soukromé zobrazení náhledu nám předat malou skupinou zákazníci přístup nejdříve možné provádění funkce nebo řešení a získat názory vylepšování. Tato nejdříve možné implementace má minimální a provozní funkce.

Naším cílem je rychle vyzkoušejte věcí, abychom našli co funguje a co nefunguje. Iteraci jsme tento proces zpětné vazby od zákazníků soukromé náhled informuje nás, že je připraven k public preview, dokud.

Během veřejné náhledu jsme zpřístupnit funkce nebo řešení pro všechny uživatele na základě názorů a ověřit naše měřítko a efektivity. V této fázi:

- Funkce náhledu se zobrazí na kartě nastavení a lze povolit tak, že všichni uživatelé
- Náhled řešení dají přidat s použitím galerii nebo pomocí publikované skriptu

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Co mám vědět o funkce a řešení?

Připojte se k oslavám o nových funkcích a řešení a budeme používat pracovat s vámi se dají je.

Funkce a řešení nejsou vyhovuje všem, proto před chce připojit soukromé náhled nebo povolení veřejné náhledu zkontrolujte, jestli OK pracujete s něco, co je ve vývoji.

Při aktivaci funkce Náhled prostřednictvím portálu, který bude zobrazeno upozornění s oznámením, že tato funkce je v náhledu.

#### <a name="for-both-private-and-public-preview"></a>*Soukromé* a *veřejné* (verze Preview)

Platí následující pravidla veřejných a privátních náhledy:

- Co je vždy nefungují správně.
  - Problémy s sahají od právě menší obtěžování hlukem prostřednictvím na něco nefunguje vůbec
- Je možné pro náhled mít negativní vliv na systém / prostředí
  - Chcete-li předejít záporné věci, které používáte s OMS, ale někdy neočekávané akce děje se systémy pokusíme
- Ztrátou dat / může dojít k poškození
- Nemůžeme vás může požádat o shromáždit protokoly pro diagnostiku nebo jiná data pomoci při řešení problémů
- Funkce nebo řešení mohou být odebrány (dočasně nebo trvale)
  - Podle našeho learnings během náhled rozhodneme není uvolnění funkce nebo řešení
- Náhledy nemusí fungovat nebo nemusí testováno s všechny konfigurace a můžeme omezit:
  - Operační systémy, které lze použít (například funkci může použijí jenom u Linux ve verze preview)
  - Typ agent (MMA, SCOM), kterou budete moct použít (například funkci, nemusí pracovat SCOM ve verze preview)  
- Náhled řešení a funkce se nevztahuje Smlouva o úrovni služeb
- Použití funkce náhledu je možné za nadbytečným poplatkům
- Funkce nebo možnosti pro funkci potřebujete / řešení je užitečné asi nebyla nalezena nebo neúplné
- Funkce / řešení nemusí být k dispozici ve všech oblastech
- Funkce / řešení nemusí být lokalizované
- Funkce / řešení pravděpodobně omezení počtu zákazníci nebo zařízení, které můžete použít
- Budete muset pomocí skriptů provádět konfigurace a povolit řešení/funkci
- V uživatelském rozhraní (UI) nebude dokončena a může změnit den
- Veřejné náhledy nemusí být vhodné pro výrobní / kritické systémy

#### <a name="for-private-preview"></a>*Soukromé* (verze Preview)

Kromě výše uvedených položek je specifické pro soukromé náhledy:

- Abyste poskytnout nám svůj názor na prostředí tak, aby jsme můžete zlepšit funkci/řešení očekávat
- Můžeme můžete kontaktovat k získání názorů pomocí zjišťování, telefonní hovory nebo e-mailu
- Co nefunguje vždy správně
- Společnost Microsoft může vyžadovat jiné zpřístupnění smlouvě o utajení účasti nebo může zahrnovat důvěrné obsahu
  - Před blogů, klientské nebo jinak komunikaci s jinými výrobci zkontrolujte, zda se správcem programů zodpovědný za náhled pochopit omezení zpřístupnění
- Nejde spustit výrobní / kritické systémy


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Jak získat přístup k funkcím soukromé náhled a řešení?

Doporučujeme zákazníkům soukromé náhledy pomocí několika různými způsoby podle toho, náhled.

- Odpovídání na měsíční průzkum zákazníka a sdělte nám oprávnění ke zpracování s vámi zvyšuje vaši šanci pozvaný soukromé náhled.
- Váš tým účet Microsoft můžete jmenovat jste.
- Můžete se přihlásit na základě podrobností napsali na twitter [msopsmgmt](https://twitter.com/msopsmgmt)
- Můžete se přihlásit na základě podrobnosti událostí sdílené komunity – podívejte nám schůzky otevíraná konferencí a komunit online.


## <a name="next-steps"></a>Další kroky

- [Hledání protokoly](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
