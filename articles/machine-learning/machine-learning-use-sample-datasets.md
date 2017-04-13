<properties
    pageTitle="Pomocí výše uvedené množiny dat ukázkové ve počítače výukové studiu | Microsoft Azure"
    description="Popisy výše uvedené množiny dat použít v ukázkové modelech součástí ML Studio Pomocí těchto sad ukázkových dat pro váš pokusy."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Pomocí výše uvedené množiny dat vzorku v Azure počítače výukové Studio

[top]: #machine-learning-sample-datasets

Při vytváření nového pracovního prostoru Azure počítač přečíst celou řadu počítány a pokusy jsou zahrnuty ve výchozím nastavení. Mnoho z těchto sad ukázkových dat se používají modely vzorku v [Azure Cortana Intelligence Galerie](http://gallery.cortanaintelligence.com/)a ostatní jsou však započítávány jako příklady různých typů dat obvykle používají ve počítače výukové.

Některé z těchto sad dat jsou dostupné v úložišti objektů Blob Azure. Pro tyto uvedené množiny dat v tabulce níže najdete přímý odkaz. Těchto sad dat můžete používat ve vaší pokusy pomocí [Importovat Data] [ import-data] modul.

Zbytek těchto sad ukázková data jsou uvedené v části **Uložit datové sady** v modulu palety nalevo od plátno experiment při otevřete nebo vytvořte nové experiment v ML Studio.
Můžete některou z těchto sad dat ve vlastní experiment přetažením experiment plátno.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Název sady dat</th>
  <th align=left>Popis sady dat</th>
</tr>

<tr>
  <td valign=top>Dospělého datovou sadu sčítání příjem binární klasifikace</td>
  <td valign=top>
Podmnožinu databázi sčítání 1994 pomocí dospělců pracovních přes stáří 16 s indexu upravená příjem > 100.<p> </p><b>Použití:</b> klasifikovat uživatelé, kteří používají účastníky procesu odhadnout, zda osoba připadá víc než 50K za rok.<p> </p><b>Související zdroje informací:</b> Kohavi, r, Becker, b, (1996). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Datovou sadu kódy letišť</td>
  <td valign=top>
Kódy letišť USA.<p> </p>Tento datovou sadu obsahuje jeden řádek pro každou letiště USA poskytuje letiště identifikační číslo a název spolu s umístění měst a států.
  </td>
</tr>

<tr>
  <td valign=top>Údaje o cenách automobilů (suroviny)</td>
  <td valign=top>
Informace o automobily tak, že volání a model včetně cena, funkce, například počet lahve a MPG, jakož i skóre pojištění rizika.<p> </p>Riziko skóre původně souvisí s cenou automatického a potom upravit pro skutečné riziko ve výrobním procesu známé znalci jako symboling. Hodnota + 3 označuje, že je riskantní, automatické a hodnoty -3 je pravděpodobně poměrně bezpečí.<p> </p><b>Použití:</b> předpovídání skóre rizika pomocí funkcí, pomocí regresní nebo multivariate klasifikace. <p> </p><b>Související zdroje informací:</b> Schlimmer, J.C. (1987). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Bike pronájem UC sadu</td>
  <td valign=top>
Bike pronájem UC datovou sadu, založený na skutečné data z velké Bikeshare společnost, která udržuje síť pronájem bike v WA Datacentrum.<p> </p>Datové má jeden řádek pro každou hodinu každý den v 2011 a 2012, pro celkových 17,379 řádky. Oblast hodinové bike splátky je od 1 do 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Obrázek faktury bran RGB</td>
  <td valign=top>
Veřejně dostupný obrázek souboru převedou na data ve formátu CSV.<p> </p>Kód pro převod obrázek není uvedený na stránce <strong>pro změnu barvy quantization pomocí clusterů prostředky K</strong> podrobností modelu.
  </td>
</tr>

<tr>
  <td valign=top>Sledování krevního daru dat</td>
  <td valign=top>
Podmnožinu dat z databáze sponzora sledování krevního transfuzním Service Center Hsin Chu Město, Tchaj-wan.<p> </p>Sponzora data obsahují měsíců od posledního daru) a četnost nebo celkový počet dary, čas od posledního daru a počtu sledování krevního poskytnuto.<p> </p><b>Použití:</b> cílem je předpovídání prostřednictvím klasifikace, zda sponzora poskytnuto sledování krevního v březnu fungovat 2007, kde 1 označuje sponzora během cílové období a 0 není sponzora. <p> </p><b>Související zdroje informací:</b> Já, systémem, (2008). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače <p> </p>Já, můžu-vývoj: Cheng Yang, král-Jang a si toho, jde Tao "Knowledge zjišťování režim omezené Funkčnosti modelu pomocí Bernoulliho posloupnost" systémy odborník na prezentace s aplikacemi, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Recenzí z Amazon</td>
  <td valign=top>
