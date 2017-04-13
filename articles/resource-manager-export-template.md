<properties
    pageTitle="Export správce prostředků Azure šablony | Microsoft Azure"
    description="Umožňuje spravovat zdroje Azure export šablony z existující skupiny zdrojů."
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
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Export šablony pro správce prostředků Azure z existujících zdrojů

Správce prostředků umožňuje export Správce prostředků šablony z existujících zdrojů ve vašem předplatném. Další informace o syntaxi šablony nebo k automatizaci nového nasazení řešení v případě potřeby můžete vygenerovaných šablony.

Je důležité mít na paměti, že dvěma různými způsoby Export šablony:

- Můžete exportovat aktuální šablona, kterou jste použili pro nasazení. Exportovaná šablona obsahuje všechny parametry a proměnné přesně tak, jak se zobrazuje v původní šabloně. Tento přístup je užitečné, když nasadili zdrojů prostřednictvím portálu. Teď budete chtít najdete v článku jak vytvářet šablonu, kterou chcete vytvořit prostředky.
- Můžete exportovat šablonu, která představuje aktuální stav skupina zdroje. Exportovaná šablona není založený na libovolnou šablonu, kterou jste použili pro nasazení. Místo toho vytvoří šablonu, která je snímek skupina zdroje. Exportovaného šablona obsahuje mnoho hodnot pevně a pravděpodobně tolik parametrů obvykle definujete. Tento přístup je užitečná, když jste změnili skupina zdroje prostřednictvím portálu nebo skriptů. Teď budete muset zachytit skupina zdroje jako šablonu.

Toto téma ukazuje obou přístupů.

V tomto kurzu se přihlásit k portálu Azure vytvořit účet úložiště a export šablonu vhodnou pro tento účet úložiště. Přidání virtuální sítě pro úpravy skupina zdroje. Nakonec exportu novou šablonu, která představuje jeho aktuálním stavu. I když tento článek se zaměřuje na infrastrukturu zjednodušené, můžete použít stejný postup Export šablony pro složitější řešení.

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

