<properties
    pageTitle="Pokročilé konfigurace a rozšíření Azure aplikaci služby web appu"
    description="Pomocí deklarace Transformation(XDT) dokumentů XML a transformace ApplicationHost.config soubor v aplikaci služby Azure web appu přidat soukromé rozšíření povolit správu vlastní akce."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure aplikaci služby v prohlížeči pokročilé konfigurace a přípon souborů

Pomocí deklarace [Transformace dokumentu XML](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) můžete převést soubor [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) ve vaší webové aplikaci služby Azure aplikace. Deklarace XDT taky slouží k přidání soukromé rozšíření povolit správu akce vlastní webová aplikace. Tento článek obsahuje ukázkové PHP webové aplikace rozšíření správce, který umožňuje řízení PHP nastavení prostřednictvím webového rozhraní.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Upřesnit konfiguraci prostřednictvím ApplicationHost.config
Platformu aplikaci služby poskytuje flexibilitu a ovládací prvek pro konfiguraci webové aplikace. I když konfiguračního souboru standardní IIS ApplicationHost.config není k dispozici v aplikaci služby přímý upravovat, platformu podporuje deklarativní model transformace ApplicationHost.config založený na XML dokumentu transformace (XDT).

Můžete využít tuto funkci transformace, vytvoříte soubor ApplicationHost.xdt XDT obsahu a místo v kořenovém webu (d:\home\site) v [Konzole Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console). Možná bude potřeba restartovat Web App pro změny se projeví.

V následujícím příkladu applicationHost.xdt ukazuje, jak přidat novou vlastní proměnnou do webové aplikace, která používá PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Soubor protokolu s transformace stav a informace z kořenového FTP v části LogFiles\Transform neexistuje.

