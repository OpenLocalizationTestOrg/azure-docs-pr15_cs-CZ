<properties 
    pageTitle="Normální rozdělení webové služby sadu | Microsoft Azure" 
    description="Normální rozdělení Web služby Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Sadu směrodatnou odchylku.



Sadu normálního rozdělení je sada ukázka webové služby ([Generátor]( https://datamarket.azure.com/dataset/aml_labs/ndg7) [Quantile kalkulačky]( https://datamarket.azure.com/dataset/aml_labs/ndq5) [Pravděpodobnost kalkulačky]( https://datamarket.azure.com/dataset/aml_labs/ndp5)), které usnadnit generování a zpracování normálního rozdělení. Služby umožňují vytvářet posloupnost směrodatnou odchylku jakékoli délky výpočtu quantiles z dané pravděpodobností a výpočty pravděpodobnosti z dané quantile. Každá z těchto služeb posílá různých výstupy podle vybrané služby (viz popis níže). Sadu směrodatnou odchylku vychází z R funkce qnorm rnorm a pnorm, které jsou součástí R stat balíčku.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.  
 

##<a name="consumption-of-web-service"></a>Využití webové služby
Sadu směrodatnou odchylku zahrnuje následující 3 služby.

###<a name="normal-distribution-quantile-calculator"></a>Kalkulačka Quantile směrodatnou odchylku.
Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené quantile.

Zadání argumentů:

* p – jeden pravděpodobností událost s normálním rozdělením. 
* Průměr – střední směrodatnou odchylku.
* SS – směrodatná odchylka směrodatnou odchylku. 
* Boční – L pro dolní straně rozdělení a od U horním rozdělení.

Výstup služby je počítaná quantile přidružené k dané pravděpodobnost.

###<a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti Kalkulačka
Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá pravděpodobnost přidružená.

Zadání argumentů:

* otázky – jeden quantile událost se směrodatnou odchylku. 
* Průměr – střední směrodatnou odchylku.
* SS – směrodatná odchylka směrodatnou odchylku. 
* Boční – L pro dolní straně rozdělení a od U horním rozdělení.

Výstup služby je počítaná pravděpodobnost, že je přidružená k dané quantile.

###<a name="normal-distribution-generator"></a>Generátor směrodatnou odchylku.
Tato služba přijímá 3 argumenty směrodatnou odchylku a vygeneruje náhodné posloupnosti čísel, která jsou normální rozdělení. Následující argumenty stanovit k němu v rámci žádosti:

* n - počet pozorování. 
* průměr – střední směrodatnou odchylku.
* ss – směrodatná odchylka směrodatnou odchylku. 

Výstup služby je posloupnost délka n s normálním rozdělením založená na argumentech střední hodnotu a ss.

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje více možností, jak využívající služby Automatická způsobem (příklad aplikace jsou tady: [Generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx) [Pravděpodobnost kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx) [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

###<a name="normal-distribution-quantile-calculator"></a>Kalkulačka Quantile směrodatnou odchylku.
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti Kalkulačka
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Generátor směrodatnou odchylku.
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


##<a name="creation-of-web-service"></a>Vytvoření webové služby 
>Tato webová služba byl vytvořený pomocí výukové počítače Azure. Bezplatná zkušební verze, jakož i úvodní videa týkající se vytváření pokusy a [publikování webové služby](machine-learning-publish-a-machine-learning-web-service.md)najdete v článku [azure.com/ml](http://azure.com/ml). 

Níže je snímek obrazovky s testu vytvořený kód webové služby a příklady pro jednotlivé moduly v testu.

###<a name="normal-distribution-quantile-calculator"></a>Kalkulačka Quantile směrodatnou odchylku.

Experiment tok:

![Experiment tok][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti Kalkulačka
Experiment tok:

![Experiment tok][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Generátor směrodatnou odchylku.
Experiment tok:

![Experiment tok][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Omezení 

Zde je velmi jednoduché příkladů kolem normálnímu rozdělení. Jak je vidět z výše uvedené příklady kódu, malé zachycení chyby je implementovaná.

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