1. V [Azure portál](https://portal.azure.com), vyberte **Nový** > **úložiště** > **účtu úložiště**.

      ![Vytvoření úložiště](./media/resource-manager-export-template/create-storage.png)

2. Vytvoření účtu úložiště s názvem **úložiště**, iniciály a datem. Název účtu úložiště musí být jedinečný přes Azure. Pokud název je už se používá, se zobrazí chybová zpráva oznamující, že název se používá. Zkuste některou variantu. Pole Skupina zdroje vytvoření nové skupiny prostředků a pojmenujte ho **ExportGroup**. Použití výchozích hodnot pro jiné vlastnosti. Vyberte možnost **vytvořit**.

      ![zadání hodnoty pro úložiště](./media/resource-manager-export-template/provide-storage-values.png)

Nasazení může trvat několik minut. Po dokončení nasazení předplatného obsahuje účet úložiště.

## <a name="view-a-template-from-deployment-history"></a>Zobrazit šablonu z historie nasazení

1. Přejděte na zásuvné skupina zdroje pro nové skupiny prostředků. Všimněte si, že zásuvné zobrazí výsledek poslední nasazení. Vyberte tento odkaz.

      ![zásuvné skupina zdroje](./media/resource-manager-export-template/resource-group-blade.png)

2. Zobrazit historii nasazení pro skupiny. V případě seznamy zásuvné pravděpodobně jedinou nasazení. Vyberte toto nasazení.

     ![poslední nasazení](./media/resource-manager-export-template/last-deployment.png)

3. Zásuvné zobrazuje přehled nasazení. Souhrn obsahuje stav nasazení a jeho operace a hodnoty, které jste uvedli pro parametry. Šablona, kterou jste použili pro nasazení zobrazíte vyberte **šablonu zobrazení**.

     ![Zobrazit nasazení souhrn](./media/resource-manager-export-template/deployment-summary.png)

4. Správce prostředků načte následující šest soubory můžete:

   1. **Šablona** – šablonu, která definuje infrastrukturu pro řešení. Při vytvoření účtu úložiště až na portálu správce prostředků použít šablonu k nasazení a tuto šablonu pro pozdější potřeby uložili.
   2. **Parametry** - parametr soubor, který můžete použít pro předávání hodnot během nasazení. Obsahuje hodnoty, které jste zadali při prvním nasazení, ale můžete změníte některá z těchto hodnot při nasazením šabloně.
   3. **Rozhraní příkazového řádku** - Azure příkazovém řádku rozhraní (rozhraní příkazového řádku) skript, který slouží k nasazení šablony.
   4. **Prostředí PowerShell** - prostředí PowerShell Azure skript, který slouží k nasazení šablony.
   5. **.NET** - předmětu A .NET využívající zavést šablonu.
   6. **Skutečné** - předmětu A skutečné využívající zavést šablonu.

     Soubory, které jsou k dispozici prostřednictvím odkazy přes zásuvné. Ve výchozím nastavení zobrazuje zásuvné šablonu.

       ![Šablona zobrazení](./media/resource-manager-export-template/view-template.png)

     Pojďme věnovat zvláštní pozornost k šabloně. Šablony by měla vypadat podobně jako:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametry": {"název": {"typ": "Řetězec"}, "časová": {"typ": "Řetězec"}, "umístění": {"typ": "Řetězec"}, "encryptionEnabled": {"Výchozí": false, "typ": "Logická hodnota"}}, "zdroje": [{"typ": "Microsoft.Storage/storageAccounts", "sku": {"název": "[parameters('accountType')]"}, "druhu": "Úložiště", "název": "[parameters('name')]", "apiVersion": "2016-01-01", "umístění": "[parameters('location')]", "vlastnosti": {"šifrování": {"služby": {"objektů blob": {"povolené": "[parameters('encryptionEnabled')]"}}, "keySource": "Microsoft.Storage"}}}]}
 
Toto je skutečná Šablona použitá k vytvoření účtu úložiště. Všimněte si, že obsahuje parametrů, které vám umožní nasazení různých typů účtů úložiště. Další informace o struktuře šablony, najdete v článku [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md). Úplný seznam funkcí, které můžete použít v šabloně najdete v článku [funkce šablony správce prostředků Azure](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Přidání virtuální sítě

Šablony, který jste stáhli v předchozí části představující infrastrukturu pro tento původní nasazení. Však nebude účtu pro všechny změny provedené po nasazení.
Ke znázornění tento problém, upravit Pojďme skupina zdroje přidáním virtuální sítě prostřednictvím portálu.

1. Ve skupině zásuvné zdrojů klikněte na **Přidat**.

      ![Přidání zdrojů](./media/resource-manager-export-template/add-resource.png)

2. Vyberte **virtuální sítě** dostupné zdroje.

      ![Vyberte virtuální sítě](./media/resource-manager-export-template/select-vnet.png)

2. Pojmenujte virtuální sítě **VNET**a použít výchozí hodnoty pro jiné vlastnosti. Vyberte možnost **vytvořit**.

      ![Nastavit upozornění](./media/resource-manager-export-template/create-vnet.png)

3. Po virtuální sítě byl úspěšně používaný skupina zdroje, podívejte se znovu historie nasazení. Nyní zobrazí dva nasazení. Pokud nevidíte druhý nasazení, budete muset zavřít vaší zásuvné skupina zdroje a znovu ho otevřete. Vyberte novější nasazení.

      ![Historie nasazení](./media/resource-manager-export-template/deployment-history.png)

4. Zobrazení šablonu vhodnou pro tento nasazení. Všimněte si, že obsahuje pouze virtuální sítě. Neobsahuje úložiště účet, který jste nainstalovali dříve. Už máte šablonu, která představuje všechny zdroje skupina zdroje.

## <a name="export-the-template-from-resource-group"></a>Export šablony z pole Skupina zdroje

Aktuální stav skupina zdroje získáte vyexportujte šablonu, která zobrazuje snímek skupina zdroje.  

> [AZURE.NOTE] Nelze exportovat šablony pro skupinu zdroje, která obsahuje více než 200 zdroje.

1. Šablony pro skupinu prostředků zobrazíte vyberte **skript automatizace**.

      ![Export pole Skupina zdroje](./media/resource-manager-export-template/export-resource-group.png)

     Ne všechny typy zdrojů podporují funkce šablony exportovat. Pokud vaše skupina zdroje obsahuje pouze úložiště klienta a virtuální sítě uvedené v tomto článku, není chyba se zobrazí. Pokud jste vytvořili jiné typy zdrojů, ale může zobrazit chybová zpráva s informacemi, že došlo k potížím s exportovat. Se naučíte, jak řešit problémy v části [Exportovat opravit problémy](#fix-export-issues) .

      

2. Znovu zobrazit šest soubory, které vám pomohou přeinstalujte řešení, ale tentokrát šablona je trochu jiný. Tato šablona má jenom dvěma parametry: jeden pro název účtu úložiště a jeden pro název virtuální sítě.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Správce prostředků není načtení šablony, které jste použili při nasazení. Namísto toho vytvořit novou šablonu, která je na základě aktuální konfigurace zdrojů. Příklad šablony Nastaví úložiště účtu umístění a replikace hodnotu:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Máte několik možností pro zachování pro práci s touto šablonou. Můžete stáhnout šablonu a pracovat na něm místně pomocí editoru JSON. Nebo můžete šablonu uložit do knihovny a na něm pracovaly prostřednictvím portálu.

     Pokud se vám pomocí editoru JSON jako [A kód](resource-manager-vs-code.md) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)můžete chtít stažení šablony místně a že editoru. Pokud nejsou nastavíte pomocí editoru JSON, můžete chtít šablonu prostřednictvím portálu pro úpravy. Zbývající část tohoto tématu předpokládá, že jste si uložili na šablonu do knihovny na portálu. Však změníte stejnou syntaxi do šablony zda místně práce pomocí editoru JSON nebo prostřednictvím portálu.

     Místně pracovat, zaškrtněte políčko **stahovat**.

      ![Stažení šablony](./media/resource-manager-export-template/download-template.png)

     S přípravou portálu, vyberte **Přidat do knihovny**.

      ![Přidat do knihovny](./media/resource-manager-export-template/add-to-library.png)

     Při přidání šablony do knihovny, zadejte název a popis šablony. Potom klikněte na **Uložit**.

     ![nastavení šablony hodnot](./media/resource-manager-export-template/set-template-values.png)

4. Pokud chcete zobrazit šablony uložené v knihovně, vyberte **Další služby**, zadejte **šablony** k filtrování výsledků, vyberte **šablony**.

      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)

5. Vyberte šablonu s názvem, který jste uložili.

      ![Vyberte šablonu](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Přizpůsobení šablony

Exportovaná šablony funguje Pokud chcete vytvořit stejný účet úložiště a virtuální sítě pro každý nasazení. Správce prostředků však nabízí možnosti tak, aby nasazení šablon mnohem větší flexibilitu. Třeba při nasazení, můžete určit typ úložiště účet, který chcete vytvořit nebo hodnot pro účely předpony adresy virtuální sítě a podsítě předponu.

V této části můžete přidat parametry exportovaného šablony tak, že při nasazení tyto materiály k dalším prostředím můžete znovu použít šablonu. Některé funkce se také přidat do šablony snížit pravděpodobnost, že dochází k chybě při nasazení šablony. Už máte uhodnout jedinečný název účtu úložiště. Místo toho šabloně vytvoří jedinečný název. Omezení hodnoty, které lze zadat typ účtu úložiště na pouze platné možnosti.

1. Vyberte **Upravit** šablonu přizpůsobit.

     ![zobrazení šablon](./media/resource-manager-export-template/show-template.png)

1. Vyberte šablonu.

     ![Úprava šablony](./media/resource-manager-export-template/edit-template.png)

1. Abyste mohli pro předávání hodnot, které chcete zadat během nasazení, nahraďte oddílu **Parametry** nové definice parametrů. Všimněte si hodnoty **allowedValues** **storageAccount_accountType**. Pokud omylem zadáte neplatnou hodnotu, že chybové považována před spuštěním nasazení. Všimněte si také, že zadání pouze předpony pro název účtu úložiště a předponu se omezí na 11 znaků. Když omezíte předponu tak, aby 11 znaků, zajistíte úplný název nepřekročí maximální počet znaků pro účet úložiště. Tuto předponu umožňuje používat stejný systém názvů na úložiště účty. Uvidíte, jak vytvořit jedinečný název v dalším kroku.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Části **proměnné** šablony je prázdné. V části **proměnné** vytvoříte hodnoty, které usnadňují syntaxi pro ostatní šablony. Nahraďte zde novou definici proměnných. Proměnná **storageAccount_name** spojí předpony parametr jedinečný řetězec vytvořený na základě identifikátoru skupina zdroje. Už máte uhodnout jedinečný název při poskytování hodnotu parametru.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Použití parametrů a proměnná v definice zdroje, nahraďte části **zdroje** nové definice zdroje. Všimněte si, že něco změnilo v definice zdroje než hodnota, která je přiřazen k vlastnost Zdroj. Vlastnosti jsou stejná jako vlastnosti z exportovaného šablony. Jednoduše přiřazujete vlastnosti hodnoty parametrů místo pevně hodnot. Umístění zdroje je nastavena pro použití na stejném místě skupina zdroje prostřednictvím výrazu **.location resourceGroup ()** . Proměnné, kterou jste vytvořili pro název účtu úložiště odkazuje prostřednictvím výrazu **proměnné** .

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Až budete hotovi, vyberte **OK** šablonu pro úpravy.

5. Zaškrtněte políčko **Uložit** změny uložte do šablony.

     ![uložení šablony](./media/resource-manager-export-template/save-template.png)

6. Abyste mohli nasadit aktualizované šablony, vyberte **instalovat**.

     ![nasazení šablony](./media/resource-manager-export-template/deploy-template.png)

7. Zadejte hodnoty parametrů a vyberte nové skupiny prostředků pro nasazení zdroje, které vám.

## <a name="update-the-downloaded-parameters-file"></a>Aktualizace souboru stažený parametrů

Pokud pracujete s stažené soubory (spíše než portálu knihovny), musíte aktualizovat soubor stažený parametrů. Odpovídá už parametrů v šabloně. Není nutné použít parametr soubor, ale když přeinstalujte prostředí ho můžete zjednodušit proces. Můžete použít výchozí hodnoty, které jsou definované v šabloně pro mnoho parametry tak, že soubor parametr jenom dvě hodnoty.

Nahrazení obsahu parameters.json soubor:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Aktualizované parametrem soubor obsahuje hodnoty pouze pro parametrů, které nemají výchozí hodnotu. Můžete zadat hodnoty pro ostatní parametry chcete hodnota, která se liší od výchozí hodnota.

## <a name="fix-export-issues"></a>Vyřešit problémy s exportem

Ne všechny typy zdrojů podporují funkce šablony exportovat. Správce prostředků konkrétně není exportovat některé typy prostředků nechcete, aby zobrazovaly citlivá data. Například pokud máte připojovacího řetězce v konfiguraci vašeho webu, pravděpodobně nechcete explicitně zobrazené v šabloně aplikace exportovaného. Ruční přidáním chybějící zdroje zpět do šablony můžete získat řešením tohoto problému.

> [AZURE.NOTE] Při pouze používání narazíte na problémy s exportem při exportu ze skupiny zdrojů a ne z vaší historie nasazení. Pokud poslední nasazení přesně představuje aktuální stav skupiny zdrojů, je třeba exportovat šablony z historie nasazení a ne ze skupiny zdrojů. Exportujte pouze ze skupiny zdrojů po provedení změn ke skupině zdroje, které nejsou podle jednoho šablony.

Třeba při exportu šablony pro skupinu zdroje, která obsahuje webové aplikace, databáze SQL a připojovací řetězec do konfigurace webů, zobrazí se následující zpráva:

![Zobrazit chyby](./media/resource-manager-export-template/show-error.png)

Výběrem zprávy zobrazí přesně nebyly exportovány typy zdrojů. 
     
![Zobrazit chyby](./media/resource-manager-export-template/show-error-details.png)

Toto téma ukazuje běžné opravy.

### <a name="connection-string"></a>Připojovací řetězec

Přidání definici připojovací řetězec do databáze v zdroje weby:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Rozšíření webu

Ve zdroji webu přidejte definici kód, který chcete nainstalovat:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Rozšíření virtuálního počítače

Příklady rozšíření virtuálního počítače najdete v článku [Ukázky konfigurace rozšíření OM Windows Azure](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Virtuální sítě brány

Přidání typu virtuální sítě brány zdroje.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Místní síti brány

Přidání typu zdroje brány místní síti.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Připojení

Přidání typu připojení zdroje.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Další kroky

Blahopřejeme! Jste se naučili způsob exportu šablony ze zdroje, které jste vytvořili v portálu.

- Šablony pomocí [prostředí PowerShell](resource-group-template-deploy.md), [Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md)a [Rozhraní REST API](resource-group-template-deploy-rest.md)nástroje můžete nasazovat.
- Jak se dají exportovat šablony prostřednictvím Powershellu najdete v tématu [Pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure](powershell-azure-resource-manager.md).
- Jak se dají exportovat šablony prostřednictvím rozhraní příkazového řádku Azure naleznete v tématu [použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows s správce prostředků Azure](xplat-cli-azure-resource-manager.md).
