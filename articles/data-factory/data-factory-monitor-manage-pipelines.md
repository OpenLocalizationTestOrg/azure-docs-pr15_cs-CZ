<properties 
    pageTitle="Sledování a správa Azure Data Factory potrubí" 
    description="Naučte se používat portál Azure a Azure Powershellu ke sledování a správa dat Azure továrny a potrubí, kterou jste vytvořili." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>


# <a name="monitor-and-manage-azure-data-factory-pipelines"></a>Sledování a správa Azure Data Factory potrubí
> [AZURE.SELECTOR]
- [Použití Azure portál/Azure prostředí PowerShell](data-factory-monitor-manage-pipelines.md)
- [Pomocí sledování a Správa aplikací](data-factory-monitor-manage-app.md)

Služba Data Factory poskytuje spolehlivý a úplné zobrazení pohyb služby úložiště, zpracování a data. Tato služba poskytuje sledování pomáhá řídicích panelů, které můžete dělat tyto věci: 

- Posuďte, rychle začátku do konce datového kanálu stavu.
- Rozpoznání problémy a akce korekční v případě potřeby. 
- Souvisejících zdrojích dat sledování. 
- Sleduje relace mezi daty ve všech svých pramenů.
- Zobrazení úplné historie účtování závislosti, stavu systému a provádění úloh.

Tento článek popisuje, jak sledovat, Správa a ladění vaší kanály. Obsahuje taky informace o tom, jak vytvořit upozornění a oznámení o selhání.

## <a name="understand-pipelines-and-activity-states"></a>Princip činnosti stavy a potrubí
Na portálu Azure, máte tyto možnosti:

- Zobrazení dat factory jako diagram
- Zobrazení aktivit v kanálu
- Zobrazení vstupní a výstupní datové sady
- apod. 

Tato část obsahuje taky jak řez přechody z jednoho stavu do jiného státu.   

