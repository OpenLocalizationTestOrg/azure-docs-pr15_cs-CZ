<properties
   pageTitle="Pokyny pro neobsahovaly text | Microsoft Azure"
   description="Pokyny k nastavení automatické měřítko dynamicky přidělení zdrojů vyžadované aplikací."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="autoscaling-guidance"></a>Pokyny pro neobsahovaly text

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Základní informace
Při minimalizace runtime náklady neobsahovaly text je proces dynamicky přidělení zdrojů vyžadované aplikací požadavky na výkon a splňovat smlouvy o úrovni služeb (rozsahu). Narůstající velikostí množství práce, aplikace může vyžadovat další zdroje informací umožňuje provádět úkoly včas. Jak služba slackens, může být zrušen minimalizovat náklady, pořád zachováním odpovídající výkon a schůzky rozsahu zdrojů.
Neobsahovaly text využívá pružnost hostující v cloudu prostředí při zjednodušení správy režijních. Stejně tak jejich zmenšením nutnosti operátor neustále sledovat výkon systému a rozhodovat o přidávání a odebírání zdroje.
>[AZURE.NOTE] Neobsahovaly text platí pro všechny zdroje, používané aplikaci, nejen pro využití zdrojů. Například, pokud váš počítač používá fronty zprávy odesílat a přijímat informace, ho bylo možné vytvořit další fronty změně velikosti.

## <a name="types-of-scaling"></a>Typy měřítka
Změna měřítka obvykle jen jednu z následujících dvou formulářů:

- **Svislý** (označovaná také jako _měřítka nahoru a dolů_). Tento formulář vyžaduje úpravy hardware (rozšířit či omezit jeho kapacitu a výkon), nebo přeinstalujte řešení pomocí alternativní hardware, který obsahuje příslušný kapacitu a výkon. V prostředí cloudu platformu hardwaru je obvykle virtualizovaném prostředí. Pokud podstatně overprovisioned původní hardwaru pomocí následným předem kapitálových výdajů, svisle škálování v tomto prostředí zahrnuje zřizování výkonnější zdroje a potom přesunutí systém do tyto nové zdroje. Změna měřítka svislé je často vyloučit proces, který vyžaduje díky systému a jeho opětovném nasazení dočasně nedostupný. Je možné zachovat původní systému a nový hardware máte k dispozici přenesený online, ale tam bude pravděpodobně některé přerušení při zpracování přechody ze starého prostředí do nového. Je běžné umožňuje provádět svislé měřítka strategii neobsahovaly text.
- **Vodorovné** (označovaná také jako _měřítko a_). Tento formulář vyžaduje nasazení řešení na další nebo méně zdroje, které jsou obvykle komoditou zdrojů, nikoli vysokým výkonem systémy. Řešení můžete pokračovat v práci bez přerušení během zřízení tyto materiály. Po dokončení procesu zřizovací kopií prvky tvořící řešení můžete nasadit na tyto další zdroje informací a k dispozici. Pokud vynechává služba další zdroje informací můžete znovu použít po prvky pomocí nich mají čistě vypnout. Mnoho cloudové systémy, včetně Microsoft Azure podporuje automatizaci tohoto formuláře nastavení velikosti.

## <a name="implement-an-autoscaling-strategy"></a>Implementace efektivní strategii neobsahovaly text
Implementace efektivní strategii neobsahovaly text obvykle zahrnuje následující součásti a procesů:

- Přístrojového vybavení a sledování systémů na úrovni aplikace, přihlašovacích údajů a infrastruktury. Tyto systémy zachytit klíčových metrik, například doby odezvy, fronty délky, využití procesoru a spotřebu paměti.
- Rozhodovací použití logických operátorů, můžete zjistit sledované změny měřítka faktory proti mezní hodnoty předdefinované systému nebo plánů a rozhodovat o tom, zda chcete změnit velikost nebo ne.
- Součásti, které odpovídají pro provádění úkolů přidružených měřítka systému, například zřizování nebo zrušit zřizování zdroje.
- Testování, sledování a ladění strategie neobsahovaly text zajistit, že funguje očekávaným způsobem.

Většina prostředí cloudové, například Azure, poskytovat předdefinované neobsahovaly text mechanismy tuto adresu obvyklé scénáře. Pokud prostředí nebo službu, kterou používáte nenabízí možnost, že takové automatické změny velikosti funkce nebo pokud máte požadavky krajní neobsahovaly text za jeho možnosti, třeba vlastní implementace. Tato vlastní implementace umožňuje shromáždit funkční a metriky systému, analyzovat je k identifikaci relevantních dat a potom podle toho změnit velikost zdroje.


