<properties
    pageTitle="Vytváření modelů technologie pro analýzu text v Azure počítače výukové Studio | Microsoft Azure"
    description="Jak vytvořit text analýzy modely v Azure počítače výukové Studio pomocí modulů pro text předzpracování, N-g nebo funkce algoritmus hash"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Vytváření modelů technologie pro analýzu text v Azure počítače výukové Studio

Můžete Azure počítače výukové sestavovat a umožňují modely analýzy text. Tyto modely můžete vyřešit, například dokument klasifikace nebo myšlenkou analýzu problémů.

V testu analýzy textu stejně obvykle:

 1. Vymazání a předběžně zpracovat datovou sadu textu
 2. Extrahování číselné funkce vektory z předem zpracovaných textu
 3. Vlaku klasifikace nebo regresní modelu
 4. Počet bodů a ověřte modelu
 5. Nasazení modelu výrobní

V tomto kurzu se naučíte, tyto kroky jako projdeme modelu analýzy myšlenkou pomocí datovou sadu Amazon recenzí (najdete v článku tohoto dokumentu výzkumu "biografie, Bollywood, výložníku polí a Blenders: přizpůsobení domény pro myšlenkou klasifikace" Jan Blitzer, Dredze označit a Fernando Pereira; Přidružení výpočetním lingvistické (ACL) 2007.) Tento datovou sadu obsahuje revize výsledků (1-2 nebo 4 až 5) a volné text. Cílem je předpovídání skóre revize: nízké (1 - 2), nebo od nejvyšší (4-5).

Můžete najít pokusy uvedené v tomto kurzu na Cortana Intelligence galerie:

[Předpovídání recenzí] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Předpovídání recenzí - prediktivní Experiment] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Krok 1: Vám pomůže vyčistit a předběžně zpracovat datovou sadu textu

