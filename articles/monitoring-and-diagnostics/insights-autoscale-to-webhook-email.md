<properties
    pageTitle="Použití automatické měřítko akcí k odesílání e-mailu a webhook oznámení. | Microsoft Azure"
    description="Informace o použití akce automatické měřítko zavolat adresy URL webových nebo odeslání e-mailová oznámení v Azure Monitor. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Použití automatické měřítko akcí odeslání e-mailu a webhook oznámení v Azure monitoru

Tento článek popisuje, jak nastavení aktivační události, aby bylo volání určité webové adresy URL nebo posílat e-maily podle automatické měřítko akcí v Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks umožňují směrovat Azure oznámení na jiných systémů pro oznámení po zpracování nebo vlastní. Například směrování oznámení služby, které můžete dělat žádost příchozí tak, aby zaslal že služby SMS protokolu chyb oznámení služby týmu pomocí konverzace nebo zprávy atd. Webhook URI musí být platná HTTP nebo HTTPS koncového bodu.

## <a name="email"></a>E-mailu
E-mailu odesílat libovolný platný e-mailovou adresu. Správci a dalších správců předplatného, kde je spuštěný pravidlo se také oznámení.


## <a name="cloud-services-and-web-apps"></a>Cloudovými službami a webových aplikací Web Apps
Je můžete určovat, jestli se z portálu Microsoft Azure pro sprá cloudovými službami a serverové farmy (Web Apps).

- Zvolte míru **měřítko tak, že** .

![Zobrazit jako](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuální počítač měřítko sady
Pro novější virtuálních počítačích vytvořené pomocí Správce zdrojů (virtuální počítač měřítko sady) můžete nakonfigurovat to pomocí rozhraní REST API Správce prostředků šablony, prostředí PowerShell a rozhraní příkazového řádku. Portál rozhraní ještě není k dispozici.
Pokud chcete použít šablonu rozhraní REST API nebo správce prostředků, zahrňte element oznámení pomocí následujících možností.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Pole                              |Povinné? |Popis|
|---                                |---        |---|
|operace                          |Ano        |Hodnota musí být "Měřítko"|
|sendToSubscriptionAdministrator    |Ano        |Hodnota musí být "true" nebo "false"|
|sendToSubscriptionCoAdministrators |Ano        |Hodnota musí být "true" nebo "false"|
|customEmails                       |Ano        |může být hodnota null [] nebo řetězců e-mailů|
|webhooks                           |Ano        |Hodnota může být null nebo platné Uri|
|serviceUri                         |Ano        |platné https Uri|
|Vlastnosti                         |Ano        |Hodnota musí být prázdné {} nebo může obsahovat klíč dvojice|


## <a name="authentication-in-webhooks"></a>Ověřování v webhooks
Existují dva typy URI ověření:

1. Token základní ověřování, kam ukládat webhook URI s tokenu ID jako parametr dotazu. Například https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Základní ověřování, kde používáte uživatelské ID a heslo. Napříkladhttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Schéma datové webhook oznámení automatické měřítko
Při generování oznámení automatické měřítko následující metadat je součástí datové webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Pole  |Povinné?|    Popis|
|---|---|---|
|Stav |Ano    |Stav, který označuje, že byl vytvořen automatické měřítko akce|
|operace| Ano |Pro zvýšení instancí bude "Měřítko," a snížení v případech, budete mít "Měřítko ve"|
|kontext|   Ano |Kontext akce automatické měřítko|
|časové razítko| Ano |Časové razítko při aktivaci automatické měřítko akce|
|ID |Ano|   Správce prostředků ID nastavení automatické měřítko|
|Jméno   |Ano|   Název nastavení automatické měřítko|
|Podrobnosti|   Ano |Vysvětlení akci, která pořídili pomocí služby Automatické měřítko a změna počtu instanci|
|subscriptionId|    Ano |ID předplatného cílové prostředku, který je s měřítkem|
|resourceGroupName| Ano|    Pole Skupina zdroje název cílové prostředek, který je s měřítkem|
|resourceName   |Ano|   Název cílové prostředek, který je s měřítkem|
|resourceType   |Ano|   Tři podporované hodnot: "microsoft.classiccompute/domainnames/slots/roles" – cloudové služby role "microsoft.compute/virtualmachinescalesets" - sady měřítko virtuálního počítače a "Microsoft.Web/serverfarms" - Web Appu|
|resourceId |Ano|Správce prostředků ID cílové prostředek, který je s měřítkem|
|portalLink |Ano    |Azure portálu odkaz na souhrnné stránce cílové zdrojů|
|oldCapacity|   Ano |Aktuální (starý) instance počet kdy automatické měřítko měřítko akce|
|newCapacity|   Ano |Nové instance počet automatické měřítko diagramů s měřítky zdroj|
|Vlastnosti|    Ne| Volitelné. Nastavení < klíče, hodnota > dvojice (například slovník < řetězec, řetězec >). Vlastnosti pole není povinné. Uživatelské rozhraní nebo pracovního aplikace na základě použití logických operátorů můžete zadat klíče a hodnoty, které lze předat pomocí datové. Alternativní způsob k předání vlastní vlastnosti odchozího hovoru webhook, je použít webhook URI samotné (jako parametry dotazu)|
