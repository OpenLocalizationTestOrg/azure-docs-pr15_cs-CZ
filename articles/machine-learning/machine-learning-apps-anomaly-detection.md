<properties 
    pageTitle="Počítač výukové aplikace: odchylky zjišťování služeb | Microsoft Azure" 
    description="Rozhraní API zjišťování odchylky je znázorněn příklad vytvořené pomocí Microsoft Azure počítače učení, které rozpozná odchylky dat časové řady s číselné hodnoty, které jsou rovnoměrně řádkování v čase." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Počítač výukové odchylky zjišťování služeb#

##<a name="overview"></a>Základní informace

[Rozhraní API zjišťování odchylky](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) je znázorněn příklad vytvořené pomocí učení Azure stroje, které rozpozná odchylky dat časové řady s číselné hodnoty, které jsou rovnoměrně řádkování v čase. 

Toto rozhraní API lze určit následující typy neobvyklých vzorky dat časové řady:

* **Kladné a záporné trendů**: například při sledování spotřebu paměti při výpočtu vzestupný trend může být potřebné může být orientační únik paměti

* **Změny v dynamické rozsah hodnot**: například při sledování výjimky vyvolané do cloudové služby, všechny změny v dynamické rozsah hodnot znamenat nestabilita stav služby a

* **Špičky a klesne**: například při sledování počet neúspěšných pokusů přihlášení ve službě nebo počet rezervace v kolekci webů pro obchodování, vrcholy pole nebo pokles znamenat neobvyklého chování.

Tyto výukové detektory počítače sledovat tyto změny hodnoty nad čas a sestavy probíhající změny v jejich hodnoty jako odchylky výsledků. Nevyžadují ad hoc mezní optimalizace a jejich výsledků můžete použít k řízení falešně pozitivní sazby. Rozhraní API je užitečné v několik příčin jako služby sledování sledováním klíčové ukazatele výkonu v průběhu času zjišťování odchylky použití sledování prostřednictvím metriky například počet vyhledávání, čísla kliknutí, sledování výkonu až čítače jako paměti, procesoru, souboru bude číst atd určitou dobu.

Nabízení odchylky rozpoznávání součástí užitečné nástroje, které vám usnadní zahájení práce. 

