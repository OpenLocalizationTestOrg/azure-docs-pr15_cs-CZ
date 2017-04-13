<properties
   pageTitle="Správce prostředků šablony pro ukládání | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení úložiště účty pomocí šablony."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Schéma šablony účtu úložiště

Vytvoří účet úložiště.

## <a name="schema-format"></a>Formát schématu

Vytvoření účtu úložiště, přidejte následující schéma oddílu zdroje šablony.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Hodnota |
| ---- | ---- |
| Typ | Výčet<br />Povinné<br />**Microsoft.Storage/storageAccounts**<br /><br />Typ zdroje k vytvoření. |
| apiVersion | Výčet<br />Povinné<br />**2015 06 15** nebo **2015 05 01 náhledu**<br /><br />Verze rozhraní API pro použití při vytváření zdroje. | 
| Jméno | Řetězec<br />Povinné<br />Mezi 3 a 24 znaky, jenom čísla a malými písmeny.<br /><br />Název účtu úložiště chcete vytvořit. Ve všech Azure musí být jedinečný název. Zvažte použití funkce [uniqueString](resource-group-template-functions.md#uniquestring) s zásady vytváření názvů, jak je vidět v následujícím příkladu. |
| umístění | Řetězec<br />Povinné<br />Oblast, která podporuje úložiště účty. K určení platné oblastí, najdete v článku [podporované oblastí](resource-manager-supported-services.md#supported-regions).<br /><br />Oblast hostovat účtu úložiště. |
| Vlastnosti | Objekt<br />Povinné<br />[vlastnosti objektu](#properties)<br /><br />Objekt, který určuje typ úložiště účet, který chcete vytvořit. |

<a id="properties" />
### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Hodnota |
| ---- | ---- | 
| Typ účtu | Řetězec<br />Povinné<br />**Standard_LRS** **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**nebo **Premium_LRS**<br /><br />Typ účtu úložiště. Povolené hodnoty odpovídají standardní místně nadbytečné, standardní zóny nadbytečné, standardní Geo přebytečné, standardní Geo-přebytečné přístup pro čtení a Premium místně nadbytečné. Informace o těchto typů účtů najdete v tématu [úložišti Azure replikace](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Příklady

Následující příklad nasadí standardní místně nadbytečné účtu úložiště s jedinečný název podle id skupiny prostředků.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Rychlý úvod šablony

Existuje mnoho rychlý úvod šablony, které obsahují účet úložiště. Následující šablony ilustrují některé běžné scénáře:

- [Vytvoření účtu standardní úložiště](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Jednoduché nasazení systému Windows OM](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Jednoduché nasazení Linux OM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Vytvoření profilu CDN, CDN koncový bod pod svým účtem úložiště počátek](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Vytvoření vysoké farmě Sharepointu Availabilty s 9 VMs pomocí prostředí Powershell DSC rozšíření](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Jednoduché nasazení 5 uzel zabezpečené služby struktury obrázku s WAD povolena](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Vytvoření virtuálního počítače z Windows obrázku s 4 disků prázdné dat](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Další kroky

- Obecné informace o databázích najdete v tématu [Úvod k úložišti tabulek Microsoft Azure](./storage/storage-introduction.md).
- Například šablony, které používají nový účet úložiště s virtuálního počítače, najdete v článku [nasazení jednoduchou Linux OM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) nebo [nasadit jednoduché OM Windows](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
