<properties
   pageTitle="Operace správy sadu zabezpečení a auditování řešení podle směrného plánu | Microsoft Azure"
   description="Tento dokument vysvětluje, jak používat OMS zabezpečení a auditování řešení provádět podle směrného plánu hodnocení všechny sledované počítačů za účelem dodržování předpisů a zabezpečení."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Hodnocení podle směrného plánu do operace správy sadu zabezpečení a auditování řešení

Tento dokument pomáhá při používání [operace správy sady (OMS) zabezpečení a auditování řešení](operations-management-suite-overview.md) funkcí hodnocení podle směrného plánu pro přístup k zabezpečené stav sledované zdrojů.

## <a name="what-is-baseline-assessment"></a>Co je podle směrného plánu zhodnocení?

Microsoft, spolu s odvětví a pro státní správu organizace Celosvětová dostupnost, definuje konfigurace systému Windows, který představuje nasazení vysoce zabezpečeného serveru. Toto nastavení je sada klíče registru, nastavení zásad auditování a nastavení zásad zabezpečení spolu s společnosti Microsoft doporučené tato nastavení. Této sady pravidel se nazývá zabezpečení podle směrného plánu. Hodnocení možností OMS zabezpečení a auditování směrný plán můžete Bezproblémová prohledávat všechny počítače se zákonnými požadavky. 

Existují tři typy pravidel:

- **Pravidla registru**: Zkontrolujte, jestli je správně nastavený klíče registru.
- **Pravidla zásad auditování**: pravidla týkající se nastavení zásad auditování.
- **Pravidla zásady zabezpečení**: pravidla týkající se oprávnění uživatele v počítači.

> [AZURE.NOTE] Přečtěte si [Použití OMS zabezpečení a posuďte základ konfigurace zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) stručný přehled tuto funkci.

## <a name="security-baseline-assessment"></a>Hodnocení zabezpečení podle směrného plánu

Můžete zkontrolovat vaše aktuální hodnocení podle směrného plánu zabezpečení u všech počítačů, které jsou kontroloval OMS zabezpečení a auditování pomocí řídicího panelu.  Provést následující kroky pro přístup k řídicího panelu zhodnocení směrný plán zabezpečení:

1. V hlavním řídicím panelu **Microsoft operace správy Suite** klikněte na dlaždici **zabezpečení a auditování** .
2. Na řídicím panelu **zabezpečení a auditování** klikněte v části **Zabezpečení domény** **Zhodnocení podle směrného plánu** . Řídicí panel **Zabezpečení podle směrného plánu hodnocení** se zobrazí, jak je znázorněno na následujícím obrázku:
    
    ![OMS zabezpečení a auditování podle směrného plánu hodnocení](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Tento řídicích panelů se dělí ve třech hlavních oblastech:

- **Počítačů ve srovnání se směrným plánem**: Tato část nabízí přehled počet počítačů, které byly přístupné a procento počítačů, které předávají funkcím hodnocení. Hodnocení dále poskytuje horní 10 počítačů a výsledek procentuální hodnotu.
- **Povinné pravidla stav**: Tento oddíl obsahuje záměr Dostávejte povědomí o nezdařeném uložení pravidla závažnosti a pravidla podle typu se nezdařila. Zobrazením první grafu můžete rychle zjistit, zda většinu selhalo pravidel je považován za kritický, nebo ne. Dále poskytuje seznam horní 10 selhalo pravidel a závažnosti. V druhém grafu zobrazí typ pravidla, která způsobila nezdařenou hodnocení. 
- **Počítačů chybějící podle směrného plánu hodnocení**: v této části Seznam počítačů, které nebyly přístupné kvůli potížím s kompatibilitou operačního systému nebo k chybám. 

### <a name="accessing-computers-compared-to-baseline"></a>Přístup k počítači ve srovnání s podle směrného plánu

V ideálním případě počítače jsou všechna vyhovovat hodnocení zabezpečení podle směrného plánu. Ale se nestane očekává se, že v některých okolnosti to. Jako součást procesu správy zabezpečení je důležité zahrnout revizí počítačů, které se nepodařilo předat všechny testy hodnocení zabezpečení. Rychlý způsob, jak vizualizace, která je tak, že vyberete možnost **přistupovat počítače** najdete v části **počítačů ve srovnání s podle směrného plánu** . Měli byste vidět ve výsledcích hledání protokolu seznamem počítačů, jak je vidět na následující obrazovce:

![Výsledky počítače přístup](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Výsledek hledání se zobrazují ve formátu tabulky, kde první sloupec má název počítače a druhá barva počet pravidla, která se nezdařila. Získat informace o typu pravidla, které se nepodařilo, klikněte do pole počet neúspěšných pravidla vedle názvu počítače. Měli byste vidět výsledek podobný takové, jaké vidíte na následujícím obrázku:

![Podrobnosti výsledky počítače přístup](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Tento výsledek hledání vám dává celkový součet přístup pravidla, počet kritické pravidel, která se nepodařilo, pravidla upozornění a pravidla informací se nezdařila.

### <a name="accessing-required-rules-status"></a>Přístup k stavu požadované pravidel

Po získání informací týkajících se procentuální hodnota číslo počítačů, které předávají funkcím zhodnocení, můžete získat další informace o tom, které se nedaří pravidla podle sytému. Tento vizualizace umožňuje určit jejich prioritu počítače, které by měl určena nejdřív zkontrolujte, zda že budou požadavkům další hodnocení. Najeďte myší na důležitou součástí grafu umístěn v této dlaždici **se nepodařilo pravidel závažnosti** , klikněte v části **povinné stav pravidla** a klikněte na něj. Měli byste vidět výsledek podobně jako na následující obrazovce:

![Prostřednictvím závažnosti podrobnosti se nezdařila pravidel](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

V protokolu výsledek zobrazí typ pravidla podle směrného plánu, které se nepodařilo popis toto pravidlo a běžných konfigurace výčet (CCE) ID tohoto pravidla zabezpečení. Tyto atributy by měly být tak, abyste provedení korekční tento problém v cílovém počítači.

> [AZURE.NOTE] Další informace o CCE přístup [Vnitrostátní chybu databáze](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Přístup k počítači chybějících hodnocení podle směrného plánu

OMS podporuje profil podle směrného plánu člena domény v systému Windows Server 2008 R2 až Windows serveru 2012 R2. Směrný plán Windows serveru 2016 ještě není konečný a přidá hned, jak je publikován. Všechny ostatní operační systémy naskenovaný prostřednictvím OMS zabezpečení a auditování zhodnocení podle směrného plánu se zobrazí v části **počítačů chybějící zhodnocení podle směrného plánu** .

## <a name="see-also"></a>Viz taky

V tomto dokumentu seznámili OMS zabezpečení a auditování hodnocení podle směrného plánu. Další informace o OMS zabezpečení, naleznete v následujících článcích:

- [Přehled správy sady (OMS) operace](operations-management-suite-overview.md)
- [Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení](oms-security-responding-alerts.md)
- [Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení](oms-security-monitoring-resources.md)

