<properties 
    pageTitle="Úpravy textové runbooks v Azure automatizaci"
    description="Tento článek obsahuje různé postupy pro práci s prostředí PowerShell a pracovních postupů prostředí PowerShell runbooks v Azure automatizaci textového editoru."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Úpravy textové runbooks v Azure automatizaci

Textový editor v Azure automatizaci lze upravovat [runbooks prostředí PowerShell](automation-runbook-types.md#powershell-runbooks) a [runbooks prostředí PowerShell pracovního postupu](automation-runbook-types.md#powershell-workflow-runbooks). To má typické funkce jiných kód editoři například technologie intellisense a barevného kódování s další speciální funkce při přístupu k prostředkům společné runbooks.  Tento článek obsahuje podrobný postup pro provádění různých funkcí tento editor.

Textový editor obsahuje nástroj pro vložení kódu aktivity, majetek i runbooks podřízené do postupu runbook. Místo zadávání textu v kódu sami, můžete vybrat ze seznamu dostupných zdrojů a mít příslušný kód vložená do postupu runbook.

Každý postupu runbook v Azure automatizaci má dvě verze konceptu a Publikováno. Upravte koncept verze postupu runbook a pak publikovat tak, aby mohli provádět. Nelze upravit, publikovaná verze. Další informace najdete v tématu [publikování postupu runbook](automation-creating-importing-runbook.md#publishing-a-runbook) .

Pro práci s [Grafickým Runbooks](automation-runbook-types.md#graphical-runbooks), najdete v článku [vytváření grafické v Azure automatizaci](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Chcete-li upravit postupu runbook pomocí portálu Azure

Chcete-li otevřít postupu runbook pro úpravy v editoru textové pomocí následujícího postupu.

1. Na portálu Azure vyberte svůj účet automatizaci.
2. Klikněte na dlaždici **Runbooks** otevřete seznam runbooks.
3. Klikněte na název, který chcete upravit a potom klikněte na tlačítko **Upravit** postupu runbook.
6. Proveďte požadované úpravy.
7. Po dokončení úprav klikněte na tlačítko **Uložit** .
8. Pokud chcete nejnovější verze konceptu postupu runbook publikování, klikněte na **Publikovat** .

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Pokud chcete vložit rutina do postupu runbook

2. Plátno textový editor umístěte kurzor místo, kam chcete umístit rutiny.
3. Rozbalte uzel **rutin** v ovládacím prvku knihovny. 
3. Rozbalte modul obsahující rutinu, který chcete použít.
4. Klikněte pravým tlačítkem myši rutinu vložit a potom vyberte **Přidat do plátno**.  Pokud rutinu má víc než jeden parametr nastaven, bude přidán výchozí sadu.  Můžete taky rozbalit rutinu vyberte nastavit jiné parametr.
4. Kód rutinu vložena s jeho úplný seznam parametrů.
5. Obsahují hodnotu odpovídající místo datový typ uzavřen <> složenou závorku pro všechny požadované parametry.  Odeberte všechny parametry, které už nepotřebujete.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Vložení kódu pro postupu runbook podřízené do postupu runbook

2. Plátno textový editor umístěte kurzor místo, kam chcete umístit kód [podřízené postupu runbook](automation-child-runbooks.md).
3. Rozbalte uzel **Runbooks** v ovládacím prvku knihovny. 
3. Klikněte pravým tlačítkem myši postupu runbook vložit a potom vyberte **Přidat do plátno**.
4. Kód postupu runbook podřízené vložena s zástupné symboly pro všechny parametry postupu runbook.
5. Nahraďte zástupný text odpovídající hodnoty pro každý parametr.

### <a name="to-insert-an-asset-into-a-runbook"></a>Pokud chcete vložit aktivum do postupu runbook

2. Plátno textový editor umístěte kurzor místo, kam chcete umístit kód podřízené postupu runbook.
3. Rozbalte uzel **prostředky** v ovládacím prvku knihovny. 
4. Rozbalte uzel pro typ materiálů, které chcete.
3. Klikněte pravým tlačítkem myši materiálů vložit a potom vyberte **Přidat do plátno**.  [Proměnná prostředky](automation-variables.md)vyberte **Přidat "získat proměnnou" plátno** nebo **Přidat "nastavit proměnnou" plátno** podle toho, jestli chcete získat nebo nastavit proměnnou.
4. Kód majetku je vložen do postupu runbook.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Chcete-li upravit postupu runbook pomocí portálu Azure

Chcete-li otevřít postupu runbook pro úpravy v editoru textové pomocí následujícího postupu.

1. Na portálu Azure vyberte **automatizace** a potom klikněte na položku název účtu, který automatizaci.
2. Vyberte kartu **Runbooks** .
3. Klikněte na název, který chcete upravit a pak vyberte kartu **Autor** postupu runbook.
5. Klikněte na tlačítko **Upravit** v dolní části obrazovky.
6. Proveďte požadované úpravy.
7. Po dokončení úprav klikněte na tlačítko **Uložit** .
8. Pokud chcete nejnovější verze konceptu postupu runbook publikování, klikněte na **Publikovat** .

### <a name="to-insert-an-activity-into-a-runbook"></a>Pokud chcete vložit aktivitu do postupu Runbook

1. Plátno textový editor umístěte kurzor místo, kam chcete umístit aktivity.
1. V dolní části obrazovky klikněte na **Vložit** a potom **aktivity**.
1. Ve sloupci **Integrace modul** vyberte modul obsahující aktivity.
1. V podokně **aktivity** vyberte aktivity.
1. Ve sloupci **Popis** Poznámka popis aktivity. Pokud chcete, klikněte na zobrazení podrobnou nápovědu k spuštění nápovědu aktivity v prohlížeči.
1. Klikněte na šipku doprava.  Pokud má aktivita parametry, budou uvedené vaše informace.
1. Klikněte na tlačítko Zkontrolovat.  Spuštění aktivitu kódu se vloží do postupu runbook.
1. Pokud aktivity vyžaduje parametry, zadejte hodnotu odpovídající místo datový typ uzavřeny v závorkách <>.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Vložení kódu pro postupu runbook podřízené do postupu runbook

1. Plátno textový editor umístěte kurzor místo, kam chcete umístit [podřízené postupu runbook](automation-child-runbooks.md).
2. V dolní části obrazovky klikněte na **Vložit** a potom **postupu Runbook**.
3. Vyberte postupu runbook vložení z Prostřední sloupec a klikněte na šipku doprava.
4. Pokud postupu runbook parametry, budou uvedené vaše informace.
5. Klikněte na tlačítko Zkontrolovat.  Spuštění vybraného postupu runbook kódu se vloží do aktuální postupu runbook.
7. Pokud postupu runbook vyžaduje parametry, zadejte hodnotu odpovídající místo datový typ uzavřeny v závorkách <>.

### <a name="to-insert-an-asset-into-a-runbook"></a>Pokud chcete vložit aktivum do postupu runbook

1. Plátno textový editor umístěte kurzor místo, kam chcete umístit aktivity k načtení majetku.
1. V dolní části obrazovky klikněte na **Vložit** a potom na **Nastavení**.
1. Ve sloupci **Nastavení akce** vyberte akce, které mají.
1. Vyberte prostředky k dispozici ve sloupci centrum.
1. Klikněte na tlačítko Zkontrolovat.  Kód pro získání nebo nastavení majetku bude vložen do postupu runbook.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Chcete-li upravit automatizaci Azure postupu runbook pomocí Windows Powershellu

Chcete-li upravit postupu runbook používat Windows PowerShell, pomocí editoru podle svého výběru a si ji uložit do souboru .ps1. Můžete použít rutinu [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) k načtení obsah postupu runbook a poté rutinu [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) nahradit stávající postupu runbook koncept změněné.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>K načtení obsah postupu Runbook pomocí Windows Powershellu

Následující ukázkové příkazy zobrazení, jak lze získat skript postupu runbook a uložte soubor skriptu. V tomto příkladu je načtena verze konceptu. Je také možné k načtení publikovaná verze postupu runbook sice nemůžete změnit tuto verzi.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Chcete-li změnit obsah postupu Runbook pomocí Windows Powershellu

Následující ukázkové příkazy ukazují, jak chcete nahradit existující obsah postupu runbook s obsahem soubor skriptu. Poznámka: Toto je stejný postup výběru jako [k importu postupu runbook z soubor skriptu prostředí Windows PowerShell](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Související články

- [Vytváření nebo do ní importují postupu runbook v Azure automatizaci](automation-creating-importing-runbook.md)
- [Přehled výukových prostředí PowerShell pracovního postupu](automation-powershell-workflow.md)
- [Grafické authoring v Azure automatizaci](automation-graphical-authoring-intro.md)
- [Certifikáty](automation-certificates.md)
- [Připojení](automation-connections.md)
- [Přihlašovací údaje](automation-credentials.md)
- [Plány](automation-schedules.md)
- [Proměnné](automation-variables.md)