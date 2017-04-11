<properties 
    pageTitle="Testování postupu runbook v Azure automatizaci | Microsoft Azure"
    description="Před publikováním postupu runbook v Azure automatizaci, můžete ji můžete otestovat zajistit, že funguje očekávaným způsobem.  Tento článek popisuje, jak test postupu runbook a zobrazit její výsledek."
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

# <a name="testing-a-runbook-in-azure-automation"></a>Testování postupu runbook v Azure automatizaci
Při testování postupu runbook spouštět [pracovní verze](automation-creating-importing-runbook.md#publishing-a-runbook) a po dokončení všech akcí, které provede. Historie úlohy se vytvoří, ale datových proudů [výstup](automation-runbook-output-and-messages.md#output-stream) a [chyb a upozornění](automation-runbook-output-and-messages.md#message-streams) se zobrazí ve sloupci Podmínka výstup podokno. Zobrazení zpráv [podrobného toku](automation-runbook-output-and-messages.md#message-streams) v podokně výstup jenom v případě, že [$VerbosePreference proměnné](automation-runbook-output-and-messages.md#preference-variables) je nastavena na pokračovat.

Přestože spuštění pracovní verze postupu runbook pořád spustí pracovní postup zpravidla a provede akcích proti zdrojů v prostředí. Z tohoto důvodu testujete pouze runbooks na testovacím zdroje.

Postup pro každý [Typ postupu runbook](automation-runbook-types.md) testu je stejný a není žádný rozdíl v testování mezi textový editor a editoru grafické Azure portálu.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Chcete-li otestovat postupu runbook na portálu Azure

Můžete pracovat s libovolným [postupu runbook typ](automation-runbook-types.md) Azure portálu.

1. Otevřete pracovní verze postupu runbook v [textový editor](automation-editing-a-runbook.md#Portal) nebo [grafický editor](automation-graphical-authoring-intro.md).
2. Klikněte na tlačítko **Testovat** otevřete zásuvné testu.
3. Pokud postupu runbook parametry, budou uvedené v levém podokně, kde můžete zadat hodnoty pro test.
4. Pokud chcete spustit test [Hybridní pracovního postupu Runbook](automation-hybrid-runbook-worker.md), nastavením možností **Spuštění** **Hybridní** pracovníka a vyberte název cílové skupiny.  V opačném zachovat výchozí **Azure** testu v cloudu.
5. Klikněte na tlačítko **Spustit** spusťte test.
6. Pokud postupu runbook je [Pracovní postup prostředí PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) nebo [grafické](automation-runbook-types.md#graphical-runbooks), můžete zastavit nebo pozastavit, pokud je zkouší pomocí tlačítek pod podokno výstup. Pokud pozastavení postupu runbook dokončí aktuální činnosti před pozastavení. Jakmile je pozastavené postupu runbook, můžete ho zastavit nebo restartujte ho.
7. Zkontrolujte výstup postupu runbook v podokně výstupu.


## <a name="next-steps"></a>Další kroky

- Naučte se vytvářet nebo importovat postupu runbook, najdete v tématu [vytváření nebo import postupu runbook v Azure automatizaci](automation-creating-importing-runbook.md)
- Další informace o vytváření grafické, najdete v článku [vytváření grafické v Azure automatizaci](automation-graphical-authoring-intro.md)
- Začínáme s runbooks prostředí PowerShell pracovního postupu, najdete v článku [svůj první postupu runbook prostředí PowerShell pracovního postupu](automation-first-runbook-textual.md)
- Další informace o konfiguraci runboks k vrácení zprávy o stavu a chyby, včetně doporučené postupy, přečtěte si téma [postupu Runbook výstup a zprávy v Azure automatizaci](automation-runbook-output-and-messages.md)