## <a name="configure-autoscaling-for-an-azure-solution"></a>Konfigurace neobsahovaly text Azure řešení
Konfigurace neobsahovaly text Azure řešení několika způsoby:

- **Automatické Azure měřítko** podporuje obvyklé scénáře měřítka, podle plánu a volitelně spouštěný měřítka operace metrice spuštěnou aplikaci (třeba využití procesoru, délka fronty nebo předdefinované a vlastní čítače). Jednoduchý neobsahovaly text zásad pro řešení můžete nakonfigurovat rychle a snadno pomocí portálu Azure. Podrobnější řízení, může být pomocí [Služby Azure správy rozhraní REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) nebo [Rozhraní REST API Azure správce prostředků](https://msdn.microsoft.com//library/azure/dn790568.aspx). [Sledování služby Azure knihovnou správy](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring) na [Microsoft přehledy Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/) (verze preview) jsou SDK, které umožňují shromažďování metriky z různých zdrojů a provádění neobsahovaly text tak, že pomocí rozhraní REST API. Pro zdroje Pokud správce prostředků Azure podpora není k dispozici, nebo pokud používáte služby Azure Cloud Services rozhraní služby správy REST API lze použít pro neobsahovaly text. Ve všech ostatních případech správce Azure zdroje.
- **Vlastní řešení**, založené na vaše přístrojového vybavení v aplikaci a funkce pro správu Azure, což může být užitečné. Například použijete Azure diagnostiky nebo jiné metody přístrojového vybavení v aplikaci a vlastní kód neustále sledovat a export metriky aplikace. Můžete mít vlastní pravidla, která práci na těchto metriky a můžete využít Správa služeb nebo správce prostředků REST API je aktivovat neobsahovaly text. Metriky pro aktivaci operace měřítka lze předdefinovaný nebo vlastní čítač, nebo jiný, který implementace aplikace. Vlastní řešení se však snadná a přistupujte pouze pokud žádný z předchozích postupů splnění vašim požadavkům. [Blokování neobsahovaly text aplikace](http://msdn.microsoft.com/library/hh680892%28v=pandp.50%29.aspx) využívá tento přístup.
- **Služeb třetí strany**, jako je [Paraleap AzureWatch](http://www.paraleap.com/AzureWatch)umožňují měřítko řešení založené na plány, služby načítání a systémem ukazatele výkonu, vlastní pravidla a kombinací různých typů pravidel.

Při výběru řešení neobsahovaly text přijmout, zvažte následující skutečnosti:

- Použijte předdefinované neobsahovaly text funkce platformy, pokud odpovídaly vašim požadavkům. V opačném případě pečlivě zvažte, zda opravdu potřebujete složitější měřítka funkce. Pár příkladů další požadavky může zahrnovat další granularity ovládacího prvku, různé způsoby zjišťování události Aktivační události pro měřítko, změnu měřítka u předplatných a měřítka jiné typy zdrojů.
- Zvažte možnost, pokud jste odhadnout zatížení aplikace s dostatečnou přesností závisí pouze na plánované neobsahovaly text (přidávání a odebírání instance podle očekávaných píků v služba). Pokud tato možnost není k dispozici, použijte informace neobsahovaly text metrice shromažďované za běhu povolit aplikaci zpracovávat neočekávané změnami služba. Obvykle můžete kombinovat těchto postupů. Například vytvořte strategii přidá zdroje, jako jsou výpočetním, ukládání a fronty podle plánu tolikrát, když víte, že aplikace je nejvíce zaneprázdněného. Tím zajistíte, že je k dispozici v případě potřeby neprodleně narazili při spuštění nové instance kapacita. Kromě toho pro každé pravidlo plánované definujte metriky, pomocí kterých informace neobsahovaly text během tohoto období zajistit, že aplikace může zpracovat trvalý ale neočekávané píků v služba.
- Často není snadné pochopit vztahy mezi metriky a kapacita požadavky, hlavně když aplikace původně nasazení. Radši zřízení nevelká kapacitu navíc na začátku a potom sledovat a ladění pravidla neobsahovaly text Přenést blíž kapacita aktuální zatížení.

### <a name="use-azure-autoscale"></a>Použití automatické Azure měřítko
Automatické měřítko umožňují konfigurovat měřítko a zobrazit v dialogovém okně Možnosti řešení. Automatické měřítko můžete automaticky přidání a odebrání výskyty webové služby Azure Cloud Services a pracovní role, Azure Mobile služeb a funkcí webových aplikací Web Apps v aplikaci služby Azure. Můžete taky povolit automatické škálování spuštění a ukončení instance z Azure virtuálních počítačích. Efektivní strategii Azure neobsahovaly text obsahuje dvě sady faktory:

- Založený rozvrh neobsahovaly text, který lze ověřit další instance jsou k dispozici, aby odpovídala očekávané Špička v části použití a můžete změnit velikost v po dobu ve špičce. Umožňuje zajistit, aby měli dostatečná instance spuštěná, bez nutnosti čekání systému reagovat na načíst.
- Na základě metriky neobsahovaly text, který reagovat na faktory například průměr využití procesoru přes poslední hodiny nebo rezervu zprávy, které řešení zpracování v Azure úložiště nebo fronty Bus služby Azure. Díky aplikaci reagovat odděleně od pravidel plánované neobsahovaly text tak, aby zahrnoval neplánované nebo nepředvídaných změnami služba.

Při použití automatické měřítko, zvažte následující možnosti:

- Strategie neobsahovaly text kombinuje plánované i na základě metriky měřítka. Můžete zadat oba typy pravidel pro službu.
- By měl konfigurace pravidel neobsahovaly text a potom sledovat výkon aplikace určitou dobu. Použití výsledků sledování způsobem, ve kterém chcete systém zmenší v případě potřeby upravte. Pokračujte nezapomeňte, že neobsahovaly text není okamžitý proces. Trvá dobu reagovat na míru ATP průměr procesoru využití větší než (pod) zadaný mezní hodnota.
- Neobsahovaly text pravidla, které používají mechanismus zjišťování podle atributu naměřené aktivační událost (třeba délka procesoru používání nebo fronty) přes agregovanou hodnotu času, nikoli okamžitých hodnot pro spuštění akce neobsahovaly text. Ve výchozím nastavení je agregační funkci průměr hodnot. Tím systému příliš rychlý reagovat nebo příčinou rychlé kmitání. Umožňuje taky čas pro nové instance, které jsou automaticky spouštěné Vyrovnat do režimu zabránění neobsahovaly další text akce nedocházelo při spuštění nové instance. Azure cloudovými službami a Azure virtuálních počítačích, je výchozí limit agregace 45 minut, takže může trvat až toto období metriky aktivovat neobsahovaly text v odpovědi na vrcholy pole v služba. Změna doby agregace pomocí SDK, ale mějte na paměti, že období maximálně 25 minut může způsobit neočekávané výsledky (Další informace najdete v článku [Automatické změny velikosti služby Cloud Services na procento využití procesoru s knihovnou správy služby pro sledování Azure](http://rickrainey.com/2013/12/15/auto-scaling-cloud-services-on-cpu-percentage-with-the-windows-azure-monitoring-services-management-library/)). Webových aplikacích pro je průměrování období mnohem kratší, který umožňuje nové instance byl dostupný pod asi pět minut po změně na míru průměr aktivační událost.
- Pokud konfigurace neobsahovaly text pomocí SDK spíše než webového portálu můžete určit plán podrobnější, ve kterém jsou aktivní pravidla. Můžete taky vytvořit vlastní metriky a jejich použití s nebo bez existující v pravidlech neobsahovaly text. Můžete například chtít použít alternativní údaje, jako například počet požadavků za sekundu nebo průměr paměti dostupnost, nebo použít vlastní čítače specifickým obchodním procesům.
- Pokud neobsahovaly text virtuálních počítačích Azure, musíte nasadíte počet výskytů virtuální počítač, který je rovno maximální počet vám umožní neobsahovaly text začít. Tyto instance musí být součástí stejná skupina dostupnosti. Mechanismus neobsahovaly text virtuálních počítačích vytvoření nebo odstranění výskyty virtuálního počítače; Místo toho neobsahovaly text pravidla, které jste nakonfigurovali spustí a příslušné číslo z těchto případů stop. Další informace najdete v tématu [automaticky měřítko spuštěné Web role, role pracovníka nebo virtuálních počítačích aplikace](./cloud-services/cloud-services-how-to-scale.md).
- Pokud nemůžete spuštění nové instance, možná protože dosáhl maximální pro přihlášení k odběru nebo dojde k chybě při spuštění portálu může zobrazit úspěšnou operaci neobsahovaly text. Následující události **ChangeDeploymentConfiguration** zobrazené v portálu však se zobrazí pouze, aby bylo požadováno spuštění služby a budou žádném případě označíte, že byla úspěšně dokončena.
- Propojení zdroje, jako jsou databáze SQL instance a fronty instanci služby výpočetním můžete webového portálu uživatelského rozhraní. Umožňuje provádět další snadný přístup k samostatné ruční a automatické konfigurace možností pro jednotlivá pole propojených zdrojů měřítka. Další informace najdete v tématu [jak: propojení zdroje do cloudové služby](cloud-services-how-to-manage.md#linkresources) a [jak změnit velikost aplikace](./cloud-services/cloud-services-how-to-scale.md).
- Při konfiguraci více zásady a pravidla, může konfliktu mezi sebou. Automatické měřítko používá následující pravidla řešení konfliktů a ujistěte se, že tam je vždy dostatečný počet instance spuštěna:
  - Měřítko operací vždy přednost velkém měřítku v operace.
  - Kdy rozšiřování operace konfliktů, pravidlo, které iniciuje největší zvýšení počtu výskytů přednost.
  - Když měřítko operace konflikt, pravidlo, které iniciuje nejmenší pokles počet instancí přednost.

<a name="the-azure-monitoring-services-management-library"></a>

## <a name="application-design-considerations-for-implementing-autoscaling"></a>Pokyny k návrhu aplikace pro provádění neobsahovaly text
Neobsahovaly text není rychlé řešení. Jednoduše přidání zdrojů do systému nebo spuštěné další instance tohoto procesu nezaručuje, že se ke zlepšení výkonu systému. Při navrhování efektivní strategii neobsahovaly text je potřeba zvážit následující skutečnosti:

- Systém musí být vodorovně scalable navržený. Vyhněte se předpoklady o příbuznost instanci řešení, které vyžadují, že kód vždy běží konkrétní instanci proces nikoli návrhu. Při změně velikosti cloudové služby nebo web ve vodorovném směru, není předpokládá, že řadu žádosti ze stejného zdroje bude vždy směrovány na stejné instanci. Ze stejného důvodu Navrhněte služeb příslušnosti nesnažte se požadovat řadu žádosti o aplikaci na směrovány vždy stejné instanci služby. Při návrhu služba, která bude číst zprávy z fronty a zpracuje neprovádění žádných všechny předpoklady o které instanci služby úchyty určité zprávy. Neobsahovaly text by mohl začít další instance služby narůstající velikostí délka fronty. [Soutěží vzorek spotřebitele](http://msdn.microsoft.com/library/dn568101.aspx) popisuje postup v případě tento scénář.
- Pokud řešení implementuje časově náročné úkolu, navrhněte tento úkol na podporu rozšiřování i škálování v. Bez termínu péče, těchto úkolů mohou zabránit, aby instance procesu vypnutím čistě při systém zmenší v nebo ho může dojít ke ztrátě dat pokud vynutit ukončení. V ideálním případě Refaktorovat dlouho probíhajících úkolu a rozdělit zpracování provádějící na menší, samostatné bloky. [Kanály a filtry vzorku](http://msdn.microsoft.com/library/dn568100.aspx) obsahuje příklady jak to můžete dosáhnout.
- Můžete taky můžete provádět mechanismus kontrolní bod, aby záznamy uveďte informace o úkolu v pravidelných intervalech a uložte tento stav do trvalé úložiště, můžete k nim získat přístup tak, že všechny instance proces spuštěný úkol. Tímto způsobem Pokud je proces vypnutí, práce, která byla provádění lze obnovit z poslední kontroly pomocí další instance.
- Spuštění úlohy na pozadí na samostatném výpočetním instance, jako je v pracovní role cloudovým službám hostovaný aplikací, budete muset zobrazit různé části aplikace pomocí různých měřítka zásady. Například, budete muset nasazení další uživatelské rozhraní výpočetním instance bez zvýšení počtu pozadí výpočet instancí nebo je opakem. Pokud nabídnout různé úrovně služeb (například služby základní a premium balíčků), budete muset rozšiřování v tématech výpočetním balíčků služeb premium agresivní než datové služby základní balíčků za účelem splnění rozsahu.
- Zvažte použití délka fronty jehož prostřednictvím uživatelského rozhraní a pozadí výpočet, že instance komunikovat jako kritéria pro strategie neobsahovaly text. Toto je nejlepší ukazatel neodpovídá nebo rozdíl mezi aktuální zatížení a kapacita zpracování úkolů pozadí.
- Pokud založit strategie neobsahovaly text na čítače obchodních procesů, jako je třeba počet objednávek za hodinu nebo průměr čas spuštění složité transakce zajistit, aby plně poznat vztah mezi výsledky tyto typy čítačů a požadavků na skutečný výpočetním kapacitu. Může být nutné měřítko víc než jednou součástí nebo výpočet jednotku změny čítačů obchodních procesů.  
- Zabránit pokoušející rozšiřování příliš a vyhnout se náklady přidružené ke spuštěným mnoho tisíce instance systému, zvažte omezení maximální počet instance, které můžete automaticky přidají. Většina neobsahovaly text mechanismy umožňují určit minimální a maximální počet instancí u pravidla typu. Kromě toho zvažte řádně snížení funkce, které systém poskytuje Pokud již byly nasazeny maximální počet výskytů a pořád přetížené systém.
- Mít na paměti, které neobsahovaly text nemusí být nejvhodnější mechanismus zpracování náhlé shluk v zátěží na projektu. Trvá dobu zřízení a spuštění nové instance služby nebo přidání zdrojů do systému a služba ve špičce může mít předaných o čase, který tyto další zdroje informací je k dispozici. V tomto případě může být lepší omezení služby. Další informace najdete v článku [Omezení vzorku](http://msdn.microsoft.com/library/dn589798.aspx).
- Naopak v případě potřeby kapacitu zpracuje všechny žádosti o při hlasitost kolísání rychle a náklady není hlavní příspěvku faktor, zvažte efektivní strategii agresivní neobsahovaly text, který se spustí další instance rychleji. Můžete také plánované zásad, který se spustí dostatečný počet výskytů podle maximální načíst před očekává se, že načítání.
- Mechanismus neobsahovaly text by měl sledovat procesu neobsahovaly text a zaznamenávat podrobnosti každé události neobsahovaly text (co vyvolala, zdrojích bylo přidat nebo odebrat a pokud). Pokud vytvoříte mechanismus vlastní neobsahovaly text, ujistěte se, zahrne tuto možnost. Analýza informací o kontaktech míra účinnosti strategie neobsahovaly text a doladit v případě potřeby. Vyladit obě krátkodobě, jakmile vzorce použití k více zřejmé a překročil dlouhodobou, případně firmy rozbalí vyvíjet požadavky aplikace. Pokud je žádost doručena horní mez definované pro neobsahovaly text, mechanismus mohou také je zobrazeno upozornění operátor, kdo může ručně spustit další zdroje informací v případě potřeby. Všimněte si, že za těchto podmínek, možná by vás zodpovědného za ručního odebrání tyto materiály po pracovní zátěž usnadňuje operátor.

## <a name="related-patterns-and-guidance"></a>Související vzorků a doporučené postupy
Následující vzorce a pokyny pro možná by vás relevantní nefunguje při provádění neobsahovaly text:

- [Omezení vzorku](http://msdn.microsoft.com/library/dn589798.aspx). Tento způsob popisuje, jak můžete aplikaci dál pracovat a setkání rozsahu při zvýšení služba umístí krajní zatížení na zdroje. Omezení lze s neobsahovaly text aby nedocházelo k systému před složité dokud systém zmenší se.
- [Soutěží spotřebitele vzorku](http://msdn.microsoft.com/library/dn568101.aspx). Tento způsob popisuje, jak implementovat fond instancí služby, které můžete zpracování zprávy od všechny instance aplikace. Neobsahovaly text lze spustit a zastavit instancí služby podle očekávaných pracovní zátěž. Tento přístup umožňuje systému zpracuje více zpráv souběžně optimalizovat výkon a zlepšit škálovatelnost a dostupnost, vyrovnat pracovní zátěž.
- [Přístrojového vybavení a Telemetrie pokyny](http://msdn.microsoft.com/library/dn589775.aspx). Přístrojového vybavení a telemetrie jsou důležité pro shromáždíte informace, které můžete řídit proces neobsahovaly text.

## <a name="more-information"></a>Další informace
- [Jak zobrazit aplikace](./cloud-services/cloud-services-how-to-scale.md)
- [Automaticky nastavit měřítko spuštěné Web role, role pracovních nebo virtuálních počítačích aplikace](cloud-services-how-to-manage.md#linkresources)
- [Postup: propojení zdroje do cloudové služby](cloud-services-how-to-manage.md#linkresources)
- [Změna velikosti propojených zdrojů](./cloud-services/cloud-services-how-to-scale.md#scale-link-resources)
- [Sledování knihovnou správy služby Azure](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring)
- [Správa služby Azure rozhraní REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx)
- [Azure správce prostředků rozhraní REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx)
- [Knihovny Microsoft Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/)
- [Operace s neobsahovaly text](http://msdn.microsoft.com/library/azure/dn510374.aspx)
- [Microsoft.WindowsAzure.Management.Monitoring.Autoscale Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.management.monitoring.autoscale.aspx)
