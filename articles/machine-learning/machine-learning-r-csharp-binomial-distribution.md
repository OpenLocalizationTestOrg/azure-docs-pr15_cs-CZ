<properties 
    pageTitle="Sadu binomické rozdělení | Microsoft Azure" 
    description="Sadu binomického rozdělení." 
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


#<a name="binomial-distribution-suite"></a>Sadu binomického rozdělení.




Sadu binomické rozdělení je sada ukázka webové služby ([Binomického generátor](https://datamarket.azure.com/dataset/aml_labs/bdg5) [Pravděpodobnost kalkulačky]( https://datamarket.azure.com/dataset/aml_labs/bdp4) [Quantile kalkulačky]( https://datamarket.azure.com/dataset/aml_labs/bdq5)), které vyřešit generování a práce s binomického rozdělení. Služby umožňují vytvářet posloupnost binomické rozdělení jakékoli délky výpočtu quantiles mimo daném pravděpodobnost a výpočtu pravděpodobnosti z dané quantile. Každá z těchto služeb posílá různých výstupy podle vybrané služby (viz popis níže). Sadu binomické rozdělení vychází z R funkce qbinom rbinom a pbinom, které jsou součástí balíček stat R. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Webovým službám může být využívané uživatelů – potenciálně přímo na tržiště pomocí mobilní aplikace prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba pak můžete publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa – požaduje žádné nastavení infrastruktury podle autora webové služby.

##<a name="consumption-of-web-service"></a>Využití webové služby
Sadu binomické rozdělení zahrnuje následující 3 služby.

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulačka Quantile binomického rozdělení.
Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené quantile.
Zadání argumentů:

- p - typu single agregované pravděpodobnost více zkušebních verzích.  
- velikost – počet pokusů.
- funkce PROB - pravděpodobnost úspěchu ve zkušební verzi.
- Boční – L pro dolní straně rozdělení U horním rozdělení. 

Výstup služby je počítaná quantile přidružené k dané pravděpodobnost.

###<a name="binomial-distribution-probability-calculator"></a>Kalkulačka binomického rozdělení pravděpodobnosti
Tato služba přijímá 4 argumenty binomického rozdělení a vypočítá pravděpodobnost přidružená.
Zadání argumentů:

- otázky – jeden quantile události binomického rozdělení. 
- velikost – počet pokusů.
- funkce PROB - pravděpodobnost úspěchu ve zkušební verzi.
- boční – L pro dolní straně rozdělení U horním rohu rozdělení nebo E, která je rovno jednoho počet úspěšných pokusů.

Výstup služby je počítaná pravděpodobnost, že je přidružená k dané quantile.

###<a name="binomial-distribution-generator"></a>Generátor binomického rozdělení.
Tato služba přijímá 3 argumenty binomického rozdělení a vygeneruje náhodné posloupnosti čísel binomially distribuovaný. Následující argumenty stanovit k němu v rámci žádosti:

- n - počet pozorování. 
- velikost – počet pokusů.
- funkce PROB - pravděpodobnost úspěchu.

Výstup služby je posloupnost délka n s binomického rozdělení založená na argumentech velikost a prob.

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje více možností, jak využívající služby Automatická způsobem (příklad aplikace jsou tady: [Generátor](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx) [Pravděpodobnost kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx) [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulačka Quantile binomického rozdělení.
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Kalkulačka binomického rozdělení pravděpodobnosti
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Generátor binomického rozdělení.
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

>Tato webová služba byl vytvořený pomocí výukové počítače Azure. Bezplatná zkušební verze, jakož i úvodní videa týkající se vytváření pokusy a [publikování webové služby](machine-learning-publish-a-machine-learning-web-service.md)najdete v článku [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky s testu vytvořený kód webové služby a příklady pro jednotlivé moduly v testu.

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulačka Quantile binomického rozdělení.

![Vytvoření pracovního prostoru][4]

####<a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Modul 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Kalkulačka binomického rozdělení pravděpodobnosti

![Vytvoření pracovního prostoru][5]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Generátor binomického rozdělení.

![Vytvoření pracovního prostoru][6]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Omezení 
Zde je velmi jednoduché příkladů kolem binomického rozdělení. Jak je vidět z výše uvedené příklady kódu, malé zachycení chyby je implementovaná.

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
