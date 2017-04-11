<properties
    pageTitle="Co je dat pro výzkum virtuálního počítače? | Microsoft Azure"
    description="Zjistěte uvedené hlavní příklady situací, funkce a jak můžete začít s dat pro výzkum virtuálních počítačích, prostředí a jste připraveni na analýzy sady nástrojů."
    keywords="Nástroje pro výzkum dat, data pro výzkum virtuálního počítače, nástroje pro data science linux dat vědy"
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
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Úvod do cloudové dat pro výzkum virtuálního počítače pro Linux a Windows

Data pro výzkum virtuálního počítače je vlastní obrázek OM v cloudu společnosti Microsoft Azure vytvořené speciálně pro plnění vědy data. Má mnoho oblíbených dat pro výzkum a dalších nástrojů jsou předinstalované a předem nakonfigurované tak, aby podnítit vytváření inteligentní aplikací pro pokročilé technologie pro analýzu. Je k dispozici v systému Windows Server 2012 nebo na základě OpenLogic 7.2 CentOS Linux verze. 

Toto téma popisuje, co můžete dělat s OM vědy dat shrnuje některé klíčové scénáře pro používání OM, rozepisuje klíčové funkce k dispozici ve verzích Windows a Linux a pokyny, jak začít používat je.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Co mám dělat s virtuálního počítače vědy dat?

Počítač virtuální vědy dat cílem je poskytovat profesionály dat na všech úrovních dovedností a rolí s prostředím vědy tření bez data. Tento OM ukládá značné čas strávený by Jestliže jste měli srovnatelná prostředí na vlastní. Místo toho spustí data projektu pro výzkum nově vytvořený OM instance. 

Výzkum OM dat a konfigurují pro práci s obecných použití scénáře. Je možné přizpůsobit prostředí nahoru nebo dolů projektu potřeb. Je možné používat preferovaný jazyk program dat pro výzkum úkoly. Můžete nainstalovat dalších nástrojů a vlastní nastavení systému potřebám přesné.
 
## <a name="key-scenarios"></a>Uvedené hlavní příklady situací
V této části navrhne některé klíčové scénáře, u kterých můžete být nasazené OM vědy Data.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Předkonfigurované analýzy plochy v cloudu

Výzkum OM dat zajišťuje konfiguraci podle směrného plánu pro dat pro výzkum týmům nahrazení místní plochy plochou spravovaných cloudu. Tento směrný plán zajišťuje všechny vědeckých dat v týmu jednotný vzhled, se kterým lze ověřit pokusy a propagace spolupráce. Sníží také náklady snížit zátěž členem a uložením na dobu potřebnou k vyhodnocení, nainstalovat a udržovat různých softwarových balíčků potřeby provádět pokročilé technologie pro analýzu.  

### <a name="data-science-training-and-education"></a>Školení pro výzkum dat a vzdělávací organizace

Organizace učitelů a učitelé, Naučte dat pro výzkum třídy obvykle poskytují obrázku virtuálního počítače studentech zajistit konzistentní vzhled a předvídatelností pracovat vzorky. Výzkum OM dat vytvoří prostředí na vyžádání konzistentní instalační program, který usnadňuje výzvy nekompatibilita a podpora. Případy, kdy je potřeba vytvořit často, zejména u školení kratší těchto prostředí prospěch podstatně.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Na vyžádání pružná výkon u velkých projektů

Data pro výzkum hackathons/soutěží nebo rozsáhlé datové modelování a průzkum vyžadují měřítko, hardwaru kapacita obvykle krátkodobá. Výzkum OM dat může pomoct replikovat prostředí pro výzkum dat rychle na vyžádání se změněnou velikostí povolujících pokusy vyžadující vysokým výkonem výpočetních prostředků proběhnout serverech.

### <a name="short-term-experimentation-and-evaluation"></a>Krátkodobá zkoušení a hodnocení

OM vědy dat se dají použít k vyhodnocení nebo další nástroje, například Microsoft R serveru SQL Server, nástroje Visual Studio, Jupyter tmavě výukové / ML sadách a nové nástroje Oblíbené v rámci komunity s minimálními nastavení plánování řízené úsilí. Protože OM vědy dat se dá nastavit tak rychle, lze použít v jiných krátkodobých scénáře použití například replikace publikované pokusy provádění ukázky, následující kurzy v online relace nebo výukové programy pro konference.


## <a name="whats-included-in-the-data-science-vm"></a>Co je součástí OM vědy dat?

Data pro výzkum virtuální počítač má mnoho oblíbených datové nástroje pro výzkum již nainstalovali a nakonfigurovali. Obsahuje taky nástroje, které usnadňují práci s různými Azure dat a analýzy produkty. Můžete prozkoumat a prediktivní modely vycházejí velkých sad dat pomocí serveru Microsoft R Server nebo SQL serveru 2016. Hostitel dalších nástrojů od komunity otevřít zdroj a od společnosti Microsoft se také zahrnuty i ukázka kód poznámkové bloky. V následující tabulce rozepisuje a porovnává hlavní komponenty zahrnuté v edicích Windows a Linux počítače virtuální vědy Data.


