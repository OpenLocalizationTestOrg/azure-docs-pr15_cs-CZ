<properties 
   pageTitle="Typy postupu Runbook Azure automatizaci"
   description="Popisuje různé typy runbooks využívající v Azure automatizace a důležité informace, které byste měli vzít v úvahu při určování, který typ použít. "
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
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure typy postupu runbook automatizaci

Azure automatizaci podporuje čtyři typy runbooks, které jsou popsány v následující tabulce.  Následující části obsahují další informace o jednotlivých typech včetně aspektech se používají.


| Typ |  Popis |
|:---|:---|
| [Grafické](#graphical-runbooks) | Na základě prostředí Windows PowerShell a vytvořené a upravené úplně v grafické editor Azure portálu. | 
| [Grafické prostředí PowerShell pracovního postupu](#graphical-runbooks) | Podle pracovního postupu pro Windows PowerShell a vytvářejí a upravují úplně v editoru grafické Azure portálu. 
| [Prostředí PowerShell](#powershell-runbooks) | Text postupu runbook podle skriptu prostředí Windows PowerShell.
| [Pracovní postup prostředí PowerShell](#powershell-workflow-runbooks) | Text postupu runbook podle pracovního postupu pro Windows PowerShell. |


## <a name="graphical-runbooks"></a>Grafické runbooks

[Grafické](automation-runbook-types.md#graphical-runbooks) a grafické pracovního postupu prostředí PowerShell runbooks se vytvářejí a upravují grafické Editor Azure portálu.  Export do souboru a pak je naimportujte do jiného účtu automatizaci, ale nemůžete vytvářet ani upravovat v jiném nástroji.  Grafické runbooks vygenerování kódu pro PowerShell, ale nemůžete přímo zobrazit nebo upravit kód. Grafické runbooks nelze převést na některý z [formátování textu](automation-runbook-types.md)ani grafickém formátu může být převeden postupu runbook text. Grafické runbooks lze převést na runbooks grafické prostředí PowerShell pracovního postupu při importu a naopak.

### <a name="advantages"></a>Výhody

- Vytvoření runbooks s minimálními znalost [Powershellu](automation-powershell-workflow.md).
- Vizuálně znázorňovat postupy pro řízení.
- Další runbooks lze zahrnout jako podřízené runbooks můžete vytvořit vysoké úrovně pracovní postupy.


### <a name="limitations"></a>Omezení

- Nemůžete upravovat postupu runbook mimo Azure portálu.
- Může vyžadovat kód aktivitu obsahující prostředí PowerShell kód provádět komplexní logiky.
- Nemůžete zobrazit ani upravit přímo Powershellu kód, který je vytvořený grafické pracovního postupu. Všimněte si, že můžete zobrazit kód, který vytvoříte v jakékoli kód činnosti.


## <a name="powershell-runbooks"></a>Runbooks prostředí PowerShell

Prostředí PowerShell runbooks vycházejí z prostředí Windows PowerShell.  Přímo upravit kód v textovém editoru Azure portálu postupu runbook.  Můžete také všechny offline textovém editoru a [import postupu runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) do Azure automatizaci.

### <a name="advantages"></a>Výhody

- Provádění všech komplexní logiky kódem prostředí PowerShell bez dalších složitostí prostředí PowerShell pracovního postupu. 
- Postupu Runbook spustí rychleji než grafické nebo pracovního prostředí PowerShell runbooks vzhledem k tomu, že není potřeba sestavují před spuštěním.

### <a name="limitations"></a>Omezení

- Musíte znát skriptů Powershellu.
- Chcete-li provést více akcí paralelně nelze použít [souběžné zpracování](automation-powershell-workflow.md#parallel-processing) .
- Nelze použít [kontroly](automation-powershell-workflow.md#checkpoints) pokračovat postupu runbook v případě chyby.
- Pracovní postup prostředí PowerShell runbooks a grafické runbooks pouze lze zahrnout jako podřízené runbooks pomocí rutiny Start AzureAutomationRunbook, která vytvoří novou úlohu.

### <a name="known-issues"></a>Známé problémy
Následující části jsou aktuální známé problémy s runbooks Powershellu.

- Prostředí PowerShell runbooks nelze nemůže získat nešifrovaném [proměnné majetku](automation-variables.md) s nulovou hodnotou.
- Prostředí PowerShell runbooks nemůže získat [proměnné majetku](automation-variables.md) s *~* v názvu.
- Get-proces ve smyčce v prostředí PowerShell postupu runbook může docházet k chybám po 80 iteracích. 
- Prostředí PowerShell postupu runbook nemusí podařit, pokud se pokusí zápis velmi velké množství dat do datového proudu výstup najednou.   Alternativním řešením tohoto problému můžete obvykle podle výstupu pouze informace, které budete potřebovat při práci s velké objekty.  Místo výstupu nějak *Procesu načíst*, můžete například výstupu jen požadovaná pole s *procesu načíst | Vyberte název_procesu procesoru*.

## <a name="powershell-workflow-runbooks"></a>Runbooks prostředí PowerShell pracovního postupu

Pracovní postup prostředí PowerShell runbooks jsou runbooks text podle [Pracovního postupu pro Windows PowerShell](automation-powershell-workflow.md).  Přímo upravit kód v textovém editoru Azure portálu postupu runbook.  Můžete také všechny offline textovém editoru a [import postupu runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) do Azure automatizaci.

### <a name="advantages"></a>Výhody

- Provedení všech komplexní logiky kódem prostředí PowerShell pracovního postupu.
- Použití [kontroly](automation-powershell-workflow.md#checkpoints) pokračovat postupu runbook v případě chyby.
- Umožňuje provést více akcí paralelně [souběžné zpracování](automation-powershell-workflow.md#parallel-processing) .
- Může obsahovat další grafické runbooks a pracovní postup prostředí PowerShell runbooks jako podřízené runbooks můžete vytvořit vysoké úrovně pracovní postupy.


### <a name="limitations"></a>Omezení

- Autor musíte znát prostředí PowerShell pracovního postupu.
- Postupu Runbook musí zacházet s další složitost pracovního prostředí PowerShell například [rekonstruován objekty](automation-powershell-workflow.md#code-changes).
- Postupu Runbook trvá déle, protože potřebuje sestavují před spuštěním zahájíte než runbooks Powershellu.
- Prostředí PowerShell runbooks pouze lze zahrnout jako podřízené runbooks pomocí rutiny Start AzureAutomationRunbook, která vytvoří novou úlohu.


## <a name="considerations"></a>Co byste měli zvážit

Byste měli vzít v úvahu následující další aspekty při určování, který typ použít pro konkrétní postupu runbook.

- Runbooks z grafické nelze převést na textový typ nebo naopak.
- Existuje omezení použití runbooks různých typů jako podřízené postupu runbook.  Další informace najdete v tématu [podřízené runbooks v Azure automatizaci](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Další kroky

- Další informace o vytváření grafické postupu runbook, najdete v článku [vytváření grafické v Azure automatizaci](automation-graphical-authoring-intro.md)
- Pro pochopení rozdíly mezi prostředí PowerShell a prostředí PowerShell pracovní postupy pro runbooks, najdete v článku [Pracovní postup výukové Windows PowerShell](automation-powershell-workflow.md)
- Další informace o tom, jak vytvářet nebo importovat postupu Runbook najdete v tématu [vytváření nebo do ní importují postupu Runbook](automation-creating-importing-runbook.md)



