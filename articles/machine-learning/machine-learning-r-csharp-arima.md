<properties 
    pageTitle="Prognózování: Autoregressive integrované přesunutí průměrnou hodnotu (ARIMA) | Microsoft Azure" 
    description="Prognózování - Autoregressive integrované přesunutí průměrnou hodnotu (ARIMA)" 
    services="machine-learning" 
    documentationCenter="" 
    authors="yijichen" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="yijichen"/> 

 
#<a name="forecasting---autoregressive-integrated-moving-average-arima"></a>Prognózování - Autoregressive integrované přesunutí průměrnou hodnotu (ARIMA)

Tato [Služba]( https://datamarket.azure.com/dataset/aml_labs/arima) používá Autoregressive integrované přesunutí průměr (ARIMA) k vytvoření předpovědí na základě historických dat poskytnutých uživatelem. Zvýší služba pro určitý produkt letos? Můžete se předpovídání Moje prodej produktů pro season vánoční tak, aby se bylo plánování Moje zásob? Prognózy modely jsou výstižný tyto otázky. Vzhledem dřívější data, zkontrolujte tyto modely skryté trendů a sezónnost odhadu budoucích trendy. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.

##<a name="consumption-of-web-service"></a>Využití webové služby 

Tato služba přijímá 4 argumenty a vypočítá ARIMA prognózy.
Zadání argumentů:

* Četnost - označuje četnost nezpracovanými daty (denně nebo týdně/měsíční nebo čtvrtletní/ročně).
* Horizont – budoucí prognózy časového rozvrhu.
* Datum: Přidání nového dat časové řady pro čas.
* Hodnota – přidání do nové hodnoty dat časové řady.

Výstup služby je počítané hodnoty prognózy. 

Ukázková vstupní může být: 

* Četnost - 12
* Horizont - 12
* Datum - 1/15/2012; 2/15/2012 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012; 9/15/2012 10/15/2012; 11/15/2012; 12/15/2012; 1/15/2013; 2/15/2013 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014; 2/15/2014 3/15/2014; 4/15/2014; 5/15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014
* Hodnoty – 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509
 
>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

    public class Input
    {
        public string frequency;
        public string horizon;
        public string date;
        public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
         byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
         return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

       
    void Main()
    {
        var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
        var json = JsonConvert.SerializeObject(input);
        var acitionUri =  "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
           
        var httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere","ChangeToAPIKey");
        httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        var query = httpClient.PostAsync(acitionUri,new StringContent(json));
        var result = query.Result.Content;
        var scoreResult = result.ReadAsStringAsync().Result;
    }

##<a name="creation-of-web-service"></a>Vytvoření webové služby 

>Tato webová služba byl vytvořený pomocí výukové počítače Azure. Bezplatná zkušební verze, jakož i úvodní videa týkající se vytváření pokusy a [publikování webové služby](machine-learning-publish-a-machine-learning-web-service.md)najdete v článku [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky s testu vytvořený kód webové služby a příklady pro jednotlivé moduly v testu.

Z v rámci Azure počítače studium nové prázdné experiment byl vytvořen. Ukázková vstupní data byl odeslán s schématem předdefinované data. Propojená s schéma dat je [Spustit skript R] [ execute-r-script] modul, který vytváří ARIMA Prognózování modelu pomocí funkcí "auto.arima" a "prognózy" z R. 

###<a name="experiment-flow"></a>Experiment toku:

![Vytvoření pracovního prostoru][2]

####<a name="module-1"></a>Modul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Vytvoření pracovního prostoru][3]  

####<a name="module-2"></a>Modul 2:
    # data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- auto.arima(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # produce forecasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # data output
    maml.mapOutputPort("data.forecast");


##<a name="limitations"></a>Omezení 

Toto je velmi jednoduché příklad pro prognózování ARIMA. Jak je vidět z výše uvedené příklady kódu, bez zachycení chyby prováděna a službě předpokládá, že všechny proměnné jsou nepřetržitý/kladné hodnoty a četnost by měl být celé číslo větší než 1. Délka vektory datum a hodnotu by měl být stejný. Proměnná Datum měli dodržovat formátu "mm/dd/rrrr".

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikováním na web marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
