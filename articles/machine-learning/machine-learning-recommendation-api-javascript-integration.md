<properties 
    pageTitle="Počítače výukové doporučení: Integrace JavaScript | Microsoft Azure" 
    description="Azure přečtěte následující dokumentaci pro počítač výukové doporučení - JavaScript integrace-" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure počítače výukové doporučení - integrace JavaScript

>[AZURE.NOTE]Měli byste začít používat službu kognitivní rozhraní API doporučení místo tato verze. Kognitivní službu doporučení se nahradí tuto službu a s novými funkcemi bude vyvinuté tam. Má nové funkce jako dávky podpory lepší Exploreru rozhraní API, opět dostanete čistší prostředí povrchový, jednotnější registrace/fakturace rozhraní API, atd.
> Další informace o [migraci do nové kognitivní služby](http://aka.ms/recomigrate)


Tento dokument znázornění jak integrovat vašemu webu pomocí účtu JavaScript. JavaScript umožňuje odesílat události získávání dat a používání doporučení po vytvoření modelu doporučení. Všechny operace využitím JS lze rovněž provést z na straně serveru.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. přehled obecné
Integrace vašeho webu s Azure ML doporučení skládat na 2 fází:

1.  Odeslání událostí do Azure ML doporučení. To vám umožní k vytvoření modelu doporučení.
2.  Používání doporučení. Po modelu můžete využívat doporučení. (Tento dokument není je vysvětleno, jak vytvářet modelu, přečtěte si úvodní příručku získat další informace o tom).


<ins>Můžu fáze</ins>

V první fázi vložíte do stránek html malé knihovna JavaScript, která umožňuje na stránce odeslání události při jejich vzniku na stránce html do Azure ML doporučení servery (přes Data Market):

![Výkres1][1]

<ins>Fáze II</ins>

Ve druhé fáze, kdy se má objevit doporučení na stránce můžete vybírat z následujících možností:

1.v serveru (na fáze vykreslování stránky) volá Azure ML doporučení Server (přes Data Market) získat doporučení. Výsledky zahrnout seznam id položky. Serveru musí obohacení výsledky s položkami metadata (například obrázky, popis) a pošlete vytvořené stránky do prohlížeče.

![Drawing2][2]

2. na jinou možnost je použití malé soubory JavaScriptu z první fázi ke zjištění jednoduchý seznam Doporučené položky. Data dostali tady jsou zeštíhlen než v první možnost.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. požadavky na

1. Vytvoření nového modelu pomocí rozhraní API. V tématu – úvodní příručka o tom, jak to udělat.
2. Kódování vaší &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; s ve formátu Base 64. (To se použije pro základní ověřování pro povolení JS kódu zavolejte rozhraní API).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. odeslat události získávání dat pomocí skriptu JavaScript
Podle těchto kroků usnadnění odesílání událostí:

1.  Zahrňte knihovny JQuery kódu. Můžete si ji z nuget v následující adresu URL.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Zahrnout knihovnu skriptu Java doporučení od následující adresu URL: http://aka.ms/RecoJSLib1

3.  Inicializace Azure ML doporučení knihovny s příslušnými parametry.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Odeslání příslušnou událost. V části Podrobné pod na všechny typy události (příklad klikněte na událost)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Omezení a podporu prohlížeče
Toto je implementace odkaz a je uveden je. Má podporovat všechny hlavní verze prohlížeče.

###<a name="32-type-of-events"></a>3,2. Typ události
Existují 5 typy události, která podporuje knihovnu: klikněte na, klikněte na doporučení, přidat do košíku pracoviště odebrání pracoviště košík a nákupu. Je další událost, která slouží k nastavení kontext uživatele s názvem přihlášení.

####<a name="321-click-event"></a>3.2.1. Klikněte na událost
Tato událost bude použito kdykoli uživatel klepnutí na položku. Obvykle poté, co uživatel klikne na položku Nová stránka se otevře se v detailech položku; na této stránce by měl být spouštěný Tato událost.

Parametry:
- události (řetězec, povinné) - "kliknutím."
- položky (řetězec, povinné) - Jedinečný identifikátor položky
- itemName (řetězec, nepovinný) – název položky
- Popis zboží (řetězec, nepovinný) - popis položky
- itemCategory (řetězec, nepovinný) - kategorie položce
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Nebo se volitelné daty:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Doporučení klikněte na událost
Tato událost bude použito kdykoli uživatel klepnutí na položku, která byla přijata z Azure ML doporučení Doporučené položky. Obvykle poté, co uživatel klikne na položku Nová stránka se otevře se v detailech položku; na této stránce by měl být spouštěný Tato událost.

Parametry:
- události (řetězec, povinné) - "recommendationclick"
- položky (řetězec, povinné) - Jedinečný identifikátor položky
- itemName (řetězec, nepovinný) – název položky
- Popis zboží (řetězec, nepovinný) - popis položky
- itemCategory (řetězec, nepovinný) - kategorie položce
- Osazuje (řetězců, nepovinný) - semen generovaných dotazů doporučení.
- recoList (řetězců, nepovinný) – výsledek žádost o doporučení, které vygenerovalo položku, na který jste kliknuli.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Nebo se volitelné daty:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Přidání nákupní košík události
Tato událost bude použito při uživatele do přidat položku nákupní košík.
Parametry:
* události (řetězec, povinné) - "addshopcart"
* položky (řetězec, povinné) - Jedinečný identifikátor položky
* itemName (řetězec, nepovinný) – název položky
* Popis zboží (řetězec, nepovinný) - popis položky
* itemCategory (řetězec, nepovinný) - kategorie položce
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Odebrání nákupní košík události
Tato událost má používat, když uživatel odebere položku, kterou chcete nákupní košík.

Parametry:
* události (řetězec, povinné) - "removeshopcart"
* položky (řetězec, povinné) - Jedinečný identifikátor položky
* itemName (řetězec, nepovinný) – název položky
* Popis zboží (řetězec, nepovinný) - popis položky
* itemCategory (řetězec, nepovinný) - kategorie položce
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Nákup události
Když uživatel koupili jeho nákupní košík bude použito Tato událost.

Parametry:
* události (řetězec) - "nákup"
* položky (koupili []) - pole blokování položky pro každou položku zakoupit.<br><br>
Zakoupené formát:
    * Položka (řetězec) - Jedinečný identifikátor položky.
    * počet (int nebo řetězec) - položky, které jste si koupili.
    * cena (plovoucí nebo řetězec) – volitelné pole - cenu položky.

Nákup 3 ukazuje tento příklad položek (33 34, 35), dvě s všechna pole vyplněné (položky, počet a cena) a (položka 34) bez cena.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Události přihlášení uživatele
Azure ML doporučení události knihovny vytvoří a používat soubor cookie k identifikaci události, které jste dostali ze stejného prohlížeče. Pokud chcete zlepšit výsledky modelu Azure ML doporučení umožňuje nastavit jedinečný identifikační uživatele, který přepíše použití souborů cookie.

Po přihlášení uživatele k vašemu webu bude použito Tato událost.

Parametry:
* události (řetězec) - "userlogin"
* uživatel (řetězec) - jedinečný identifikační uživatele.
        <script>
  Pokud (typeof AzureMLRecommendationsEvent == "Nedefinováno") {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({události: "userlogin" uživatele: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. doporučení prostřednictvím JavaScript používat
Kód, který využívá doporučení je spouštěný kliknutím událost některé JavaScript klienta webové stránky. Odpověď na doporučení obsahuje doporučené položky ID, jejich jména a jejich hodnocení. Doporučujeme tuto možnost použít jenom pro zobrazení seznamu doporučená položek – složitější zpracování (třeba přidat na položku metadata) se provádí integraci straně serveru.

###<a name="41-consume-recommendations"></a>4.1 používání doporučení
Využívat doporučení, které bude nutné obsahovat požadované JavaScript knihovny na stránce a volání AzureMLRecommendationsStart. V části 2.

Využívat doporučení pro jednu nebo více položek, budete muset volat metodu nazvanou: AzureMLRecommendationsGetI2IRecommendation.

Parametry:
* položky (matice řetězců) – jeden nebo více položky k získání doporučení pro. Pokud používání Fbt sestavení pak můžete nastavit tady pouze jednu položku.
* numberOfResults (int) - počet požadované výsledky.
* includeMetadata (logická hodnota, nepovinný) – Pokud nastavena na hodnotu "true" označuje, že pole metadat musí vyplněné ve výsledku.
* Funkce - funkci, která bude zpracovávat doporučení zpracování vrácena. Data je vrácena jako pole:
    * Položka - položka jedinečné id
    * Název - názvu položky (pokud jsou v katalogu)
    * hodnocení – doporučení hodnocení
    * metadata - řetězec, který představuje metadata položky

Příklad: Tento kód požaduje 8 doporučení pro položku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (a nikoli zadáním includeMetadata - implicitně s textem, že není potřeba žádná metadata), poté zřetězí výsledek do vyrovnávací paměť.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
