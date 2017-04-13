<properties
   pageTitle="Přehled zdrojů poskytovatele sítě | Microsoft Azure"
   description="Další informace o nové zprostředkovatele v Azure správce prostředků sítě"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Zprostředkovatele zdroje
Podpora potřeba v dnešním obchodní úspěch je možnost vytvářet a spravovat rozsáhlé sítě aplikace aktivní, flexibilní, zabezpečené a opakující způsobem. Správce Azure zdroje (ARM) umožňuje vytvářet tyto aplikace jako jediná kolekce zdrojů v skupiny zdrojů. Tyto materiály se spravuje přes různé poskytovatelů zdroje v části ARM.

Azure správce prostředků závisí na jiný zdroj poskytovatelů k poskytnutí přístupu k prostředkům. Existují tři hlavní zdroje poskytovatelů: sítě, ukládání a výpočetním. Tento dokument popisuje vlastnosti a výhody zprostředkovatele zdrojů, včetně:

- **Metadata** – můžete přidat informace k prostředkům pomocí značek. Tyto značky lze použít ke sledování využití prostředků různých skupin zdrojů a předplatná.
- **Větší kontrolu nad sítě** – síť, kterou zdroje jsou volně svázáno a můžete řídit je podrobnějších způsobem. To znamená, že máte větší flexibilitu při správě sítě zdroje.
- **Rychlejší konfigurace** – protože síťových prostředků jsou volně svázáno, můžete vytvořit a organizovat síťové prostředky souběžně. To významně má sníženou konfigurace čas.
- **Řízení přístupu na základě rolí** - RBAC obsahuje výchozí role s konkrétním rozsahem zabezpečení, kromě povolení vytváření vlastních rolí správy zabezpečení.
- **Snadnější správu a nasazení** – je snadněji nasazovat a spravovat aplikace od můžete vytvořením balíčku celou aplikaci jako jediná kolekce zdrojů v skupina zdroje. A rychlejší nasadit, protože nasazením jednoduše zadáním datové JSON šablony.
- **Rychlé přizpůsobení** - deklarativní šablony můžete použít k povolení opakující a rychlé úpravy nasazení.
- **Opakující přizpůsobení** - deklarativní šablony můžete použít k povolení opakující a rychlé úpravy nasazení.
- **Rozhraní pro správu** – můžete používáte některou z následujících rozhraní ke správě zdrojů:
    - Základě REST API
    - Prostředí PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure rozhraní příkazového řádku
    - Náhled portálu
    - Jazyk šablony ARM

## <a name="network-resources"></a>Síťové prostředky
Síť zdroje můžete spravovat teď nezávisle na sobě, takže není nutné je spravováno prostřednictvím jedné výpočetní zdroj (virtuální počítač). Díky vyšší stupeň flexibilitu a flexibilitu v psaní infrastrukturu složité a velkých měřítko ve skupině zdroje.

Koncepční zobrazení aplikace ukázkové nasazení týkající se aplikace vícevrstevného je uveden níže. Jednotlivé zdroje, které se zobrazí, například nic, veřejnou IP adresy a VMs, dá se ovládat nezávisle na sobě.

![Model prostředků sítě](./media/resource-groups-networking/Figure2.png)

Každý zdroj obsahuje jednu sadu vlastností a nastavení jednotlivých vlastností. Zobrazení společných vlastností jsou:

|Vlastnost|Popis|Ukázkové hodnoty|
|---|---|---|
|**Jméno**|Název jedinečný zdroje. Každý typ zdroje obsahuje vlastní naming omezení.|NIC01 PIP01, VM01,|
|**umístění**|Azure oblast, ve kterém je umístěn zdroje|westus eastus|
|**ID**|Jedinečný identifikátor URI na základě identifikační|/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Můžete zkontrolovat, že jednotlivých vlastností zdroje v následujících částech.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Rozhraní pro správu
Můžete spravovat Azure sítě zdrojů pomocí různých rozhraní. V tomto dokumentu jsme se zaměřuje na tažení těchto rozhraní: rozhraní REST API a šablony.

### <a name="rest-api"></a>ROZHRANÍ REST API
Jak jsme zmínili dříve, síťových prostředků můžete spravuje přes celou řadu rozhraní, včetně rozhraní REST API, .NET SDK, Node.JS SDK, Java SDK, Powershellu, rozhraní příkazového řádku, Azure portál a šablony.

