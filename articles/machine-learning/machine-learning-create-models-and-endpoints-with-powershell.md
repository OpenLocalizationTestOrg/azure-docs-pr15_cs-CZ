<properties
pageTitle="Vytvoření více modelů z pokus | Microsoft Azure"
description="Pomocí prostředí PowerShell provést více modely výukové počítače a webové koncové body služby stejný algoritmus ale různých školení datové sady."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Vytvoření mnoho modely výukové počítače a web koncové body služby z pokus pomocí prostředí PowerShell

Tady je běžných problémů výukové počítače: Chcete-li vytvořit mnoho modelů, které mají stejného pracovního postupu školení a použít stejný algoritmus, ale mají různé školení datové sady předávat na vstupu. Tento článek ukazuje, jak můžete to udělat ve velkém měřítku v Azure počítače výukové Studio pomocí jenom jeden testu.

Například Řekněme, že jste vlastníkem firmy franšíza pronájem globální bike. Chcete-li vytvořit regresní model odhadnout služba pronájem založené na historické data. Je potřeba 1 000 umístění pronájem opačném konci světa a získaných datovou sadu pro každé umístění, které jsou důležité funkce například datum, čas a počasí a přenos, které jsou specifické pro každé umístění.

Může školení modelu jednou pomocí sloučené verze všechny datové sady přes všechna umístění. Ale každý umístění obsahuje jedinečné prostředí, a proto je vhodnější by jak proškolit regresní modelu samostatně pomocí datové pro každé umístění. Tak, každý školení model může vzít v úvahu velikosti různých store, hlasitost, zeměpisná oblast, základního souboru, bike ovládáním přenosy prostředí, *atd*.

Který může být nejlepším řešením, ale nechcete vytvořit 1 000 pokusy školení Azure počítač přečíst s každý z nich představující jedinečné umístění. Kromě toho jednoznačná úkolu, je také zdá poměrně neefektivní od každého pokusu byste měli stejné součásti s výjimkou školení datovou sadu.

Naštěstí jsme toho můžete dosáhnout pomocí [Azure počítače výukové přeškolení rozhraní API](machine-learning-retrain-models-programmatically.md) a automatizaci úkolů s [Azure počítače výukové Powershellu](machine-learning-powershell-module.md).

> [AZURE.NOTE] Chcete-li našimi ukázkovými proběhnout rychleji, jsme budete snižte počet umístění a od 1 000 10. Ale se 1 000 umístění použít stejné zásady a postupy. Jediný rozdíl je, že pokud si chcete školení od 1 000 datové sady budete pravděpodobně chtít představit paralelně spuštěných následujících skriptů Powershellu. Jak na to je nad rámec v tomto článku, ale najdete příklady prostředí PowerShell multithreading využívaný na Internetu.  

## <a name="set-up-the-training-experiment"></a>Nastavení testu školení

