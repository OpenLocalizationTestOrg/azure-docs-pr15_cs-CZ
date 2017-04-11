<properties 
    pageTitle="Spustit skripty výukové Python počítači | Microsoft Azure" 
    description="Obrysy navrhnout zásadami pro podporu pro Python skripty v Azure počítače učení a scénáře základní použití, funkce a omezení." 
    keywords="počítač Python výuka, pandas, python pandas, skripty python spustit skripty python"
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
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Spustit skripty výukové počítače Python v Azure počítače výukové Studio

Toto téma popisuje zásadami návrh aktuální podporu pro Python skripty Azure počítač přečíst. Hlavní funkce jsou uvedeny také, včetně podpory pro import existujícího kódu, export vizualizace a nakonec jsou uvedeny některé omezení a probíhající práce.

[Python](https://www.python.org/) je nezbytné nástroj v nástroj hrudníku mnoho vědeckých data. Obsahuje:

-  Syntaxe elegantní a stručné 
-  různé platformy podpory 
-  sbírku výkonné knihoven a 
-  zdokonaleným vývojového nástroje. 

Python probíhá v všechny fáze pracovního postupu obvykle používají ve počítače výukové modelování použili, z dat jedí a zpracování funkce konstrukce modelu školení a ověření a nasazení modelů. 

Azure počítače výukové Studio podporuje vkládání Python skripty na různé části počítače výukové testu a také bezproblémové publikování jako scalable, operationalized webové služby na Microsoft Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Principy Python skriptů počítač přečíst
Je třeba Python v Azure počítače výukové Studio primární rozhraní prostřednictvím [Skriptu Python spustit] [ execute-python-script] modulu viz obrázek 1.

![obrazek1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![obrazek2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Obrázek 1. Modul **Spustit skript Python** .

[Spuštění skriptu Python] [ execute-python-script] modul přijme až tři vstupů a vytvoří až dva výstupy (popsané níže), stejně jako jeho R analogový, [Spustit skript R] [ execute-r-script] modul. Python kód, který bude proveden zadaná do pole parametr jako speciálně pojmenovanou vstupní bod funkci `azureml_main`. Tady jsou důležitým zásady slouží k provádění tento modul:

1.  *Musí být idiomatickým Python uživatelům.* Většina uživatelů Python faktor svůj kód jako funkce uvnitř modulů kontroly, tedy relativně méně častých uvedení spoustu spustitelný příkazy v modulu nejvyšší úrovně. Pole skript v důsledku toho trvá speciálně pojmenovanou funkci Python namísto jenom posloupnost příkazů. Objekty zveřejněným v funkci jsou standardní typy knihoven Python například [Pandas](http://pandas.pydata.org/) datových rámců a maticemi [NumPy](http://www.numpy.org/) .
2.  *Musí mít vysokou věrností mezi místních i cloudových spuštění.* Back-end slouží k provedení kódu Python vychází z [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, často používaný různé platformy matematickém Python rozdělení. To je součástí zavřít 200 nejběžnější balíčků Python. Vědeckých dat můžete proto ladění a získání kódu na místním prostředí Anaconda kompatibilní s prohlížečem výukové Azure počítače. Ke spuštění jako součást pokus Azure počítače výukové vysoké spolehlivé použijte existující vývojové prostředí ATP [Python Tools for Visual Studio](http://aka.ms/ptvs) [IPython](http://ipython.org/) Poznámkový blok. Další, `azureml_main` vstupní bod je vanilkové funkce Python a tvořit bez Azure počítače výukové konkrétní kódu nebo nainstalovaných v SDK.
3.  *Musí být Bezproblémová rozšiřitelných s jiných Azure počítače výukové moduly.* [Spuštění skriptu Python] [ execute-python-script] modul přijme jako vstupů a výstupy standardní sady dat Azure počítače výukové. Základní rámec transparentně a efektivně přemosťuje Azure počítače učení s pomocí Python runtimes (podporuje funkce, jako jsou chybějící hodnoty). Python lze tedy ve spojení s existující Azure počítače výukové postupy, včetně těch, které zavolat a připojit R a SQLite. Jednu proto počítat pracovních postupů, které:
  * použití Python a Pandas pro data předzpracování a čištění, 
  * kanálu data do SQL transformace připojení více datové sady funkcí formuláře 
  * školení modelů pomocí rozsáhlé kolekce algoritmů Azure počítač přečíst a 
  * vyhodnocení a po výsledky zpracovat pomocí R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Základní použití scénáře klávesového ovládání počítače výukové Python skriptů
V této části jsme některé základní použití [Spustit skript Python] průzkum[ execute-python-script] modul.
Jak jsme zmínili dříve, nějaké vstupy modulu Python vystavit jako Pandas datových rámců. Další informace o Python Pandas a jak ji můžete použít k s daty pracovat efektivněji a efektivně najdete v *Python pro analýzu dat* (O'Reilly 2012) západní McKinney. Funkce musí vrátit jeden snímek dat Pandas sbalí uvnitř Python [posloupnost](https://docs.python.org/2/c-api/sequence.html) například n-tici, seznam nebo pole NumPy. Vrátí první prvek tuto řadu v první výstupní port modulu se pak. Tento režim je znázorněno na obrázku 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Obrázek 2. Mapování porty parametrů při zadávání a vrátí hodnotu výstupní port.

Podrobnější sémantiku jak získat vstupní porty namapované parametry `azureml_main` funkce se zobrazují v tabulce 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabulka 1. Mapování vstupní portů parametrů funkce.

Mapování mezi vstupní porty a protokoly parametry funkcí je pozice. První připojeného vstupní port namapovaných na první parametr funkce a druhý vstupní (Pokud připojení) je namapované druhý parametr funkce.

## <a name="translation-of-input-and-output-types"></a>Překlad vstupní a výstupní typů
Jak je vysvětleno, zadávání sady dat Azure počítač přečíst se převedou na data, která snímků v Pandas a výstupní data rámce se převedou na sady dat Azure počítače výukové zpět. Následující jsou převáděny:

1.  Řetězec a číselné sloupce se převedou jako-je a chybějící hodnoty v množině hodnot budou převedeny na "NA" v Pandas. Dojde k stejné převodu na cestě zpět (NA hodnoty v Pandas se převedou na chybějící hodnoty Azure počítač přečíst).
2.  Index vektory v Pandas nepodporuje Azure počítač přečíst. Všechny snímky zadávání dat ve funkci Python mít vždycky 64-bit číselného indexu od 0 do počet řádků mínus 1. 
3.  Sady dat Azure výukové počítač nemůže obsahovat duplicitní sloupec názvy a názvy sloupců, které nejsou řetězce. Obsahuje-li rámeček dat výstupu sloupců není číselného typu, volá architektura `str` názvů sloupců. Stejně tak všechny duplicitní názvy sloupců jsou automaticky pozměnění zajistit, že jsou jedinečné názvy. Přípona (2) se přidá do prvního duplikovat (3) do druhého duplicitní, atd.

## <a name="operationalizing-python-scripts"></a>Operationalizing Python skriptů
Všechny [Spustit skript Python] [ execute-python-script] modulů používaných v bodování experimentovat se označují jako při publikování jako webové služby. Obrázek 3 příkladem bodování experiment obsahující kódu pro vyhodnocení výrazu jednoho Python. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Obrázek 3. Webová služba pro vyhodnocení výrazu Python.

Webová služba vytvořená z této experiment používá jako vstup výrazu Python (jako řetězec), odešle video interpreter Python a vrací tabulku obsahující výraz a vyhodnocený výsledek.

## <a name="importing-existing-python-script-modules"></a>Import existující moduly Python skriptu
Běžné případu použití pro mnoho vědeckých dat je začlenit existující skripty Python do Azure počítače výukové pokusy. Místo zřetězení a vložit celý kód do jediného skriptu pole [Spustit skript Python] [ execute-python-script] modul přijme třetí vstupní port ke kterému jde připojit zip soubor, který obsahuje Python moduly. Soubor je pak rozbaleny rámcem spuštění za běhu a obsah se přidají do knihovny cestu video interpreter Python. `azureml_main` Vstupní bod funkce pak můžete importovat moduly přímo.

Jako příklad zvažte soubor Hello.py obsahující jednoduché funkce "Ahoj, světe".

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Obrázek 4. Funkce definované uživatelem.

Dále vytvoříme souboru Hello.zip, která obsahuje Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Obrázek 5. ZIP souboru, který obsahuje uživatelem definovaných Python kód.

Pak nahrajte to jako datovou sadu do výukového Studio Azure počítače. Vytvoření a spuštění jednoduché experiment používající kód Python v souboru Hello.zip připojením ke třetí vstupní port Python skript spustit, jak je vidět na tomto obrázku.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Obrázek 6. Ukázka experimentovat s uživatelem definovaných kód Python odeslat jako soubor zip.

Modul výstup ukazuje, že byl soubor zip nebalených a funkci `print_hello` skutečně spustit.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Obrázek 7. Uživatelsky definovaná funkce se používá uvnitř [Spustit skript Python] [ execute-python-script] modul.

## <a name="working-with-visualizations"></a>Práce s vizualizacemi
Pozemků vytvořené pomocí MatplotLib, který můžete vizualizovat v prohlížeči může být vrácen [Spustit skript Python][execute-python-script]. Ale pozemků nejsou automaticky přesměrována na obrázky, jsou při použití R. Tak uživatel explicitně, uložte všechny pozemků soubory PNG Pokud mají vrátit zpátky na výukové Azure počítače. 

Abyste mohli generovat obrázky z MatplotLib, musíte soutěží následujícím způsobem:

* Přepnutí z výchozí na základě rt vykreslování back-end do "AGG" 
* Vytvoření nového objektu obrázek 
* získání osy a generovat všechny pozemků do něj 
* Uložte obrázek do souboru PNG 

Tento postup je znázorněn v následující obrázek 8, který vytvoří matici bodového grafu pomocí funkce scatter_matrix Pandas.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Obrázek 8. Uložení MatplotLib hodnoty na obrázky.



Obrázek 9 zobrazuje pokus používá skript zobrazené dříve vrátíte vykreslí prostřednictvím druhý port výstupu.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Obrázek 9. Vizualizace pozemků generované kódem Python.

Je možné vrátit více obrázků tak, že je uložíte do různých obrázků, runtime modul Azure počítače výukové vyzvedne všechny obrázky a zřetězí vizualizaci.


## <a name="advanced-examples"></a>Rozšířené příklady
Prostředí Anaconda nainstalovaných v Azure počítače výukové obsahuje běžné balíčků jako NumPy, SciPy a další Scikits a tyto efektivně lze nejrůznější věci zpracování dat v kanálu výukové typické počítače. Jako příklad následující testu a skript ukazuje použití celek žáky v Scikits informace pro výpočet funkce důležitost skóre pro datovou sadu. Skóre pak lze provést výběr sledovaných funkcí před podávání do jiného počítače výukové modelu.

Funkce Python pro výpočet skóre důležitost a pořadí založené na této funkce je uveden níže:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Obrázek 10. Funkce rank funkce podle výsledků.
 Následující testu potom vypočítá a vrátí skóre důležitost funkcí v "Pima indické Diabetes" sady dat Azure počítač přečíst:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Obrázek 11. Vyzkoušejte rank funkcí v sadě dat Pima indické Diabetes.

## <a name="limitations"></a>Omezení 
[Spuštění skriptu Python] [ execute-python-script] aktuálně platí následující omezení:

1.  *V izolovaném prostoru spuštění.* Modul runtime Python je aktuálně v izolovaném prostoru a v důsledku toho neumožňuje přístup k síti nebo na místním pevném disku trvalý způsobem. Jsou všechny soubory uložené místně samostatný a po dokončení modulu odstraněny. Kód Python nemáte přístup ke většina adresáře v počítači, který spustí, ikon aktuální adresář a jeho podadresáře.
2.  *Nedostatečná propracovaných vývoje a ladění podpory.* Modul Python aktuálně nepodporuje funkce integrovaném vývojovém prostředí, například technologie intellisense a ladění. Také pokud modulu nepovede za běhu, úplné zásobníku Python neexistuje, ale musí být zobrazeny v protokolu výstup modulu. Doporučujeme aktuálně vyvíjet a ladění jejich Python skripty v prostředí, například IPython a importujte kód do modulu.
3.  *Výstup jednu datovou rámeček.* Vstupní bod Python je povolena pouze vrátit rámeček jednu datovou jako výstup. Není aktuálně umožňuje se vrátíte libovolného Python objekty, jako jsou přímo školení modely runtime modul Azure počítače učení. Jako [Spustit skript R][execute-r-script], která má stejné omezení, je však možné v mnoha případech okurky objektů do pole bajtů a stiskněte ENTER, které uvnitř rámečku data.
4.  *Nelze upravit Python instalace*. V současné době jediný způsob, jak přidat vlastní moduly Python spočívá ve využití mechanismus souborů zip popsané výše. Když to je vhodné pro malé moduly, je zátěž pro velké moduly (zejména s nativní DLL) nebo velký počet moduly. 


##<a name="conclusions"></a>Závěrů
[Spuštění skriptu Python] [ execute-python-script] modul umožňuje dat vědecký pracovník začlenit existující Python kód do pracovních postupů výukové počítač hostující v cloudu dozvědět Azure počítače a bezproblémově umožňují je jako součást webové služby. Modul skript Python přirozeným spolupracuje s jiných modulů Azure počítač přečíst a se dá použít pro řadu úkolů z průzkum předzpracování k extrakci funkce hodnocení a po zpracování výsledků. Modul runtime back-end pro spuštění vychází z Anaconda, dobře testované a rozšířený Python rozdělení. To zjednodušuje za vás vlaku stávající aktiva kód do cloudu.

Očekáváme poskytují další funkce [Spustit skript Python] [ execute-python-script] modul například možnost školení a umožňují modely v Python a přidání lepší podpora pro vývoj a ladění doručení s kódem v Azure počítače výukové Studio.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Python](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
