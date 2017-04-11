<properties 
    pageTitle="Multivariační lineární regrese | Microsoft Azure" 
    description="Multivariační lineární regrese" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariační lineární regrese   
 

 
Předpokládejme, že máte datovou sadu a chcete rychle předpovídání závislé proměnné (y) pro jednotlivé uživatele (i) podle nezávislé proměnné. Lineární regrese je Oblíbené statistické postup používá pro tyto předpovědí. Závislé proměnné y je zde dosadí nepřetržitý hodnotu.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Jednoduchý scénář může být místo, kam se pokouší výzkumný předpovídání tloušťky jednotlivce (y) podle jejich výšku (x). Další rozšířené situace může být kde výzkumný obsahuje další informace o jednotlivých (například weight (váha), pohlaví, závodní) a pokusí se předpovídání tloušťky jednotlivé. Tato [Webová služba]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) vešel modelu lineární regrese k datům a uloží předpovídané cena (y) pro jednotlivá pole pozorování ve data.

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.  

##<a name="consumption-of-web-service"></a>Využití webové služby  
Tato webová služba vám bude radit předpovězené hodnoty závislé proměnné podle nezávislé proměnné jednotlivých pozorování. Webová služba očekává koncového uživatele na zadávání dat jako řetězci kde řádky jsou oddělené čárkou (,) a sloupců jsou oddělené středníkem (;). Webová služba očekává 1 řádku směrem a předpokládá, že v prvním sloupci být závislé proměnné. Příklad sadu může vypadat takto:

![Ukázková data][1]

Pozorování bez závislé proměnné by měl zadávání jako "NA" y. Data pro zadávání za výše uvedených datovou sadu by následující řetězec: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1 NEDEF; 3; 4". Výstup je předpovězené hodnotu všech řádků podle nezávislé proměnné. 

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Z v rámci Azure počítače studium nové prázdné experiment byl vytvořen a dvě [Spustit skript R] [ execute-r-script] moduly byly doplněné do pracovního prostoru. Tato webová služba spustí pokus Azure počítače učení s podkladového skript R. Tvoří 2 části tohoto experiment: definice schématu a školení modelu + bodování. První modul definuje očekávané strukturu vstupní datovou sadu, kde první proměnná představuje závislé proměnné a nezávisí zbývající proměnné. Druhý modul přizpůsobí obecný lineární regrese modelu doplňku pro vstupní data.  
  
![Experiment toku][3]

####<a name="module-1"></a>Modul 1:
 
####<a name="schema-definition"></a>Definice schématu  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Modul 2:
####<a name="lm-modeling"></a>Modelování LM   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Omezení
Toto je velmi jednoduché příklady několika lineární regrese webové služby. Jak je vidět z výše uvedené příklady kódu, je prováděn bez zachycení chyby a službě předpokládá, že všechno, co je nepřetržitý proměnná (bez kategorií funkcí povolené), jako služby pouze vstupů číselné hodnoty v čase vystavením této webové služby. Službu aktuálně zpracuje také velikost omezí data splatnosti žádostí a odpovědí povahy volání webové služby a skutečnosti, jestli modelu je právě vejdou pokaždé, když místo toho webové služby. 

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
