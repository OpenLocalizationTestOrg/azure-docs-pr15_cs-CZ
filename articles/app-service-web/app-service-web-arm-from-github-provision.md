<properties 
    pageTitle="Nasazení webové aplikace, které otevře GitHub úložiště" 
    description="Použití šablony správce prostředků Azure k nasazení webové aplikace obsahující projektu v úložišti GitHub." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Nasazení aplikace web propojen GitHub úložiště

V tomto tématu se dozvíte, jak vytvořit šablonu aplikace Správce prostředků Azure, které do webových aplikací, který je propojená s projektem v úložišti GitHub nasadí. Naučíte se, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

Další informace o vytváření šablon najdete v tématu [Vytváření šablony Azure správce prostředků](../resource-group-authoring-templates.md).

Dokončení šablonu vyberete najdete v článku [Web App propojené GitHub šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Co budete nasazovat

Pomocí této šablony můžete nasadit do webových aplikací, které obsahuje tento kód z projektu v GitHub.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

Adresa URL pro úložiště GitHub, která obsahuje projektu pro nasazení. Tento parametr obsahuje výchozí hodnotu, ale tato hodnota je určená jenom k předvedení zadejte adresu URL pro úložiště. Při testování šablony můžete použít tuto hodnotu, ale budete chtít zadejte adresu URL vlastní úložiště při práci s šablonou.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>větev

Větvi úložiště při nasazení aplikace. Výchozí hodnota je předlohy, ale můžete zadat název žádnou větev v úložišti, kterou chcete nasadit.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Materiály pro nasazení

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>V prohlížeči

Vytvoří v prohlížeči, který je propojený do GitHub projektu. 

Zadáte název webové aplikace pomocí parametru **název webu** a umístění webové aplikace pomocí parametru **siteLocation** . V elementu **dependsOn** definuje šablona web appu jako závislé na službě hostingu plán. Protože jsou závislé na hostingu plánu, web appu nevytvoří nedokončili hostingu plán obsahuje vzniku. **DependsOn** element pouze slouží k určení pořadí nasazení. Pokud nezaškrtnete web appu jako závisí na hostitelském plánu, Azure zdroje Mananger se pokusí vytvořit oba zdroje ve stejnou dobu a může dojít k chybě, pokud web appu se vytvoří před plánem hostingu.

Web appu má i podřízené zdroje, která je definovaná v následující části **zdroje** . Tento zdroj podřízené definuje ovládacího prvku zdroje pro projekt nasazených pomocí web appu. V této šabloně ovládacího prvku zdroje je propojená s konkrétní GitHub úložiště. Úložiště GitHub je definován s kódem **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** můžete pevného kódu adresu URL úložiště když budete chtít vytvořit šablonu, která opakovaně nasadí jeden projekt současně vyžaduje minimální počet parametrů.
Místo pevné kódování URL úložiště, můžete přidat parametru pro adresu URL úložiště a použijte tuto hodnotu pro vlastnost **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Prostředí PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
