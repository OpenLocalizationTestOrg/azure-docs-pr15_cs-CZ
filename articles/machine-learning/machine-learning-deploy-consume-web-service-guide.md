<properties
    pageTitle="Azure počítače výukové webovým službám: Nasazení a spotřeby | Microsoft Azure"
    description="Zdroje pro nasazení a jinými webové služby."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure počítače výukové webové služby: Nasazení a spotřeby

Můžete Azure počítače výukové nasazení pracovních postupů počítače učení s pomocí modely jako webové služby. Webovým službám lze potom zavolat modely počítače learningové z aplikace přes Internet dělat předpovědí v reálném čase nebo v režimu dávku. Protože webových služeb jsou RESTful, můžete jim zavolá z různých programovacím jazyce a platformy, třeba .NET a Java a od aplikací, jako je Excel.

Následující části uvádějí odkazy na podrobné pokyny, kód a si přečtěte následující dokumentaci k vám pomůžou začít pracovat.

## <a name="deploy-a-web-service"></a>Nasazení webové služby

### <a name="with-azure-machine-learning-studio"></a>S Azure počítače výukové Studio

Počítač výukové Studio portálu Microsoft Azure počítače výukové webovým službám usnadňují a nasazovat a spravovat webové služby bez psaní kódu.

Následující odkazy poskytují obecné informace o tom, jak implementovat nové webové služby:

* Základní informace o tom, jak nasaďte novou webové služby, které je založeno na správce prostředků Azure najdete v článku [nasazení nové webové služby](machine-learning-webservice-deploy-a-web-service.md).
* Informace o tom, jak nasazení webové služby najdete v článku [nasazení webové služby Azure počítače výukové](machine-learning-publish-a-machine-learning-web-service.md).
* Úplné informace o tom, jak vytvářet a publikovat webové služby, najdete v článku [návodu krok 1: vytvoření pracovního prostoru aplikace Groove počítače výukové](machine-learning-walkthrough-1-create-ml-workspace.md).
* Konkrétní příklady, které nasazení webové služby najdete v článku:

    * [Návod krok 5: Nasazení webové služby Azure počítače výuka](machine-learning-walkthrough-5-publish-web-service.md)
    * [Nasazení webové služby do více oblastí](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Pomocí webové služby zdroje poskytovatele rozhraní API Azure správce prostředků rozhraní API)

Zprostředkovatel Azure počítače výukové zdroje webové služby umožňuje nasazení a správu webové služby pomocí rozhraní REST API volání. Další informace najdete v článku Principy [Počítači výukové webové služby (nebo ZBÝVAJÍCÍ)](https://msdn.microsoft.com/library/azure/mt767538.aspx) na webu MSDN.

### <a name="with-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell

Azure počítače výukové zdroje zprostředkovatel webové služby umožňuje nasazení a správu webové služby pomocí rutin prostředí PowerShell.

Použití rutin musí prvním přihlášení k účtu Azure z prostředí PowerShell pomocí rutiny [AzureRmAccount přidat](https://msdn.microsoft.com/library/mt619267.aspx) . Pokud neznáte jak volat příkazy Powershellu, které jsou založeny na správce prostředků, najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Export prediktivní testu, použijte [Tento ukázkový kód](https://github.com/ritwik20/AzureML-WebServices). Po vytvoření souboru .exe od kódu můžete zadávat:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Spuštění aplikace vytvoří šablonu webu služby JSON. Použití šablony pro nasazení webové služby, musíte přidat následující informace:

* Název účtu úložiště a klíčem

    Název účtu úložiště a klíč můžete získat z [Azure portál](https://portal.azure.com/) nebo [Azure klasické portálu](http://manage.windowsazure.com/).
* ID potvrzení plánu

    ID plánu můžete získat z portálu Microsoft [Azure počítače výukové webovým službám](https://services.azureml.net) přihlášení a klepnutím na název plánu.

Přidáte do šablony JSON podřízených uzel *Vlastnosti* na stejné úrovni jako uzel *MachineLearningWorkspace* .

Tady je příklad:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Podívejte se na následující články a ukázkového kódu pro další podrobnosti:

* [Azure počítače výukové rutiny]( https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN
* Ukázka [návod](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) na GitHub

## <a name="consume-the-web-services"></a>Využívat webové služby

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Z Azure počítače výukové webové služby UI (testování)

Můžete otestovat webová služba z portálu Microsoft Azure počítače výukové webové služby. Platí to i pro testování odpovědi na žádosti o službu (záznamy) a rozhraní služby spuštění dávky (BES).

* [Nasazení nové webové služby](machine-learning-webservice-deploy-a-web-service.md)
* [Nasazení webové služby Azure počítače výuka](machine-learning-publish-a-machine-learning-web-service.md)
* [Návod krok 5: Nasazení webové služby Azure počítače výuka](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Z aplikace Excel

Můžete si stáhnout šablonu Excelu, který využívá webové služby:

* [Jinými webové služby Azure počítače výukové z aplikace Excel](machine-learning-consuming-from-excel.md)
* [Doplněk aplikace Excel Azure počítače výukové webové služby](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>Z klienta na základě REST

Azure počítače výukové webových služeb jsou RESTful rozhraní API. Můžete používat tyto rozhraní API různé platformy, třeba .NET Python, R, Java, atd. Stránka **spotřebě** webové služby na [portálu Microsoft Azure počítače výukové webovým službám](https://services.azureml.net) má ukázku kódu, který můžete vám pomůžou začít. Další informace najdete v článku [jak používat webové služby Azure výukové stroje, které byly nasazeny z počítače výukové testu](machine-learning-consume-web-services.md).

