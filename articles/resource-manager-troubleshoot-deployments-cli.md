<properties
   pageTitle="Zobrazit nasazení operace s Azure rozhraní příkazového řádku | Microsoft Azure"
   description="Popisuje, jak používat rozhraní příkazového řádku Azure zjistěte, jestli nejsou problémy s z nasazení Správce prostředků."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Zobrazit nasazení operace s Azure rozhraní příkazového řádku

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [Prostředí PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md)
- [ROZHRANÍ REST API](resource-manager-troubleshoot-deployments-rest.md)

Pokud jste dostali k chybě při nasazení zdroje Azure, můžete zobrazit další podrobné informace o nasazení operacích, které byly provedeny. Azure rozhraní příkazového řádku obsahuje příkazy, které umožňují najít chyby a určit potenciální opravy.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Ověřením šablony a infrastrukturu před nasazením se můžete vyhnout chybám. Můžete taky protokolu další žádosti a odpověď informací při nasazení, které mohou být užitečné později pro řešení potíží. Informace o ověřování a informací o protokolování žádostí a odpovědí, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Použití protokolů auditování řešit problémy s

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Zobrazit chyby nasazení, použijte podle těchto kroků:

1. Pokud chcete zobrazit v protokolech auditování, příkaz **protokol azure skupiny zobrazit** . Můžete zahrnout **– nasazení poslední** možnost načíst pouze protokol posledních nasazení.

        azure group log show ExampleGroup --last-deployment

2. Příkaz **Zobrazit protokol azure skupiny** vrátí hodně informací. Návody na řešení, obvykle Chcete-li zaměřit na neúspěšných operací. Tento skript používá pro hledání v protokolu selhání nasazení možnost **– json** a nástroj [jq](https://stedolan.github.io/jq/) JSON.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Uvidíte, že **Vlastnosti** obsahuje informace ve formátu json o nezdařeném uložení operaci.

    Můžete použít **– podrobného** a **- vv** možnosti zobrazíte další informace z protokoly.  Použití **– podrobného** možnost, která zobrazí kroky operace absolvovat na `stdout`. Úplné žádosti historie použijte možnost **- vv** . Zprávy často poskytují zásadní vodítek o možné příčině všechny chyby.

3. Zaměření na zprávy o stavu pro nezdařeném uložení položek, použijte tento příkaz:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Poradce při potížích s pomocí operace nasazení

1. Pokud potřebujete celkový stav nasazení s příkazem **Zobrazit nasazení azure skupiny** . V příkladu dole se nezdařila nasazení.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Zobrazí zpráva nezdařené operace nasazení, můžete:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Další kroky

- Pokud potřebujete pomoc s řešením chyby konkrétní nasazení najdete v článku [řešení běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md).
- Další informace o používání protokolů auditování ke sledování jiných typů akce, najdete v článku [auditování operace s správce prostředků](resource-group-audit.md).
- Ověřte nasazení před ho spustíte, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md).
