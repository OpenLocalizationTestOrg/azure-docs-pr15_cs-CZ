<properties 
   pageTitle="Plány v Azure automatizaci | Microsoft Azure"
   description="Automatizace plány se používají k plánování runbooks v Azure automatizaci, aby se spouštěl automaticky. Popisuje, jak vytvářet a spravovat plánu ve tak, aby automaticky vyjít postupu runbook v určitou dobu nebo na plán opakování."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Plánování postupu runbook v Azure automatizaci

Naplánování postupu runbook v Azure automatizaci na začít od určitého času můžete propojit jeden nebo více plány. Plán můžete nakonfigurovat tak, aby běžel jednorázovou nebo předešli opakování problému každou hodinu nebo denní plán pro runbooks Azure klasické portálu a runbooks na portálu Azure, můžete kromě naplánovat jejich pro týdně, měsíčně, konkrétní dny v týdnu nebo dny v měsíci nebo určitý den v měsíci.  Postupu runbook můžou být propojené s více plánů a plán může obsahovat více runbooks s klientem propojený.

>[AZURE.NOTE]  Plány aktuálně nepodporují DSC Azure automatické konfigurace.

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí PowerShell systému Windows

Rutiny pro správu v následující tabulce slouží k vytváření a Správa plánů používat Windows PowerShell v Azure automatizaci. Odesláním jako součást [modul Azure Powershellu](../powershell-install-configure.md).

