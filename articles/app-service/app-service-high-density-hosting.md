<properties
    pageTitle="Vysoký hustoty hostující na aplikaci služby Azure | Microsoft Azure"
    description="Vysoký hustoty hostující na aplikaci služby Azure"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Vysoký hustoty hostující na aplikaci služby Azure

Pokud chcete použít aplikaci služby, aplikace je oddělené z kapacity přidělené dvou koncepty:

- **Aplikace:** Představuje aplikace a její konfiguraci runtime. Například obsahuje verzi .NET modul runtime mají načíst, nastavení aplikace atd.

- **Plán služeb aplikací:** Definuje vlastnosti kapacitu, sadu dostupných funkcí a místo aplikace. Vlastnosti může být například velké počítače (čtyři jádra), čtyři instance, prémiových funkcí východoasijských USA.

Aplikace je vždy propojená s plánem aplikaci služby, ale plán služeb aplikací může poskytovat schopnost jeden nebo více aplikací.

V důsledku toho platformu poskytuje možnost izolace jednoho aplikace nebo více aplikace sdílení zdrojů sdílením plán služeb aplikací.

Však při více aplikací sdílet plán služeb aplikací, instance tohoto aplikace běží na všechny výskyty tohoto plán služeb aplikací.

## <a name="per-app-scaling"></a>Za měřítka aplikace
*Za měřítka aplikace* je funkce, která lze povolit na úrovni plán služeb aplikací a pak použít na aplikace.

Za aplikace měřítka měřítko aplikace nezávisle plán služeb aplikací, který je hostitelem ho. Tímto způsobem, plán služeb aplikací můžete nakonfigurovat tak, aby poskytují 10 instance, ale aplikace se dá nastavit zobrazit pouze 5.

Následující šablonu Azure správce vytvoří aplikaci služby plán, který je měřítko 10 instance a aplikace, který je nakonfigurovaný na použití za škálování aplikace a měřítko na pouze 5 instance.

Plán služeb aplikace je ve vlastnosti **měřítko na webu** true ( `"perSiteScaling": true`). Aplikace je nastavení **počet zaměstnanců** pomocí 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Doporučená konfigurace pro hostování vysoké hustoty

Za měřítka aplikace je funkce, která je povolen v veřejné Azure regionů a prostředím aplikace služby. Doporučenými postupy je však umožňuje využívat pokročilých funkcí a větší fondů kapacita aplikace služby prostředí.  

Tyto kroky pro nastavení vysoké hustoty hostingu pro aplikace:

1. Konfigurace prostředí služeb aplikací a zvolte pracovní skupinu, která se snaží o hostingu scénář vysoké hustoty.

1. Vytvoření plánu jediné aplikaci služby a měřítko ho používat všechny dostupné kapacity ve fondu pracovní.

1. Nastavte změny měřítka příznak na webu true (pravda) na plán služeb aplikací.

1. Nové weby jsou vytvořené a přiřazené k této aplikaci služby plán s vlastností **numberOfWorkers** nastavenou na **1**. Pomocí této konfiguraci dává nejvyšší hustotu možné na tento pracovní fond.

1. Počet zaměstnanců je možné konfigurovat nezávisle na jeden web s udělit další zdroje informací podle potřeby. Například maximum použít web může **numberOfWorkers** na hodnotu **3** mít další zpracování kapacitu pro tuto aplikaci během nízká použijte webů by **numberOfWorkers** nastavena na hodnotu **1**.
