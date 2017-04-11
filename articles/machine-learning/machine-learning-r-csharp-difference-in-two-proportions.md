<properties 
    pageTitle="Rozdíl v proporce otestujte | Microsoft Azure" 
    description="Rozdíl v otestujte poměr stran" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Rozdíl v otestujte poměr stran


Jsou dva proporce statistické různých? Předpokládejme, že chce uživatel porovnat dvě filmy chcete zjistit, zda jeden film výrazně vyšší podíl "to se mi líbí" při porovnání k druhému. Velké vzorek může být statistické velký rozdíl mezi proporce 0,50 a 0,51. Pomocí malého vzorového nemusí být dostatek dat chcete zjistit, zda jsou ve skutečnosti různých tyto poměr stran. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Tato [Webová služba]( https://datamarket.azure.com/dataset/aml_labs/prop_test) provádí testu hypotéz rozdílů ve dvou hodnot založených na vstupu uživatele počet úspěšných pokusů a celkový počet pokusů skupin 2 porovnání. V jedné možné scénáře tato webová služba nazvat z v aplikaci porovnání video o tom, zda nějaká videa je skutečně "dali lajk" dochvilnější spoje, než ostatní, na základě film hodnocení.

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.


##<a name="consumption-of-web-service"></a>Využití webové služby

Služba přijímá 4 argumenty a nemá hypotéz zkouška poměr stran.

Zadání argumentů:

* Successes1 - počet úspěšných událostí v ukázkové 1.
* Successes2 - počet úspěšných událostí v ukázkové 2.
* Total1 - velikost vzorku 1.
* Total2 - velikost vzorku 2.

Výstup služby je výsledkem hypotéz testování spolu s chi-square statistický SV, p hodnotu a deklarovaná čistá nebo hrubá v ukázkové 1/2 a interval spolehlivosti meze.

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Z v rámci Azure počítače studium nové prázdné experiment byl vytvořený pomocí dvou [Spustit skript R] [ execute-r-script] moduly. V první modulu, který je definován schéma dat při druhý modul používá příkaz prop.test ve R provádět test hypotéz 2 poměr stran. 


###<a name="experiment-flow"></a>Experiment tok:

![Experiment tok][2]


####<a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Modul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Omezení 

Toto je velmi jednoduché příklad test rozdíl v 2 poměr stran. Jak je vidět z výše uvedené příklady kódu, je prováděn bez zachycení chyby a službě předpokládá, že všechny proměnné nepřetržitý.

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
