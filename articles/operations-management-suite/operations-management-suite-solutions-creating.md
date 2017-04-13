<properties
   pageTitle="Vytváření řešení pro správu v sadě správy operace (OMS) | Microsoft Azure"
   description="Řešení pro správu rozšířit použitelnost funkce z operace správy sady (OMS) zadáním sbalené správy scénáře, které zákazníci můžete přidat do jejich OMS pracovního prostoru.  Tento článek obsahuje podrobné informace o tom, jak můžete vytvořit řešení pro správu se nemusí používat v vlastní prostředí nebo k dispozici pro zákazníky."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Vytváření řešení pro správu v operace správy sady (OMS) (verze Preview)

>[AZURE.NOTE]Toto je předběžná verze dokumentace pro vytváření řešení pro správu v OMS, které jsou v současnosti ve verzi preview. Libovolné schéma píše níže se mohou změnit.  

Řešení pro správu rozšířit použitelnost funkce z operace správy sady (OMS) zadáním sbalený správy scénáře, které zákazníci můžete přidat do jejich OMS pracovního prostoru.  Tento článek obsahuje informace týkající se vytváření vlastních řešení pro správu, můžete použít vlastní prostředí nebo být k dispozici pro zákazníky pomocí komunity.

## <a name="planning-your-management-solution"></a>Plánování řešení pro správu
Řešení pro správu v OMS obsahovat více zdrojů podpůrné scénáři konkrétní správy.  Při plánování řešení, by měly zaměřit na scénáře, který se pokoušíte dosáhnout a všechny požadované prostředky pro podporu.  Každý řešení by měl být obsaženy a definovat jednotlivé zdroje, které si žádá, i když jeden nebo více zdrojů jsou definovány taky další řešení.  Při instalaci řešení pro správu jednotlivé zdroje se vytvoří, pokud již existuje a můžete určit, co se stane s zdroje při odebrání řešení.  

Například řešení pro správu můžou obsahovat [postupu runbook Azure automatizaci](../automation/automation-intro.md) , který shromáždí data do úložiště protokolu analýzy pomocí [plán](../automation/automation-schedules.md) a [zobrazení](../log-analytics/log-analytics-view-designer.md) , které obsahuje jednotlivé vizualizace shromážděná data.  Stejný plán může používat jiné řešení.  Jako autor řešení správy by definovat všechny tři zdroje, ale určit, že postupu runbook a zobrazení by měl být automaticky odebrány při odebrání řešení.    By také definovat plánu, ale určit, že pokud řešení byly odebrány v případě, že byl stále použít jiné řešení, ho měli zůstaly zachované.

## <a name="management-solution-files"></a>Správa řešení souborů
Řešení pro správu jsou implementovaná jako [šablony řízení zdrojů](../resource-manager-template-walkthrough.md).  Výukové hlavní úkolu v naučit, jak vytvářet řešení pro správu jak [Autor šablony](../resource-group-authoring-templates.md).  Tento článek obsahuje jedinečné podrobnosti šablon pro řešení a jak definovat typické řešení zdroje.