Chceme pomocí příklad [experimentovat školení](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , jsme jste již vytvořili v [Galerii Intelligence Cortana](http://gallery.cortanaintelligence.com). Otevřete tento experiment v [Azure počítače výukové Studio](https://studio.azureml.net) pracovního prostoru.

>[AZURE.NOTE] K vyzkoušení v tomto příkladu, můžete použít standardní pracovní prostor spíše než bezplatné pracovního prostoru. Jsme budete vytvářet jeden koncový bod pro jednotlivé zákazníky - celkem 10 koncové body - a od volného prostoru se omezí na 3 koncové body, které budou vyžadovat standardní pracovního prostoru. Pokud máte jenom volného prostoru, jenom upravte skripty dole umožňující pouze 3 umístění.

Testu používá k importu datovou sadu školení *customer001.csv* s klientem Azure úložiště modul **Importovat Data** . Předpokládejme jsme odebrané všechna umístění pronájem bike datové sady školení a uložené ve stejném umístění úložiště objektů blob s názvy souborů od *rentalloc001.csv* *rentalloc10.csv*.

![Obrázek](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Všimněte si, že modul **Webové služby výstup** je přidán modulu **Vlaku modelu** .
Po nasazení tento experiment jako webové služby koncový bod přidružený k, že výstup vrátí ve formátu souboru .ilearner školení modelu.

Navíc nezapomeňte, že nastavení parametr webové služby pro adresu URL, která používá modulu **Importovat Data** . To umožňuje použít parametr Chcete-li určit jednotlivé školení datové sady jak proškolit modelu doplňku pro každé umístění.
Existují další způsoby, které jsme může to již provedli, například dotaz SQL zobrazený pomocí parametr webové služby k načtení dat z databáze SQL Azure nebo jednoduše pomocí **Webové služby vstupní** modulu v datové sadě předat webové služby.

![Obrázek](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Teď Pojďme spustit tento školení experiment používají výchozí hodnotu *rental001.csv* školení datovou sadu. Pokud můžete zobrazit výstup modulu **vyhodnotit** (klikněte na záhlaví a vyberte **vizualizace**), najdete v článku jsme získat dobré výkonu *AUC* = 0.91. V tomto okamžiku můžeme začít nasazení webové služby mimo tento kurz testu.

## <a name="deploy-the-training-and-scoring-web-services"></a>Nasazení školení a bodování webové služby

Abyste mohli nasadit školení webové služby, klikněte na tlačítko **Nastavení webové služby** pod plátno experiment a vyberte **Nasazení webové služby**. Volání této webové služby "" Bike pronájem školení".

Teď potřebujeme nasazení bodování webové služby.
K tomuto účelu jsme klikněte na **Nastavení webové služby** pod plátno a vyberte **Prediktivní webové služby**. Tím vytvoříte bodování testu.
Jsme potřeba provést několik menší úpravy snažíme usnadnit jeho práce jako webové služby, jako je odebrání popisek sloupce "oznámení" z vstupních dat a omezení výstup pouze id instance a odpovídající předpovídanými hodnotu.

Uložení sami, které fungují, můžete jednoduše otevřít [prediktivní experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) v galerii, ve které je už počítat.

Abyste mohli nasadit webové služby, spustit prediktivní pokus a potom klikněte na tlačítka **Nasazení webová služba** pod plátno. Pojmenování bodování webové služby "Bike pronájem bodování" ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Vytvoření 10 koncové body identickými webové služby pomocí prostředí PowerShell

Tato webová služba je součástí výchozí koncový bod. Ale jsme zajímá, ne jako výchozí koncový bod protože se nezdařila. Potřebujeme dělat je vytvořit 10 další koncové body, jedno pro každé umístění. Jsme to udělat pomocí prostředí PowerShell.

Nejdřív jsme nastavení naše prostředí PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Spusťte tento příkaz Powershellu:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Teď vytvořili jsme 10 koncové body a všechny obsahují stejný školení model školení na *customer001.csv*. Můžete tyto informace zobrazit v portálu pro správu Azure.

![Obrázek](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Aktualizovat koncové body používat zvláštní vzdělávání datové sady pomocí prostředí PowerShell

Dalším krokem je aktualizovat koncové body s jedinečně školení na jednotlivé zákazníky jednotlivých datových modelů. Ale nejdřív potřeba plodiny tyto modely z webové služby **Bike pronájem školení** . Přejděte zpět do **Bike pronájem školení** webové služby. Potřebujeme volání jeho koncového bodu BES 10krát s 10 datové sady různých školení k vytvoření 10 různých modelů. Použijeme rutiny prostředí PowerShell **InovkeAmlWebServiceBESEndpoint** akce.

Bude taky muset zadat pověření pro váš účet úložiště objektů blob do `$configContent`, a to, v poli `AccountName`, `AccountKey` a `RelativeLocation`. `AccountName` Může jít o některý z vašeho účtu názvy jak je vidět na **Portálu Správa klasické Azure** (karta*úložiště* ). Po kliknutí na účet úložiště jeho `AccountKey` najdete stisknutím tlačítka **Správa přístupových kláves** dole a kopírování *Primární klíč přístup*. `RelativeLocation` Je relativní cestu vzhledem k úložišti uložení nový model. Například cesta `hai/retrain/bike_rental/` skriptu pod odkazuje na kontejner s názvem `hai`, a `/retrain/bike_rental/` jsou podsložky. V současné době nejde vytvořit podsložky prostřednictvím portálu uživatelského rozhraní, ale existuje [Několik Průzkumník úložiště Azure](../storage/storage-explorers.md) , pomocí kterých můžete k tomu nevyzve. Je vhodné vytvořit nové kontejner v úložišti pro ukládání nových školení modelů (soubory .ilearner) takto: na stránce úložiště, klikněte na tlačítko **Přidat** dole a pojmenujte ho `retrain`. Souhrn potřebné změny, které skript pod příslušnými `AccountName`, `AccountKey` a `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Koncový bod BES je podporované režim jen pro tuto operaci. Záznamy nelze použít pro vytváření školení modely.

Jak vidíte výše místo stavba 10 různých BES úlohy konfigurace json souborů dynamicky místo vytvoření řetězce config jsme kanálu *jobConfigString* parametru rutinu **InvokeAmlWebServceBESEndpoint** , protože je ale skutečně nemusíte uchovávat kopie na disku.

V případě všechno dobře, po chvíli byste měli vidět 10 .ilearner soubory z *model001.ilearner* *model010.ilearner*ve vašem účtu Azure úložiště. Teď můžeme začít aktualizovat tyto modely pomocí rutiny prostředí PowerShell **Oprava AmlWebServiceEndpoint** naše 10 bodování koncové body webové služby. Upozorňujeme, že znovu, jsme pouze oprava jiného než výchozího koncové body, které jsme programově dříve vytvořili.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

To by měla běžet velmi rychle. Po dokončení spuštění se úspěšně vytvořili jsme 10 prediktivní koncové body webové služby, každý obsahující školení modelu jedinečně školení na konkrétní datovou sadu pronájem umístění, všechny z jednoho školení testu. Ověření, můžete zkusit volání tyto koncové body pomocí rutiny **InvokeAmlWebServiceRRSEndpoint** jim poskytnete stejné vstupní data a očekávat a zobrazte tak různých předpovědí výsledky od modely vyškolení sadami různých školení.

## <a name="full-powershell-script"></a>Úplné skript Powershellu

Tady je seznam úplné zdrojového kódu:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
