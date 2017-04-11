<properties 
    pageTitle="Skript Powershellu pro vytvoření aplikace přehledy zdroje" 
    description="Automatizovat vytváření poštovních aplikace přehledy zdroje." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Skript Powershellu pro vytvoření aplikace přehledy zdroje

*Přehledy aplikace je v náhledu.*

Když chcete sledovat nové aplikace - nebo nové verze aplikace - při [Interpretaci aplikace Visual Studio](https://azure.microsoft.com/services/application-insights/), nastavit novou zdroje v Microsoft Azure. Kde je telemetrickými daty z aplikace analyzovat a zobrazí je zdroj. 

Vytvoření nového zdroje automatizovat pomocí prostředí PowerShell.

Například pokud vyvíjíte aplikace mobilním zařízení, je pravděpodobné, že kdykoli, nebude několik publikované verze aplikace nepoužívá zákazníkům. Nechcete, aby k získání výsledků telemetrie z různých verzí smíšený. Tak můžete získat proces vytváření k vytvoření nového zdroje pro každý Tvůrce dotazů.

## <a name="script-to-create-an-application-insights-resource"></a>Skript k vytvoření aplikace přehledy zdroje

Zobrazit specifikace relevantní rutinu:

* [Nové AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Nové AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*Skript Powershellu*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Co dělat s iKey

Jednotlivé zdroje je určen jeho přístrojového vybavení klíč (iKey). IKey je výstup skriptu vytváření zdroje. Vytvořit skript by měl obsahovat, že iKey SDK přehledy aplikace vložený do aplikace.

Existují dva způsoby, jak zpřístupnit iKey SDK:
  
* V [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Nebo v [kódu inicializace](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Viz taky

* [Vytvoření aplikace přehledy a testovací prostředky webu ze šablony](app-insights-powershell.md)
* [Nastavení sledování Azure diagnostiky pomocí prostředí PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Nastavit upozornění pomocí prostředí PowerShell](app-insights-powershell-alerts.md)

 