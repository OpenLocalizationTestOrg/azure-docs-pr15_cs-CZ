<properties 
    pageTitle="Azure WebJobs si přečtěte následující dokumentaci zdroje" 
    description="Doporučené učební zdroje pro používání Azure WebJobs a Azure WebJobs SDK." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs si přečtěte následující dokumentaci zdroje

## <a name="overview"></a>Základní informace

Toto téma obsahuje odkazy na prostředky si přečtěte následující dokumentaci o tom, jak používat Azure WebJobs a Azure WebJobs SDK. Azure WebJobs poskytují snadný způsob, jak spustit skripty a programy jako procesy na pozadí v kontextu [aplikaci služby web app, rozhraní API aplikace nebo mobilní aplikaci](../app-service/app-service-value-prop-what-is.md). Nahrávání a spustit spustitelný soubor, jako je třeba jako cmd bat, exe (.NET) ps1, sh, php, kopírovat, js a sklenice. Tyto aplikace spustili WebJobs při plánování (cron) nebo nepřetržitě.

Má [WebJobs SDK](websites-webjobs-resources.md) účel zjednodušit kód, který napíšete pro běžné úkoly, můžete provést, například zpracování obrázků, fronty zpracování WebJob RSS agregace, soubor údržby a odesílání e-mailů. WebJobs SDK má integrované funkce pro práci s Azure úložiště a služby Bus pro plánování úkolů a zpracování chyb a pro další obvyklé scénáře. Kromě toho je navržen tak, aby byl extensible a je [otevřete úložiště zdrojů pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Funkce Azure](../azure-functions/functions-overview.md) (momentálně v náhledu) je založena na verzi SDK WebJobs, se kterými spolupracuje C# skript Node.js a jiných jazyků. 

Vytváření, nasazení a správu WebJobs je bezproblémové pomocí integrovaného nástrojů ve Visual Studiu. Můžete vytvářet WebJobs ze šablon, publikování a správa (spustit a zastavit/monitor/ladění) je. 

Řídicí panel WebJobs na portálu Azure poskytuje možnosti výkonné správy poskytující úplnou kontrolu nad spuštění WebJobs, včetně možnost vyvolat jednotlivých funkcích v rámci WebJobs. Řídicí panel taky zobrazí runtimes (funkce) a výstup protokolování. 

##<a name="getstarted"></a>Začínáme s WebJobs a WebJobs SDK

