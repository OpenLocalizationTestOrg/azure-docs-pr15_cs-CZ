<properties
    pageTitle="Retrain existující prediktivní webové službě | Microsoft Azure"
    description="Zjistěte, jak retrain modelu a aktualizovat webové služby na používání nově školení modelu Azure počítač přečíst."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Retrain existující prediktivní webové služby

Tento dokument popisuje rekvalifikaci proces následujících situacích:

* Máte experiment školení a prediktivní testu, který jste nainstalovali jako operationalized webové služby.
* Máte nová data má prediktivní webová služba můžete provádět, jeho hodnocení.

Začnete s existující webové služby a pokusy, musíte tyto kroky:

1. Aktualizujte modelu.
    1. Úprava experiment školení pro webové služby vstupů a výstupů.
    2. Nasazení experiment školení jako rekvalifikaci webové služby.
    3. Umožňuje retrain modelu experiment školení dávku spuštění služby (BES).
2. Pomocí rutin prostředí PowerShell výukové počítače Azure aktualizovat prediktivní testu.
    1.  Přihlaste se ke svému účtu správce prostředků Azure.
    2.  Pokud potřebujete definice webové služby.
    3.  Exportujte definici webu služby formátu JSON.
    4.  Aktualizujte odkaz na objektů blob ilearner v ve formátu JSON.
    5.  Naimportujte ve formátu JSON definici webové služby.
    6.  Aktualizujte webové služby s novou definici webové služby.

## <a name="deploy-the-training-experiment"></a>Nasazení experiment školení

Nasazení experiment školení jako rekvalifikaci webové služby, musíte přidat webové služby vstupů a výstupů do modelu. Připojením modulu *Výstup webové služby* pokus * [Vlaku modelu] [ train-model] * modulu povolíte experiment školení k vytvoření nové školení modelu, který můžete použít v prediktivní testu. Pokud máte modul *Vyhodnocení Model* , můžete také připojit webové služby výstup k získání výsledků hodnocení jako výstup.

Aktualizace experiment školení:

1. Připojení *Webové služby vstupní* modul k zadávání dat (třeba *Čisté chybějící Data* modul). Obvykle Chcete-li zajistit, že zadávání dat zpracovávají stejným způsobem jako původní data školení.
2. Připojení *Webové služby výstup* modulu do výstupu modulu *Vlaku modelu* .
3. Pokud máte modul *Vyhodnocení modelu* a chcete výstup výsledků hodnocení, připojení *Webové služby výstup* modulu do výstupu modulu *Vyhodnocení modelu* .

Spusťte svůj testu.

Pak zaveďte experiment školení jako webová služba vypočítávající modelu školení a model hodnocení výsledků.  

V dolní části experiment plátna klikněte na **Nastavení webové služby**a vyberte **Nasazení [New] webové služby**. Na portálu Azure počítače výukové webovým službám se otevře stránka **Nasazení webové služby** . Zadejte název pro webové služby, zvolte plán plateb a pak klikněte na **nasazení**. Spuštění dávky metody můžete použít jenom pro vytváření školení modelů.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Retrain modelu pomocí nových dat pomocí BES

V tomto příkladu Pracujeme s programem C# vytvořit rekvalifikaci aplikaci. Můžete taky Python nebo R ukázkový kód k provedení tohoto úkolu.

Chcete-li zavolat rekvalifikaci rozhraní API:

1. Vytvoření aplikace konzoly C# ve Visual Studiu (**Nový** > **projektu** > **Počítač s Windows** > **Aplikace konzoly**).
2.  Přihlaste se k portálu počítače výukové webové služby.
3.  Klikněte na webové služby, které pracujete.
2.  Klikněte na **používat**.
3.  V dolní části stránky **spotřebě** , v části **Ukázka kód** klikněte na **dávku**.
5.  Zkopírujte ukázková C# kód pro spuštění dávky a vložit ho do souboru Program.cs. Ujistěte se, že oboru, zůstane nedotčený.

