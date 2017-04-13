<properties
   pageTitle="Vývojové a testovací prostředí | Microsoft Azure"
   description="Naučte se pomocí Správce prostředků Azure šablon můžete rychle konzistentní vytvoření, odstranění vývoj a testovacím prostředí."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Vývojové a testovací prostředí Microsoft Azure

Vlastní aplikace jsou obvykle nasazeny více vývoj a testování prostředí před nasazením výroby. Po vytvoření prostředí místně výpočetních prostředků jsou získané nebo přidělený každé prostředí pro každou aplikaci. Prostředí často obsahují několik fyzické nebo virtuálních počítačích s konkrétní konfigurace nasazené ručně nebo se složité automatizaci skriptů. Nasazení často prohlédněte hodin a mít za následek nekonzistentní konfigurace v prostředích.

## <a name="scenario"></a>Scénář ##

Když zřizujete test prostředí v Microsoft Azure a vývoj, můžete jenom zaplatit prostředky, které používáte.  Tento článek vysvětluje, jak rychle a konzistentní můžete vytvořit, zachovat a odstraňte vývoj a testovacím prostředí pomocí Správce prostředků Azure šablon a parametr soubory, jak je znázorněno níže.

![Scénář](./media/solution-dev-test-environments/scenario.png)

Tři testování prostředí a vývoj jsou uvedeny výše.  Má každá webové aplikace a SQL databáze zdroje, které jsou v souboru šablony jazyku.  Názvy aplikace a databáze v každém prostředí se liší a jsou uvedeny v jedinečný parametr souborů pro každé prostředí.  

Pokud znáte není správce prostředků Azure koncepty, je vhodné před čtení v tomto článku přečtěte si článek [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) .

Chcete nejdřív projděte si kroky v tomto článku jak je uvedeno bez čtení všech odkazované články vám rychle získat zkušenosti s používáním šablon správce prostředků Azure. Po už kroky jednou, budete moct projděte si odpovědi na většinu otázek, které vznikly jméno čas prostřednictvím pomocí experimentování další s tímto postupem a čtením odkazované články.

## <a name="plan-azure-resource-use"></a>Plánování využití prostředků Azure
Až budete mít nejvyšší úrovně návrhu aplikace, můžete definovat:

- Které Azure zdroje bude obsahovat aplikace. Můžete vytvářet aplikace a nasazení jako webovou aplikaci Azure s databáze SQL Azure.  Může vytvořit aplikace ve virtuálních počítačích pomocí PHP a MySQL IIS a SQL serveru nebo další součásti. V [aplikaci služby Azure, cloudovými službami a porovnání virtuálních počítačích]( app-service-web/choose-web-site-cloud-service-vm.md) článku vám pomůže se rozhodnout Azure prostředků, které můžete chtít používat pro aplikaci.
- K čemu požadavků na úrovně služeb třeba dostupnost, zabezpečení a měřítko, které splňují aplikace.

## <a name="download-an-existing-template"></a>Stáhněte si existující šablony
Šablony pro správce prostředků Azure definuje všechny Azure prostředky, které využívá vaše aplikace. Několik šablon existovat, že můžete nasadit přímo na portálu Azure nebo stáhnout, upravit a uložte v systému správy zdrojového kódu aplikace.  Postupujte podle pokynů níže ke stažení existující šablony.