Další ukázky naleznete v tématu [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Poznámka:**<br />
Prvky ze seznamu modulů v části `system.webServer` nemůže být odstraněné nebo se změněným pořadím, ale je možné použít přidání do seznamu.


##<a id="extend"></a>Rozšířit webovou aplikaci

###<a id="overview"></a>Základní informace o rozšíření soukromé webové aplikace

Služba aplikace podporuje rozšíření webové aplikace jako bod rozšiřitelnost pro akci správy. Některé funkce platformy aplikaci služby jsou ve skutečnosti implementovaná jako předinstalovanou rozšíření. Během rozšíření předinstalovaná platformy nelze upravovat, můžete vytvořit a nakonfigurovat soukromé rozšíření webové aplikace pro. Tato funkce také závisí na XDT deklarace. Základní kroky při vytváření rozšíření soukromé webové aplikace jsou následující:

1. Webová aplikace rozšíření **obsahu**: vytvoření libovolné webové aplikace nepodporuje aplikace služby
2. Webová aplikace rozšíření **deklarace**: vytvoření souboru ApplicationHost.xdt
3. Webová aplikace rozšíření **nasazení**: umístěte obsah SiteExtensions složky ve složce`root`

Vnitřní odkazů pro web app odkazovat na cestu vzhledem k cestě aplikace uvedené v souboru ApplicationHost.xdt. Jakákoli změna k souboru ApplicationHost.xdt vyžaduje aplikaci Koš webu.

**Poznámka**: Další informace o těchto klíčové prvky je k dispozici na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Podrobný příklad zahrnuta znázorňující postup pro vytvoření a povolení rozšíření soukromé webové aplikace. Zdrojový kód, který následuje příklad PHP správce můžete stáhnout z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Příklad rozšíření webové aplikace: PHP správce

Správce PHP je rozšíření webové aplikace, které správcům webové aplikace umožňuje snadno zobrazit a jejich nastavení PHP pomocí rozhraní webových takže není nutné PHP ini upravovat přímo. Běžné konfigurace souborů pro PHP vložit soubor php.ini umístěné pod Program Files a. user.ini soubor umístěný v kořenové složce svojí webové aplikace. Vzhledem k tomu php.ini soubor databáze není upravitelný přímo na platformě aplikaci služby, rozšíření PHP správce zabírá. user.ini soubor, který chcete použít změny nastavení.

####<a id="PHPwebapp"></a>Správce PHP webovou aplikaci

Následující obrázek je na domovské stránce Správce PHP nasazení:

![TransformSitePHPUI][TransformSitePHPUI]

Jak vidíte, rozšíření webové aplikace je stejně jako běžný webovou aplikaci, ale s dalších souborem ApplicationHost.xdt umístí do kořenové složce ve web appu (Další informace o souboru ApplicationHost.xdt jsou k dispozici v následující části tohoto článku).

Rozšíření PHP správce byla vytvořená pomocí šablony aplikace Visual Studio ASP.NET MVC 4 webové aplikace. V okně Průzkumník následující zobrazení ukazuje strukturu rozšíření PHP správce.

![TransformSiteSolEx][TransformSiteSolEx]

Pouze zvláštní logiku potřebné pro vstup a výstup je vyznačte, kde je umístěn v adresáři kořenových ve web appu. Následující příklad ukazuje, prostředí proměnné "Domů" označuje cesta ke kořenové web appu a kořenových cesta lze vyrobeno připojením "site\wwwroot":

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Až budete mít adresář, můžete pro čtení a zápis k souborům operací běžných souborů.

Jeden bod opatrně rozšíření webové aplikace jde o zpracování hodnot interní odkazy.  Pokud máte všechny odkazy v souborech ve formátu HTML, které poskytují absolutní cesty interní odkazy na svoji webovou aplikaci, musíte se ujistit, že tyto odkazy se před s názvem vaší rozšíření jako kořenový. To je potřeba, protože je kořenové linky "/`[your-extension-name]`/" místo se pouze "/", takže interní propojení musíte aktualizovat příslušným způsobem. Předpokládejme například, že váš kód obsahuje odkaz na následující:

`"<a href="/Home/Settings">PHP Settings</a>"`

Když na odkaz je součástí rozšíření webové aplikace, odkaz musí být následujícím způsobem:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Tento požadavek můžete vyřešit tak, že buď jenom relativní cesty v rámci vaší webové aplikace nebo v případě aplikace ASP.NET pomocí `@Html.ActionLink` metodu, která vytvoří na příslušné odkazy za vás.

####<a id="XDT"></a>Soubor applicationHost.xdt

Kód pro web app linky přejde v části %HOME%\SiteExtensions\[vaše jméno přípona]. Budete název to kořenové rozšíření.  

Zaregistrujte svůj rozšíření webové aplikace se souborem applicationHost.config, budete muset umístění souboru nazvaného ApplicationHost.xdt v kořenovém rozšíření. Obsah souboru ApplicationHost.xdt by měl být následujícím způsobem:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Název, který jste vybrali jako název svého rozšíření by měl mít stejný název jako kořenovou složku rozšíření.

Výsledek je obrazovky pro přidání nového cestě k aplikaci `system.applicationHost` seznam weby pod webem Správce služeb. Správce služeb web je koncový bod správy webu pomocí přihlašovacích údajů zvláštní přístup. Obsahuje adresa URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Nasazení rozšíření webové aplikace

Instalace rozšíření webové aplikace, můžete FTP zkopírujte všechny soubory webové aplikace `\SiteExtensions\[your-extension-name]` složky ve web appu, podle kterého chcete nainstalovat rozšíření.  Ujistěte se, jak zkopírovat soubor ApplicationHost.xdt do tohoto umístění stejně. Restartujte povolte rozšíření webové aplikace.

Je třeba neuvidíte rozšíření webové aplikace na:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Všimněte si, že adresa URL vypadá stejně jako adresa URL pro váš web app s tím rozdílem, že používá HTTPS a obsahuje ".scm".

Je možné zakázat všechna soukromé (ne předinstalovaná) rozšíření webové aplikace pro při vývoji a vyšetřování přidáním nastavení aplikace s klávesou `WEBSITE_PRIVATE_EXTENSIONS` a hodnotu `0`.

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
