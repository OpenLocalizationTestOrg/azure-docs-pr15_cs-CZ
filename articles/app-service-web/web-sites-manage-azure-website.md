<properties 
    pageTitle="Spravovat web app v aplikaci služby Azure" 
    description="Odkazy na prostředky pro správu web app v aplikaci služby Azure." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Spravovat web app v aplikaci služby Azure

Toto téma obsahuje odkazy na prostředky pro správu web app v [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714). Správa zahrnuje všechny úkoly, které zachovat webovou aplikaci bezproblémový. 

Přes dobu trvání do webových aplikací provedete různými úkoly správy, při přechodu z počáteční nasazení normálnímu operace, Údržba a aktualizace.

Mnoho úlohy správy webu aplikace mohou provést na portálu Azure.

## <a name="before-you-deploy-your-web-app-to-production"></a>Před nasazením webovou aplikaci výrobní

### <a name="choose-a-tier"></a>Zvolte Vrstva

Azure aplikaci služby je k dispozici v pěti úrovní: uvolnit, sdílené, Basic, Standard a Premium. Informace o funkci a ceny pro každou úroveň najdete v tématu [ceny podrobnosti](/pricing/details/app-service/). 

- [Aplikace služby plány](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) umožňují seskupení více webových aplikací web apps ve skupinovém rámečku na stejné úrovni.
- Vždycky můžete [přepínat úrovní](web-sites-scale.md) po vytvoření aplikace pro web.

### <a name="configuration"></a>Konfigurace

Nastavení možnosti konfigurace pomocí [Portálu Azure](https://portal.azure.com/) . Podrobnosti najdete v tématu [Konfigurace web apps v aplikaci služby Azure](web-sites-configure.md). Tady je stručný kontrolní seznam:

- Vyberte **verze runtime** pro .NET, PHP, Java nebo Python, v případě potřeby.
- Povolení **WebSockets** , pokud webovou aplikaci používá protokol WebSocket. (To zahrnuje aplikace, které používají [ASP.NET SignalR](http://www.asp.net/signalr) nebo [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Používáte úlohy nepřetržitý web? Pokud ano, povolte **Vždy v počítači**.
- Nastavte **výchozí dokument**, například index.html.

Kromě tyto základní nastavení můžete nakonfigurovat následující:

- Socket Layer šifrování **SSL (Secure).** Použít protokol SSL s vlastním názvem domény, musíte získat certifikát SSL a nakonfigurovat webovou aplikaci používat. V tématu [Povolení HTTPS pro web app v aplikaci služby Azure](web-sites-configure-ssl-certificate.md).
- **Vlastní název domény.** Web appu automaticky obsahuje subdoménu v části azurewebsites.net. Můžete přiřadit vlastní název domény, například contoso.com. Přečtěte si téma [konfigurace vaší vlastní doménou v aplikaci služby Azure](web-sites-custom-domain-name.md).

Konfigurace specifické pro určitý jazyk:

- **PHP**: [Konfigurace PHP ve webových aplikacích pro službu Azure aplikace](web-sites-php-configure.md).
- **Python**: [Konfigurace Python s Azure služba aplikací Web Apps](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Spuštěná aplikace pro web

Spuštěná aplikace pro web chcete Přesvědčte se, zda je k dispozici, a že upraví podle provozu. Budete taky muset Poradce při potížích.

### <a name="monitoring"></a>Sledování

- Pomocí portálu Azure můžete [Přidat měřítka](web-sites-monitor.md) třeba využití procesoru a počet požadavků klientů.
- [Měřítko webovou aplikaci](web-sites-scale.md) v odpověď přenosy. V závislosti na vaší osy můžete změnit počet VMs a/nebo velikost instancí OM. V úrovní Standard a Premium můžete taky nastavit neobsahovaly text tak webovou aplikaci nastaví automaticky na pevný plán nebo v odpovědi na načíst.  
 
### <a name="backups"></a>Zálohování

- Nastavit [Automatické zálohování](web-sites-backup.md) webovou aplikaci. Další informace o zálohy v [tomto videu](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Zjistěte, jaké možnosti pro [obnovení databáze](../sql-database/sql-database-business-continuity.md) v databázi SQL Azure.

### <a name="troubleshooting"></a>Řešení potíží

- Pokud dojde k chybě, můžete [Poradce při potížích s ve Visual Studiu](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), pomocí diagnostických protokolů a živou ladění v cloudu. 
- Mimo aplikaci Visual Studio způsoby různých shromáždit protokoly pro diagnostiku. V tématu [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure](web-sites-enable-diagnostic-log.md).
- Node.js aplikací najdete v článku [jak ladění Node.js web app v aplikaci služby Azure](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Obnovení dat

- [Obnovení](web-sites-restore.md) do webových aplikací, která byla dříve zálohovat.


## <a name="when-you-update-your-web-app"></a>Pokud aktualizujete webovou aplikaci

Pokud nepovolíte automatické zálohování, můžete vytvořit [Ruční zálohování](web-sites-backup.md).

Zvažte použití [přípravu nasazení](web-sites-staged-publishing.md). Tato možnost umožňuje publikovat aktualizace pracovní nasazení, která se spouští vedle sebe s nasazením výroby. 

Pokud používáte Visual Studio týmovou, můžete nastavit nepřetržitý nasazení z ovládacího prvku zdroje:

- [Používání správy verzí Team Foundation (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Použití libovolná](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