### <a name="navigate-to-your-data-factory"></a>Přejděte na výrobce dat
1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
2.  Klikněte na **zdroje dat** v nabídce na levé straně. Pokud ho nevidíte, klikněte na **Další služby >** a klikněte na **zdroje dat** v rámci **MĚŘÍTKA + technologie pro analýzu** kategorie. 

    ![Procházení všech -> zdroje dat](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    Měli byste vidět všechny továrny dat v zásuvné **továrny Data** . 
4. V zásuvné továrny dat vyberte factory dat, které vás zajímají.

    ![Výběr dat factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)  
5.  a byste měli vidět na domovskou stránku (zásuvné**factory dat** ) data factory.

    ![Zásuvné factory dat](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Zobrazení diagramu factory dat
Zobrazení diagramu factory dat obsahuje jedno podokno sklenice ke sledování a správa dat továrny a jeho prostředky.

Zobrazení diagramu factory dat zobrazíte kliknutím na **diagramu** na domovské stránce factory data.

![Zobrazení diagramu](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Můžete přiblížit, oddálit, zvětšit tvar lupy na 100 %, uzamknout rozložení diagramu a automaticky umístit kanály a tabulky. Můžete taky najdete v článku informace o souvisejících zdrojích dat (se zobrazením směrem nahoru a dolů položek z vybraných položek).
 

### <a name="activities-inside-a-pipeline"></a>Aktivity uvnitř potrubí 
1. Klikněte pravým tlačítkem kanál a klikněte na **Otevřít kanálu** zobrazíte všechny aktivity v kanálu spolu s vstupní a výstupní datové sady pro činnosti. Tato funkce je užitečná, když svůj kanál zahrnuje víc aktivity a chcete se dozvědět provozní souvisejících zdrojích jednoho kanálu.

    ![Nabídka Otevřít kanálem k odesílání zpráv](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. V následujícím příkladu se zobrazí dvě aktivity v kanálu s jejich vstupů a výstupů. Aktivity s názvem **JoinData** typu HDInsight podregistru aktivity a **EgressDataAzure** typu kopírovat aktivity jsou v této ukázkové kanálem k odesílání zpráv. 
    
    ![Aktivity uvnitř kanálů](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. Po kliknutí na odkaz factory dat v s popisem cesty v levém horním můžete přejít zpět na domovskou stránku Data Factory.

    ![Přejděte zpět do továrny dat](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-state-of-each-activity-inside-a-pipeline"></a>Zobrazit stav každé z těchto činností uvnitř kanálů
Aktuální stav aktivitu můžete zobrazit tak, že zobrazení stavu všech datových sad vytvořené pomocí aktivity. 

Příklad: v následujícím příkladu **BlobPartitionHiveActivity** proběhla úspěšně a vyrobeno datovou sadu s názvem **PartitionedProductsUsageTable**, což je **připravena** .

![Stav kanálem k odesílání zpráv](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Poklikáním **PartitionedProductsUsageTable** v zobrazení diagramu hodnotí všechny výseče vytvořené pomocí různých aktivity spustí uvnitř kanálů. Uvidíte, že **BlobPartitionHiveActivity** proběhla úspěšně každý měsíc pro poslední osm měsíce a vyrobeno výsečí **připravena** .

Výseče datovou sadu dat factory může mít jednu z následujících stavů:

<table>
<tr>
    <th align="left">Stav</th><th align="left">Dílčím stavem</th><th align="left">Popis</th>
</tr>
<tr>
    <td rowspan="8">Čekání</td><td>ScheduleTime</td><td>Čas nebyla pocházet pro řez spustit.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Odesílání dat závislosti připraveni není.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Pro využití prostředků nejsou k dispozici.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Všechny instance aktivity jsou zaneprázdněn spouštěním ostatních výsečí.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Aktivity se pozastaví a nedá spustit výseče, dokud je obnoven.</td>
</tr>
<tr>
<td>Opakovat</td><td>Opakované spuštění aktivity.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření ona nezačne.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Čekání na ověření k opakování.</td>
</tr>
<tr>
<tr
<td rowspan="2">Probíhající</td><td>Ověřování</td><td>Ověření probíhá.</td>
</tr>
<td></td>
<td>Zpracovávání výseče.</td>
</tr>
<tr>
<td rowspan="4">Se nezdařila.</td><td>TimedOut</td><td>Spuštění trvala delší než, která je povolené na základě aktivity.</td>
</tr>
<tr>
<td>Zrušení</td><td>Zrušeno akci uživatele.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se nezdařilo.</td>
</tr>
<tr>
<td></td><td>Nepovedlo se generovat a/nebo ověřit výseč.</td>
</tr>
<td>Jste připravení</td><td></td><td>Řez je připraven k spotřebu.</td>
</tr>
<tr>
<td>Přeskočilo</td><td></td><td>Řez není zpracovat.</td>
</tr>
<tr>
<td>Žádná</td><td></td><td>Řez, který používá existují jiný stav, ale resetování.</td>
</tr>
</table>



Můžete zobrazit údaje o řez po kliknutí na položku výsečí v zásuvné **Naposledy aktualizováno výsečí** .

![Podrobnosti o výseč](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
Jestliže řez proběhlo několikrát, se zobrazí víc řádků v seznamu **spustí aktivity** . Můžete zobrazit údaje o aktivitu spustíte kliknutím na spouštěcím položky v seznamu **že spustí aktivity** . V seznamu vidíte všechny soubory protokolu spolu s chybovou zprávu případné. Tato funkce je užitečný pro zobrazení a ladění protokoly aniž byste museli opustit data factory.

![Spustit podrobnosti o aktivitách](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Jestliže řez nejsou **připravena** , uvidíte nadřazeného řezy, které nejsou připravení a blokují aktuální řez spuštění v seznamu **, které nejste připravení nadřazeného výsečí** . Tato funkce je užitečná, když svůj řez je v **čekání** stavu a chcete se dozvědět nadřazeného závislostí na kterých se čeká řez.

![Není připraveno nadřazeného výsečí](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Diagram stavu datové sady
Když nasadíte factory dat a kanály máte platné aktivní období, datové řezy přechodu z jednoho stavu do jiného. Stav výseč aktuálně spočívá následující diagram stavu:

![Diagram stavu](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Tok datovou sadu stavu přechodu v factory dat: čekání -> průběh/v-probíhající (Validating) -> připravených/neúspěšné

Výseče spustit ve stavu **čekání** před podmínky splnění před spuštěním. Potom provádění zahájení aktivity a výseč jsou uvedeny v **Probíhající** stavu. Spuštění aktivity může úspěšné nebo selže. Řez je označený jako **připravený**"nebo **se nezdařilo** na základě výsledku spuštění. 

Můžete obnovit řez se pořád vracet **připravení** nebo **selhání** stavu **čekání** stavu. Můžete taky označit stavu řez na **Přeskočit**, které zabrání spuštění aktivity a zpracování výseče.


## <a name="manage-pipelines"></a>Správa potrubí
Můžete spravovat svůj potrubí pomocí prostředí PowerShell Azure. Můžete třeba pozastavit a obnovit potrubí spuštěním rutiny prostředí PowerShell Azure. 

### <a name="pause-and-resume-pipelines"></a>Pozastavit potrubí
Je možné pozastavit nebo pozastavit potrubí pomocí rutiny prostředí Powershell **Pozastavení AzureRmDataFactoryPipeline** . Tato rutina je užitečná, když nechcete spouštět vaší kanály, dokud podařilo odstranit problém.

Příklad: v následujícím snímku obrazovky byl zjištěn problém s **PartitionProductsUsagePipeline** v **productrecgamalbox1dev** dat factory a chceme za účelem pozastavení kanálu.

![Pozastaví kanálem k odesílání zpráv](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Za účelem pozastavení kanálů, spusťte tento příkaz Powershellu:

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Příklad:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Jakmile se problém má pevné s **PartitionProductsUsagePipeline**, pozastavené kanálem k odesílání zpráv lze obnovit spuštěním následujícího příkazu Powershellu:

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Příklad:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## <a name="debug-pipelines"></a>Ladění potrubí
Azure Data Factory nabízí spoustu možností přes Azure portálem a Azure PowerShell ladění a odstraňování problémů s kanály.

### <a name="find-errors-in-a-pipeline"></a>Nalezení chyb v kanálu
Spustit aktivity nepovede v kanálu, datovou sadu vytvořené pomocí kanálu při v chybovém stavu vzhledem k chybě. Ladění a Poradce při potížích v Azure Data Factory pomocí následující mechanismy.

#### <a name="use-azure-portal-to-debug-an-error"></a>Pomocí Azure portál ladění Chyba:

3.  V **TABULCE** zásuvné klepněte na výseč problém se **Stav** nastaveno **Neúspěšné**.

    ![Tabulka zásuvné s problém výsečí](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.  V zásuvné **VÝSEČ** klikněte na činnosti, které se nepodařilo spustit.
    
    ![Dataslice zobrazí se chybová zpráva](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.  Z zásuvné **Podrobnosti o spuštění AKTIVITĚ** můžete stahovat soubory přidružené zpracování HDInsight. Klikněte na stáhnout pro stav/stderr stáhnout soubor protokolu chyb, který obsahuje podrobné informace o chybě.

    ![Aktivity spustit podrobnosti zásuvné s chybou](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### <a name="use-the-powershell-to-debug-an-error"></a>Použití Powershellu ladění chybu
1.  Spusťte **Azure Powershellu**.
3.  Příkaz **Get-AzureRmDataFactorySlice** najdete v článku výseče a jejich stavy. Měli byste vidět řez se stavem: **se nezdařila**.       

            Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Příklad:
        
            Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Nahraďte **Počáteční_datum_čas** Počáteční_datum_čas hodnotu, kterou jste definovali pro sadu AzureRmDataFactoryPipelineActivePeriod.
4. Spusťte rutinu **Get-AzureRmDataFactoryRun** zobrazení podrobností o aktivitě spuštění pro výseče.

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Příklad:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    Hodnota Počáteční_datum_čas je čas zahájení výseč chyby/problém, který je uvedeno v předchozím kroku. Datum a čas by měl být uzavřen ve dvojitých uvozovkách.
5.  Měli byste vidět výstupu s podrobnostmi o chybě (podobně jako tento):

            Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
            ResourceGroupName       : ADF
            DataFactoryName         : LogProcessingFactory3
            TableName               : EnrichedGameEventsTable
            ProcessingStartTime     : 10/10/2014 3:04:52 AM
            ProcessingEndTime       : 10/10/2014 3:06:49 AM
            PercentComplete         : 0
            DataSliceStart          : 5/5/2014 12:00:00 AM
            DataSliceEnd            : 5/6/2014 12:00:00 AM
            Status                  : FailedExecution
            Timestamp               : 10/10/2014 3:04:52 AM
            RetryAttempt            : 0
            Properties              : {}
            ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                            Jobs/841b77c9-d56c-48d1-99a3-
                        8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                        more details.
            ActivityName            : PigEnrichLogs
            PipelineName            : EnrichGameLogsPipeline
            Type                    :
    
    
6.  Rutina **Uložit AzureRmDataFactoryLog** fungovat s hodnotou Id najdete v článku z výstupu a stahování souborů pomocí **-DownloadLogsoption** pro rutiny.

            Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## <a name="rerun-failures-in-a-pipeline"></a>Spusťte k chybám v kanálu

### <a name="using-azure-portal"></a>Azure portálu

Jakmile řešeními problémů a ladění chyb v kanálu, je znovu spustit selhání tak, že přejdete na řez chyby klepnutím na tlačítko **Spustit** na panelu s příkazy.

![Spusťte selhalo výseč](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

V případě, že výseč obsahuje ověření se nezdařilo v kvůli selhání zásad (pro ex: data nejsou k dispozici), můžete opravit chybu a ověřte znovu kliknutím na tlačítko **Ověřit** na panelu s příkazy.
![Oprava chyb a ověření](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="using-azure-powershell"></a>Pomocí prostředí PowerShell Azure

Spusťte chyby pomocí rutiny Set-AzureRmDataFactorySliceStatus. Najdete v tématu [Nastavení AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) syntaxi a další podrobnosti o rutině. 

**Příklad:** Následující příklad nastaví stav všech výřezů tabulce "DAWikiAggregatedData" počkat v Azure data factory "WikiADF".

UpdateType je nastavený na UpstreamInPipeline, což znamená, že stavy každý řez k tabulce a závislé (nadřazeného) tabulky jsou nastavené "Čeká se." Další možná hodnota pro tento parametr je "Osoba,."

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## <a name="create-alerts"></a>Vytvořit upozornění
Azure protokoly uživatele události při Azure zdrojů (například data factory) je vytvořili, aktualizovat nebo odstranit. Vytvořit upozornění na tyto události. Data Factory vám umožní zaznamenat různé metriky a vytvořit upozornění metrice. Doporučujeme používat události v reálném čase sledování a metriky historických důvodů. 

### <a name="alerts-on-events"></a>Upozornění na události
Azure události poskytují užitečný podstatu co je nového v Azure zdroje. Azure protokoly uživatele události při Azure zdrojů (například data factory) je vytvořili, aktualizovat nebo odstranit. Pokud chcete použít Azure Data Factory, se vytvářejí události při:

- Azure Data Factory je vytvořili/aktualizovat a odstranit.
- Zpracování dat (označované jako jako spuštění) začít/dokončil.
- Shluk HDInsight na vyžádání je vytvořen a odebrat.

Můžete vytvořit upozornění pro tyto uživatele události a nakonfigurovat e-mailová oznámení odešlete Administrators a dalších správců předplatného. Kromě toho můžete zadat další e-mailové adresy uživatelů, kteří potřebují pro příjem e-mailová oznámení, pokud jsou splněny podmínky. Tato funkce je užitečná, když chcete oznámení o selhání a nechcete, aby nepřetržitě sledování dat factory.

> [AZURE.NOTE] V současné době portálu není zobrazena upozornění na události. Pomocí [sledování a Správa aplikací](data-factory-monitor-manage-app.md) pro zobrazování veškerých upozornění.

#### <a name="specifying-an-alert-definition"></a>Určení definici upozornění:
Chcete-li definici upozornění, vytvoříte soubor JSON popisující operace, které chcete být upozorňováni na. V následujícím příkladu odešle upozornění e-mailové oznámení pro operaci RunFinished. Jako konkrétní, e-mailové oznámení odeslané při spuštění v data factory dokončení a selže spustit (stav = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

Z definici JSON **podřízeného stavu** je možné odebrat Pokud nechcete upozorňovat na konkrétní chyby.

V tomto příkladu nastaví upozornění pro všechny zdroje dat ve vašem předplatném. Pokud budete potřebovat upozornění je nastaveno factory konkrétní data, je možné zadat data factory **resourceUri** ve **zdroji dat**:

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Následující tabulka obsahuje seznam dostupných operací a stavy (a dílčí stavů).

Název operace | Stav | Stav Sub
-------------- | ------ | ----------
RunStarted | Začínáme | Spuštění
RunFinished | Nepodařilo / bylo úspěšné | FailedResourceAllocation<br/><br/>Úspěšně<br/><br/>FailedExecution<br/><br/>TimedOut<br/><br/>< zrušeno<br/><br/>FailedValidation<br/><br/>Opuštěné
OnDemandClusterCreateStarted | Začínáme
OnDemandClusterCreateSuccessful | Úspěšně
OnDemandClusterDeleted | Úspěšně

Další informace o prvcích JSON použitých v tomto příkladu najdete v článku [Vytvoření upozornění pravidlo](https://msdn.microsoft.com/library/azure/dn510366.aspx) . 

#### <a name="deploying-the-alert"></a>Nasazení upozornění 
Abyste mohli nasadit upozornění, získáte pomocí rutiny Powershellu Azure: **AzureRmResourceGroupDeployment nový**, jak je vidět v následujícím příkladu:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Po úspěšném dokončení nasazení skupina zdroje se zobrazí tyto zprávy:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [AZURE.NOTE] Rozhraní REST API [Vytvořit pravidlo upozornění](https://msdn.microsoft.com/library/azure/dn510366.aspx) můžete vytvořit pravidlo výstrahy. Datové JSON probíhá podobně jako v příkladu JSON.  

#### <a name="retrieving-the-list-of-azure-resource-group-deployments"></a>Načtení seznamu Azure zdrojů skupina nasazení
Načtení seznamu nasazeném nasazení Azure pole Skupina zdroje, získáte pomocí rutiny: **Get-AzureRmResourceGroupDeployment**, jak je vidět v následujícím příkladu:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### <a name="troubleshooting-user-events"></a>Poradce při potížích s uživateli události


1. Zobrazí se všechny události generované po kliknutí na dlaždici **metriky a operací** .

    ![Dlaždice metriky a provoz](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)

2. Klikněte na dlaždici **události** zobrazíte události. 

    ![Dlaždice události](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. V zásuvné **události** můžete zobrazit podrobnosti o událostech, filtrování událostí a tak dál. 

    ![Zásuvné události](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Klikněte na **operace** v seznamu operací, který způsobuje chybu.
    
    ![Vyberte operaci](./media/data-factory-monitor-manage-pipelines/select-operation.png) 
5. Klikněte na událost **chyby** zobrazíte podrobnosti o chybě.

    ![Chyba události](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)
  

Článku [Azure přehled rutin](https://msdn.microsoft.com/library/mt282452.aspx) najdete v tématu rutiny prostředí PowerShell, který slouží k přidání nebo načtení/odebrat upozornění. Tady je několik příkladů použití rutinu **Get-AlertRule** : 


    PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
        
            Properties :
            Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
            Condition   :
            DataSource :
            EventName             :
            Category              :
            Level                 :
            OperationName         : RunFinished
            ResourceGroupName     :
            ResourceProviderName  :
            ResourceId            :
            Status                : Failed
            SubStatus             : FailedExecution
            Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
            Condition   :
            Description : One or more of the data slices for the Azure Data Factory has failed processing.
            Status      : Enabled
            Name:       : ADFAlertsSlice
            Tags       :
            $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
            Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
            Location   : West US
            Name       : ADFAlertsSlice
    
    PS C:\> Get-AlertRule -res $resourceGroup

            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
            Location   : West US
            Name       : FailedExecutionRunsWest3

    PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0

Spusťte následující příkazy získat nápovědu zobrazíte podrobnosti a příklady pro rutinu Get-AlertRule. 

    get-help Get-AlertRule -detailed 
    get-help Get-AlertRule -examples


- Pokud se zobrazí upozornění generování události na portálu zásuvné, ale nechcete dostávat e-mailová oznámení, zkontrolujte, jestli budete dostávat e-maily od externích odesílatelů je nastavený e-mailovou adresu. Výstrahy e-mailů mohou zablokoval nastavení e-mailu.

### <a name="alerts-on-metrics"></a>Upozornění na metriky
Data Factory vám umožní zaznamenat různé metriky a vytvořit upozornění metrice. Můžete sledovat a vytvořit upozornění na následující metriky výseče u výrobce data.
 
- Spustí nezdařeném uložení
- Úspěšně spuštěna

Tyto metrik jsou užitečné a umožní vám přehled o celkové nezdařené a úspěšně spuštěna ve svých dat factory. Metriky jsou emitovány pokaždé, když je řez spustit. Horní části hodiny jsou tyto metriky agregované a posune ke svému účtu úložiště. Proto aby metriky nastavení účtu úložiště.


#### <a name="enabling-metrics"></a>Povolení metriky:
Chcete-li povolit metriky, klikněte na od Data Factory zásuvné:

**Sledování** -> **míru** -> **diagnostiky nastavení** -> **diagnostiky**

![Diagnostika odkaz](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

V **diagnostiky** zásuvné klepněte **na** a vyberte účet, úložiště a uložte.

![Diagnostika zásuvné](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Po uložení, může to trvat až jednu hodinu metriky být viditelné na sledování zásuvné, protože se metriky agregace stane každou hodinu.


### <a name="setting-up-alert-on-metrics"></a>Nastavení upozornění u metriky:

Klikněte na **Data Factory metriky** zásuvné: 

![Dlaždice metriky factory dat](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Na zásuvné **míru** klikněte na tlačítko **+ Přidat upozornění** na panelu nástrojů. 
![Data factory metrických zásuvné – přidání oznámení](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Na stránce **Přidat pravidlo výstrahy** proveďte následující kroky a klikněte na **OK**.
 
- Zadejte název pro upozornění (Příklad: failed upozornění).
- Zadejte popis upozornění (Příklad: odeslání e-mailu, když dojde k selhání).
- Vyberte metriky (neúspěšné spustí porovnání úspěšně spuštěna).
- Určete podmínku a mezní hodnota.   
- Toto období zadejte. 
- Určete, zda mají být vlastníky přispěvatelé a čteček odesílány e-mailu.
- apod. 

![Data factory metrických zásuvné – přidání oznámení](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Po úspěšném přidání pravidlo výstrahy zásuvné zavře a uvidíte nové upozornění na stránce **míru** . 

![Data factory metrických zásuvné – přidání oznámení](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Počet upozornění byste měli vidět i na dlaždici **upozornění** . Klepněte na **upozornění** .

![Data factory metrických zásuvné - upozornění pravidel](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

V **upozornění na** zásuvné vidíte všechny existující upozornění. Přidat upozornění, klikněte na **Přidat upozornění** na panelu nástrojů.

![Pravidla výstrah zásuvné](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Oznámení:
Jakmile upozornění pravidlo podmínce, by měla získat oznámení aktivované e-mail. Jakmile problém vyřeší a upozornění podmínka neodpovídá žádné další, dostanete oznámení přeložena e-mail.

Toto chování se liší od události kde prezentujícímu se odešle upozornění při každé chybě pravidlo, které upozornění nesplňuje.

### <a name="deploying-alerts-using-powershell"></a>Nasazení upozornění pomocí prostředí PowerShell
Můžete nasadit upozornění pro metriky stejným způsobem jako u události. 

**Upozornění definice:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
Nahraďte příslušnými hodnotami subscriptionId resourceGroupName a dataFactoryName ve výběru.

*metricName* k nyní podporuje dvě hodnoty:
- FailedRuns
- SuccessfulRuns

**Nasazení upozornění:**

Abyste mohli nasadit upozornění, získáte pomocí rutiny prostředí PowerShell Azure: **AzureRmResourceGroupDeployment nový**, jak je vidět v následujícím příkladu:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Měli byste vidět následující zprávou po úspěšném nasazení:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


Taky můžete použít rutinu **Přidat AlertRule** nasazení pravidlo výstrahy. Další informace v tématu [AlertRule přidat](https://msdn.microsoft.com/library/mt282468.aspx) .  

## <a name="move-data-factory-to-a-different-resource-group-or-subscription"></a>Přesunutí dat factory jiné skupině zdrojů nebo předplatného
Factory dat můžete přesunout do skupiny různých zdrojů nebo jiné předplatné pomocí tlačítka panelu příkaz **Přesunout** na domovské stránce výrobce data. 

![Přesunutí dat factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Můžete také posouváním související materiály (například upozornění přidružené factory dat) spolu s data factory.

![Dialogové okno přesunout zdroje](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