Začneme testu tak, že vydělí skóre revize do kategorií dolního a horního bloky formulace problém jako dvě třídy klasifikace. Používáme [upravit Metadata] (https://msdn.microsoft.com/library/azure/dn905986.aspx) a moduly [skupiny kategorií Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Vytvoření popisků](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Potom jsme vyčistěte text pomocí modulu [předběžně zpracovat Text] (https://msdn.microsoft.com/library/azure/mt762915.aspx). Čištění omezí šum v sadě dat, vám pomůžou vyhledat nejdůležitější funkce a zlepšit přesnost konečný modelu. Odebrání stopwords - běžné slova jako "na" nebo "a" - a čísel speciální znaky, duplicitních znaků, e-mailové adresy a adresy URL. Jsme taky převést text na malá písmena, lemmatize slova a zjistit hranice věty, které pak označeny "|||" symbol v předem zpracovaných textu.

![Předběžně zpracovat textu](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Co dělat, když chcete použít vlastní seznam stopwords? Můžete ji v předat volitelné předávat na vstupu. Můžete taky použít vlastní C# syntaxe regulárních výrazů k nahrazení podřetězec a odebrání slova tak, že slovní: podstatných jmen, akce nebo přídavných jmen.

Po dokončení předzpracování jsme rozdělit vlaku data a otestujte sady.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Krok 2: Extrahování číselné funkce vektory z předem zpracovaných textu

K vytvoření modelu textových dat, obvykle máte k převodu textu volné vektory číselné funkce. V tomto příkladu jsme funkce [extrahovat N-gramatika z textu] (https://msdn.microsoft.com/library/azure/mt762916.aspx) modul k transformaci dat text do těchto formátu. Tento modul používá sloupec hodnotami oddělenými mezery slov a vypočítá slovníku slov nebo N-g slov, které se objeví ve vaší datovou sadu. Potom spočítá spočítá, kolik časy jednotlivých aplikace word nebo N-gramatika, zobrazí se v jednotlivých záznamů a vytvoří vektory funkce od těch. V tomto kurzu budeme nastavení N-gramatika velikosti 2, takže naše funkce vektory obsahovat jednotlivá slova a kombinací dvou nebo více následujících slov.

![Extrahování N-g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Použijeme TF * spočítá IDF (termínů počet_plateb inverzní dokumentu frekvence) váhu N-gramatika. Tento přístup přidá tloušťky slova, která se zobrazí často jeden záznam, ale jsou méně častých napříč celou sadu. Další možnosti obsahuje binární TF a grafu zvážení.

Tyto funkce text mají často vysoké dimenzionalitu. Například pokud vaše souhrnu 100 000 jedinečných slov, místo funkce budou mít 100 000 rozměry nebo více použijete-li N-g. Modul extrahovat funkce N-gramatika vám sadou možností zmenšit dimenzionalitu. Můžete vyloučit slova, která jsou krátké long i příliš běžné nebo moc časté mít významné předpovězené hodnoty. V tomto kurzu budeme vyloučit N-g, které se zobrazí v méně než 5 záznamů nebo vyšší než 80 % záznamy.

Také můžete výběr funkcí vyberte jenom funkce, které jsou nejčastěji vzájemném vztahu s cílem předpovědí. Vyberte 1000 funkce používáme výběr chí funkcí. Slovník vybraný text nebo N-g můžete zobrazit kliknutím na pravý výstup modul extrahovat N-g.

Jako alternativní přístup k používání extrahovat funkce N-gramatika můžete použít funkci algoritmus hash modul. Poznámka: v případě, že [funkce algoritmus hash] (https://msdn.microsoft.com/library/azure/dn906018.aspx), které neobsahují předdefinované funkce Výběr možnosti nebo TF * IDF zvážení.

## <a name="step-3-train-classification-or-regression-model"></a>Krok 3: Školení klasifikace nebo regresní modelu

Teď byla transformována text do sloupců číselné funkce. Datové stále obsahuje řetězec sloupce z předchozích fází, takže používáme vybrat sloupce v sadě dat je vyloučit.

Potom používáme [dvě třídy logistickou regresní] (https://msdn.microsoft.com/library/azure/dn905994.aspx) odhadnout Naším cílem: skóre nízké nebo vysoké revize. V tomto okamžiku byla transformována problém analýzy text na běžná klasifikace problém. Nástroje dostupné v Azure počítače výukové můžete zlepšit modelu. Můžete třeba experimentovat s jinou třídění, pokud chcete zjistit, jak přesné výsledky poskytují nebo použít hyperparameter optimalizace ke zlepšení přesnosti.

![Vlaku a skóre](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Krok 4: Skóre a ověřte modelu

Jak by ověřit školení model? Skóre proti datovou sadu test jsme vyhodnocení přesnost. Model však dozvěděli slovníku N-g a jejich gramáží z datovou sadu školení. Proto doporučujeme používejte tento slovník a tyto gramáží extrahování funkce z testovací data, aby vytvořili novou slovníku. Proto jsme přidat modul extrahovat funkce N-gramatika bodování větve testu připojení slovníku výstup z pobočky školení a nastavte slovník režim jen pro čtení. Jsme taky zakázat filtrování N-g podle frekvence nastavte minimální a 1 instance maximální na 100 % a vypněte výběr funkcí.

Po textový sloupec v testovací data byla transformována číselné funkce sloupce, jsme vyloučit sloupce řetězec z předchozích fází jako ve vzdělávání větvi. Potom používáme skóre modelu modul aby předpovědí a modulu vyhodnocení modelu, který chcete vyhodnotit přesnost.

## <a name="step-5-deploy-the-model-to-production"></a>Krok 5: Nasazení modelu výrobní

Model hotovou téměř být nasazené na výroby. Nasazené webové služby, trvá volné textového řetězce jako vstup a vrátí předpověď "" nebo "vysoké." Použije zkušenosti N-gramatika slovník pro převod textu na funkce a školení logistickou regresní model a dosáhnout předpovídání pomocí těchto funkcí. 

Pokud chcete nastavit prediktivní testu, jsme nejdřív je uložit slovník N-gramatika jako datovou sadu a školení logistickou regresní model z větvi školení testu. Uložte jsme testu pomocí příkazu "Uložit jako" pro vytvoření grafu experiment pro prediktivní testu. Doporučujeme odebrat pobočky školení a modul rozdělenými daty testu. Jsme připojte dříve uložené N-gramatika slovník a model extrahovat funkce N-gramatika a skóre modelu moduly, respektive. Odebrání taky modulu vyhodnocení modelu.

Vložení vybrat sloupce v modulu datovou sadu před modulu předběžně zpracovat Text, který chcete odebrat popisek sloupce jsme zrušení výběru možnost "Skóre sloupec pro datovou sadu" v modulu skóre. Tímto způsobem webová služba nepožaduje popisek, který se pokouší předpovídání a slouží není ozvěnu vstupní funkcemi v odpovědi.

![Prognostické testu](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Teď máme pokus, kterou lze publikovat jako webové služby a s názvem použití odpovědi na žádosti o nebo dávku spuštění rozhraní API.

## <a name="next-steps"></a>Další kroky

Další informace o modulů analýzy text [MSDN přečtěte následující dokumentaci pro] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
