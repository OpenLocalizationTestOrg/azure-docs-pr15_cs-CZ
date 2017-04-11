<properties
    pageTitle="Ladění Node.js web app v aplikaci služby Azure"
    description="Naučte se ladění Node.js web app v aplikaci služby Azure."
    tags="azure-portal"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Ladění Node.js web app v aplikaci služby Azure

Azure umožňuje předdefinovaných diagnostiky kvůli usnadnění ladění Node.js aplikací hostované ve webových aplikacích pro [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714) . V tomto článku se dozvíte, jak povolit protokolování stdout a stderr, zobrazit informace o chybě v prohlížeči a jak stáhnout a zobrazení souborů protokolu.

Diagnostika pro aplikace Node.js hostitelem Azure poskytuje společnost [IISNode]. Když tento článek popisuje nejčastější nastavení pro shromažďování diagnostické informace, nenabízí kompletní odkaz pro práci s IISNode. Další informace o práci s IISNode najdete v tématu [IISNode Readme] na GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Povolení protokolování

Ve výchozím nastavení webové aplikace pro aplikaci služby pouze zaznamenává diagnostické informace o nasazení, třeba když nasadíte web appu pomocí libovolná. Tyto informace jsou užitečné, pokud máte potíže při nasazení, například k neúspěšnému při instalaci modulu odkazuje ve **package.json**, nebo pokud používáte skript vlastního nasazení.

Povolit protokolování stdout a stderr datových proudů, musíte vytvořit soubor **IISNode.yml** v kořenovém adresáři aplikace Node.js a přidejte tyto:

    loggingEnabled: true

Díky protokolování stderr a stdout z aplikace Node.js.

Soubor **IISNode.yml** lze také na ovládací prvek zda popisný chyby nebo vývojář vracejí do prohlížeče když dojde k chybě. Povolit vývojář chyby, přidejte do souboru **IISNode.yml** následující řádek:

    devErrorsEnabled: true

Po povolení této možnosti IISNode vrátí poslední 64 kB informace odesílané do stderr místo popisný chyby, jako je "došlo k chybě interní server".

> [AZURE.NOTE] Během devErrorsEnabled je užitečná, když Diagnostika problémů průběhu vývoje, povolit na provozním prostředí může způsobit chyby vývoj odesílaného koncovým uživatelům.

Pokud soubor **IISNode.yml** v rámci aplikace neexistuje, musíte restartovat webovou aplikaci po publikování aktualizované aplikace. Chcete-li jednoduše změnit nastavení v existující **IISNode.yml** soubor, který předtím publikovaly, restartování není nutné.

> [AZURE.NOTE] Pokud svoji webovou aplikaci byla vytvořená pomocí nástroje příkazového řádku Azure nebo rutiny prostředí PowerShell Azure, vytvoří se automaticky výchozí **IISNode.yml** soubor.

Web appu, vyberte web app na [Portálu Azure](https://portal.azure.com)a klepněte na tlačítko **RESTARTOVAT** :

![Restartujte tlačítko][restart-button]

Pokud máte nainstalované nástroje příkazového řádku Azure ve vývojovém prostředí, můžete tento příkaz restartujte web appu:

    azure site restart [sitename]

> [AZURE.NOTE] Když loggingEnabled a devErrorsEnabled nejsou nejčastěji používané možnosti konfigurace IISNode.yml pro zachycení diagnostické informace, IISNode.yml mohou sloužit k konfigurovat celou řadu možností pro hostitelské prostředí. Úplný seznam možností konfigurace najdete v článku [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) soubor.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Přístup k protokoly

Protokoly pro diagnostiku lze přistupovat třemi způsoby; Pomocí protokolu FTP (File Transfer), stahování Zip archivu nebo jako o živé aktualizována toku protokolu (označovaná taky jako zadní). Stahování archivu Zip souborů protokolu nebo zobrazení živých proudu vyžadují nástroje Azure příkazového řádku. Toto je možné nainstalovat pomocí tento příkaz:

    npm install azure-cli -g

Po instalaci nástroje můžete k nim získat přístup pomocí příkazu "azure". Nástroje příkazového řádku musí být nakonfigurované nejdřív můžete předplatné Azure. Informace o tom, k provedení tohoto úkolu naleznete v tématu **stažení a importovat nastavení publikování** článku [jak pomocí Azure příkazového řádku nástrojů](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Dostupnost diagnostické informace prostřednictvím protokolu FTP, přejděte na [Portál Azure](https://portal.azure.com), vyberte webovou aplikaci a pak vyberte **řídicího PANELU**. V části **odkazy** odkazy **Protokoly pro diagnostiku FTP** a **FTPS diagnostických PROTOKOLŮ** umožňují přístup k protokolům pomocí protokolu FTP.

> [AZURE.NOTE] Pokud jste to ještě nakonfigurovali dříve uživatelské jméno a heslo pro FTP nebo nasazení, můžete provedete na stránce Správa **Rychlý úvod** tak, že vyberete **nastavit přihlašovací údaje nasazení**.

Adresa URL FTP vrácený v řídicím panelu je adresáři **LogFiles** , který bude obsahovat následující podadresáře:

* [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení například Libovolná v adresáři se stejným názvem se vytvoří a bude obsahovat informace týkající se nasazení.

* nodejs - Stdout a stderr informací zachycených ze všech instancí aplikace (Pokud loggingEnabled je PRAVDA.)

###<a name="zip-archive"></a>ZIP archivu

Ke stažení Zip archiv diagnostických protokolů, použijte následující příkaz nástrojích Azure příkazového řádku:

    azure site log download [sitename]

To stáhne **diagnostics.zip** aktuálního adresáře. Tento archiv obsahuje následující strukturu adresářů:

* nasazení - protokol informací o nasazení aplikace

* LogFiles

    * [Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení například Libovolná v adresáři se stejným názvem se vytvoří a bude obsahovat informace týkající se nasazení.

    * nodejs - Stdout a stderr informací zachycených ze všech instancí aplikace (Pokud loggingEnabled je PRAVDA.)

###<a name="live-stream-tail"></a>Dynamický toku (zadní)

Zobrazíte dynamický toku diagnostických informací protokolu zadejte následující příkaz nástrojích Azure příkazového řádku:

    azure site log tail [sitename]

Vrátí toku protokolu událostí, které mají být aktualizovány při jejich vzniku na serveru. Tomto proudu vrátí informace o nasazení, jakož i stdout a stderr informace (Pokud loggingEnabled je PRAVDA.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Další kroky

V tomto článku se naučíte povolit a přístup k diagnostické informace pro Azure. Když tyto informace je užitečné v Principy potíží s aplikací, může vést k potížím s modulu používáte nebo které verzi Node.js používané aplikace služby webovými aplikacemi se liší od toho používali v prostředí pro nasazení.

Informace v tématu Práce s moduly na Azure najdete v článku [Použití Node.js moduly s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).

Informace o určení Node.js verze aplikace najdete v tématu [Určení verze Node.js v aplikaci Azure].

Další informace najdete v tématu taky [Středisko pro vývojáře Node.js](/develop/nodejs/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

[IISNode]: https://github.com/tjanczuk/iisnode
[Soubor Readme pro IISNode]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Určení verze Node.js v aplikaci Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
