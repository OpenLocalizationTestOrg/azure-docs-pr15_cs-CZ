<properties
    pageTitle="Azure vzorky úvodní Monitor rozhraní příkazového řádku. | Microsoft Azure"
    description="Ukázkové příkazy rozhraní příkazového řádku pro sledování Azure funkce. Azure Monitor je služby Microsoft Azure, která umožňuje odeslat oznámení, zavolejte na webové adresy URL na základě hodnot nakonfigurované telemetrickými daty a automatické měřítko cloudové služby, virtuálních počítačích a webových aplikací Web Apps."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure ukázky úvodní Monitor a platformy rozhraní příkazového řádku

V tomto článku se dozvíte, ukázka, že rozhraní příkazového řádku (rozhraní příkazového řádku) příkazy, které usnadňují přístup k funkcím Azure Monitor. Azure Monitor umožňuje automatické měřítko cloudové služby, virtuálních počítačích a webových aplikací Web Apps a odeslat oznámení nebo volání adresy URL webových na základě hodnot nakonfigurované telemetrickými daty.

>[AZURE.NOTE] Azure Monitor je nový název pro co označovalo jako "Azure přehledy" až do 25 Sept 2016. Však obory názvů a tím následující příkazy stále obsahovat "přehledy".


## <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud jste ještě nenainstalovali rozhraní příkazového řádku Azure, najdete v článku [instalace Azure rozhraní příkazového řádku](../xplat-cli-install.md). Pokud jste seznámeni s Azure rozhraní příkazového řádku, můžete získat další informace o na [použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows s správce prostředků Azure](../xplat-cli-azure-resource-manager.md).


V systému Windows nainstalujte z [webu Node.js](https://nodejs.org/)npm. Po dokončení instalace pomocí CMD.exe s oprávněními spustit jako správce, spustit ze složky s nainstalovaným npm následující:

```console
npm install azure-cli --global
```

Další přejděte do složky/kdekoli chcete a v příkazového řádku napište:

```console
azure help
```

## <a name="log-in-to-azure"></a>Přihlaste se k Azure

Cílem prvního kroku je k přihlášení k účtu Azure.

```console
azure login
```

Po spuštění tohoto příkazu, budete muset přihlásit pomocí pokynů na obrazovce. Potom se zobrazí váš účet, TenantId a výchozí ID předplatného. Všechny příkazy pracovat v rámci předplatného výchozí.

Chcete-li zobrazit podrobnosti o aktuálního předplatného, zadejte následující příkaz.

```console
azure account show
```

Změnit pracovní kontextu jiné předplatné, použijte následující příkaz.

```console
azure account set "subscription ID or subscription name"
```

Používat příkazy Správce prostředků Azure a sledování Azure, musíte být v režimu správce prostředků Azure.

```console
azure config mode arm
```

Chcete-li zobrazit seznam všech podporovaných příkazů Azure Monitor, proveďte následující kroky.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Zobrazení protokolů auditování pro předplatné

Chcete-li zobrazit seznam protokolů auditování, proveďte následující kroky.

```console
azure insights logs list [options]
```

Vyzkoušejte následující kroky zobrazit všechny dostupné možnosti.

```console
azure insights logs list -help
```

Tady je příklad do seznamu protokolů tak, že resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Příklad seznamu protokoly volajícím

```console
azure insights logs list --caller "myname@company.com"
```

Příklad seznamu protokoly volajícím na typu zdroje v rámci počáteční a koncové datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Práce s upozornění
Můžete informace v části Práce se upozornění.

### <a name="get-alert-rules-in-a-resource-group"></a>Získání pravidla výstrah ve skupině zdroje

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Vytvořit pravidlo metrické upozornění

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Vytvoření pravidla upozornění protokolu

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Vytvořit pravidlo výstrahy webtest

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Odstranění pravidla výstrahy

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profily protokolu
Pomocí informací v této části můžete pracovat s profily protokolu.

### <a name="get-a-log-profile"></a>Získání protokolu profilu

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Přidání profilu protokolu bez uchovávání informací

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrání protokolu profilu

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Přidání profilu protokolu s uchovávání informací

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Přidání profilu protokolu s uchovávání informací a EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostika
Použití informací v tomto oddílu pro práci s diagnostiky nastavení.

### <a name="get-a-diagnostic-setting"></a>Získání diagnostiky nastavení

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Vypnutí diagnostiky

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Povolit diagnostické nastavení bez uchovávání informací

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatické měřítko
Pomocí informací v této části můžete pracovat s nastavením automatické měřítko. Potřebujete změnit tyto příklady.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Získat nastavení automatické měřítko pro skupinu prostředků

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Získat nastavení automatické měřítko podle názvu ve skupině zdroje

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Nastavení auotoscale

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
