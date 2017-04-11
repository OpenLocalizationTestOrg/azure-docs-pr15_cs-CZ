<properties 
   pageTitle="Podřízené runbooks v Azure automatizaci | Microsoft Azure"
   description="Popisuje různé metody počínaje postupu runbook v Azure automatizaci jiné postupu runbook a sdílení informací mezi nimi."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Podřízené runbooks v Azure automatizaci

Je doporučený postup v Azure automatizaci psát opakovaně použitelný, moduly runbooks s samostatná funkce, která může používat jiné runbooks. Nadřazené postupu runbook často zavolá jeden nebo více runbooks podřízené provádět požadovanou funkci. Existují dva způsoby kontaktování postupu runbook podřízené a má každá výrazných rozdílů, které je třeba porozumět tak, že můžete určit, která bude nejvíc různých scénářích.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Vyvolání postupu runbook podřízené pomocí vloženého spuštění

Volat do řádku postupu runbook z jiného postupu runbook, nesou název této postupu runbook a poskytují hodnoty parametrů přesně tak, jako byste používali aktivity nebo rutiny.  Všechny runbooks ve stejném účtu automatizaci jsou k dispozici všem ostatním použít tímto způsobem. Postupu runbook nadřazené počká postupu runbook podřízené dokončete teprve pak přejděte na další řádek a všechny výstup je vrácen přímo do nadřazeného.

Pokud vyvoláte vložené postupu runbook běží ve stejné úkolu jako nadřazený postupu runbook. Nastane bez uvedení do seznamu historie úlohy postupu runbook podřízené, který ji spustil. Bude přidružený k nadřazeného požadované výjimky a všechny toku výstup z podřízené postupu runbook. Výsledkem méně úlohy a usnadňují jejich použití sledovat a řešit problémy s od požadované výjimky vyvolané postupu runbook podřízené a všech jeho výstup toku jsou přidružené k projektu nadřazené.

Po publikování postupu runbook všechny podřízené runbooks, která volá musí být publikované. Je to proto Azure automatizaci vytvoří přidružení s všechny podřízené runbooks při kompilovaný postupu runbook. Pokud nejsou, postupu runbook nadřazené zobrazí publikovat správně, ale vygeneruje výjimky při spuštění. V takovém případě můžete znovu publikovat postupu runbook nadřazené správně odkázat runbooks podřízené. Není potřeba publikovat postupu runbook nadřazené všech podřízených runbooks změny protože přidružení se již byly vytvořeny.

