<properties 
    pageTitle="O analýzy pomocí výukové Azure počítači | Microsoft Azure" 
    description="Pravděpodobnost výskytu události analýzy o" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>O analýzy 

V mnoha případech hlavní výsledek v části hodnocení je čas na událost zájmu. Jinými slovy otázka "při této události dojde?" je dotaz. Jako příklad, zvažte situace, kdy data popisuje uplynulý čas (dny, roky, protokolem ujetých atd.) do události úrok (nákazy relapse, aplikace PhotoDraw stupeň dostali, selhání číselník brzdy) dojde. Jednotlivé instance v datech představuje na konkrétní objekt (pacienta student, auta, atd.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Tato [Webová služba]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) najdete odpovědi na otázky "co je pravděpodobnost, že událost potřebné nastane n času pro objekt x?" Zadáním modelu analýzy o tato webová služba umožňuje zadat datový model a otestovat. Hlavní motiv testu je model uplynulá doba, dokud potřebné události. 

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači, například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.  

##<a name="consumption-of-web-service"></a>Využití webové služby

V následující tabulce je zobrazen schématu vstupní data webové služby. Šest kusů informace jsou potřebné jako vstup: školení dat, testování data, času potřebné, rejstřík "time" dimenze, index "události" rozměry a typy proměnných (nepřetržitý nebo koeficient). Školení data představuje řetězcem, řádky jsou oddělené čárkou, kde jsou sloupce oddělte je středníkem. Počet funkcí dat je flexibilní. Všechny prvky ve vstupním řetězci musí být číslo. V okně data školení dimenzi "time" označuje, že počet časových jednotek (dny, roky, protokolem ujetých atd.), jež uplynuly od počátečního bodu studie (pacienta příjem obchodu pozornost programy, aplikace PhotoDraw studie počáteční student, Auto začali být řízené atd) do události úrok (pacienta návratu do obchodu použití student získání aplikace PhotoDraw stupeň auto s selhání brzdy číselník atd.) Probíhá. Dimenzi "událost" označuje, zda potřebné události na konci studie. Hodnotu "události = 1" znamená, že potřebné události v době označen dimenzi "time"; "události = 0" znamená, že události úrok nedošlo podle času označen dimenzi "time".

- trainingdata - řetězec znaků. Řádky jsou oddělené čárkou a sloupců jsou oddělené středníkem. Každý řádek obsahuje dimenze "time", "událost" dimenze a prognostických proměnných.
- testingdata – jeden řádek dat, která obsahuje prognostických proměnných pro konkrétní objekt.
- time_of_interest - uplynulý čas úrok n.
- index_time - indexu sloupce (počínaje 1) dimenze "time".
- index_event - indexu sloupce (počínaje 1) dimenze "událost".
- variable_types - řetězci znaků na znaky s středníky jako oddělovače v nich. Nepřetržitý proměnné představuje 0 a 1 představuje faktor proměnné.


Výstup je pravděpodobnost události výskytu určitou dobu. 

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Interpretace tento test vypadá takto. Za předpokladu, že cílem data, je model uplynulý čas do výnosové pacientů, kteří obdrželi sady dvou pozornost k použití obchodu. Výstup webové služby čtení: pro pacientů, přičemž 35 let, s předchozí obchod pozornost 2 krát trvá dlouho domácí pozornost program a využití heroinu i kokainu pravděpodobnost návratu do obchodu použití je 95.64 % den 500.

##<a name="creation-of-web-service"></a>Vytvoření webové služby

>Tato webová služba byl vytvořený pomocí výukové počítače Azure. Bezplatná zkušební verze, jakož i úvodní videa týkající se vytváření pokusy a [publikování webové služby](machine-learning-publish-a-machine-learning-web-service.md)najdete v článku [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky s testu vytvořený kód webové služby a příklady pro jednotlivé moduly v testu.

Z v rámci Azure počítače studium nové prázdné experiment byl vytvořen a dvě [Spustit skript R] [ execute-r-script] moduly byly doplněné do pracovního prostoru. Schéma dat byl vytvořený pomocí jednoduchého [Spustit skript R][execute-r-script], která definuje schéma vstupní data webové služby. Tento modul je pak propojen druhý [Spustit skript R] [ execute-r-script] modul, který hlavní práce. Tento modul znamená předzpracování dat, vytváření modelů a předpovědí. V kroku data předzpracování zadávání dat představované dlouhé řetězce transformovat a převedený snímek data. V seznamu krok stavební modelu balíčku externí R "survival_2.37 7.zip" první instalaci pro provádění analýzy o. Potom funkce "coxph" spouštět po zpracování dat úkoly řady. Podrobné informace o funkci "coxph" pro analýzu o může číst z dokumentace R. V kroku předpovědí testovací instance pochází do školení modelu pomocí funkce "surfit" a křivky o této testovací instance pochází jako "křivka" proměnná. Nakonec se získá pravděpodobnost času potřebné. 

###<a name="experiment-flow"></a>Experiment tok:

![Vyzkoušejte toku][1]

####<a name="module-1"></a>Modul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Modul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Omezení

Tato webová služba může trvat pouze číselné hodnoty jako funkce proměnné (sloupce). Sloupec "události" může trvat pouze hodnoty hodnot 0 a 1. Sloupec "time" musí být kladné celé číslo.

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