|Rutiny pro správu|Popis|
|:---|:---|
|**Azure rutin Správce zdrojů**||
|[Get-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Načte plánu.|
|[Nové AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Vytvoří nový plán.|
|[Odebrat AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Odebere plánu.|
|[Nastavení AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|Nastavení vlastností pro existující plán.|
|[Get-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Načte naplánované runbooks.|
|[Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Přiřadí postupu runbook plánu.|
|[Zrušení registrace AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Dissociates postupu runbook z plánu.|
|**Azure rutiny pro správu služby**||
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Načte plánu.|
|[Nové AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Vytvoří nový plán.|
|[Odebrat AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Odebere plánu.|
|[Nastavení AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|Nastavení vlastností pro existující plán.|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Načte naplánované runbooks.|
|[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Přiřadí postupu runbook plánu.|
|[Zrušení registrace AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Dissociates postupu runbook z plánu.|

## <a name="creating-a-schedule"></a>Vytvoření plánu

Vytvoření nového plánu pro runbooks na portálu Azure na portálu klasické nebo přes Windows PowerShell. Máte taky možnost vytvořit nový plán po propojení postupu runbook plán na portálu Azure klasické nebo Azure.

>[AZURE.NOTE] Po přidružení plánu s postupu runbook automatizaci ukládá s aktuálními verzemi moduly ve vašem účtu a odkazy na této plánu.  To znamená, že pokud měli modulu s verze 1.0 ve vašem účtu, když jste vytvořili plán a pak aktualizujte modulu verze 2.0, plánu bude dál používat 1.0.  Abyste mohli používat verzi aktualizované modul, musíte vytvořit nový plán. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Vytvoření nového plánu v portálu Azure

1. Na portálu Azure z účtu automatizaci klikněte na dlaždici **prostředky** otevřete zásuvné **prostředky** .
2. Klikněte na dlaždici **plány** otevřete zásuvné **plány** .
3. Klikněte na **Přidat do plánu** v horní části zásuvné.
4. Na **Nový plán** zásuvné zadejte **název** a volitelně i **Popis** nového kalendáře.
5. Vyberte, zda plánu se spustí jednorázové nebo opakovaném plánu tak, že vyberete **jednou** nebo **opakování**.  Pokud vyberete **jednou** zadat **Počáteční čas** a potom klikněte na **vytvořit**.  Pokud vyberete **opakování**, zadejte **čas zahájení** a četnost, jak často se má postupu runbook opakovat - **hodiny**, **den**, **týden**nebo **měsíc**.  **Týden** nebo **měsíc** v rozevíracím seznamu, vyberete **možnost opakování** se zobrazí v zásuvné při výběru zobrazí **možnost opakování** zásuvné a můžete vybrat den týdne, pokud jste vybrali **týden**.  Pokud jste vybrali **měsíc**, můžete zvolit **pracovních dnů** nebo konkrétní dny, měsíce v kalendáři a nakonec udělejte chcete spustit na poslední den v měsíci nebo Ne a potom klikněte na **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Vytvoření nového plánu Azure klasické portálu

1. Na portálu Azure klasické vyberte automatizace a potom vyberte název účtu, který automatizaci.
1. Vyberte kartu **prostředky** .
1. V dolní části okna klikněte na **Přidat nastavení**.
1. Klikněte na **Přidat plánu**.
1. Zadejte **název** a volitelně **Popis** nového plánu schedule.your spustí **Jednorázové**, **hodinách**, **denní**, **týdenní**nebo **měsíční**.
1. Zadejte **Čas zahájení** a dalších možností v závislosti na typu plánu, který jste vybrali.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Vytvoření nového plánu používat Windows PowerShell

Můžete použít rutinu [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) k vytvoření nového plánu v Azure automatizaci pro klasické runbooks nebo rutinu [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) pro runbooks Azure portálu. Je nutné zadat počáteční čas na plán a požadovanou četnost by měla běžet.

Následující ukázkové příkazy ukazuje, jak chcete vytvořit plán pro 15 a 30 každý měsíc pomocí rutiny správce prostředků Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Následující ukázkové příkazy ukazují, jak vytvořit nový plán, který se spustí každý den na 3:30 odp začínající na 20 ledna 2015 rutina Správa služby Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Propojení postupu runbook plánu

Postupu runbook můžou být propojené s více plánů a plán může obsahovat více runbooks s klientem propojený. Pokud postupu runbook parametry, můžete zadat hodnoty pro ně. Nutné zadat hodnoty všech parametrů, povinná a můžete obdržet hodnoty všech parametrů, volitelné.  Tyto hodnoty se použije při každém spuštění postupu runbook tak, že tento kalendář.  Můžete připojit stejné postupu runbook na jiný plán a zadejte jiné hodnoty parametru.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Chcete vytvořit odkaz plánu postupu runbook pomocí portálu Azure

1. Na portálu Azure z účtu automatizaci klikněte na dlaždici **Runbooks** otevřete zásuvné **Runbooks** .
2. Klikněte na název postupu runbook naplánování.
3. Pokud do plánu není aktuálně propojení postupu runbook, pak bude vám nabídnuta možnost vytvoření nového plánu nebo odkazu na existující plán.  
4. Pokud postupu runbook parametry, můžete vybrat možnost **změnit spustit nastavení (výchozí: Azure)** a zásuvné **parametrů** jsou uvedeny kterého můžete zadat informace příslušným způsobem.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Pokud chcete vytvořit odkaz na plán postupu runbook pomocí portálu Azure klasické

1. Na portálu Azure klasické vyberte **automatizace** a potom klikněte na položku název účtu, který automatizaci.
2. Vyberte kartu **Runbooks** .
3. Klikněte na název postupu runbook naplánování.
4. Klikněte na kartu **plán** .
5. Pokud do plánu není aktuálně propojení postupu runbook, pak budete mít možnost **odkaz na nový plán** nebo **odkaz na existující plán**.  Pokud postupu runbook je propojený na plán, klikněte na **odkaz** v dolní části okna na tyto možnosti jsou dostupné.
6. Pokud postupu runbook parametry, zobrazí se výzva pro jejich hodnoty.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Pokud chcete vytvořit odkaz plánu postupu runbook používat Windows PowerShell

[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) slouží k propojení plánu klasické postupu runbook nebo [Register AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) rutiny pro runbooks Azure portálu.  Zadejte hodnoty postupu runbook parametrů s parametrem parametry. Další informace o zadání hodnoty parametru naleznete v tématu [spuštění postupu Runbook v Azure automatizaci](automation-starting-a-runbook.md) .

Následující ukázkové příkazy ukazují, jak chcete vytvořit odkaz na plán postupu runbook rutina správce prostředků Azure pomocí parametrů.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Následující ukázkové příkazy ukazují, jak propojit plánu pomocí rutiny Správa služby Azure s parametry.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Zakázání plánu

Když zakážete plánu, všechny runbooks s klientem propojený spustí podle tohoto plánu. Můžete ručně zakázat plánu nebo nastavit dobu platnosti pro plány s frekvenci při byla vytvořená. Když vypršení časového limitu, budou zakázány plánu.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Jak zakázat plánu z portálu Microsoft Azure

1. Na portálu Azure z účtu automatizaci klikněte na dlaždici **prostředky** otevřete zásuvné **prostředky** .
2. Klikněte na dlaždici **plány** otevřete zásuvné **plány** .
2. Klikněte na název plánu otevřete zásuvné podrobnosti.
3. Změna **povolený** na hodnotu **Ne**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Jak zakázat plánu z portálu Microsoft Azure klasické

Můžete zakázat plán na portálu Azure klasické na stránce Podrobnosti plánu pro plán.

1. Na portálu Azure klasické vyberte automatizace a potom klikněte na položku název účtu, který automatizaci.
1. Vyberte kartu prostředky.
1. Klikněte na název plánu otevřete její stránku s podrobnostmi.
2. Změna **povolený** na hodnotu **Ne**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Jak zakázat plán s prostředí Windows PowerShell

Chcete-li změnit vlastnosti existující plán pro klasické postupu runbook nebo rutinu [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) pro runbooks Azure portálu můžete použít rutinu [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) . Jak zakázat plánu, zadejte **hodnotu false** parametru **IsEnabled** .

Následující ukázkové příkazy ukazují, jak zakázat plán pro postupu runbook pomocí rutiny správce prostředků Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Následující ukázkové příkazy ukazují, jak zakázat plánu pomocí rutiny Správa služby Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Další kroky

- Začínáme s runbooks v Azure automatizaci, najdete v článku [spuštění postupu Runbook v Azure automatizaci](automation-starting-a-runbook.md) 