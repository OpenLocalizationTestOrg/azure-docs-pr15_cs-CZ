<properties
    pageTitle="Zřízení Linux dat pro výzkum virtuálního počítače | Microsoft Azure"
    description="Konfigurace a vytvoření Linux dat pro výzkum virtuálního počítače na Azure technologie pro analýzu a počítače výukové."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Zřízení Linux dat pro výzkum virtuálního počítače

Virtuální počítač Linux dat pro výzkum je Azure virtuální počítač, který je součástí sady předinstalovaná nástroje. Tyto nástroje se běžně používají při provádění analýzy dat a strojních učení. Součásti klíčové software součástí jsou:

- Microsoft R Server Developer Edition
- Anaconda Python rozdělení (verze 2.7 a 3.5), včetně knihoven analýzy Oblíbené dat
- JupyterHub - víceuživatelská server Poznámkový blok Jupyter podporující R, Python, Julie jádra
- Průzkumník Azure úložiště
- Azure rozhraní příkazového řádku (rozhraní příkazového řádku) pro správu Azure prostředků
- PostgresSQL databáze
- Výukové obráběcích
    - [Sada nástrojů počítačové síti (CNTK)](https://github.com/Microsoft/CNTK): důkladné výukové sada nástrojů software z Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): rychlé počítače výukové systému, který podporuje techniky jako je online, hash, allreduce snížení, learning2search, aktivní a interaktivní výuka.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): poskytuje rychlé a přesné zesílen stromu implementaci nástroje.
    - [Rattle](http://rattle.togaware.com/) (R analytické nástroje pro další snadno): nástroj, který zajišťuje začínáte pracovat s technologie pro analýzu dat a automatické učení v R snadno s průzkum dat na základě grafického rozhraní a modelování pomocí automatické generování kódu R.
- Azure SDK Java, Python, node.js skutečné, PHP
- Knihovny v R a Python pro použití v Azure počítače učení a další služby Azure
- Nástroje pro vývoj a editory IME (zatmění Emacs, gedit, vi)

Dělají dat pro výzkum zahrnuje iterace na pořadí úkolů:

1. Hledání, načítání a předzpracování dat
2. Vytváření a testování modely
3. Nasazení modely spotřebu inteligentní aplikace

Vědeckých dat pomocí různých nástrojů k dokončení těchto úkolů. Může být docela časově náročný najít odpovídající verze softwaru a potom stáhnout, kompilaci a nainstalujte tyto verze.

Virtuální počítač Linux dat pro výzkum můžete výrazně usnadnění tento zatížení. Můžete podnítit analýzy projektu. Umožňuje práci na úkolech v různých jazycích, včetně R Python, SQL, Java a C++. Zatmění poskytuje IDE vyvíjet a otestovat kód, který se snadno používá. Azure SDK součástí OM umožňuje vytvářet své aplikace pomocí různých služeb na Linux pro platformu Microsoft cloud. Kromě toho máte přístup ovládacích prvků například skutečné Perl, PHP a node.js, které jsou také jsou předinstalované.

Existuje žádné poplatky software pro tento obrázek OM vědy data. Platíte pouze Azure hardwaru poplatky za použití, které jsou vyhodnoceny v závislosti na velikosti virtuálního počítače, který zřizujete s obrázkem OM. Další informace o poplatky výpočetním se nachází na [OM výpis stránky na webu Azure Marketplace ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Než budete moct vytvářet Linux dat pro výzkum virtuálního počítače, musíte mít takto:

- **Azure předplatné**: jednu získáte v tématu [bezplatnou zkušební verzi Azure získat](https://azure.microsoft.com/free/).
- **Účet Azure úložiště**: A vytvořte si ho, najdete v tématu [Vytvoření účet Azure úložiště](storage-create-storage-account.md#create-a-storage-account). Můžete taky účtu úložiště vytvořením jako součást proces vytváření OM, pokud nechcete používat existující účet.


## <a name="create-your-linux-data-science-virtual-machine"></a>Vytvoření virtuálního počítače vědy Linux dat

Tady je postup pro vytvoření instance aplikace Linux dat pro výzkum virtuálního počítače:

1.  Přejděte na seznam na [Azure portál](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)virtuálního počítače.
2.   Klikněte na **vytvořit** (dole) vyvoláte průvodce. ![konfigurace dat – vědy OM](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Následující části obsahují vstupů pro každého kroku v průvodci (výčtu na pravé straně na předchozím obrázku) slouží k vytvoření Microsoft dat pro výzkum virtuálního počítače. Tady jsou vstupy potřebné pro nastavení každý z těchto kroků:

    na. **Základní informace**:

  - **Název**: název serveru pro výzkum dat vytváříte.
  - **Uživatelské jméno**: první účet přihlašovací ID.
  - **Heslo**: první heslo účtu (není nutné můžete použít veřejný klíč SSH heslo).
  - **Předplatné**: Pokud máte víc předplatných, vyberte jedním dnem počítači, je vytvořili a fakturovat. Musíte mít oprávnění k vytváření zdroje pro toto předplatné.
  - **Pole Skupina zdroje**: můžete vytvořit novou nebo použít existující skupinu.
  - **Umístění**: Vyberte datovém centru, které je nejvhodnější. Je to obvykle datovém centru, které obsahuje většinu dat nebo nejvíce odpovídá vaší fyzické umístění nejrychlejší přístupu k síti.

    b. **Velikost**:

  - Vyberte jeden z typů serveru, které splňují požadavek funkční a omezení nákladů. Vyberte možnost **Zobrazit vše** zobrazte další možnosti OM velikostí.

    c. **Nastavení**:

  - **Typ disku**: Zvolte **Premium** podle potřeby disk plné state (SSD). Jinak zvolte **Standardní**.
  - **Úložiště účtu**: můžete vytvořit nový účet Azure úložiště ve vašem předplatném nebo použít na stejném místě, který byl vybrán existující úrovně na **základní informace o** kroku průvodce.
  - **Ostatní parametry**: ve většině případů, jednoduše použít výchozí hodnoty. Vzít v úvahu jiné než výchozí hodnoty, najeďte myší na informační odkaz na informace o konkrétních polí.

    d. **Shrnutí**:

  - Zkontrolujte, že všechny informace, které jste zadali správné.

    e. **Koupit**:

  - Zřizování zahájíte klikněte na **koupit**. Odkaz je dostupný s podmínkami poskytování transakce. OM nemá žádné další poplatky za výpočetním velikost serveru, který jste vybrali v kroku **velikost** .

Zřizování brát asi 10 – 20 minut. Stav zřizování se zobrazí na portálu Azure.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Jak získat přístup ke Linux dat pro výzkum virtuálního počítače

Po vytvoření OM se můžete přihlásit k němu pomocí SSH. Použijte přihlašovací údaje účtu, které jste vytvořili v části **základní informace o** kroku 3 pro rozhraní prostředí text. Ve Windows můžete si stáhnout nástroj klienta SSH jako [nátěrové](http://www.putty.org). Pokud preferujete grafického plochy (systém Windows X), můžete použít X11 přesměrování na nátěrové nebo instalace klienta X2Go.

>[AZURE.NOTE] Klient X2Go proveden výrazně vyšší než X11 přesměrování při testování. Doporučujeme používat X2Go klienta pro grafického rozhraní plochy.


## <a name="installing-and-configuring-x2go-client"></a>Instalace a konfigurace X2Go klienta

OM Linux již zřizování serverem X2Go a je připraven k připojení klientů přijmout. Připojení k grafické plochy Linux OM, postupujte na váš klient:

1. Stáhněte a nainstalujte klienta X2Go pro svoji platformu klienta z [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Spusťte X2Go klienta a vyberte **Novou relaci**. Otevře okno Konfigurace s více listech. Zadejte následujících parametrů konfigurace:
    * **Karta relace**:
        - **Host (hostitel)**: název hostitele nebo IP adresu vaší OM vědy dat Linux.
        - **Přihlášení**: uživatelské jméno na OM Linux.
        - **SSH portu**: ponechte 22, výchozí hodnota.
        - **Relace typu**: Změňte hodnotu na XFCE. OM Linux v současné době podporuje pouze XFCE plochy.
    * **Karta médií**: můžete vypnout zvukovou podporu a tisku při nemusíte používat klienta.
    * **Sdílené složky**: Pokud chcete adresáře z vaší klientské počítače připojeny Linux OM, přidejte adresářů klientského počítače, které chcete sdílet s OM na této kartě.

Po přihlášení angličtině pomocí SSH klienta nebo XFCE grafické plochy přes klienta X2Go, jste připraveni začít používat nástroje, které jsou nainstalovali a nakonfigurovali na OM. Na XFCE zobrazí se zástupců v nabídce aplikace a ikony na ploše pro řadu nástrojů.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Nástroje nainstalovaný na Linux dat pro výzkum virtuálního počítače

### <a name="microsoft-r-open"></a>Otevřít Microsoft R
R patří mezi Nejoblíbenější jazyky pro analýzu dat a výukové počítače. Pokud chcete použít pro svůj analýzy R, má OM Microsoft R otevřete (MRO) s knihovny matematické jádra (MKL). MKL optimalizuje matematických operací běžné v analytical algoritmů. MRO je kompatibilní s CRAN-R ze 100 % a některé z knihovny R publikovaných v galerii CRAN možné nainstalovat na MRO. Můžete upravit aplikací R v jednom z výchozí editory, jako je vi, Emacs nebo gedit. Taky můžete stáhnout a použít jiné IDEs, například [RStudio](http://www.rstudio.com). Jednoduchý skript (installRStudio.sh) je pohodlný, k dispozici v adresáři **/dsvm/tools** , který se instaluje RStudio. Pokud používáte editoru Emacs, práce se soubory R v editoru Emacs poznámku Emacs balíček ESS (Speaks Statistika Emacs), což zjednodušuje byl předinstalovaný.

Spuštění R, stačí zadat **R** v prostředí. Tím přejdete do interaktivní prostředí. Se dají aplikace R, obvykle v editoru jako Emacs nebo vi nebo gedit a potom spustit skripty v rámci R. Pokud nainstalujete RStudio, máte úplné grafické prostředí integrovaném vývojovém prostředí se dají aplikace R.

Je také R skriptu pro instalaci [balíčků horní 20 R](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) , pokud chcete. Tento skript lze spustit, poté, co jste v rozhraní interaktivní R, které mohou být zadány (jak je uvedeno) tak, že zadáte **R** prostředí.  

### <a name="python"></a>Python
Pro vývoj pomocí Python Anaconda Python rozdělení 2.7 a 3.5 nainstalován. Toto rozdělení obsahuje základní Python spolu s asi 300 nejoblíbenější balíčků analýzy matematické, inženýrské a data. Můžete použít výchozí textovém editoru. Kromě toho můžete Spyder integrovaném vývojovém prostředí Python zahrnutý v systému Anaconda Python rozdělení. Spyder potřebuje grafické stolním počítači nebo X11 přesměrování. Zástupce Spyder je součástí grafického plochy.

Vzhledem k tomu nemáme Python 2.7 a 3.5, budete muset specificky aktivace požadované Python, kterou chcete pracovat v aktuální relaci. Aktivační proces nastaví proměnné PATH požadovanou verzi Python.

Pokud chcete aktivovat Python 2.7, spusťte následující z prostředí:

    source /anaconda/bin/activate root

Python 2.7 nainstalovaný na */anaconda/bin*.

Aktivace Python 3.5, spusťte následující z prostředí:

    source /anaconda/bin/activate py35


Python 3.5 je nainstalovaná na */anaconda/envs/py35/bin*.

Vyvolat Python interaktivní relace, stačí zadejte **python** prostředí. Pokud jsou grafického rozhraní nebo mají X11 přesměrování set up, můžete napsat **spyder** spustit integrovaném vývojovém Python prostředí.

### <a name="jupyter-notebook"></a>Jupyter poznámkového bloku

Rozdělení Anaconda taky získáváte Jupyter poznámkového bloku prostředí pro sdílení kód a analýzy. Poznámkový blok Jupyter prostřednictvím JupyterHub. Přihlášení pomocí místní Linux uživatelské jméno a heslo.

Server Poznámkový blok Jupyter předem nakonfigurované s Python 2, Python 3 a R jádra. Existuje ikony na ploše s názvem "Jupyter Poznámkový blok" spuštění prohlížeče pro přístup k serveru Poznámkový blok. Pokud používáte OM přes klienta SSH nebo X2Go, získáte také [https://localhost:8000 /](https://localhost:8000/) pro přístup k serveru Jupyter Poznámkový blok.

>[AZURE.NOTE] Dál, pokud se vám zobrazí upozornění certifikát.

Dostanete se k serveru Jupyter Poznámkový blok z libovolného hostitele. Stačí zadat *https://\<OM DNS nebo IP adresu\>: 8000 /*

>[AZURE.NOTE] Port 8000 je otevřen v bráně firewall ve výchozím nastavení při zřizování OM.

Sbalení ukázkové poznámkových bloků – jeden v Python a jeden v R. Zobrazí se na odkaz ukázky na domovské stránce poznámkového bloku po ověření do poznámkového bloku Jupyter pomocí místní Linux uživatelské jméno a heslo. Nový poznámkový blok můžete vytvořit výběrem **Nový**a potom příslušnou jazykovou jádra. Pokud nevidíte tlačítko **Nová** , klepněte na ikonu **Jupyter** nahoře zleva přejděte na domovskou stránku serveru Poznámkový blok.


### <a name="ides-and-editors"></a>IDEs a editory IME

Máte na výběr několik editory kód. Platí to i vi/VIM Emacs, gEdit a zatmění. gEdit zatmění jsou grafické editory a muset být přihlášení k grafické plochy jejich použití. Tyto editory mít plochy a aplikací zástupců v nabídce spustit je.

**VIM** a **Emacs** jsou textové editory. Na Emacs jsme jste nainstalovali balíček doplněk s názvem Emacs Speaks statistiky (ESS), který usnadňuje práci s R v editoru Emacs. Další informace najdete v [ESS](http://ess.r-project.org/).

**Zatmění** je otevřít zdroj extensible integrovaném vývojovém prostředí, který podporuje více jazyků. Vývojáři edition Java je instance nainstalovaných OM. Moduly plug-in pro nejsou k dispozici několik Oblíbené jazyky, které je možné nainstalovat rozšiřte zatmění prostředí. Máme také modulu plug-in nainstalovaných v zatmění s názvem **Azure sada nástrojů pro Eclipse**. Umožňuje vytvořit, vývoj, otestujte a nasazovat Azure aplikace pomocí zatmění vývojové prostředí, které podporuje jazyky, jako je Java. Je také **Azure SDK jazyka Java** , která umožňuje přístup k různé služby Azure z prostředí Java. Další informace o Azure sada nástrojů pro Eclipse najdete v [Azure sada nástrojů pro Eclipse](../azure-toolkit-for-eclipse.md).

**Latexů** je instalována pomocí balíček texlive spolu s balíčku Emacs doplněk [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) , což zjednodušuje vytváření dokumentů latexů v rámci Emacs.  

### <a name="databases"></a>Databáze

#### <a name="postgres"></a>Postgres
Otevřít zdroj databáze **Postgres** je dostupná na OM, služby a initdb již provedli. Potřebujete k vytvoření databáze a uživatele. Další informace najdete v [dokumentaci Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Grafické klienta SQL
**SQuirrel SQL**, grafický klienta SQL získala připojit k jiné databáze (jako je Microsoft SQL Server Postgres a MySQL) a spouštění dotazů SQL. Lze spustit z relaci grafické plochy (například pomocí klienta X2Go). Vyvolat SQuirrel SQL, můžete ji spustit pomocí ikony na ploše nebo spusťte tento příkaz na prostředí.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Před prvním použití nastavení ovladače a aliasy databáze. Ovladače JDBC jsou umístěny na:

*/USR/share/Java/jdbcdrivers*

Další informace najdete v tématu [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Nástroje příkazového řádku pro přístup k Microsoft SQL Server

Balení ovladač ODBC pro systém SQL Server také dodané s dvou nástrojů příkazového řádku:

**BCP**: hromadné nástroj bcp slouží ke kopírování dat mezi instanci aplikace Microsoft SQL Server a datového souboru ve formátu zadané uživatelem. Nástroj bcp mohou sloužit k importu velkého množství nových řádků do tabulky SQL serveru nebo exportovat data z tabulek do datových souborů. Import dat do tabulky, musíte použít formát soubor vytvořený pro tuto tabulku nebo pochopit strukturu tabulky a typy dat, které jsou platné pro její sloupce.

Další informace najdete v tématu [připojení s bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**SqlCmd**: můžete zadat příkazy jazyce Transact-SQL s sqlcmd nástroje, jakož i postupy systému a soubory na příkazovém řádku skriptů. Tento nástroj používá ODBC provést listy jazyce Transact-SQL.

Další informace najdete v tématu [připojení s sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Existují některé rozdíly v tento nástroj platformách Linux a Windows. Najdete v dokumentaci podrobnosti.


#### <a name="database-access-libraries"></a>Databáze aplikace access knihoven

Nejsou k dispozici v R a Python do databáze aplikace access knihoven.

- V R **RODBC** nebo balíček **dplyr** umožňuje dotazu nebo spuštění příkazů SQL na databázovém serveru.
- V Python knihovna **pyodbc** poskytuje přístup k databázi s ODBC jako základní vrstvy.  

Přístup k **Postgres**:

- Z R: Použil funkci balíček **RPostgreSQL**.
- Z Python: Použijte knihovnu **psycopg2** .


### <a name="azure-tools"></a>Azure nástroje
Následující Azure nástroje nainstalovaných v OM:

- **Azure rozhraní příkazového řádku**: rozhraní příkazového řádku Azure umožňuje vytvářet a spravovat Azure zdrojů pomocí příkazů prostředí. Vyvolat Azure nástroje, stačí zadejte **azure nápovědy**. Další informace najdete v tématu [stránky si přečtěte následující dokumentaci Azure rozhraní příkazového řádku](../virtual-machines-command-line-tools.md).
- **Microsoft Azure úložiště Explorer**: Microsoft Azure úložiště Explorer je grafický nástroj, který slouží k procházení objektů, které jste uložili ve vašem účtu Azure úložiště a k nahrávání a stahování dat do a z objektů BLOB Azure. Máte přístup k Průzkumníka úložišť z ikony zástupce z plochy. Můžete ho vyvolat z prostředí řádku zadáním **StorageExplorer**. Musíte být přihlášení z klienta X2Go nebo máte X11 přesměrování set up.
- **Azure knihoven**: Následují některé předinstalovanou knihoven.

 - **Python**: souvisejících s Azure knihovny v Python, které jsou instalovány jsou **azure**, **azureml**, **pydocumentdb**a **pyodbc**. Pomocí prvních tří knihoven mají přístup k služby Azure úložiště, výukové počítače Azure a Azure DocumentDB (NoSQL databáze na Azure). Knihovnu čtvrtou pyodbc (spolu s Microsoft ovladač ODBC SQL serveru), umožňují přístup k SQL serveru, databázi SQL Azure a Azure SQL datový sklad Python pomocí nějakého rozhraní ODBC. Zadejte **seznam pip** zobrazíte všechny uvedené knihovny. Ujistěte se, spusťte tento příkaz v Python 2.7 i 3,5 prostředí.
 - **R**: souvisejících s Azure knihovny R, které jsou instalovány se **AzureML** **RODBC**.
 - **Java**: seznam knihoven Azure Java najdete v adresáři **/dsvm/sdk/AzureSDKJava** na OM. Klíčové knihovny jsou Azure ukládání a správa rozhraní API, DocumentDB a JDBC ovladačů pro systém SQL Server.  

Dostanete se k [portálu Azure](https://portal.azure.com) z předinstalovaná prohlížeče Firefox. Na portálu Azure můžete vytvořit, Správa a sledování Azure zdroje.

### <a name="azure-machine-learning"></a>Výukové Azure počítače

Azure učení počítače je plně spravovaných cloudové služby, který umožňuje sestavit, nasazení a sdílet prediktivní analýzy řešení. Vytvořením pokusy a modely z Azure počítače výukové Studio. Ho můžete k nim získat přístup z webového prohlížeče v počítači dat pro výzkum virtuální navštivte [Výukové počítače Microsoft Azure](https://studio.azureml.net).

Po přihlášení k Azure počítače výukové Studio máte přístup k hodnocení plátno kde můžete vytvořit logický tok na počítači výukové algoritmů. Také mají přístup do poznámkového bloku Jupyter hostitelem Azure počítače výukové a můžete Bezproblémová práce s pokusů ve počítače výukové studiu. Umožňují počítače výukové modely, které jste vytvořili podle obtékání v rozhraní webové služby. Díky klientům napsané v jiném jazyce vyvolat předpovědí z počítače výukové modely. Další informace najdete v [počítači výukové si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/machine-learning/).

Můžete také vytvořit modely v R nebo Python na OM a ho nasadit v výrobní na výukové počítače Azure. Knihovny jsme jste nainstalovali v R (**AzureML**) a Python (**azureml**) pro povolení této funkce.

Informace o tom, jak nasadit modely v R a Python na výukové Azure počítače najdete v tématu [deset věcí, které můžete provádět na jiné vědecké dat virtuálního počítače](machine-learning-data-science-vm-do-ten-things.md) (zejména v části "vytvářet modely pomocí R nebo Python a umožňují je pomocí Azure výukové počítače").

>[AZURE.NOTE] Tyto pokyny psané pro verzi systému Windows pro výzkum OM Data. Ale poskytují údaje o nasazení modely Azure počítače výukové není k dispozici OM Linux.

### <a name="machine-learning-tools"></a>Výukové obráběcích

OM získáváte několik počítače výukových nástrojů a algoritmech předem kompilovaný a jsou předinstalované místně. Jedná se o:

* **CNTK** (Výpočetním sítě nástrojů z Microsoft Research): důkladné výukové sady nástrojů.
* **Vowpal Wabbit**: rychlé online školení algoritmus.
* **xgboost**: nástroj, který obsahuje optimalizované, zesílen stromu algoritmů.
* **Python**: Anaconda Python je součástí algoritmů výukové počítače s knihovnami jako Scikit další sady. Instalace jiné knihovny pomocí `pip install` příkaz.
* **R**: bohaté knihovna funkcí výukové počítače je k dispozici pro R. Některé z knihoven, které jsou předinstalované jsou lm glm, randomForest, rpart. Spuštěním možné nainstalovat jiné knihovny:

        install.packages(<lib name>)

Tady jsou některé další informace o první tři počítače výukové nástroje v seznamu.

#### <a name="cntk"></a>CNTK
Toto je otevřít zdroj, hloubkové výukové sady nástrojů. Je nástroj příkazového řádku (cntk) a je už na cestě.

Provádět základní vzorek provést následující příkazy v prostředí:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Výstup modelu je v *~/cntkdemo/Output/Models*.

Další informace naleznete v části CNTK [GitHub](https://github.com/Microsoft/CNTK)a [CNTK wikiwebu](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit je počítač výukové systému, který používá techniky jako je online, hash, allreduce snížení, learning2search, aktivní a interaktivní výuka.

Spuštění nástroje na velmi základní příklad, postupujte takto:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Existují další, větší ukázky v adresáři. Další informace o zobrazit najdete v článku [Tato část GitHub](https://github.com/JohnLangford/vowpal_wabbit)a [Vowpal Wabbit wikiwebu](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Toto je do knihovny, která je navržený a optimalizované pro algoritmů zesílen (strom). Cíl tuto knihovnu je posunout limity výpočtu strojů mezními hodnotami potřebné k poskytnutí rozsáhlé stromu zvýšení, scalable přenosný a přesné.

Má formu příkazového řádku, jakož i R knihovny.

Pokud chcete použít tuto knihovnu R, můžete zahájit relaci interaktivní R (stejně tak, že zadáte **R** prostředí) a načíst knihovnu.

Tady je jednoduchý příklad spuštění ve výzvě k zadání R:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Přepínač příkazového řádku xgboost spustíte tady jsou příkazy provést v prostředí:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Soubor .model zapisuje na adresář určený. Informace o tomto příkladu ukázku se nachází [na GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Další informace o xgboost najdete v tématu [xgboost si přečtěte následující dokumentaci stránky](https://xgboost.readthedocs.org/en/latest/)a jeho [Github úložiště](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle ( **R** **A**nalytical **T**rukopis **T**o **L**získat **E**asily) používá průzkum dat na základě grafického rozhraní a modelování. Prezentuje statistické grafy a vizuální souhrnů dat, transformace dat, která můžete snadno modelovat, vytvoří závadou a kontrolované modely z dat, nabídne vám výkonu modely graficky, a skóre nové datové sady. Generuje také kód R, replikace operace v uživatelském rozhraní, které lze spustit přímo v R nebo použít jako výchozí bod pro další analýzu.

Spustit Rattle, je třeba mít v grafické přihlašovací relaci plochy. V terminálu zadejte ```R``` zadat R prostředí. Do příkazového řádku R zadejte následující příkazy:

    library(rattle)
    rattle()

Teď grafického rozhraní se objeví s sadu karet. Ukážeme si úvodní v Rattle potřeby použijte datovou sadu počasí vzorku a sestavíte modelu. V některých kroků se zobrazí výzva k automaticky nainstalujete a zaveďte některé požadované balíčky R, které nejsou v systému.

>[AZURE.NOTE] Pokud nemáte přístup k instalaci balíčku v adresáři systému (výchozí), se zobrazí výzva při okna R konzoly nainstalovat balíčků do své osobní knihovny. Pokud se zobrazí tyto výzvy, přijměte *y* .

1. Klikněte na **Spustit**.
2. Dialogové okno se zobrazí, s dotazem, pokud budete chtít použít uvedenou množinu dat příklad počasí. Kliknutím na tlačítko **Ano** načíst příkladu.
3. Klikněte na kartu **modelu** .
4. Klikněte na **spouštět** k vytvoření rozhodovacího stromu.
5. Klikněte na **Kreslení** a zobrazení rozhodovacího stromu.
6. Klikněte na přepínač **struktury** a klikněte na **spouštět** vytvářet strukturu náhodné.
7. Klikněte na kartu **vyhodnotit** .
8. Klikněte na přepínač **rizika** a klikněte na **spouštět** zobrazíte dva pozemků výkonu rizika (kumulativní).
9. Klikněte na kartu **protokolu** zobrazíte generovat R kód předchozí operace.
(Z důvodu chyb v dané verzi Rattle, budete muset vložit *#* znak před *Tento protokol... exportovat* text protokolu.)
10. Kliknutím na tlačítko **Export** souboru R skript s názvem *weather_script. R* do výchozí složky.

Ukončíte Rattle a R. Teď můžete upravit generovaný skript R nebo ji používat jako je spouštět kdykoli opakovat veškerý obsah, který byl vykonat v rámci Rattle uživatelského rozhraní. Zejména pro začátečníky v R jde snadný způsob, jak můžete rychle udělat analýzu a počítače výukové do jednoduchých grafického rozhraní při automatické generování kódu v R, pokud chcete změnit nebo další.


## <a name="next-steps"></a>Další kroky
Tady je, jak můžete dál učení a průzkum:

* Návod [pro výzkum dat na Linux dat pro výzkum virtuální počítač](machine-learning-data-science-linux-dsvm-walkthrough.md) se dozvíte, jak provádět několik běžných úkolů pro výzkum dat s OM vědy dat Linux zřízení tady. 
* Průzkum různé datové nástroje pro výzkum na jiné vědecké dat OM tak, že vyzkoušet nástroje popisované v tomto článku. Můžete taky spustit *dsvm. Další informace* v prostředí v rámci virtuálního počítače pro základní úvod a odkazy na další informace o nástroji nainstalovaných OM.  
* Naučte se vytvářet analytical řešení začátku do konce systematicky pomocí [Týmu dat pro výzkum obrázku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Navštivte [Cortana analýzy Galerie](http://gallery.cortanaanalytics.com) počítače učení s pomocí data analýzy ukázky používající sadu Cortana analýzy.
