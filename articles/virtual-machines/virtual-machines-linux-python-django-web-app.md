<properties 
    pageTitle="Python web app s Django na Linux | Microsoft Azure" 
    description="Zjistěte, jak hostovat na základě Django webovou aplikaci na Azure pomocí virtuálního počítače Linux." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Vítáme webová aplikace na Linux OM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac a Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Tento kurz popisuje hostovat web Django založené na Microsoft Azure pomocí virtuálního počítače Linux. Tento kurz předpokládá, že máte žádné zkušenosti pomocí Azure. Po dokončení tohoto průvodce, budete mít Django aplikace založené na nahoru a spuštěné v cloudu.

Se dozvíte, jak:

* Instalační program Azure virtuálního počítače na hostitele Django. Když tento kurz vysvětluje, jak to provést ve skupinovém rámečku **Linux**, stejné může taky provést OM serveru Windows použitý ve Azure. 
* Vytvořte novou aplikaci Django z Linux.

Provedením tohoto kurzu vytvoříte jednoduché Vítáme webové aplikace. Aplikace se použitý ve Azure virtuálního počítače.

Snímek obrazovky s dokončeným aplikace je níže:

![Zobrazení stránky světě Ahoj na Azure okně prohlížeče](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Vytváření a konfigurace Azure virtuálního počítače na hostitele Django

1. Postupujte podle pokynů vzhledem k tomu, [tady](virtual-machines-linux-quick-create-portal.md) vytvoření Azure virtuálního počítače *Se systémem Ubuntu serveru 14.04 l* rozdělení.  Pokud chcete, můžete ověřování hesla místo SSH veřejným klíčem.

1. Úprava skupiny zabezpečení sítě přenosy příchozí http na port 80, postupujte podle pokynů [v tomto poli](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Ve výchozím nastavení nový virtuální počítač nemá plně kvalifikovaný název domény (FQDN).  Můžete vytvořit jednu podle pokynů [v tomto poli](virtual-machines-linux-portal-create-fqdn.md).  Tento krok je nepovinný krok pro dokončení tohoto kurzu.

## <a id="setup"> </a>Nastavení vývojové prostředí:

**Poznámka:** Potřebujete-li nainstalovat Python nebo byste chtěli pomocí knihoven klienta, najdete v tématu [Průvodce instalací Python](../python-how-to-install.md).

OM Linux systémem Ubuntu už součástí Python 2.7 předinstalované, ale neobsahuje Apache nebo django nainstalovaný.  Postupujte podle kroků pro připojení k vaší OM a nainstalujte Apache a django.

1.  Spuštění nové okno **terminálu** .
    
1.  Zadejte tento příkaz připojit k OM Azure.  Pokud jste nevytvořili plně kvalifikovaný název domény, můžete se připojit pomocí veřejné adresy IP zobrazené v v portálu Azure klasické virtuálního počítače.

        $ ssh yourusername@yourVmUrl

1.  Zadejte následující příkazy k instalaci django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Zadejte následující příkaz nainstalovat Apache mod wsgi:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Vytvoření nové aplikace Django

1.  Otevřete okno **terminálu** , který jste použili v předchozí části ssh do svého OM.
    
1.  Zadejte následující příkazy k vytvoření nového projektu Django:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Skript **django admin.py** generuje základní struktura na základě Django weby:
    -   **HelloWorld/Manage.PY** vám pomůže začít, který je hostitelem a zastavit hostovat svůj web, na základě Django
    -   **HelloWorld/HelloWorld/Settings.PY** obsahuje Django nastavení pro aplikaci.
    -   **HelloWorld/HelloWorld/URLs.PY** obsahuje kód mapování mezi všechny adresy url a jeho zobrazení.

1.  Vytvoření nového souboru s názvem **views.py** v adresáři **/var/www/helloworld/helloworld** . To bude obsahovat požadované zobrazení vykreslení stránky "Vítáme". Spusťte editor a zadejte tyto údaje:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Teď nahraďte obsah souboru **urls.py** takto:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Nastavení Apache

1.  Vytvoření konfigurační soubor virtuální hostitele Apache **/etc/apache2/sites-available/helloworld.conf**. Nastavte obsah na následující a nahradit *yourVmName* skutečný název počítače, který používáte (příklad *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Povolení webu pomocí následujícího příkazu:

        $ sudo a2ensite helloworld

1.  Restartujte Apache pomocí následujícího příkazu:

        $ sudo service apache2 reload

1.  Nakonec načte webovou stránku v prohlížeči:

    ![Zobrazení stránky světě Ahoj na Azure okně prohlížeče](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Vypnutí virtuálního počítače Azure

Po dokončení se tento kurz, vypnutí a odebrat nově vytvořený Azure virtuálního počítače a uvolnit tak prostředky pro ostatní výukové programy pro účtovány Azure nadbytečným poplatkům.
