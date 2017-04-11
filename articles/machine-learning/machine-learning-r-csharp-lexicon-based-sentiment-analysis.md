<properties 
    pageTitle="Lexikon na základě myšlenkou analýzy | Microsoft Azure" 
    description="Lexikon na základě myšlenkou analýzy" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Lexikon na základě myšlenkou analýzy 

Jak se měřit názorů uživatelů a postoje směrem k značky nebo témata v online sociálních sítích, jako je Facebook publikuje, tweety kontrolami, atd.? Analýza myšlenkou poskytuje metodu pro analýzu tyto otázky.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Obecně dvěma způsoby myšlenkou analýzy. Jednu používá algoritmu sledovaných učení a druhý lze považovat za závadou výukové. Algoritmus sledovaných výukové obecně vytvoří model klasifikace na velké s poznámkami souhrnu. Jeho přesnost je hlavně na základě kvality poznámky a obvykle školení bude trvat dlouho. Kromě toho při použijeme algoritmu na jinou doménu, bude výsledek není obvykle dobrý. Ve srovnání s kontrolována učení na základě lexikon závadou výukové použití myšlenkou slovník, který nevyžaduje ukládání souhrnu velkých dat a školení: Díky celého procesu rychleji. 

Naší [služby](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) jsou založeny na míru lexikon MPQA (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), což je jedno z nejčastěji používaných míru lexikon. V MPQA jsou 5097 kladný a 2533 kladná slova. A všechna tato slova jsou označena jako velký nebo malý polarita. Celý souhrnu je klikněte v části Obecné GNU veřejné licence. Webová služba se dají použít u všech krátké vět, například tweety a Facebooku příspěvky. 

>Tato webová služba může být využívané uživatelů – potenciálně prostřednictvím mobilní aplikaci, prostřednictvím webu nebo dokonce v místním počítači například. Ale webové služby má taky sloužit jako příklad použití Azure počítače výuka k vytvoření webovým službám nad R kód. S několika řádky kódu R a kliknutími tlačítka v Azure počítače výukové Studio můžete pokus vytvořené pomocí kódu R a publikován jako webové služby. Webová služba můžete pak publikované na webu Azure Marketplace a spotřebované množství uživatelům a zařízením opačném konci světa se žádné nastavení infrastruktury podle autora webové služby.

##<a name="consumption-of-web-service"></a>Využití webové služby

Zadávání dat může být text, ale webová služba funguje lépe s krátké věty. Výstup je číselnou hodnotu mezi -1 a 1. Každá hodnota pod 0 označuje, že je záporné; myšlenkou textu kladné-li větší než 0. Absolutní hodnota atributu výsledku označuje: Zkontrolujte sílu přidružené myšlenkou. 

>Tuto službu hostované na webu Azure Marketplace, je službou OData. Tyto můžou volat prostřednictvím příspěvku nebo získat metody. 

Existuje několik možností, jak využívající služby Automatická způsobem (aplikace příkladu je [tady](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Počáteční C# kód pro webové služby spotřebu:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Zadání je "dnes Dobrý den." Výstup je "1", která označuje kladné myšlenkou přidružený k zadávání věty. 

##<a name="creation-of-web-service"></a>Vytvoření webové služby
>Tato webová služba byl vytvořený pomocí výukové počítače Azure. Bezplatná zkušební verze, jakož i úvodní videa týkající se vytváření pokusy a [publikování webové služby](machine-learning-publish-a-machine-learning-web-service.md)najdete v článku [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky s testu vytvořený kód webové služby a příklady pro jednotlivé moduly v testu.


Z v rámci Azure počítače studium nové prázdné experiment byl vytvořen. Následující obrázek znázorňuje experiment tok na základě lexikon myšlenkou analýzy. Soubor "sent_dict.csv" je lexikon míru MPQA a je nastavena jako vstupů [Spustit skript R][execute-r-script]. Jiné vstup je vybraných revize z datové sady Amazon revize pro test, kde provádět výběr, změny název sloupce jsme rozdělení operace. Používáme balíčku hash ukládat lexikon míru v paměti a zrychlení výpočtu skóre obrázku. Celý text bude tokenizovaný balíčkem "tm" a ve srovnání s slovo do slovníku myšlenkou. Nakonec skóre vypočítá přidáním tloušťky každého subjektivní slova v textu. 

###<a name="experiment-flow"></a>Experiment tok:

![Vyzkoušejte toku][2]


####<a name="module-1"></a>Modul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Instalace hash balíčku
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Omezení

Z pohledu algoritmus na základě lexikon myšlenkou analýza je obecné myšlenkou analytický nástroj, který nemusí vyšší výkon než metoda vyhodnocení konkrétních polí. Zápor problém není dobře řešit. Jsme napevno do kódu několik zápor slova náš program, ale lepší způsob pomocí zápor slovník a vytvořit některá pravidla. Webová služba provádí lépe krátké a jednoduché vět, například tweety a příspěvků na Facebooku, než na dlouhý a složitý věty například Amazon recenze. 

##<a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
Nejčastější dotazy na spotřebu webové služby nebo publikování na webu Azure Marketplace, najdete v článku [tady](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
