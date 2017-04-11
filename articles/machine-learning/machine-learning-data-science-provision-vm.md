<properties 
    pageTitle="Zřízení Microsoft Data pro výzkum virtuálního počítače | Microsoft Azure" 
    description="Konfigurace a vytvoření dat pro výzkum virtuálního počítače na Azure technologie pro analýzu a počítače výukové." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Zřízení Microsoft Data pro výzkum virtuálního počítače

Microsoft dat pro výzkum virtuálního počítače je obrázek Azure virtuálního počítače (OM) předem nainstalovali a nakonfigurovali nástroji několik Oblíbené běžně používané pro technologie pro analýzu dat a výukové počítače. Jsou nástroje:

- Microsoft R Server Developer Edition
- Anaconda Python rozdělení
- Jupyter Poznámkový blok (R, Python jádra)
- Visual Studio komunity Edition
- Power BI desktop
- SQL Server 2016 Developer Edition
- Výukové obráběcích
    - [Sada nástrojů počítačové síti (CNTK)](https://github.com/Microsoft/CNTK): důkladné výukové sada nástrojů software z Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): rychlé počítače výukové systému, který podporuje techniky jako je online, hash, allreduce snížení, learning2search, aktivní a interaktivní výuka.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): poskytuje rychlé a přesné zesílen stromu implementaci nástroje.
    - [Rattle](http://rattle.togaware.com/) (R analytické nástroje pro další snadno): nástroj, který zajišťuje začínáte pracovat s technologie pro analýzu dat a automatické učení v R snadno s průzkum dat na základě grafického rozhraní a modelování pomocí automatické generování kódu R.
    - [mxnet](https://github.com/dmlc/mxnet): hloubkové výukové rámec sice pro efektivity a flexibilitu
- Knihovny v R a Python pro použití v Azure počítače učení a další služby Azure
- Libovolná včetně libovolná flám pro práci s zdrojového kódu úložištích včetně GitHub, Visual Studio týmovou
- Windows porty několik Oblíbené Linux nástrojů příkazového řádku (včetně awk sed, perl, grep, hledání, wget, otočení atd.) získat přístup prostřednictvím příkazový řádek. 


Dělají dat pro výzkum zahrnuje iterace na pořadí úkolů: hledání zavádění a předem zpracování dat, vytváření a testování modely a nasazení modely spotřebu inteligentní aplikace. Vědeckých dat pomocí řady nástrojů k dokončení těchto úkolů. Může být docela časově náročný najít odpovídající verzích softwaru a pak budou moct stáhnout a nainstalujte je. Microsoft dat pro výzkum virtuálního počítače můžete usnadnění tento zatížení zadáním připravené k použití obrázek, který můžete zřízení na Azure všechny několika Oblíbené nástroji předem nainstalovali a nakonfigurovali. 

Virtuální počítač Microsoft dat pro výzkum jump-starts analýzy projektu. Umožňuje práci na úkolech v různých jazycích včetně R Python, SQL a C#. Visual Studio poskytuje IDE vyvíjet a otestovat kód, který se snadno používá. Azure SDK součástí OM umožňuje vytvářet své aplikace pomocí různých služeb na platformě cloudu společnosti Microsoft. 

Existuje žádné poplatky software pro tento obrázek OM vědy data. Můžete jenom zaplatit poplatky za použití Azure které závisí na velikosti virtuální počítač, který zřizujete. Další informace o poplatky výpočetním najdete v části Podrobnosti ocenění na stránce [dat pro výzkum virtuálního počítače](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) . 


## <a name="prerequisites"></a>Zjistit předpoklady pro

Než budete moct vytvářet Microsoft dat pro výzkum virtuálního počítače, musíte mít takto:

- **Azure předplatné**: jednu získáte v tématu [bezplatnou zkušební verzi Azure získat](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Účet Azure úložiště**: A vytvořte si ho, najdete v tématu [Vytvoření účet Azure úložiště](storage-create-storage-account.md#create-a-storage-account). Můžete taky účtu úložiště vytvořením jako součást proces vytváření OM, pokud nechcete používat existující účet.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Vytvoření virtuálního počítače Microsoft Data vědy

Tady je postup pro vytvoření instance aplikace Microsoft dat pro výzkum virtuálního počítače:

1.  Přejděte do virtuálního počítače výpisu na [Azure portálu](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Klikněte na tlačítko **vytvořit** v dolní části posloužit průvodce. ![konfigurace dat – vědy OM](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Průvodce slouží k vytvoření Microsoft dat pro výzkum virtuální počítač vyžaduje **vstupů** pro jednotlivá pole **pět kroků** uvedených na pravé straně na tomto obrázku. Tady jsou vstupy potřebné pro nastavení každý z těchto kroků:
    
    1.   **Základní informace**
        1.   **Název**: název serveru pro výzkum dat vytváříte.
        2.   **Uživatelské jméno**: jméno účtu správce.
        3.   **Heslo**: heslo účtu správce.
        4.   **Předplatné**: Pokud máte víc předplatných, vyberte jedním dnem počítači, je vytvořili a fakturovat.
        5.   **Pole Skupina zdroje**: můžete vytvořit novou nebo použít existující skupinu.
        6.   **Umístění**: Vyberte datovém centru, které je nejvhodnější. Je to obvykle datovém centru, které obsahuje většinu dat nebo nejvíce odpovídá vaší fyzické umístění nejrychlejší přístupu k síti.
         
    2.   **Velikost**: Vyberte jeden z typů serveru, které splňují požadavek funkční a omezení nákladů. Získejte další možnosti velikostí OM tak, že vyberete "Zobrazit vše".
    
    3.   **Nastavení**:
        1.   **Typ disku**: Zvolte Premium podle potřeby jednotce (SSD), jinak zvolte "Standardní".
        2.   **Úložiště účtu**: můžete vytvořit nový účet Azure úložiště ve vašem předplatném nebo použít existující ve stejném *umístění* , které jste vybrali v kroku **Základy** průvodce.
        3.   **Ostatní parametry**: obvykle jenom použít výchozí hodnoty. Najeďte myší na informační odkaz na informace o konkrétních polí v případě, že chcete zvažte použití jiného než výchozího hodnot.

    4.   **Shrnutí**: Ověřte správnost všechny informace, které jste zadali.
    5.   **Koupit**: klikněte na **koupit** zahájíte zřizování. Odkaz je dostupný s podmínkami poskytování transakce. OM nemá žádné další poplatky za výpočetním velikost serveru, který jste vybrali v kroku **velikost** . 


>[AZURE.NOTE] Zřizování brát asi 10 – 20 minut. Stav zřizování se zobrazí na portálu Azure.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Jak získat přístup k Microsoft dat pro výzkum virtuálního počítače

Po vytvoření OM můžete do něho pomocí přihlašovacích údajů správce účet, které jste nakonfigurovali v předchozí části **Základy** Vzdálená plocha. 

Jakmile váš OM vytvořili a zřízení, jste připraveni začít používat nástroje, které jsou nainstalovali a nakonfigurovali v něm. Existují dlaždice nabídky start a ikony na ploše pro řadu nástrojů. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Postup vytvoření silného hesla na serveru Jupyter poznámkového bloku 

Pokud chcete vytvořit vlastní silné heslo pro server Jupyter Poznámkový blok na počítači nainstalovaný, spusťte tento příkaz z příkazového řádku na počítači virtuální vědy Data.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Zvolte silné heslo po zobrazení výzvy.

Zobrazí algoritmus hash hesla ve formátu "sha1:xxxxxx" do výstupu. Zkopírujte tento algoritmus hash hesla a nahraďte stávající algoritmus hash, který je v souboru konfigurace Poznámkový blok c:\: **C:\ProgramData\jupyter\jupyter_notebook_config.py** s název parametru ***c.NotebookApp.password***.

Nahrazení pouze části (xxxxxx) hodnoty existující hash, který je součástí nabídky. Nabídky a ***sha1:*** předpony pro hodnotu parametru obě muset být zachován.

Nakonec budete muset zastavení a nové spuštění Jupyter serveru, který běží v OM jako windows naplánovaný úkol s názvem **Start_IPython_Notebook**. Pokud nové heslo není přijato po restartování tento úkol, zkuste usmrcujících všechny spuštěné procesy python pomocí Správce úloh a restartujte naplánovaný úkol. Můžete také zkuste restartování virtuální počítač.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Nástroje nainstalovaný na Microsoft dat pro výzkum virtuálního počítače

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Pokud chcete použít pro svůj analýzy R, OM má nainstalovanou edici karta Vývojář v Microsoft R serveru. Microsoft R Server je platformy analýzy obecně nasadit podnikových podle R, který podporuje scalable a zabezpečené. Podpora řadu statistik velký dat, prediktivní modelování a automatické učení funkcí, R Server podporuje široká řada analýzy – průzkum, analýzy, vizualizace a modelování. Pomocí a rozšíření otevřít zdroj R, serveru Microsoft R je plně kompatibilní se službou skripty R, funkce a balíčků CRAN k analýze dat ve velkém měřítku enterprise. Také adresy omezení v paměti při otevřít zdroj R přidáním paralelní a blokového zpracování data. Umožňují dělat technologie pro analýzu dat mnohem větší než co vejde do hlavního paměti.  Visual Studio komunity Edition zahrnuté v OM obsahuje R nástroje pro rozšíření Visual Studia, který obsahuje celou integrovaném vývojovém prostředí pro práci s R. Taky můžete stáhnout a použít jiné IDEs také například [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Pro vývoj pomocí Python Anaconda Python rozdělení 2.7 a 3.5 nainstalován. Toto rozdělení obsahuje základní Python spolu s asi 300 nejoblíbenější balíčků analýzy matematické inženýrské a data. Můžete Python nástroje pro Visual Studio (PTVS), které máte nainstalované aplikace Visual Studio 2015 komunity edition nebo jednu IDEs kombinovaný s Anaconda jako je nečinný nebo Spyder. Chcete-li spustit jednu z těchto hledáním na panelu hledání (**Win** + **S** klíče).

>[AZURE.NOTE] Tak, aby ukazovaly nástroje Python for Visual Studio Anaconda Python 2.7 a 3.5, je potřeba vytvořit vlastní prostředí pro každou verzí. Nastavit stezky prostředí v Visual Studio Edition 2015 komunity, přejděte na **Nástroje** -> **Python nástroje** -> **Python prostředí** a potom klikněte na **+ vlastní**. 

V části C:\Anaconda se instaluje anaconda Python 2.7 a Anaconda Python 3.5 je nainstalovaná v části c:\Anaconda\envs\py35. Podrobný postup najdete v článku [si přečtěte následující dokumentaci PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) . 

### <a name="jupyter-notebook"></a>Jupyter poznámkového bloku
Rozdělení anaconda taky získáváte Jupyter poznámkového bloku prostředí pro sdílení kód a analýzy. Server Poznámkový blok Jupyter předem nakonfigurované s Python 2, Python 3 a R jádra. Existuje ikony na ploše s názvem "Jupyter Poznámkový blok, který spuštění prohlížeče pro přístup k serveru Poznámkový blok. Pokud používáte OM prostřednictvím vzdálené plochy, můžete také navštívit [https://localhost:9999 /](https://localhost:9999/) pro přístup k serveru Poznámkový blok Jupyter při přihlášení v angličtině.
 
>[AZURE.NOTE] Dál, pokud se vám zobrazí upozornění certifikát. 

Sbalení výběru poznámkových bloků v Python a R. Poznámkové bloky Jupyter ukazují, jak pracovat s Microsoft R Server, SQL Server 2016 R Services (v databázi analýzy), Python a dalších Azure technologií po přihlášení k Jupyter. Zobrazí se na odkaz ukázky na domovské stránce poznámkového bloku po ověření do poznámkového bloku Jupyter pomocí hesla, kterou jste vytvořili v předchozím kroku. 

### <a name="visual-studio-2015-community-edition"></a>Edice Visual Studio 2015 komunity
Na OM nainstalovanou edici Visual Studio komunity. Je bezplatné verze Oblíbené integrovaném vývojovém prostředí společnosti Microsoft, který můžete použít k vyzkoušení a malým týmům. Můžete se podívat licenčních podmínek [tady](https://www.visualstudio.com/support/legal/mt171547).  Otevřete aplikaci Visual Studio poklepáním na ikony na ploše nebo do nabídky **Start** . Můžete taky vyhledávat programy s **Win** + **S** a zadání "Visual Studio". Jednou tam můžete vytvořit projekty v jazyce jako C# Python, R, node.js. Moduly plug-in jsou nainstalované taky, díky kterým ji užitečné pro práci s Azure služby, jako třeba katalog dat Azure Azure HDInsight (Hadoop, jiskrou) a jezera dat Azure. 

>[AZURE.NOTE] Může se zobrazit zpráva oznamující, že vypršení platnosti vašeho zkušebního období. Zadejte svoje přihlašovací údaje účtu Microsoft nebo vytvořte nový účet zdarma získat přístup k Visual Studio Edition komunity. 

### <a name="sql-server-2016-developer-edition"></a>Karta Vývojář v SQL serveru 2016 edition
Karta Vývojář v verzi SQL serveru 2016 se službami R spuštění analýzy v databázi je k dispozici v OM. R služby poskytují platformu pro vývoj a zavádění inteligentní aplikace. Jazyk R bohaté a výkonné a mnoho balíčků od komunity slouží k vytváření modelů a generovat předpovědí pro vaše data SQL serveru. Technologie pro analýzu poblíž dat můžete zachovat, protože R služby (v databázi) integrovat R jazyka SQL serveru. Díky náklady a zabezpečení rizik spojených s přesun dat.

>[AZURE.NOTE] Vývojář edition SQL serveru 2016 lze použít pouze pro účely test vývoje a. Je třeba licence ke spuštění ve výrobním. 

SQL server můžete přistupovat spuštěním **SQL Server Management Studio**. Vaše jméno OM vyplněné jako název serveru. Pomocí ověřování Windows při přihlášení jako správce ve Windows. Jakmile se dostanete SQL Server Management Studio můžete vytvořit jiní uživatelé, vytvoření databáze, import dat a spouštění dotazů SQL. 

Povolit v databázi analýzy pomocí Microsoft R, spusťte tento příkaz jako a načasovat akce v SQL Server management studio po přihlášení jako správce serveru. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Několik Azure nástrojů nainstalovaných v OM:

- Je zástupce na ploše pro přístup k dokumentaci Azure SDK. 
- **AzCopy**: slouží k přesouvání dat a odhlášení z účtu Microsoft Azure úložiště. Chcete-li zobrazit použití, zadejte **Azcopy** příkazového řádku zobrazíte využití. 
- **Microsoft Azure úložiště Explorer**: slouží k procházení objektů, které jste uložili v rámci svého účtu úložiště Azure a systémy přepravy data mezi libovolnými Azure úložiště. Můžete zadat **Průzkumníka úložišť** hledání nebo ho najít v nabídce Start systému Windows pro přístup k tento nástroj. 
- **Adlcopy**: slouží k přesunutí dat do jezera dat Azure. Použití zobrazíte do příkazového řádku zadejte **adlcopy** . 
- **dtui**: slouží k přesunutí dat do a z Azure DocumentDB NoSQL databáze v cloudu. Na příkazovém řádku zadejte **dtui** . 
- **Brána pro správu dat Microsoft**: umožňuje přesun dat mezi místním zdrojům dat a cloudu. Pracovní postup slouží v rámci nástroje jako Azure Data Factory. 
- **Microsoft Azure Powershell**: nástroj sloužící ke správě Azure zdrojů v prostředí Powershell skriptovacího jazyka i nainstalovali vaší OM. 

###<a name="power-bi"></a>Power BI

Můžete vytvořit řídicí panely a skvělé vizualizace, **Power BI Desktop** nainstalován. Tento nástroj získat data z různých zdrojů, vytváření řídicích panelů a sestavy a publikovat do cloudu. Informace najdete v tématu webu [Power BI](http://powerbi.microsoft.com) . Power BI desktop můžete najít v nabídce Start. 

>[AZURE.NOTE] Musíte mít účet Office 365 pro přístup k Power BI. 

## <a name="additional-microsoft-development-tools"></a>Další vývojářské nástroje společnosti Microsoft
[**Instalace Microsoft webové platformy**](https://www.microsoft.com/web/downloads/platform.aspx) mohou sloužit k zjišťovat a stáhnout další Microsoft vývojového nástroje. Je také zástupce nástroj umístěna na ploše Microsoft dat pro výzkum virtuálního počítače.  

## <a name="important-directories-on-the-vm"></a>Důležité adresářů v OM

| Položky                          | Adresář |
| ------------------------------| ---------------- |
|Konfigurace serveru Jupyter poznámkového bloku | C:\ProgramData\jupyter |
|Poznámkový blok Jupyter vzorky adresáři| c:\dsvm\notebooks|
|Další příklady | c:\dsvm\samples|
| Anaconda (výchozí: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 prostředí | c:\Anaconda\envs\py35|
|Samostatný serveru R instance adresáře (instance výchozí R) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| Adresář instance serveru R v databázi | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Různé nástroje | c:\dsvm\tools|

>[AZURE.NOTE] Instance aplikace Microsoft dat pro výzkum virtuální počítač před 1.5.0 (před 3 září 2016) použít strukturu mírně odlišnou adresářů než popsaný v předchozí tabulce. 

## <a name="next-steps"></a>Další kroky
Tady jsou některé další kroky platit učení a průzkum. 

* Průzkum různé datové nástroje pro výzkum na jiné vědecké dat OM kliknutím nabídky start a vrácení uvedené v nabídce Nástroje.
* Přejděte na **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** ukázky knihovnu RevoScaleR v R, který podporuje analýzy dat ve velkém měřítku organizace.  
* Přečtěte si článek: [10 věci, které můžete provádět na jiné vědecké dat virtuálního počítače](http://aka.ms/dsvmtenthings)
* Naučte se vytvářet konce analytical řešení systematicky pomocí [Týmu dat pro výzkum obrázku](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Navštivte [Galerie Intelligence Cortana](http://gallery.cortanaintelligence.com) pro počítač učení s pomocí data analýzy ukázky používající sadu Intelligence Cortana. Také uvádíme ikona v nabídce **Start** a na ploše počítače virtuální do této galerie.

