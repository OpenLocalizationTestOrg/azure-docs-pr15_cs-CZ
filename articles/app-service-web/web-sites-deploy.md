<properties
    pageTitle="Nasazení aplikace Azure aplikace služby"
    description="Naučte se nasadit aplikaci služby Azure aplikace."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Nasazení aplikace Azure aplikace služby

Tento článek vám pomůže určit nejlepší možností pro nasazení souborů pro web app, mobilní aplikace back-end nebo rozhraní API aplikace služby [Azure aplikace](http://go.microsoft.com/fwlink/?LinkId=529714)a provede vás na příslušné zdroje s pokyny specifické pro požadovanou možnost.

## <a name="overview"></a>Azure Přehled nasazení aplikace služby

Azure aplikace služba udržuje rozhraní za vás (ASP.NET, PHP, Node.js atd.). Některé rámců jsou standardně zatímco jiné, jako jsou Java a Python, může být nutné jednoduché zaškrtnutí konfigurace ji povolit. Kromě toho můžete přizpůsobit aplikační rozhraní, například verze PHP nebo počet bitů vaší runtime. Další informace najdete v tématu [Konfigurace aplikace v aplikaci služby Azure](web-sites-configure.md).

Vzhledem k tomu, že nemusíte bát framework webového serveru nebo aplikace, nasazení aplikace služby aplikace je předmětem nasazení kódu, binární soubory, soubory obsahu a struktury jejich odpovídajících adresáře do [adresáře **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/ / kořenových/App_Data/úlohy webů/** adresáře pro WebJobs). Služba aplikace podporuje následující možnosti nasazení: 

- [FTP nebo FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): použití Oblíbené FTP nebo FTPS povoleno nástroj k přesunu souborů do Azure, z [FileZilla](https://filezilla-project.org) plnohodnotně vybaveného IDEs jako [NetBeans](https://netbeans.org). Toto je sporná, i když proces nahrát soubor. Žádné další služby jsou poskytovaná aplikací, jako je například správu verzí souboru struktury řízení apod. 

- [Kudu (libovolná/Mercurial nebo OneDrive nebo Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): použijte [stroj pro nasazení](https://github.com/projectkudu/kudu/wiki) v aplikaci služby. Použít kód pro Kudu přímo z libovolné úložiště. Kudu také poskytuje přidané služby kdykoli kód se posune, včetně správy verzí, obnovení balíčku, MSBuild a [zavěšení web](https://github.com/projectkudu/kudu/wiki/Web-hooks) pro nepřetržitý nasazení a dalších úlohách automatizaci. Modul nasazení Kudu podporuje 3 různé typy zdrojů nasazení:   
    * Synchronizace obsahu z Onedrivu a Dropboxu   
    * Nepřetržitý nasazení založené na úložiště s automaticky synchronizovat ze GitHub, Bitbucket a Visual Studio týmovou  
    * Nasazení založené na úložiště s ruční synchronizaci z místní libovolná  

- [Nasazení webu](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): nasazení kódu aplikaci služby přímo z oblíbených Microsoft nástroje jako Visual Studio pomocí stejného nástroje, který umožňuje automatizovat nasazení serveru IIS servery. Tento nástroj podporuje pouze změn nasazení, vytvoření databáze, transformace připojovací řetězec, atd. Web nasazení se liší od Kudu v tom, že binární soubory aplikace jsou vytvořené před nasazením Azure. Podobně jako FTP žádné další služby jsou k dispozici prostřednictvím aplikace služby.

Nástroje pro vývoj Oblíbené web podpory jednu nebo víc z těchto procesů nasazení. Během nástroj, který vyberete určuje nasazení procesy, které můžete využít, skutečné DevOps k dispozici závisí na kombinací procesu nasazení a konkrétní nástroje, které zvolíte. Například pokud provedete nasazení webu z [Aplikace Visual Studio s Azure SDK](#vspros), i když nedostáváte automation z Kudu, dostanete obnovení balíčku a MSBuild automatizace ve Visual Studiu. 

>[AZURE.NOTE] Tyto procesy nasazení není ve skutečnosti [zřízení Azure zdroje](../resource-group-template-deploy-portal.md) , který může být nutné aplikace. Však většina propojené návodů ukazují, jak zřídit aplikace a nasazení kódu začátku do konce. Taky můžete najít další možnosti pro vytváření Azure zdrojů v části [automatické nasazení pomocí nástrojů příkazového řádku](#automate) .
     
## <a name="ftp"></a>Nasazení prostřednictvím protokolu FTP ručně zkopírováním soubory Azure
Pokud jste použili k ruční kopírování webového obsahu na webový server, můžete nástroj [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) ke zkopírování souborů, například Průzkumníka Windows nebo [FileZilla](https://filezilla-project.org/).

V oblasti IT ruční kopírování souborů jsou:

- Znalosti a minimální složitost FTP nástrojů. 
- Pokud budete vědět, přesně kde soubory jsou.
- Přidané cenného papíru s FTPS.

Jsou nevýhody ruční kopírování souborů:

- Problémy se dozvědět, jak chcete nasadit soubory do správné adresáře v aplikaci služby. 
- Bez správy verzí pro vrácení zpět po docházet k chybám.
- Historie vestavěné nasazení k řešení potíží nasazení.
- Potenciální dlouhé nasazení čase, protože celou řadu nástrojů FTP není poskytnout jenom změn kopírování a jednoduše zkopírovat všechny soubory.  

### <a name="howtoftp"></a>Nasazení ruční kopírování souborů na Azure
Kopírování souborů na Azure zahrnuje několika jednoduchými kroky:

1. Za předpokladu, že jste již vytvořili nasazení pověření získat informace o připojení FTP tak, že přejdete na **Nastavení** > **Vlastnosti**a potom kopírování hodnoty pro **Uživatele FTP/nasazení**, **Název hostitele FTP**a **FTPS název hostitele**. Zkopírujte hodnotu **FTP/nasazení uživatele** uživatele zobrazená portálem Azure včetně názvu aplikace k tomu správný kontext, serveru FTP.
2. Z vašeho klienta FTP pomocí informace o připojení, které jste shromáždili se připojit k aplikaci.
3. Kopírování souborů a jejich strukturu odpovídajících adresářů [ **/site/wwwroot** adresáře](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo **/ / kořenových/App_Data/úlohy webů/** adresáře pro WebJobs).
4. Přejděte na vaše aplikace URL pro ověření, že aplikace správně funguje. 

Další informace najdete v tématu následující zdroje:

* [Vytvořit web appu PHP MySQL a nasazení pomocí FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Nasazení synchronizace složky cloudu výsledků
Dobrý alternativy [ruční kopírování souborů](#ftp) se synchronizuje souborů a složek aplikace služby z službě cloudového úložiště jako OneDrive nebo Dropbox. Synchronizace se složkou cloudu využívá Kudu procesu nasazení (najdete v článku [Přehled nasazení procesů](#overview)).

V oblasti IT o synchronizaci se složkou cloudu jsou:

- Zjednodušení nasazení. Služby, jako třeba OneDrive a Dropboxu desktopovou synchronizovat klientům, poskytovat tak, aby místního adresáře pracovní taky adresáře nasazení.
- Jedním kliknutím nasazení.
- Všechny funkce v modulu Kudu nasazení je dostupný (například obnovení balíčku, automation).

Jsou nevýhody synchronizaci se složkou cloudu:

- Bez správy verzí pro vrácení zpět po docházet k chybám.
- Bez automatického nasazení je ruční synchronizace.

### <a name="howtodropbox"></a>Nasazení synchronizace složky cloudu výsledků
Na [Portálu Azure](https://portal.azure.com)můžete určit složku pro synchronizace obsahu v cloudového úložiště OneDrive nebo Dropbox, pracovat s kódem aplikace a obsah do této složky a synchronizovat s aplikací služby kliknutím tlačítko.

* [Synchronizace obsahu ve složce cloudové služby Azure aplikace](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Nasazení nepřetržitě od služby cloudové zdroje ovládacího prvku
Pokud váš tým vývoj používá cloudové zdroje kódu management (Správce služeb) služby jako je třeba [Visual Studio týmovou](http://www.visualstudio.com/), [GitHub](https://www.github.com)nebo [BitBucket](https://bitbucket.org/), můžete nakonfigurovat aplikaci služby integrace s úložiště a nasazovat nepřetržitě. 

V oblasti IT nasazení od služby ovládací prvek cloudové zdroje jsou:

- Ovládací prvek verze umožňující vrácení zpět.
- Možnost pro nastavení nepřetržitý nasazení libovolná (a Mercurial, pokud to jde) úložištích. 
- Nasazení větev specifické nástroje můžete nasazovat různých větvích do různých [sloty](web-sites-staged-publishing.md).
- Všechny funkce v modulu Kudu nasazení je dostupný (například Správa verzí nasazení vrácení zpět, obnovení balíčku, automatizaci).

CON nasazení od služby cloudové zdroje ovládacího prvku je:

- Znalost službu odpovídajících Správce služeb povinné.

###<a name="vsts"></a>Nasazení nepřetržitě od služby cloudové zdroje ovládacího prvku
Na [Portálu Azure](https://portal.azure.com)můžete nakonfigurovat souvislé nasazení z GitHub, Bitbucket a Visual Studio týmovou.

* [Nepřetržité nasazení služby Azure aplikace](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Nasazení z místní libovolná
Pokud váš tým vývoj používá místní místní zdroj službu kódu v management (Správce služeb) založené na libovolná, můžete to nakonfigurovat jako zdroj nasazení aplikace služby. 

V oblasti IT nasazení z místní libovolná jsou:

- Ovládací prvek verze umožňující vrácení zpět.
- Nasazení větev specifické nástroje můžete nasazovat různých větvích do různých [sloty](web-sites-staged-publishing.md).
- Všechny funkce v modulu Kudu nasazení je dostupný (například Správa verzí nasazení vrácení zpět, obnovení balíčku, automatizaci).

CON nasazení z místního libovolná je:

- Znalost požadováno příslušných systémem Správce služeb.
- Řešení zapnout klíč pro nepřetržitý nasazení. 

###<a name="vsts"></a>Nasazení z místního libovolná
Na [Portálu Azure](https://portal.azure.com)můžete nakonfigurovat místní nasazení libovolná.

* [Libovolná místního nasazení služby Azure aplikace](app-service-deploy-local-git.md). 
* [Publikování na Web Apps z libovolné libovolná/hg repo](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Nasazení pomocí integrovaném vývojovém prostředí
Pokud už používáte [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) s [Azure SDK](https://azure.microsoft.com/downloads/)nebo jiných integrovaném vývojovém prostředí sad například [Xcode](https://developer.apple.com/xcode/) [zatmění](https://www.eclipse.org)a [IntelliJ PŘEDSTAVU](https://www.jetbrains.com/idea/), můžete nasadit Azure přímo z v rámci vašeho integrovaném vývojovém prostředí. Tato možnost je ideální pro jednotlivé vývojář.

Visual Studio podporuje všechny tři nasazení procesy, (FTP libovolná a nasazení webu), v závislosti na předvolbám, zatímco ostatní IDEs můžete nasadit aplikaci služby pokud mají FTP nebo libovolná integrace (najdete v článku [Přehled nasazení procesů](#overview)).

V oblasti IT nasazení pomocí integrovaném vývojovém prostředí jsou:

- Potenciálně minimalizace nástrojů pro svého životního cyklu aplikace začátku do konce. Můžete vyvíjet ladění, sledování a nasazení aplikace Azure všechny bez přechodu mimo vaší integrovaném vývojovém prostředí. 

Nevýhody nasazení prostřednictvím integrovaném vývojovém prostředí jsou:

- Přidané složitosti nástrojů.
- Pořád vyžaduje systém ovládacího prvku zdroje pro tým projektu.

<a name="vspros"></a>Je další profesionály nasazení pomocí aplikace Visual Studio Azure SDK:

- Azure SDK dává Azure prostředky prvotřídní občanů ve Visual Studiu. Vytvořit, odstranit, upravit, spustit a ukončení aplikace, dotazu back-end databáze SQL, live – ladění Azure aplikace a různých dalších věcech. 
- Živé úpravy souborů kódu v Azure.
- Živé ladění aplikací na Azure.
- Integrované Azure explorer.
- Odlišné-nasazení. 

###<a name="vs"></a>Nasazení přímo z aplikace Visual Studio

* [Začínáme s Azure a ASP.NET](web-sites-dotnet-get-started.md). Postup vytvoření a nasazení jednoduchý ASP.NET MVC web projektu pomocí aplikace Visual Studio a nasazení webu.
* [Jak nasazení WebJobs Azure pomocí aplikace Visual Studio](websites-dotnet-deploy-webjobs.md). Jak nastavit tak, aby nasadit jako WebJobs aplikace konzoly projekty.  
* [Nasazení aplikace zabezpečené technologie ASP.NET MVC 5 s členství, OAuth a SQL databáze do webových aplikací](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Postup vytvoření a nasazení projektu technologie ASP.NET MVC s databázi SQL pomocí aplikace Visual Studio nasazení webu a Entity Framework kód první migrace.
* [Nasazení ASP.NET webu pomocí aplikace Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Části 12 řadě kurzů, které bude pokrývat detailní oblasti úlohy nasazení než ostatní uživatelé v tomto seznamu. Některé funkce Azure nasazení byly přidány od kurzu napsané, ale poznámek přidat později vysvětlují, co tu chybí.
* [Nasazení ASP.NET webu Azure ve Visual Studio 2012 z úložiště libovolná přímo](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Vysvětluje, jak nasaďte projekt ASP.NET webové aplikace Visual Studio pomocí modulu plug-in libovolná potvrďte kód libovolná a spojovací Azure libovolná úložiště. Spuštění ve Visual Studiu 2013 libovolná podpora je předdefinované a nevyžaduje instalaci modulu plug-in.

###<a name="aztk"></a>Nasazení používat sadách Azure zatmění a IntelliJ věci

Společnost Microsoft neposkytuje nasazení Web Apps Azure přímo z zatmění a IntelliJ přes [Azure sada nástrojů pro Eclipse](../azure-toolkit-for-eclipse.md) a [Azure sada nástrojů pro IntelliJ](../azure-toolkit-for-intellij.md). Následující kurzy ilustrují kroky, které jsou součástí nasazení jednoduché "Ahoj" prezentace v prohlížeči Azure pomocí některého z integrovaném vývojovém prostředí:

*  [Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění](./app-service-web-eclipse-create-hello-world-web-app.md). Tento kurz se dozvíte, jak používat Azure sada nástrojů pro Eclipse k vytvoření a nasazení Ahoj světě webové aplikace pro Azure.
*  [Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ](./app-service-web-intellij-create-hello-world-web-app.md). Tento kurz se dozvíte, jak používat Azure sada nástrojů pro IntelliJ k vytvoření a nasazení Ahoj světě webové aplikace pro Azure.


## <a name="automate"></a>Automatizace nasazení pomocí nástrojů příkazového řádku

* [Automatizace nasazení s MSBuild](#msbuild)
* [Kopírování souborů pomocí nástroje FTP a skriptů](#ftp)
* [Automatizace nasazení používat Windows PowerShell](#powershell)
* [Automatizace nasazení pomocí rozhraní API .NET správy](#api)
* [Nasazení z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](#cli)
* [Nasazení z příkazového řádku nasazení webu](#webdeploy)
* [Použití FTP dávku skriptů](http://support.microsoft.com/kb/96269).
 
Další možností nasazení je můžete do cloudové služby, třeba [Chobotnice nasazení](http://en.wikipedia.org/wiki/Octopus_Deploy). Další informace najdete v tématu [Nasazení ASP.NET aplikací k webům Azure](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automatizace nasazení s MSBuild

Pokud používáte [Visual STUDIU](#vs) pro vývoj, můžete k automatizaci všechno, co můžete dělat v integrovaném vývojovém vašeho prostředí [MSBuild](http://msbuildbook.com/) . Můžete nakonfigurovat MSBuild použít [Nasazení webu](#webdeploy) nebo [FTP/FTPS](#ftp) ke zkopírování souborů. Nasazení webu automatizovat také mnoho dalších nasazení úkoly týkající se, například nasazení databází.

Další informace o používání MSBuild příkazového řádku nasazení najdete v následujících zdrojích:

* [Nasazení ASP.NET webu pomocí aplikace Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Desetinu v řadě kurzů o nasazení Azure pomocí aplikace Visual Studio. Ukazuje, jak používat příkazového řádku pro nasazení po nastavení publikování profily ve Visual Studiu.
* [Uvnitř modul Microsoft sestavení: použití MSBuild a Team Foundation Build](http://msbuildbook.com/). Kniha tištěné obsahující kapitol o používání MSBuild nasazení.

###<a name="powershell"></a>Automatizace nasazení používat Windows PowerShell

Funkce nasazení MSBuild nebo FTP můžete provádět z [Prostředí Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Pokud to uděláte, můžete použít taky kolekce rutin prostředí Windows PowerShell, který usnadnění správy Azure REST API pro volání.

Další informace najdete v následujících zdrojích:

* [Nasazení aplikace web propojen GitHub úložiště](app-service-web-arm-from-github-provision.md)
* [Vytvořit web app s databázi SQL](app-service-web-arm-with-sql-database-provision.md)
* [Zřízení a nasazení microservices předvídatelností v Azure](app-service-deploy-complex-application-predictably.md)
* [Stavební reálném světě cloudu aplikace s Azure - automatizovat všechno](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-knihy kapitoly, která vysvětluje, jak aplikace zobrazena v e knize využívá skripty Windows Powershellu vytvářet Azure testovacím prostředí a nasadit na něj. Odkazy na další dokumentaci Azure Powershellu v části [zdroje](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) .
* [Pomocí skriptů Windows Powershellu publikovat pro vývojáře a Test prostředí](../vs-azure-tools-publishing-using-powershell-scripts.md). Jak používat Windows PowerShell, který skripty nasazení aplikace Visual Studio generuje.

###<a name="api"></a>Automatizace nasazení pomocí rozhraní API .NET správy

Můžete napsat kód C# funkce MSBuild nebo FTP nasazení. Pokud to uděláte, můžete využít Azure správy rozhraní REST API k provádění funkce správy webu.

Další informace najdete v tématu následující zdroje:

* [Automatizace všechno, co s knihovny správy Azure a .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Úvod do správy .NET rozhraní API a odkazy na další si přečtěte následující dokumentaci.

###<a name="cli"></a>Nasazení z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)

Přepínač příkazového řádku v systému Windows, Mac a Linux počítačích umožňuje nasazení pomocí FTP. Pokud to uděláte, můžete taky zpřístupníte správy Azure REST API pomocí rozhraní příkazového řádku Azure.

Další informace najdete v tématu následující zdroje:

* [Nástroje azure příkazového řádku](/downloads/#cmd-line-tools). Stránka portálu v Azure.com pro informace o nástroji příkazového řádku.

###<a name="webdeploy"></a>Nasazení z příkazového řádku nasazení webu

[Nasazení webu](http://www.iis.net/downloads/microsoft/web-deploy) je software společnosti Microsoft pro nasazení služby IIS, které nejen poskytuje funkce Inteligentní soubor synchronizace, ale taky můžete provádět nebo koordinaci mnoho dalších nasazení související úkoly, které nelze automatické při použití FTP. Například nasazení webu nástroje můžete nasazovat novou databázi nebo aktualizace databáze spolu s webovou aplikaci. Web nasazení můžete minimalizovat také dobu potřebnou k aktualizaci stávajícího webu vzhledem k tomu můžete inteligentně zkopírovat pouze změněné soubory. Microsoft Visual Studio a Team Foundation Server je podpora nasazení předdefinované webu, ale můžete také nasazení webu přímo z příkazového řádku pro automatizaci nasazení. Web nasazení příkazy jsou velmi výkonné ale křivky výukové může být prudký.

Další informace najdete v tématu následující zdroje:

* [Jednoduchý Web Apps: nasazení](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog tak, že David Ebbo o nástroj, který napsal usnadnit použít nasazení webu.
* [Nástroj pro nasazení web](http://technet.microsoft.com/library/dd568996). Úřední si přečtěte následující dokumentaci na webu Microsoft TechNet. Datum, ale pořád vhodným místem pro zahájení.
* [Pomocí webu nasazení](http://www.iis.net/learn/publish/using-web-deploy). Úřední si přečtěte následující dokumentaci na webu Microsoft IIS.NET. Také datovaná ale vhodným místem pro zahájení.
* [Nasazení ASP.NET webu pomocí aplikace Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild je modul Tvůrce dotazů používaných Visual Studiu a jej lze také z příkazového řádku pro nasazení webovými aplikacemi Web Apps. Tento kurz je součástí řady, která je hlavně o nasazení aplikace Visual Studio.

##<a name="nextsteps"></a>Další kroky

V některých případech můžete chtít mohli snadno přepínat a zpět pracovní a produkční verzi aplikace. Další informace najdete v tématu [Přípravu nasazení na Web Apps](web-sites-staged-publishing.md).

Zálohování a obnovení plán na místě je důležitou součástí každého nasazení pracovního postupu. Informace o službě aplikace zálohování a obnovení funkce najdete v tématu [Zálohování webových aplikací](web-sites-backup.md).  

Informace o tom, jak pomocí řízení přístupu na základě rolí v Azure spravovat přístup k aplikaci služby nasazení najdete v článku [RBAC a webové aplikace publikování](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
