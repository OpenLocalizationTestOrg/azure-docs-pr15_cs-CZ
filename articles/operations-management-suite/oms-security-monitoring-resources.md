<properties
   pageTitle="Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení | Microsoft Azure"
   description="Tento dokument vám pomůže při použití OMS zabezpečení a auditování funkce ke sledování svých prostředcích a identifikovat potíže se zabezpečením."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení

Tento dokument umožňuje používat OMS zabezpečení a funkcí auditování sledovat zdroje a identifikovat potíže se zabezpečením.

## <a name="what-is-oms"></a>Co je OMS?

Microsoft operace správy sady (OMS) je cloudu společnosti Microsoft na základě řešení pro správu IT, který vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury. Další informace o OMS, přečtěte si článek [Operace správy sadu](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Sledování zdroje

Pokaždé, když je možné, budete chtít, aby zabezpečení incidentem neopakoval na prvním místě. Je však možné nechcete, aby všechny zabezpečení incidentem. Při incidentu zabezpečení, je třeba zajistit, že je minimalizovaná jeho vliv.  Existují tři kritické doporučení, které můžete použít k minimalizaci číslo a jejich dopad incidentem zabezpečení:

- Běžně posuďte chyby ve vašem prostředí.
- Pravidelně kontrolovat všech počítačů a síťových zařízení, aby měli všechny nejnovějších aktualizací nainstalovaný.
- Kontrola pravidelně všechny protokoly a mechanismy protokolování, včetně operační systém událostí protokoly, konkrétní protokoly aplikací a průnik zjišťování systému.

OMS zabezpečení a auditování řešení umožňuje IT aktivně sledovat všechny zdroje, které pomáhají minimalizovat vliv zabezpečení incidentem. OMS zabezpečení a auditování má zabezpečení domény které slouží ke sledování zdroje. Domény zabezpečení obsahuje rychlý přístup možností pro sledování zabezpečení, které následující domény bude předmětem další podrobnosti:

- Hodnocení malwaru
- Aktualizovat hodnocení
- Identita a přístup

> [AZURE.NOTE] Přehled o všech možnostech přečtěte si [Začínáme s operace správy sadu zabezpečení a auditování řešení](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Ochrana sledování systému

V obrana při přístupu název hloubkové je důležité pro celkového stavu zabezpečení vaší majetku každý další úroveň ochrany. Počítačích s zjištěné hrozeb a počítačů s ochranou nedostatečná jsou vidět v této dlaždici Malware hodnocení v části zabezpečení domény. Pomocí informací o hodnocení Malware můžete určit plán chcete servery, které potřebují ho použít zámek. Přístup k tuto možnost, postupujte následujícím způsobem:

1. Na řídicím panelu hlavní **Microsoft operace správy Suite** klikněte na dlaždici **zabezpečení a auditování** .

    ![Zabezpečení a auditování](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Na řídicím panelu **zabezpečení a auditování** klikněte v části **Zabezpečení domény** **Antimalware zhodnocení** . Řídicí panel **Antimalware zhodnocení** vypadá takto:

![Hodnocení malwaru](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Řídicí panel **Malware hodnocení** můžete použít k identifikaci následující otázky zabezpečení:

- **Aktivní hrozeb**: počítačů, které byly ohroženo a aktivní hrozeb za systému.
- **Remediated hrozeb**: byly remediated počítačů, které byly ohroženo, ale rizika.
- **Aktuální podpis**: počítačů, které mají povoleno ochrana proti malwaru, ale podpisu je aktuální.
- **Žádné ochrany v reálném čase**: počítačů, ve kterých není nainstalovaný antimalware.

### <a name="monitoring-updates"></a>Sledování aktualizací 

Použití nejnovější aktualizace zabezpečení je vhodné, zabezpečení a by měl být součástí strategie řízení aktualizace. Služba Microsoft Agent sledování (HealthService.exe) přečte aktualizovat informace z sledované počítačů a potom odešle tyto aktualizované informace OMS službě v cloudu pro zpracování. Služba Microsoft sledování nakonfigurování jako automatické služby a by měl být vždy spuštěn v cílovém počítači.

![Sledování aktualizací](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Použití logických operátorů se použije aktualizace dat a cloudovou službu záznamů data. Pokud chybějící aktualizace najdete, se zobrazí na řídicím panelu **aktualizace** . Můžete na řídicím panelu **aktualizace** se dá pracovat s chybějícím aktualizace a sestavit plán použít k serverům, které potřebujete. Postupujte podle pokynů pro přístup k řídicím panelu **aktualizace** :

1. Na řídicím panelu hlavní **Microsoft operace správy Suite** klikněte na dlaždici **zabezpečení a auditování** .
2. Na řídicím panelu **zabezpečení a auditování** klikněte v části **Zabezpečení domény** **Aktualizovat hodnocení** . Řídicí panel aktualizace vypadá takto:

![Aktualizovat hodnocení](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

V tomto řídicího panelu lze provést zhodnocení aktualizace pomůže porozumět aktuální stav počítače a adresa nejdůležitější hrozeb. Pomocí dlaždici **kritický nebo aktualizace zabezpečení** správce IT bude moct přistupovat podrobné informace o aktualizacích, které nebyly nalezeny, jak je ukázáno v následujícím příkladu:

![výsledek hledání](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Tato zpráva obsahovat důležité informace, které mohou sloužit k identifikaci typu hrozbou, ke kterému se tento systém ohroženo, což zahrnuje znalostní báze Microsoft KB články související s aktualizace zabezpečení a MS Bulletin, který se má další podrobnosti o chybu.

### <a name="monitoring-identity-and-access"></a>Sledování identit a přístup

Uživatelům pracovat kdekoliv, pomocí různých zařízeních a přístup k velké množství cloudu a místní aplikací je nezbytné ochrany své přihlašovací údaje. Útoky krádež přihlašovací údaje jsou ve kterých se zlými úmysly původně získá přístup k běžný uživatel přihlašovací údaje pro přístup k systému v síti. Opakovaně pokoušeli útok počáteční jenom způsob, jak získat přístup k síti, je ultimate cílem je Seznamte se s oprávněními. 

Útočníků zůstanou v síti, pomocí volně dostupné nástroje extrahovat přihlašovací údaje z relací dalších účtů přihlášený. V závislosti na konfiguraci systému lze extrahovat těchto přihlašovacích údajů v podobě křížky, požadavky nebo hesla i ve formátu prostého textu.  

> [AZURE.NOTE] stroje, které jsou přímo vystaven na Internetu uvidí počet neúspěšných pokusů o pokusu o přihlášení pomocí všech typu známý uživatelská jména (například správce). Ve většině případů je v pořádku Jestliže nepoužívaná známý uživatelská jména a hesla se dostatečně.

Je možné stanovit tyto před neoprávněným před napadnout účet oprávnění. Můžete využít **OMS zabezpečení a řešení auditování** sledovat identit a přístup. Postupujte podle pokynů pro přístup k řídicím panelu **identit a přístupu** :

1. V hlavním řídicím panelu **Microsoft operace správy Suite** klikněte na zabezpečení a auditování dlaždice.
2. Na řídicím panelu **zabezpečení a auditování** klikněte v části **Zabezpečení domény** **identit a přístup** . Řídicí panel **identit a přístup** vypadá takto:

![identita a přístup](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Jako součást strategie běžná sledování musí obsahovat identity sledování. Správce IT by měl vypadat, pokud existuje konkrétní platné uživatelské jméno, které má mnoho pokusů. To může označení buď útočník, který získali skutečné uživatelské jméno a pokuste se hrubou platnost nebo nástroj pro automatické použití pevně heslo, které vypršela platnost.

Povolit tento řídicího panelu IT rychlá identifikace potenciální hrozeb související s identit a přístupu k prostředkům společnosti. Je určité důležité také identifikovat trendy potenciální, například v této dlaždici přihlášení v čase, uvidíte po dobu kolikrát pokus o přihlášení proběhlo. V tomto případě tlačítko **Souborový_server** doručeny 35 pokusy o přihlášení. Kliknutím na něj můžete prozkoumat další podrobnosti o tomto počítači. 

![Další informace](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Sestava vytvořený pro tento počítač přináší důležité podrobnosti o této vzorku. K tomu, že sloupec **účet** vám nabídne uživatelský účet, který jste použili k vyzkoušejte pro přístup k systému, ve sloupci **TIMEGENERATED** je uveden časový interval, ve kterém pokus platné a ve sloupci **LOGONTYPENAME** je uveden na místo, kde platné tento pokus. Pokud tyto pokusy prováděly místně v systému pomocí programu, sloupci **proces** by být tento proces zobrazeným názvem společnosti. V případech, kde je tu pokus o přihlášení z programu už máte název procesu k dispozici a teď můžete provést další vyšetřování v cílovém systému.

## <a name="see-also"></a>Viz taky

V tomto dokumentu se naučíte používat OMS zabezpečení a auditování řešení ke sledování svých prostředcích. Další informace o OMS zabezpečení, naleznete v následujících článcích:

- [Přehled správy sady (OMS) operace](operations-management-suite-overview.md)
- [Začínáme s operace správy sadu zabezpečení a auditování řešení](oms-security-getting-started.md)
- [Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení](oms-security-responding-alerts.md)