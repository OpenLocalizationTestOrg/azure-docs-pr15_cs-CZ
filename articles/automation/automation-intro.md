<properties
    pageTitle="Co je Azure automatizaci | Microsoft Azure"
    description="Zjistěte, jaké hodnoty poskytuje Azure automatizaci a projděte si odpovědi na běžné otázky tak, aby mohli rovnou začít v tématu Vytvoření, použití runbooks a DSC automatizaci Azure."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Co je automatizaci, azure automatizaci, azure automatizaci příklady"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure automatizace přehled

Automatizace Microsoft Azure umožňuje uživatelům automatizovat ruční dlouho probíhajících, k chybám a často opakované úkoly, které provádí běžně v prostředí cloudu a enterprise. Šetří čas a zvyšuje spolehlivost běžná úkoly správy a dokonce plánuje, aby automaticky provádět v pravidelných intervalech. Můžete automatizace procesů pomocí runbooks nebo automatizovat konfigurační řízení pomocí konfigurace stavu žádoucí. Tento článek obsahuje stručný přehled Azure automatizaci a odpovědi na některé časté otázky. Můžete odkázat na jiné články v tomto knihovně podrobnější informace o různých tématech.


## <a name="automating-processes-with-runbooks"></a>Automatizace procesů pomocí runbooks

Postupu runbook je sada úkoly, které v Azure automatizaci provádět některé automatického procesu. Je možné je jednoduchý proces, například počáteční virtuálního počítače a vytvoření protokolu položky nebo máte složité postupu runbook, že kombinuje jiných menší runbooks provádět složitý proces přes více zdrojů nebo i víc mračnech a v místní prostředí.  

Například může máte existující ručního procesu pro zkrácení databázi SQL, když se blíží maximální velikost, který obsahuje několik kroků například připojení k serveru, připojení k databázi, získání aktuální velikost databáze, zaškrtněte, pokud byla překročena mezní hodnoty a pak ho zkrátit a upozornit uživatele. Místo provádění ručně každý z těchto kroků, můžete vytvořit postupu runbook, který bude provádět všechny tyto úkoly jako samostatný proces. By zahájení postupu runbook, zadejte požadované informace jako název serveru SQL, název databáze a příjemců e-mailu a potom sednout během dokončení procesu. 


## <a name="what-can-runbooks-automate"></a>Co můžete automatizovat runbooks?

Runbooks v Azure automatizaci na základě prostředí Windows PowerShell nebo pracovního postupu pro Windows PowerShell, takže přehrávají všechno, co můžete dělat Powershellu. Pokud aplikaci nebo službu rozhraní API, může s ním pracovat postupu runbook. Pokud máte modul Powershellu pro aplikaci, můžete načtení modulu do automatizaci Azure a zahrnout tyto rutiny vaší postupu runbook. Azure runbooks automatické spuštění v Azure cloudu a můžete přístup do cloudu zdroje a externích zdrojů, které jsou přístupné z cloudu. Použití [Pracovního postupu Runbook hybridní](automation-hybrid-runbook-worker.md)runbooks poběží ve vaší místní datového centra pro správu místních zdrojů. 


## <a name="getting-runbooks-from-the-community"></a>Získání runbooks od komunity