Recenze knih v Amazon přijatá z webu amazon.com výzkumných Vysokoškoláky Pensylvánie (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">myšlenkou</a>). V tématu papíru zdroje informací "biografie, Bollywood, výložníku polí a Blenders: přizpůsobení domény pro myšlenkou klasifikace" Jan Blitzer, Dredze označit a Fernando Pereira; Přidružení výpočetní lingvistické (ACL) 2007.<p> </p>Původní datovou sadu má 975K recenze s pořadím 1, 2, 3, 4 a 5. Recenze byly vytvořeny v angličtině a jsou z časového období 1997 2007. Tento datovou sadu byl odběr dolů k recenze 10 kB.
  </td>
</tr>

<tr>
  <td valign=top>Zadní rakoviny dat</td>
  <td valign=top>
Jedna z tři související rakoviny datové sady poskytovanou radiology instituce, která se zobrazí často ve počítače výukové dokumentace. Sloučí diagnostických informací pomocí funkcí z laboratorní analýzy 300 blíží tkáňových vzorků.<p> </p><b>Použití:</b> klasifikaci typu rakoviny, na základě 9 atributů, některé z nich jsou lineární a jiné kategorií. <p> </p><b>Související zdroje informací:</b> Wohlberg, čísel, ulice, W.N. a Mangasarian, O.L. (1995). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Funkce rakoviny zadní <td valign=top>
Datové informacemi 102K podezřelých oblastí (kandidátů) X-ray obrázků popsány 117 funkcemi. Funkce jsou speciální a jejich význam není zjištěné při tvůrci datovou sadu (Siemens zdravotní péče). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Informace o rakoviny zadní</td>
  <td valign=top>
Datové obsahuje dodatečné informace pro každou oblast podezřelé rentgenového. Každý příklad uvádí informace (například popisek, pacienta ID, souřadnice opravy relativní celý obrázek) o odpovídající počtu řádků v sadě dat zadní rakoviny funkce. Každého pacienta obsahují několik příkladů. Pro pacientů, kteří mají rakovinu, některé příklady jsou kladné a jiné záporné. Pro pacientů, kteří nemají rakovinu není záporná příkladech pro všechny. Datové obsahuje příklady 102K. Tendenční datové 0,6 % bodů jsou kladné, zbývající, není záporná. Datové byl k dispozici ve zdravotnictví Siemens.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Popisky Appetency CRM sdílí</td>
  <td valign=top>
Popisky z KDD Cup 2009 zákazníka relace předpovědí ověřovací kód programu (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>Popisky konve CRM sdílí</td>
  <td valign=top>
Popisky z KDD Cup 2009 zákazníka relace předpovědí ověřovací kód programu (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Sdílené datové sady CRM</td>
  <td valign=top>
Tato data pochází z KDD Cup 2009 zákazníka relace předpovědí ověřovací kód programu (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Datové obsahuje 50K zákazníků společnosti francouzština Telecom oranžovou. Jednotlivé zákazníky má 230 anonymních funkce 190 z nich jsou číselné a 40 jsou kategorií. Funkce jsou velmi sparse.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Popisky Upselling CRM sdílí</td>
  <td valign=top>
Popisky z KDD Cup 2009 zákazníka relace předpovědí ověřovací kód programu (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energetický efektivity regresní dat</td>
  <td valign=top>
Sbírka simulovaný energie profily, podle 12 jiné budovy obrazce. Budovy lišit s ohledem na liší 8 funkce, třeba použitém oblast, sklo oblasti pro rozdělení a orientaci.<p> </p><b>Použití:</b> pomocí regresní nebo klasifikace předpovídání energetické účinnosti hodnocení závislosti mezi dvěma skutečné hodnotami odpovědi. Klasifikace více předmětu je zaokrouhlit proměnnou odpověď na nejbližší celé číslo. <p> </p><b>Související zdroje informací:</b> Xifara A. & Tsanas A. (2012). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Zpoždění letů dat</td>
  <td valign=top>
Osobní letech ve včasných data o výkonu z TranStats shromažďování dat USA oddělení o přenosu (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Ve včasných</a>).<p> </p>Datové zahrnuje časové období duben říjen 2013. Před odesláním Azure ML Studio, datové zpracování následujícím způsobem:<ul><li>Datové byla filtrována pokrýval pouze 70 nejvytíženějšímu letišť v kontinentální spojené</li><li>Zrušené lety byly označený jako posunout delší než 15 minut</li><li>Byly odfiltrované odkloněných lety</li><li>Jste vybírali následující sloupce: rok, měsíc, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, zrušeno</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Výkonu ve včasných letech (suroviny)</td>
  <td valign=top>
Záznamy letadla ve letu hlediska příletů a odletů ve Spojených státech od Říjen 2011.<p> </p><b>Použití:</b> předpovídání zpoždění letů. <p> </p><b>Související zdroje informací:</b> z USA oddělení dopravy <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Doménové aktivuje dat</td>
  <td valign=top>
Daty, počasí, například teplotní a vlhkostní indexy a rychlost větru z oblasti severovýchodní Portugalsko, spolu s záznamy o struktuře aktivována.<p> </p><b>Použití:</b> tento složité regresní úkol je, kde je odhadnout karamelizovaný oblasti doménové aktivována. <p> </p><b>Související zdroje informací:</b> Cortez, t. a Morais A. (2008). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače <p> </p>[Cortez a Morais 2007] P. Cortez a A. Morais. Dat dolování přístup doménové aktivována předpovídání pomocí povětrnostních Data. V J. Neves M. F. Santos a Edit.: J. Machado, nové trendy v umělé měřítka, řízení 13 EPIA 2007 – portugalštiny konference umělé Intelligence prosinec, 523-Guimarães, Portugalsko, s. 512, 2007. APPIA, ISBN-13 978 – 989 95618-0 – 9. Jsou dostupné na webu: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Datovou sadu UC němčina platební kartou</td>
  <td valign=top>
UC Statlog (němčina platební kartou) datové sady (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + němčina + platební + dat</a>) pomocí souboru german.data.<p> </p>Datové klasifikuje lidé označená sadu atributy jako nízké nebo vysoké platební rizika. Každý příklad představuje osoby. Mívají 20 funkce číselných a kategorií, popisku binární (platební hodnota rizika). Vysoký Dal rizika položek máte popisek = 2, zhoršeným Dal rizika položek máte popisek = 1. Náklady misclassifying nízké riziko příklad jako vysoké je 1, zatímco náklady misclassifying vysokým rizikem příklad co nejnižší 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>Názvy IMDB filmu</td>
  <td valign=top>
Datové obsahuje informace o videa, které byly hodnocení v Twitter tweety: IMDB film ID název videa a žánr, výrobní rok. V datové sadě jsou filmy 17 kB. Datové byla zavedená v knize "S. Dooms, T. De Pessemier a L. Martens. MovieTweetings: film hodnocení datovou sadu odebrané Twitter. Dílny na Crowdsourcing a lidské výpočtu pro systémy doporučení, CrowdRec na RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>Data Iris dvě třídy</td>
  <td valign=top>
Toto je možná nejlepší známé databáze najdete v dokumentaci rozpoznávání vzorku. Uvedenou množinu dat je poměrně malá, obsahující 50 příklady každý Okvětní lístek naměřené hodnoty ze tří odrůd iris.<p> </p><b>Použití:</b> předpovídání iris typ z hodnoty.  <p> </p><b>Související zdroje informací:</b> Fisher R.A. (1988). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Video Tweety</td>
  <td valign=top>
Datové je rozšířená verze datovou sadu Tweetings video. Datové má 170K hodnocení filmy extrahovaných z strukturované tweety na Twitter. Jednotlivé instance představuje do tweetu a je řazené kolekce členů: ID uživatele IMDB film ID, hodnocení, časové razítko, počet oblíbených položek pro tuto tweetu a počet retweets tento tweetu. Datové byl k dispozici A. Said, S. Dooms, B. Loni a D. Tikk doporučení systémy ověřovací kód programu 2014.
  </td>
</tr>

<tr>
  <td valign=top>MPG data automobily různých</td>
  <td valign=top>
Tento datovou sadu je mírně změnila verze datovou sadu poskytovanou knihovnu StatLib Carnegie TruSecure. Datové byla použita v 1983 American statistické přidružení budeme.<p> </p>Data jsou uvedeny spotřeby paliva pro různé automobily v km / galon spolu s informacemi o takové číslo lahve posunutí engine, koňské síly, celková hmotnost akcelerace a.<p> </p><b>Použití:</b> předpovídání paliva na základě 3 s více hodnotami samostatné atributy a 5 nepřetržitý atributy. <p> </p><b>Související zdroje informací:</b> StatLib, Carnegie TruSecure (1993). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr>
  <td valign=top>Datovou sadu Pima indiánů Diabetes binární klasifikace</td>
  <td valign=top>
Podmnožinu dat z vnitrostátní instituce Diabetes a útrobní a ledviny nemocí databáze. Datové byl filtrované fokus na samice pacientů indických dědictví Pima. Data obsahují lékařích data například cukru v krvi a inulinový úrovně, jakož i faktory životní styl.<p> </p><b>Použití:</b> předpovídání, zda předmět obsahuje diabetes (binární klasifikace). <p> </p><b>Související zdroje informací:</b> Sigillito V. (1990). Počítač UC výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače </td>
</tr>

<tr>
  <td valign=top>Data o zákaznících restaurace</td>
  <td valign=top>
Sada metadata zákazníků, včetně účastníky procesu a předvolby.<p> </p><b>Použití:</b> pomocí této sadě dat v kombinaci s dalších dvou restaurace sady dat, školení a testování systému doporučení. <p> </p><b>Související zdroje informací:</b> Bache, K. a Lichman, M. (2013). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače.
  </td>
</tr>

<tr>
  <td valign=top>Data funkcí restaurace</td>
  <td valign=top>
Sada metadata restaurace a jejich funkce, jako je typ jídla, obědvajících styl a místo.<p> </p><b>Použití:</b> pomocí této sadě dat v kombinaci s dalších dvou restaurace sady dat, školení a testování systému doporučení. <p> </p><b>Související zdroje informací:</b> Bache, K. a Lichman, M. (2013). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače.
  </td>
</tr>

<tr>
  <td valign=top>Hodnocení restaurace</td>
  <td valign=top>
Obsahuje hodnocení dané uživatelům restaurace v měřítku od 0 2.<p> </p><b>Použití:</b> pomocí této sadě dat v kombinaci s dalších dvou restaurace sady dat, školení a testování systému doporučení. <p> </p><b>Související zdroje informací:</b> Bache, K. a Lichman, M. (2013). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače.
  </td>
</tr>

<tr>
  <td valign=top>Oceli datovou sadu Annealing více třídy</td>
  <td valign=top>
Tento datovou sadu obsahuje řadu obsahující záznamy ze oceli žíhání pokusy s fyzické atributy (šířka, tloušťky, typ (smyčka, listu, atd.) výsledné oceli typů.<p> </p><b>Použití:</b> předpovídání všech atributů dvě číselné třídy; tvrdost nebo síly. Může taky analyzovat korelace mezi atributy.<p> </p>Oceli hodnocení postupujte standardní sada definované SAE a jinými organizacemi. Hledáte konkrétní "testu" (proměnné třídy) a znát hodnoty potřebné. <p> </p><b>Související zdroje informací:</b> šterlinků, D. & Buntine w (NEDEF). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie, školy informace a jiné vědecké počítače <p> </p>Užitečné Průvodce oceli hodnocení najdete tady: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Telescope dat</td>
  <td valign=top>
Záznamy vysoké energie gama částic roztržení spolu s šum na pozadí, obě simulovaného pomocí typu Monte Carlo procesu.<p> </p>Záměr simulace byla ke zlepšení přesnosti tratě povětrnostních Cherenkov gama dalekohledy, pomocí statistických metod k rozlišení mezi požadovanou signálem (Cherenkov záření sprchy) a šum na pozadí (hadronic sprchy zahájil: po cosmic paprsky v prostředí velká).<p> </p>Data byla předem zpracovaných k vytvoření podlouhlého clusteru s long osy otočen směrem do středu fotoaparátu. Vlastnosti této Elipsa (někdy označovány jako Hillas parametry) patří mezi obrázek parametrů, které se dá použít pro diskriminace.<p> </p><b>Použití:</b> předpovídání, zda obrázek na oslavu představuje šum signál nebo pozadí.<p> </p><b>Poznámky:</b> jednoduchý klasifikace přesnost je smysluplný pro tato data od klasifikaci událost nastavit jako pozadí je horší než klasifikaci událost signálu jako pozadí signálu. Porovnání různých třídění bude použito ROC grafu. Pravděpodobnost přijetí událost nastavit jako pozadí pod některým z následujících mezní hodnoty musí být signál: 0,01 0,02, 0,05, 0,1 nebo 0,2.<p> </p>Všimněte si také, že počet událostí pozadí (pro hadronic sprchy h) podceňována, zatímco v skutečné hodnoty třídy h nebo šum představuje většinou události. <p> </p><b>Související zdroje informací:</b> Bock, R.K. (1995). Výukové úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>stroj UC. Irvine, CA: Vysokoškoláky Kalifornie školy informace </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Datovou sadu počasí</td>
  <td valign=top>
Každou hodinu na základě Nevyžádaná pošta je přesunuta počasí připomínek NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">sloučená data z 201304 201310</a>).<p> </p>Údaje o počasí zahrnuje připomínky z místům počasí letiště překrývajícím časové období duben říjen 2013. Před odesláním Azure ML Studio, datové zpracování následujícím způsobem:<ul><li>ID počasí stanice byly namapované odpovídající letiště ID</li><li>Byly odfiltrované počasí stanic nejsou spojeny s 70 nejvytíženějšímu letišť</li><li>Díky sloupci Datum byl rozdělit na samostatných sloupcích rok, měsíc a den</li><li>Jste vybírali následující sloupce: AirportID, rok, měsíc, den, čas, podle časového pásma, SkyCondition, viditelnost, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, rychlost větru, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, výškoměru</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedie SP 500 datové sady</td>
  <td valign=top>
Data je odvozeno z Wikipedie (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) založené na články S & P 500 společnosti, uložených jako dat XML.<p> </p>Před odesláním Azure ML Studio, datové zpracování následujícím způsobem:<ul><li>Extrahuje text obsah pro každou konkrétní společnost</li><li>Odebrání formátování wikiwebu</li><li>Odebrat jiné než alfanumerické znaky</li><li>Převést text na malá písmena.</li><li>Byly přidány známé společnosti kategorie</li></ul><p> </p>Všimněte si, že pro některé společnosti článek nalezena, takže počtu zobrazených záznamů je menší než 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Datové obsahuje data o zákaznících a údajů o svou odpověď přímé poštovní kampaní. Každý řádek představuje zákazníka. Datové funkcemi, 9 o účastníky procesu uživatele a dřívější chování a 3 popisek sloupce (navštívit, převod a výdajů).  Navštivte web se sloupcem binární označuje, že zákazníka navštívili po marketingové kampaně, převod označuje, zákazníků koupili něco a věnovat se o velikost, který uplynul.  Datové byl zpřístupnili tak, že Jan Hillstrom pro MineThatData e-mailu technologie pro analýzu a Data dolování ověřovací kód programu.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Funkce test příklady v sadě dat RCV1 V2 Reuters příspěvky. Datové má 781K články spolu s jejich ID (první sloupec datové). Každý článek tokenizovaný stopworded se nasadí. Datové byl k dispozici tak, že David. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Funkce příklady školení v sadě dat RCV1 V2 Reuters příspěvky. Datové má 23K články spolu s jejich ID (první sloupec datové). Každý článek tokenizovaný stopworded se nasadí. Datové byl k dispozici tak, že David. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Datová sada o od KDD Cup 1999 Knowledge zjišťování a Data dolování nástroje soutěže (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Datové jste stáhli uložených v úložišti objektů Blob Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) a cs zahrnuje odeslání školení a testování datové sady. Školení datovou sadu má přibližně 126K řádcích a sloupcích 43, včetně popisků; 3 sloupci jsou součástí informací popisku a 40 sloupce tvořené funkce číselné a řetězec/kategorií, které jsou k dispozici pro školení modelu. Testovací data má přibližně 22,5 K testování příkladů s stejné 43 sloupce jako školení data.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Téma přiřazení diskusních příspěvků v sadě dat RCV1 V2 Reuters příspěvky. Zpravodajský článek můžete přidělovat tématům. Formát každý řádek je "<topic name>  <document id> 1". Datové obsahuje téma přiřazení 2.6M. Datové byl zpřístupnili tak, že David. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Tato data pochází z KDD Cup 2010 Student výkonu hodnocení ověřovací kód programu (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">student výkonu hodnocení</a>). Data použitá je sada Algebra_2008_2009 školení (Stamper, J., Niculescu-Mizil A. Ritter, S. Gordon, G.J. a Koedinger, k. r. (2010). Algebra můžu 2008 2009. Ověřovací kód programu sadám dat z KDD Cup 2010 vzdělávací dat dolování ověřovací kód programu. Jestli chcete na <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> nebo <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Datové jste stáhli uložených v úložišti objektů Blob Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) a obsahuje protokoly z student tutoring systému. Předaném funkce patří ID problému a stručný popis, ID studenta, časové razítko a kolik pokusy student před řešení tohoto problému správné způsobem. Původní datovou sadu obsahuje záznamy 8,9 M, tento datovou sadu má dolů odběr první každý druhý řádek 100 kB. Datové má 23 odděleného tabulátory sloupce různých typů: číselné kategorií a časové razítko.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
