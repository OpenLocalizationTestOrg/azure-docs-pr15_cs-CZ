<properties
    pageTitle="Vytváření nebo do ní importují postupu runbook v Azure automatizaci"
    description="Tento článek popisuje, jak vytvořit nový postupu runbook v Azure automatizaci nebo importovat ze souboru."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Vytvoření a import postupu runbook v Azure automatizaci

Automatizace Azure můžete přidat postupu runbook [Vytvoření nového](#creating-a-new-runbook) nebo importováním existující postupu runbook ze souboru nebo z [Galerie postupu Runbook](automation-runbook-gallery.md). Tento článek obsahuje informace o vytváření a import runbooks ze souboru.  Všechny informace o přístupu k runbooks komunity a moduly do [Galerie postupu Runbook a modulu pro automatizaci Azure](automation-runbook-gallery.md), můžete získat.

## <a name="creating-a-new-runbook"></a>Vytvoření nového postupu runbook

Vytvoření nového postupu runbook v Azure automatizaci pomocí jedné z Azure portály nebo prostředí Windows PowerShell. Po vytvoření postupu runbook jste ho mohl(a) upravovat pomocí informací v [Prostředí PowerShell výukové pracovní postup](automation-powershell-workflow.md) a [grafické autorské v Azure automatizaci](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Vytvoření nového postupu runbook automatizaci Azure pomocí portálu Azure klasické

Práce s [runbooks prostředí PowerShell pracovního postupu](automation-runbook-types.md#powershell-workflow-runbooks) můžete jenom na portálu Azure.

1. Na portálu Azure klasické klikněte na, **novou** **aplikaci služby**, **automatizaci**, **postupu Runbook**, **Vytvořit**.
2. Zadejte požadované informace a potom klikněte na **vytvořit**. Postupu runbook název musí začínat písmenem a nemůže mít písmena, číslice, podtržítka a typ čáry.
3. Pokud chcete upravit postupu runbook nyní a pak klikněte na **Upravit postupu Runbook**. Jinak klikněte na **OK**.
4. Na kartě **Runbooks** se zobrazí nová postupu runbook.


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Vytvoření nového postupu runbook automatizaci Azure pomocí portálu Azure

1. Na portálu Azure si potřebujete založit účet automatizaci.
2. Klikněte na dlaždici **Runbooks** otevřete seznam runbooks.
3. Klikněte na tlačítko **Přidat postupu runbook** a pak **vytvořit nové postupu runbook**.
2. Zadejte **název** postupu runbook a vyberte jeho [Typ](automation-runbook-types.md). Postupu runbook název musí začínat písmenem a nemůže mít písmena, číslice, podtržítka a typ čáry.
3. Kliknutím na tlačítko **vytvořit** vytvořte postupu runbook a otevřete dialogové okno editor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Vytvoření nového postupu runbook automatizaci Azure pomocí prostředí Windows PowerShell

Vytvoření prázdného [prostředí PowerShell pracovního postupu runbook](automation-runbook-types.md#powershell-workflow-runbooks)můžete použít rutinu [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) . Buď můžete zadat **název** parametru vytvoření prázdného postupu runbook, můžete později upravovat nebo můžete zadat parametrem **cesta** k importu souboru postupu runbook. Parametr **typu** by měl být zahrnut do zadejte jednu ze čtyř typů postupu runbook.

Následující ukázkové příkazy ukazují, jak vytvořit nový prázdný postupu runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Import postupu runbook ze souboru automatizaci Azure

Můžete vytvořit nový postupu runbook v Azure automatizaci importováním skript Powershellu nebo prostředí PowerShell pracovního postupu (.ps1 linku) nebo exportovaného grafické postupu runbook (.graphrunbook).  Je třeba určit [Typ postupu runbook](automation-runbook-types.md) vytvořený z importu přijímat v úvahu následující skutečnosti.

- Soubor .graphrunbook mohou pouze importovat do nového [grafické postupu runbook](automation-runbook-types.md#graphical-runbooks)a grafické runbooks pouze nelze vytvořit ze souboru .graphrunbook.
- Soubor .ps1 obsahující prostředí PowerShell pracovního postupu lze importovat pouze do [Powershellu pracovního postupu runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Pokud soubor obsahuje více pracovních postupů Powershellu, import se nezdaří. Musíte uložit každý pracovní postup vlastní souboru a importovat každý zvlášť.
- .Ps1 soubor, který neobsahuje pracovního postupu lze importovat do [postupu runbook prostředí PowerShell](automation-runbook-types.md#powershell-runbooks) nebo [prostředí PowerShell pracovního postupu runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Pokud se importuje do Powershellu pracovního postupu runbook, pak se převedou na pracovního postupu a komentáře se zahrne do postupu runbook určující změny provedené.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Import postupu runbook ze souboru s Azure klasické portálu
Následující postup umožňuje importovat soubor skriptu do Azure automatizaci.  Všimněte si, že můžete importovat pouze soubor .ps1 do Powershellu pracovního postupu runbook tento portálu.  Pro jiné typy musí používat portál Azure.

1. Na portálu Správa Azure vyberte **automatizaci** a pak vyberte účet automatizaci.
2. Klikněte na **importovat**.
3. Klikněte na **Procházet** a vyhledejte soubor skriptu pro import.
4. Pokud chcete upravit postupu runbook nyní a pak klikněte na **Upravit postupu Runbook**. Jinak klikněte na OK.
5. Nové postupu runbook se zobrazí na kartě **Runbooks** pro automatizaci účet.
6. Před spuštěním musíte [Publikovat postupu runbook](#publishing-a-runbook) .


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Import postupu runbook ze souboru pomocí portálu Azure
Následující postup umožňuje importovat soubor skriptu do Azure automatizaci.  

>[AZURE.NOTE] Všimněte si, že můžete importovat pouze soubor .ps1 do Powershellu pracovního postupu runbook na portálu.

1. Na portálu Azure si potřebujete založit účet automatizaci.
2. Klikněte na dlaždici **Runbooks** otevřete seznam runbooks.
3. Klikněte na tlačítko **Přidat postupu runbook** a pak **importovat**.
4. Klikněte na **soubor postupu Runbook** vyberte soubor, který chcete importovat
2. Pokud je povoleno do pole **název** , máte možnost ho změnit.  Postupu runbook název musí začínat písmenem a nemůže mít písmena, číslice, podtržítka a typ čáry.
3. [Typ postupu runbook](automation-runbook-types.md) vyberou se automaticky, ale můžete změnit typ po přijetí příslušných omezení do účtu. 
3. Nové postupu runbook se zobrazí v seznamu runbooks pro automatizaci účet.
4. Před spuštěním musíte [Publikovat postupu runbook](#publishing-a-runbook) .

>[AZURE.NOTE] Po importu grafické postupu runbook nebo grafický postupu runbook prostředí PowerShell pracovního postupu, máte možnost převést na jiný typ, pokud je potřeba. Nelze převést na textový.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Můžete importovat postupu runbook z soubor skriptu prostředí Windows PowerShell

Můžete použít rutinu [Import AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) k importu souboru skript jako koncept prostředí PowerShell pracovního postupu runbook. Pokud postupu runbook již existuje, import se nezdaří, pokud nepoužijete *– vyšší* parametr. 

Následující ukázkové příkazy ukazují, jak importovat soubor skriptu do postupu runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Publikování postupu runbook

Při vytváření nebo importovat nové postupu runbook, musíte publikujte před spuštěním ho.  Každý postupu runbook v automatizaci má koncept a publikovaná verze. Pouze publikované verze je k dispozici proběhnout a pracovní verze je možné upravit. Publikovaná verze je ovlivněna změn verze konceptu. Když pracovní verze by měly být k dispozici, pak můžete publikovat který přepíše publikovaná verze verze konceptu.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Chcete-li publikovat postupu runbook Azure klasické portálu

1. Otevřete postupu runbook Azure klasické portálu.
1. V horní části obrazovky klikněte na **autora**.
1. V dolní části obrazovky klikněte na **Publikovat** a potom **Ano** zprávu ověření.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Chcete-li publikovat postupu runbook pomocí portálu Azure

1. Na portálu Azure otevřete postupu runbook.
1. Klikněte na tlačítko **Upravit** .
1. Kliknutím na tlačítko **Publikovat** a pak **Ano** ověřovací zprávu.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Chcete-li publikovat postupu runbook pomocí Windows Powershellu

Můžete použít rutinu [Publikovat AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) publikovat postupu runbook používat Windows PowerShell. Následující ukázkové příkazy ukazují, jak publikovat ukázkové postupu runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Další kroky
- Dozvědět, jak můžete využívat postupu Runbook a prostředí PowerShell modul galerie, najdete v článku [galerií postupu Runbook a modulu pro automatizaci Azure](automation-runbook-gallery.md)
- Další informace o úpravách prostředí PowerShell a pracovních postupů prostředí PowerShell runbooks textový editor najdete v tématu [Úpravy textové runbooks v Azure automatizaci](automation-edit-textual-runbook.md)
- Další informace o vytváření grafické postupu runbook, najdete v článku [vytváření grafické v Azure automatizaci](automation-graphical-authoring-intro.md)