* [Webové aplikace](http://anomalydetection-aml.azurewebsites.net/) vám pomůže zjistit a vizualizovat výsledky odchylky zjišťování rozhraní API s daty. 

* [Ukázkový kód](http://adresultparser.codeplex.com/) ukazuje, jak programově přístup k rozhraní API a analýza výsledků v jazyce C#.

>[AZURE.NOTE]
>Vyzkoušejte **řešení IT odchylky přehledy** technologii [Toto rozhraní API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Chcete-li získat toto řešení konce používaný k předplatnému Azure <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **, začněte tady >**</a>


##<a name="api-definition"></a>Rozhraní API definice

Tato služba poskytuje rozhraní REST API přes HTTPS určené různé způsoby včetně webu nebo mobilní aplikaci, R, Python, Excel atd.  Budete posílat tuto službu prostřednictvím rozhraní REST API volání dat časové řady a spustí kombinace výše popsaných typů tři odchylky. Služba kompatibilní s Azure počítače výukové informacemi o platformě, bez problémů přizpůsobí potřebám vaší společnosti, který obsahuje rozsahu.

###<a name="headers"></a>Záhlaví
Ujistěte se, zahrnout správné záhlaví v žádosti o, která by měla být následujícím způsobem:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Svůj klíč účtu ze svého účtu můžete najít v [Azure Data Market](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Rozhraní API skóre

Rozhraní API skóre slouží výpočetnímu odchylky zjišťování dat – sezónní časové řady. Rozhraní API spustí celá řada odchylky detektory na datech a vrátí jeho skóre odchylky. Následující obrázek znázorňuje příklad odchylky, které lze zjistit rozhraní API skóre. Časové řady má 2 různých změny na úrovni a 3 vrcholy pole. Červené tečky zobrazení času, pro niž je zjištěno Změna úrovně, zatímco černé tečky zobrazit zjištěné vrcholy pole.

![Rozhraní API skóre][1]
    
**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Příklad žádost o textu**

V pozvánce na schůzku níže pošleme časovou řadu (viz zkrácený) s datovým bodům z 3/1/2016 až 3/10/2016 a parametrech detektorů zásobníku k rozhraní API pro zjišťování, pokud je některý z těchto datových bodů neobvyklých. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Odpověď na to bude: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality rozhraní API

Rozhraní API ScoreWithSeasonality slouží výpočetnímu odchylky zjišťování časové řady, máte sezónní vzorků. Toto rozhraní API je užitečný pro zjišťování odchylek ve vzorcích pro sezónní.  

Následující obrázek ukazuje příklad odchylky zjištěnou sezónní časovou řadu. Časové řady má jeden zásobníku (1st černá tečka), dvě pokles (2 černou tečkou a jeden na konci) a změna jedné úrovně (červená tečka). Všimněte si, že dip uprostřed časových řad a změna úrovně pouze discernable po sezónní součásti budou odebrány z řady.

![Sezónnost rozhraní API][2]

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Příklad žádost o textu**

V pozvánce na schůzku níže pošleme k rozhraní API pro zjišťování, pokud je některý z těchto datových bodů neobvyklých časovou řadu (viz zkrácený) s datovými body z 3/1/2016 až 3/10/2016 a parametry. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Odpověď na to bude: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detektory

Zjištění odchylky rozhraní API podporuje detektory 3 kategorií. Informace o konkrétních vstupní parametry a výstupy pro každý detekce najdete v následující tabulce.

|Detekce kategorie|Detekce|Popis|Vstupní parametry|Výstupy
|---|---|---|---|---|
|Detektory zásobníku|TSpike detekce|Zjištění vrcholy pole a poklesu úplně hodnotami jsou z první a třetí QUARTIL|*tspikedetector.sensitivity:* , která bude hodnota celé číslo v rozsahu 1-10 výchozí: 3; Vyšší hodnoty zachytí další krajní hodnoty a tím méně citlivé|TSpike: binární hodnoty – 1, pokud je zjištěno zásobníku/dip; "0" jinak|
||ZSpike detekce|Zjištění vrcholy pole a poklesu založené na jaké míry datapoints jsou od jejich střední hodnoty.|*zspikedetector.sensitivity:* trvat hodnota celé číslo v rozsahu 1-10 výchozí: 3; Vyšší hodnoty zachytí další krajní hodnoty, které méně citlivé|ZSpike: binární hodnoty – 1, pokud je zjištěno zásobníku/dip; "0" jinak|
|Detekce pomalé trendu|Detekce pomalé trendu|Zjištění pomalé pozitivní trend podle nastavení utajení|*trenddetector.sensitivity:* mezní na detekce skóre (výchozí: 3,25, 3,25 – 5 je vhodnější oblast k výběru z; Vyšší méně citlivé)|TScore: plovoucí číslo představující odchylky skóre na trendu.|
|Změna úrovně detektory|Změna úrovně jednosměrné detekce|Zjištění nahoru úroveň změnit podle nastavení utajení|*upleveldetector.sensitivity:* mezní na detekce skóre (výchozí: 3,25, 3,25 – 5 je vhodnější oblast k výběru z; Vyšší méně citlivé)|PScore: plovoucí číslo představující odchylky skóre nahoru na úrovni změnit|
||Změna detekce obousměrný úrovně|Změna úrovně nahoru a dolů podle utajení sady zjišťování|*bileveldetector.sensitivity:* mezní na detekce skóre (výchozí: 3,25, 3,25 – 5 je vhodnější oblast k výběru z; Vyšší méně citlivé)|RPScore: číslo představující odchylky, které skóre na nahoru a dolů úroveň změnit plovoucí

###<a name="parameters"></a>Parametry

Podrobnější informace o těchto vstupních parametrů je uvedený v následující tabulce:

|Vstupní parametry|Popis|Výchozí nastavení|Typ|Platný rozsah|Navržená oblast|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Interval agregace v sekundách pro agregaci při zadávání časových řad|0 (je použita agregace)|celé číslo|0: jinak přeskočit agregace, > 0|5 minut 1 den, závislé časových řad
|preprocess.aggregationFunc|Funkce se používá pro agregaci dat do zadané AggregationInterval|průměr|vytvořit výčet|Střed_hodn; SUMA, délka|NENÍ K DISPOZICI|
|preprocess.replaceMissing|Hodnoty k dává chybějící data|lkv (označované poslední hodnotu)|výčtu|nula, lkv, střední hodnotu|NENÍ K DISPOZICI|
|detectors.historyWindow|Historie (počet datových bodů) používá pro výpočet skóre odchylky|500|celé číslo|10 2000|Závislý časových řad|
|upleveldetector.Sensitivity|Utajení nahoru úrovně změnit detekce. |3,25|dvojité|Žádná|3,25 5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Utajení pro úroveň obousměrný změnit detekce. |3,25|dvojité|Žádná|3,25 5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|Utajení detekce kladné trendu. |3,25|dvojité|Žádná|3,25 5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |Utajení TSpike detekce|3|celé číslo|1 – 10|3 – 5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |Utajení ZSpike detekce|3|celé číslo|1 – 10 |3 – 5(Lesser values mean more sensitive)|
|seasonality.enable|Zda má provést sezónnosti analýzy|PRAVDA|Logická hodnota|PRAVDA, NEPRAVDA|Závislý časových řad|
|seasonality.numSeasonality |Maximální počet pravidelných cyklů zjišťování|1|celé číslo|1, 2|1-2|
|seasonality.Transform |Jestli sezónní (a) trendu komponenty se odstraní před použitím odchylky zjišťování|deseason|výčtu|žádná, deseason, deseasontrend|NENÍ K DISPOZICI|
|postprocess.tailRows |Počet datových bodů nejnovější zůstat ve výsledcích výstup|0|celé číslo|0 (ponechat všem datovým bodům), nebo zadejte počet bodů na výsledky|NENÍ K DISPOZICI|

###<a name="output"></a>Výstup
Rozhraní API spustí všechny detektory svých dat časové řady a vrátí skóre odchylky a binární zásobníku indikátory jednotlivých bodech v čase. Následující tabulka uvádí výstupy z rozhraní API. 

|Výstupy|Popis|
|---|---|
|Čas|Časová razítka z nezpracovanými data nebo agregované (a/nebo) imputované Pokud agregace (nebo) chybí se použije imputace dat|
|OriginalData|Hodnoty z nezpracovanými data nebo agregované (a/nebo) imputované Jestliže agregace (nebo) chybí se použije imputace dat|
|ProcessedData|Jednu z následujících akcí: <ul><li>Pokud byl zjištěn významné sezónností a vlastními deseason možnost; očištěných časovou řadu.</li><li>očištěné upravují a kolísání časových řad Pokud byl zjištěn významné sezónnost a vybraná možnost deseasontrend</li><li>v ostatních případech to je stejný jako OriginalData</li>|
|TSpike|Binární indikátor označuje, zda je tak, že TSpike detekce nerozpoznal zásobníku|
|ZSpike|Binární indikátor označuje, zda je tak, že ZSpike detekce zjištěno zásobníku|
|Pscore|Plovoucí číslo představující odchylky skóre na nahoru úroveň změnit|
|Palert|Hodnota 1 nebo 0, že je nahoru úroveň změnit odchylky podle vstupní utajení|
|RPScore|Plovoucí odchylky číslo představující počet bodů na úrovni změnit obousměrný|
|RPAlert|Hodnota 1 nebo 0, že je úroveň obousměrný změnit odchylky podle vstupní utajení|
|TScore|Plovoucí odchylky číslo představující počet bodů na kladné trendu|
|TAlert|Hodnota 1 nebo 0 označující, že je kladné trendu odchylky podle vstupní utajení|


Tento výstup můžete analyzovat pomocí [jednoduchého analyzátor](https://adresultparser.codeplex.com/) - má ukázkový kód, který ukazuje, jak se můžete připojit k rozhraní API a analyzovat výstup. Zjištěné odchylky můžete vizualizovat na tabuli a/nebo předána lidské experty korekční akce nebo integrovaný evidence systémy.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
