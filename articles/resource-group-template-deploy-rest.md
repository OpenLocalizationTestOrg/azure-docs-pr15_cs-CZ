<properties
   pageTitle="Nasazení zdroje s šablona a rozhraní REST API | Microsoft Azure"
   description="Nasazení zdroje Azure pomocí Správce prostředků Azure a rozhraní REST API Správce prostředků. Zdroje jsou definované v šabloně správce prostředků."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Nasazení zdroje se šablonami správce a správce prostředků REST API

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-template-deploy.md)
- [Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [ROZHRANÍ REST API](resource-group-template-deploy-rest.md)

Tento článek vysvětluje, jak nasadit svých prostředcích Azure pomocí rozhraní REST API Správce prostředků šablonami správce prostředků.  

> [AZURE.TIP] Pokud potřebujete pomoc s ladění chybu při nasazení najdete v článku:
>
> - [Zobrazit nasazení operace pomocí rozhraní REST API](resource-manager-troubleshoot-deployments-rest.md) Další informace o získání informací o, který vám pomůže Poradce při potížích s vaší chyby
> - [Poradce při běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md) se dozvíte, jak vyřešit běžné chyby při zavedení

Šablony můžou být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátor URI. Pokud vaše šablona nachází v účtu úložiště, můžete omezit přístup k šabloně a zadat sdílené přístupový token podpisu (přidružení zabezpečení) během nasazení.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Nasazení pomocí rozhraní REST API
1. Nastavit [běžné parametry a záhlaví](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), včetně tokeny ověřování.
2. Pokud nemáte existující skupiny zdrojů, vytvořte skupinu zdrojů. Zadejte svoje id předplatného, název nové skupiny prostředků a umístění, které potřebujete pro řešení. Další informace najdete v tématu [Vytvoření skupiny zdrojů](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Ověření nasazení před spuštěním spuštěním operaci [Ověřit nasazení šablony](https://msdn.microsoft.com/library/azure/dn790547.aspx) . Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení (viz v dalším kroku).

3. Vytvoření nasazení. Zadejte svoje id předplatného, na název skupiny prostředků pro nasazení, nasazení a odkaz na vaší šabloně. Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file). Další informace o rozhraní REST API zdroje skupinu najdete v článku [Vytvoření nasazení šablony](https://msdn.microsoft.com/library/azure/dn790564.aspx). Všimněte si, že je nastaven **režimu** **Přírůstková**. Dokončení nasazení spustíte nastavení **režimu** **Dokončeno**. Buďte opatrní při použití úplné režimu jako můžete neúmyslně odstraníte prostředky, které nejsou v šabloně.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Pokud chcete zaznamenat obsah odpověď, žádost o obsahu nebo obojí, připojte **debugSetting** žádosti o.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Můžete nastavit váš účet úložiště používat sdílené přístupový token podpisu (přidružení zabezpečení). Další informace najdete v tématu [Delegování přístupu s sdílených přístup podpisem](https://msdn.microsoft.com/library/ee395415.aspx).

4. Pokud potřebujete stav nasazení šablony. Další informace najdete v tématu [získání informací o nasazení šablony](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Další kroky
- Příklad nasazení prostředků přes klienta knihovnu .NET najdete v článku [nasazení zdrojů pomocí knihoven .NET a šablony](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definice parametrů v šabloně, najdete v článku [vytváření šablon](resource-group-authoring-templates.md#parameters).
- Nasazení řešení různých prostředích, najdete v článku [test prostředí v Microsoft Azure a vývoj](solution-dev-test-environments.md).
- Podrobné informace o použití odkazu na KeyVault k předání zabezpečené hodnot najdete v článku [předání zabezpečené hodnot během nasazení](resource-manager-keyvault-parameter.md).