* [Úvod k Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs jsou otřesné a měli byste začít používat právě teď?](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Příspěvek na blogu od Tróje Hunt.)
* [Funkce Azure WebJobs](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Co je WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Pokyny pro úlohy pozadí společností Microsoft Vzory a postupy](/documentation/articles/best-practices-background-jobs/)
* [Oznámení 1.1.0 RTM SDK WebJobs Microsoft Azure](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Začínáme s Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Použití úložiště Azure fronty s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Jak pomocí WebJobs SDK úložišti objektů blob Azure](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Použití úložiště tabulek Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Jak Bus služby Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Jak používat WebHooks s WebJobs SDK s příklady GitHub, IFTTT a HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK shrnutí (ke stažení PDF)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [Nastavení WebJobs si přečtěte následující dokumentaci v GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videa
    * [WebJobs a WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure řada WebJobs videí v kanálu 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Úvodní informace o WebJobs nástroje for Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure aktualizace WebJobs s Pranav Rastogi - rozšiřitelnost verze 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Viz také v následujících částech o [Nasazení WebJobs](#deploy) a [testování a ladění WebJobs](#debug).

##<a name="deploy"></a>Nasazení WebJobs

* [Jak nasazení WebJobs Azure pomocí aplikace Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Nasazení WebJobs pomocí portálu Azure](web-sites-create-web-jobs.md)
* [Povolení příkazového řádku nebo nepřetržitý Odprezentování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Libovolná nasazení aplikace konzoly .NET Azure pomocí WebJobs](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Nasazení WebJob F # Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Nasazení vlastní služby jako Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videa
    * [Úvodní informace o WebJobs nástroje for Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Plánování WebJobs

* [Azure WebJob dialogové okno Přidat](websites-dotnet-deploy-webjobs.md#configure)
* [Vytvoření plánované WebJob v portálu Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Zachycení Plánovač úlohy WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Plánování Azure WebJobs pomocí výrazů cron](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Plánování jednotlivých funkcích WebJob pomocí WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testování a ladění WebJobs

* [Karta Vývojář v nové a ladění funkce Azure WebJobs ve Visual Studiu](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Zobrazení řídicího panelu WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Jak psát protokoly pomocí WebJobs SDK a tyto informace zobrazit v řídicím panelu](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Vzdálené ladění WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Kdo napsali, které objektů blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hostingu interaktivní kód v cloudu](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Přidání sledování k Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Sledování a Diagnostika a odstraňování potíží úložišti tabulek Microsoft Azure](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videa
    * [WebJobs nástrojů a vzdálené ladění](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Změna měřítka WebJobs

* [Změna měřítka webové aplikace se Azure weby](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure aplikaci služby: Architecting rozsáhlé měřítko připravené pro firmy Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Titulní stránky měřítko webových aplikací web apps s WebJobs, včetně WebJobs SDK.
* Videa
    * [Rozšiřování WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Další zdroje informací WebJobs

* [Azure příspěvek na blog WebJobs GA tak, že Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Spuštění úloh prostředí Powershell Web na Azure aplikaci služby](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Získání potvrzení o vaší Azure spouštěný WebJobs dokončení](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Jednoduchý zásady uchovávání informací zálohování webových aplikací s WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure webové aplikace a služby cloudu pomalé při prvním požadavku](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Ukazuje, jak používat WebJobs tak, aby napodobily AlwaysOn funkci, která je dostupný jenom u standardní ceny osy.
* [Vypnutí WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Vypnutí pro SDK WebJobs, najdete v článku [vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatizace zálohování s Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videa
    * [Azure videa WebJobs tak, že Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure řada WebJobs videí v kanálu 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Další zdroje informací WebJobs SDK

* [Poznámky k verzi WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK zdrojového kódu](https://github.com/Azure/azure-webjobs-sdk)
* [Rozšíření WebJobs SDK zdrojového kódu](https://github.com/Azure/azure-webjobs-sdk-extensions), s [podrobné pokyny k rozšíření modelu](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Zdrojový kód WebJobs SDK skriptu](https://github.com/Azure/azure-webjobs-sdk-script/) (slouží k [Azure funkce](../azure-functions/functions-overview.md))
* [WebJob nahrát soubory FREB k základnímu úložišti Azure pomocí WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Hostitelem Azure webjobs mimo Azure, s výhodami protokolování z Azure hostované webjob](http://bypassion.dk/?p=510)
* [Vytváření nástroj pro Import dat s Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Přehled funkcí Azure](../azure-functions/functions-overview.md)
* Videa
    * [Azure řada WebJobs videí v kanálu 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Ukázkové WebJob aplikace

* [Ukázkové aplikace poskytnutých týmem WebJobs na GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Jednoduchý Azure Web App s využitím WebJobs SDK back-end WebJobs](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Znázorňuje použití plánované a řízeného událostmi WebJobs. Najdete v blogovém příspěvku [opětovné vytvoření SiteMonitR pomocí Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogy

* [Azure blogu](/blog)
* [Apple Amitu blogu](http://blog.amitapple.com/). Se zaměřuje na WebJobs (ne SDK).
* [Magnus Mårtensson blogu](http://magnusmartensson.com/)

##<a name="gethelp"></a>Získání nápovědy pomocí WebJobs

* [StackOverflow WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow pro WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow u Azure funkcí](http://stackoverflow.com/questions/tagged/azure-functions)
* [Fórum komunity Azure a technologie ASP.NET](http://forums.asp.net/1247.aspx)
* [Azure aplikace služby Web Apps – fórum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure webové aplikace uživatele hlasovou webu](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Použití značka #AzureWebJobs.
* [Sestava WebJobs chyb nebo emise](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

