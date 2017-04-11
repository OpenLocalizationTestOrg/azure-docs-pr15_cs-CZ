<properties
    pageTitle="Zpracování stavu Správce prostředků šablony | Microsoft Azure"
    description="Zobrazuje doporučené postupy pro sdílení dat o stavu se šablonami správce prostředků Azure a propojené šablony pomocí složitých objektů"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Sdílení stavu Správce prostředků Azure šablony

Toto téma popisuje doporučené postupy pro správu a sdílení stavu v rámci šablony. Parametry a proměnné zobrazené v tomto tématu jsou příklady typů objektů můžete definovat pohodlně uspořádání vašim požadavkům na nasazení. Z těchto příkladů je implementovat vlastní objektů s nemovitostí s hodnotou, které vyhovují prostředí.

Toto téma je součástí větší dokument. Si Pokud chcete přečíst celou papíru, stáhněte si [světě třídy zdroje Správce šablony aspektech a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Zadejte standardní konfiguraci nastavení

Namísto nabízejí šablonu, která poskytuje celkové flexibilitu a některé změny, běžné vzorek je poskytovat výběru známé konfigurace. Ve skutečnosti mohou uživatelé vybrat standardní velikosti tričko například izolovaného prostoru, malý, střední a velká. Další příklady tričko velikosti jsou častou, například komunity edition nebo enterprise edition. V ostatních případech může být specifické pro úlohu konfigurace technologie – třeba zmenšení mapy nebo bez sql.

S složité objekty můžete vytvořit proměnné, které obsahují kolekce dat, někdy označované jako "vlastnost vaky" a použijte tato data řídit deklaraci prostředků v šabloně. Tento přístup poskytuje dobrý známé konfigurací různých velikostí, které jsou automaticky předem nakonfigurovaná pro zákazníky. Bez známé konfigurace uživatelé šablony musí určit obrázku pro změnu velikosti na vlastní faktor omezení zdrojů platformy a výpočty k identifikaci výsledné rozdělení účtů úložiště a další zdroje (z důvodu omezení prostředků a velikost obrázku). Kromě připojený k pobočkové lepší pro zákazníka, jsou snadněji několik známé konfigurace podpory a pomáhají vám poskytovat do vyšší úrovně hustoty.

Následující příklad ukazuje, jak definovat proměnné obsahující složité objekty představující kolekce dat. Kolekce definovat hodnoty, které se používají pro velikost virtuálního počítače, nastavení sítě, nastavení operačního systému a nastavení dostupnosti.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Všimněte si, že proměnnou **tshirtSize** zřetězí požadovanou velikost tričko poskytovanou parametru (**malá**, **Střední**, **velká**) do textu **tshirtSize**. Tuto proměnnou použít k načtení přidružené složité objektová proměnná pro danou tričko velikost.

Pak můžete odkázat tyto proměnné dál v šabloně. Možnost odkaz s názvem proměnné a jejich vlastnosti zjednodušuje syntaxe šablony a usnadňuje Principy kontextu. Následující příklad definuje zdroj nasazení pomocí objektů zobrazeny dříve nastavení hodnot. Například, nastavte velikost OM načtením hodnoty pro `variables('tshirtSize').vmSize` při hodnotu pro velikost disku je načtená z kontingenčního seznamu `variables('tshirtSize').diskSize`. Kromě toho nastavit URI pro šablonu propojené s hodnotou pro `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Předání stavu do šablony

Stát sdílet do šablony prostřednictvím parametrů, které jsou zdrojem přímo během nasazení.

V následující tabulce jsou uvedeny běžně používaných parametrů v šablony.

Jméno | Hodnota | Popis
---- | ----- | -----------
umístění    | Řetězec s omezením seznamu Azure oblastí   | Umístění, kde jsou nasazeny zdroje.
storageAccountNamePrefix    | Řetězec    | Jedinečný název účtu úložiště, kde jsou uložená OM disků DNS
název_domény  | Řetězec    | Název domény veřejně přístupný jumpbox OM ve formátu: **{název_domény}. { umístění}.cloudapp.com** například: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Řetězec    | Uživatelské jméno pro VMs
adminPassword   | Řetězec    | Heslo pro VMs
tshirtSize  | Řetězec s omezením seznamu nabízených velikosti trička   | Velikost jednotky pojmenované měřítko trvat. Třeba "Small", "střední", "Velké"
virtualNetworkName  | Řetězec    | Název virtuální sítě, který chce příjemce používat.
enableJumpbox   | Řetězec s omezením seznamu (povolit nebo zakázat) | Parametr, který identifikuje povolit jumpbox pro prostředí. Hodnoty: "povoleno", "zakázané"

**TshirtSize** použitým v předchozí části je definován takto:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Stát předat propojené šablony

Při připojování k propojené šablony se často použijte kombinaci statického a generovaného proměnné.

### <a name="static-variables"></a>Statická proměnných

Statické proměnné se často používá k poskytování základní hodnot, například adresy URL, které se používají v šabloně.

V následující úryvek šablony `templateBaseUrl` Určuje kořenové umístění šablony v GitHub. Na další řádek vytvoří novou proměnnou `sharedTemplateUrl` , zřetězí základní adresu URL s známé název šablony, sdílené materiály. Pod řádkem, složité objektová proměnná slouží k uložení tričko velikost, kde je základní adresu URL aby umístění šablony známý konfigurační a uložené v `vmTemplate` vlastnost.

Výhodou tento přístup je, že pokud se změní umístění šablony jenom potřebujete změnit statická proměnná na jednom místě, který splňuje v celém propojené šablony.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Vygenerovaný proměnných

Kromě statický proměnné několik proměnných vygenerují dynamické. V této části jsou uvedeny některé běžné typy vygenerovaných proměnných.

#### <a name="tshirtsize"></a>tshirtSize

Máte zkušenosti s touto vygenerovaných proměnnou z výše uvedené příklady.

#### <a name="networksettings"></a>networkSettings

V kapacitu, funkce nebo šablona obory řešení začátku do konce propojené šablony obvykle vytvořit prostředky, které jsou v síti. Jedním ze jednoduchých způsobů, je použít objekt složité ukládat nastavení sítě a předejte propojené šablony.

Příklad komunikaci nastavení sítě vejde níže.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Prostředků vytvořené v propojených šablon jsou umístěny často sady dostupnosti. V následujícím příkladu zadat název sady dostupnost a také poruch domény a aktualizaci domény počet používat.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Pokud potřebujete více dostupnost sad (například jedno pro předlohy uzly) a jiné pro uzly dat, můžete použít název jako předponu, zadat více sad dostupnost nebo postupujte podle vzoru dříve pro vytváření proměnnou pro určitou velikost tričko s klipartem.

#### <a name="storagesettings"></a>storageSettings

Podrobnosti o úložiště jsou často nasdílel propojené šablony. V následujícím příkladu objekt *storageSettings* obsahuje podrobné informace o název účtu a kontejneru úložiště.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Propojené šablonami budete muset předejte nastavení operačního systému k různým typům uzly známý konfigurační různých typů. Komplexní objekt je snadný způsob, jak ukládat a sdílet informace o operačním systému a také usnadňuje podporuje více možností operační systém nasazení.

Následující příklad ukazuje objektu pro *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Proměnnou vygenerovaných *machineSettings* je složité objekt obsahující kombinaci základní proměnné pro vytváření virtuálního počítače. Proměnné zahrnout správce uživatelské jméno a heslo, předpony pro názvy OM a odkaz na obrázek operační systém.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Poznámka: Tento *osImageReference* načte hodnoty z proměnnou *osSettings* definované v šabloně hlavní. To znamená, že můžete snadno změnit operační systém virtuálního počítače – úplně nebo podle preferencí příjemce šablony.

#### <a name="vmscripts"></a>vmScripts

Objekt *vmScripts* obsahuje podrobnosti o skripty ke stažení a na instanci OM, včetně mimo a uvnitř odkazy provést. Mimo odkazy se týkají infrastrukturu. Vnitřní odkazy se týkají nainstalované softwaru nainstalovaného a konfigurace.

Použít vlastnost *scriptsToDownload* seznam skripty ke stažení bude v angličtině. Tento objekt obsahuje taky odkazy na argumenty příkazového řádku pro různé typy akcí. Tyto akce zahrnovat spouštění výchozí instalace pro každý jednotlivé uzel, instalace, které se spouští po nasazení všechny uzly a jakékoli další skripty, které mohou být specifické pro dané šablony.

V tomto příkladu je ze šablony slouží k nasazení MongoDB, která vyžaduje arbiter předvádění dostupnost. *ArbiterNodeInstallCommand* *vmScripts* k instalaci arbiter přidala.

V části proměnné najdete proměnných, které definují určitý text, který chcete spustit skript s velkými hodnotami.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Vrátí stavu ze šablony

Nejen můžete předat data do šablony, se taky dají sdílet data, volající šablonu. V části **výstupy** propojené šablony zajistíte klíč/dvojice, které můžete využívané šabloně zdroje.

Následující příklad ukazuje, jak předat soukromé IP adresu generovaného v šabloně propojené.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

V hlavním šablony můžete tato data pomocí následující syntaxe:

    "[reference('master-node').outputs.masterip.value]"

Tento výraz v části výstupy nebo části zdroje hlavní šablony můžete použít. Výraz nelze použít v části proměnných, protože vychází průběžný stav. Vrátit hodnotu z hlavního šablony, můžete:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Příklad použití výstupy část šablony propojené se vrátíte dat disků virtuálního počítače najdete v článku [Vytvoření více disků dat pro virtuální počítač](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Definování nastavení ověřování virtuálního počítače

Stejné vzorek vidíte dříve nastavení umožňuje určit nastavení ověřování virtuálního počítače. Vytvoření parametru pro předávání v poli Typ ověřování.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Přidání proměnné pro různými typy ověřování a proměnnou pro uložení typ, který slouží k této nasazení na základě hodnoty parametru.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Při definování virtuální počítač, nastavte **osProfile** proměnnou, kterou jste vytvořili.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Další kroky
- Další informace o v částech šablony, najdete v článku [Vytváření šablon správce prostředků Azure](resource-group-authoring-templates.md)
- Funkce, které jsou k dispozici v rámci šablon najdete v tématu [Funkce šablony správce prostředků Azure](resource-group-template-functions.md)

