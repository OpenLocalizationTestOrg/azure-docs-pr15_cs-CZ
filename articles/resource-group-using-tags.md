<properties
    pageTitle="Používání značek k uspořádání Azure zdroje | Microsoft Azure"
    description="Ukazuje, jak použít značky uspořádání prostředků pro fakturace a správu."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Používání značek k uspořádání Azure prostředků.

Správce prostředků umožňuje logicky uspořádat zdrojů pomocí značek. Značky tvořen klíč/dvojice identifikující zdroje s vlastnostmi, které definujete. Označit zdroje patří do stejné kategorie, platí pro tyto materiály stejnou značku.

Při zobrazení zdroje s určitou značkou, uvidíte zdroje ze všech skupin zdroje. Není omezena pouze prostředky ve stejné skupině zdroje, který umožňuje uspořádat zdroje způsobem, který je nezávislý relací nasazení. Značky může být užitečné, když potřebujete uspořádání prostředky pro fakturace nebo Správa.

Každý značky, které přidáte ke zdroji nebo skupina zdroje automaticky přidají i do taxonomie nevyžádaného předplatného. Můžete taky předem zadat taxonomii pro vaše předplatné s názvy značek a hodnoty, které chcete použít jako zdroje jsou příznakem v budoucnu.

Každý zdroj nebo skupina zdroje můžete mít maximálně 15 značky. Název značky se omezí na 512 znaků a hodnoty značky se omezí na 256 znaků.

> [AZURE.NOTE] Značky lze použít pouze na zdroje, které podporují operace správce prostředků. Pokud jste vytvořili virtuálního počítače, virtuální sítě nebo úložiště až do klasického nasazení modelu (například portálu klasické), nelze použít značky pro daný zdroj. K podpoře označování, přeinstalujte tyto materiály prostřednictvím Správce prostředků. Další zdroje domovské stránce podpory značky.

## <a name="templates"></a>Šablony

Označení zdroj během nasazení, jednoduše přidat element **značky** na zdroj, který nasazujete a zadejte název značky a hodnotu. Název značky a hodnota nemusí předem existují ve vašem předplatném. Pro jednotlivé zdroje můžete přidat až 15 značky.

Následující příklad ukazuje úložiště účtu se značkou.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Správce prostředků v současné době nepodporuje zpracování objektu pro názvy značek a hodnoty. Místo toho projít objektu pro značku hodnoty, ale pořád zadejte názvy značek, jak je vidět v následujícím příkladu.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portál

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>Prostředí PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>ROZHRANÍ REST API

Portál a prostředí PowerShell používají [Správce prostředků REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) na pozadí. Pokud potřebujete integrace označování do jiného prostředí, můžete dosáhnout značky pomocí získáte na číslo id zdroje a aktualizovat nastavení značky oprava hovoru.


## <a name="tags-and-billing"></a>Značky a fakturace

Podporované státní můžete pomocí značky fakturační data seskupit. Například [virtuálních počítačích integrovaný s správce prostředků Azure](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) umožňují definice a použití značek k uspořádání fakturační použití virtuálních počítačích. Pokud jsou spouštění více virtuálních pro jiné organizace, můžete podle nákladové středisko značky k použití skupiny.  
Můžete také značky na zařadit do kategorií náklady prostředím runtime; například fakturační použití VMs spuštěné v pracovním prostředí.

Můžete získat informace o značky [Používání zdrojů Azure a rozhraní API RateCard](billing-usage-rate-card-overview.md) nebo souboru použití hodnoty oddělené čárkami (CSV). Použití soubor stáhnout z [portálu Azure účty](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com). Další informace o programový přístup k fakturačních údajů najdete v článku [získání podstatu využití prostředků Microsoft Azure](billing-usage-rate-card-overview.md). Operace rozhraní REST API najdete v článku Principy [Azure fakturace REST API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Stažení použití CSV služby, které podporují značky s fakturací značky zobrazí ve sloupci **značky** . Další informace najdete v tématu [Vysvětlení informací na faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).

![Zobrazit značky v fakturace](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Další kroky

- Omezení a konvence můžete použít napříč předplatného s vlastní zásady. Zásad, které definujete může vyžadovat, aby všechny zdroje obsahovat hodnotu pro určité značky. Další informace najdete v tématu [Použití zásady pro přidávání a používání zdrojů a řízení přístupu](resource-manager-policy.md).
- Úvod do problematiky pomocí prostředí PowerShell Azure při nasazení materiály najdete v článku [použití Azure pomocí Správce prostředků Azure](./powershell-azure-resource-manager.md).
- Úvod do používání rozhraní příkazového řádku Azure při nasazení prostředků najdete v článku [použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows s Azure řízení zdrojů](./xplat-cli-azure-resource-manager.md).
- Úvod do na portálu najdete v článku [Azure portálu pro správu Azure prostředků.](./azure-portal/resource-group-portal.md)  
