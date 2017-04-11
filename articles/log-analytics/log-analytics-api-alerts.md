<properties
   pageTitle="Protokol analýzy upozornění REST API"
   description="V protokolu analýzy upozornění rozhraní REST API umožňuje vytvářet a spravovat upozornění v sadě správy operace (OMS).  Tento článek obsahuje podrobnosti o rozhraní API a několik příkladů pro provádění různých operací."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Protokol analýzy upozornit rozhraní REST API

V protokolu analýzy upozornění rozhraní REST API umožňuje vytvářet a spravovat upozornění v sadě správy operace (OMS).  Tento článek obsahuje podrobnosti o rozhraní API a několik příkladů pro provádění různých operací.

Rozhraní REST API protokolu analýzy hledání je RESTful a můžete k nim získat přístup prostřednictvím REST API Azure správce prostředků. V tomto dokumentu najdete příklady kde se pracuje s rozhraní API z příkazového řádku prostředí PowerShell pomocí [ARMClient](https://github.com/projectkudu/ARMClient), nástroj příkazového řádku otevřít zdroj, který usnadňuje vyvolání rozhraní API Azure správce prostředků. Použití ARMClient a prostředí PowerShell je jednu z mnoha možností pro přístup k rozhraní API pro hledání protokolu analýzy. Pomocí těchto nástrojů můžete využít Azure správce prostředků rozhraní API RESTful volat do pracovních prostorů OMS a spusťte hledání příkazy v nich. Rozhraní API bude výstupní výsledky hledání můžete ve formátu JSON umožňuje používat výsledky hledání v mnoha různými způsoby programově.

## <a name="prerequisites"></a>Zjistit předpoklady pro
V současné době upozornění pouze vytvořením uložené vyhledávání v protokolu analýzy.  Můžete vytvořit odkaz [Log hledání REST API](log-analytics-log-search-api.md) pro další informace.

## <a name="schedules"></a>Plány
Uložené hledání můžete mít nejméně jeden plán. Plán Určuje, jak často vyhledávání spustit a časový interval, přes který je označen kritéria.
Plány mít vlastností v následující tabulce.

| Vlastnost  | Popis |
|:--|:--|
| Interval | Jak často se spouští hledání. Zadaný v minutách. |
| QueryTimeSpan | Časový interval přes které kritéria Vyhodnocená každá její položka. Musí být rovna nebo větší než Interval. Zadaný v minutách. |
| Verze | Rozhraní API verze použitá.  V současné době to měli vždy nastavit 1. |

Zvažte například dotazu událostí se Interval 15 minut a časový interval 30 minut. V tomto případě každých 15 minut by spustí dotaz a by být výstraha Pokud kritéria nadále překládal true přes období 30 minut.

### <a name="retrieving-schedules"></a>Načítání plány
Použijte metodu Get k načtení všechny plány uloženého hledání.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Použijte metodu Get s ID plán k načtení plánu konkrétní uloženého hledání.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Toto je ukázkový odpověď pro harmonogram.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Vytvoření plánu
Použijte metodu umístění s plánu jedinečných ID pro vytvoření nového plánu.  Všimněte si dvou plány barvou stejné ID i když jsou přidružené k jiné uložená hledání.  Při vytváření plánu v konzole OMS identifikátor GUID se vytvoří ID plánu.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Úprava plánu
Použijte metodu umístění s ID existující plán pro stejný uložené hledání pro úpravy, které plánu.  Textu žádosti o musí obsahovat etag plánu.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Odstranění kalendáře
Použijte metodu odstranit pomocí ID plán pro odstranění kalendáře.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akce
Plán může obsahovat více akcí. Akce může definovat jednoho nebo více procesů provádět například odesílání Hromadná a od postupu runbook, nebo ho může definovat mezní hodnota, která určuje, kdy výsledky hledání kritériím některé.  Některé akce definovat i tak, aby procesy provádí při splnění je mezní hodnota.

Vlastnosti mít všechny akce v následující tabulce.  Různé typy upozornění mají různé další vlastnosti, které jsou popsané níže.

| Vlastnost | Popis |
|:--|:--|
| Typ | Typ akce.  Možné hodnoty jsou aktuálně upozornění a Webhook. |
| Jméno | Zobrazovaný název upozornění. |
| Verze | Rozhraní API verze použitá.  V současné době to měli vždy nastavit 1. |

### <a name="retrieving-actions"></a>Načítání akce
Použijte metodu Get k načtení všechny akce pro harmonogram.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Použijte metodu Get s ID akce k načtení určitou akci pro harmonogram.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Vytváření nebo úpravy akcí
Použijte metodu umístění s ID akce, které jsou jedinečné pro plán vytvořit novou akci.  Při vytváření akce v konzole OMS identifikátor GUID je ID akce.

Použijte metodu umístění s existující ID akce pro stejný uložené hledání pro úpravy, které plánu.  Textu žádosti o musí obsahovat etag plánu.

Žádost o formát pro vytvoření nové akce se liší podle typu akce, abyste tyto příklady jsou uvedeny níže.

### <a name="deleting-actions"></a>Odstranění akce
Použijte metodu odstranit s ID akce odstranit akce.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Oznámení akce
Plán by měla být jenom jedna oznámení akce.  Oznámení akce nastala některá oddílů v následující tabulce.  Každý se píše další dole.

| Oddíl | Popis |
|:--|:--|
| Mezní hodnota | Kritéria pro při spuštění akce. |  
| EmailNotification | Odesílání pošty více příjemcům. |
| Remediation | Začněte postupu runbook v Azure automatizaci pokoušet identifikované problém vyřešit. |

#### <a name="thresholds"></a>Mezní hodnoty
Oznámení akce by měl mít jenom jeden mezní hodnota.  Pokud výsledky uloženého hledání svou mezní hodnoty v akci přidruženou hledání, všechny ostatní procesy v této akci pracují.  Akce mohou také obsahovat pouze mezní hodnoty, takže lze použít při akcích ostatních typů, které neobsahují mezní hodnoty.

Mezní hodnoty mají vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Operátor | Operátor porovnání mezní hodnota. <br> gt = větší než <br> lt = menší než |
| Hodnota | Prahovou hodnotu. |

Zvažte například dotazu událostí v intervalu 15 minut, časový interval 30 minut a prahové hodnoty větší než 10. V tomto případě každých 15 minut by spustí dotaz a by být výstraha Pokud vrátí 10 události, které byly vytvořené během určitého 30 minut.

Následuje ukázka odpověď pro akce se jenom mezní hodnota.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Použijte metodu umístění s ID jedinečné akce vytvořit novou akci prahovou hodnotu pro harmonogram.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Chcete-li upravit akci prahovou hodnotu pro harmonogram použijte metodu umístění s existující ID akce.  Textu žádosti o musí obsahovat etag akce.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mailového oznámení
E-mailová oznámení odesílat poštu do jednoho nebo více příjemců.  Obsahují vlastností v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Příjemci | Seznam adres pošta. |
| Předmět | Předmět e-mailu. |
| Příloh | Přílohy nejsou podporované aktuálně, takže to bude vždy hodnotu "Žádné". |

Následuje ukázka odpověď pro e-mailové oznámení akce s prahovou hodnotu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Použijte metodu umístění s ID jedinečné akce k vytvoření nové e-mailu akce pro harmonogram.  Následující příklad vytvoří e-mailové oznámení prahovou hodnotu, takže e-mailu odeslaný po výsledky uložené hledání překračují je mezní hodnota.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Chcete-li upravit akci e-mailu pro harmonogram použijte metodu umístění s existující ID akce.  Textu žádosti o musí obsahovat etag akce.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Nápravné akce
Remediations start postupu runbook v Azure automatizaci pokoušející se problém označenými upozornění.  Musíte vytvořit webhook pro postupu runbook používá v akci remediation a pak zadejte identifikátor URI ve vlastnosti WebhookUri.  Při vytváření tuto akci pomocí konzoly OMS se automaticky vytvoří nový webhook postupu runbook.

Remediations zahrnout vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| RunbookName | Název postupu runbook. Tím se musí shodovat publikované postupu runbook v automatizaci účet nakonfigurovaný v řešení automatizace sady OMS pracovního prostoru. |
| WebhookUri | Identifikátor URI webhook.
| Vypršení platnosti | Datum vypršení platnosti a čas webhook.  Pokud webhook nemá platnosti, může to být platná termínu. |

Následuje ukázka odpověď pro nápravné akce se prahovou hodnotu.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Použijte metodu umístění s ID jedinečné akce vytvořit novou akci remediation pro harmonogram.  Následující příklad vytvoří remediation prahovou hodnotu, takže postupu runbook se spustí při výsledky uložené hledání překračují je mezní hodnota.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Použijte metodu umístění s existující ID akce nápravné akci pro harmonogram.  Textu žádosti o musí obsahovat etag akce.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Příklad
Následuje dokončení příkladu k vytvoření nové e-mailových oznámení.  Tím vytvoříte nový plán spolu s akce obsahující mezní hodnota a e-mailu.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook akce
Webhook akce spustit proces volání adresy URL a volitelně poskytnutím datové k odeslání.  Jsou podobné Remediation akce s výjimkou jsou určeny webhooks, může vyvolat procesy než runbooks Azure automatizaci.  Taky poskytují další možnost datové části nedoručila ji do vzdáleného procesu.

Akce Webhook není prahovou hodnotu, ale místo toho má být přidán plán, který má upozornění akce se prahovou hodnotu.  Můžete přidat víc Webhook akce, které všechny spustí při splnění je mezní hodnota.

Akce Webhook zahrnout vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| WebhookUri | Předmět e-mailu. |
| CustomPayload | Vlastní datové nechat zasílat webhook.  Formát závisí na co webhook očekává. |

Následuje ukázka odpověď pro webhook akce a přidružená oznámení akce s prahovou hodnotu.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Vytvoření nebo úprava webhook akce
Použijte metodu umístění s ID jedinečné akce vytvořit novou akci webhook pro harmonogram.  Následující příklad vytvoří Webhook akce a oznámení akce s prahovou hodnotu, tak, aby webhook se spouští při výsledky uložené hledání překračují je mezní hodnota.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Chcete-li upravit akci webhook pro harmonogram použijte metodu umístění s existující ID akce.  Textu žádosti o musí obsahovat etag akce.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Další kroky

- Pomocí [Rozhraní REST API pro hledání protokolu](log-analytics-log-search-api.md) v protokolu analýzy.
