<properties
   pageTitle="Vytvoření šablony nasazení aplikace logiky | Microsoft Azure"
   description="Naučte se vytvořit šablonu logiky aplikace nasazení a použití ke správě vydání"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Vytvoření šablony nasazení aplikace logiky

Po vytvoření logiku aplikace můžete vytvořit jako šablonu správce prostředků Azure. Tímto způsobem můžete snadno nasadit aplikaci logiky prostředí nebo kde může být nutné pole Skupina zdroje. Úvod do šablon správce prostředků nezapomeňte podívejte se na články o [vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md) a [nasazení zdrojů pomocí Správce prostředků Azure šablon](../resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Šablona nasazení aplikace logiky

Použití logických operátorů aplikace má tři základní součásti:

* **Použití logických operátorů aplikace zdroje**. Tento zdroj obsahuje informace o věcech jako cena za plán, umístění a definici pracovního postupu.
* **Definice pracovního postupu**. Je to, co se zobrazí v zobrazení kódu. Zahrnuje definici kroky tok a jak by měl provést modul. Toto je `definition` vlastnost logiky aplikace zdroje.
* **Připojení**. Toto jsou samostatné prostředky, které bezpečně ukládá metadata libovolnou spojnici připojení, například připojovacího řetězce a přístupový token. Tyto v aplikaci logiku v odkazu `parameters` část logiku aplikace zdroje.

Zobrazit všechny tyto použitelné pro existující logiku aplikace pomocí nástroje jako [Explorer Azure zdroje](http://resources.azure.com).

Chcete-li šablonu pro aplikace logiky pro práci s nasazení skupina zdroje, potřebujete definovat zdroje a parametrizovat podle potřeby. Třeba když nasazujete vývoj, test a provozním prostředí, můžete budete pravděpodobně chtít používat jinými řetězci připojení k databázi SQL v každém prostředí. Nebo můžete chtít nasadit v rámci různých předplatných nebo skupiny zdrojů.  

## <a name="create-a-logic-app-deployment-template"></a>Vytvoření šablony nasazení aplikace logiky

Nejjednodušší způsob, jak mají šablona nasazení aplikace platné logiky je použití [Visual Studio Tools for logiky aplikace](./app-service-logic-deploy-from-vs.md).  V sekci nástroje Visual Studio generovat platné nasazení šablonu, která mohou sloužit přes předplatné nebo umístění.

Několik dalších nástrojů vám mohou pomoci při vytváření logiky šablony nasazení aplikace. Můžete vytváříte ručně, tedy pomocí zdroje už zde popsané podle potřeby vytvářet parametry. Další možností je pomocí prostředí PowerShell modulu [logiky aplikace šablony poznámkové bloky pro školy](https://github.com/jeffhollan/LogicAppTemplateCreator) . Tento modul otevřít zdroj nejdřív vyhodnotí aplikaci logiky a všechna připojení, používá a potom generuje prostředky šablony s potřebných parametrů pro nasazení. Například pokud máte aplikaci použití logických operátorů, který obdrží zprávu z fronty Bus služby Azure a přidání dat do databáze Azure SQL, nástroj zachovat všechny logiky průběhu a parametrizovat řetězce připojení SQL a služby Bus tak, aby mohla být nastavena na nasazení.

>[AZURE.NOTE] Připojení musí být ve stejné skupině zdroje logiky aplikace.

### <a name="install-the-logic-app-template-powershell-module"></a>Instalace modulu logiky šablony aplikace pro PowerShell

Nejjednodušší způsob, jak nainstalovat modul spočívá ve využití [Prostředí PowerShell Galerie](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)pomocí příkazu `Install-Module -Name LogicAppTemplate`.  

Můžete taky nainstalovat modul Powershellu ručně:

1. Stáhněte si nejnovější verzi aplikace [logiky aplikace šablony poznámkové bloky pro školy](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Extrahování složky ve složce modul prostředí PowerShell (obvykle `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Modulu pro práci s klienta a předplatné přístup token, doporučujeme použít pomocí nástroje [ARMClient](https://github.com/projectkudu/ARMClient) příkazového řádku.  Tento [příspěvek blogu](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) popisuje ARMClient podrobněji.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generování logiky šablony aplikace pomocí prostředí PowerShell

Po instalaci prostředí PowerShell můžete vygenerovat šablony pomocí tento příkaz:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`je ID Azure předplatného. Tento řádek nejdřív získá přístupový token prostřednictvím ARMClient, pak potrubí prostřednictvím skriptu prostředí PowerShell a potom vytvoří šablonu v souboru JSON.

## <a name="add-parameters-to-a-logic-app-template"></a>Přidání parametry do šablony aplikace logiky

Po vytvoření šablony aplikace použití logických operátorů, můžete dál přidat nebo změnit parametrů, které může být nutné. Například pokud vaše definice obsahuje číslo ID zdroje s Azure funkce nebo vnořené pracovního postupu, který chcete nasadit v jednom nasazení, můžete do šablony přidat další materiály a parametrizovat ID podle potřeby. Platí pro všechny odkazy na vlastní rozhraní API nebo Swagger koncové body, které plánujete nasazení s každé pole Skupina zdroje.

## <a name="deploy-a-logic-app-template"></a>Nasazení šablony aplikace logiky

Nasazení šablony pomocí nástroje, včetně Powershellu, rozhraní REST API, správu verze Visual Studio a nasazení šablona portál Azure jiné číslo. Viz Tento článek o [nasazení prostředků pomocí Správce prostředků Azure šablon](../resource-group-template-deploy.md) pro další informace. Doporučujeme také vytvořit [parametr souboru](../resource-group-template-deploy.md#parameter-file) pro ukládání hodnoty parametru.

### <a name="authorize-oauth-connections"></a>Povolit připojení OAuth

Po zavedení funguje aplikace logiku začátku do konce s platné parametry. Však OAuth připojení se potřebujete k tomu generovat token platný přístup. Můžete provést otevírat logiky v návrháři a povolení připojení. Nebo pokud chcete proces zautomatizovat, můžete skript souhlas s každé OAuth připojení. Příklad skriptu na není GitHub v rámci projektu [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) .

## <a name="visual-studio-release-management"></a>Správa verzi aplikace Visual Studio

Běžné situace pro nasazení a správu prostředí, je použít nástroje, jako je Správa vydání Visual Studio šablonou nasazení aplikace logiku. Visual Studio týmovou obsahuje [Nasazení pole Skupina zdroje Azure](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) úkolu, že můžete přidat na libovolnou vybudování nebo uvolněte kanálem k odesílání zpráv. Musíte mít [služby základní](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) registraci pro nasazení a pak si můžete vygenerovat definici vydání.

1. V části Správa vydání Pokud chcete vytvořit novou definici, vyberte **prázdné** začít s prázdnou definice.

    ![Vytvoření nové, prázdné definice][1]   

1. Zvolte zdroje, které potřebujete pro tuto. Šablona aplikace logiky generovaného ručně nebo jako součást procesu sestavení pravděpodobně bude.
1. Přidání úkolu **Azure zdrojů skupina nasazení** .
1. Konfigurace [služby základní](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)a odkazovat na šablonu a parametrů šablony soubory.
1. Pokračujte vytvoření postup v procesu uvolnění pro všechny prostředí, automatického testování nebo schvalovatelé podle potřeby.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
