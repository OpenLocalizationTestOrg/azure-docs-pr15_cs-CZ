<properties 
    pageTitle="Propojení zdroje v správce prostředků Azure | Microsoft Azure" 
    description="Vytvořte propojení mezi související materiály do jiné skupiny prostředků v Azure správce." 
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
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Propojení zdroje v správce prostředků Azure

Při nasazení můžete označit zdroje jako závisí na jiné zdroje, ale tento životního cyklu končí nasazení. Po nasazení je určen vztah mezi závislé prostředky. Správce prostředků poskytuje funkci zdroje propojení vytvořit trvalý vztahy mezi zdroji.

S propojením zdrojů, můžete dokument relace, které zahrnují skupiny zdrojů. Je třeba běžné mít databáze pomocí samostatném životního cyklu uložena v jedné skupině zdrojů a aplikace s jinou životního cyklu uložena v jiné skupině zdrojů. Aplikace se připojí k databázi, kterou chcete označit propojení mezi aplikace a databáze. 

Na stejném předplatném musí patřit všech propojených zdrojů. Jednotlivé zdroje můžou být propojené s 50 další zdroje informací. Je možné jenom dotazu související materiály je prostřednictvím rozhraní REST API. Pokud některý z propojených zdrojů jsou odstranili nebo přesunuli, musí vlastník odkaz vyčistit zbývající odkaz. **Není** upozornění při odstranění zdroj, který je propojený na jiné zdroje.

## <a name="linking-in-templates"></a>Propojení v šablony

Definovat propojení v šabloně, patří typ zdroje, který kombinuje poskytovatele názvů zdrojů a typ zdroje zdroje s **/providers/links**. Název musí obsahovat název zdroje zdroje. Zadáte pole číslo id zdroje cílové zdroje. Následující příklad vytvoří vazbu mezi webu a účet úložiště.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Úplný popis formátu šablony najdete v článku [zdroje odkazy – schéma šablony](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Propojení s rozhraní REST API

Definovat propojení mezi nasazeném zdrojů, spusťte:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Nahraďte {id předplatného} id předplatného. Nahrazení {skupina zdroje}, {obor názvů zprostředkovatele, {typ zdroje} a {– název zdroje} s hodnotami, které identifikují první zdroj v odkazu. {Název odkazu} nahraďte názvem odkaz na vytvořit. Použití 2015-01-01 pro rozhraní api verzi.

V pozvánce na schůzku patří objekt, který definuje druhý zdroje v poli odkaz:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Element vlastnosti obsahuje identifikátor druhý zdroje.

Dotaz odkazy v předplatné:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Další příklady, jak získat informace o propojení, včetně najdete v článku [Propojených zdrojů](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Další kroky

- Můžete také uspořádat zdroje s klíčovými slovy. Další informace o označování materiály, najdete v článku [použití značek k uspořádání zdroje](resource-group-using-tags.md).
- Popis toho, jak vytvořit šablonu a definujte zdroje, které mají být nasazené najdete v tématu [vytváření šablony](resource-group-authoring-templates.md).
