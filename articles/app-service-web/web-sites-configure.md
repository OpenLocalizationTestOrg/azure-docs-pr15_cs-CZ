<properties 
    pageTitle="Konfigurace webové aplikace v aplikaci služby Azure" 
    description="Postup při konfiguraci do webových aplikací v aplikaci služby Azure" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Konfigurace webové aplikace v aplikaci služby Azure #

Toto téma vysvětluje, jak nakonfigurovat web appu pomocí [Portálu Azure].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Nastavení aplikace

1. Na [Portálu Azure]otevřete zásuvné pro web app.
2. Klikněte na **všechna nastavení**.
3. Klikněte na **Nastavení aplikace**.

![Nastavení aplikace][configure01]

**Nastavení aplikace** zásuvné má nastavení seskupené podle několik kategorií.

### <a name="general-settings"></a>Obecná nastavení

**Verze framework**. Pokud aplikace používá některý tyto rámce nastavení těchto možností: 

- **.NET Framework**: nastavte .NET framework verze. 
- **PHP**: nastavení verze PHP nebo **Vypnuto, pokud** chcete zakázat PHP. 
- **Java**: Vyberte verze Java nebo **Vypnuto, pokud** chcete zakázat Java. Pomocí možnosti **Web kontejneru** zvolit mezi Tomcat a molo verzí.
- **Python**: Vyberte Python verze nebo **Vypnuto, pokud** chcete zakázat Python.

Technických důvodů povolení jazyka Java aplikace zakáže .NET PHP a Python možnosti.

<a name="platform"></a>
**Platformu**. Slouží k výběru zda webovou aplikaci běží v prostředí 32bitová verze nebo 64bitová verze. Prostředí 64-bit vyžaduje Basic nebo standardní režimu. Uvolněte a sdílené režimy vždy spustit v prostředí 32bitová verze.

**Web Sockets**. Nastavení **zapnuto** , pokud chcete povolit WebSocket Protocol (protokol); Pokud například webovou aplikaci používá [ASP.NET SignalR] nebo [socket.io].

<a name="alwayson"></a>
**Vždy zapnuto**. Ve výchozím nastavení jsou web apps uvolnění Pokud jsou nedělá některé dobu. Díky systémové prostředky. V režimu Basic nebo Standard můžete povolit **Vždy na** zachovat aplikaci načtení vždy. Pokud získáte úlohy nepřetržitý webové aplikace byste měli **Vždycky na**povolit nebo úlohy web nemusí spustit problémy se spolehlivým.

**Spravovat verze kanálem k odesílání zpráv**. Nastaví služby IIS [režim kanálů]. Nechte hodnotu nastavenou na integrovaný (výchozí) Pokud nemáte starší verze aplikace, které si žádá starší verzi serveru IIS.

**Automatické zaměnit**. Pokud povolíte automatického vyměňovat pro nasazení úsek, aplikaci služby automaticky zaměnit web appu do provozu, když nabízená aktualizaci na tuto pozici. Další informace najdete v tématu [nasadit na pracovní sloty pro web apps v aplikaci služby Azure] (web weby připravené – publishing.md).

### <a name="debugging"></a>Ladění

**Vzdálené ladění**. Umožňuje vzdálené ladění. Když povolíte, můžete ve Visual Studiu vzdálené ladění připojit přímo do webové aplikace. Vzdálené ladění zůstane povolené 48 hodin. 

### <a name="app-settings"></a>Nastavení aplikace

Tato část obsahuje název/dvojice, které je web app načte při spuštění. 

- K aplikacím .NET tato nastavení jsou vložené do konfiguraci .NET `AppSettings` za běhu přepsat existující nastavení. 

- PHP, Python, Java a uzel aplikace přístup k nastavení jako proměnné za běhu. Pro každý nastavení aplikací vytvořené dvě proměnné prostředí; jedna z nich zadaný název položky nastavení aplikace a jiné s předponou APPSETTING_. Obsahují stejné hodnoty.

### <a name="connection-strings"></a>Připojovací řetězec

Připojení řetězce propojených zdrojů. 

Pro .NET aplikace, jsou tyto připojovací řetězec vložené do konfiguraci .NET `connectionStrings` nastavení za běhu přepsání existujícími položkami kde klávesu odpovídá názvu propojené databáze. 

PHP, Python, Java a uzel aplikací budou k dispozici jako proměnné za běhu, předponou typ připojení tato nastavení. Proměnná předpony prostředí jsou následujícím způsobem: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- Databáze SQL:`SQLAZURECONNSTR_`
- Vlastní:`CUSTOMCONNSTR_`

