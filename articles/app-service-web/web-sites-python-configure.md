<properties 
    pageTitle="Konfigurace Python s Azure služba aplikací Web Apps" 
    description="Tento kurz popisuje možnosti pro vytváření a konfigurace základní serveru webové aplikace kompatibilní s Python rozhraní brány (WSGI) na Azure aplikace služby Web Apps." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurace Python s Azure služba aplikací Web Apps

Tento kurz popisuje možnosti pro vytváření a konfigurace aplikace základní kompatibilní se standardem Python Webový Server brány rozhraní (WSGI) na [Azure aplikace služby Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Popisuje funkce libovolná nasazení, například virtuální prostředí a pomocí requirements.txt instalační balíček.


## <a name="bottle-django-or-flask"></a>Lahev na Django nebo baňky?

Azure Marketplace obsahuje šablony pro lahve, Django a baňky předlohy. Pokud vyvíjíte první webovou aplikaci v aplikaci služby Azure nebo znáte není libovolná, doporučujeme proveďte jeden z těchto kurzech, které obsahují podrobné pokyny k vytváření aplikace fungovat v galerii pomocí libovolná nasazení z Windows nebo Mac:

- [Vytváření webových aplikací web apps s lahev na](web-sites-python-create-deploy-bottle-app.md)
- [Vytváření webových aplikací web apps s Django](web-sites-python-create-deploy-django-app.md)
- [Vytváření webových aplikací web apps s baňky](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Vytvoření webové aplikace na portálu Azure

Tento kurz předpokládá existující Azure předplatné a přístup na portál Azure.

Pokud nemáte aplikaci pro existující web, můžete vytvořit jednu z [Portálu Azure](https://portal.azure.com).  Klikněte na tlačítko Nový v levém horním rohu a potom na **Web + Mobile** > **Web appu**.

## <a name="git-publishing"></a>Libovolná publikování

Postupujte podle pokynů v [Místním nasazení libovolná aplikace služby Azure](app-service-deploy-local-git.md)konfigurace libovolná publikování pro nově vytvořený web app. Tento kurz libovolná používá k vytváření, Správa a publikovat naše Python webové aplikace služby Azure aplikace.

Po publikování libovolná úložišti libovolná bude vytvořen a přidružený k vaší webové aplikaci. Adresa URL v úložišti se zobrazí a nadále lze nabízená data z místního vývojové prostředí do cloudu. Publikování aplikací prostřednictvím libovolná, ujistěte se, že libovolná klienta i nainstalovali a postupujte podle pokynů uvedených Pokud chcete použít obsahem webu aplikace pro aplikaci služby Azure.


## <a name="application-overview"></a>Přehled aplikací

V dalších částech vzniká následující soubory. By měly být umístěny v kořenové úložiště libovolná.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>Obslužná rutina WSGI

WSGI je standard Python označená [období 3333](http://www.python.org/dev/peps/pep-3333/) definování rozhraní mezi webový server a Python. Poskytuje standardizovaným rozhraní pro psaní různých webových aplikací a rámce pomocí Python. Oblíbené rámce webové Python Dnes pomocí WSGI. Azure aplikace služby Web Apps nabízí podpory těchto rámce; Kromě toho zkušení uživatelé mohou i vytvářet své vlastní jako vlastní rutiny se řídí WSGI specifikace pokyny.

Tady je příklad `app.py` , který definuje vlastní rutinu:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Spuštění této aplikace místně `python app.py`, vyhledejte `http://localhost:5555` ve webovém prohlížeči.


## <a name="virtual-environment"></a>Virtuální prostředí

Ačkoli aplikace příklad nevyžaduje externí balíčků, je pravděpodobné, že aplikace vyžaduje některé.

K plánování externích balíčku závislosti, podporuje Azure libovolná nasazení vytváření virtuálních prostředí.

Když Azure rozpozná requirements.txt kořenové úložiště, automaticky vytvoří prostředí virtuální s názvem `env`. Pouze v takovém na prvním nasazení nebo během jakékoli nasazení za vybrané Python runtime změnil.

Pravděpodobně bude nutné vytvořit virtuální prostředí místně pro vývoj, ale nechcete zahrnout do libovolná úložiště.


## <a name="package-management"></a>Správa balíčku

Balíčky uvedené v requirements.txt nainstaluje automaticky v prostředí virtuální pomocí pip. K tomu dojde na každé nasazení, ale pip přeskočí instalace již nainstalovali balíček.

Příklad `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python verze

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Příklad `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Je potřeba vytvořit soubor web.config můžete určit, jak zacházet s žádostí o serveru.

Poznámka: Pokud máte otevřený soubor web.x.y.config v úložišti, kde x.y odpovídá vybrané runtime Python, a pak Azure bude automaticky příslušný soubor jako web.config.

Následující příklady web.config spolehnout skriptu prostředí virtuální proxy, který je popsaný v další části.  Práce s obslužné WSGI použitých v tomto příkladu `app.py` nad.

Příklad `web.config` pro Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Příklad `web.config` pro Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statické soubory se uskutečněných jednotlivými webového serveru přímo, bez opakovaného Python kód pro zvýšení výkonu.

V předchozích příkladech umístění statické soubory na disku by měly odpovídat na místo v adrese URL. To znamená, že na žádost o `http://pythonapp.azurewebsites.net/static/site.css` poslouží souboru na disk v `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`je místo, kam zadáte obslužné WSGI. V předchozích příkladech má `app.wsgi_app` protože obslužné rutiny je funkce s názvem `wsgi_app` v `app.py` v kořenové složce.

`PYTHONPATH`můžete přizpůsobit, ale pokud nainstalujte všechny závislosti mezi úkoly v prostředí virtuální tím, že zadáte v requirements.txt neměli musíte ho změnit.


## <a name="virtual-environment-proxy"></a>Virtuální prostředí Proxy

Tento skript slouží k načtení obslužné WSGI, aktivovat virtuální prostředí a protokolu chyb. Je určená obecné a použité bez změny.

Obsah `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Přizpůsobení libovolná nasazení

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Poradce při potížích – instalační balíček

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Poradce při potížích – virtuální prostředí

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Python](/develop/python/).

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)





 