Rozhraní Rest API odpovídat specifikace protokolu HTTP 1.1. Obecné URI strukturu rozhraní API je uveden níže:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

A parametrech do složených závorek představují následující prvky:

- **id předplatného** – id Azure předplatného.
- **obor názvů zdrojů zprostředkovatele** – obor názvů pro použitého zprostředkovatele. Zprostředkovatele zdroje hodnotu *Microsoft.Network*.
- **název oblasti** - název Azure oblasti

Při volání rozhraní REST API jsou podporovány následující metody HTTP:

- **Umístění** – slouží k vytvoření prostředku daného typu vlastnost zdroj nebo změna přidružení zdroje.
- **GET** - použitý k načtení informací zřizování zdroje.
- **Odstranění** – slouží k odstranění existující zdroj.

Žádostí a odpovědí v souladu s datové části formátu JSON. Další informace najdete v tématu [API správy zdrojů Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>Jazyk šablony ARM
Kromě řízení zdrojů imperativně (prostřednictvím rozhraní API nebo SDK), můžete také deklarativní stylu programování vytvářet a spravovat zdroje sítě pomocí jazyka šablony ARM.

Znázornění ukázkové šablony uvedeny níže:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Šablona je primárně JSON popis zdroje a hodnoty instancí vložené prostřednictvím parametry. Na následujícím obrázku můžete použít k vytvoření virtuální sítě s 2 podsítí.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Máte možnost ručně zadat hodnoty parametrů při použití šablony nebo můžete použít parametr soubor. Možné množiny hodnoty parametrů na použít přes šablonu výše ukazuje tento příklad:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Hlavních výhod používání šablon jsou:

- Je možné vytvářet složité infrastruktury ve skupině zdrojů ve stylu deklarativní. Průběhu vytváření zdrojů, včetně správy závislost uskutečněných jednotlivými ARM.
- Infrastruktura můžete vytvořit opakující způsobem přes různé regiony a v oblasti jednoduše změnit parametry.
- Deklarativní styl vede k kratší předstihu v vytváření šablon a zavádění infrastrukturu.

Ukázky šablon najdete v článku [Azure rychlý úvod šablony](https://github.com/Azure/azure-quickstart-templates).

Další informace o jazyk šablony ARM najdete v článku [Jazykové šablony Azure správce prostředků](../resource-group-authoring-templates.md).

Výše uvedené ukázkové šablony používá virtuální sítě a podsítě zdroje. Existují další zdroje sítě, které můžete použít podle těchto pokynů:

### <a name="using-a-template"></a>Použít šablonu?

Můžete nasadit služby Azure ze šablony pomocí prostředí PowerShell AzureCLI, nebo pomocí technologie Klikni a nasazení z GitHub. Abyste mohli nasadit služby ze šablony v GitHub, proveďte následující kroky:

1. Otevřete soubor template3 z GitHub. Jako příklad otevřete [virtuální sítě s dvěma podsítí](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klikněte na **Deploy Azure**a pak se přihlaste k portálu Azure pomocí svých přihlašovacích údajů.
3. Ověření šablonu a potom klikněte na **Uložit**.
4. Klikněte na **Upravit parametry** a vyberte umístění, například *Západní USA*, vnet a podsítí.
5. V případě potřeby změňte parametry **ADDRESSPREFIX** a **SUBNETPREFIX** a potom klikněte na **OK**.
6. Klikněte na **Výběr skupina zdroje** a potom klikněte na skupina zdroje, chcete-li přidat vnet a podsítí. Případně můžete vytvořit nové skupiny prostředků po kliknutí na **nebo vytvořit nový**.
3. Klikněte na **vytvořit**. Všimněte si dlaždici zobrazení **zřizování šablony nasazení**. Po dokončení nasazení, zobrazí se na obrazovku podobné dole.

![Ukázková šablona nasazení](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Další kroky

[Azure jazyk šablony správce prostředků](../resource-group-authoring-templates.md)

[Azure sítě – běžně používaných šablony](https://github.com/Azure/azure-quickstart-templates)

[Výpočet zprostředkovatele prostředků](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure Přehled Správce zdrojů](../azure-resource-manager/resource-group-overview.md)
