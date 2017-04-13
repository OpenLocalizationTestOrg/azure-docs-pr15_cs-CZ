<properties
   pageTitle="Zobrazení v řešení pro správu sady správy operace (OMS) | Microsoft Azure"
   description="Řešení pro správu v sadě správy operace (OMS) se obvykle obsahují jedno nebo více zobrazení vizualizovat data.  Tento článek popisuje, jak exportovat zobrazení vytvořený Návrhářem zobrazení a zahrnout do řešení pro správu. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Zobrazení v řešení pro správu sady správy operace (OMS) (verze Preview)

>[AZURE.NOTE]Toto je předběžná verze dokumentace pro vytváření řešení pro správu v OMS, které jsou v současnosti ve verzi preview. Libovolné schéma píše níže se mohou změnit.    

[Řešení pro správu v sadě operace Management (OMS)](operations-management-suite-solutions.md) se obvykle obsahují jedno nebo více zobrazení vizualizovat data.  Tento článek popisuje, jak exportovat zobrazení vytvořené pomocí [Zobrazení návrhu](../log-analytics/log-analytics-view-designer.md) a zahrnout do řešení pro správu.  

>[AZURE.NOTE]Příklady v tomto článku použít parametry a proměnných, které jsou potřeba nebo společné řešení pro správu a popsány v [řešení pro správu vytvoření v sadě operace Management (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že jste již vědět, jak [vytvářet řešení pro správu](operations-management-suite-solutions-creating.md) a struktury soubor řešení.


## <a name="overview"></a>Základní informace

Řešení pro správu zahrnout zobrazení, vytvoření **zdroje** pro ni v [souboru řešení](operations-management-suite-solutions-creating.md).  JSON popisující podrobné konfigurace zobrazení je obvykle složité přes a něco ne, že se zobrazí autorovi typické řešení by mohli vytvořit ručně.  Nejběžnější způsob je vytvoření zobrazení pomocí [Zobrazení návrhu](../log-analytics/log-analytics-view-designer.md), exportovat a potom přidáte podrobné konfigurace řešení. 

Toto jsou základní kroky pro přidání zobrazení řešení následujícím způsobem.  Každý krok se píše dále v následujících částech.

1. Zobrazení exportujte do souboru.
2. Vytvoření zobrazení zdroje v řešení.
3. Přidání části zobrazení podrobností.

## <a name="export-the-view-to-a-file"></a>Zobrazení exportovat do souboru
Postupujte podle pokynů v [Protokolu analýzy zobrazení návrhu](../log-analytics/log-analytics-view-designer.md) zobrazení exportovat do souboru.  Exportovaný soubor bude ve formátu JSON s stejné [prvky jako soubor řešení](operations-management-suite-solutions-creating.md#management-solution-files).  

Element **zdroje** zobrazit soubor bude mít zdroje s typem **Microsoft.OperationalInsights/workspaces** představující OMS pracovního prostoru.  : Tento element bude mít podřízený s typem **zobrazení** , které představuje zobrazení a obsahuje podrobné konfiguraci.  Bude zkopírovat podrobnosti o: Tento element a zkopírujte ho do vašeho řešení.


## <a name="create-the-view-resource-in-the-solution"></a>Vytvoření zobrazení zdroje v řešení
Následující zdroj zobrazení přidáte do prvku **zdroje** souboru řešení.  To používá proměnné popsaných níže, musíte taky přidat.  Všimněte si, že vlastnosti **řídicího panelu** a **OverviewTile** zástupné symboly, které bude přepsat ze souboru exportovaného zobrazení odpovídající vlastnosti.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Přidejte následující proměnné do prvku [proměnné](operations-management-suite-solutions-creating.md#variables) souboru řešení a nahradit hodnoty, které jsou pro vaše řešení.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Všimněte si, může kopírovat celé zobrazení zdroj z vaší exportovaný soubor zobrazení, že je potřeba provést následující změny pro bude platit řešení.  

- Potřebujete-li změnit ze **zobrazení** pro **Microsoft.OperationalInsights/workspaces** **Typ** pro zobrazení zdroje.
- Vlastnost **název** pro zobrazení zdroje, musí změnit tak, aby obsahovat název pracovního prostoru.
- Závislost na pracovní prostor, musí být odebrány od zdrojů workspace není definované v řešení.
- Vlastnost **DisplayName** musí být přidán do zobrazení.  **Id**, **název**a **DisplayName** musí všechny odpovídat.
- Názvy parametrů musí změnit podle požadovanou sadu parametry.
- Proměnné třeba podle řešení a použít v příslušné vlastnosti.

## <a name="add-the-view-details"></a>Přidání části zobrazení podrobností
Zobrazení zdroje v exportovaný soubor zobrazení bude obsahovat dva prvky v prvku **Vlastnosti** s názvem **řídicích panelů** a **OverviewTile** , které obsahují podrobné konfigurace zobrazení.  Zkopírujte do prvku **Vlastnosti** zobrazení zdroje v souboru řešení těchto dvou elementů a jejich obsah. 

## <a name="example"></a>Příklad
V následujícím příkladu zobrazuje soubor jednoduché řešení s zobrazení.  Tři tečky (...) se zobrazí pro obsah **řídicího panelu** a **OverviewTile** důvodů místa.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Další kroky

- Další podrobné informace o vytváření [řešení pro správu](operations-management-suite-solutions-creating.md).
- Zahrnout [automatizaci runbooks v řešení pro správu](operations-management-suite-solutions-resources-automation.md).