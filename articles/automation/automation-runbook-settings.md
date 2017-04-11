<properties 
   pageTitle="Nastavení postupu Runbook"
   description="Popisuje nastavení postupu runbook v Azure automatizaci a jak lze změnit pomocí portálu Správa Azure a prostředí Windows PowerShell."
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

# <a name="runbook-settings"></a>Nastavení postupu Runbook

Každý postupu runbook v Azure automatizaci obsahuje několik nastavení, která pomáhají identifikovat a změnit chování protokolování. Každá z těchto nastavení je popsána níže následovaný postupy o tom, jak je upravit.

## <a name="settings"></a>Nastavení

### <a name="name-and-description"></a>Název a popis

Název postupu runbook nelze změnit po jeho vytvoření. Popis vynechán a může být 512 znaků.

### <a name="tags"></a>Značky

Značky umožňují přiřadit význačná slova a fráze k identifikaci postupu runbook. Například po odeslání postupu runbook do [Galerie postupu Runbook](https://msdn.microsoft.com/library/dn781422.aspx)zadat určité značky k identifikaci kategorií, které by měl být podle postupu runbook. Zadání více značek postupu runbook oddělte je čárkami.

### <a name="logging"></a>Zapnout protokolování

Ve výchozím nastavení nejsou záznamy podrobné a průběhu zapsán do historie úlohy. Můžete změnit nastavení pro konkrétní postupu runbook přihlásit tyto záznamy. Další informace o těchto záznamů najdete v tématu [postupu Runbook výstup a zprávy](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Změna nastavení postupu runbook

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Změna nastavení postupu runbook pomocí portálu pro správu Azure

Změna nastavení pro postupu runbook v portálu pro správu Azure na stránce **Konfigurovat** pro postupu runbook.

1. Na portálu Správa Azure vyberte **automatizace** a potom klikněte na položku název účtu, který automatizaci.
1. Vyberte kartu **Runbooks** .
1. Klikněte na název postupu runbook.
1. Vyberte kartu **Konfigurovat** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Změna nastavení postupu runbook používat Windows PowerShell

Chcete-li změnit nastavení postupu runbook můžete použít rutinu [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) . Pokud chcete zadat více značek, můžete buď poskytnout maticových nebo jeden řetězec hodnoty oddělené čárkami parametru značky. Získáte aktuální značky s [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Následující ukázkové příkazy ukazují, jak nastavit vlastnosti postupu runbook. Tato ukázka přidá tři značek do existující značky a určuje, že by měl ukládány podrobného záznamy.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Související články
- [Výstup postupu Runbook a zprávy](../automation-runbook-output-and-messages) 
- [Vytváření nebo do ní importují postupu Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 