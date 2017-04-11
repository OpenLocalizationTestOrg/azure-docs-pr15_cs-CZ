<properties
    pageTitle="Naučte se spravovat AzureML webové služby pomocí rozhraní API správy | Microsoft Azure"
    description="Příručka znázorňující, jak spravovat AzureML webové služby pomocí rozhraní API správy."
    keywords="výuka, Správa rozhraní api pro počítač"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Naučte se spravovat AzureML webové služby pomocí rozhraní API správy

##<a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak rychle začít používat správu rozhraní API pro správu AzureML webové služby.

##<a name="what-is-azure-api-management"></a>Co je Správa rozhraní API Azure?

Správa Azure rozhraní API je Azure služba, která umožňuje spravovat koncové body rozhraní REST API po definování přístupu uživatelů omezení použití a sledování řídicího panelu. Klikněte na [zde](https://azure.microsoft.com/services/api-management/) podrobné informace o správě rozhraní API Azure. Klikněte na [zde](../api-management/api-management-get-started.md) průvodce o tom, jak začít používat správu rozhraní API Azure. Tato další příručka, která tato příručka je založena na, zahrnuje další témata, včetně konfigurace oznámení osy ceny, zpracování odpovědí, ověřování uživatelů vytváření produkty, vývojář předplatné a použití dashboarding.

##<a name="what-is-azureml"></a>Co je AzureML?

AzureML je služby Azure pro počítač výukové, který umožňuje snadno vytvářet, nasazení a sdílet pokročilé technologie pro analýzu řešení. Klikněte na [zde](https://azure.microsoft.com/services/machine-learning/) podrobné informace o AzureML.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tato příručka, je potřeba k provedení:

* Účet Azure. Pokud nemáte účet Azure, klikněte na [zde](https://azure.microsoft.com/pricing/free-trial/) podrobné informace o tom, jak vytvořit účet bezplatnou zkušební verzi.
* Účet AzureML. Pokud nemáte účet AzureML, klikněte na [zde](https://studio.azureml.net/) podrobné informace o tom, jak vytvořit účet bezplatnou zkušební verzi.
* Pracovní prostor, přihlašovacích údajů a api_keya pokus AzureML nasazený jako webové služby. Klikněte na [zde](machine-learning-create-experiment.md) podrobné informace o tom, jak vytvořit pokus AzureML. Klikněte na [zde](machine-learning-publish-a-machine-learning-web-service.md) podrobné informace o tom, jak nasazení pokus AzureML jako webové služby. Dodatek A případně obsahuje pokyny, jak vytvořit a testování simple pokusy AzureML nasadit jako webové služby.

##<a name="create-an-api-management-instance"></a>Vytvoření instance rozhraní API správy

Níže jsou uvedeny kroky pro použití rozhraní API správy ke správě AzureML webové služby. Nejdřív vytvořte instanci služby. Přihlaste se k [Portálu klasické](https://manage.windowsazure.com/) a klikněte na **Nový** > **Aplikace služby** > **Rozhraní API správy** > **vytvořit**.

![vytvořit instanci](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Zadejte jedinečný **adresu URL**. Tato příručka používá **demoazureml** – budete muset zvolte něco jiného. Vyberte požadovanou **oblast** a **předplatného** pro instanci služby. Po provedení požadované hodnoty, klikněte na tlačítko Další.

![vytvoření 1 služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Zadání hodnoty **Název organizace**. Tato příručka používá **demoazureml** – budete muset zvolte něco jiného. Zadejte e-mailovou adresu v poli **e-mail správce** . Tento e-mailovou adresu se používá pro upozornění systému správy rozhraní API.

![Vytvoření 2 služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Zaškrtněte políčko pro vytvoření instance služby. *Trvá až půl hodiny pro novou službu vytvořit*.

##<a name="create-the-api"></a>Vytvoření rozhraní API

Po vytvoření instance služby dalším krokem je vytvoření rozhraní API. Rozhraní API se skládá ze sady operacích, které lze zavolat a od klientské aplikaci. Rozhraní API operace jsou servery proxy stávající webové služby. Tato příručka vytvoří rozhraní API tento proxy stávající záznamy AzureML a BES webovým službám.

Rozhraní API vytváří a konfigurovat z portálu Publisheru rozhraní API, který je prostřednictvím portálu klasické Azure. Přístup na portál Publisheru, vyberte instanci služby.

![Vyberte instanci služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě.

![Správa služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** a klikněte na **Přidat rozhraní API**.

![rozhraní API správy nabídka](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Jako **název rozhraní API webových**zadejte **AzureML ukázku API** . Zadejte **https://ussouthcentral.services.azureml.net** jako **Adresa URL webové služby**. Zadejte **azureml ukázku** **přípona rozhraní API webových adres URL**. Zaškrtněte políčko **HTTPS** jako **Adresa URL webu rozhraní API** schéma. Vyberte **Starter** jako **produkty**. Až budete hotovi, klikněte na **Uložit** vytvořte rozhraní API.

![Přidání – nové-rozhraní api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Přidání akcí

Klikněte na **Přidat operace** a přidejte k této rozhraní API operace.

![Přidání operace](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Zobrazí se okno **Nová operace** a na kartu **podpis** budou vybrané ve výchozím nastavení.

##<a name="add-rrs-operation"></a>Přidání záznamů operace

Nejdřív vytvořte operaci pro službu AzureML záznamy. Vyberte **Publikovat** jako **HTTP operace**. Typ **/services/ /workspaces/ {pracovního prostoru} {služby} / execute?api verze = {apiversion} & Podrobnosti = {Podrobnosti}** jako **Adresa URL šablony**. Jako **Zobrazovaný název**zadejte **Spustit záznamy** .

![Přidání záznamů operace podpisu](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klikněte na tlačítko **odpovědi** > **Přidat** na levé a vyberte **200 OK**. Klepněte na tlačítko **Uložit** uložte operaci.

![Přidání záznamů operace odpovědi](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>Přidání BES operace

Snímky obrazovek neplatí pro operace BES jsou velmi podobné těm, které pro přidávání operaci záznamy.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Odeslání (ale nespouštět) spuštění dávky projektu

Klikněte na **Přidat operace** přidáte operaci AzureML BES rozhraní API. Vyberte **Publikovat** pro **Akce HTTP**. Typ **/services/ /workspaces/ {pracovního prostoru} {služby} / jobs?api verze = {apiversion}** **Adresa URL šablony**. Zadejte **zobrazované jméno** **BES odeslat** . Klikněte na tlačítko **odpovědi** > **Přidat** na levé a vyberte **200 OK**. Klepněte na tlačítko **Uložit** uložte operaci.

###<a name="start-a-batch-execution-job"></a>Spusťte dávku spuštění úlohy

Klikněte na **Přidat operace** přidáte operaci AzureML BES rozhraní API. Vyberte **Publikovat** pro **Akce HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {pracovního prostoru} {jobid} / start?api verze = {apiversion}** **Adresa URL šablony**. Zadejte **zobrazované jméno** **BES Start** . Klikněte na tlačítko **odpovědi** > **Přidat** na levé a vyberte **200 OK**. Klepněte na tlačítko **Uložit** uložte operaci.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Získat stav nebo výsledek dávku spuštění úlohy

Klikněte na **Přidat operace** přidáte operaci AzureML BES rozhraní API. Vyberte **získat** pro **Akce HTTP**. Typ **/workspaces/ {pracovního prostoru} /services/ {služby} /jobs/ {jobid} ?api verze = {apiversion}** **Adresa URL šablony**. **Zobrazovaný název**zadejte **BES stavu** . Klikněte na tlačítko **odpovědi** > **Přidat** na levé a vyberte **200 OK**. Klepněte na tlačítko **Uložit** uložte operaci.

###<a name="delete-a-batch-execution-job"></a>Odstranění dávky spuštění úlohy

Klikněte na **Přidat operace** přidáte operaci AzureML BES rozhraní API. **Příkaz HTTP**zaškrtněte políčko **Odstranit** . Typ **/workspaces/ {pracovního prostoru} /services/ {služby} /jobs/ {jobid} ?api verze = {apiversion}** **Adresa URL šablony**. Zadejte **zobrazované jméno** **BES odstranit** . Klikněte na tlačítko **odpovědi** > **Přidat** na levé a vyberte **200 OK**. Klepněte na tlačítko **Uložit** uložte operaci.

##<a name="call-an-operation-from-the-developer-portal"></a>Zavolat operaci z portálu pro vývojáře

Operace můžete volat přímo na portálu pro vývojáře, která poskytuje pohodlný způsob, jak prohlédněte si a otestujte operace rozhraní API. V tomto kroku průvodce bude **Spouštět RR** metodu zavoláte přidaný do rozhraní **API ukázku AzureML**. V nabídce nahoře klikněte na **portál pro vývojáře** napravo od klasické portálu.

![Karta Vývojář v portálu](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Horní v nabídce klikněte na **rozhraní API** a klikněte na **AzureML ukázku API** zobrazíte operace dostupné.

![rozhraní api demoazureml](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Vyberte **Spustit záznamy** pro operaci. Klikněte na **vyzkoušet**.

![Zkuste it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Parametry požadavku zadejte svůj **pracovní prostor** **služby**, **2.0** **apiversion**a **true** **Podrobnosti**. **Pracovní prostor** a **službách** můžete najít v řídicím panelu AzureML webové služby (viz **Test webové služby** v dodatku A).

Žádost o záhlaví, klikněte na **Přidat záhlaví** **Typu obsahu** a **aplikace/json**, a pak klikněte na **Přidat záhlaví** a zadejte zadejte **se tak mohli ověřovat** a **nosný <YOUR AZUREML SERVICE API-KEY> **. Vyhledání kódu **klíč rozhraní api** v řídicím panelu AzureML webové služby (viz **Test webové služby** v dodatku A).

Typ **{"Vstupů": {"input1": {"ColumnNames": ["Sloupec2"], "hodnoty": [["Toto je dobrý den"]]}}, "GlobalParameters": {}}** pro požadavku.

![rozhraní api azureml ukázku](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klikněte na **Odeslat**.

![odeslání](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Až vyvolání operaci portálu pro vývojáře zobrazí **Požadavku na adresu URL** z službu back-end, **Stav odpovědi**, **hlavičky odpovědi**a veškerý **obsah odpověď**.

![Stav odpovědi](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Dodatek A – vytvoření a testování jednoduché AzureML webové služby

###<a name="creating-the-experiment"></a>Vytváření testu

V následující tabulce jsou kroky pro vytváření simple pokusy AzureML a nasazení jako webové služby. Web služby přijímá, zadávání sloupce libovolného textu a vrátí sadu funkcí vyjádřených jako celá čísla. Příklad:

Text | Hash textu
--- | ---
Toto je dobrý den | 1, 1, 2, 2, 0, 2, 0, 1

Nejdřív pomocí prohlížeče podle svého výběru přejděte: [https://studio.azureml.net/](https://studio.azureml.net/) a zadejte svoje přihlašovací údaje k přihlášení. Dále vytvořte nový prázdný testu.

![hledání experiment šablon](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Přejmenujte ho na **SimpleFeatureHashingExperiment**. Rozbalte položku **Uložené datové sady** a přetáhněte **Recenzí z Amazon** na vaše testu.

![jednoduché – funkce-algoritmus hash testu](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Rozbalte **Transformace dat** a **manipulace** a přetažením **Vybrat sloupce v sadě dat** na vaše testu. Připojte **recenzí z Amazon** vyberte **sloupce v datové sady**.

![Výběr sloupce](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klikněte na **Vybrat sloupce v sadě dat** a klikněte na **Spustit volič sloupce** a vyberte **Sloupec2**. Klikněte na políčko použít tyto změny.

![Výběr sloupce](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Rozbalení **Textu analýzy** a přetáhněte **Algoritmus hash funkce** testu. Připojení **Vyberte sloupce v sadě dat** k **algoritmus hash funkce**.

![připojení sloupce projektu](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Zadejte hodnotu **3** pro **Hashing bitsize**. Tím vytvoříte 8 (23) sloupce.

![algoritmus hash bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Chcete v tomto okamžiku, klikněte na příkaz **Spustit** testování testu.

![spuštění](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Vytvoření webové služby

Nyní vytvořte webové služby. Rozbalte **Webové služby** a **vstupní** přetáhněte vaší experiment. **Vstupní** připojte k **algoritmus hash funkce**. **Výstup** taky přetažením vaší testu. **Výstup** se připojte k **algoritmus hash funkce**.

![výstup pro – funkce hash](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klikněte na **publikovat webové služby**.

![publikování webové služby](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Kliknutím na tlačítko **Ano** publikovat testu.

![Ano publikování](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Testování webové služby

Webové služby AzureML se skládá z RSS (žádostí a odpovědí služba) a koncové body BES (dávku spuštění služby). RSS trvá synchronní spuštění. BES je pro spuštění asynchronní úloh. Testování webové služby se zdrojem ukázkové Python níže, budete muset stáhnout a nainstalovat Azure SDK pro Python (viz: [Jak nainstalovat Python](../python-how-to-install.md)).

Taky musíte **pracovního prostoru**, **přihlašovacích údajů**a **api_keya** vaše experiment pro zdroji vzorku. Pracovní prostor a službách najdete po kliknutí na **Žádostí a odpovědí** nebo **Spuštění dávky** experiment v řídicím panelu webové služby.

![vyhledání pracovního prostoru a service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Najděte **api_keya** po kliknutí na pokus v řídicím panelu webové služby.

![Vyhledání rozhraní api klíče](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Koncový bod RR test

#####<a name="test-button"></a>Tlačítko Test

Snadný způsob, jak otestovat koncový bod rr je kliknutím na tlačítko **Test** na řídicím panelu webové služby.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Zadejte **Sloupec2** **Toto je dobrý den** . Klikněte na zaškrtnutí.

![zadání dat](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Zobrazí se něco jako

![Ukázkový výstup](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Ukázkový kód

Další způsob, jak otestujte vaše záznamy pochází z kód klienta. Pokud kliknete na **žádostí a odpovědí** na řídicím panelu a posuňte se dolů, zobrazí se ukázkový kód pro C# Python a R. Zobrazí se také syntaxe pozvánce na schůzku záznamy, včetně žádost URI, záhlaví a textu.

Tato příručka zobrazuje pracovní Python příklad. Musíte se pomocí **pracovního prostoru**, **přihlašovacích údajů**a **api_keya** vaše experiment jej upravit.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Koncový bod BES test
**Spuštění dávky** klikněte na řídicí panel a posuňte se dolů. Zobrazí se ukázkový kód pro C# Python a R. Zobrazí se také syntaxe BES žádosti o odeslání úlohy spuštění úlohy, získat stav nebo výsledky úlohy a odstranění úlohy.

Tato příručka zobrazuje pracovní Python příklad. Potřebujete změnit s **pracovního prostoru**, **přihlašovacích údajů**a **api_keya** vaše testu. Navíc budete muset změnit **název účtu úložiště**, **úložiště klíč účtu**a **název kontejneru úložiště**. Nakonec se potřebujete změnit umístění **vstupní** a **výstupní soubor**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