1. Procházení existující šablony v úložišti GitHub [Azure rychlý úvod šablony](https://github.com/Azure/azure-quickstart-templates/) . V seznamu uvidíte složku "[201 – web aplikace databázi sql](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Od řada vlastních aplikací zahrnout webové aplikace a databáze SQL, tato šablona umožňuje jako příklad ve zbývající části tohoto článku vám pomůže pochopit, jak pomocí šablony. Je nad rámec tohoto článku plně všechno tato šablona vytvoří a konfiguruje vysvětlit, ale pokud chcete použít k vytvoření skutečné prostředí ve vaší organizaci, je vhodné plně poznat přečíst v článku [poskytnutí web app s databázi SQL](app-service-web/app-service-web-arm-with-sql-database-provision.md) .
Poznámka: Tento článek je určen pro 2015 prosinec verze šablony [201 – web aplikace databázi sql](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) . Na odkazy dole, přejděte na šablonu a mají parametr soubory, které verze šablony.
2. Klikněte na soubor [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) ve složce 201 – web aplikace databázi sql a prohlížet jeho obsah. Toto je soubor šablony správce prostředků Azure. 
3. V režimu zobrazení klikněte na tlačítko "[suroviny](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)". 
4. Pomocí myši vyberte obsah tento soubor a uložit do počítače jako soubor s názvem "TestApp1-Template.json." 
5. Zkontrolujte obsah šablony a Všimněte si takto:
 - Části **zdroje** : Tento oddíl definuje typy Azure zdrojů vytvořené pomocí této šablony. Mezi další typy prostředků tato šablona vytvoří [Azure Web App](app-service-web/app-service-web-overview.md) a [Databáze SQL Azure](sql-database/sql-database-technical-overview.md) zdroje. Pokud chcete spustit a spravovat web a servery SQL ve virtuálních počítačích, můžete použít šablony "[iis 2vm sql 1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" nebo "[svítilna aplikace](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)", ale pokyny v tomto článku jsou založené na šabloně [201 – web aplikace databázi sql](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - **Parametry** oddíl: Tento oddíl definuje parametrů, které jednotlivé zdroje můžete nakonfigurována. Některé parametry zadané v šabloně mají vlastnosti "Výchozí", zatímco ostatní co nedělat. Když nasadíte Azure zdroje se šablonou, je nutné zadat hodnoty všech parametrů, které nemají vlastnosti výchozí hodnota zadané v šabloně.  Pokud nezadáte hodnoty parametrů s vlastností výchozí hodnota, bude použita hodnota zadaná pro parametr VýchozíHodnota v šabloně.

Šablona definuje Azure zdrojů se vytvářejí a parametry jednotlivé zdroje můžete třeba nastavit. Další informace o šablony a jak si Navrhněte vlastní tak, že pro čtení v článku [Doporučené postupy pro navrhování správce prostředků Azure šablony](best-practices-resource-manager-design-templates.md) .

## <a name="download-and-customize-an-existing-parameter-file"></a>Stáhněte si a přizpůsobení existujícího souboru parametrů

Když budete chtít pravděpodobně *stejné* Azure prostředků vytvořené v každém z prostředí, budete pravděpodobně chtít konfigurace prostředky, které mají být *různé* v každém prostředí.  Toto je místo, kam se soubory parametrů. Vytvořte parametr soubory obsahovat jedinečné hodnoty v každém prostředí provedením následujících kroků.   

1. Zobrazte obsah souboru [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) ve složce 201 – web aplikace databázi sql. Toto je parametr soubor pro soubor šablony, které jste uložili v předchozí části. 
2. V režimu zobrazení klikněte na tlačítko "[suroviny](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)". 
3. Pomocí myši vyberte obsah tohoto souboru a je uložíte do tří samostatných souborů na počítači s následujícími názvy:
 - TestApp1 parametry Development.json
 - TestApp1 parametry Test.json
 - TestApp1 parametry Pre Production.json

3. Použití textu nebo JSON editor, upravte vývojové prostředí parametrem soubor, který jste vytvořili v kroku 3, nahradí hodnoty uvedené napravo od hodnoty parametrů v souboru s *hodnotami* uvedené napravo od **Parametry** pod: 
 - **název webu**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Centrální USA*
 - **webu název serveru**: *testapp1devsrv*
 - **serverLocation**: *Centrální USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *nahrazení pomocí hesla*
 - **název databáze**: *testapp1devdb*

4. Použití textu nebo JSON editor upravit testovací soubor parametr prostředí jste vytvořili v kroku 3, nahradí hodnoty uvedené napravo od hodnoty parametrů v souboru s *hodnotami* uvedené napravo od **Parametry** níže:
 - **název webu**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Centrální USA*
 - **webu název serveru**: *testapp1testsrv*
 - **serverLocation**: *Centrální USA*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *nahrazení pomocí hesla*
 - **název databáze**: *testapp1testdb*

5. Použití textu nebo JSON editor se upravte před výrobní parametrem soubor, který jste vytvořili v kroku 3.  Co je pod nahradíte celý obsah souboru:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

V souboru před výrobní parametrů výše uvedené parametry **sku** a **requestedServiceObjectiveName** byly, *přidali*, že nebyly přidané v souborech parametry vývoj a testování. Důvodem je, že jsou hodnoty zadané těchto parametrů v šabloně a v prostředí vývoj a testování jsou použity výchozí hodnoty, ale v před výrobního prostředí jiné než výchozí hodnoty pro tyto parametry používají.

Důvod, proč jiné než výchozí hodnoty jsou použity tyto parametrů v před provozním prostředí je testovat hodnoty těchto parametrů, které můžete chtít pro provozním prostředí, může být testováno.  Tyto všechny parametry týkající se Azure [hostingu plány pro Web App](https://azure.microsoft.com/pricing/details/app-service/)nebo **sku** a [Databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/)Azure nebo **requestedServiceObjectiveName** používané aplikace.  Jinou SKU a cíle názvy služeb, které mají různé náklady a funkce podporují úrovně metriky jiné službě.

Následující tabulka obsahuje výchozí hodnoty pro tyto parametry zadané v šabloně a hodnoty, které jsou použité namísto výchozí hodnoty v souboru parametry před výroby.

| Parametr | Výchozí hodnota | Soubor hodnotu parametru |
|---|---|---|
| **SKU** | Uvolnit | Standardní |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Vytvoření prostředí
Všechny Azure zdroje je vytvořit v rámci [Azure pole Skupina zdroje](azure-resource-manager/resource-group-overview.md). Skupiny zdrojů umožňují seskupení Azure zdrojů, dá se ovládat společně.  [Oprávnění](./active-directory/role-based-access-built-in-roles.md) můžete přiřadit skupinám zdrojů, tak, aby se konkrétní lidi ve vaší organizaci můžete vytvořit, upravit, odstranit nebo zobrazit jejich a zdroje v nich.  Upozornění a fakturační informace o zdrojů ve skupině zdroje můžete zobrazit na [Portál Azure](https://portal.azure.com). Skupiny zdrojů se vytvářejí v Azure [oblast](https://azure.microsoft.com/regions/).  V tomto článku všechny zdroje vytvořené v oblasti centrální USA. Když začnete vytvářet skutečné prostředí, budete vyberte oblast, která odpovídá vašim požadavkům. 

Vytvoření skupiny prostředků pro každé prostředí některým z následujících metod.  Všechny metody dosáhnout stejný výsledek.

###<a name="azure-command-line-interface-cli"></a>Azure rozhraní příkazového řádku (rozhraní příkazového řádku)

Ujistěte se, že máte rozhraní příkazového řádku [nainstalovaný](xplat-cli-install.md) v aplikaci Windows OS X nebo Linux počítače a ke kterým jste [připojení](xplat-cli-connect.md) [účtu Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nazývané také pracovního nebo školního účtu) k předplatnému Azure. Z příkazového řádku rozhraní příkazového řádku zadejte následující příkaz a vytvořte skupina zdroje pro vývojové prostředí.

    azure group create "TestApp1-Development" "Central US"

Pokud neproběhne úspěšně, vrátí příkazu takto:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Pokud chcete vytvořit skupina zdroje pro testovacím prostředí, zadejte následující příkaz:

    azure group create "TestApp1-Test" "Central US"

Pokud chcete vytvořit skupina zdroje pro před provozním prostředí, zadejte následující příkaz:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>Prostředí PowerShell

Ujistěte se, že máte 1.01 Powershellu Azure nebo vyšší nainstalovaný na počítači s Windows a připojili k vašemu předplatnému podrobných informací uvedených v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](powershell-install-configure.md) [účet Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nazývané také pracovní nebo školní účet). Na příkazovém řádku prostředí PowerShell zadejte následující příkaz a vytvořte skupina zdroje pro vývojové prostředí.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Pokud neproběhne úspěšně, vrátí příkazu takto:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Pokud chcete vytvořit skupina zdroje pro testovacím prostředí, zadejte následující příkaz:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Pokud chcete vytvořit skupina zdroje pro před provozním prostředí, zadejte následující příkaz:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure portálu

1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nazývané také pracovní nebo školní e). Klikněte na nový--> Správa--> Skupina zdroje a zadejte "TestApp1 vývoj" do pole název zdroje skupiny, vyberte předplatné a vyberte "Centrální nám" do pole zdrojů skupina umístění jak je znázorněno na následujícím obrázku.
   ![Portál](./media/solution-dev-test-environments/rgcreate.png)
2. Klikněte na tlačítko Vytvořit skupinu zdroje.
3. Klikněte na tlačítko Procházet, přejděte dolů v seznamu skupiny zdrojů a klikněte na skupiny zdrojů, jak je ukázáno v následujícím příkladu.
   ![Portál](./media/solution-dev-test-environments/rgbrowse.png) 
4. Po klepnutí na skupiny zdrojů uvidíte zásuvné skupiny zdroje s novou skupinou zdroje.
   ![Portál](./media/solution-dev-test-environments/rgview.png)
5. Vytvoření TestApp1-testu a TestApp1 Pre výrobních zdrojů skupiny stejným způsobem jako jste vytvořili skupina zdroje TestApp1 vývoj výše.

##<a name="deploy-resources-to-environments"></a>Nasazení zdroje se prostředí

Nasazení Azure materiály ke skupinám zdrojů pro každé prostředí pomocí šablony souboru pro řešení a soubory parametrů pro každé prostředí některým z následujících metod.  Obě metody dosáhnout stejný výsledek.

###<a name="azure-command-line-interface-cli"></a>Azure rozhraní příkazového řádku (rozhraní příkazového řádku)

Z příkazového řádku rozhraní příkazového řádku zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro vývojové prostředí: nahrazení [cesta] cesta ke svým souborům uloženým v předchozích krocích.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Po zobrazení zprávy "Čeká se na nasazení dokončete" několik minut, příkaz vrátí následující Pokud neproběhne úspěšně:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Pokud příkaz neproběhne úspěšně, vyřešit chybové zprávy a zkuste znova.  Běžné problémy používáte hodnoty parametrů, které nejsou dodržovat naming omezení Azure zdrojů. Další tipy najdete v článku [Poradce při potížích zdrojů skupina nasazení v Azure](./resource-manager-troubleshoot-deployments-cli.md) .

Z příkazového řádku rozhraní příkazového řádku zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro testovacím prostředí [cesta] nahraďte cestu ke svým souborům uloženým v předchozích krocích.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Z příkazového řádku rozhraní příkazového řádku zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro před provozním prostředí, [cesta] nahraďte cestu ke svým souborům uloženým v předchozích krocích.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>Prostředí PowerShell

Z příkazového řádku Azure PowerShell (verze 1.01 nebo vyšší) zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro vývojové prostředí: nahrazení [cesta] cesta ke svým souborům uloženým v předchozích krocích.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Po zobrazení blikající kurzor několik minut, příkaz vrátí následující Pokud neproběhne úspěšně:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Pokud příkaz neproběhne úspěšně, vyřešit chybové zprávy a zkuste znova.  Běžné problémy používáte hodnoty parametrů, které nejsou dodržovat naming omezení Azure zdrojů. Další tipy najdete v článku [Poradce při potížích zdrojů skupina nasazení v Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  Na příkazovém řádku prostředí PowerShell zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro testovacím prostředí [cesta] nahraďte cestu ke svým souborům uloženým v předchozích krocích.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  Na příkazovém řádku prostředí PowerShell zadejte příkaz dole nasadit zdrojů Skupina zdroje, že kterou jste vytvořili pro před provozním prostředí, [cesta] nahraďte cestu ke svým souborům uloženým v předchozích krocích.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Soubory šablony a parametru lze verzí a udržované kódem aplikace v systému správy zdrojů.  Můžete také uložit výše uvedeného soubory skriptů a jejich uložení s kódem taky příkazy.

> [AZURE.NOTE] Šablony můžete nasadit přímo kliknutím na tlačítko "Nasadit na Azure" v článku [poskytnutí Web App s databázi SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) Azure.  Mohou být užitečné to se naučit používat šablony, ale byste není umožňují upravit, verze a uložení šablony a parametr hodnoty kódem aplikace tak další podrobné informace o této metody není uvedené v tomto článku.

## <a name="maintain-environments"></a>Udržovat prostředí
V celém vývoj konfiguraci Azure prostředků v různých prostředích může být nekonzistentně změněno úmyslně nebo omylem.  Nepotřebných odstraňování problémů a řešení problému může dojít během cyklu vývoje aplikace.

1. Změníte prostředí otevřením [Azure portálu](https://portal.azure.com). 
2. Přihlaste se do něho se stejným účtem, který jste použili k dokončení těchto kroků. 
3. Co zobrazené na obrázku dole, klikněte na Procházet--> skupiny zdrojů (budete muset posunout dolů zobrazíte skupiny zdrojů).
   ![Portál](./media/solution-dev-test-environments/rgbrowse.png)
4. Po klepnutí na skupiny zdrojů na obrázku nahoře, zobrazí se zásuvné skupiny zdrojů a skupin tři zdrojů, který jste vytvořili v předchozím kroku, jak je znázorněno na následujícím obrázku. Klikněte na skupina zdroje TestApp1 vývoj a zobrazí se zásuvné, který obsahuje seznam zdrojů vytvořené pomocí šablony v nasazení skupina zdroje TestApp1 vývoj prováděny v předchozím kroku.  Odstranění zdroje TestApp1DevApp v prohlížeči kliknutím TestApp1DevApp v zásuvné skupiny TestApp1 vývoj zdroje a potom na Odstranit v zásuvné TestApp1DevApp Web app.
   ![Portál](./media/solution-dev-test-environments/portal2.png)
5. Klikněte na tlačítko "Ano" při portálu se zobrazí dotaz, jestli opravdu že chcete odstranit zdroj. Zavření zásuvné skupiny TestApp1 vývoj zdroje a znovu ho otevřete teď zobrazí ho bez Web appu, který jste právě odstranili.  Obsah skupina zdroje je teď liší by měl být. Dále můžete experimentovat odstranění více zdrojů z více zdrojů skupin nebo dokonce změnou nastavení pro některé z zdrojů. Místo na portálu Azure odstranit zdroj ze skupiny zdrojů, může pomocí příkazu Powershellu [Odebrat AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) nebo nebo příkaz "azure zdroje odstranit" z rozhraní příkazového řádku provádění stejného úkolu.
6. Všechny zdroje a konfiguraci, která mají být ve skupinách zdroje zpátky do stavu, ve kterém by měl být na získáte znovu nasadit prostředí skupinám zdrojů pomocí stejného příkazy, které jste použili v části [zdroje nasazení se prostředí](#deploy-resources-to-environments) , ale nahradit "Deployment1" s "Deployment2."
7.  Jak je vidět v části Souhrn zásuvné TestApp1 vývoj obrázku vidíte v kroku 4, uvidíte, že znovu existuje Web appu, který jste odstranili z portálu v předchozím kroku, stejně jako jakékoli jiné zdroje mohou rozhodli jste odstranit. Pokud jste změnili konfiguraci některý ze zdrojů, můžete taky ověřit, že byla nastavena jejich zpět na hodnoty v parametru soubory příliš. Jednou z výhod nasazení prostředí se šablonami správce prostředků Azure je, že můžete snadno znovu nasadit prostředí zpátky na známý stav kdykoli.
8. Když kliknete na text v části "Příjmení"nasazení na obrázku dole, zobrazí se zásuvné zobrazující historie nasazení skupina zdroje.  Protože použili název "Deployment1" pro první nasazení a "Deployment2" druhý nasazení budete mít dvě položky.  Zásuvné zobrazující výsledky každého nasazení se zobrazí po kliknutí na nasazení.
  ![Portál](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Odstranění prostředí
Jakmile dokončíte pomocí prostředí, je vhodné se odstraní, takže není vzniknou hovorného pro Azure zdroje, které už používáte.  Odstranění prostředí je ještě snazší než jejich vytvoření.  V předchozích krocích jednotlivé skupiny zdrojů Azure byly vytvořené pro každé prostředí a potom byly nasazeny zdrojů do skupiny zdrojů. 

Odstranění prostředí některým z následujících metod.  Všechny metody dosáhnout stejný výsledek.

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Na řádku rozhraní příkazového řádku zadejte tento příkaz:

    azure group delete "TestApp1-Development"

Po zobrazení výzvy zadejte y a stiskněte klávesu enter odebrat vývojové prostředí a všech jeho prostředků. Po několika minutách vrátí následující příkaz:

    info:    group delete command OK

Na řádku rozhraní příkazového řádku zadejte následující příkaz pro odstranění zbývajících prostředí:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>Prostředí PowerShell

Z příkazového řádku Azure PowerShell (verze 1.01 nebo vyšší) zadejte následující příkaz pro odstranění skupina zdroje a její obsah.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Po zobrazení výzvy si jistí, chcete-li odebrat skupina zdroje, zadejte y, následovaný klávesu enter.

Zadejte následující příkaz pro odstranění zbývajících prostředí:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure portálu

1. Na portálu Azure procházet skupiny zdrojů stejně jako v předchozích krocích. 
2. Vyberte skupinu pro zdroje TestApp1 vývoj a klikněte na tlačítko Odstranit ve zásuvné skupiny TestApp1 vývoj zdroje. Zobrazí se nové zásuvné. Zadejte název skupiny zdrojů a klikněte na tlačítko Odstranit.
![Portál](./media/solution-dev-test-environments/rgdelete.png)
3. Odstranění TestApp1-testu a TestApp1 Pre výrobních zdrojů skupiny stejným způsobem jako jste odstranili TestApp1 vývoj skupina zdroje.

Bez ohledu na to metody, kterou používáte skupiny zdrojů a všechny ostatní prostředky, které jsou obsaženy přestane existovat a budete už vzniknou fakturační nákladů pro zdroje.  

Aby byl minimalizován Azure výdaje využití prostředků zaplatíte během vývoj aplikací [Azure automatizaci](automation/automation-intro.md) můžete použít k naplánování úkolů, které:

- Zastavte virtuálních počítačích na konci každého dne a restartujte na začátku každého dne.
- Odstranění celých prostředí na konci každého dne a znovu je vytvořte na začátku každého dne.
 
Teď máte zkušenosti, jak je snadné vytvářet, spravovat a odstranění vývoj a testovacím prostředí, můžete se dozvědět víc o tom, jaký vám jenom nebyla tak, že další experimentovat se výše uvedené kroky a čtení odkazy uvedenými v tomto článku.

## <a name="next-steps"></a>Další kroky

- [Delegovat správu](./active-directory/role-based-access-control-configure.md) různých zdrojů v každém prostředí přiřazením Microsoft Azure AD skupinám nebo uživatelům určité role, které mají možnost provádět podmnožinu operace Azure zdroje.
- [Přiřazení značky](resource-group-using-tags.md) k skupiny zdrojů pro každé prostředí a/nebo jednotlivých zdrojů. Můžete přidat značku "Prostředí" skupinám zdrojů a nastavte jeho hodnotu tak, aby odpovídaly názvům prostředí. Značky může být užitečné, když potřebujete uspořádání prostředky pro fakturace nebo Správa.
- Sledovat oznámení a fakturace zdrojů Skupina zdroje v [Azure portálu](https://portal.azure.com).
