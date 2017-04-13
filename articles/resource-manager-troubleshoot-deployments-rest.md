<properties
   pageTitle="Zobrazit operace nasazení pomocí rozhraní REST API | Microsoft Azure"
   description="Popisuje, jak používat Azure správce prostředků REST API zjistěte, jestli nejsou problémy s z nasazení Správce prostředků."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Zobrazit nasazení operace s Azure správce prostředků REST API

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [Prostředí PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md)
- [ROZHRANÍ REST API](resource-manager-troubleshoot-deployments-rest.md)

Pokud jste dostali k chybě při nasazení zdroje Azure, můžete zobrazit další podrobné informace o nasazení operacích, které byly provedeny. Rozhraní REST API obsahuje operacích, které umožňují najít chyby a určit potenciální opravy.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Ověřením šablony a infrastrukturu před nasazením se můžete vyhnout chybám. Můžete taky protokolu další žádosti a odpověď informací při nasazení, které mohou být užitečné později pro řešení potíží. Informace o ověřování a informací o protokolování žádostí a odpovědí, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Poradce při potížích s pomocí rozhraní REST API

1. Nasazení zdroje s operace [Vytvoření nasazení šablony](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Uchovávání informací, které mohou být užitečné pro ladění, nastavte vlastnost **debugSetting** v žádosti o JSON a **requestContent** a/nebo **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Ve výchozím nastavení je hodnota **debugSetting** nastavena na **žádný**. Při zadání hodnoty **debugSetting** , zvažte pečlivě typ informací, které předáváte během nasazení. Protokolování informace o žádosti nebo odpovědi může potenciálně vystavit citlivá data, která se načte prostřednictvím operace nasazení. 

2. Získání informací o nasazení s operací [získat informace o nasazení šablony](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    V odpovědi Všimněte si, zejména prvky **provisioningState** , **correlationId** a **chyb** . **CorrelationId** slouží ke sledování událostí souvisejících a může být užitečné při práci se na technickou podporu k řešení problému.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Získání informací o nasazení operace s operací [seznam všechny operace nasazení šablony](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Odpověď na bude obsahovat žádost a/nebo odpověď informace k různým co zadaná ve vlastnosti **debugSetting** během nasazení.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Získání události z protokolů auditování nasazení se operace [seznam správy událostí v předplatné](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Další kroky

- Pokud potřebujete pomoc s řešením chyby konkrétní nasazení najdete v článku [řešení běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md).
- Další informace o používání protokolů auditování ke sledování jiných typů akce, najdete v článku [auditování operace s správce prostředků](resource-group-audit.md).
- Ověřte nasazení před ho spustíte, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md).