Přidáte do NuGet Microsoft.AspNet.WebApi.Client, jak je uvedeno v komentářích. Pokud chcete přidat odkaz na Microsoft.WindowsAzure.Storage.dll, může musíte nejdřív nainstalovat [knihovny klienta služby Azure úložiště](https://www.nuget.org/packages/WindowsAzure.Storage).

Následující obrázek ukazuje stránce **spotřebě** na portálu Azure počítače výukové webové služby.

![Používání stránky][1]

### <a name="update-the-apikey-declaration"></a>Aktualizace apikey deklarace

Vyhledejte deklaraci **apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

V části **základní spotřeby informace** stránce **spotřebě** vyhledejte primárního klíče a jeho zkopírování do **apikey** deklarace.

### <a name="update-the-azure-storage-information"></a>Aktualizovat informace o úložišti Azure

Ukázkový kód BES nahraje souboru z místní disk (třeba "C:\temp\CensusIpnput.csv") k základnímu úložišti Azure, zpracuje ji a data zapisuje výsledků zpět k základnímu úložišti Azure.  

Aktualizovat informace o úložišti Azure, musíte získat název účtu úložiště, klíče a informací o kontejneru účtu úložiště z portálu Microsoft Azure klasické a potom by měl být stejný tento aktualizace correspondi po spuštění testu, výsledný pracovního postupu:

![Po spuštění výsledné pracovního postupu][4]NG hodnoty v kódu.

1. Přihlaste se k portálu Azure klasické.
1. Ve sloupci levém navigačním panelu klikněte na **úložiště**.
1. Ze seznamu úložiště účtů vyberte skupinu pro ukládání retrained modelu.
1. V dolní části stránky klikněte na **Správa přístupových kláves**.
1. Zkopírujte a **Primární klíč přístup** uložit a zavřete dialogové okno.
1. V horní části stránky klikněte na **kontejnery**.
1. Vyberte kontejneru stávající nebo vytvořte nový účet a uložte název.

Vyhledejte *StorageAccountName* *StorageAccountKey*a *StorageContainerName* prohlášení a aktualizovat hodnoty, které jste uložili z portálu Microsoft klasické.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Taky musíte se ujistit, že vstupní soubor je k dispozici v místě, které zadáte kód.

### <a name="specify-the-output-location"></a>Zadejte umístění výstupu

Při zadávání umístění výstupu v datové požádat o příponu souboru, který je uveden v *RelativeLocation* musí být zadán jako `ilearner`. Naleznete v následujícím příkladu:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Následuje příklad rekvalifikaci výstup: ![přeškolení výstup][6]

## <a name="evaluate-the-retraining-results"></a>Vyhodnocení rekvalifikaci výsledků

Při spuštění aplikace zahrnuje adresy URL a sdílené přístupový token podpisy, které jsou potřebné pro přístup k výsledků hodnocení.

Uvidíte výsledky retrained modelu součtem *BaseLocation*, *RelativeLocation*a *SasBlobToken* ve výsledcích výstup pro *output2* (jak ukazuje předchozí obrázek rekvalifikaci výstup) a vložte úplnou adresu URL do adresního řádku prohlížeče.  

Zkontrolujte výsledky a zjistit, zda nově školení modelu provádí dostatečném nahradit stávající.

Zkopírujte *BaseLocation*, *RelativeLocation*a *SasBlobToken* ve výsledcích výstupu.

## <a name="retrain-the-web-service"></a>Retrain webové služby

Při retrain nové webové služby aktualizujte definici prediktivní webové služby neodkazuje nový model školení. Definice webové služby vnitřní reprezentaci školení modelu webová služba ani přímo měnit. Ujistěte se, že dostáváte definice webové služby prediktivní testu a není váš školení testu.

## <a name="sign-in-to-azure-resource-manager"></a>Přihlaste se ke Správci Azure zdroje

Musíte prvním přihlášení k účtu Azure z prostředí PowerShell pomocí rutiny [AzureRmAccount přidat](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Získání objektu definice webové služby

Dál potřebujete objekt definice webové služby tak, že zavoláte rutinu [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

A zjistit název skupiny zdroje stávající webové služby, spusťte rutinu Get-AzureRmMlWebService bez parametrů zobrazíte webové služby předplatného. Vyhledejte webové služby a podívejte se na jeho identifikátorem webové služby. Název skupiny zdrojů je čtvrtý element do pole ID, hned za *resourceGroups* element. V následujícím příkladu je název skupiny prostředků výchozí MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Můžete taky a zjistit název skupiny zdroje stávající webové služby, přihlaste se k portálu Azure počítače výukové webové služby. Vyberte webová služba. Název skupiny zdroje je element pátý adresu URL webové služby hned za *resourceGroups* element. V následujícím příkladu je název skupiny prostředků výchozí MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Export objektu webové služby definice jako JSON

Upravíte definici školení modelu použití nově školení modelu, musíte se nejdřív pomocí rutiny [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) exportovat do souboru ve formátu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Aktualizace odkazu na objektů blob ilearner

V aktiva vyhledejte [školení model], aktualizovat *uri* hodnotu v *locationInfo* uzel s URI objektů blob ilearner. Identifikátor URI je generováno aplikací součtem *BaseLocation* a *RelativeLocation* z výstupu BES přeškolení volání.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Naimportujte ve formátu JSON objekt definice webové služby

Musíte použít rutinu [AzureRmMlWebService importovat](https://msdn.microsoft.com/library/azure/mt767925.aspx) soubor můžete převést změněné JSON zpět definice webové služby objekt, který slouží k aktualizaci predicative testu.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Aktualizovat webové služby

Nakonec rutina [Aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) aktualizovat prediktivní testu.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
