<properties 
    pageTitle="Data pro výzkum na Linux dat pro výzkum virtuální počítač | Microsoft Azure" 
    description="Jak provést několik běžných úkolů dat pro výzkum pomocí OM vědy dat Linux." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Data pro výzkum na Linux dat pro výzkum virtuálního počítače

Tento návod ukazuje, jak můžou provádět několik běžných dat pro výzkum s OM vědy dat Linux. Virtuální počítač vědy dat Linux (DSVM) je k dispozici v Azure, která je předinstalovaný s sadu nástrojů běžně používané technologie pro analýzu dat a počítač výukové virtuálního počítače obrázek. Klíčové softwarových součástí jsou rozpis v tématu [poskytování Linux dat pro výzkum virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md) . Obrázek OM snadné začít pracovat způsobem dat pro výzkum v minutách, aniž by bylo nutné nainstalovat a nakonfigurovat jednotlivých nástrojích jednotlivě. Můžete snadno škálování OM, v případě potřeby a ho není při použití zastavit. Zdroj je vlastně pružná a efektivně náklady. 

Úkoly vědy dat prokázáno v tomto návodu sledovat kroků uvedených v [Týmu dat pro výzkum obrázku](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Tento proces poskytuje systematický přístup k jiné vědecké dat, které týmům dat vědeckých efektivně spolupracovat přes životním cyklu sestavování inteligentní aplikace. Proces pro výzkum data také poskytuje iterativních framework pro výzkum dat, který lze přejít jednotlivcem.

Jsme analyzovat datovou sadu [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) v tomto návodu. To je sada e-mailů, které jsou označené jako nevyžádané pošty nebo šunku (což znamená, nejsou nevyžádané pošty), a taky obsahuje několik statistik obsah e-maily. Statistiky součástí jsou popsány v dalšího ale jeden oddíl. 


## <a name="prerequisites"></a>Zjistit předpoklady pro

Než budete moct použít Linux dat pro výzkum virtuálního počítače, musíte mít takto:

- **Azure předplatného**. Pokud už nemáte jeden, v tématu [Vytvoření Azure účet zdarma dnes](https://azure.microsoft.com/free/).
- [**Výzkum dat Linux OM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Další informace o vytváření tento OM najdete v článku [poskytnutí Linux dat pro výzkum virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) na počítači nainstalovaný a otevřít relaci XFCE. Informace o instalaci a konfigurace **klienta X2Go**najdete v tématu [instalace a konfigurace X2Go klienta](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- **AzureML účtu**. Pokud ještě nemáte jeden zaregistrovat si nový účet na [domovskou stránku AzureML](https://studio.azureml.net/). Je bezplatná použití osy vám pomůžou začít.


## <a name="download-the-spambase-dataset"></a>Stáhnout sadu spambase

Datovou sadu [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) je poměrně kratších seznamů data, která obsahuje pouze 4601 příklady. Je to vhodné velikost používat, když prokázat, že některé klíčové funkce OM vědy Data tak, jak se pořád požadavky na zdroj menší.

>[AZURE.NOTE] Tento návod byla vytvořená na D2 velikosti v2 Linux dat pro výzkum virtuálního počítače. Velikost DSVM je schopen zpracování postupy v tomto návodu.

Pokud potřebujete víc místa, můžete vytvořit další disk a připojte k vaší OM. Discích pomocí trvalý Azure úložiště, tak jejich data se zachová, i když server reprovisioned kvůli změně velikosti nebo bude ukončena. Přidat na disk a připojte ji k vaší OM, postupujte podle pokynů v [Přidat disku, který chcete OM Linux](../virtual-machines/virtual-machines-linux-add-disk.md). Tento postup použijte rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku), které je už nainstalovaná na DSVM. Proto tyto postupy se teď dá úplně od OM samotné. Další možností zvětšíte úložiště je použití [Azure souborů](../storage/storage-how-to-use-files-linux.md).

Stažení dat, otevřete okno terminálu a spusťte tento příkaz:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Pokud chcete stažený soubor tak se Pojďme vytvořit jiný soubor, který nemá záhlaví nemá řádek záhlaví. Spusťte tento příkaz Vytvořit soubor s příslušnou záhlaví:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Poté zřetězí dvě soubory společně s příkazu:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Datové obsahuje několik typů statistik na každé e-mailech: 

- Sloupce jako ***word\_frekvence\_WORD*** označení procenta slova v e-mailu, *odpovídající*. Například pokud *word\_frekvence\_zkontrolujte* je 1, 1 % všech slov v e-mailu byla *provést*. 
- Sloupce jako ***znak\_frekvence\_znak*** označení procenta všechny znaky v e-mailu, které byly *znak*. 
- ***velké\_spustit\_délka\_nejdelší*** nejdelší délka posloupnost velká písmena. 
- ***velké\_spustit\_délka\_průměr*** je průměrná délka všech řad velká písmena. 
- ***velké\_spustit\_délka\_celkové*** je celkovou délku všech řad velká písmena. 
- ***spamu*** označuje, jestli e-mailu považoval za spam nebo ne (1 = Nevyžádaná pošta, 0 = nevyžádanou).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Prozkoumání datovou sadu s Microsoft R otevřít

Pojďme zkoumat data a proveďte některé základní počítače učení s R. Výzkum OM dat je součástí [Microsoft R otevřete](https://mran.revolutionanalytics.com/open/) předinstalované. Podprocesy matematické knihoven v této verzi R nabídnout lepší výkon než různé verze jedním podprocesem. Microsoft R otevřít také poskytuje reprodukovatelnosti pomocí snímek úložišti CRAN balíčku.

Získat kopie použité v tomto návodu ukázek kódu, klonovat úložišti **Azure-počítače-výukové-dat – vědy** pomocí libovolná, který je předinstalovaný v OM. Z příkazového řádku libovolná spuštění:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Otevřete okno terminálu a spusťte novou relaci R v konzole interaktivní R.

>[AZURE.NOTE] Můžete taky RStudio pro následující postupy. Pokud chcete nainstalovat RStudio, tento příkaz v terminálu proveďte:`./Desktop/DSVM\ tools/installRStudio.sh`

Pokud chcete importovat data a nastavit prostředí, spusťte:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Chcete-li zobrazit přehled o jednotlivých sloupců:

    summary(data)

Pro jiné zobrazení dat:

    str(data)

Zobrazí se typ každé proměnné a první několika hodnot v datové sadě. 

Ve sloupci *nevyžádané pošty* byla text jako celé číslo, ale lepší je skutečně kategorií proměnné (nebo faktorem). Chcete-li nastavit jeho typ:

    data$spam <- as.factor(data$spam)

Některé průzkumné analýzy proveďte použil funkci balíček [ggplot2](http://ggplot2.org/) knihovně Oblíbené grafovým r, už nainstalovaný v OM. Poznámka z pracovní souhrnných dat dříve, zobrazeného máme přehled o frekvenci znak vykřičník. Pojďme vykreslete tyto frekvencí tady pomocí následujících příkazů:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Protože nula panelu je zkosení grafu, zbavme se jí:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Není-důvodu hustoty větší než 1, který bude vypadat zajímavé. Podívejme se na jenom tato data:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Potom ho rozdělte podle šunka a nevyžádané pošty:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Tyto příklady by vám umožňují provést podobné pozemků ostatních sloupců, které můžete prozkoumat data obsažená v nich.


## <a name="train-and-test-an-ml-model"></a>Školení a otestujte modelu ML

Teď Pojďme naučit několik modely výukové počítače ke klasifikaci e-mailů v sadě dat jako obsahující období nebo šunka. Jsme školení modelu rozhodovací strom a náhodné doménové model v tomto oddílu a potom otestovat jejich přesnost jejich předpovědí. 

>[AZURE.NOTE] Balíček rpart (rekurzivní rozdělení a regresní stromy) používá následující kód je už nainstalovaná na OM vědy Data.


Nejdřív uvedeme Pojďme rozdělit datovou sadu na školení a otestujte sady:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

A pak vytvoření rozhodovacího stromu ke klasifikaci e-maily.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Tady je výsledek:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Pokud chcete zjistit, jak dobře provádí u sady školení, použijte následující kód:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Chcete-li zjistit, jak dobře provádí u sady test:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Zkusme modelu náhodné doménové taky. Náhodné strukturami školení velký počet stromové struktury rozhodování a výstup třídy, která je režim klasifikace ze všech stromové struktury rozhodování jednotlivé. Poskytují výkonnější počítač výukové přístup jako oprava pro tendence modelu rozhodovací strom overfit školení datovou sadu. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Nasazení modelu ml Azure

[Azure počítače výukové Studio](https://studio.azureml.net/) (AzureML) je do cloudové služby, který usnadňuje sestavovat a nasazovat prediktivní analýzy modely. Jednou z hodní funkce AzureML je schopnost publikovat libovolnou funkci R jako webové služby. Balíček AzureML R usnadňuje udělat přímo z našich R relace v DSVM nasazení. 

Abyste mohli nasadit kód rozhodovací strom s předchozím oddílem, budete muset přihlásit Azure počítače výukové Studio. Potřebujete svoje ID pracovní prostor a token se tak mohli ověřovat sigh v. Hledání tyto hodnoty a inicializace proměnné AzureML s nimi:

V levé nabídce vyberte **Nastavení** . Poznamenejte si **ID pracovního prostoru**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Vyberte **Autorizace tokenů** z nabídky režijních nákladů a poznamenejte si svůj **Primární Token se tak mohli ověřovat**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Načtení **AzureML** balíčku a pak nastavte hodnoty proměnných pomocí svého ID token a pracovních prostorů v relaci R na DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Pojďme zjednodušit model usnadnit Tato ukázka provádět. Vyberte třemi proměnnými v rozhodovacího stromu nejblíže k kořenovém a vytvoření nové stromu pomocí pouze třemi proměnnými:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Potřebujeme předpovědí funkci, která používá funkce jako vstup a vrátí předpovězené hodnoty:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Publikujte funkci predictSpam AzureML pomocí funkce **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Tato funkce trvá funkci **predictSpam** vytvoří webová služba s názvem **spamWebService** s definovaný vstupů a výstupů a vrátí informace o nové koncového bodu.

Zobrazení podrobností o publikované webové služby, včetně jejího koncového bodu rozhraní API a přístupové klávesy pomocí příkazu:

    spamWebService[[2]]

Vyzkoušejte si to na první nastavte 10 řádků test:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Používat další nástroje, které jsou k dispozici

V dalších částech ukazují, jak používat některé z nástroje nainstalovaným OM vědy dat Linux. Tady je seznam nástroje popisované:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & Squirrel SQL
- SQL Server datový sklad


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) je nástroj, který poskytuje implementaci rychlé a přesné zesílen stromu.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost také zavolat z python nebo příkazový řádek.

## <a name="python"></a>Python

Pro vývoj aplikací pomocí Python distribuce Anaconda Python 2.7 a 3.5 nainstalovány v DSVM. 

>[AZURE.NOTE] Rozdělení Anaconda obsahuje [Condas](http://conda.pydata.org/docs/index.html), které můžete použít k vytvoření vlastní prostředí pro Python, kam mají různé verze a/nebo balíčků nainstalovaný v nich.

Pojďme číst v některých datovou sadu spambase a klasifikovat e-maily s počítačích vector podpory ve scikit – přečtěte si:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Pokud chcete vytvořit předpovědí:

    clf.predict(X.ix[0:20, :])

Jak publikovat koncový bod AzureML zobrazíte provedeme zjednodušuje model třemi proměnnými podle kroků uvedených dříve jsme publikování modelu R. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Model publikujete AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] To je dostupný jenom u python 2.7 není zatím podporované a na 3.5. Spusťte **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

Distribuce Anaconda v DSVM získáváte Jupyter poznámkového bloku prostředí různé platformy vám umožní sdílet Python, R nebo Julie kód a analýzy. Poznámkový blok Jupyter prostřednictvím JupyterHub. Přihlášení pomocí místní Linux uživatelské jméno a heslo na ***https://\<OM DNS nebo IP adresu\>: 8000 /***. Všechny soubory konfigurace JupyterHub se nacházejí v adresáři **/etc/jupyterhub**.

Několik poznámkových bloků ukázkové jsou už nainstalované OM:

- V tématu [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) pro poznámkový blok Python vzorku.
- Ukázkový **R** Poznámkový blok najdete v článku [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) .
- V tématu [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) pro jiné ukázkový **Python** Poznámkový blok.

>[AZURE.NOTE] Jazyk Julie neexistuje taky z příkazového řádku na OM vědy dat Linux.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analytické nástroje pro další snadno) je grafický R nástroj pro dolování dat. Má intuitivní rozhraní, který usnadňuje načíst, prozkoumat, transformace dat, vytvořit a vyhodnocení modely.  V článku [Rattle: r A grafického rozhraní dolování dat](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) nabízí návod, který ukazuje jeho funkcí.

Nainstalujte a spusťte Rattle pomocí následujících příkazů:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] V DSVM není nutné instalovat. Ale Rattle můžete být vyzváni k instalaci další balíčků při načítání.

Rattle používá rozhraním kartu. Většina karty odpovídají kroků [Procesu vědy dat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), třeba načítání dat nebo ní. Proces pro výzkum dat přecházel zleva doprava mezi kartami. Ale posledního ouška obsahuje protokol příkazů R spusťte Rattle. 


Načtení a konfigurace sadě dat:

- Načtení souboru, klikněte na kartu **Data** potom 
- Klikněte na volič vedle **názvu souboru** a vyberte **spambaseHeaders.data**. 
- Načtení souboru. v horní řádek tlačítek vyberte **Spustit** . Měli byste vidět souhrn jednotlivých sloupců, včetně identifikované datový typ, ať už jde vstup, cíl nebo jiný typ proměnné a počet jedinečných hodnot.
- Rattle správně byl zjištěn sloupci **spamu** jako cíl. Vyberte sloupec nevyžádané pošty a pak nastavte **Cílový datový typ** na **Categoric**.

Můžete prozkoumat data: 

- Vyberte kartu **Prozkoumat** . 
- Klikněte na **Shrnutí**a pak **spouštět**, zobrazíte některé informace o uvedených typech proměnných a některé souhrnné statistiky. 
- Zobrazit další typy statistických údajů o jednotlivých proměnných, vyberte další možnosti jako **popisujících** nebo **Základy**.

**Prozkoumání** karty umožňuje generovat mnoho kvalifikovaná pozemků. Chcete-li vykreslete histogram dat:


- Vyberte **rozdělení**.
- Kontrola **Histogram** **word_freq_remove** a **word_freq_you**.
- Vyberte **Spustit**. Měli byste vidět oba hustoty pozemků v okně jednoho grafu, kde je jasné, že na slovo "je" se zobrazí mnohem častěji e-mailů než "Odebrat".

Srovnávací pozemků jsou také zajímavé. Vytvořit:


- Potom vyberte **korelace** **Typ** 
- Vyberte **Spustit**. 
- Rattle vás upozorní, doporučuje se maximálně 40 proměnné. Výběrem možnosti **Ano** zobrazit grafu. 

Existuje několik zajímavých korelace, které přichází: "technologie" je důrazně vazba na "HP" a "praktická cvičení", například. To je také důrazně řadě "650", protože směrové číslo oblasti dárců datovou sadu 650.

Číselné hodnoty pro vzájemný vztah mezi slova jsou k dispozici v okně prozkoumat. Je zajímavé osobní zprávu, například, že je negativně vazba "technologie" s"" a "peníze".

Rattle lze transformovat datovou sadu zpracovávání některé běžné problémy. Například umožňuje měřítko funkce dává chybějících hodnot, zpracování odlehlé hodnoty a odebrání proměnné nebo poznámky s chybějící data. Rattle také zjistit přidružení pravidla mezi pozorování a/nebo proměnné. Tyto karty jsou mimo rozsah tento úvodní informace.

Rattle můžete taky Analýza obrázku. Pojďme vyloučit několik funkcí, které přehlednější výstupu. Na kartě **Data** vyberte **Ignore** u všech proměnných kromě těchto deseti položek:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- nevyžádané pošty

Přejděte zpět na kartě **clusteru** vyberte **KMeans**a nastavte *počet clusterů* 4. Potom **Spustit**. Výsledky jsou zobrazeny v okně výstupu. Jednoho obrázku má vysokou četnost "Jirka" a "hp" a bude pravděpodobně legitimní e-mailová adresa.

Vytvoření modelu jednoduché rozhodnutí stromu počítače výukové: 

- Vyberte kartu **modelu** 
- Jako **Typ**zvolte **strom** . 
- Vyberte **Spustit** a zobrazení stromu v textovém formátu v okně výstupu. 
- Klikněte na tlačítko **Nakreslit** zobrazíte grafické verze. Takto vypadá podobný strom, kterou jsme získané dříve pomocí *rpart*.

Jednou z hodní funkce Rattle je jeho možnost běhu několika postupů výukové počítače a rychle porovnat. Tady je postup:

- Jako **Typ**vyberte **vše** . 
- Vyberte **Spustit**. 
- Po dokončení můžete klikněte na libovolný jeden **Typ**, třeba **SVM**a zobrazte výsledky. 
- Můžete taky porovnat výkon modelech na ověření nastavení na kartě **vyhodnotit** . Například výběr **Chyby matice** uvidíte matice zmatky, celkové chyby a chybové průměrné třídy pro každý model na stránce ověření nastavit. 
- Můžete taky vykreslete ROC křivky, analýza utajení a proveďte jiné druhy model hodnocení.

Jakmile dokončíte vytváření modelů, vyberte kartu **protokolu** k zobrazení kódu R spusťte Rattle během relace. Můžete vybrat tlačítko **Export** ji chcete uložit. 

>[AZURE.NOTE] V aktuální verzi Rattle je chyba. Upravte skript nebo použít k opakovanému uvedení postupujte později, vložte znak # před *Tento protokol... exportovat* text protokolu. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL

DSVM získáváte PostgreSQL nainstalovaný. PostgreSQL je sofistikované, otevřít zdroj relační databáze. Tato část popisuje dejte naše datovou sadu nevyžádanou poštu do PostgreSQL a poté dotaz.

Před načtením data, budete muset povolit ověřování hesla z localhost. Na příkazovém řádku:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

V dolní části konfiguračního souboru je několik řádků s podrobnými povolených připojení:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Změňte plnou čáru "IPv4 místní připojení" a použít md5 místo ident, aby společnost Microsoft se přihlaste pomocí uživatelského jména a hesla:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

A znovu ho spusťte službu postgres:

    sudo systemctl restart postgresql

Spuštění psql terminálu interaktivní pro PostgreSQL, jako předdefinovaný postgres uživatele, spusťte tento příkaz z řádku:

    sudo -u postgres psql

Vytvořte nový uživatelský účet, pomocí stejného uživatelského jména účtem Linux aktuálně přihlášení jako a zadejte heslo:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Pak se přihlaste k psql jako uživatele:

    psql

A importovat data do nové databáze:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nyní se podíváme data a spouštět některé dotazů pomocí **Squirrel SQL**, grafický nástroj, který vám umožní komunikovat s databázemi prostřednictvím ovladač JDBC.

Začněte tím, spusťte Squirrel SQL v nabídce aplikace. Nastavení ovladač:

- Vyberte **Windows**, pak **ovladače zobrazení**. 
- Klikněte pravým tlačítkem myši na **PostgreSQL** a vyberte **Upravit ovladač**. 
- Vyberte **cestu výběr**a potom na **Přidat**. 
- **Název souboru** zadejte ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** a 
- Vyberte **Otevřít**.
- Vyberte seznam ovladače, a pak vyberte **org.postgresql.Driver** v poli **Název třídy**a vyberte **OK**.

Nastavení připojení k místnímu serveru:
 
- Vyberte **Windows**, pak **zobrazení aliasy.** 
- Klikněte **+** tlačítka Nový alias. 
- Název *databáze nevyžádané pošty*, vyberte v rozevíracím seznamu **ovladač** **PostgreSQL** .
- Nastavte adresu URL *jdbc:postgresql://localhost/spam*. 
- Zadejte svoje *uživatelské jméno* a *heslo*. 
- Klikněte na **OK**. 
- Otevřete okno **připojení** , poklikejte na alias ***databáze nevyžádané pošty*** . 
- Zaškrtněte políčko **Připojit**.

Spuštění některé dotazy:

- Vyberte kartu **SQL** .
- Zadání jednoduchého dotazu jako `SELECT * from data;` do textového pole dotazu v horní části karty SQL. 
- Stiskněte **Kombinaci kláves Ctrl + Enter** ho spusťte. Ve výchozím nastavení Squirrel SQL vrátí prvních 100 řádků z dotazu. 

Existuje mnoho víc dotazů, že můžete spustit, které můžete prozkoumat data. Například jak četnost slovo *Zkontrolujte* liší mezi nevyžádanou poštu a šunka?

    SELECT avg(word_freq_make), spam from data group by spam;

A co jsou vlastnosti e-mailu, které často obsahují *3d*?

    SELECT * from data order by word_freq_3d desc;

Většina e-mailů, které mají vysoké výskyt *3d* jsou očividně nevyžádané pošty, může to být užitečná funkce pro vytváření prediktivní modelu ke klasifikaci e-maily.

Pokud chcete provést výukové počítače s daty uloženými v databázi PostgreSQL, zvažte použití [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server datový sklad

Azure SQL datový sklad je cloudové, škálování databáze může zpracování obrovské objemy dat, relační i -relační. Další informace najdete v tématu [Co je Azure SQL datový sklad?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Připojení ke datový sklad a vytvořit tabulku, spusťte tento příkaz z příkazového řádku:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Klikněte do příkazového řádku sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopírování dat s bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Konce řádků ve stažený soubor jsou styl systému Windows, ale bcp předpokládá, že formátu UNIX tak potřeba dát bcp, s příznakem - r.

A dotaz se sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Můžete taky dotazovat s Squirrel SQL. Podobně jako kroky pro PostgreSQL pomocí Microsoft MSSQL JDBC ovladač serveru, kterou můžete najít v ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Další kroky

Základní informace o tématech, která vás provede jednotlivými úkoly, které zahrnují procesu dat pro výzkum v Azure najdete v článku [Týmu dat pro výzkum obrázku](http://aka.ms/datascienceprocess).

Popis jiných začátku do konce návody, které ukazují kroků v procesu týmu dat pro výzkum konkrétních situacích najdete v tématu [návody týmu dat pro výzkum obrázku](data-science-process-walkthroughs.md). Návody také ukazují, jak do cloudu a místní nástroje a služby zkombinovat pracovní postup nebo vytvořit aplikaci inteligentní kanálem k odesílání zpráv.

