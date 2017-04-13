<properties
    pageTitle="Azure vzorky úvodní Monitor Powershellu. | Microsoft Azure"
    description="Použití Powershellu ke přístup k funkcím Azure Monitor například automatické měřítko, oznámení, webhooks a vyhledávání protokoly aktivity."
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor Powershellu úvodní vzorky

Tento článek ukazuje, přehrajte příkazy Powershellu pro umožňují přístup k funkcím Azure Monitor. Azure Monitor umožňuje automatické měřítko Cloudovým službám, virtuálních počítačích a webových aplikací Web Apps a odeslat oznámení nebo volání adresy URL webových na základě hodnot nakonfigurovanou telemetrickými daty.

>[AZURE.NOTE] Azure Monitor je nový název pro co označovalo jako "Azure přehledy" až do 25 Sept 2016. Však obory názvů a tím následující příkazy stále obsahovat "přehledy".

## <a name="set-up-powershell"></a>Nastavení prostředí PowerShell
Pokud jste to ještě neudělali, nastavte prostředí PowerShell spusťte ve vašem počítači. Další informace najdete v tématu [jak instalace a konfigurace prostředí PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Příklady v tomto článku

Příklady v následujícím článku ukazují, jak můžete používat rutiny Azure Monitor. Můžete taky zkontrolovat celý seznam rutiny prostředí PowerShell Monitor Azure na [rutiny Azure monitoru (přehledy)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).


## <a name="sign-in-and-use-subscriptions"></a>Přihlaste se a používání předplatného

Nejdřív se přihlaste se k Azure předplatné.

```PowerShell
Login-AzureRmAccount
```

Je nutné přihlášení. Po odebrání účtu, zobrazí se TenantId a výchozí Id předplatného. Azure rutiny pro práci v rámci předplatného výchozí. Chcete-li zobrazit seznam předplatných, ke kterým máte přístup k, zadejte následující příkaz.

```PowerShell
Get-AzureRmSubscription
```

Změnit pracovní kontextu jiné předplatné, použijte následující příkaz.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Načtení protokolů auditování pro předplatné
Použití `Get-AzureRmLog` rutiny.  Tady je několik běžných příkladů.

Položky protokolu vám poskytnou toto datum/čas prezentace:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Zobrazit položky protokolu mezi čas/časové období:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Získání protokolu položek z určité skupiny zdrojů:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Získání protokolu položek z určitého zdroje poskytovatelem mezi čas/časové období:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Zobrazit všechny položky protokolu s konkrétní volající:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Následující příkaz získá poslední 1000 události z protokolu auditování:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`podporuje mnoho dalších parametrů. Zobrazit `Get-AzureRmLog` odkaz Další informace.

>[AZURE.NOTE] `Get-AzureRmLog`poskytuje pouze 15 dní. Použití parametru **- MaxEvents** umožňuje dotazu poslední N událostí za 15 dní. Přístup k události starší než 15 dní můžete rozhraní REST API nebo SDK (C# vzorek pomocí SDK). Pokud nezadáte **čas spuštění**, výchozí hodnota je **Čas_ukončení** mínus 1 hodinu. Pokud nezadáte **Čas_ukončení**, výchozí hodnota je aktuální čas. Vždy jsou UTC.

## <a name="retrieve-alerts-history"></a>Načtení upozornění historie
Pokud chcete zobrazit všechny upozornění události, můžete dotaz protokoly správce prostředků Azure pomocí následující příklady.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Pokud chcete zobrazit historii pro konkrétní pravidlo výstrahy, můžete použít `Get-AzureRmAlertHistory` rutina předejte číslo ID zdroje pravidla výstrahy.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzureRmAlertHistory` Rutina podporuje různá parametry. Další informace najdete v článku [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Získat informace o pravidlech upozornění
Všechny z následujících příkazů chovalo na skupina zdroje s názvem "montest".

Zobrazte všechny vlastnosti upozornění pravidla:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Načtení všechna upozornění ve skupině zdroje:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Načtení všechna upozornění pravidla nastavit pro cílovou zdroj. Například všechna upozornění pravidla nastavit virtuálního počítače.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`podporuje další parametry. Další informace najdete v článku [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Vytváření pravidel upozornění
Můžete použít `Add-AlertRule` rutina vytvářet, aktualizovat a zakažte pravidlo výstrahy.

Můžete vytvořit e-mailu a webhook vlastností pomocí `New-AzureRmAlertRuleEmail` a `New-AzureRmAlertRuleWebhook`, v tomto pořadí. V rutinu pravidlo výstrahy přiřadíte jako akce vlastnost **Akce** pravidla výstrahy.

Následující části obsahuje vzorku, které vám ukáže, jak vytvořit pravidlo výstrahy s různými parametry.

### <a name="alert-rule-on-a-metric"></a>Upozornění pravidlo pro metriky
Následující tabulka popisuje parametry a hodnoty k vytvoření upozornění pomocí metriky.


|Parametr|Hodnota|
|---|---|
|Jméno|  simpletestdiskwrite|
|Umístění toto upozornění pravidlo|   Východní USA|
|ResourceGroup| montest|
|TargetResourceId|  /subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|MetricName upozornění, která se vytvoří|   \PhysicalDisk (_Total) \Disk zápisy/s. Zobrazit `Get-MetricDefinitions` rutina pod o tom, jak získat přesné metrické názvy|
|operátor|  GreaterThan|
|Mezní hodnota (počet/sec v tomto metriky)|    1|
|Velikost_okna (hh formát)|  00:05:00|
|Agregátor (statistický metrické, který využívá průměrný počet, v tomto případě)|  Průměr|
|vlastní e-mailů (řetězců)|'foo@example.com','bar@example.com'|
|odeslání e-mailu a vlastníci, přispěvatelům čtenáři|    -SendToServiceOwners|

Vytvoření e-mailu akce

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Vytvoření Webhook akce

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Vytvoření pravidla na míru % procesoru na klasické OM

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Načtení pravidlo výstrahy

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Rutina upozornění přidat taky automaticky aktualizuje pravidla Pokud již existuje upozornění pravidlo pro dané vlastnosti. Zakázání pravidlo výstrahy, použijte parametr **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Upozornění na událost protokolu auditování

>[AZURE.NOTE] Tato funkce je pořád v náhledu.

V tomto scénáři odešlete e-mailu je-li web úspěšně spuštěna v předplatného v zdrojů skupina *abhingrgtest123*.

Nastavení e-mailu pravidla

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Nastavení pravidla webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Vytvoření pravidla na události

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Načtení pravidlo výstrahy

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

`Add-AlertRule` Rutina umožňuje různé další parametry. Další informace najdete v tématu [Přidání AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Získat seznam dostupných metriky pro upozornění
Můžete použít `Get-AzureRmMetricDefinition` rutina zobrazíte seznam všech metriky určitého zdroje.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Následující příklad vytvoří tabulku s metrikou název a jednotky pro něj.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Úplný seznam dostupných možností `Get-AzureRmMetricDefinition` na [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx)je k dispozici.


## <a name="create-and-manage-autoscale-settings"></a>Vytváření a Správa nastavení automatické měřítko
Prostředku, například do webových aplikací, OM, cloudové služby nebo nastavte měřítko OM může mít jenom jeden automatické měřítko nastavení nakonfigurována.
Jednotlivá nastavení automatické měřítko však může mít více profily. Například jednu profilu založených na výkonu měřítko a druhý pro harmonogram založeny profilu. Každý profil může obsahovat více pravidel nakonfigurovaný na něj. Další informace o nastavení automatické měřítko najdete v článku [jak automatické měřítko aplikace](../cloud-services/cloud-services-how-to-scale.md).

Tady je postup, který použijete:

1. Vytvoření pravidla.
2. Vytvoření profilů mapování pravidla, která jste vytvořili dříve do profilů.
3. Volitelné: Vytvoření upozornění pro automatické měřítko nakonfigurováním vlastnosti webhook a e-mailu.
4. Vytvoření nastavení automatické měřítko s názvem na cílový prostředek mapováním profily a oznámení, které jste vytvořili v předchozích krocích.

Následující příklady ukazují, jak můžete vytvořit nastavení automatické měřítko pro změnu velikosti OM nastavit pomocí míru využití procesoru operačního systému Windows.

Nejdřív vytvořte pravidla pro škálování, s zvýšení počtu instance.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Dále vytvořte pravidla pro měřítko se změnami, s snížení počtu instance.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Vytvořte profilu pro pravidla.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Vytvoření webhook vlastnosti.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Vytvořte vlastnost oznámení pro nastavení automatické měřítko, včetně e-mailu a webhook, který jste vytvořili dříve.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Nakonec vytvořte nastavení automatické měřítko přidat na profil, který jste vytvořili výše.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Další informace o správě nastavení automatické měřítko najdete v článku [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automatické měřítko historie
Následující příklad ukazuje, jak můžete zobrazit posledních automatické měřítko a oznámení událostí. Pomocí pole Hledat protokolu auditování zobrazit historii automatické měřítko.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Můžete použít `Get-AzureRmAutoScaleHistory` rutina k načtení automatické měřítko historie.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Další informace najdete v článku [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Zobrazení podrobností o nastavení automatické měřítko
Můžete použít `Get-Autoscalesetting` rutina získat další informace o nastavení automatické měřítko.

Následující příklad ukazuje podrobnosti o všechna nastavení automatické měřítko ve skupině zdroje "myrg1".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Následující příklad zobrazuje informace o všechna nastavení automatické měřítko ve skupině zdroje "myrg1" a konkrétně nastavení automatické měřítko s názvem "MyScaleVMSSSetting".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Odebrání nastavení automatické měřítko
Můžete použít `Remove-Autoscalesetting` rutina odstranit nastavení automatické měřítko.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Správa profilů protokolu protokolů auditování

Můžete vytvořit *profil protokolu* exportovat data z protokolů auditování k účtu úložiště a konfigurace uchovávání dat pro něj. Data v případě potřeby můžete přenášet rozbočovače události. Všimněte si, že tato funkce momentálně v náhledu a můžete vytvořit jenom jeden profil protokolu jedno předplatné. Tyto rutiny s aktuálního předplatného slouží k vytváření a Správa profilů protokolu. Můžete taky zvolit u určitého předplatného. I když prostředí PowerShell ve výchozím nastavení aktuálního předplatného, můžete kdykoli změnit tuto pomocí `Set-AzureRmContext`. Můžete nakonfigurovat protokolů auditování směrování data do úložiště účtu nebo centrální událostí v rámci tohoto předplatného. Data zapisuje jako objektů blob soubory ve formátu JSON.

### <a name="get-a-log-profile"></a>Získání protokolu profilu
Pokud chcete načíst existující protokolu profily, použijte `Get-AzureRmLogProfile` rutiny.

### <a name="add-a-log-profile-without-data-retention"></a>Přidání bez uchovávání dat protokolu profilu

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrání protokolu profilu

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Přidání profilu protokolu s uchovávání dat

Vlastnost **- RetentionInDays** můžete určit s počet dní, jako kladné celé číslo, které data se zachovají.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Přidání profilu protokolu s uchovávání informací a EventHub
Kromě směrování dat do účtu úložiště, můžete také přenášet ho k rozbočovači události. Nezapomeňte, že v této verzi preview a úložiště je konfigurace účtu povinné, ale události centrální konfigurace je nepovinný krok.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurace protokolování diagnostiky
Mnoho služby Azure poskytují a další protokolování telemetrie, včetně Azure sítě skupiny zabezpečení, Vyrovnávání zatížení Software, trezoru klíč, Azure vyhledávací služby a logiky aplikace a může být nakonfigurované pro uložení dat ve vašem úložišti Azure účtu. Operace lze provádět pouze na úrovni zdroje a účtu úložiště by měly tvořit ve stejné oblasti jako cílové zdrojů, kde je nakonfigurovaný nastavení diagnostických nástrojů.

### <a name="get-diagnostic-setting"></a>Získání diagnostiky nastavení

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Vypnutí diagnostiky

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Povolit diagnostické nastavení bez uchovávání informací

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Povolit diagnostické nastavení s uchovávání informací

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Povolit diagnostické nastavení s uchovávání informací pro konkrétní protokolu kategorii

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
