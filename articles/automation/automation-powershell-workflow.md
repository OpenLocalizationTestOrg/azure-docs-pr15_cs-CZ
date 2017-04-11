<properties 
   pageTitle="Výukové prostředí PowerShell pracovního postupu"
   description="Tento článek je určená jako rychlý výuky autoři známé prostředí PowerShell znát určité rozdíly mezi prostředí PowerShell a prostředí PowerShell pracovního postupu."
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

# <a name="learning-windows-powershell-workflow"></a>Výukové Windows PowerShell pracovního postupu

Runbooks v Azure automatizaci jsou implementovaná jako Windows PowerShell pracovní postupy.  Pracovní prostředí PowerShell systému Windows je podobný skriptu prostředí Windows PowerShell, ale mám některé významné rozdíly, které může být matoucí novému uživateli.  Tento článek je určená pro uživatele už znáte prostředí PowerShell a stručně popisuje koncepty vyžadují Pokud převádíte skript Powershellu pracovního prostředí PowerShell pro použití v postupu runbook.  

Pracovní postup je několik kroků naprogramované, připojení, které úkoly – náročné nebo vyžadují koordinaci několika kroky ve víc zařízeních nebo spravovaných uzlů. Výhody pracovního postupu na normální skriptu patří možnost souběžně prováděl určitou akci proti několika zařízeních a možnost automaticky obnovit z k chybám. Pracovní prostředí PowerShell systému Windows je skriptu prostředí Windows PowerShell, který využívá modelu Windows Workflow Foundation. Během pracovního postupu je napsané pomocí prostředí Windows PowerShell syntaxe a spuštění pomocí prostředí Windows PowerShell, zpracován modelu Windows Workflow Foundation.

