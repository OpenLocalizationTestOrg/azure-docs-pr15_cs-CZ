<properties
   pageTitle="Vytváření šablon Azure správce prostředků | Microsoft Azure"
   description="Vytvoření šablony správce prostředků Azure pomocí deklarativní JSON syntaxe pro nasazení aplikace Azure."
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
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Vytváření šablon správce prostředků Azure

Toto téma popisuje strukturu šablony pro správce prostředků Azure. Zobrazí různé části šablony a vlastnosti, které jsou dostupné v těchto oddílech. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnoty pro nasazení. 

Šablona pro prostředky, které jste již nainstalovali naleznete v tématu [Export správce prostředků Azure šablony z existujících zdrojů](resource-manager-export-template.md). Vytvoření šablony, najdete v článku [Správce prostředků šablony návodu](resource-manager-template-walkthrough.md). Doporučení týkající se vytváření šablon najdete v článku [Doporučené postupy pro vytváření šablon správce prostředků Azure](resource-manager-template-best-practices.md).

Úkol vytváření šablon můžete zjednodušit dobré editor JSON. Informace o používání aplikace Visual Studio s šablon najdete v tématu [Vytvoření a nasazení Azure zdroje skupiny pomocí aplikace Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informace o používání a kód najdete v článku [práce s Azure správce prostředků šablon ve Visual Studiu kódu](resource-manager-vs-code.md).

Omezení velikosti šablona 1 MB a každý parametr soubor 64 kB. Limit 1 MB platí pro konečný stav šablony po rozbalení s definice iterativních zdroje a hodnoty proměnných a parametry. 

## <a name="template-format"></a>Formát šablony

Nejjednodušší struktury šablona obsahuje následující elementy:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Název elementu   | Povinné | Popis
| :------------: | :------: | :----------
| $schema        |   Ano    | Umístění souboru JSON schématu popisující verze jazyka šablony. Použijte adresu URL zobrazenou v předchozím příkladu.
| contentVersion |   Ano    | Verze šablony (například 1.0.0.0). Zadejte libovolnou hodnotu tohoto prvku. Když nasadíte zdrojů pomocí šablony, tuto hodnotu mohou sloužit k ověření, že je použit správnou šablonu.
| Parametry     |   Ne     | Hodnoty, které jsou k dispozici při spuštění nasazení můžete přizpůsobit zdroje nasazení.
| proměnné      |   Ne     | Hodnoty, které jsou používána jako JSON neúplné v šabloně zjednodušit výrazy jazyka pro šablonu.
| zdroje informací      |   Ano    | Typy zdrojů, které jsou nasazení nebo aktualizovala v skupina zdroje.
| výstup        |   Ne     | Hodnoty, které se vracejí po nasazení.

Doporučujeme zkontrolovat v částech šablony podrobněji dál v tomto tématu. Prozatím zvážíme, některé syntaxe, které tvoří šablonu.

## <a name="expressions-and-functions"></a>Výrazy a funkce

Základní syntaxe šablony je JSON. Však výrazech a funkcích rozšířit JSON, která je dostupná v šabloně. Použití výrazů vytvoříte hodnoty, které nejsou striktně literálů. Výrazy jsou uzavřeny s hranaté závorky [a] a jsou vyhodnoceny při nasazení šablony. Výrazy můžete umístit na libovolné místo v hodnotu řetězce JSON a vždy vrátí jinou hodnotu JSON. Pokud je potřeba použít literál typu řetězec, který začíná závorek [, je nutné použít dva hranaté závorky [[.

Obvykle používáte výrazy s funkcemi k provádění operací pro konfiguraci nasazení. Stejně jako v JavaScript, volání funkce jsou formátované jako **functionName(arg1,arg2,arg3)**. Vlastnosti odkazovat pomocí operátorů tečkou a [index].

Následující příklad ukazuje, jak používat několik funkcí při vytváření hodnoty:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Úplný seznam šablony funkcí naleznete v tématu [funkce šablony správce prostředků Azure](resource-group-template-functions.md). 


## <a name="parameters"></a>Parametry

V části Parametry šablony zadejte hodnoty, které je můžete zadat při nasazení zdroje. Tyto hodnoty parametrů umožňují přizpůsobit nasazení zadáním hodnoty, které jsou přizpůsobené konkrétním prostředí (třeba vývojáře test a výrobní). Není nutné poskytnout parametrů v šabloně, ale bez parametrů by nasazení šablony vždy stejné zdroje se stejným názvy, umístění a vlastnosti.

Tyto hodnoty parametrů v šabloně slouží k nastavení hodnoty pro nasazeném zdroje. V dalších částech šablony lze použít pouze parametry deklarované v části parametry.

Definice parametrů pomocí následující strukturu:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Název elementu   | Povinné | Popis
| :------------: | :------: | :----------
| Název parametru  |   Ano    | Název parametru. Musí být platná identifikátor JavaScript.
| Typ           |   Ano    | Typ hodnoty parametrů. Zobrazíte seznam pod povolené typy.
| Výchozí hodnota   |   Ne     | Výchozí hodnota parametru, pokud je zadána žádná hodnota parametru.
| allowedValues  |   Ne     | Pole povolené hodnoty parametru abyste měli jistotu, že je k dispozici hodnota vpravo.
| minValue       |   Ne     | Minimální hodnota int typ parametrů tato hodnota je včetně.
| maxValue       |   Ne     | Maximální hodnota int typ parametrů tato hodnota je včetně.
| minLength      |   Ne     | Minimální délka řetězce, secureString a pole Typ parametrů tato hodnota je včetně.
| maxLength      |   Ne     | Maximální délka řetězce, secureString a pole Typ parametrů, tato hodnota je včetně.
| Popis    |   Ne     | Popis parametr, který se zobrazí uživatelům šablony prostřednictvím rozhraní portálu vlastní šablonu.

Povolené typy a hodnoty jsou:

- **řetězec**
- **secureString**
- **Funkce INT**
- **Logická hodnota**
- **objekt** 
- **secureObject**
- **pole**

Chcete-li parametr jako volitelné, zadejte výchozí hodnotu (může být prázdný řetězec). 

Pokud zadáte název parametru, který odpovídá některému z parametrů v příkazu pro nasazení šablony, se zobrazí výzva k zadat hodnotu parametru s přípona **FromTemplate**. Například pokud zahrnete parametr s názvem **ResourceGroupName** v šabloně, která je stejná jako parametr **ResourceGroupName** v [Nový AzureRmResourceGroupDeployment] [ deployment2cmdlet] rutina, zobrazí se výzva k zadání hodnoty pro **ResourceGroupNameFromTemplate**. Obecně neměli byste tento zapomíná a to tak, že není pojmenování parametrů se stejným názvem jako parametry používané pro nasazení operace.

>[AZURE.NOTE] Všechna hesla, klíče a další tajemství používejte typ **secureString** . Parametry šablony s typem secureString nečte po nasazení zdroje. 

Následující příklad ukazuje, jak definovat parametry:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Informace o vstupní hodnoty parametrů během nasazení najdete v článku [nasadit aplikaci Správce prostředků Azure šablony](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Proměnné

V části proměnné vytvářet hodnoty, které lze použít v celém vaši šablonu. Obvykle proměnné jsou na základě hodnot poskytované parametry. Nemusíte definovat proměnné, ale budou často zjednodušit šablony jejich zmenšením složených výrazech.

Definování proměnných následující strukturu:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Následující příklad ukazuje, jak definovat proměnná, která je vytvořen ze dvou hodnoty parametrů:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Následující příklad ukazuje proměnná, která je složité typu JSON a proměnných, které jsou vytvořen z jiných proměnných:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Zdroje informací

V části zdroje definujete prostředky, které jsou nasazeném a aktualizovat. V této části můžete získat složité, protože je třeba porozumět typů, které nasazujete zadat správné hodnoty. 

Definování zdroje s struktuře následující:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Název elementu             | Povinné | Popis
| :----------------------: | :------: | :----------
| apiVersion               |   Ano    | Verze rozhraní REST API pro použití při vytváření zdroje.
| Typ                     |   Ano    | Typ zdroje. Tato hodnota je tvořen kombinací oboru poskytovatele zdroje a typ zdroje (například **Microsoft.Storage/storageAccounts**).
| Jméno                     |   Ano    | Název zdroje. Název musí splňovat URI součásti omezení podle RFC3986. Kromě toho Azure služby, které poskytují název zdroje chcete třetími stranami ověřit jména Ujistěte se, zda je pokus o oklamání jiné identity. Zjistěte, [Zkontrolovat název zdroje](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| umístění                 |   Liší  | Podporované geo umístění zadané zdroje. Můžete vybrat libovolný z dostupných umístění, ale obvykle má smysl vyberte tu, která je tomu vašemu nejbližší uživatelů. Obvykle také má smysl umístit prostředky, které spolupracují s překrývajícími ve stejné oblasti. Většina typy zdrojů vyžadují umístění, ale některé typy (třeba přiřazování rolí) nevyžadují umístění.
| značky                     |   Ne     | Značky, které jsou přidružené k zdroje.
| komentáře                 |   Ne     | Poznámky pro dokumentaci prostředků v šabloně
| dependsOn                |   Ne     | Prostředky, které zdroje definovaného závisí na. Závislosti mezi zdroje jsou vyhodnoceny a zdroje jsou nasazenou v jejich závislá pořadí. Když prostředků nezávisí na sobě, nasazením souběžně. Hodnota může být čárkou oddělený seznam zdroje názvy nebo identifikátory jedinečné zdroje.
| Vlastnosti               |   Ne     | Nastavení konfigurace specifické pro zdroje. Hodnoty pro vlastnosti jsou stejná jako hodnoty, které zadáte v hlavním textu žádosti o pro rozhraní REST API operaci (metoda umístění), která má vytvořit zdroj. Odkazy na zdroje si přečtěte následující dokumentaci schématu nebo rozhraní REST API najdete v článku [Správce prostředků poskytovatelů, oblastí, verze rozhraní API a schémat](resource-manager-supported-services.md).
| Kopírovat                     |   Ne     | V případě potřeby více než jedna instance počet zdrojů k vytvoření. Další informace najdete v tématu [vytvoření několika instancích zdrojů v Azure správce](resource-group-create-multiple.md). |
| zdroje informací                |   Ne     | Podřízené prostředky, které jsou závislé na zdroje definovaného. Můžete zadat jenom typy zdrojů, které jsou povoleny schématem nadřazené zdroje. Plně kvalifikovaný název typu zdroje podřízené obsahuje nadřazený typ zdroje, jako je **Microsoft.Web/sites/extensions**. Závislost na zdroje nadřazené není určen; je nutné explicitně definovat tuto závislost. 

Znalost hodnoty můžete zadat pro **apiVersion**, **Typ**a **umístění** není okamžitě zjevných. Naštěstí můžete zjistit tyto hodnoty pomocí prostředí PowerShell Azure nebo Azure rozhraní příkazového řádku.

Všech poskytovatelů zdroje s **prostředím PowerShell**, použijte:

    Get-AzureRmResourceProvider -ListAvailable

Ze seznamu vrácené najdete poskytovatele zdroje, které vás zajímají. Typy prostředků zdroj zprostředkovatele (jako je třeba úložiště), použijte:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Verze rozhraní API pro typ zdroje (třeba úložiště účty), použijte:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Podporované umístění pro typ zdroje, použijte:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Získání všech poskytovatelů zdroje s **Azure rozhraní příkazového řádku**, můžete:

    azure provider list

Ze seznamu vrácené najdete poskytovatele zdroje, které vás zajímají. Typy prostředků zdroj zprostředkovatele (jako je třeba úložiště), použijte:

    azure provider show Microsoft.Storage

Podporované umístění a rozhraní API verze, použijte:

    azure provider show Microsoft.Storage --details --json

Další informace o poskytovatelích zdroje, najdete v článku [poskytovatelé správce prostředků, oblastí, verze rozhraní API a schémata](resource-manager-supported-services.md).

Části zdroje obsahuje matici prostředky do nasazení. V rámci jednotlivé zdroje můžete definovat maticových podřízené zdrojů. Části zdroje proto, může mít strukturu jako:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Následující příklad ukazuje **Microsoft.Web/serverfarms** zdrojů a **Microsoft.Web/sites** zdroje s podřízeným **rozšíření** zdroje. Všimněte si, že web je označený jako jako závisí na serverové farmy od serverové farmy musí existovat před může být nasazené na web. Všimněte si příliš, zda je zdroj **rozšíření** podřízený web.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Výstupy

V části výstupy zadejte hodnoty, které vracejí z nasazení. Můžete třeba vracet URI pro přístup k nasazeném zdroje.

Následující příklad ukazuje strukturu definici výstup:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Název elementu   | Povinné | Popis
| :------------: | :------: | :----------
| outputName     |   Ano    | Název výstupní hodnotu. Musí být platná identifikátor JavaScript.
| Typ           |   Ano    | Typ výstupní hodnotu. Výstupní hodnoty podporovat stejný typy jako šablonu vstupní parametry.
| Hodnota          |   Ano    | Výraz jazyka šablony, který je Vyhodnocená a vrácená jako výstupní hodnotu.


Následující příklad zobrazuje hodnota vrácená v části výstupů.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Další informace o práci s výkonem najdete v článku [Sdílení stavu Správce prostředků Azure šablony](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Další kroky
- Pokud chcete zobrazit úplný šablony pro mnoho různých typů řešení, najdete v článku [Azure rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/).
- Podrobnosti o těchto funkcích, které můžete použít u v rámci šablon najdete v tématu [Funkce šablony Azure správce prostředků](resource-group-template-functions.md).
- Zkombinování několika šablon při nasazení, najdete v článku [použití šablon propojené s správce prostředků Azure](resource-group-linked-templates.md).
- Budete muset používat prostředky, které jsou ve skupině jiné zdroje. Tento postup je běžný při práci s účty úložiště nebo virtuální sítě, které jsou sdíleny více skupin zdroje. Další informace najdete v nápovědě k [funkci resourceId](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
