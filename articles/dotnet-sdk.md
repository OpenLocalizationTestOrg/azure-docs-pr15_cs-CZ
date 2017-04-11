<properties
    pageTitle="Co je Azure .NET SDK"
    description="Zjistěte, co je součástí Azure .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Co je Azure SDK pro .NET?

## <a name="overview"></a>Základní informace

Azure SDK pro .NET sady nástrojů Visual Studia, je nástroje příkazového řádku, runtime binární klienta knihoven, které vám pomůžou vývoj, otestovat a nasazení aplikace, které se spouštějí v Azure Tento článek popisuje, co se zobrazí, když nainstalujete SDK. V SDK můžete stáhnout ze [stránky souborů ke stažení Azure](https://azure.microsoft.com/downloads/).

Azure SDK pro .NET také zahrnuje [knihoven klienta služby náročný Azure](http://go.microsoft.com/fwlink/?LinkId=510472). Tyto knihovny jsou nainstalovali samostatně pomocí NuGet.

##<a id="included"></a>Co je součástí Azure SDK pro .NET

Azure SDK pro .NET nainstaluje těchto produktů:

- [Visual Studio komunity Edition 2015](#vwd)
- [Emulátoru úložiště Microsoft Azure](#stgemulator)
- [Nástroje pro úložiště Microsoft Azure](#stgtools)
- [Nástroje pro vytváření Microsoft Azure](#auth)
- [Emulátoru Microsoft Azure](#emulator)
- [HDInsight Tools for Visual Studio a Microsoft podregistru ovladač ODBC](#hdinsight)
- [Knihovny Microsoft Azure pro .NET](#libraries)
- [Mobilní aplikace SDK Microsoft Azure](#mobile)
- [Prostředí PowerShell Microsoft Azure](#ps)
- [Microsoft Azure nástroje pro Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET a nástroje Web for Visual Studio](#wte)
- [Microsoft Azure dat jezera Tools for Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio komunity Edition 2015

V počítači nemáte Visual Studiu, nainstaluje SDK [Visual Studio komunity Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Emulátoru úložiště Microsoft Azure

[Azure úložiště emulátoru](http://msdn.microsoft.com/library/hh403989.aspx) používá instanci systému SQL Server a místním pevném disku, aby napodobily Azure úložiště (fronty, tabulky, objekty BLOB), takže můžete otestovat místně.

###<a id="stgtools"></a>Nástroje pro úložiště Microsoft Azure

Nainstalujete [AzCopy](http://aka.ms/AzCopy), nástroj příkazového řádku používaného pro přenos dat do a od účtu Azure úložiště.

###<a id="auth"></a>Nástroje pro vytváření Microsoft Azure

Tato volba zahrnuje následující:

* [Nástroj CSPack příkazového řádku](http://msdn.microsoft.com/library/gg432988.aspx) pro vytvoření balíčků nasazení.
* [nástroj CSEncrypt příkazového řádku](http://msdn.microsoft.com/library/hh404001.aspx) pro šifrování hesla, které slouží ke cloudové služby role instance pomocí připojení ke vzdálené ploše.
* Binární runtime soubory, které cloudových služeb projekty vyžadují pro komunikaci s jejich prostředí a diagnostických nástrojů. Tyto binární nejsou k dispozici v NuGet balíčků.

###<a id="emulator"></a>Emulátoru Microsoft Azure

[Emulátoru Azure](http://msdn.microsoft.com/library/dn339018.aspx) napodobuje prostředí cloudové služby, takže můžete otestovat cloudové služby projekty místně na váš počítač před nasazením Azure.

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio a Microsoft podregistru ovladač ODBC

Nástrojích HDInsight v Průzkumníku serveru umožňují navigaci podregistru databází a propojené úložiště účty pro HDInsight clusterů, vytvářet tabulky a vytvořit a odeslat podregistru dotazů. Další informace najdete v tématu [Začínáme s používáním HDInsight Hadoop Tools for Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>Knihovny Microsoft Azure pro .NET

Tato volba zahrnuje následující:

* NuGet balíčky Azure úložiště služby Bus a ukládání do mezipaměti, které jsou uložené v počítači, aby Visual Studio můžete vytvořit nový cloudové služby projekty při offline.
* Sady Visual Studio modul plug-in, který umožňuje projekty [v roli mezipaměti](http://msdn.microsoft.com/library/dn386103.aspx) místně ve Visual Studiu.

###<a id="mobile"></a>Mobilní aplikace SDK Microsoft Azure

Nástroje pro práci s [Aplikací Mobile aplikace služby Azure](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>Prostředí PowerShell Microsoft Azure

Azure Powershellu umožňuje [automatizovat vytváření Azure prostředí a nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Microsoft Azure nástroje pro Microsoft Visual Studio

Umožňuje pracovat s Azure zdrojů, především cloudovými službami a virtuálních počítačích:

* [Vytvoření, otevření a publikovat cloudové služby projekty](cloud-services/cloud-services-dotnet-get-started.md).
* [Vytvoření nasazení balíčků cloudové služby projektů](http://msdn.microsoft.com/library/ff683672.aspx).
* [Vytváření virtuálních počítačích Azure při vytváření nových projektů web](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Vytvoření prostředí PowerShell skripty při vytváření nové virtuálních počítačích](http://msdn.microsoft.com/library/dn642480.aspx).
* [Zobrazit a spravovat nastavení služby cloudu v systému windows vlastnosti projektu Visual Studio](http://msdn.microsoft.com/library/ee405486.aspx).
* Zobrazení a správa [cloudové služby](http://msdn.microsoft.com/library/ff683675.aspx), [virtuálních počítačích](http://msdn.microsoft.com/library/jj131259.aspx)a [Služby Bus](http://msdn.microsoft.com/library/jj149828.aspx) v Průzkumníku serveru.
* [Spustit v režimu ladění vzdáleně pro cloudovými službami a virtuálních počítačích](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatizace zdroje zřizujete pomocí Azure zdroje skupinových nasazení projektech](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Nástroje služby aplikace Microsoft Visual Studio

Umožňuje pracovat s weby Azure:

* [Publikování webové projekty na Azure weby](app-service-web/web-sites-dotnet-get-started.md).
* [Publikování projektu aplikace konzoly pro Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Vytvoření webu Azure a databáze SQL zdroje při vytváření nového projektu nebo při publikování web projektu](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Nasazení skriptů Powershellu vytvořit při vytváření nové weby](http://msdn.microsoft.com/library/dn642480.aspx).
* [Spravovat a Poradce při potížích s weby Azure v Průzkumníku serveru](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Spustit v režimu ladění vzdáleně pro weby a WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Nemusíte instalace SDK Azure pro .NET použití těchto funkcí. jsou taky součástí aplikace Visual Studio aktualizace.

##<a id="datalake"></a>Microsoft Azure dat jezera Tools for Visual Studio

Další informace najdete v tématu [kurz: vývoj U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Co je ale nezahrnutý při instalaci Azure SDK pro .NET

Existuje několik věcí, které může být vhodné pro Azure vývoj, které nejsou součástí při instalaci sady SDK. Nejdůležitější z nich jsou následující:

* [Knihoven klienta](http://go.microsoft.com/fwlink/?LinkId=510472).

    Azure SDK zahrnuje knihoven klienta pro využívající služby Azure, ale nikoli všech nainstalovaných při instalaci SDK. Pokud aplikace potřebuje klienta knihovny, která SDK nenainstaluje, můžete ji získat od [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Pokud aplikace používá knihovnu klienta, kterého SDK nainstalovat, je vhodné aktualizovat aktuální verzi na NuGet.org.

    **Místní kopie knihoven klienta.** Azure SDK pro .NET slouží ke kopírování s vaším počítačem balíčků NuGet pro některé knihoven Azure klienta, například úložiště služby Bus a ukládání do mezipaměti. Těchto knihoven klienta automaticky součástí aplikace jsou nových projektů cloudové služby, takže místní balíčků NuGet povolit Visual Studio k vytváření projektů, i když nejste připojení k Internetu. Knihoven klienta obecně aktualizovány častěji, než jsou vydána nová verze SDK, takže knihoven klienta na NuGet.org, jsou často více než dosáhnout pomocí SDK aktuální.

    **Šablony projektů, které obsahují knihoven klienta.** Jen šablony projektů [Azure Cloudová služba](cloud-services/cloud-services-dotnet-get-started.md) a služba Azure aplikací [Mobilní aplikace](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) automaticky vkládat některé knihoven klienta. U jiných knihoven nebo jiných šablon nainstalujte [klienta knihovny NuGet balíčků](http://go.microsoft.com/fwlink/?LinkId=510472) , které potřebujete.

* [Mobilní aplikace project šablony](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Šablony mobilní aplikace jsou dostupné pouze ve Visual Studiu 2013 aktualizace 2 a novější. Nejsou k dispozici v Visual Studio 2012 a starších verzích a nikoli ve Visual Studiu 2013 aktualizace 1 nebo starší, i když nainstalujete Azure SDK pro .NET.

##<a id="faq"></a>Nejčastější dotazy

- [Mnoho Azure funkce už jsou ve Visual Studiu. Je potřeba nainstalovat Azure SDK pro .NET?](#azinvs)
- [Chci knihovně klienta. Je potřeba nainstalovat SDK Azure pro .NET ho získat?](#clientlib)
- [Kde najdu starší verze aplikace Azure SDK pro .NET](#olderversions)
- [Jaké jsou zásady životního cyklu verzích SDK Azure pro .NET?](#lifecycle)
- [Které verze operačního systému hosta je kompatibilní se službou Azure SDK pro .NET?](#guestos)
- [Jak odinstalovat Azure SDK pro .NET](#uninstall)

###<a id="azinvs"></a>Mnoho Azure funkce už jsou ve Visual Studiu. Je potřeba nainstalovat Azure SDK pro .NET?

Je vhodné nainstalujete SDK, pokud chcete vyvíjet Azure pomocí nástroje nejnovější. Pokud raději není nainstalujete SDK, stačí při splnění následujících podmínek:

* Jste nainstalovali nejnovější [Aktualizace Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5).
* Vyvíjíte jenom pro weby Azure nebo mobilní služby, ne pro Cloudovým službám nebo virtuálních počítačích.
* Aplikace nepoužívá úložiště, nebo využití úložiště, ale nemusíte emulátoru úložiště nebo nástroje AzCopy.

###<a id="clientlib"></a>Chci knihovně klienta. Je potřeba nainstalovat SDK Azure pro .NET ho získat?

V SDK nainstaluje knihoven klienta jenom abyste mohli vytvářet cloudové služby projekty i v případě, že nebudete připojení k Internetu. Aktuální knihoven klienta jsou dostupné v NuGet balíky u [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Další informace najdete v tématu [není zahrnutých při instalaci SDK Azure pro .NET](#notincluded) dříve v tomto dokumentu.

###<a id="olderversions"></a>Kde najdu starší verze aplikace Azure SDK pro .NET

Pro starší verze najdete v článku stažení [Azure SDK pro .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) stránky.

###<a id="lifecycle"></a>Jaké jsou zásady životního cyklu verzích SDK Azure pro .NET?

V tématu [zásady životního cyklu podpory Microsoft Azure Cloudovým službám](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Které verze operačního systému hosta je kompatibilní se službou Azure SDK pro .NET?

V tématu [Azure Host verzích OS a SDK kompatibility matice](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Jak odinstalovat Azure SDK pro .NET

Každá položka uvedené v tomto článku v části [Co je součástí SDK Azure pro .NET](#included) je samostatného programu v seznamu instalovaných programů v Ovládacích panelech Windows **programy a funkce**.  Nejde žádným způsobem odinstalovat jako skupina; budete muset odinstalovat jednotlivé aplikace jednotlivě.

Pokud máte v Azure SDK pro .NET už nainstalovaná a nainstalovat novou verzi, je obecně není potřeba Odinstalujte starou. Ve většině případů instalace SDK aktualizuje existující program spíše než přidáte novou a ponechání starý.

Ale pokud chcete odebrat bez delší potřeby nezdařené dřívější verze, odinstalujte jenom aplikace, které zadejte číslo starší verze a odinstalovat je pouze, pokud existuje stejné program novou verzí. Například po aktualizaci z 2,5 2.6 můžete vidět 2,5 a 2.6 verzích "Microsoft Azure nástroje pro Microsoft Visual Studio 2013", a pak odinstalujete 2,5 verze. Ale může zobrazit pouze 2,5 verzi "Nástroje systému Microsoft Azure vytváření" a by pak nebyl bezpečných odinstalovat.

Všimněte si, že verze čísla v programu názvy v **programy a funkce** mohou být zavádějící.  Například verze 2.6 obsahuje "Microsoft Azure mobilní aplikace SDK verze 1.0" Pokud instalace SDK for Visual Studio 2013 a "Microsoft Azure mobilní aplikace SDK verze 2.0" pro Visual Studio 2015; verze v tomto případě není verze SDK ale indikátor z nich platí pro aplikace Visual Studio verze aplikace.

Tento článek neuvádí každý program, který každý starší verzi Azure SDK zahrnuté; existuje jiných aplikacích, které můžete odinstalovat ze starších verzí SDK, protože starší verze SDK někdy součástí aplikace, které vlastním vynechán z novější verze. Například verze 2,5 nainstaluje "Azure zdroje správce nástroje for Visual Studio", ale není v seznamu v tomto článku proto, že ho už se zobrazí jako samostatný program **programy**a funkce.  Tento článek obsahuje jenom seznam programů, které jsou součástí Azure SDK pro .NET verze 2.6.

> **Poznámka:** Některé programy, která má v SDK může samostatně nainstalovaný také v jiných kontextech a může být potřeba i v případě, abyste nemuseli úplné SDK. Stejné může být pravdivé programů, které byly nainstalovány aplikací SDK starší, ale vlastním vynechán v novějších verzích SDK. Když začnete s odinstalací programy, dbejte na to aby nedošlo k odebrání něco, co je pořád potřeba ve vašem počítači.



##<a id="versions"></a>Verze

V tématu jakou verzi je aktuální nebo stáhnout starší verze najdete na stránce [Azure SDK pro .NET historie verzí](https://azure.microsoft.com/downloads/archive-net-downloads/) .

##<a id="resources"></a>Zdroje informací

Stažení aktuální SDK Azure pro .NET nebo v knihovně klienta, zobrazit [stránky souborů ke stažení Azure](https://azure.microsoft.com/downloads/).

Azure SDK .NET zdrojového kódu, včetně knihoven klienta najdete v článku [GitHub.com/Azure](https://github.com/azure/).

Azure klienta knihovny odkaz si přečtěte následující dokumentaci najdete v článku Principy [Azure .NET](https://azure.microsoft.com/documentation/api/).