[Galerie postupu Runbook](automation-runbook-gallery.md#runbooks-in-runbook-gallery) obsahuje runbooks od společnosti Microsoft a komunity můžete použít beze změny ve vašem prostředí nebo přizpůsobit pro svou vlastní potřebu. Jsou také užitečné jako odkazy se dozvíte, jak vytvořit vlastní runbooks. Můžete dokonce přispívat vlastní runbooks do galerie, že si myslíte, že ostatní uživatelé mohou užitečné. 


## <a name="creating-runbooks-with-azure-automation"></a>Vytváření Runbooks s Azure automatizaci 

Můžete [vytvořit vlastní runbooks](automation-creating-importing-runbook.md) od začátku nebo úprava runbooks z [Galerie postupu Runbook](http://msdn.microsoft.com/library/azure/dn781422.aspx) pro vašim požadavkům. Existují tři různé [typy postupu runbook](automation-runbook-types.md) , můžete si vybrat z na základě požadavků a prostředí PowerShell. Pokud chcete pracovat přímo s kódem Powershellu, můžete pomocí [postupu runbook prostředí PowerShell](automation-runbook-types.md#powershell-runbooks) nebo [prostředí PowerShell pracovního postupu runbook](automation-runbook-types.md#powershell-workflow-runbooks) , upravovat offline nebo [textový editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) na portálu Azure. Pokud chcete upravovat postupu runbook bez nebyly přístupné pro základní kód, můžete vytvořit [grafické postupu runbook](automation-runbook-types.md#graphical-runbooks) pomocí [grafické editor](automation-graphical-authoring-intro.md) Azure portálu. 

Chcete raději sledování pro čtení? Podívejte se na pod videa z Microsoft Ignite relace na květen 2015. Poznámka: Během konceptů a funkcí popisované v tomto videu jsou správné, že Azure automatizaci má zvýšily výrazně, protože zaznamenané v tomto videu, je teď má rozsáhlejší uživatelské rozhraní na portálu Azure a podporuje další možnosti.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatizace konfigurační řízení s žádoucí konfigurace stavu 

[DSC prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx) je Správa platformu, která umožňuje spravovat, nasazení a prosazování konfigurace fyzické tabulkami hosts a virtuálních počítačích pomocí deklarativní syntaxe Powershellu. Můžete definovat konfigurace serveru centrální DSC roztáhnout, cílových počítačů automaticky načtení a použití. DSC k dispozici sadu rutiny prostředí PowerShell, který slouží ke správě konfigurace a prostředky.  

[DSC automatizaci Azure](automation-dsc-overview.md) je cloudový řešení pro PowerShell DSC poskytující služby potřebné pro organizace prostředí.  Můžete spravovat zdroje DSC v Azure automatizaci a použít konfigurace virtuální nebo pole fyzicky stroje, které načítat ze serveru vyžádat DSC v Azure cloudu.  Je také služby reporting services, které informuje že o důležité události, jako je třeba uzly jste se od jejich přiřazené konfigurace a pokud novou konfiguraci, které byly použity. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Vytvoření vlastního DSC konfigurací s automatizaci Azure

[Konfigurace DSC](automation-dsc-overview.md#azure-automation-dsc-terms) zadejte požadovaný stav uzel.  Více uzlů můžete použít stejnou konfiguraci dosažení všechny udržovat identickými stavu.  Můžete vytvořit editor konfiguraci pomocí veškerý text v místním počítači a importujte ho na místo, kam ho kompilaci automatizaci Azure a použít ho uzlů.


## <a name="getting-modules-and-configurations"></a>Získání moduly a konfigurace 

Můžete získat [modulů](automation-runbook-gallery.md#modules-in-powershell-gallery) obsahující rutin, které můžete použít v runbooks a konfigurace DSC z [Galerie Powershellu](http://www.powershellgallery.com/). Spuštění této galerii z portálu Microsoft Azure a importovat moduly přímo do automatizaci Azure nebo můžete stáhnout a importovat ručně. Nejde nainstalovat moduly přímo z portálu Microsoft Azure, ale můžete stáhnout nainstalujte je jako všechny ostatní moduly. 


## <a name="example-practical-applications-of-azure-automation"></a>Příklad praktické aplikace automatizace Azure 

Následující je pár příkladů jaké druhy automatizaci scénáře s Azure automatizaci. 

* Vytvoření a zkopírování virtuálních počítačích v různých předplatných Azure. 
* Plán kopie souborů z místního počítače k úložišti objektů Blob Azure kontejner. 
* Automatizace funkce zabezpečení, které jako zamítnutí žádosti o z klienta ke zjištění útoku služby. 
* Zajistěte, aby počítačích neustále zarovnáte zásadách nakonfigurovaném zabezpečení.
* Správa nepřetržitý nasazení kódu aplikace v cloudu a místní infrastruktury. 
* Vytvoření struktuře služby Active Directory v Azure pro prostředí laboratorní. 
* Pokud databáze se blíží maximální velikost Zkraťte tabulky v databázi SQL. 
* Aktualizujte nastavení prostředí Azure webu. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Jak Azure automatizace týkající se dalších nástrojů automatizace?

[Služba Správa automatizaci (SMA)](http://technet.microsoft.com/library/dn469260.aspx) slouží k automatizaci úkolů správy v soukromé cloudu. Hned po instalaci používat místně v datovém centru jako součást sady [Microsoft Azure](https://www.microsoft.com/en-us/server-cloud/). SMA a Azure automatizace používat stejný formát postupu runbook na základě prostředí Windows PowerShell a pracovního postupu pro Windows PowerShell, ale SMA nepodporuje [grafické runbooks](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) je určená pro automatizaci místních zdrojů. Používá formát různých postupu runbook než Azure automatizaci a služby správy automatizace a má grafického rozhraní pro vytvoření runbooks bez nutnosti jakékoli skriptování. Jeho runbooks jsou skládající se ze aktivity ze sady integrace vzniku speciálně pro Orchestrator. 


## <a name="where-can-i-get-more-information"></a>Kde můžu získat další informace? 

Jsou k dispozici pro vás další informace o Azure automatizace a vytváření vlastních runbooks různých zdrojů. 

* **Knihovna automatizaci Azure** není, kde se nacházíte teď. Články z této knihovny na konfigurace a správy Azure automatizaci a pro vytváření vlastních runbooks zadejte úplnou si přečtěte následující dokumentaci. 
* [Rutiny prostředí PowerShell Azure](http://msdn.microsoft.com/library/jj156055.aspx) obsahuje informace o automatizaci Azure operace pomocí Windows Powershellu. Runbooks pomocí těchto rutin pro práci s Azure zdroje. 
* [Správa blogu](https://azure.microsoft.com/blog/tag/azure-automation/) obsahuje nejnovější informace o Azure automatizace a jinými technologiemi správy od Microsoftu. Odebíráte má tento blogu dodržovat předpisy aktuální s nejnovějšími od týmu služeb Azure automatizaci. 
* [Fórum komunity automatizaci](http://go.microsoft.com/fwlink/p/?LinkId=390561) umožňuje pokládat dotazy k automatizaci Azure používala společnostmi Microsoft a automatizaci komunity. 
* [Rutiny pro automatizaci Azure](https://msdn.microsoft.com/library/mt244122.aspx) obsahuje informace o automatizace úkolů správy. Rutiny pro správu automatizaci účty, prostředky, runbooks DSC v ní.


## <a name="can-i-provide-feedback"></a>Poskytnutí zpětné vazby 

**Zadejte váš názor!** Pokud hledáte řešení postupu runbook automatizaci Azure nebo modul integrace požadavek skriptu do odešlete Centrum skriptů. Pokud máte žádostí o názor nebo funkce pro automatizaci Azure, vystavit je na [Hlasové uživatele](http://feedback.windowsazure.com/forums/34192--general-feedback). Dík! 


