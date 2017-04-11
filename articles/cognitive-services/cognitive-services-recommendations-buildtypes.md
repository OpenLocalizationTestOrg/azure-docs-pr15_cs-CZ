<properties
    pageTitle="Typy Tvůrce dotazů a modelu kvalita: doporučení rozhraní API | Microsoft Azure"
    description="Azure počítače výukové doporučení – úvodní příručka"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Typy Tvůrce dotazů a kvalitu modelu #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Typy podporovaného Tvůrce dotazů ##

Rozhraní API doporučení v současné době podporuje dva typy sestavení: *doporučení* a *FBT*. Každý je vytvořené pomocí různých algoritmy a každý z nich má jiný pevnosti. Tento dokument obsahuje popis těchto sestavení a technik pro porovnání kvalitu modelech generovaného.

Pokud jste to ještě neudělali, doporučujeme provést [Rychlý start](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Typ buildu doporučení ###

Typ buildu doporučení využívá matice factorization doporučení. Vygeneruje [Skryté funkce](https://en.wikipedia.org/wiki/Latent_variable) vektory podle své transakce popisuje všechny požadované položky a potom pomocí těchto skryté vektory a umožňuje porovnávat položky, které jsou podobné.

Pokud školení modelu podle nákupy ve vašem úložišti elektronických a poskytují telefon se systémem Lumia 650 jako vstup do modelu, modelu vrátí sadu položky, které mají za následek zakoupit lidmi, kteří jsou pravděpodobně koupit Lumia 650 telefon. Položky nemusí být doplňkovou. V tomto příkladu je možné, že modelu vrátí další telefony protože lidmi, kteří jako Lumia 650 může jako další telefony.

Sestavení doporučení má dvě funkce, díky kterým ji atraktivní:

* *Sestavení doporučení podporuje *studenou položku* umístění **

Položky, které nemají významné použití nazývají studenou položky. Například pokud dostáváte dodávky telefon, který se nikdy prodané před systému nemůžete odvodit doporučení pro tento produkt transakcí samostatně. To znamená, že by měl systému čerpat z informace o produktu vlastní.

Pokud chcete použít umístění studenou položky, budete muset zadat informace funkcí pro všechny položky v katalogu. Následuje několik prvních řádků katalogu může vypadat takto (Formát poznámky klíč = hodnota pro funkci).

>Pro2 6CX 00001, povrchový, Surface, zadejte = hardwaru, úložiště = 128 GB paměti = 4G, výrobce = Microsoft

>73H-00013, důsledku Xbox 360 herní,, typ = softwaru, jazyka = angličtina hodnocení = zdokonaleným

>DVA 0F05, Minecraft Xbox 360, herní,, * typ = softwaru, jazyka = španělština, hodnocení = mládežnických

Taky musíte nastavit následující parametry Tvůrce dotazů:

| Vytváření parametru         | Poznámky
|------------------     |-----------
|*useFeaturesInModel*     | Nastavte na **hodnotu true**.  Označuje, pokud funkce se dají použít k vylepšení modelu doporučení.
|*allowColdItemPlacement*   | Nastavte na **hodnotu true**. Označuje, pokud doporučení by měl taky nabízená studenou položky pomocí funkce podobnosti.
| *modelingFeatureList*   | Hodnoty oddělené seznam názvů funkcí pro použití v dialogu sestavení doporučení k vylepšení doporučení. Například "jazyk a úložiště" pro předchozí příklad.

**Sestavení doporučení podporuje doporučení uživatele**

Doporučení sestavení podporuje [doporučení uživatele](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). To znamená, že dát jí tak přizpůsobených doporučení pro uživatele podle jejich transakce historie. Doporučení uživatel může poskytnout poslední historii transakce pro daného uživatele nebo ID uživatele.

Jeden při přihlášení na úvodní stránce je klasické například místo, kam můžete chtít použít doporučení uživatele. Zde můžete zvýšit úroveň obsah, který se vztahuje na určité uživatele.

Můžete taky použít typu sestavení doporučení po uživateli k rezervaci. V tomto okamžiku máte seznam položek, které uživatel koupit a zadáte doporučení podle aktuální košík market.

#### <a name="recommendations-build-parameters"></a>Doporučení sestavit parametry

| Jméno  |   Popis |    Typ <br>  Platné hodnoty <br> (výchozí hodnota)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Celkový čas výpočetním a přesnost modelu se projeví počtu iterací, které provede modelu. Čím je číslo, přesnější model, ale časového využití trvá déle.  |   Celé číslo, <br>  10 až 50 <br>(40)
| *NumberOfModelDimensions* |   Počet rozměry vztahují ke řadu funkcí, které modelu pokusí se najít v rámci dat. Zvýšení počtu rozměry vám umožní lepší optimalizaci výsledků do menší clusterů. Však příliš mnoho dimenze zabrání modelu nalézt korelace mezi položkami. |  Celé číslo, <br> 10 až 40 <br>(20) |
| *ItemCutOffLowerBound* |  Určují minimální počet bodů použití, které by měl být položky na jej považuje za část modelu. |        Celé číslo, <br> 2 nebo více <br> (2) |
| *ItemCutOffUpperBound* |  Určuje maximální počet bodů použití, které by měl být položky na jej považuje za část modelu. |  Celé číslo, <br>2 nebo více<br> (2147483647) |
|*UserCutOffLowerBound* |   Určují minimální počet transakce, který uživatel musí splnili považuje za část modelu. | Celé číslo, <br> 2 nebo více <br> (2)
| *UserCutOffUpperBound* |  Určuje maximální počet transakce, který uživatel musí splnili považuje za část modelu. | Celé číslo, <br>2 nebo více <br> (2147483647)|
| *UseFeaturesInModel* |    Označuje, pokud funkce se dají použít k vylepšení modelu doporučení. |     Logická hodnota<br> Výchozí: PRAVDA
|*ModelingFeatureList* |    Hodnoty oddělené seznam názvů funkcí pro použití v dialogu sestavení doporučení k vylepšení doporučení. To záleží na funkce, které jsou důležité. |    Řetězec, aby byl 512 znaků
| *AllowColdItemPlacement* |    Označuje, pokud doporučení by měl taky nabízená studenou položky pomocí funkce podobnosti. | Logická hodnota <br> Výchozí: False
| *EnableFeatureCorrelation*    | Označuje, pokud funkce se dají použít v úvahu. | Logická hodnota <br> Výchozí: False
| *ReasoningFeatureList* |  Hodnoty oddělené seznam názvů funkcí pro důvody vět, například vysvětlení doporučení. To záleží na funkce, které jsou důležité pro zákazníky. | Řetězec, aby byl 512 znaků
| *EnableU2I* | Povolte přizpůsobených doporučení taky se mu říká uživatelům položky doporučení (U2I). | Logická hodnota <br>Výchozí: PRAVDA
|*EnableModelingInsights* | Určuje, zda má být offline hodnocení provedena shromažďovat modelování přehledy (to znamená přesnost a různorodosti metriky). Pokud nastavena na hodnotu true, podmnožinu dat nepoužije pro školení, protože bude muset vyhrazeno pro účely testování modelu. Další informace o [offline hodnocení](#OfflineEvaluation). | Logická hodnota <br> Výchozí: False
| *SplitterStrategy* | Pokud povolíte modelování přehledy nastavena na *hodnotu true*, je to, jak rozdělit dat pro vyzkoušení.  | Řetězec, *RandomSplitter* nebo *LastEventSplitter* <br>Výchozí: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>Typ FBT buildu ###

Často nakoupily společném Tvůrce dotazů (FBT) znamená rozbor spočítá, kolikrát dvě nebo tři různé produkty spoluvytváření dojít k sobě. Provede řazení výše uvedené množiny podle podobnosti funkce (**spolu výskytů**, **Jaccard**, **zvedněte**).

Představit **Jaccard** a **zvedněte** jako způsoby, jak normalizovat spolu výskyty.  To znamená, že položky budou vráceny jenom v případě, že místo zakoupení společně s počáteční hodnota položky.

V našem příkladu telefonní Lumia 650 telefonní X vrátí jenom v případě, že telefonní X jste koupili ve stejné relaci Lumia 650 telefon. Protože to může být pravděpodobně by očekáváme položky doplňkovou 650 Lumia má být vrácen. například Ochrana obrazovky nebo adaptér power pro Lumia 650.

Dvě položky jsou v současné době dosadí zakoupili ve stejné relaci, pokud se objevují ve transakce s stejné ID uživatele a časové razítko.

Sestavení FBT nepodporují studenou položky, protože definicí očekávají dvě položky chcete zakoupit v rámci jedné transakce. Během FBT buildy můžete vrátit sad položek (trojice), nepodporují přizpůsobených doporučení protože přijetí jedné počáteční hodnota položky jako vstup.


#### <a name="fbt-build-parameters"></a>Parametry FBT Tvůrce dotazů

| Jméno  |   Popis |       Typ  <br> Platné hodnoty <br> (výchozí hodnota)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Jak konzervativní se model. Počet výskytů dalších položek pro modelování. |  Celé číslo, <br> 3 – 50 <br> (6).
| *FbtMaxItemSetSize* | Počet položek v sadě časté Bounds.| Celé číslo  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimální skóre, které často sady měli uvedené ve vrácených výsledků. Vyšší lepší. | Dvojité <br> 0 a vyšší <br> (0)
| *FbtSimilarityFunction* | Definuje funkce podobnosti pro použití v dialogu sestavení. **Zvedněte** upřednostňuje serendipity, **spolu výskyt** upřednostňuje předvídatelnost a **Jaccard** je narušení zabezpečení mezi nimi. | Řetězce  <br>  <i>cooccurrence, výtah, jaccard</i><br> Výchozí: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Vytvoření hodnocení a výběru ##

Tyto pokyny mohou vám pomůže určit zda používejte doporučení sestavení nebo sestavení FBT, ale nenabízí konečné odpovědí v případech, kdy může použijte některý z nich. Taky i když víte, že chcete použít předdefinované FBT, pořád můžete zvolit **Jaccard** nebo **zvedněte** jako funkce podobnosti.

Nejlepší způsob, jak vybrat mezi dva různé verze je testovat v reálném světě (online hodnocení) a sledovat převodu sazbu pro různé verze. Kurz převodu může měřit podle doporučení kliknutí, číslo skutečné nákupy z doporučení vidět nebo dokonce na částky skutečné nákupní byly vidět různé doporučení. Můžete vybrat vaše míru sazba převodu podle firmy cíle.

V některých případech je vhodné před uvedením ve výrobním vyhodnocení modelu v režimu offline. Když offline hodnocení není náhrady samolepicích online hodnocení, mohou sloužit jako metriky.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Offline hodnocení  ##

Cílem offline vyhodnocení je předpovídání přesnost (počet uživatelů, kteří budou koupit Doporučené položky) a různorodým doporučení (počet položek, které jsou vám doporučené).
V rámci přesnosti a různorodosti hodnocení metriky systém najde výběru uživatelů a rozdělí transakce pro tyto uživatele do dvou skupin: datovou sadu školení a test datové sady.

> [AZURE.NOTE]Pokud chcete používat offline metriky, musí mít časová razítka ve vašich datech použití.
> Časové údaje požaduje pro její rozdělení použití správně mezi školení a otestujte datové sady.

> Offline hodnocení nemusí taky poskytují výsledky pro malé použití souborů. Pro hodnocení, které má být rozsáhlými musí být alespoň 1 000 použití bodů v datové sadě test.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Přesnost na k ###
V následující tabulce představuje výstup offline hodnocení s přesností na k.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Procento |   13.75 | 18.04   | 21 |  24.31 | 26.61
|Uživatelé při zkoušce |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Uživatelé považuje za | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Uživatelé nezahrnují | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
V předchozí tabulce *k* představuje počet doporučení zobrazují zákazníkovi. V tabulce bude číst následujícím způsobem: "Zadáno zkušební období jedinou doporučení byl zákazníkům, pouze 13.75 uživatelů by zakoupili tohoto doporučení." Tento příkaz je založena na za předpokladu, že byla modelu školení s daty nákup. Další způsob, jak to, že je přesnost od 1 13.75.

Zjistíte, že jako další položky se zobrazí zákazníkovi, pravděpodobnost zákazníků zakoupení doporučené položku přejde. Pro předchozí experiment pravděpodobnost téměř zdvojnásobuje na 26.61 procent až 5 položek doporučují.

#### <a name="percentage"></a>Procento
Zobrazí se procento uživatelů, kteří alespoň jedno z doporučení *k* serveru. Procento vypočítána tak, že vydělí počet uživatelů, které serveru alespoň jeden doporučení celkový počet uživatelů, které považuje za. Přečtěte si článek uživatelů považuje za další informace.

#### <a name="users-in-test"></a>Uživatelé při zkoušce
Data v této řadě představují celkový počet uživatelů v sadě dat testu.

#### <a name="users-considered"></a>Uživatelé považuje za
Uživatel považován pouze pokud systému doporučené alespoň na úrovni položky *k* založené na modelu generovaný pomocí datovou sadu školení.

#### <a name="users-not-considered"></a>Uživatelé nezahrnují
Data v této řadě představují žádní uživatelé nezahrnují. Uživatelé, kteří nedostali aspoň *k* Doporučené položky.

Uživatel nezahrnují = uživatelů v testu – považuje za uživatele

<a name="Diversity"></a>
### <a name="diversity"></a>Různorodosti ###
Metriky různorodosti změřit typ položky doporučené. V následující tabulce představuje výstup offline hodnocení různorodosti.

|Percentil bloku |    0-90|  90 99| 99 100
|------------------|--------|-------|---------
|Procento        | 34.258 | 55.127| 10.615


Součet položek doporučené: 100,000

Jedinečných položek doporučené: 954

#### <a name="percentile-buckets"></a>Percentil bloky
Každý blok percentilu reprezentované období (minimální a maximální hodnoty, které v rozsahu 0 až 100). Položky zavřít 100 Nejoblíbenější položky a položky zavřít 0 nejméně Oblíbené. Například-li procentuální hodnotu v bloku percentilu 99 100 10.6, znamená to vrácení 10.6 procent doporučení pouze horních jedno procento Nejoblíbenější položky. Minimální hodnota percentilu bloku je včetně a maximální hodnota je výhradním, s výjimkou 100.
#### <a name="unique-items-recommended"></a>Doporučené jedinečných položek
Jedinečných položek doporučené míru zobrazuje počet různých položek vrácených pro hodnocení.
#### <a name="total-items-recommended"></a>Celkový počet položek doporučené
Celkový počet položek doporučené míru ukazuje číslo položky doporučené. Některé mohou být duplicitní položky.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Metriky offline hodnocení ###
Přesnost a různorodosti offline metriky může být užitečné, když vyberete které sestavení používat. V čase sestavit jako součást odpovídajících FBT nebo doporučení sestavit parametry:

-   Nastavení parametrů sestavení *enableModelingInsights* **logickou hodnotu PRAVDA**.
-   Volitelně můžete vybrat *splitterStrategy* (buď *RandomSplitter* nebo *LastEventSplitter*).
*RandomSplitter* rozdělí využití vlaku a test řad podle procent test dané *randomSplitterParameters* a náhodné inokula hodnoty.
*LastEventSplitter* rozdělí využití vlaku a otestujte sady podle poslední transakce pro každého uživatele.

To bude aktivovat Tvůrce dotazů, který používá jen podmnožinu dat pro školení a zbytek data pro výpočet metriky hodnocení.  Po dokončení sestavování získat výstup hodnocení, musíte pro volání [získání sestavení metriky API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), předávání odpovídajících *modelId* a *buildId*.

 Následuje JSON výstup pro hodnocení vzorku.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