Parametry postupu runbook podřízené s názvem vložené může být libovolný typ dat, včetně složité objekty a není bez [serializaci JSON](automation-starting-a-runbook.md#runbook-parameters) jako při spuštění postupu runbook na portálu Správa Azure nebo s rutinu Start AzureRmAutomationRunbook.


### <a name="runbook-types"></a>Typy postupu Runbook

Jaké typy můžete volat sobě:

- [Postupu runbook prostředí PowerShell](automation-runbook-types.md#powershell-runbooks) a [grafické runbooks](automation-runbook-types.md#graphical-runbooks) upoutat každý další vložené (obě jsou Powershellu na základě).
- [Prostředí PowerShell pracovního postupu runbook](automation-runbook-types.md#powershell-workflow-runbooks) a grafické pracovního postupu prostředí PowerShell runbooks upoutat každý další vložené (obě jsou prostředí PowerShell pracovního postupu založeného)
- Typy prostředí PowerShell typů pracovních postupů prostředí PowerShell nelze volat vzájemně vložené a musíte použít Start AzureRmAutomationRunbook.
    
Při publikování pořadí věci:

- Publikovat pořadí runbooks pouze pro pracovní postup prostředí PowerShell a grafické pracovního postupu prostředí PowerShell runbooks otázkách.


Při volání grafické nebo pracovního prostředí PowerShell podřízené postupu runbook pomocí vložené používáte jenom název postupu runbook.  Při volání postupu runbook podřízené prostředí PowerShell musí před jeho název s *.\\ * Chcete-li určit, že skript se nachází v místním adresáři. 

### <a name="example"></a>Příklad

Následující příklad vyvolá podřízené postupu runbook test, která přijímá tři parametry, složité objekt, celé a logická hodnota. Výstup postupu runbook podřízené se přiřadí proměnné.  V tomto případě postupu runbook podřízené je prostředí PowerShell pracovního postupu runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Toto je stejný příklad pomocí prostředí PowerShell postupu runbook jako podsložky.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Spuštění postupu runbook podřízené pomocí rutiny

Můžete použít rutinu [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) způsobem popsaným v tématu [spuštění postupu runbook používat Windows PowerShell](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell)spusťte postupu runbook. Existují dva způsoby použití pro tuto rutinu.  V jednom režimu rutinu vrátí id úlohy hned podřízené úlohy se vytvoří postupu runbook podřízené.  V ostatních režimu, což povolíte tak, že zadá **-čekat** parametr, rutinu počkat do podsložky projektu dokončení a vrátí výstup z podřízené postupu runbook.

Úlohy z podřízené postupu runbook začít rutina poběží v samostatné projektu z nadřazeného postupu runbook. Výsledkem víc práce než vyvolání vložené postupu runbook a vytváří obtížnější ke sledování. Nadřazený můžete začít více runbooks podřízené asynchronní bez nutnosti čekání u každé dokončení. Pro tuto stejný druh paralelního spuštění volání vložené runbooks podřízené postupu runbook nadřazené třeba použít [paralelní klíčových slov](automation-powershell-workflow.md#parallel-processing).

Parametry pro podřízené postupu runbook začít rutina slouží jako hashtable popsanou v [Parametrech postupu Runbook](automation-starting-a-runbook.md#runbook-parameters). Lze použít pouze jednoduché datové typy. Pokud postupu runbook má parametr složité datový typ, pak musí být jmenuje vložené.

### <a name="example"></a>Příklad

V následujícím příkladu postupu runbook podřízené začíná parametry a poté na něj čeká dokončete pomocí Start AzureRmAutomationRunbook-čekat parametr. Po dokončení jeho výstup odebrané podřízené postupu runbook.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Porovnání metod pro volání postupu runbook podřízené

Následující tabulka shrnuje rozdíly mezi těmito dvěma metodami pro volání postupu runbook z jiného postupu runbook.

| | Vložené| Rutina|
|:---|:---|:---|
|Úlohy|Podřízené runbooks spustit do stejné úlohy jako nadřazený.|Samostatné úlohy se vytvoří podřízené postupu runbook.|
|Spuštění|Nadřazené postupu runbook čeká postupu runbook podřízené dokončit před pokračováním.|Nadřazené postupu runbook budou dál problémy hned po spuštění postupu runbook podřízené, že *nebo* postupu runbook nadřazené čeká podřízené úkoly k dokončení.|
|Výstup|Nadřazené postupu runbook přímo dostali výstup z podřízené postupu runbook.|Nadřazené postupu runbook musíte získat výstup z podřízené postupu runbook projektu *nebo* postupu runbook nadřazené přímo dostali výstup z podřízené postupu runbook.|
|Parametry|Hodnoty parametrů postupu runbook podřízené jsou určeny samostatně a můžete použít libovolný datový typ.|Hodnoty pro parametry postupu runbook podřízené musí být sloučena do jednoho hashtable a mohou obsahovat pouze jednoduché, pole a data objektu typy této serializaci JSON účinek.|
|Automatizace účet|Nadřazené postupu runbook můžete použít jenom postupu runbook podřízené ve stejném automatizaci účtu.|Nadřazené postupu runbook můžete použít postupu runbook podřízené z libovolného účtu automatizaci ze stejného Azure předplatné a dokonce i jiné předplatné, pokud máte připojení k ní.|
|Publikování|Podřízené postupu runbook musí být publikováno před publikováním nadřazené postupu runbook.|Podřízené postupu runbook musí publikovat kdykoli před tím, než nadřazené postupu runbook.|

## <a name="next-steps"></a>Další kroky

- [Spuštění postupu runbook v Azure automatizaci](automation-starting-a-runbook.md)
- [Výstup postupu Runbook a zprávy v Azure automatizaci](automation-runbook-output-and-messages.md)