Základní struktura souboru řešení správy je stejná jako [Správce prostředků šablonu](resource-group-authoring-templates.md#template-format) , která vypadá takto.  Každá z těchto částí popisuje prvky nejvyšší úrovně a a jejich obsah v řešení.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametry

[Parametry](../resource-group-authoring-templates.md#parameters) jsou zobrazené hodnoty, které vyžadují uživateli při instalaci řešení pro správu.  Existuje standardní parametry, jejichž všechna řešení a podle potřeby můžete přidat další parametry pro konkrétní řešení.  Jak bude zadání hodnoty parametru při instalaci řešení závisí na konkrétní parametr a jak se instaluje řešení.


Pokud uživatel nainstaluje váš řešení pro správu [Webu Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) a [Rychlý úvod Azure šablony](operations-management-suite-solutions.md#finding-and-installing-solutions) se zobrazí výzva k vyberte [OMS workspace a automatizaci účtu](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Jsou použity k naplnění hodnot všech standardní parametry.  Uživatel se výzva k přímým zdrojem hodnot pro standardní parametry, ale se zobrazí výzva k zadání hodnoty pro všechny další parametry.

Pokud uživatel nainstaluje váš řešení [ještě jeden způsob](operations-management-suite-solutions.md#finding-and-installing-solutions), je třeba zadat hodnotu pro všechny parametry standardní a všechny další parametry.

Ukázka parametru jsou uvedeny níže.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Následující tabulka popisuje atributy parametru.

| Atribut | Popis |
|:--|:--|
| Typ        | Datový typ pro parametr. Vstupní ovládací prvek zobrazený u uživatele, závisí na datovém typu.<br><br>Logická hodnota - rozevírací nabídku<br>řetězec - textového pole<br>Funkce INT - textového pole<br>SecureString - pole pro heslo<br> |
| kategorie    | Volitelné kategorie pro parametr.  Parametry ve stejné kategorie jsou seskupené. |
| ovládací prvek     | Další funkce pro parametry řetězce.<br><br>Zobrazí se data a času - ovládací prvek data a času.<br>automaticky generované GUID - hodnota Guid a parametr nezobrazí. |
| Popis | Volitelný popis parametru.  Zobrazit informace bubliny vedle parametr. |


### <a name="standard-parameters"></a>Standardní parametry
V následující tabulce jsou uvedeny standardní parametry pro všechny řešení pro správu.  Tyto hodnoty jsou vyplněné pro uživatele místo dotazování na jejich řešení je nainstalovaná z Azure Marketplace nebo rychlý úvod šablony.  Uživatel musí zadat hodnoty je nainstalovaného řešení s jiným způsobem.

>[AZURE.NOTE]Uživatelské rozhraní webu Azure Marketplace a rychlý úvod šablony očekává názvy parametrů v tabulce.  Pokud používáte jiný parametr názvy klikněte uživateli se zobrazí výzva, je a nesmí být vyplněno automaticky.


| Parametr | Typ | Popis |
|:--|:--|:--|
| název účtu       | řetězec |  Název účtu Azure automatizaci. |
| pricingTier       | řetězec | Ceny vrstva pracovního prostoru protokolu technologie pro analýzu a automatické Azure účtu. |
| regionId          | řetězec | Oblast účet Azure automatizaci. |
| Název řešení      | řetězec | Název řešení. |
| workspaceName     | řetězec | Přihlaste se název pracovního prostoru analýzy. |
| workspaceRegionId | řetězec | Oblast pracovního prostoru protokolu analýzy. |





### <a name="sample"></a>Ukázka
Následuje entitu parametru ukázkové řešení.  Platí to i všechny standardní parametry a dva další parametry do stejné kategorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Odkazujete hodnoty parametrů v dalších prvků řešení se syntaxe **parametrů (název parametru)**.  Například název pracovního prostoru zobrazíte použijete **parameters('workspaceName')** 

## <a name="variables"></a>Proměnné

Element **proměnné** obsahuje hodnoty, které budete používat ve zbývající části řešení pro správu.  Tyto hodnoty nejsou přístupné pro uživatele instalace řešení.  Jsou určeny chcete, aby Autor na jednom místě, kde můžete spravovat hodnoty, které mohou být použity tisknutím v celé řešení. Měli byste umístit všechny hodnoty konkrétní do vašeho řešení proměnných nikoli hardcoding je v prvku **zdroje** .  Díky kód zpřehlednit a umožňuje snadno změnit tyto hodnoty v novějších verzích.

Následuje příklad prvku **proměnné** s typické parametry použité v řešení.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Odkazujete proměnnými hodnotami prostřednictvím řešení s syntaxe **proměnné ("proměnná název")**.  Například pro přístup k je proměnná Název řešení, použijete **variables('solutionName')** 


## <a name="resources"></a>Zdroje informací

Element **zdroje** definuje různých zdrojů součástí vašeho řešení pro správu.  To bude největší a nejčastěji složité část šablony.  Zdroje jsou tato pole definovány následující strukturu.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Závislosti
Prvky **dependsOn** Určuje [závislost](../resource-group-define-dependencies.md) na jiné zdroje.  Po instalaci řešení zdroj vytvořen až závislými vytvořili.  Například řešení může [začít postupu runbook](operations-management-suite-solutions-resources-automation.md#runbooks) při instalaci pomocí [úloh zdroje](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Zdroje úlohy bude závislá na postupu runbook prostředku, abyste měli jistotu, že postupu runbook vznikne před vytvořením projektu.

### <a name="oms-workspace-and-automation-account"></a>Pracovní prostor OMS a automatizaci účtu
Řešení pro správu vyžadují [pracovního prostoru OMS](../log-analytics/log-analytics-manage-access.md) obsahovat zobrazení a [automatizaci účet](../automation/automation-security-overview.md#automation-account-overview) má být runbooks a související materiály.  Tyto musí být k dispozici zdroje v řešení se vytvářejí a neměli je to možné definovat ve vlastní řešení.  Uživatel bude [Zadejte pracovní prostor a účtu](operations-management-suite-solutions.md#oms-workspace-and-automation-account) při nasazení řešení, ale jako autor byste měli zvážit následující body.


## <a name="solution-resource"></a>Řešení zdroje
Každý řešení vyžaduje zadání zdroje v prvku **zdroje** , která definuje řešení samotné.  To bude mít typu **Microsoft.OperationsManagement/solutions**.  Následuje příklad řešení zdroje.  V následujících částech jsou popsány různé prvky.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Název řešení
Název řešení musí být jedinečný v Azure předplatné. Doporučené použití hodnotu takto.  To proměnnou s názvem **název řešení** pro základní název a parametr **workspaceName** používá k zajištění jedinečný název.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

To by překládal název například následující.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Závislosti
Řešení musí mít tento zdroj [závislost](../resource-group-define-dependencies.md) na každý zdroj v řešení vzhledem k tomu, musí existovat před vytvořením řešení.  To uděláte přidáním položky pro jednotlivé zdroje v elementu **dependsOn** .


### <a name="properties"></a>Vlastnosti
Vlastnosti má zdroj řešení v následující tabulce.  Platí to i pro zdroje odkaz a obsažené řešení, které definují správě zdroje po instalaci používat řešení.  Jednotlivé zdroje v řešení měl seznam obsahovat **referencedResources** nebo **containedResources** vlastností.

| Vlastnost | Popis |
|:--|:--|
| workspaceResourceId | ID pracovního prostoru OMS ve formuláři * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<název pracovního prostoru\>*. |
| referencedResources   | Seznam zdrojů v řešení, které byste neměli při řešení se odebere odebrány. |
| containedResources   | Seznam zdrojů v řešení, které by měl být při řešení se odebere odebrány. |

Výše uvedeném příkladu je řešení s postupu runbook, plánu a zobrazení.  Plán a postupu runbook jsou *odkazované* v prvku **Vlastnosti** , nebudou odebrány při odebrání řešení.  Zobrazení je *obsažena* , odstraní se při odebrání řešení.


### <a name="plan"></a>Plánování
Entita **plán** řešení zdroje obsahuje vlastností v následující tabulce. 

| Vlastnost | Popis |
|:--|:--|
| Jméno          | Název řešení. |
| verze       | Verzi řešení podle autora. |
| produktu       | Jedinečný řetězec k identifikaci řešení. |
| aplikace Publisher     | Vydavatel řešení. |


## <a name="other-resources"></a>Další zdroje
Můžete získat informace a příklady prostředky, které jsou společná pro řešení pro správu v následujících článcích.

- [Zobrazení a řídicí panely](operations-management-suite-solutions-resources-views.md)
- [Automatizace zdroje](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testování řešení pro správu
Před nasazením řešení pro správu, je vhodné otestovat pomocí [Test AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell).  Ověří tento soubor řešení a Nápověda k identifikaci problémy před pokusem o nasazení.



## <a name="next-steps"></a>Další kroky

- Další podrobnosti o [vytváření správce prostředků Azure šablony](../resource-group-authoring-templates.md).
- Hledání [Šablon rychlý úvod Azure](https://azure.microsoft.com/documentation/templates) příklady různých šablon správce prostředků.
- Zobrazte podrobnosti týkající se [přidávání zobrazení řešení pro správu](operations-management-suite-solutions-resources-views.md).
- Zobrazte podrobnosti týkající se [přidávání automatizaci prostředky k řešení pro správu](operations-management-suite-solutions-resources-automation.md).
 