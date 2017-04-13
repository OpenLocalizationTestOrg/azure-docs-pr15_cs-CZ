<properties
   pageTitle="Začínáme s operace správy sadu zabezpečení a auditování řešení | Microsoft Azure"
   description="Tento dokument pomáhá Začínáme se operace správy sadu zabezpečení a funkce řešení auditování sledovat hybridní cloudu."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Začínáme s operace správy sadu zabezpečení a auditování řešení
Tento dokument vám pomůže začít rychle s možnostmi řešení operace správy sady (OMS) zabezpečení a auditování a provede vás jednotlivé možnosti.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft operace správy sady (OMS) je společnosti Microsoft cloudové IT řešení pro správu, který umožňuje spravovat a chránit vaše místní a cloudové infrastruktury. Další informace o OMS, přečtěte si článek [Operace správy sadu](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Řídicí panel OMS zabezpečení a auditování

Řešení OMS zabezpečení a auditování nabízí úplný zobrazení do vaší organizace IT zabezpečení postoje pomocí integrovaného vyhledávání dotazů důležitá problémy, které vyžadují pozornost. Řídicí panel **zabezpečení a auditování** je na domovské obrazovce pro všechno, co týkající se zabezpečení v OMS. Poskytuje uceleném přehled o stavu zabezpečení počítače. Obsahuje taky možnost zobrazit všechny události z posledních 24 hodin, 7 dní nebo jiných časové rozmezí vlastní. Přístup k řídicím panelu **zabezpečení a auditování** , postupujte takto:

1. Na řídicím panelu hlavní **Microsoft operace správy Suite** klikněte na **Nastavení** klikněte na levé straně.
2. V **Nastavení** zásuvné v části **řešení** vyberte možnost **zabezpečení a auditování** .
3. Řídicí panel **zabezpečení a auditování** se zobrazí:

    ![Řídicí panel OMS zabezpečení a auditování](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Pokud přistupujete tento řídicí panel pro první a nemáte zařízení kontroloval OMS, nebude mít údaje z agenta žádné dlaždice. Jakmile nainstalujete agent, může trvat dlouho k naplnění, proto se zobrazí původně by mohla chybět některá data jako pořád nahrávají do cloudu.  V tomto případě je normální zobrazíte několik dlaždic bez konkrétní informace. Přečtěte si [počítače se systémem Windows připojit přímo do OMS](https://technet.microsoft.com/library/mt484108.aspx) Další informace o tom, jak nainstalovat OMS zástupce v systému Windows a [počítačů připojení Linux OMS](https://technet.microsoft.com/library/mt622052.aspx) Další informace o tom, k provedení tohoto úkolu v systému Linux.

> [AZURE.NOTE] Agent shromáždí informace podle aktuální události, které jsou povolené, například název počítače, IP adresy a uživatelského jména. Ale žádné soubory a dokumentů, název databáze nebo soukromá data odebírají.   

Řešení jsou kolekce použití logických operátorů, vizualizace a dat WIA pravidla, která adresa klíčové zákazníků. Zabezpečení a auditování řešením je ostatním můžete přidat zvlášť. Přečtěte si článek [řešení přidat](https://technet.microsoft.com/library/mt674635.aspx) Další informace o tom, jak přidat nové řešení.

Řídicí panel OMS zabezpečení a auditování jsou uspořádána ve čtyřech hlavních kategorií:

- **Zabezpečení domén**: v této oblasti, budete moct dál prozkoumání zabezpečení záznamy v čase, přístup k posouzení malware, aktualizovat hodnocení zabezpečení sítě, informace identit a přístup, počítačích s události zabezpečení a rychle mít přístup k řídicí panel centra zabezpečení Azure.
- **Důležitá problémy**: tuto možnost vám umožní rychlá identifikace počet aktivní problémy a závažnosti těchto problémů.
- **Zjišťování (verze Preview)**: umožňuje identifikaci vzorů útok vizualizací výstrah zabezpečení přijmou vůči zdroje.
- **Riziko Intelligence**: umožňuje identifikaci vzorů útok vizualizací celkový počet servery s odchozí útokům IP, škodlivý hrozbou typ a mapování, které ukazuje, kde jsou tyto IP adresy pocházejících z. 
- **Běžné dotazy zabezpečení**: Tato možnost je k dispozici seznam nejčastější dotazy zabezpečení, které můžete použít ke sledování prostředí. Když kliknete na jednu z těchto dotazů, otevře zásuvné **vyhledávání** s výsledky tohoto dotazu.

> [AZURE.NOTE] Další informace o způsobu OMS zajistí ochranu dat přečtete v tématu že jak OMS zabezpečuje vaše data.

## <a name="security-domains"></a>Zabezpečení domén

Při sledování zdrojů, je důležité, abyste mohli rychle zobrazit aktuální stav prostředí. Také je ale důležité, abyste mohli sledovat zpět události, které v minulosti, která může vést k lepší znalost co je nového ve vašem prostředí v určitém bodě v čase. 

> [AZURE.NOTE] uchovávání informací je podle OMS ceny plán. Další informace získáte v [Microsoft operace správy Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) ceny stránky.

Vyřešení incidentu odpověď forenzních vyšetřování příklady použití přímo prospěch ve výsledcích k dispozici v této dlaždici **Zabezpečení záznamy určitou dobu** .

![Zabezpečení záznamů v čase](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Když kliknete na této dlaždici, zásuvné **hledání** se otevře, zobrazující výsledek dotazu pro **Události zabezpečení** (typ = SecurityEvents) s daty podle posledních sedmi dnů, jak je ukázáno v následujícím příkladu:

![Zabezpečení záznamů v čase](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Ve výsledcích hledání je rozdělen do dvou panelů: v levém podokně nabízí hierarchické počet událostí zabezpečení, které nebyly nalezeny počítačů, které nebyly nalezeny tyto události, počet účty, které nebyly zjištěny v těchto počítačů a typ činností. V pravém podokně poskytuje celkový počet výsledků a chronologickém zobrazení zabezpečení události s názvem a událostí aktivitou počítače. Můžete taky kliknout na **Zobrazit další** zobrazíte další informace o této události, jako jsou data události, ID události a zdroj události.

> [AZURE.NOTE] Další informace o OMS vyhledávacího dotazu přečtěte si [OMS hledání odkaz](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Hodnocení Antimalware

Tato možnost umožňuje rychle identifikovat počítačích s nedostatečná protection a počítačů, které jsou ohroženo část malware. Přečtěte si malware posuzování stavu a zjištěné hrozeb sledované serverech a odeslaný data OMS službě v cloudu pro zpracování. Servery s zjištěné hrozeb a servery s ochranou nedostatečná jsou vidět v řídicím panelu hodnocení malware přístupný po kliknutí na dlaždici **Antimalware hodnocení** . 

![hodnocení malwaru](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Stejně jako jakékoli jiné živou dlaždici k dispozici v řídicím panelu OMS když kliknete na něm, zásuvné **hledání** se otevře výsledek dotazu. Pro tuto možnost Pokud kliknete na tlačítko Možnosti **Není vytváření sestav** v části **Stav ochrany**bude potřeba, výsledek dotazu, který ukazuje tento jedné položky, která obsahuje název počítače a jeho pořadí, jak je ukázáno v následujícím příkladu:

![výsledek hledání](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *pořadí* je testu pojmenování tak, aby odrážely stav ochrany (, vypnutí, aktualizace, atd) a hrozeb, které se nacházejí. Problémy, jako číslo informujete agregace.

Když kliknete do pole název počítače, budete mít chronologickém zobrazení stavu ochrany pro tento počítač. Toto je velmi užitečné pro scénáře, ve které je potřeba porozumět, pokud antimalware nainstalovaný jednou a nastane okamžik, která byla odebrána.   

### <a name="update-assessment"></a>Aktualizovat hodnocení 

Tato možnost umožňuje rychle zjistit celkový vystavení potenciální problémy zabezpečení a jestli nebo jak kritické tyto aktualizace jsou ve vašem prostředí. OMS zabezpečení a auditování řešení jenom poskytnout vizualizace tyto aktualizace, skutečné data pocházejí z [Řešení aktualizací systému](https://technet.microsoft.com/library/mt484096.aspx), který je jiného modulu v rámci OMS. Tady je příkladem aktualizace:

![aktualizace systému](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Další informace o aktualizacích řešení přečtěte si [Aktualizovat servery s řešením aktualizace systému](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identita a přístup

Identita by měl být řídicí z vašeho podniku ochrana identity by mělo být vaše nejvyšší prioritu. I když v minulosti byly hranice kolem organizace a tyto hranice byly jednu primární obranný omezení, v současné době s víc dat a dalších aplikací přesunutí do cloudu identitu se změní na nové obvod. 

> [AZURE.NOTE] aktuálně data jsou založeny pouze na události zabezpečení přihlašovací údaje (událost ID 4624) v budoucích přihlášení Office365 a dat Azure AD by vás však započítávány.

Monitorování vašich aktivit najdete identity budete moct udělat aktivní před incident zabírá místo nebo informace akce zastavit pokus útok. Řídicí panel **identit a Access** nabízí přehled identity stavu, včetně počtu selhání pokusů o přihlášení, uživatelský účet, který jste použili při tyto pokusy, účty, které byly uzamčen, účty s změně nebo resetování hesla a aktuálně počet účty, které jsou přihlášení. 

Po kliknutí na dlaždici **identit a přístup** vidíte řídicím panelu takto:

![identita a přístup](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Informace dostupné v tomto řídicího panelu okamžitě vám mohou pomoci k identifikaci potenciální podezřelé aktivity. Například existuje 338 pokusy o přihlášení jako **Správce** a 100 % tyto pokusy se nezdařila. Příčin může být přímým útokům týkající se tohoto účtu. Když kliknete na tento účet bude získat další informace, který pomůže určit cílový prostředek potenciální útok:

![výsledky hledání](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Podrobné sestava poskytující důležité informace o této události, včetně: cílový počítač, zadejte přihlašovací (v tomto případu přihlášení do sítě), aktivitu (v tomto případě události 4625) a úplný časovou osu s jednotlivými pokusy. 

### <a name="computers"></a>Počítačů

Této dlaždici lze použít pro přístup k všech počítačů aktivně události zabezpečení. Když kliknete na této dlaždici uvidíte seznam počítačů s událostmi zabezpečení a počet události na každém počítači:

![Počítačů](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Můžete pokračovat svůj výzkum kliknutím na každém počítači a kontrola událostí zabezpečení, které byly příznakem.

### <a name="azure-security-center"></a>Centrum zabezpečení Azure

Této dlaždici je v podstatě zástupce na ploše pro přístup k řídicí panel centra zabezpečení Azure. [Začínáme s Centrum zabezpečení Azure](../security-center/security-center-get-started.md) přečíst další informace o tomto řešení.

## <a name="notable-issues"></a>Důležitá problémy

Hlavní záměr tuto skupinu možností je poskytovat rychlé zobrazení problémů, ke kterým máte ve vašem prostředí kategorizace kritický, upozornění a informační. Typ dlaždice aktivní problém je vizualizace z těchto problémů, ale neumožňuje, které můžete prozkoumat další podrobnosti o nich pro muset používat dolní části této dlaždici, který obsahuje název úkolu (jméno), kolik objekty kdyby to (počet) a jak důležité je (ZÁVAŽNOSTI).

![Důležitá problémy](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Uvidíte, že byly těchto problémů už zahrnutých v různých oblastech skupiny **Zabezpečení domény** , který posílí záměr toto zobrazení: vizualizace nejdůležitější problémy ve vašem prostředí z jednoho místa.

## <a name="detections-preview"></a>Zjišťování (verze Preview)

Hlavní záměr tato možnost je umožnit IT rychlá identifikace potenciální hrozeb jejich prostředí prostřednictvím a závažnosti toto riziko.

![Riziko Intel](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Tuto možnost lze také během vyšetřování incidentu k provádění hodnocení a získat další informace o útok.

> [AZURE.NOTE] Další informace o používání OMS incidentu odpověď podívejte se na [postup můžete využít Azure Centrum zabezpečení a Microsoft operace správy Suite pro odpověď incidentu](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Riziko měřítka

Nový oddíl intelligence hrozbou, že má toto řešení zabezpečení a auditování vizualizuje vzorek možné útok několika způsoby: Celkový počet servery s odchozí útokům IP, škodlivý hrozbou typ a mapování, které ukazuje, kde jsou tyto IP adresy pocházejících z. Můžete ovládat mapy a klikněte na IP adresy další informace.

Žlutá pushpins na mapě označení příchozích z nebezpečného IP adresy. Je běžné serverů, které jsou vystaven na Internetu zobrazíte příchozí útokům, ale doporučujeme revizí tyto pokusy o Ujistěte se, že žádný z existujících byl úspěšný. Tyto indikátory jsou založeny na protokoly služby IIS, WireData a protokoly brána Windows Firewall.  

![Riziko Intel](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Běžné dotazy zabezpečení

Seznam běžné dotazy zabezpečení dostupné může být užitečné pro můžete rychle zjistit informací o zdroji a upravit ho podle potřeby vašeho prostředí. Tyto běžné dotazy jsou:

- Všechny aktivity zabezpečení
- Zabezpečení aktivity na počítači "computer01.contoso.com" (nahradit názvu počítače)
- Zabezpečení aktivity na počítači "computer01.contoso.com" účtu "Správce" (nahradit vlastního názvu počítače a účtu)
- Přihlaste se aktivity ve počítače
- Účty, které ukončen Microsoft antimalware libovolného počítače
- Pokud byl ukončen procesu antimalware Microsoft počítačů
- Počítačů, kde "hash.exe" byl spuštěn (nahradit s názvem jiný proces)
- Všechny názvy procesů, které byly provedeny
- Přihlaste se aktivita účtu
- Účty, kteří vzdáleně přihlášení k lyncu na počítači "computer01.contoso.com" (nahradit názvu počítače)

## <a name="see-also"></a>Viz taky

V tomto dokumentu byly vydané OMS zabezpečení a auditování řešení. Další informace o OMS zabezpečení, naleznete v následujících článcích:

- [Přehled správy sady (OMS) operace](operations-management-suite-overview.md)
- [Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení](oms-security-responding-alerts.md)
- [Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení](oms-security-monitoring-resources.md)