Podrobné informace o témata v tomto článku najdete v článku [Začínáme s pracovním postupem Windows PowerShell](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Typy postupu runbook

Existují tři typy postupu runbook v Azure automatizaci, *Prostředí PowerShell pracovního postupu*, *prostředí PowerShell* a *grafické*.  Když vytvoříte postupu runbook a postupu runbook nelze převést na jiný typ je vytvořené definujete typ postupu runbook.

Prostředí PowerShell pracovního postupu runbooks a runbooks Powershellu pro uživatele, kteří chcete pracovat přímo kód prostředí PowerShell buď používáte textový editor v Azure automatizaci nebo offline editor například PowerShell ISE. Pokud vytváříte prostředí PowerShell pracovního postupu runbook je třeba porozumět informace v tomto článku. 

Grafické runbooks umožňují vytvářet postupu runbook používáte stejné aktivity a rutinách, ale pomocí grafického rozhraní, které slouží ke skrytí složitostí podkladového pracovního postupu Powershellu.  Koncepty v tomto článku například kontrolních bodů a paralelního spuštění pořád pro grafické runbooks vhodná, ale nebudete mít obavu podrobné syntaxe. 

## <a name="basic-structure-of-a-workflow"></a>Základní struktura pracovního postupu

Prvním krokem při převodu skript Powershellu prostředí PowerShell pracovního postupu je uveden pomocí klíčového slova **pracovního postupu** .  Spuštění pracovního postupu **pracovní postup** klíčovým slovem následuje text skriptu uzavřeny v závorkách. Název pracovního postupu sleduje klíčového slova **pracovního postupu** , jak ukazuje následující syntaxi. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Název pracovního postupu se musí shodovat název automatizaci postupu runbook. Pokud je v průběhu importu postupu runbook název souboru název pracovního postupu se musí shodovat a musí končit .ps1.

Pokud chcete přidat parametry pracovního postupu, použijte klíčové slovo **parametr** stejným způsobem skriptu. 

## <a name="code-changes"></a>Změny kódu

Prostředí PowerShell pracovního postupu kódu téměř stejný kód skript Powershellu s výjimkou několik významné změny.  Následující oddíly popisují změny, které budete muset tak, abyste skript Powershellu pro něj spuštění pracovního postupu.

### <a name="activities"></a>Aktivity

Aktivita je s konkrétním úkolem v pracovním postupu. Stejně jako skript se skládá z jednoho nebo více příkazů, pracovního postupu se skládá z jedné nebo více činnosti, které se provádí v pořadí. Pracovního postupu pro Windows PowerShell automaticky převede spoustu rutin prostředí Windows PowerShell a aktivit při spuštění pracovního postupu. Při zadání jednu z těchto rutin v vaší postupu runbook odpovídající aktivitu skutečně spuštěnými modelu Windows Workflow Foundation. U těchto rutin bez odpovídající aktivity pracovního postupu pro Windows PowerShell automaticky spustí se v rámci [InlineScript](#inlinescript) aktivity. Je sada rutinách vyloučíte a nelze použít v pracovním postupu, pokud je explicitně zahrnout do bloku InlineScript. Další informace o těchto koncepty najdete v článku [Použití aktivity v pracovních postupech skriptu](http://technet.microsoft.com/library/jj574194.aspx).

Činnosti pracovního postupu sdílejí sadu běžné parametry pro konfiguraci jejich operace. Podrobnosti o běžných parametry pracovního postupu najdete v tématu [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Parametry pozice

Parametry pozice nelze použít s aktivity a rutin v pracovním postupu.  To znamená je, že je nutné použít názvy parametrů.

Například zvažte následující kód, který získá všechny spuštěné služby.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Pokud se pokusíte spustit tento téhož kódu v pracovním postupu, dostanete zprávu jako "nastavení parametru nelze přeložit pomocí zadaného pojmenovaných parametrů."  Tento problém můžete vyřešit, jednoduše zadejte název parametru jako v následujícím příkladu.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Rekonstruované objektů

Objekty v pracovních postupech jsou rekonstruován.  To znamená, že jejich vlastnosti jsou pořád dostupné, ale ne jejich metody.  Například zvažte následující kód Powershellu, která zastaví službu metodou zastavit službu objektu.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Když zkusíte spustit tento pracovní postup, zobrazí se takováto chybová sdělují "vyvolání metody není podporováno v pracovním postupu Windows PowerShell".  

Jednou z možností je zalomení tyto dva řádky kódu v bloku [InlineScript](#InlineScript) v takovém případě $Service bude objekt služby v rámci bloku. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Další možností je rutina jiného, které provede stejné funkce jako prostředek, pokud je k dispozici.  V případě našimi ukázkovými rutinu zastavit službu funkčně jako prostředek ukončit a můžete použít následující pracovního postupu.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

Aktivity **InlineScript** je užitečná, když je potřeba spustit jednu nebo více příkazů jako tradiční skript Powershellu místo prostředí PowerShell pracovního postupu.  Během příkazy v pracovním postupu se odesílají modelu Windows Workflow Foundation pro zpracování, příkazy v bloku InlineScript jsou zpracovány pomocí prostředí Windows PowerShell. 

InlineScript syntaxí ukázáno v následujícím příkladu.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Chcete se vrátit výstup z InlineScript přiřazením výstup proměnné. Následující příklad zastaví službu a poté uloží název služby.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Předání hodnot do bloku InlineScript, ale je nutné použít **$Using** obor modifikátor.  V následujícím příkladu je stejné jako v předchozím příkladu, s tím rozdílem, že název služby poskytuje společnost proměnné. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Když InlineScript aktivity můžou kritické v některých pracovních postupů, nepodporují konstrukce pracovního postupu a lze používat pouze v případě potřeby z těchto důvodů:

- Nelze použít [kontroly](#Checkpoints) uvnitř bloku InlineScript. Pokud v rámci bloku dojde k selhání, musíte obnovení od začátku blokování.
- Nelze použít [paralelního spuštění](#parallel-execution) uvnitř InlineScriptBlock.
- InlineScript ovlivňuje škálovatelnost pracovní postup od obsahuje relace prostředí Windows PowerShell pro celý délku blok InlineScript.

Další informace o použití InlineScript naleznete v tématu [Systém Windows PowerShell příkazy v pracovním postupu](http://technet.microsoft.com/library/jj574197.aspx) a [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Paralelní zpracování

Jednou z výhod Windows PowerShell pracovní postupy je provádět sady příkazů paralelně namísto sebou s typické skriptu. 

**Paralelní** klíčové slovo slouží k vytvoření blok skriptu s více příkazů, které se spustí současně. To syntaxí ukázáno v následujícím příkladu. V tomto případě Activity1 a Activity2 začnou ve stejnou dobu. Activity3 začnou až poté, co jste dokončili Activity1 a Activity2.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Například zvažte následující příkazy Powershellu, které zkopírovat více souborů do cíle sítě.  Tyto příkazy jsou postupně spustit, takže než jednoho souboru musí být dokončen kopírování před tím, než na další.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Následující pracovní postup spuštěn stejné příkazy paralelně tak, aby všechny začínaly kopírování ve stejnou dobu.  Až poté, co jsou všechny úplně zkopírovali je dokončení zobrazí zpráva o.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Můžete použít **ForEach – paralelní** konstrukce k příkazům procesu pro každou položku v kolekci současně. Položky v kolekci se provádí souběžně při spuštění příkazy v bloku skriptu postupně. To syntaxí ukázáno v následujícím příkladu. V tomto případě Activity1 začnou ve stejnou dobu u všech položek v kolekci. Pro každou položku Activity2 začnou po dokončení Activity1. Activity3 začnou až po Activity1 a Activity2 dokončili u všech položek.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Následující příklad je podobný jako v předchozím příkladu kopírování souborů současně.  V tomto případě zobrazí pro každý soubor po zkopírováním.  Až poté, co jsou všechny úplně zkopírovali zpráva závěrečnou kompletní zobrazený.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Nedoporučujeme systém podřízené runbooks paralelně od má bylo prokázáno nedůvěryhodných výsledkům.  Výstup postupu runbook podřízené někdy se neobjeví a nastavení v postupu runbook jedno dítě může ovlivnit jiné paralelní podřízené runbooks 


## <a name="checkpoints"></a>Kontrolních bodů

*Kontrolní bod* poskytuje zobrazení aktuálního stavu pracovního postupu, který obsahuje aktuální hodnotu proměnné a všechny výstup generovaný k tomuto bodu. Pokud pracovní postup končí zobrazuje chyba nebo je pozastavené, pak při příštím spuštění začne jeho poslední kontroly místo start worfklow.  Kontrolní bod můžete nastavit v pracovním postupu s aktivitou **Kontrolní bod pracovního postupu** .

V následující ukázce kódu výjimku vyskytne po Activity2 příčinou pracovní postup ukončit. Když pracovní postup je znovu spustit, spustí se spuštěním Activity2, protože to byla jen po poslední kontroly.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Měli byste nastavit kontrolních bodů v pracovním postupu po činnosti, které mohou být chybám výjimky a by neměly být opakuje, pokud je pracovní postup obnoven. Pracovní postup může například vytvořit virtuální počítač. Kontrolní bod můžete nastavit před a po příkazy k vytvoření virtuálního počítače. Pokud se nezdaří vystavením by příkazy opakovat, pokud pracovní postup je spuštěn znovu. Pokud worfklow selže po úspěšném vystavením pak virtuální počítač nevytvoří znovu při obnovení pracovního postupu.

Následující příklad zkopíruje najednou několik souborů do síťové umístění a nastaví kontrolní bod po jednotlivých souborů.  Pokud dojde ke ztrátě umístění v síti, bude pracovní postup končí řetězcem chyby.  Pokud je spuštěn znovu, bude pokračovat v poslední kontroly, což znamená, že bude přeskočilo pouze soubory, které již byly zkopírovány.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Protože uživatelské jméno přihlašovací údaje nejsou trvalý po telefonickém hovoru s aktivity [Pracovní postup pozastavit](https://technet.microsoft.com/library/jj733586.aspx) nebo za poslední kontroly, budete muset nastavit přihlašovací údaje na hodnotu null a potom načíst je znova z obchodu materiálů po **Pracovní postup pozastavit** nebo kontrolní bod.  Jinak, může zobrazit tato chybová zpráva: *úkoly pracovního postupu nejde obnovit, buď protože trvalé dat nelze úplně uložit nebo uložit trvalé dat došlo k poškození. Restartování pracovního postupu.*

Následující téhož kódu ukazuje, jak pro tento pracovní postup prostředí PowerShell runbooks.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Tento krok není povinný, pokud používáte spustit jako účet nakonfigurována služby základní ověřujete.  

Další informace o kontroly najdete v článku [Přidání kontroly skriptu pracovního postupu](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Další kroky

- Začínáme s runbooks prostředí PowerShell pracovního postupu, najdete v článku [svůj první postupu runbook prostředí PowerShell pracovního postupu](automation-first-runbook-textual.md) 
