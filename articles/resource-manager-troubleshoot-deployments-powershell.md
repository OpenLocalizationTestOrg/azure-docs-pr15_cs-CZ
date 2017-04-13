<properties
   pageTitle="Zobrazit nasazení operace s prostředím PowerShell | Microsoft Azure"
   description="Popisuje, jak pomocí Powershellu Azure zjistěte, jestli nejsou problémy s z nasazení Správce prostředků."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Zobrazit nasazení operace s prostředím PowerShell Azure

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [Prostředí PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md)
- [ROZHRANÍ REST API](resource-manager-troubleshoot-deployments-rest.md)

Můžete zobrazit operace nasazení prostřednictvím Powershellu Azure. Můžete nejdůležitější zobrazení operací, když jste přijali chybu při nasazení tak tento článek se zaměřuje na prohlížení operacích, které se nezdařil. Prostředí PowerShell obsahuje rutinách, které umožňují snadné vyhledání chyby a určit potenciální opravy.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Ověřením šablony a infrastrukturu před nasazením se můžete vyhnout chybám. Můžete taky protokolu další žádosti a odpověď informací při nasazení, které mohou být užitečné později pro řešení potíží. Informace o ověřování a informací o protokolování žádostí a odpovědí, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Poradce při potížích s pomocí operace nasazení

1. Chcete-li získat celkový stav nasazení, pomocí příkazu **Get-AzureRmResourceGroupDeployment** . Můžete filtrovat výsledky pouze nasazení, které se nezdařil.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Selhání nasazení vracející ve formátu následující:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Každý nasazení většinou tvoří více operací s každé operace představující kroku v procesu nasazení. Chcete-li zjistit, co je špatně s nasazení, obvykle potřebujete zobrazit podrobné informace o operace nasazení. Můžete zobrazit stav operace s **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Vracející více operací s každého z nich ve formátu následující:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Získat další informace o nezdařeném uložení operace, načítejte vlastnosti pro operace s stavu **se nezdařila** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Která vrací všechny neúspěšných operace s každého z nich ve formátu následující:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Poznamenejte si ID sledování pro operaci. Použijete, v dalším kroku zaměření na určité operace.

4. Stavová zpráva určité selhalo operace, použijte tento příkaz:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Vracející:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Použití protokolů auditování řešit problémy s

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Zobrazit chyby nasazení, použijte podle těchto kroků:

1. K načtení položky protokolu, spusťte příkaz **Get-AzureRmLog** . Parametry **ResourceGroup** a **Stav** umožňuje vrátit pouze události, které se nepodařilo pro skupinu jeden zdroj. Pokud nezadáte čas zahájení a ukončení, budou vráceny položky poslední hodinu.
Například k načtení selhalo operace pro poslední hodinu spuštění:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Můžete zadat určitou časový interval. V následujícím příkladu podíváme pro neúspěšných akce posledního dne. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Nebo můžete zadat přesné počáteční a koncové datum selhalo akce:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Pokud tento příkaz vrátí moc položek a vlastnosti, se můžete zaměřit auditování načtením **Vlastnosti** . Budeme se týkají taky parametr **DetailedOutput** zobrazíte chybové zprávy.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Které vlastnosti položek protokolu vrátí ve formátu následující:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Na základě těchto výsledků, Pojďme zaměření se na druhý prvek. Výsledky dále upřesníte prohlížíte zprávy o stavu pro danou položku.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Vracející:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Další kroky

- Pokud potřebujete pomoc s řešením chyby konkrétní nasazení najdete v článku [řešení běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md).
- Další informace o používání protokolů auditování ke sledování jiných typů akce, najdete v článku [auditování operace s správce prostředků](resource-group-audit.md).
- Ověřte nasazení před ho spustíte, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md).