|**Edice systému Windows** | **Linux Edition** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7 3.5 | Anaconda Python 2.7 3.5 |
|Poznámkový blok Jupyter Server (R, Python) | JupyterHub: Poznámkové bloky Jupyter více uživatelů (R, Python, Julie) |
|SQL Server 2016 Developer Edition: Scalable v databázi technologie pro analýzu služby R | Postgres, SQuirreL SQL (nástroj pro správu databáze), ovladačů SQL serveru a přepínač příkazového řádku (bcp, sqlcmd) |
|Visual Studio komunity Edition 2015 (integrovaném vývojovém prostředí) </br> -Azure HDInsight (Hadoop) Data jezera SQL Server Data tools </br> -Nástroje Node.js, Python a R for Visual Studio |IDEs a editory IME </br> :-Zatmění s modul plug-in Azure sada nástrojů </br> -Emacs (plus pár ESS, auctex) gedit |
|Power BI desktop | -- |
|Výukové obráběcích </br> -Integrace se službou Azure počítače výuka </br> -CNTK hloubkové výukové/AI) </br> -Xgboost (Oblíbené ML nástroj v soutěží vědy dat) </br> -Vowpal Wabbit (rychlé online student) </br> -Rattle (vizuální rychlého dat a analýzy nástroj) </br> -Mxnet hloubkové výukové/AI) | Výukové obráběcích </br> -Integrace s výukové Azure počítače </br> -CNTK hloubkové výukové/AI) </br> -Xgboost (Oblíbené ML nástroj v soutěží vědy dat) </br> -Vowpal Wabbit (rychlé online student) </br> -Rattle (vizuální rychlého dat a analýzy nástroj)  |
| SDK přístup k Azure a Cortana sadu Intelligence služby | SDK přístup k Azure a Cortana sadu Intelligence služby |
| Nástroje pro přesun dat a řízení zdrojů Azure a velký Data: Azure úložiště Exploreru rozhraní příkazového řádku, Powershellu, AdlCopy (Azure dat jezera), AzCopy, dtui (pro DocumentDB), Microsoft Brána pro správu dat | Nástroje pro přesun dat a řízení zdrojů Azure a velký Data: Průzkumníka úložišť Azure, rozhraní příkazového řádku |
| Libovolná Visual Studio týmovou modulu plug-in | Libovolná |
| Windows port nejoblíbenější Linux/Unix nástroje příkazového řádku přístupných GitBash/příkazový řádek | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Jak začít s OM vědy Data Windows

- Vytvoření instance OM ve Windows tak, že přejdete na [tuto stránku](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) výběr zelené tlačítko **vytvořit virtuální počítač** .
- Přihlaste se k OM z vzdálené plochy pomocí přihlašovacích údajů, které jste zadali při vytvoření OM.
- Seznamte se s a spuštění nástroje dostupné, klikněte na tlačítko **Start** .


## <a name="get-started-with-the-linux-data-science-vm"></a>Začínáme s OM Linux dat vědy

- Vytvoření instance OM na Linux (založené na OpenLogic CentOS) tak, že přejdete na [tuto stránku](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) výběr na tlačítko **vytvořit virtuální počítač** .
- Přihlaste se k OM z SSH klientů, jako je nátěrové nebo SSH příkazu pomocí přihlašovacích údajů jste zadali, když jste vytvořili OM.
- Na příkazovém řádku prostředí zadejte dsvm. Další informace.
- Grafické plochy, stáhněte si X2Go klienta pro svoji platformu klienta [sem](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) a postupujte podle pokynů v dokumentu Linux dat pro výzkum OM [poskytování Linux dat pro výzkum virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Další kroky

### <a name="for-the-windows-data-science-vm"></a>Pro výzkum Data Windows OM

- Další informace o tom, jak spustit konkrétní nástrojů dostupných na verzi systému Windows, najdete v článku [poskytnutí Microsoft dat pro výzkum virtuálního počítače](machine-learning-data-science-provision-vm.md) a
-  Další informace o tom, jak dělat nejrůznější věci, které jsou potřebné pro data projektu pro výzkum na OM Windows najdete v článku [deset věcí, které můžete provádět na jiné vědecké dat virtuálního počítače](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Pro výzkum Linux dat OM

- Další informace o tom, jak spouštět konkrétní nástroje dostupné ve verzi Linux najdete v článku [poskytnutí Linux dat pro výzkum virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md).
- Informace, které ukazuje, jak provádět několik běžných dat pro výzkum s OM Linux najdete v článku [vědy dat na Linux dat pro výzkum virtuální počítač](machine-learning-data-science-linux-dsvm-walkthrough.md).
