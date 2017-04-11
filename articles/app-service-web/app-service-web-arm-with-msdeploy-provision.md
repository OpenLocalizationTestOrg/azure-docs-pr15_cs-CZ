<properties
    pageTitle="Nasazení webové aplikace pomocí MSDeploy certifikátem hostname a ssl"
    description="Použití šablony pro správce prostředků Azure nasadit do webových aplikací pomocí MSDeploy a nastavte si vlastní hostname a certifikát SSL"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Nasazení aplikace web s MSDeploy, vlastní hostname a certifikát SSL

Tato příručka, provede vytváření nasazení začátku do konce pro webovou aplikaci Azure, využívání MSDeploy i na šabloně ARM přidat vlastní hostname a certifikát SSL.

Další informace o vytváření šablon najdete v tématu [Vytváření šablony Azure správce prostředků](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Vytvoření ukázkové aplikace

Budete nasazovat webové aplikace technologie ASP.NET. Cílem prvního kroku je vytvořit jednoduchý webové aplikace (nebo můžete použít existující – v takovém případě můžete tento krok přeskočit).

Otevřete aplikaci Visual Studio 2015 a zvolte Soubor > Nový projekt. V zobrazeném dialogovém okně zvolte Web > ASP.NET webové aplikace. V části šablony vyberte Web a vyberete šablonu MVC. Vyberte _typ ověřování změnit_ _Bez_ověřování. To je právě ukázková aplikace co nejjednodušší.

V tomto okamžiku bude mít do základní ASP.Net webových aplikací připravená k použití jako součást procesu nasazení.

###<a name="create-msdeploy-package"></a>Vytvoření balíčku MSDeploy

Dalším krokem je vytvořit balíček pro nasazení web appu Azure. K tomuto účelu uložte projekt a potom spusťte následující z příkazového řádku:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Tím vytvoříte balíčku ZIP ve složce PackageLocation. Aplikace je nyní připravena k nasazení, kterého teď můžete vytvářet, šablony pro správce prostředků Azure to udělat.

###<a name="create-arm-template"></a>Vytvoření šablony ARM
Nejdřív Začněme základní šablonu ARM, který vytvoří webová aplikace a hostingu plán (Všimněte si, že parametry a proměnné nejsou zobrazeny neopakujeme).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Dále budou potřebujete změnit zdroje webové aplikace se vnořené MSDeploy zdroje. To vám umožní odkaz balíček vytvořili a nástroji řekněte správce prostředků Azure pomocí MSDeploy nasazení balíčku Azure web Appu. Na následujícím obrázku je Microsoft.Web/sites zdroje s vnořené MSDeploy zdroje:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Teď zjistíte, že zdroje MSDeploy trvá **packageUri** vlastnost, která je definována následujícím způsobem:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Tento **packageUri** trvá uri úložiště účtu, který odkazuje na účtu úložiště, kde bude nahrajete balíčku zip k. Správce prostředků Azure bude využít [Sdílené podpisy přístup](../storage/storage-dotnet-shared-access-signature-part-1.md) k rozbalovací balíček místně z účtu úložiště při nasazení šablony. Tento proces se automatické prostřednictvím Powershellu skript, který bude nahrání balíčku a volání rozhraní API správy Azure k vytvoření klávesy povinná a předejte můžou být do šablony jako parametry (*_artifactsLocation* a *_artifactsLocationSasToken*). Je třeba definovat parametry složky a název souboru balíčku odesílán v části kontejneru úložiště.

Pak budete muset přidat v jiné vnořené zdroje nastavit vazby hostname využíval vlastní doménu. Bude musíte nejdřív ověřit vlastní název hostitele a nastavit tak, aby být ověřený Azure, že vlastníte doménu ho – přečtěte si téma [konfigurace vaší vlastní doménou v aplikaci služby Azure](web-sites-custom-domain-name.md). Po dokončení můžete přidat následující šablony v části Microsoft.Web/sites zdroje:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Nakonec budete muset přidat další nejvyšší úrovně zdroje Microsoft.Web/certificates. Tento zdroj bude obsahovat certifikát SSL a bude existovat na stejné úrovni jako web app a hostingu plán.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Musíte být platná certifikát SSL, abychom nastavení tohoto zdroje. Až budete mít platný certifikát budete muset extrahovat bajtů pfx jako řetězec ve formátu Base 64. Jednou z možností zleva to je pomocí následujícího příkazu Powershellu:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Můžete pak předat to jako parametr do šablony ARM nasazení.

V tomto okamžiku šabloně ARM je připraven.

###<a name="deploy-template"></a>Nasazení šablony

Poslední kroky se mají část vůbec do nasazení úplného začátku do konce. Usnadnit nasazení že můžete využít **Nasazení AzureResourceGroup.ps1** Powershellu skript, který je přidán při vytváření projekt Azure pole Skupina zdroje ve Visual Studiu pomoci při nahrávání všech artefaktů podle šablony. Vyžaduje vytvořili úložiště účet, který chcete použít předem. V tomto příkladu jsem vytvořil(a) účet sdílené úložiště pro package.zip nahrát. Skript bude využít AzCopy k nahrání balíčku k tomuto účtu úložiště. Předat do umístění složky artefakt a skript automaticky nahrávaly všech souborů v rámci adresáře do kontejneru pojmenované. Po volání nasazení AzureResourceGroup.ps1 budete muset aktualizovat vazby SSL mapovat vlastní hostname s certifikát SSL.

Následující Powershellu zobrazí úplný nasazení volání Deploy-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

V tomto okamžiku aplikace by již byly nasazeny a by měla procházením prostřednictvím https://www.yourcustomdomain.com
