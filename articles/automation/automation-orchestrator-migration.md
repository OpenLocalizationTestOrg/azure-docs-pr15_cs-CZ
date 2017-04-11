<properties
   pageTitle="Migrace z Orchestrator Azure automatizace | Microsoft Azure"
   description="Popisuje, jak migrovat runbooks a integrace sady z System Center Orchestrator automatizace Azure."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migrace z Orchestrator Azure automatizace (verze Beta)

Runbooks v [Systému Centrum Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) vycházejí aktivity ze sady integrace napsané speciálně pro Orchestrator během runbooks v Azure automatizaci vycházejí z prostředí Windows PowerShell.  [Grafické runbooks](automation-runbook-types.md#graphical-runbooks) v Azure automatizaci mají podobný vzhled k Orchestrator runbooks s jejich aktivitách představující rutin prostředí PowerShell, podřízené runbooks a prostředky.

[Systém Centrum Orchestrator migrace sada](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) obsahuje nástroje při převodu runbooks z Orchestrator automatizace Azure.  Kromě převod runbooks sami, je nutné převést sady integrace s činnostmi, které využívají runbooks integrační moduly pomocí rutin prostředí Windows PowerShell.  

Toto je základní proces pro převod Orchestrator runbooks automatizace Azure.  Každý z těchto kroků se píše níže.

1.  Stáhněte si [System Center Orchestrator migrace nástrojů](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , který obsahuje nástroje a moduly popisované v tomto článku.
2.  Naimportujte [standardní aktivity modul](#standard-activities-module) Azure automatizaci.  Jedná se o převedený verzích standardní Orchestrator činnosti, které mohou být použity převedené runbooks.
3.  Importujte [System Center Orchestrator integrace moduly](#system-center-orchestrator-integration-modules) do Azure automatizaci pro tyto integrace sady používané vaší runbooks, přistupovat System Center.
4.  Převedení vlastní a třetích stran integrace sady použití [Převaděče Pack integrace](#integration-pack-converter) a importovat do Azure automatizaci.
5.  Převedení Orchestrator runbooks použití [Převaděče postupu Runbook](#runbook-converter) a nainstalujte v Azure automatizaci.
6.  Ručně vytvořte požadované prostředky Orchestrator Azure automatizaci od tohoto postupu Runbook převaděče nepřevádí tyto materiály.
7.  Konfigurace [Hybridního pracovního postupu Runbook](#hybrid-runbook-worker) ve vašem Centru místních dat ke spuštění převedené runbooks, který bude přistupovat k místních zdrojů.

## <a name="service-management-automation"></a>Automatizace Správa služeb

[Automatizace Správa služeb](http://technet.microsoft.com/library/dn469260.aspx) (SMA) jsou uloženy a běží runbooks ve vašem Centru místních dat jako Orchestrator a používá stejnou integrační moduly jako Azure automatizaci. [Převaděč postupu Runbook](#runbook-converter) převede Orchestrator runbooks grafické runbooks přes které nejsou podporovány v SMA.  Můžete nainstalovat [Standardní modul aktivity](#standard-activities-module) a [System Center Orchestrator integrace moduly](#system-center-orchestrator-integration-modules) do SMA, ale je nutné ručně [přepisu vaší runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Pracovního postupu Runbook hybridní

Runbooks v Orchestrator se ukládají na databázovém serveru a spouštět na serverech postupu runbook, jak v Centru pro místní data.  Runbooks v Azure automatizaci uložené v Azure cloudu a mohlo by umožnit spuštění ve vašem Centru místních dat pomocí [Pracovního postupu Runbook hybridní](automation-hybrid-runbook-worker.md).  Je to, jak se obvykle spustí runbooks převést z Orchestrator, protože jsou určeny místních serverů.

## <a name="integration-pack-converter"></a>Integrace sady převaděčů

Převaděč Pack integrace převede integrace sady, které jsou vytvořené pomocí [Orchestrator integrace sada nástrojů (OIT)](http://technet.microsoft.com/library/hh855853.aspx) do integrace moduly založené na prostředí Windows PowerShell, který jde importovat do automatizaci Azure nebo služby správy automatizaci.  

Při spuštění převaděč Pack integrace se zobrazí průvodce, který vám umožní vybrat soubor integrace pack (.oip).  Průvodce potom seznamy aktivity zahrnuté v této integrace pack a umožňuje vybrat, které se přesunou.  Po dokončení Průvodce vytvoří integrace moduly, které obsahuje odpovídající rutiny pro každou aktivity v původní integrace pack.


### <a name="parameters"></a>Parametry

Jakékoli vlastnosti aktivity sady integrace se převedou na parametry rutinu odpovídající v modulu integrace.  Rutin prostředí Windows PowerShell nastaven [běžné parametry](http://technet.microsoft.com/library/hh847884.aspx) , kterou budete moct použít s všechny rutin.  Například podrobné parametr - způsobí, že rutina výstup podrobné informace o jeho operace.  Žádné rutina možná bude potřeba parametr se stejným názvem jako běžné parametr.  Pokud aktivitu vlastnost se stejným názvem jako běžné parametr, Průvodce vás vyzve k zadejte jiný název parametru.

### <a name="monitor-activities"></a>Monitor aktivity

Sledování runbooks v Orchestrator začít s [monitor aktivity](http://technet.microsoft.com/library/hh403827.aspx) a spusťte nepřetržitě čekající na uplatnit zvláštní událost.  Azure automatizaci nepodporuje runbooks monitoru, takže nebude převést všechny monitor aktivity sady integrace.  Místo toho zástupný symbol rutina vytvořených v modulu integrace monitor aktivity.  Tato rutina má žádné funkce, ale umožňuje převedené postupu runbook, který používá k instalaci.  Tohoto postupu runbook nebudou moct spouštět v Azure automatizaci, ale je možné nainstalovat tak, že můžete změnit.

### <a name="integration-packs-that-cannot-be-converted"></a>Integrace sady, které nelze převést

Integrace sady, které nebyly vytvořené pomocí OIT nelze převést s převaděčem integrace Pack. Existují také některé integrace sady poskytované společností Microsoft, který nelze převést aktuálně v tomto nástroji.  Převedená verzí sady tyto integrace byla [k dispozici ke stažení](#system-center-orchestrator-integration-modules) tak, že můžete mít nainstalovanou na automatizaci Azure nebo služby správy automatizaci.


## <a name="standard-activities-module"></a>Aktivity standardní modul

Orchestrator obsahuje sadu [standardní činnosti](http://technet.microsoft.com/library/hh403832.aspx) , které nejsou součástí sady pro integraci ale používají mnoho runbooks.  Modul standardní aktivit je integrace moduly, které zahrnuje rutina odpovídající pro každou z těchto činností.  Tento modul integrace v Azure automatizaci nainstalujte před importem převedené runbooks, využívající standardní aktivity.

Kromě podpora převedené runbooks, v modulu standardní aktivity lze použít tak, že někdo zkušenosti s Orchestrator vytvářet nové runbooks v Azure automatizaci.  Zatímco funkce všech standardních činností lze provést pomocí rutin, může fungovat jinak.  Rutiny pro správu v modulu převedené standardní aktivity funguje stejně jako odpovídající aktivity a použít stejné parametry.  Můžete se tím existující autora postupu runbook Orchestrator v jejich přechod k automatizaci Azure runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>Moduly integrace Orchestrator System Center

Microsoft poskytuje [Integrace sady](http://technet.microsoft.com/library/hh295851.aspx) pro vytváření runbooks k automatizaci součásti System Center a další produkty.  Některé z těchto sad integrace jsou aktuálně založeny na OIT, ale nemůžete aktuálně převedou na integrační moduly z důvodu známé problémy.  [System Center Orchestrator integrace moduly](https://www.microsoft.com/download/details.aspx?id=49555) obsahuje převedené verzí sady tyto integrace, které lze importovat do Azure automatizace a automatizace Správa služeb.  

Verzí RTM tento nástroj bude publikování aktualizované verze sad integrace podle OIT, které lze převést s převaděčem integrace Pack.  Pokyny pro budou uvedené taky při převodu runbooks pomocí aktivit ze sady integrace bez správy OIT.

## <a name="runbook-converter"></a>Převaděč postupu Runbook

Převaděč postupu Runbook převede Orchestrator runbooks [grafické runbooks](automation-runbook-types.md#graph-runbooks) , který jde importovat do Azure automatizaci.  

Převaděč postupu Runbook prováděné jako modul prostředí PowerShell s rutina s názvem **ConvertFrom SCORunbook** , které provede převod.  Při instalaci nástroj vytvoří zástupce na relaci Powershellu, který se načítá rutiny.   

Toto je základní proces převést postupu runbook Orchestrator a importujte ho na Azure automatizaci.  V následujících oddílech další podrobnosti o použití nástroje a pracovat s ním převedené runbooks.

1. Exportujte runbooks jeden nebo více z Orchestrator.
2. Získáte integrace modulů pro všechny aktivity v postupu runbook.
3. Převedení runbooks Orchestrator exportovaného souboru.
4. Prohlédněte si informace v protokolech ověřit převod a zjistit požadované Ruční úkoly.
4. Převedená runbooks naimportujte Azure automatizaci.
5. Vytvořte požadované prostředky v Azure automatizaci.
6. Úprava postupu runbook v Azure automatizaci upravit libovolné požadované aktivity.

### <a name="using-runbook-converter"></a>Použití převaděče postupu Runbook

Syntaxe **ConvertFrom SCORunbook** vypadá takto:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath – cesta k exportu souboru, který obsahuje runbooks převést.
- Modul – čárkou seznamu modulů integrace obsahující aktivity v runbooks.
- OutputFolder - cestu ke složce vytvořit převést grafické runbooks. 


Následující příklad příkazu převede runbooks do exportovaného souboru s názvem **MyRunbooks.ois_export**.  Tyto runbooks pomocí balíčků integrace služby Active Directory a správce ochranu dat.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Protokoly

Převaděč postupu Runbook vytvoří následující souborů protokolu ve stejném umístění jako převedené postupu runbook.  Pokud soubory, které už existuje, bude přepsat informace z posledního přepočtu.

| Soubor | Obsah |
|:---|:---|
| Převodník postupu Runbook - Progress.log | Podrobný postup převodu včetně informací pro každou aktivitu úspěšně převedou a upozornění pro každou aktivitu není převedou. |
| Převodník postupu Runbook - Summary.log  | Shrnutí poslední převod včetně všechna upozornění a zpracování úkolů, které potřebujete provést například vytváření proměnné potřebných pro převedený postupu runbook.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Export runbooks z Orchestrator

Převaděč postupu Runbook spolupracuje export souboru z Orchestrator, který obsahuje jeden nebo více runbooks.  Odpovídající postupu runbook Azure automatizaci pro každou postupu runbook Orchestrator vytvoří v souboru exportu.  

Export postupu runbook z Orchestrator, klikněte pravým tlačítkem myši na název postupu runbook v Návrháři postupu Runbook a zaškrtněte políčko **Exportovat**.  Pokud chcete exportovat všechna runbooks ve složce, klikněte pravým tlačítkem myši na název složky a zaškrtněte políčko **Exportovat**.


### <a name="runbook-activities"></a>Aktivity postupu Runbook

Převaděč postupu Runbook převede jednotlivé aktivity v postupu runbook Orchestrator na odpovídající aktivita v Azure automatizaci.  U těchto činností, které nelze převést vytvoří se aktivitu zástupného symbolu ve postupu runbook textem upozornění.  Po importu převedené postupu runbook do Azure automatizaci, je nutné nahradit některou z těchto akcí platné činnosti, které provedení požadované funkce.

Činnosti Orchestrator v [Standardní modul aktivity](#standard-activities-module) se převedou.  Existují některé standardní Orchestrator činnosti, které nejsou v tomto modulu přes a převáděny.  **Odeslat událost platformy** má například žádné Azure automatizaci odpovídající vzhledem k tomu, že se jedná specifické pro Orchestrator.

Protože chybí ekvivalent na ně v Azure automatizaci nejsou převést [Monitor aktivity](https://technet.microsoft.com/library/hh403827.aspx) .  Výjimky jsou monitor aktivity v [Převést integrace sady](#integration-pack-converter) , které budou převedeny na zástupný symbol aktivity.

Všechny aktivity před [Převést pack integrace](#integration-pack-converter) se převedou Pokud zadejte cestu do modulu integrace s parametrem **moduly** .  Pro System Center integrace sady můžete [System Center Orchestrator integrace moduly](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator zdroje

Převaděč postupu Runbook pouze převede runbooks, ne další Orchestrator zdroje, jako jsou čítače, proměnné nebo připojení.  Čítače nejsou podporovány v Azure automatizaci.  Proměnné a připojení jsou podporovaná, ale je nutné vytvořit ručně.  Soubory protokolu informuje Pokud postupu runbook vyžaduje těchto zdrojů a zadat odpovídající prostředky, které je potřeba vytvořit v Azure automatizaci pro převedené postupu runbook fungovat správně.

Například postupu runbook může použít proměnnou k naplnění určitou hodnotu činnost.  Převedená postupu runbook bude aktivitu převést na a zadejte proměnné materiálů v Azure automatizaci se stejným názvem jako proměnná Orchestrator.  To poznamenat v souboru **Postupu Runbook převaděče - Summary.log** , která se vytvoří po převodu.  Bude je potřeba ručně vytvořit tohoto proměnné majetku v Azure automatizaci před použitím postupu runbook. 

 
### <a name="input-parameters"></a>Vstupní parametry

Runbooks v Orchestrator přijmout vstupních parametrů s aktivitou **Inicializace Data** .  Pokud postupu runbook převodu obsahuje tato činnost, [vstupní parametry](automation-graphical-authoring-intro.md#runbook-input-and-output) v postupu runbook Azure automatizaci vytvoří pro každý parametr aktivity.  Činnosti [pracovního postupu skriptovat ovládací prvek](automation-graphical-authoring-intro.md#activities) se vytvoří v převedené postupu runbook, která načte a vrátí každý parametr.  Činnosti v postupu runbook používající vstupní parametry v nápovědě k výstup z této aktivity.

Důvod, proč použití tohoto strategie je nejlepší odrážet funkčnost Orchestrator postupu runbook.  Aktivity v nové grafické runbooks by odkazoval přímo na vstupní parametry pomocí zdroj zadávání dat postupu Runbook.


### <a name="invoke-runbook-activity"></a>Vyvolání postupu Runbook aktivity

Runbooks v Orchestrator začínat jiných runbooks aktivity **Vyvolat postupu Runbook** . Když postupu runbook převodu obsahuje tuto aktivitu a nastavíte možnost **Čekání na dokončení** , pak aktivitu postupu runbook vytvoří se pro něj ve převedené postupu runbook.  Pokud není nastavená možnost **Čekání na dokončení** , je vytvořena aktivita skript pracovního postupu, který používá **AzureAutomationRunbook spustit** spusťte postupu runbook.  Po importu převedené postupu runbook do Azure automatizaci musí úprava tuto aktivitu pomocí informace podle aktivity.




## <a name="related-articles"></a>Související články

- [System Center 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Automatizace Správa služeb](https://technet.microsoft.com/library/dn469260.aspx)
- [Pracovního postupu Runbook hybridní](automation-hybrid-runbook-worker.md)
- [Standardní aktivity Orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
- [Stažení systému Orchestrator migrace sada nástrojů](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
