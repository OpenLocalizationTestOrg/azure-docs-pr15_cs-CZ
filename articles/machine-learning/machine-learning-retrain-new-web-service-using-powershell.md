<properties
    pageTitle="Retrain nové webové služby pomocí rutin prostředí Management výukové počítače | Microsoft Azure"
    description="Zjistěte, jak programově retrain modelu a aktualizovat webové služby na používání nově školení modelu Azure počítač přečíst pomocí rutin prostředí Management výukové počítače."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Retrain nové webové služby pomocí rutin prostředí Management výukové počítače

Při retrain nové webové služby aktualizujte definici prediktivní webové služby neodkazuje nový model školení.  

## <a name="prerequisites"></a>Zjistit předpoklady pro

Musíte mít nastavit experiment školení a prediktivní experiment uvedeno v [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Prediktivní testu musí být nasazené jako správce prostředků Azure (Nový) na základě počítač výukové webové služby. 
 
Další informace o nasazení webové služby najdete v článku [nasazení webové služby Azure počítače výukové](machine-learning-publish-a-machine-learning-web-service.md).

Tento postup vyžaduje, že máte nainstalovanou rutiny výukové počítače Azure. Informace o instalaci rutiny výukové počítače najdete v článku Principy [Azure počítače výukové rutiny](https://msdn.microsoft.com/library/azure/mt767952.aspx) na webu MSDN.

Z výstupu rekvalifikaci zkopírovali následující informace:

* BaseLocation
* RelativeLocation

Kroky, kterými jsou:

1.  Přihlaste se ke svému účtu správce prostředků Azure.
2.  Získání definice webové služby
3.  Exportovat jako JSON definice webové služby
4.  Aktualizujte odkaz na objektů blob ilearner v ve formátu JSON.
5.  Naimportujte ve formátu JSON definici webové služby
6.  Aktualizovat webové služby s novou definici webové služby

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Přihlaste se ke svému účtu správce prostředků Azure

Musíte nejdřív přihlásit ke svému účtu Azure z prostředí PowerShell pomocí rutiny [AzureRmAccount přidat](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Získání definice webové služby

Dál potřebujete webová služba tak, že zavoláte rutinu [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . Definice webové služby vnitřní reprezentaci školení modelu webová služba ani přímo měnit. Ujistěte se, že se dostáváte definice webové služby prediktivní testu a není váš školení testu.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

A zjistit název skupiny zdroje stávající webové služby, spusťte rutinu Get-AzureRmMlWebService bez parametrů zobrazíte webové služby předplatného. Vyhledejte webové služby a podívejte se na jeho identifikátorem webové služby. Název skupiny zdrojů je čtvrtý element do pole ID, hned za *resourceGroups* element. V následujícím příkladu je název skupiny prostředků výchozí MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Můžete taky a zjistit název skupiny zdroje stávající webové služby, přihlaste se k portálu Microsoft Azure počítače výukové webové služby. Vyberte webová služba. Název skupiny zdroje je element pátý adresu URL webové služby hned za *resourceGroups* element. V následujícím příkladu je název skupiny prostředků výchozí MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportovat jako JSON definice webové služby

Chcete-li změnit definici školení modelu používat nově školení modelu, musíte se nejdřív pomocí rutiny [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) exportovat do souboru ve formátu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Aktualizujte odkaz na objektů blob ilearner v ve formátu JSON.

V aktiva vyhledejte [školení model], aktualizovat *uri* hodnotu v *locationInfo* uzel s URI objektů blob ilearner. Identifikátor URI je generováno aplikací součtem *BaseLocation* a *RelativeLocation* z výstupu BES přeškolení volání.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Naimportujte ve formátu JSON definici webové služby

Soubor můžete převést změněné JSON zpět definice webové služby, který slouží k aktualizaci testu Predicative musíte použít rutinu [AzureRmMlWebService importovat](https://msdn.microsoft.com/library/azure/mt767925.aspx) .

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Aktualizovat webové služby s novou definici webové služby

Nakonec [Aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) rutina slouží k aktualizaci prediktivní testu.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Souhrn

Rutiny pro správu Powershellu výukové počítače můžete aktualizovat školení modelu prediktivní webové služby podpora scénářů jako:

* Pravidelné model přeškolení novými daty.
* Rozdělení modelu zákazníkům s cílem dát retrain modelu pomocí svá data.
