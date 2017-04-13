<properties
    pageTitle="Python web app s Django | Microsoft Azure"
    description="Tento kurz se naučíte hostovat na základě Django webu v Azure pomocí Windows serveru 2012 R2 Datacentra virtuálního počítače pomocí klasické nasazení modelu."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Django Vítáme webová aplikace na OM serveru Windows

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac a Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Tento kurz popisuje hostovat web Django založené na Microsoft Azure pomocí virtuálního počítače v systému Windows Server. Tento kurz předpokládá, že máte žádné zkušenosti pomocí Azure. Po dokončení tohoto kurzu, budete mít Django aplikace založené na nahoru a spuštěné v cloudu.

Se dozvíte, jak:

* Nastavte si Azure virtuálního počítače na hostitele Django. Když tento kurz vysvětluje, jak provést v systému Windows Server, stejné může taky provést Linux OM hostované v Azure.
* Vytvořte novou aplikaci Django z Windows.

Provedením tohoto kurzu vytvoříte jednoduché Vítáme webové aplikace. Aplikace se použitý ve Azure virtuálního počítače.

Snímek obrazovky s dokončeným aplikace správce Další.

![Zobrazení stránky světě Ahoj na Azure okně prohlížeče][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Vytváření a konfigurace Azure virtuálního počítače na hostitele Django

1. Postupujte podle pokynů vzhledem k tomu, [tady](virtual-machines-windows-classic-tutorial.md) vytvoření Azure virtuálního počítače rozdělení Windows serveru 2012 R2 Datacentra.

1. Sdělte Azure tak, aby směrovaly port 80 z webu s portem 80 na virtuálního počítače:
 - Přejděte na nově vytvořený virtuálního počítače Azure klasické portálu a klikněte na kartu **koncové body** .
 - Kliknutím na tlačítko **Přidat** v dolní části obrazovky.
    ![Přidání koncový bod](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Otevřete protokolu **TCP** je **veřejné PORT 80** jako **soukromé PORT 80**.
![][port80]
1. Karta **řídicí panel** klikněte na **Připojit** ke **Vzdálené plochy** umožňuje vzdáleně přihlásit nově vytvořený Azure virtuálního počítače.  

**Důležitá poznámka:** Níže uvedených pokynů se předpokládá, že jste přihlášeni správně do virtuálního počítače a k vydání příkazy tam spíše než místního počítače.

## <a id="setup"> </a>Instalaci Python, Django, WFastCGI

**Poznámka:** Chcete-li stáhnout v Internet Exploreru možná bude potřeba nakonfigurovat nastavení IE ESC (Start/správy nástroje/Správce/místního serveru, klikněte na **Rozšířeného zabezpečení aplikace Internet Explorer**, vypnuté).

1. Nainstalujte nejnovější 2.7 Python nebo 3.4 z [python.org][].
1. Nainstalujte wfastcgi a django balíčků pomocí pip.

    Python 2.7 použijte následující příkaz.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Python 3.4 použijte následující příkaz.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Instalace služby IIS s FastCGI

1. Instalace služby IIS s podporou FastCGI.  To může trvat několik minut provést.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Vytvoření nové aplikace Django

1.  Z *C:\inetpub\wwwroot*zadejte tento příkaz Vytvořit nový projekt Django:

    Python 2.7 použijte následující příkaz.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Python 3.4 použijte následující příkaz.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Výsledku příkazu Nový AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  Příkaz **Správce django** vygeneruje základní struktura na základě Django weby:

  -   **helloworld\manage.PY** vám pomůže začít, který je hostitelem a zastavit hostovat svůj web, na základě Django
  -   **helloworld\helloworld\settings.PY** obsahuje Django nastavení pro aplikaci.
  -   **helloworld\helloworld\urls.PY** obsahuje kód mapování mezi všechny adresy url a jeho zobrazení.

1.  Vytvoření nového souboru s názvem **views.py** v adresáři *C:\inetpub\wwwroot\helloworld\helloworld* . To bude obsahovat požadované zobrazení vykreslení stránky "Vítáme". Spusťte editor a zadejte tyto údaje:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Nahraďte obsah souboru urls.py takto.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>Konfigurace služby IIS

1. Odemknete oblasti obslužné rutiny v globální applicationhost.config.  To vám umožní použít rutinu python v souboru web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Povolte WFastCGI.  To bude přidání aplikace do globální applicationhost.config, které odkazuje na spustitelný vaše video interpreter Python a skript wfastcgi.py.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Vytvoření souboru web.config v *C:\inetpub\wwwroot\helloworld*.  Hodnota `scriptProcessor` atribut by měly odpovídat výstup v předchozím kroku.  Zobrazení stránky pro [wfastcgi][] na pypi další na wfastcgi nastavení.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Aktualizujte umístění na serveru IIS výchozí webu přejděte na složku django projektu.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Nakonec načtěte webovou stránku v prohlížeči.

![Zobrazení stránky světě Ahoj na Azure okně prohlížeče][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Vypnutí virtuálního počítače Azure

Až skončíte s tohoto kurzu, vypněte a/nebo odebrání nově vytvořený Azure virtuálního počítače a uvolnit tak prostředky pro ostatní výukové programy pro účtovány Azure nadbytečným poplatkům.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
