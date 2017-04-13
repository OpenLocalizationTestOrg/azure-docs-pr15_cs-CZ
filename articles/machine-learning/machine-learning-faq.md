<properties
    pageTitle="Azure počítače výukové nejčastější dotazy týkající se | Microsoft Azure"
    description="Azure počítače výukové Úvod: Nejčastější dotazy týkající se fakturace, funkce a omezení do cloudové služby pro efektivnější prediktivní modelování."
    keywords="počítač výukové úvod, prediktivní modelování, co je počítač výuky"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="garye"/>

# <a name="azure-machine-learning-frequently-asked-questions-faq-billing-capabilities-limitations-and-support"></a>Azure výukové počítače nejčastější dotazy:, funkce, omezení podpory fakturace a

V tomto článku najdete odpovědi na otázky týkající se Azure počítače výukové do cloudové služby pro vývoj prediktivní modely a operationalizing řešení prostřednictvím webové služby. V tomto článku popisuje dotazy týkající se používání služby včetně fakturační modelu, funkce, omezení a podpora.

## <a name="general-questions"></a>Obecné otázky

**Co je Azure počítače výukové?**

Azure učení počítače je plně spravované služby, které můžete použít k vytváření, otestovat, ovládání a správa prediktivní analytický řešení v cloudu. Pouze v prohlížeči můžete přihlášení, odeslat data a okamžitě začít pokusy výukové počítače. Prognostické modelování a přetažením velké palety moduly a knihovně výchozích šablon něco v ní běžné počítače výukové úkoly jednoduchého a snadné.  Další informace najdete v tématu [Přehled služby Azure počítače učení](https://azure.microsoft.com/services/machine-learning/). Úvod výukové počítače překrývajícím klíčové terminologií a pojmy naleznete v tématu [Úvod do výukové Azure počítače](machine-learning-what-is-machine-learning.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co je Studio výukové počítače?**

Počítač výukové Studio je workbench prostředí, které můžete získat přístup prostřednictvím webového prohlížeče. Počítač výukové Studio hostuje palety modulů vizuální složení rozhraní, který umožňuje vytvářet začátku do konce, pracovního postupu pro výzkum data ve formě pokus.

Další informace o počítači výukové Studio najdete v tématu [Co je počítač výukové Studio?](machine-learning-what-is-ml-studio.md)

**Co je služba strojového výukové rozhraní API aplikace?**

Služba strojového výukové API umožňuje nasadit prediktivní modelů, třeba můžou být vytvořené ve počítače výukové studiu jako scalable, chybám, webové služby. Vytvořené pomocí služeb strojového výukové API webových služeb jsou REST API poskytující rozhraní pro komunikaci mezi externích aplikacích a prediktivní analýzy modely.

Další informace najdete v tématu [připojení webové služby strojového výukové](machine-learning-connect-to-azure-machine-learning-web-service.md).

**Kde jsou uvedeny Moje klasické webových služeb? Kde jsou uvedeny Moje nových webových služeb na základě správce prostředků Azure?**

Klasické webovým službám a nové správce prostředků Azure pomocí webové služby jsou uvedené v portálu [Microsoft Azure počítače výukové webové služby](https://services.azureml.net/) . 

Klasický webovým službám najdete taky ve [Počítače výukové studiu](http://studio.azureml.net) na kartě Web služby.

## <a name="microsoft-azure-machine-learning-web-service-questions"></a>Otázky Microsoft Azure počítače výukové webové služby

**Co je Azure počítače výukové webových služeb?**

Počítač výukové webovým službám poskytují rozhraní mezi aplikací a počítač výukové bodování modelu pracovního postupu. Pomocí webové služby Azure počítače výuka, externí aplikaci komunikovat s počítače výukové pracovního postupu bodování modelu můžete v reálném čase. Volání webové služby strojového výukové vrátí výsledky předpovědí externí aplikaci. Aby volání webové služby strojového výukové předáte rozhraní API klíč, který byl vytvořený při nasazení webové služby. Webová služba strojového výukové vychází z REST, výběr oblíbených architektura webu programování projekty.

Azure výukové počítač má dva typy služeb:

* Odpovědi na žádosti o službu (RR) - zhoršeným latence, vysoce scalable služba, která poskytuje rozhraní pro příslušnosti modely vytvořené a nasazeny z Studio výukové počítače.
* Dávkové spuštění služby (BES) - asynchronní služby, které skóre a dávka pro záznamy.

Existuje několik způsobů, jak používat rozhraní REST API a přístup k webové službě. Aplikace můžete napsat například v jazyce C# R a Python pomocí kódu vzorku za vás přihlášení vygenerované při nasazení webové služby.

Ukázkový kód neexistuje na: spotřebovávat stránky webové služby na portálu Azure počítače výukové webové služby.
Rozhraní API stránce Nápověda v řídicím panelu webové služby ve počítače výukové studiu.

Nebo můžete použít ukázkový sešit Microsoft Excel vytvoří (také k dispozici v řídicím panelu webové služby v Studio).

**Jaké hlavní aktualizace pomocí nových webových služeb ML Azure?**

Další informace o nových výukové webových služeb strojového Azure vyhledejte [si přečtěte následující dokumentaci související](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>Počítač výukové Studio otázky

### <a name="importing-and-exporting-data-for-machine-learning"></a>Import a export dat pro vzdělávání počítače

**Jaké zdroje dat podporuje výukové počítače?**

Načtení dat do počítače výukové Studio experiment jedním ze tří způsobů: uložením souboru místní jako datovou sadu pomocí modulu importem dat z datové služby cloudu nebo importem datovou sadu uložili z jiného experimentovat. Zobrazíte další informace o podporovaných formátech souborů najdete v článku [Import dat školení do počítače výukové Studio](machine-learning-data-science-import-data.md).


#### <a id="ModuleLimit"></a>Jak velké uvedenou množinu dat lze pro svůj moduly?

Moduly ve počítače výukové studiu podporu běžné případy použití datové sady až 10 GB hustotu číselná data. Pokud modulu zabírá víc než jeden vstup, 10 GB představuje součet všech vstupní velikostí. Můžete taky přehrajte větší datové sady prostřednictvím podregistru nebo databáze SQL Azure dotazů nebo tak, že výuka podle počty předzpracování před požití.  

Následující typy dat můžete rozbalit do větších datové sady během funkce normalizace a jsou omezené na menší než 10 GB:

- Sparse
- Kategorií
- Řetězce
- Binární data

Následující moduly jsou omezené na datové sady menší než 10GB:

- Doporučení modulů
- SMOTE modul
- Skriptování moduly: R, Python, SQL
- Moduly, kde může být větší než zadávání dat, například spojení nebo algoritmus hash funkce velikost dat výstupu.
- Křížového ověřování, ladění Hyperparameters modelu, anglických řadových regresní a jeden a všechny Multiclass, když počet iterací je obrovské.

Pro datové sady větší než několik GB by měl odeslání dat do Azure úložiště nebo databáze SQL Azure nebo použití HDInsight, nikoli přímo nahrávání z místního souboru.


####<a id="UploadLimit"></a>Jaká jsou omezení nahrávání dat?
Pro datové sady větší než pár GB odeslat dat Azure úložiště nebo databáze SQL Azure nebo použití HDInsight, nikoli přímo nahrávání z místního souboru.

**Můžete číst data z Amazon S3?**

Pokud máte malou část dat a chcete vystavit přes http URL a pak můžete [Importovat Data] [ import-data] modul. Pro všechny větších krocích data, která chcete přenést do úložiště Azure a potom použijte [Importovat Data] [ import-data] modul přenést do svého testu.
<!--
<SEE CLOUD DS PROCESS>
-->

**Existuje možnost vstupní předdefinované obrázek?**

Informace o zadávání možnost [Importovat obrázky] obrázek[ image-reader] odkaz.

### <a name="modules"></a>Moduly

**Algoritmus, zdroje dat, formát dat ve sloupcích nebo operace transformace dat, které hledám není v Azure počítače výukové Studio. Jaké mám možnosti?**

Můžete navštivte [Fórum komunity názory uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231) zobrazíte poslat žádost o funkci, které jsme sledování. Přidejte svůj hlas na žádost o Pokud možnost, kterou hledáte už požádal. Pokud možnost, kterou hledáte neexistuje, vytvořte novou žádost o. V tomto fórum můžete zobrazit stav žádosti o příliš. Sledovat tento seznam jsme často aktualizovat stav dostupnosti funkcí. Kromě toho s integrovanou podporu pro R a Python vlastní transformace lze vytvořit podle potřeby.


**Se dají přenést svůj existující kód do počítače výukové Studio?**

Ano, můžete přenést existující R nebo Python kód do počítače výukové Studio, spustit ve stejné experimentovat s Azure počítače výukové žáky a nasazení řešení jako webové služby prostřednictvím výukové počítače Azure. Další informace najdete v tématu [rozšířit experimentovat s R](machine-learning-extend-your-experiment-with-r.md) a [Automatické spuštění Python výukové skriptů v Azure počítače výukové Studio](machine-learning-execute-python-scripts.md).

**Je možné použít nějak [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) stanovit vzor?**

Ne, která není podporována, však vlastního kódu R a Python mohou sloužit k definování modulu.

**Kolik moduly můžete můžu paralelně provádět v mém pokus?**  

Až se čtyřmi moduly lze spustit paralelně pokus.


### <a name="data-processing"></a>Zpracování dat.

**Existuje možnost vizualizace dat (pokud už jste vizualizace R) interaktivně v rámci testu?**

Po kliknutí na výstup modulu, můžete vizualizace dat a získání statistických údajů.

**Náhled výsledků nebo dat v prohlížeči, počet řádků a sloupců je nutné omezit velikost, proč?**

Vzhledem k tomu data odeslána do prohlížeče a může být velký, velikost dat se omezí na zabránit zpomalovat Studio výukové počítače. Vizualizace všechna data/výsledek, je lepší stáhnout data a používat aplikace Excel nebo jiný nástroj.

### <a name="algorithms"></a>Algoritmů

**K čemu existující algoritmů podporují ve počítače výukové studiu?**

Počítač výukové Studio poskytuje stavu techniky algoritmů Scalable zesílen rozhodnutí stromečky, doporučení Hodnota bayesovského systémy hloubkové Neural sítích, a rozhodnutí Jungles vyvinuté na Microsoft Research. Součástí jsou také balíčků výukové Scalable otevřít zdrojového počítače jako Vowpal Wabbit. Počítač výukové Studio podporuje počítače výukové algoritmy pro multiclass a binární klasifikace regresní a clusterů. Zobrazit úplný seznam modulů [výukové počítače][machine-learning-modules].

**Můžete automaticky vpravo počítače výukové algoritmus pro účely Moje data navrhnout?**

Ne, avšak existují různé způsoby ve počítače výukové Studio porovnají výsledky každého algoritmu k určení ten správný pro váš problém.

**Máte k dispozici všechny pokyny k výběru jednoho algoritmus nad jiné pro zadané algoritmů?**
Zjistěte, [jak zvolit algoritmus](machine-learning-algorithm-choice.md).

**Jsou zadané algoritmů napsané v R nebo Python?**

Ne, tyto algoritmy převážně napsané v zkompilované jazyce, poskytovat vyšší výkon.

**Jsou cokoli v podrobnostech algoritmů k dispozici?**

Dokumentace poskytuje některé informace o algoritmech a parametrech přidělený optimalizace jsou popsané optimalizovat algoritmus pro použití.  

**Existuje podpora online školení?**

Ne, je podporována aktuálně jenom programové přeškolení.

**Můžete vizualizace vrstvy Neural čisté modelu pomocí předdefinovaných modul?**

Ne.

**Můžete vytvořit vlastní moduly v jazyce C# nebo jiný jazyk?**

Aktuálně nové vlastní moduly lze vytvořit pouze v R.

### <a name="r-module"></a>Modul R

**Balíčky R, které jsou k dispozici v počítači výukové Studio?**

Počítač výukové Studio podporuje 400 + CRAN R balíčky dnes a tady je [aktuální seznam](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) všechny zahrnuté balíčků. Viz také [rozšířit experimentovat s R](machine-learning-extend-your-experiment-with-r.md) se dozvíte, jak načíst tento seznam. Nejsou-li balíček, který chcete v tomto seznamu, zadejte název balíčku na [fóru názory uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231).

**Je možné vytvářet vlastní modul R?**

Ano, [Autor vlastní R moduly Azure počítač přečíst](machine-learning-custom-r-modules.md) Další informace najdete v.

**Existuje prostředí REPL r?**

Ne, je žádné REPL prostředí R v studio.

### <a name="python-module"></a>Modul Python

**Je možné vytvářet vlastní modul Python?**

Nesynchronizujete ale je možné použít jeden nebo více [Spustit skript Python] [ python] moduly získat stejný výsledek.

**Existuje prostředí REPL pro Python?**

Poznámkové bloky Jupyter můžete použít ve počítače výukové studiu. Další informace najdete v tématu [úvodní informace o Jupyter poznámkových bloků v Azure počítače výukové Studio] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Webové služby

###<a name="retraining-models-programmatically"></a>Programově přeškolení modely

**Jak můžu Retrain výukové počítače Azure modely programově?**

Použití rekvalifikaci rozhraní API. Další informace najdete v tématu [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md). Ukázkový kód je také k dispozici v [Microsoft Azure počítače výukové přeškolení ukázku](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Vytvoření

**Můžete nasadit modelu, místně nebo v aplikaci bez připojení k Internetu?**

Ne.


**Existuje latence podle směrného plánu, kterou očekává pro všechny webové služby?**

V tématu [limity Azure předplatného](../azure-subscription-service-limits.md)

### <a name="use"></a>Použití

**Pokud by chcete spustit Moje prediktivní modelu jako služba spuštění dávky a odpověď na žádost o službu?**

Odpověď na žádost o služby (RR) je latencí minimum, maximum měřítko webová služba, která se používá k poskytování rozhraní příslušnosti modely, které jsou vytvořené a nasazeny z prostředí hodnocení. Spuštění dávky služby (BES) je služba asynchronní hodnocení dávku záznamy. Vstupní BES je podobný zadávání dat použít v záznamech prostředků. Hlavní rozdíl je, že BES přečte blokem záznamy z různých zdrojů, jako jsou objektů Blob služby a služby Tabulka v Azure, databáze SQL Azure HDInsight (podregistru dotaz) a HTTP zdrojů. Další informace najdete v tématu [jak využívat počítače výukové webové služby](machine-learning-consume-web-services.md).

**Jak mám aktualizovat modelu nasazeném webové služby?**

Aktualizace prediktivní modelu už nasazeném služby je jednoduchá – stačí upravili a opětovného spuštění testu, který jste použili k vytváření a školení model uložte. Když máte novou verzi školení modelu k dispozici v Studio výukové počítač zeptá, jestli chcete aktualizovat webové služby. Podrobnosti o tom, jak aktualizovat nasazeném webové služby najdete v tématu [nasazení webové služby strojového výukové](machine-learning-publish-a-machine-learning-web-service.md).

Můžete taky použít rozhraní API přeškolení.
Další informace najdete v tématu [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md). Ukázkový kód je také k dispozici v [Microsoft Azure počítače výukové přeškolení ukázku](https://azuremlretrain.codeplex.com/).

**Jak sledovat Moje webová služba nasazenou v výrobní?**

Po nasazení modelu prediktivní můžete ho sledovat z Azure klasického portál (klasický webové služby pouze) nebo portálu Azure počítače výukové webové služby. Má každá nasazeném služba své vlastní řídicí panel, kde navíc přehledně uvidíte, sledování informací pro službu. Další informace o správě nasazeném webové služby v tématu [Správa webové služby na portálu Azure počítače výukové webovým službám](machine-learning-manage-new-webservice.md) a [Spravovat pracovní prostor výukové počítače Azure](machine-learning-manage-workspace.md).

**Existuje na místo, kde můžete zobrazit výstup Moje RR/BES?**

Záznamy odpovědi webové služby je obvykle tam, kde vidíte výsledek. Můžete také napsat ho k úložišti objektů blob Azure. Pro BES výstup je došlo k zápisu objektů blob ve výchozím nastavení. Můžete taky napsat výstup k databázi nebo tabulky pomocí [Exportovat Data] [ export-data] modul.

**Můžete vytvořit webovým službám jenom z modelů vytvořených ve počítače výukové studiu?**

Ne, můžete taky vytvořit webové služby přímo z poznámkových bloků Jupyter nebo RStudio.

**Kde lze najít informace o kódy chyb?**

Seznam kódy chyb a popisy naleznete v tématu [Kódy chyb modul výukové počítače](https://msdn.microsoft.com/library/azure/dn905910.aspx) .

## <a name="scalability"></a>Škálovatelnost:

**Co je škálovatelnost webová služba?**

V současné době výchozí koncový bod zřízení s 20 souběžně prováděné žádosti o prostředku každý koncový bod. Je možné přizpůsobit pro 200 souběžně prováděné žádosti na jeden koncového bodu a velikost každé webové služby na 10 000 koncové body za webová služba můžete změnit podle popisu v [měřítka webové služby](machine-learning-scaling-webservice.md). Pro BES každý koncový bod umožňuje zpracování žádostí o 40 současně a další požadavky za 40 požadavků jsou ve frontě. Tyto požadavky ve frontě spuštěn jako fronty ztracena.


**Jsou R úlohy rozšířit mezi uzly?**

Ne.  


**Množství zpracovávaných dat můžete použít pro vzdělávání?**

Moduly ve počítače výukové studiu podporu běžné případy použití datové sady až 10 GB hustotu číselná data. Pokud modulu zabírá víc než jeden vstup, je celková velikost pro všechny vstupy společně 10 GB. Můžete taky přehrajte větší datové sady prostřednictvím podregistru nebo databáze SQL Azure dotazů nebo tak, že předzpracování s [učení s počtem] [ counts] moduly před požití.  

Následující typy dat můžete rozbalit do větších datové sady během funkce normalizace a jsou omezené na menší než 10 GB:

- sparse
- kategorií
- řetězce
- binární data

Následující moduly jsou omezené na datové sady menší než 10 GB:

- Doporučení modulů
- SMOTE modul
- Skriptování moduly: R, Python, SQL
- Moduly, kde může být větší než zadávání dat, například spojení nebo algoritmus hash funkce velikost dat výstupu.
- Ověřit křížově ladění Hyperparameters modelu, anglických řadových regresní a jeden a všechny Multiclass, když je velmi velkého počtu iterací.

Pro datové sady větší než několik GB by měl odeslání dat do Azure úložiště nebo databáze SQL Azure nebo použití HDInsight, nikoli přímo nahrávání z místního souboru.


**Jsou všechny vektory omezení velikosti?**

Řádky a sloupce se každý omezuje omezení .NET int maximální: 2 147 483 647.

**Upraví velikost virtuálního počítače, který se používá k spuštění webové služby**

Ne.  

## <a name="security-and-availability"></a>Zabezpečení a dostupnosti

**Kdo má přístup k koncový bod http webové služby ve výchozím nastavení? Jak omezit přístup k koncový bod**

Po zavedení webové služby výchozí koncový bod se vytvoří pro službu. Výchozí koncový bod můžete s názvem pomocí svého rozhraní API klíče. Další koncové body můžete přidat s vlastní klíče z portálu Azure klasické nebo programově pomocí rozhraní API webových služeb správy. Přístupové klávesy jsou potřebné pro volání webové služby. Další informace najdete v tématu [připojení webové služby strojového výukové](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Co se stane, když Můj účet Azure úložiště nebyl nalezen?**

Počítač výukové Studio závisí na předávaných Azure úložiště uživatele ukládat zprostředkovatel dat při spuštění pracovního postupu. Tento účet úložiště je za předpokladu, že na počítač výukové Studio v době se vytvoří pracovního prostoru aplikace Groove. Po vytvoření pracovního prostoru, pokud účtu úložiště odstraněna už najdete, pracovní prostor přestane fungovat a všechny pokusů v tomto pracovním prostoru se nezdaří.

Pokud omylem odstranili účtu úložiště, znovu vytvořte účet úložiště se stejným názvem v oblasti stejným účtem odstraněné úložiště. Poté synchronizován přístupové klávesy.


**Co se stane, když Moje úložiště účtu přístupová klávesa se nesynchronizují?**

Počítač výukové Studio závisí na navrhovaný Azure úložiště uživatele k ukládání zprostředkovatel dat při spuštění pracovního postupu. Tento účet úložiště je za předpokladu, že na počítač výukové Studio v době pracovního prostoru aplikace Groove vytvořené a přístupových kláves souvisí s pracovního prostoru. změny přístupové klávesy, po vytvoření pracovního prostoru, pracovní prostor už nebude mít přístup účtu úložiště. Přestane fungovat a všechny pokusů v tomto pracovním prostoru se nezdaří.

Pokud jste změnili účtu úložiště přístupové klávesy, synchronizován přístupových kláves v pracovním prostoru na portálu Azure klasické.  


## <a name="azure-marketplace"></a>Azure Marketplace

V tématu [Nejčastější dotazy týkající se publikování a používání aplikací ve počítače výukové Marketplace](machine-learning-marketplace-faq.md).

## <a name="support-and-training"></a>Školení a podpora

**Kde můžu získat školicí kurzy pro výukové počítače Azure?**

[Azure počítače výukové si přečtěte následující dokumentaci Centrum](https://azure.microsoft.com/services/machine-learning/) hostitelem videa kurzy a návody. Podrobné návody seznámení se službami a projděte si data pro výzkum životního cyklu importu dat, čištění dat, vytváření prediktivní modely a nasazení ve výrobní s Azure počítače výukové.

Jsme přidáváte nové materiál do výukového centra počítače průběžné. Odesláním žádosti o další výukové materiály na výukové centrum pro počítače na [fóru názory uživatelů](https://windowsazure.uservoice.com/forums/257792-machine-learning).

Školicí kurzy najdete taky v [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Jak získat podporu výukové počítače Azure?**

K získání technické podpory pro Azure počítače výukové, přejděte na [Podporu Azure](/support/options/) a vyberte **Výukové počítače**.

Azure výukové počítač má i fóru komunity na webu MSDN, kde můžete klást otázky týkající se Azure počítače výukové. Fórum kontroloval Azure počítače výukové týmu. Navštivte [Fórum komunity Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Otázky fakturace

**Jak funguje fakturace výukové počítače?**

Existují dvě složky pro službu Azure počítače výukové. Počítač výukové Studio a automatické učení webové služby.

Při vyhodnocování počítače výukové Studio můžete zdarma fakturační osy.  Bezplatné osy umožňuje s omezenou kapacitou nasadit klasické webové služby.

Jakmile jste se rozhodli, že Azure počítače výukové odpovídá vašim potřebám, můžete registraci standardní osy. Abyste si zaregistrovali, musíte mít předplatné Microsoft Azure.

Ve standardním osy je faktura měsíční pro jednotlivých pracovních prostorech definujete ve počítače výukové studiu. Při spuštění pokus v studio je faktura pro využití prostředků při spuštění pokus. Když nasadíte klasické webové služby, transakce a výpočet, že se vám účtovat poplatky hodin na základě platba podle příjmů (srážek daně ze MZDY).

Nové webové služby strojového výukové Představte fakturace plány jednotného zasílání zpráv, pomocí kterých pro další předvídatelnosti náklady. Zákazníkům, kteří potřebují velké množství kapacity vrstvené ceny nabízí diskontního sazby.

Při vytváření plánu se rozhodnete pevné náklady, které jsou součástí však započítávány počet hodin výpočetním rozhraní API a rozhraní API transakce. Pokud potřebujete víc součástí množství, můžete přidat další instance do svého plánu. Pokud potřebujete mnohem víc součástí množství, můžete vyšší úroveň plán, který poskytuje mnohem že víc součástí množství a půjde vám ještě snadněji diskontního sazby.

Když jsou však započítávány množství v existující instance čerpány, další použití bude účtováno průměrných sazbou přidružený k fakturaci osy plán.

Poznámka: Však započítávány množství jsou znovu přidělit každých 30 dní a nevyužitá však započítávány množství není přejít do dalšího období.

Další fakturace a ceny informace najdete v tématu [Počítače studijní ocenění](https://azure.microsoft.com/pricing/details/machine-learning/).

**Má počítač výukové bezplatnou zkušební verzi?**

 Azure výukové počítače mají možnost bezplatného předplatného (viz [Počítače studijní ocenění](https://azure.microsoft.com/pricing/details/machine-learning/) podrobnosti) a počítač výukové Studio obsahuje rychlý hodnocení 8 hodin zkušební verzi k dispozici (přihlášení k [Počítači výukové Studio](https://studio.azureml.net/?selectAccess=true&o=2) pro tuto zkušební verzi).

 Při vaší registraci k Azure bezplatnou zkušební verzi, můžete zkusit Azure služby pro za měsíc. Další informace o bezplatnou zkušební verzi Azure, navštěvujte blog o [Azure bezplatnou zkušební verzi časté otázky](/pricing/free-trial-faq/).

**Co je transakce?**

Transakce představuje volání rozhraní API, která odpovídá výukové počítače Azure. Transakce z hovorů odpovědi na žádosti o službu (záznamy) a spuštění dávky služby (BES) jsou agregované a započítávány plánu fakturace.

**Můžu používat množství zahrnuté transakci v plánu pro záznamy a BES transakce?**

Ano, své transakce z RR a BES agregované se započítávány plánu fakturace.

**Co je hodinu výpočetním rozhraní API?**

Hodiny výpočetním rozhraní API je že fakturační jednotka pro volání rozhraní API čas provést používajících ML výpočetním zdroje. Vaše hovory agregován pro fakturaci.

**Jak dlouho hovor typické výrobní rozhraní API trvat?**

Výrobní rozhraní API volání dobu se může lišit výrazně, obecně od stovky milisekund několik sekund, ale může vyžadovat minut podle toho, složitosti zpracování dat a modelu výukové počítače. Určení standardů modelu pro službu strojového výukové se nejlíp seznámíte odhad doby výroby rozhraní API volání.

**Co je výpočet Studio hodiny?**

Výpočet Studio hodiny je fakturační jednotky agregační přihlášení používané vaší pokusy výpočet zdrojů v studio.

**V nové webové služby co je vrstva zařízením nebo zkoušení určeny?**

Nové webové služby Azure ML obsahují více úrovní, které můžete použít k zřízení plánu fakturace. Odchylka nebo zkoušení osy je osy, která poskytuje omezené množství však započítávány, které umožňují testování vaší experiment jako nové webové služby bez přijímají nákladů. Máte možnost "The Tires zahájení" vás jak to funguje.

**Nejdou nějaké poplatky samostatné úložiště?**

Počítač výukové bezplatné osy nevyžaduje ani povolit samostatné úložiště. Počítač výukové standardní osy vyžaduje, aby uživatelé mít účet Azure úložiště. Azure úložiště je [Fakturované odděleně](https://azure.microsoft.com/pricing/details/storage/).

**Fungování počítače výukové podpory vysoké dostupnosti**

Výrobní rozhraní API volání dobu se může lišit výrazně, obecně od stovky milisekund několik sekund, ale může vyžadovat minut podle toho, složitosti zpracování dat a modelu výukové počítače. Určení standardů modelu pro službu strojového výukové se nejlíp seznámíte odhad doby výroby rozhraní API volání.

**Jaké konkrétní pro využití prostředků mé hovory výrobní rozhraní API poběží?**

Služba strojového výukové je víceklientské služby, a skutečné výpočetním zdrojů použitých v back-end lišit optimalizovaná pro výkon a předvídatelnost.

### <a name="management-of-new-web-services"></a>Správa nové webové služby

**Co se stane, když odstraním Můj plán?**

Plán je odebraný z vašeho předplatného a je faktura poměrná nápovědu.

Poznámka: Nelze odstranit plán, který nepoužívá webové služby. Odstranit plánu, musíte přiřadit nový plán k webové službě nebo odstranění webové služby.

**Jaký je plán instance?**

Instanci plánu je jednotka však započítávány množství, které můžete přidat do svého fakturační plánu. Vyberte fakturační vrstvy u vašeho plánu fakturační přijde s jednou instancí. Pokud potřebujete víc součástí množství, můžete přidat instance vybraného fakturační osy do svého plánu.

**Kolik instancí plán můžete přidat?**

Jednou instancí osy zařízením nebo zkoušení můžete definovat v předplatné.

Úrovně S1, S2 a S3 můžete přidat až potřebné.

Poznámka: V závislosti na vaší očekávané použití může být nákladů efektivnější upgradovat na vyšší úroveň však započítávány množství, spíše než přidáte instance aktuální osy.

**Co se stane při změně plánu úrovní (Upgrade / omezit)?**

Odstranění starý plán a aktuální využití fakturované za poměrná základna. Zbývající období, se vytvoří nový plán s celou však započítávány množství upgradovali/snížit úroveň.

Poznámka: Však započítávány množství přiřazených vztaženou na období a nevyužitá množství není převrácení.

**Co se stane, když mám zvětšit instance v plánu?**

Množství jsou zahrnuty na základě poměrná a může trvat 24 hodin účinné.

**Co se stane, když odstraním instanci plán?**

Instanci je odebraný z vašeho předplatného a je faktura poměrná nápovědu.


### <a name="signing-up-for-new-web-services-plans"></a>Registrace pro plány nové webové služby

**Jak se můžu zaregistrovat pro plán?**

Máte dva způsoby, jak vytvořit fakturační schémata.

Pokud nejprve nasadíte nové webové služby, můžete vybrat existující plán nebo vytvořit nový plán.

Plánů vytvořili tímto způsobem se ve výchozím nastavení oblasti a webové služby má být nasazené na dané oblasti.

Pokud chcete, abyste mohli nasadit služby oblasti jiné než výchozí oblasti, mohou chcete definovat fakturační plány před nasazením služby,

V takovém případě můžete se přihlásit k portálu Azure počítače výukové webovým službám a přejděte na stránku, na plány. Odtud můžete můžete přidat a odstranit plány, i změnit existující plány.

**Jaký plán vyberte budete chtít začít?**

Doporučujeme, že začnete s standardní S1 úroveň a sledovat a služby pro použití. Pokud zjistíte, že používáte však započítávány množství informací, můžete přidat instancí nebo přesunout do vyšší úrovně a získat lepší diskontního sazby. V případě potřeby v celém fakturačním cyklu můžete upravit fakturační plánu.

**Oblasti, které jsou dostupné v na nové plány?**

V oblasti tři výrobní, ve kterých podporujeme nových webových služeb jsou dostupné na nové plány fakturace:

* Jižní centrální USA
* Západní Evropě
* Jihovýchodní Asie

**Mám ve více oblastech webové služby. Je potřeba plán pro všechny oblasti?**

Ano. Plán ceny se liší podle oblasti. Když nasadíte webové služby do jiné oblasti, musíte ji přiřadit konkrétní plánu dané oblasti.

### <a name="new-web-services---overages"></a>Nové webové služby – Overages

**Jak zjistím, pokud je můj web využití služeb v nadsazení?**

Použití můžete zobrazit na vaše plány na stránce plán v portálu Azure počítače výukové webové služby. Přihlaste se k portálu a klikněte na možnost nabídky plány.

V transakce a výpočetním sloupce tabulky, zobrazí se však započítávány množství plán a procento použité.

**Co se stane, pokud použiji nahoru množství zahrnout ve vrstvě zařízením nebo zkoušení?**

Služby, které mají zařízením nebo zkoušení osy jejich přiřazení se zastaví do dalšího období nebo přesunout do jedné ze placené úrovní.

**Klasický webovým službám a Overages nové webové služby jak ceny počítají pro pracovního vytížení požádat o odpověď o prostředcích a dávku (BES)?**

Pro záznamy zatížení bude účtovaná pro každé volání transakce rozhraní API, kdy byla a po dobu výpočetním spojené s těmito požadavky. Transakce rozhraní API výrobní prostředku, který počítá skutečné náklady jako celkový počet rozhraní API volání, který můžete vytvořit vynásobené cenu 1 000 transakce (nastavená hodnota průběžně jednotlivé transakce). Počítá náklady RR rozhraní API výrobní rozhraní API výpočet hodinu jako množství času potřebné pro každé rozhraní API volání spustíte vynásobené celkový počet rozhraní API transakce vynásobené cena za hodinu výpočet rozhraní API výroby.

Například pro standardní S1 nadsazení 1,000,000 transakce rozhraní API, které přebírají 0,72 sekund každé spuštění byste měli mít za následek (1 000 000 *transakce Kč 0,50 / 1K rozhraní API) ve 500 transakce rozhraní API výrobních nákladů a (1 000 000* 0,72 sec * $2 / hr) 400 v výrobní rozhraní API výpočet hodin, celkově $ 900.

BES úlohu bude účtovaná stejným způsobem, ale náklady transakce rozhraní API představuje počet dávku úloh, které odesíláte a výpočetním náklady představují doby výpočetním přidružené tyto úlohy dávku. Transakce rozhraní API BES výrobních nákladů se vypočítávají jako celkový počet úloh odeslaný vynásobené cenu 1 000 transakce (nastavená hodnota průběžně jednotlivé transakce). BES rozhraní API výrobní rozhraní API výpočet hodinové náklady jsou vypočítány jako množství času potřebné pro každý řádek v práci spuštění vynásobený celkovým počtem řádků v práci vynásobené celkový počet úloh vynásobené cena za hodinu výpočet rozhraní API výroby. Při použití počítače výukové kalkulačky měřiči transakce představuje počet úloh, které chcete odeslat a čas v poli transakce představuje kombinované dobu potřebnou pro všechny řádky v každé úlohy na spustit.

Například s standardní S1 nadsazení, pokud odešlete 100 úlohy denně, že každý skládají 500 řádků, které trvat 0,72 sekund a pak bude vaše měsíční průměrných náklady (100 úlohy denně = 3,100 úlohy/měsíc *transakce Kč 0,50 / 1K rozhraní API) 1.55 transakce rozhraní API výrobních nákladů a (500 řádky* 0,72 sec *3,100 úlohy* $2/hr) 620 v výrobní rozhraní API výpočet hodin , pro celkových $621.55.

### <a name="azure-ml-classic-web-services"></a>Azure klasické ML webové služby

**Neexistuje platba podle příjmů dál?**
Ano, klasické webových služeb jsou pořád dostupné v Azure počítače výukové.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Azure počítače výukové zdarma a standardní vrstvy

**Co je součástí Azure počítače výukové bezplatné osy?**

Azure počítače výukové bezplatné osy je určen k poskytování hloubkovou Úvod do Studio výukové Azure počítače. Potřebujete je účet Microsoft pro registraci. Bezplatné osy poskytuje zdarma přístup k jeden pracovní prostor Azure počítače výukové Studio jednoho [účtu Microsoft](https://www.microsoft.com/account/default.aspx). Zahrnuje možnost používání funkce až 10 GB úložiště a možnost umožňují modely jako pracovní rozhraní API. Pracovního vytížení bezplatné osy se nevztahuje SLA a jsou určené pro vývoj a potřebu. Uvolnit osy pracovního vytížení nelze€™ t přístup k datům pomocí připojení k místní SQL server.

**Co je součástí Azure počítače výukové standardní osy a plány?**

Azure počítače výukové standardní osy je zaplacené produkční verzi Azure počítače výukové Studio. Poplatek měsíční studia služby Azure ML fakturované za na základě za měsíc pracovního prostoru a poměrná částečné měsíců. Azure hodin experiment ML Studio je faktura / hod výpočetním pro aktivní hodnocení. Fakturace je průběžně částečné hodin.  

Služby Azure ML API účtovány podle toho, zda je klasické webovým službám nebo nové webové služby.

Následující náklady jsou agregované na pracovní prostor pro vaše předplatné.

* Počítač výukové pracovního prostoru předplatné - předplatné počítače výukové pracovního prostoru je měsíční poplatek, které poskytuje přístup k centru ML Studio a je potřeba spustit pokusů v studio a využití výrobní rozhraní API i.
* Studio experimentovat hodiny – tato metr agregace všechny výpočet nákladů nabíhání pokusy aplikaci ML Studio a spuštěním výrobní volání rozhraní API v testovacím prostředí.
* Datová připojení k serveru SQL místní ve modelech pro vaše školení a bodování.
* Klasický webové služby:
    * Rozhraní API výrobní výpočtu doby - tento metr zahrnuje výpočetním poplatky nabíhání webových služeb spuštěných ve výrobním.
    * Výrobní rozhraní API transakce (1000s) – tento metr zahrnuje poplatky nabíhání za volání webové služby výroby.

Kromě předchozí poplatky, v případě nových webových služeb poplatky agregován plán vybraná:

* Standardní S1/S2/S3 rozhraní API plánování (jednotky) – tento metr představuje typu instance vybraný pro nové webové služby
* Standardní průměrných rozhraní API S1/S2/S3 výpočtu doby - tento metr zahrnuje výpočetním poplatky nabíhání nový Web služby ve výrobním po jsou však započítávány množství v existující instance čerpány. Další použití bude účtováno v overate kurz spojený s S1/S2/S3 plán osy.
* Standardní S1/S2/S3 nadsazení rozhraní API transakce (1,000s) – tento metr obsahuje poplatků za volání výrobní nové webové služby nabíhání po jsou však započítávány množství v existující instance čerpány. Další použití bude účtováno v overate kurz spojený s S1/S2/S3 plán osy.
* Součástí výpočetním rozhraní API množství hodiny – pomocí nových webových služeb, tento metr představuje však započítávány počet hodin výpočet rozhraní API
* Zahrnuté transakce množství rozhraní API (v 1,000s) – pomocí nových webových služeb, tento metr představuje však započítávány množství rozhraní API transakce


**Jak se můžu zaregistrovat bezplatnou ML Azure osy?**

Potřebujete se svým účtem Microsoft. Přejděte na [domovskou stránku Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/)a klepněte na tlačítko **Spustit**. Přihlaste se pomocí svého účtu Microsoft a pracovního prostoru v Free osy se vytvoří. Můžete začít zkoumání a vytvořte počítače výukové pokusy hned.

**Jak se můžu zaregistrovat Azure ML standardní osy?**

Nejdřív musíte mít přístup k předplatnému Azure k vytvoření pracovního prostoru aplikace Groove standardní ML. Registrace k zadání 30denní zkušební verzi Azure neváhejte a novější upgradovat na placené předplatné Azure nebo zakoupit placené přímý Azure předplatné. Můžete pak vytvořit pracovní prostor výukové počítače z portálu Microsoft Azure klasické po získání přístupu k předplatnému. Zobrazení [podrobných pokynů](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Můžete taky můžete být pozvaní vlastníkem standardní ML pracovního prostoru pro přístup k centru vlastníka.

**Můžete zadat vlastní účet úložiště objektů blob Azure pomocí bezplatné osy?**

Ne, standardní osy odpovídá verzi služba strojového výukové, která byla k dispozici před byly vydané úrovně.

**Můžete nasadit počítači výukové modely jako rozhraní API v bezplatné osy?**

Ano, můžete umožňují modely výukové počítače s pracovní rozhraní API služeb jako součást bezplatné osy. Umístění pracovní rozhraní API služeb do provozu a získat výrobní koncový bod pro službu operationalized, je nutné použít standardní osy.

**Jaký je rozdíl mezi Azure bezplatnou zkušební verzi Azure počítače výukové bezplatné osy?**

[Bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/free/) nabízí přeplatky, které se dají použít pro všechny služby Azure pro jeden měsíc. Azure počítače výukové bezplatné osy nabízí nepřetržitý přístup speciálně k služby Azure počítače výukové pro testovacím úloh.

**Jak přejdu pokus z bezplatné vrstvě standardní osy?**

Zkopírujte vaší pokusy z bezplatné vrstvě standardní osy:

1.  Přihlaste se k Azure počítače výukové Studio a ujistěte se, že se zobrazí volného prostoru a standardní pracovního prostoru v pracovním prostoru výběr v horním navigačním panelu.
2.  Přepněte do volného prostoru, pokud jste ve standardním pracovního prostoru.
3.  V zobrazení seznamu experiment vyberte pokus, který chcete zkopírovat a klikněte na tlačítko příkaz Kopírovat.
4.  Vyberte standardní pracovního prostoru v dialogovém okně místní a klikněte na tlačítko Kopírovat.
    Společně s testu do standardní pracovního prostoru se zkopírují všechny přidružené datové sady, školení model, atd.
6.  Budete muset znovu testu a znovu publikovat webové služby v standardní pracovního prostoru.

### <a name="studio-workspace"></a>Pracovní prostor Studio

**Zobrazí různé faktury různých pracovních prostorů?**

Pracovní prostor náklady jsou samostatně rozdělen pro každou příslušných metr na jedné faktury.

**Jaké konkrétní pro využití prostředků Moje pokusy poběží?**

Služba strojového výukové je víceklientské služby, a skutečné výpočetním zdrojů použitých v back-end lišit optimalizovaná pro výkon a předvídatelnost.

### <a name="guest-access"></a>Přístup Guest

**Co je hosta k Studio výukové počítače Azure?**

Přístup Guest je omezený zkušební setkat i v případě, který umožňuje vytváření a spouštění pokusů ve počítače výukové studiu Azure zdarma a bez ověření. Relace typu Host jsou dočasnou (není možné uložit) a 8 hodin. Další omezení zahrnout chybějící R a Python podpory, chybějící pracovních rozhraní API a kapacita velikost a úložiště s omezeným přístupem datovou sadu. Naproti uživatelů, kteří rozhodnout, přihlaste se pomocí účtu Microsoft mají úplný přístup ke oddělených Free počítače výukové Studio popsané nad který obsahuje trvalý pracovní prostor a další možnosti. Zvolte volného prostředí počítače výukové kliknutím **Začínáme** na [https://studio.azureml.net](https://studio.azureml.net)a výběrem uhodnout přístup a přihlaste se pomocí účtu Microsoft.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
