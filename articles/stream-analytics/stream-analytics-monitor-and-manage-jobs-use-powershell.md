<properties 
    pageTitle="Sledování a Správa úloh toku analýzy pomocí prostředí PowerShell | Microsoft Azure" 
    description="Naučte se používat Azure prostředí PowerShell a rutinách ke sledování a Správa úloh toku analýzy." 
    keywords="Azure powershellu, rutiny prostředí powershell azure, příkazového prostředí powershell skriptů powershellu" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Sledování a Správa úloh toku analýzy pomocí rutin prostředí PowerShell Azure

Zjistěte, jak můžete sledovat a spravovat zdroje toku analýzy pomocí rutin prostředí PowerShell Azure a skriptů powershellu, které provádět základní úkoly toku analýzy.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Požadavky pro spuštění rutiny prostředí PowerShell Azure pro analýzy toku

 - Vytvoření skupiny zdroje Azure předplatné. Následujícím obrázku je ukázkový skript Powershellu Azure. Azure PowerShell informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md);  

Azure prostředí PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Úlohy toku analýzy vytvořené programově nemusíte sledování standardně zapnutá.  Můžete ručně povolit sledování na portálu Azure tak, že přejdete na stránku projektu Monitor a klikněte na tlačítko Povolit nebo dosáhnete programově provedením kroků c:\ [Azure toku technologie pro analýzu – sledování toku analýzy úlohy programově](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure rutiny prostředí PowerShell pro analýzy toku
Tyto rutiny prostředí PowerShell Azure slouží ke sledování a Správa úloh Azure toku analýzy. Všimněte si, že Azure PowerShell má různé verze. 
**Příklady uvedené určen prvního příkazu Powershellu Azure 0.9.8, druhý příkaz je určený pro Azure PowerShell 1.0.** Příkazy Azure PowerShell 1.0 bude mít vždycky "AzureRM" příkazu.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Obsahuje seznam všech úloh toku analýzy podle Azure předplatné nebo daná skupina zdrojů nebo získá úlohy informace o určitém projektu v rámci skupiny zdrojů.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy toku analýzy v Azure předplatného.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy toku analýzy ve skupině prostředků StreamAnalytics výchozí centrální spojené.

**Příklad 3**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o projektu toku analýzy StreamingJob ve skupině prostředků StreamAnalytics výchozí centrální spojené.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Obsahuje seznam všech vstupy, které jsou definovány zadanou práci toku analýzy nebo získání informací o konkrétní vstupní.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o všech vstupů podle projektu StreamingJob.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Tento příkaz prostředí PowerShell vrátí informace o zadávání s názvem EntryStream podle projektu StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Obsahuje seznam všech výstupů, které jsou definovány zadanou práci toku analýzy nebo získání informací o konkrétní výstupu.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o výstupy podle projektu StreamingJob.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Tento příkaz prostředí PowerShell vrátí informace o výstupu s názvem definovaná v projektu StreamingJob.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Získání informací o kvóty datových proudů jednotek v zadané oblasti.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Tento příkaz prostředí PowerShell vrátí informace o kvóty a použití te000126961 streamování jednotek v oblasti centrální USA.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Získání informací o určitou transformaci podle projektu toku analýzy.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o transformaci s názvem StreamingJob úlohy StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nové AzureStreamAnalyticsInput | Nové AzureRMStreamAnalyticsInput
Vytvoří nový vstupní úlohy toku analýzy nebo aktualizuje existující zadaný vstupní.
  
Lze zadat název vstupní v souboru .json nebo na příkazovém řádku. Pokud jsou zadány oba, název do příkazového řádku musí být stejný jako ten v souboru.

Pokud zadání vstupu, které už existuje a není zadán – vyšší parametru rutinu zobrazí dotaz, jestli se mají nahradit stávající vstupní.

Pokud zadáte řetězec-vynutit parametr a určete existující zadat název, vstup nahradí bez potvrzení.

Podrobné informace o JSON soubor struktury a obsahu najdete v příručce [Vytvořit vstupní (Azure toku technologie pro analýzu)] [ msdn-rest-api-create-stream-analytics-input] část [toku analýzy správy REST API odkaz knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Tímto příkazem prostředí PowerShell vytvoříte nové vstupní ze souboru Input.json. Pokud již je definován existující vstupní s názvem souboru vstupní definice, rutinu zobrazí dotaz, zda chcete nahradit.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Tento příkaz prostředí PowerShell vytvoří nový vstupní v úlohu s názvem EntryStream. Pokud již je definován existující vstupní s tímto názvem, rutinu zobrazí dotaz, zda chcete nahradit.

**Příklad 3**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Tento příkaz prostředí PowerShell nahradí definici existující vstupní zdroje s názvem EntryStream s definicí ze souboru.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nové AzureStreamAnalyticsJob | Nové AzureRMStreamAnalyticsJob
Vytvoří novou úlohu toku analýzy v Microsoft Azure nebo aktualizuje definici existující zadaný úlohu.

V souboru .json nebo na příkazovém řádku lze zadat název projektu. Pokud jsou zadány oba, název do příkazového řádku musí být stejný jako ten v souboru.

Pokud zadáte název projektu, který již existuje a není zadán – vyšší parametru rutinu zobrazí dotaz, jestli se mají nahrazení existujícího projektu.

Pokud zadáte řetězec-vynutit parametr a zadejte název existujícího projektu, nahradí definici úlohy bez potvrzení.

Podrobné informace o JSON soubor struktury a obsahu najdete v příručce [Vytvořit úlohy analýzy toku] [ msdn-rest-api-create-stream-analytics-job] část [toku analýzy správy REST API odkaz knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Tento příkaz prostředí PowerShell vytvoří nový projekt ze definici v JobDefinition.json. Pokud již je definován existující úlohu s názvem souboru definice úlohy, rutinu zobrazí dotaz, zda chcete nahradit.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Tento příkaz prostředí PowerShell nahradí definice úlohy StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nové AzureStreamAnalyticsOutput | Nové AzureRMStreamAnalyticsOutput
Vytvoří nový výstup úlohy toku analýzy nebo aktualizuje existující výstupu.  

Lze zadat název výstup v souboru .json nebo na příkazovém řádku. Pokud jsou zadány oba, název do příkazového řádku musí být stejný jako ten v souboru.

Pokud zadáte výstup, která už existuje a není zadán – vyšší parametru rutinu zobrazí dotaz, jestli se mají nahradit stávající výstup.

Pokud zadáte řetězec-vynutit parametr a určete existující výstup název, nahradí výstup bez potvrzení.

Podrobné informace o JSON soubor struktury a obsahu najdete v příručce [Vytvořit výstup (Azure toku technologie pro analýzu)] [ msdn-rest-api-create-stream-analytics-output] část [toku analýzy správy REST API odkaz knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Tímto příkazem prostředí PowerShell vytvoříte nový výstup s názvem "výstup" úlohy StreamingJob. Pokud již je definován existující výstup s tímto názvem, rutinu zobrazí dotaz, zda chcete nahradit.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Tento příkaz prostředí PowerShell nahradí definici "výstup" úlohy StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nové AzureStreamAnalyticsTransformation | Nové AzureRMStreamAnalyticsTransformation
Vytvoří novou transformaci úlohy toku analýzy nebo aktualizuje existující transformace.
  
Lze zadat název transformace v souboru .json nebo na příkazovém řádku. Pokud jsou zadány oba, název do příkazového řádku musí být stejný jako ten v souboru.

Pokud zadáte transformaci, která už existuje a není zadán – vyšší parametru rutinu zobrazí dotaz, jestli se mají nahradit stávající transformace.

Pokud zadáte řetězec-vynutit parametr a zadejte název existující transformace, transformace nahradí bez potvrzení.

Podrobné informace o JSON soubor struktury a obsahu najdete v příručce k [Vytvoření transformace (Azure toku technologie pro analýzu)] [ msdn-rest-api-create-stream-analytics-transformation] část [toku analýzy správy REST API odkaz knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Tímto příkazem prostředí PowerShell vytvoříte novou transformaci s názvem StreamingJobTransform úlohy StreamingJob. Pokud již je definován transformaci existující s tímto názvem, rutinu zobrazí dotaz, zda chcete nahradit.

**Příklad 2**

Azure prostředí PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Tento příkaz prostředí PowerShell nahradí definici StreamingJobTransform úlohy StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Odebrat AzureStreamAnalyticsInput | Odebrat AzureRMStreamAnalyticsInput
Asynchronní odstraní konkrétní vstupní z toku analýzy projektu v Microsoft Azure.  
Pokud zadáte – vyšší parametr, vstup odstraní bez potvrzení.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Příkaz prostředí PowerShell odebere vstupní EventStream úlohy StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Odebrat AzureStreamAnalyticsJob | Odebrat AzureRMStreamAnalyticsJob
Asynchronní odstraní konkrétní toku analýzy projektu v Microsoft Azure.  
Pokud zadáte – vyšší parametr, úkoly se odstraní bez potvrzení.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tento příkaz prostředí PowerShell Odebere úlohu StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Odebrat AzureStreamAnalyticsOutput | Odebrat AzureRMStreamAnalyticsOutput
Asynchronní odstraní konkrétní výstup z toku analýzy projektu v Microsoft Azure.  
Pokud zadáte – vyšší parametr, odstraní se výstup bez potvrzení.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Příkaz prostředí PowerShell odebrán výstup výstup do této úlohy StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Zahájení AzureStreamAnalyticsJob | Zahájení AzureRMStreamAnalyticsJob
Asynchronní nasazuje a spustí toku analýzy projektu v Microsoft Azure.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Tento příkaz prostředí PowerShell spustí úlohu StreamingJob s čas začátku vlastní výstup nastavit 12 prosinec 2012 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Zastavit AzureStreamAnalyticsJob | Zastavit AzureRMStreamAnalyticsJob
Asynchronní zastavení toku analýzy úlohy v aplikaci Microsoft Azure a zrušte přidělí prostředky, které byly jestli byly název použitý. Definice úlohy a metadata zůstanou k dispozici v rámci předplatného prostřednictvím Azure portálem a rozhraní API, pro správu tak, že můžete upravovat a restartování projektu. Se nezapočítávají pro práci se přestal.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tento příkaz prostředí PowerShell zastavení úlohy StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Testuje možnost analýzy toku připojení k zadané vstupní.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Tento příkaz prostředí PowerShell srovnává stav připojení vstupní EntryStream StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Testuje možnost analýzy toku připojení k zadané výstupní.

**Příklad 1**

Azure prostředí PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Tento testů příkazového prostředí PowerShell stav připojení výstup výstup ve StreamingJob.  

## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
