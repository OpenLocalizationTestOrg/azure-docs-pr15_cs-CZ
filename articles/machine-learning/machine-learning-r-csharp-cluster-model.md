<properties 
    pageTitle="Clusteru modelu | Microsoft Azure" 
    description="Model clusteru" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Model clusteru    

Jak můžeme předpovídání skupiny platební služba chování Pokud chcete snížit riziko poplatků vypnout vydavatelů kreditních karet? Jak můžeme definovat skupiny pro osobou vlastností zaměstnanců Pokud chcete zvýšit výkon v práci? Jak lze lékařů klasifikovat pacientů do skupin podle vlastnosti jejich nemocí? Zásadně všechny tyto otázky můžete odpovědět pomocí analýzy obrázku.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Analýza clusteru klasifikuje sadu pozorování do dvou nebo více vzájemně se vylučujících Neznámý skupin na základě kombinací proměnné. Obrázku analýzy účel zjistit systém uspořádání pozorování obvykle lidí a jejich vlastnosti do skupin, kde členové skupiny sdílet vlastnosti společné. Tato [Služba](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) využívá prostředky K metodologie běžně používaných clusterů postup libovolných dat obrázku do skupin. Tato webová služba zabírá data a počet k clusterů předávat na vstupu a vytvoří předpovědí z nich k skupin, do kterých patří každý poznámky. 

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.  

##<a name="consumption-of-web-service"></a>Využití webové služby   
Tato webová služba seskupení dat do sady k skupin a uloží přiřazení skupiny pro každý řádek. Webová služba očekává koncového uživatele na zadávání dat jako řetězci kde řádky jsou oddělené čárkou (,) a sloupců jsou oddělené středníkem (;). Webová služba předpokládá, že řádek 1 najednou. Příklad sadu může vypadat takto:

![Ukázková data][1]

Předpokládejme, že uživatel chtěli rozdělení tato data do 3 vzájemně se vylučujících skupin. Vstupní za výše uvedených datovou sadu by následující data: hodnota = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Výstup je předpovídané členství ve skupinách jednotlivých řádcích.

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Z v rámci Azure počítače studium nové prázdné experiment byl vytvořen a dvě [Spustit skript R] [ execute-r-script] moduly doplněné do pracovního prostoru. Schéma dat byl vytvořený pomocí jednoduchého [Spustit skript R][execute-r-script]. Potom schéma dat odkazoval části model clusteru znova vytvořené pomocí [Skriptu R spustit][execute-r-script]. V části [Provést skript R] [ execute-r-script] použitý pro model clusteru, webové služby pak používá funkci "k: znamená", která je přednastavených do [Spustit skript R] [ execute-r-script] Azure počítače učení.    
   

     
![Experiment toku][3]

####<a name="module-1"></a>Modul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Omezení
Toto je velmi jednoduché příklad clusterů webové služby. Jak je vidět z výše uvedené příklady kódu, je prováděn bez zachycení chyby a službě předpokládá, že všechno, co je nepřetržitý proměnná (bez kategorií funkcí povolené), jako služby pouze vstupů číselné hodnoty v čase vystavením této webové služby. Službu aktuálně zpracuje také velikost omezí data splatnosti žádostí a odpovědí povahy volání webové služby a skutečnosti, jestli modelu je právě vejdou pokaždé, když místo toho webové služby. 

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