Například, pokud připojovací řetězec MySql byly s názvem `connectionstring1`, byste měli získat přístup prostřednictvím prostředí proměnné `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Výchozí dokumenty

Výchozí dokument je webová stránka, která se zobrazí v kořenové adresy URL pro web.  V seznamu první odpovídající soubor používá. 

Webové aplikace pomocí modulů kontroly, že směrování založené na adresu URL, nikoli podávání statický obsah v takovém případě tam je bez výchozí dokument jako takové.    

### <a name="handler-mappings"></a>Obslužná rutina mapování

Pomocí této oblasti můžete přidat vlastní skript procesorů zpracovávat požadavky pro konkrétní přípony. 

- **Rozšíření**. Přípona souboru ke zpracování *.php ATP handler.fcgi. 
- **Procesor Cesta skriptu**. Absolutní cesta procesoru skriptu. Požadavky na soubory, které odpovídají koncovku souboru budou zpracovány skript procesorem. Použít cestu `D:\home\site\wwwroot` odkázat na vaše aplikace kořenový adresář.
- **Další argumenty**. Volitelné argumenty příkazového řádku pro procesor skriptu 


### <a name="virtual-applications-and-directories"></a>Virtuální aplikací a adresářů 
 
Abyste mohli nakonfigurovat virtuální aplikací a adresářů, zadejte každý virtuální adresář a odpovídající pole fyzicky cestu vzhledem k kořenový Web. Volitelně můžete zaškrtnutím políčka **aplikace** označit virtuální adresář jako aplikace.


## <a name="enabling-diagnostic-logs"></a>Povolení protokoly pro diagnostiku

Chcete-li povolit diagnostických protokolů:

1. V zásuvné pro webovou aplikaci klikněte na **všechna nastavení**.
2. Klikněte na **protokoly pro diagnostiku**. 

Možnosti pro psaní protokoly pro diagnostiku z webové aplikace, která podporuje protokolování: 

- **Protokolování aplikace**. Data zapisuje protokoly aplikace do systému souborů. Protokolování vydrží dobu 12 hodin. 

**Úroveň**. Pokud je povoleno protokolování aplikace, tato možnost určuje, že množství informací poskytovaných bude zaznamenán (chyby, upozornění, informace nebo podrobné).

**Protokolování na webový server**. Ukládání protokolů ve formátu souboru diakritikou protokolu W3C. 

**Podrobné chybové zprávy**. Uloží podrobné chybovými zprávami souborech. Soubory, které se ukládají v části /LogFiles/DetailedErrors. 

**Sledování požadavků se nezdařila**. Protokoly nepovedlo žádosti ke XML souborům. Soubory, které se ukládají v části /LogFiles/W3SVC*xxx*, kde je xxx jedinečný identifikátor. Tato složka obsahuje soubor XSL a jeden nebo víc souborů XML. Zkontrolujte, že stáhnout soubor XSL, protože nabízí funkcí pro formátování a filtrování obsahu souborů XML.

Zobrazení souborů protokolu, musíte vytvoření mapování přihlašovacích FTP, následujícím způsobem:

1. V zásuvné pro webovou aplikaci klikněte na **všechna nastavení**.
2. Klikněte na **Nasazení pověření**.
3. Zadejte uživatelské jméno a heslo.
4. Klikněte na **Uložit**.

![Nasazení nastavit přihlašovací údaje][configure03]

Úplné uživatelské jméno FTP je "app\username" kde *aplikaci* název svojí webové aplikace. Uživatelské jméno je uvedený v aplikaci web zásuvné v části **Essentials**.  

![Přihlašovací údaje FTP nasazení][configure02]

## <a name="other-configuration-tasks"></a>Další úkoly konfigurace

### <a name="ssl"></a>SSL 

V režimu Basic nebo Standard můžete nahrávat certifikáty SSL pro vlastní doménu. Další informace najdete v tématu [HTTPS povolit pro web app]. 

Nahrané certifikáty zobrazíte kliknutím na **Všechna nastavení** > **vlastní domény a SSL**.

### <a name="domain-names"></a>Názvy domén

Přidání jmen vlastní domény pro web app. Další informace najdete v tématu [Konfigurace vlastního názvu domény pro web app v aplikaci služby Azure].

Pokud chcete zobrazit názvy domén, klikněte na **Všechna nastavení** > **vlastní domény a SSL**.

### <a name="deployments"></a>Nasazení

- Nastavte si nepřetržitý nasazení. V tématu [Použití libovolná pro nasazení Web Apps v aplikaci služby Azure](./web-sites-deploy.md).
- Nasazení sloty. V tématu [nasazení se pracovní prostředí pro Web Apps v Azure aplikaci služby].


Nasazení sloty zobrazíte kliknutím na **Všechna nastavení** > **sloty nasazení**.

### <a name="monitoring"></a>Sledování

V režimu Basic nebo Standard můžete otestovat dostupnost HTTP nebo HTTPS koncové body z až tři geo distributed umístění. Sledování test selže, pokud kód odpovědi HTTP je chyba (4xx nebo 5xx) nebo odpověď trvá déle než 30 sekund. Koncový bod se považuje za k dispozici, pokud zkoušku sledování úspěšné ze zadaných umístění. 

Další informace najdete v tématu [jak: sledování stavu koncový bod web].

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby], které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="next-steps"></a>Další kroky

- [Konfigurace vaší vlastní doménou v aplikaci služby Azure]
- [Povolení HTTPS pro aplikaci služby Azure aplikace]
- [Změnit velikost do webových aplikací v aplikaci služby Azure]
- [Sledování základy pro Web Apps v aplikaci služby Azure]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure portálu]: https://portal.azure.com/
[Konfigurace vaší vlastní doménou v aplikaci služby Azure]: ./web-sites-custom-domain-name.md
[Nasazení se pracovní prostředí pro Web Apps v aplikaci služby Azure]: ./web-sites-staged-publishing.md
[Povolení HTTPS pro aplikaci služby Azure aplikace]: ./web-sites-configure-ssl-certificate.md
[Postup: sledování stavu koncový bod web]: http://go.microsoft.com/fwLink/?LinkID=279906
[Sledování základy pro Web Apps v aplikaci služby Azure]: ./web-sites-monitor.md
[režim kanálů]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Změnit velikost do webových aplikací v aplikaci služby Azure]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Vyzkoušejte aplikaci služby]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
