<properties 
    pageTitle="Python webové aplikace s lahev na v Azure" 
    description="Kurz, která vás seznámí s spuštěným Python web appem v Azure aplikace služby Web Apps." 
    services="app-service\web" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-bottle-in-azure"></a>Vytváření webových aplikací web apps s lahev na v Azure

Tento kurz popisuje, jak začít systém Python v Azure aplikace služby Web Apps. Web Apps poskytuje omezené bezplatná hostingu a rychlého nasazení a používáte Python! Narůstající velikostí aplikace, můžete přepnout na placené hostování a taky můžete integrovat s všechny ostatní služby Azure.

Vytvoříte web appu pomocí rozhraní lahev na web (viz alternativní verze tohoto kurzu [Django](web-sites-python-create-deploy-django-app.md) a [baňky](web-sites-python-create-deploy-flask-app.md)). Vytvoření webové aplikace z webu Azure Marketplace, nastavíte libovolná nasazení a klonovat úložiště místně. Potom se spustí web appu místně, proveďte změny, potvrdit a použít pro [Azure aplikace služby Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Kurz ukazuje, jak to provést z Windows nebo Mac a Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Systémy Windows, Mac a Linux
- Python 2.7 nebo 3.4
- setuptools, pip, virtualenv (pouze Python 2.7)
- Libovolná
- [Nástroje Python 2.2 for Visual Studio][] Poznámka: (PTVS) -: Toto je nepovinný krok

**Poznámka**: publikování TFS aktuálně nepodporuje Python projektů.

### <a name="windows"></a>Windows

Pokud ještě nemáte Python 2.7 nebo 3.4 nainstalovaný (32bitová verze), doporučujeme nainstalovat [Azure SDK Python 2.7] nebo [Azure SDK Python 3.4] pomocí webové platformy. Nainstalujete 32bitovou verzi Python setuptools, pip, virtualenv, atd (32bitová verze Python je nainstalovaných v počítačích Azure hostitele). Můžete taky dostanete Python z [python.org].

Libovolná doporučujeme [Libovolná pro Windows] nebo [GitHub pro Windows]. Pokud používáte Visual Studiu, můžete použít integrovanou libovolná podporu.

Také, doporučujeme nainstalovat [Python nástroje 2.2 for Visual Studio]. Toto je nepovinný krok, ale pokud máte [Visual Studiu], včetně bezplatné Visual Studio komunity 2013 nebo Visual Studio Express 2013 pro Web, potom to vám nabídne skvělé integrovaném vývojovém Python prostředí.

### <a name="maclinux"></a>Mac a Linux

Má mít Python a libovolná už nainstalovaný, ale zkontrolujte, jestli že máte Python 2.7 nebo 3.4.


## <a name="web-app-creation-on-the-azure-portal"></a>Vytvoření webové aplikace na portálu Azure

Cílem prvního kroku při vytváření aplikace je vytvořit web appu prostřednictvím [Portálu Azure](https://portal.azure.com).  

1. Přihlaste se k portálu Azure a klikněte na tlačítko **Nový** v levém dolním rohu. 
3. Do vyhledávacího pole zadejte "python".
4. Ve výsledcích hledání vyberte **lahev na**a potom klikněte na **vytvořit**.
5. Nakonfigurujte nové lahev na aplikaci, jako je vytvoření nového plánu aplikace služby a nové skupiny prostředků pro ho. Potom klikněte na **vytvořit**.
6. Postupujte podle pokynů v [Místním nasazení libovolná aplikace služby Azure](app-service-deploy-local-git.md)konfigurace libovolná publikování pro nově vytvořený web app.
 
## <a name="application-overview"></a>Přehled aplikací

### <a name="git-repository-contents"></a>Libovolná úložiště obsah

Tady je přehled soubory, které budete v počáteční libovolná úložiště, které jsme budete klonovat v další části.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Hlavní zdroje pro aplikaci. Obsahuje 3 stránek (o kontaktu index) s rozložení předlohy.  Statický obsah a skripty obsahují zavádění, jquery, knihovny a odpovědět na ni.

    \app.py

Podpora místní vývoje serveru. Slouží k místní spuštění aplikace.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

Soubory pro použití s [Python Tools for Visual Studio]projektu.

    \ptvs_virtualenv_proxy.py

Proxy serveru IIS virtuální prostředí a PTVS vzdálené ladění podpory.

    \requirements.txt

Externí balíčků potřeby tak, že této aplikace. Skript pro nasazení se pip instalace balíčků uvedené v tomto souboru.
 
    \web.2.7.config
    \web.3.4.config

Konfigurace služby IIS soubory. Skript pro nasazení pomocí příslušné web.x.y.config a Kopírovat jako web.config.

### <a name="optional-files---customizing-deployment"></a>Volitelné soubory – vlastní nastavení nasazení

[AZURE.INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Volitelné soubory - Python runtime

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Další soubory na server

Některé soubory na serveru, které ale nejsou přidali do úložiště libovolná. Toto jsou vytvářeny skript pro nasazení.

    \web.config

Konfigurační soubor Internetové informační služby. Vytvořená z web.x.y.config v každé nasazení.

    \env\

Virtuální prostředí Python. Pokud vytvoří se při nasazení kompatibilní virtuální prostředí ještě neexistuje na web appu.  Balíčky uvedené v requirements.txt jsou pip nainstalovaný, ale pip přeskočí instalaci, pokud už máte nainstalované balíčky.

Další 3 oddíly popisují, jak pokračovat vývoj aplikací webu klikněte v části 3 různých prostředích:

- Windows s Python Tools for Visual Studio
- Windows s příkazového řádku
- Mac a Linux příkazového řádku


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web App vývoj – Windows – Python nástroje for Visual Studio

### <a name="clone-the-repository"></a>Klonovat úložiště

Nejdřív klonovat úložiště pomocí adresy url umístěna na portálu Azure. Další informace najdete v tématu [Místní nasazení libovolná aplikace služby Azure](app-service-deploy-local-git.md).

Otevřete soubor řešení (SLN), který je součástí kořenové úložiště.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Vytvořit virtuální prostředí

Nyní vytvoříme virtuální prostředí pro místní vývoj. Klikněte pravým tlačítkem myši na **Prostředí Python** vyberte **Přidat virtuální prostředí …**.

- Přesvědčte se, zda je v poli Název prostředí `env`.

- Vyberte základní video interpreter. Zkontrolujte, že použít stejnou verzi aplikace Python vybrané pro webovou aplikaci (v runtime.txt nebo **Nastavení aplikace** zásuvné webovou aplikaci na portálu Azure).

- Ujistěte se, že je zaškrtnuté políčko si stáhněte a nainstalujte balíčků.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Klikněte na **vytvořit**. To vytvořit virtuální prostředí a nainstalujte závislosti uvedené v requirements.txt.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru

Stisknutím klávesy F5 ladění spustit a váš webový prohlížeč na stránku místně spuštěné automaticky otevře.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Nastavit zarážky ve zdrojích, použijte windows podívejte se na video atd. Zobrazit [Python Tools for Visual Studio dokumentace] Další informace o jednotlivých funkcích.

### <a name="make-changes"></a>Proveďte změny

Teď můžete experimentovat změnou zdrojů aplikace a/nebo šablony.

Po otestování změny potvrďte je do úložiště libovolná:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>Instalace další balíčků

Závislosti za Python a lahev na může mít vaše aplikace.

Další balíčků pomocí pip můžete nainstalovat. K instalaci balíčku, klikněte pravým tlačítkem myši na virtuální prostředí a vyberte **Nainstalovat Python balíčku**.

Instalace Azure SDK Python, které vám umožňuje přístup k Azure úložiště, bus služby a další služby Azure, zadejte například `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Klikněte pravým tlačítkem myši na virtuální prostředí a vyberte **Generovat requirements.txt** aktualizovat requirements.txt.

Pak potvrďte změny provedené v requirements.txt libovolná úložiště.

### <a name="deploy-to-azure"></a>Nasazení Azure

Spustit nasazení, klepněte na **synchronizovat** nebo **nabízená**. Synchronizace se push a vložit.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

První nasazení bude určitou dobu trvat, jako se vytváří prostředí virtuální balíčků Instalační, atd.

Visual Studio se nezobrazují průběh nasazení. Pokud chcete zkontrolovat výstup, naleznete v tématu [Poradce při potížích – nasazení](#troubleshooting-deployment).

Přejděte na adresu URL Azure zobrazíte provedené změny.


## <a name="web-app-development---windows---command-line"></a>Vývoj pro web app – Windows – příkazového řádku

### <a name="clone-the-repository"></a>Klonovat úložiště

Nejdřív klonovat úložiště pomocí adresy URL umístěna na portálu Azure a přidejte Azure úložiště jako vzdálené. Další informace najdete v tématu [Místní nasazení libovolná aplikace služby Azure](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Vytvořit virtuální prostředí

Vytvoříme nové prostředí virtuální pro účely vývoj (nepřidávejte jej do úložiště). Virtuální prostředí v Python nejsou přemístitelný, aby každý vývojář pracují na aplikaci bude vytvářet vlastní místně.

Zkontrolujte, že použít stejnou verzi aplikace Python vybrané pro webovou aplikaci (v runtime.txt nebo zásuvné nastavení aplikace pro web app na portálu Azure)

Pro Python 2.7:

    c:\python27\python.exe -m virtualenv env

Pro Python 3.4:

    c:\python34\python.exe -m venv env

Nainstalujte všechny externí balíčky vyžaduje vaše aplikace. Soubor requirements.txt kořenové úložiště slouží k instalaci balíčků ve vašem prostředí virtuální:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru

Samostatné aplikace v části vývojového serveru pomocí následujícího příkazu:

    env\scripts\python app.py

Na konzole se zobrazí adresu URL a přijímá port serveru:

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Potom otevřete webový prohlížeč na tuto adresu URL.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Proveďte změny

Teď můžete experimentovat změnou zdrojů aplikace a/nebo šablony.

Po otestování změny potvrďte je do úložiště libovolná:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace další balíčků

Závislosti za Python a lahev na může mít vaše aplikace.

Další balíčků pomocí pip můžete nainstalovat. Instalace Azure SDK Python, které vám umožňuje přístup k Azure úložiště, bus služby a další služby Azure, zadejte například:

    env\scripts\pip install azure

Zkontrolujte, že aktualizace requirements.txt:

    env\scripts\pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení Azure

Spustit nasazení, použít změny pro Azure:

    git push azure master

Zobrazí se výstup skript pro nasazení včetně virtuální prostředí vytváření instalace balení, vytvoření web.config.

Přejděte na adresu URL Azure zobrazíte provedené změny.


## <a name="web-app-development---maclinux---command-line"></a>Vývoj aplikací – Mac a Linux - příkazového řádku pro web

### <a name="clone-the-repository"></a>Klonovat úložiště

Nejdřív klonovat úložiště pomocí adresy URL umístěna na portálu Azure a přidejte Azure úložiště jako vzdálené. Další informace najdete v tématu [Místní nasazení libovolná aplikace služby Azure](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Vytvořit virtuální prostředí

Vytvoříme nové prostředí virtuální pro účely vývoj (nepřidávejte jej do úložiště). Virtuální prostředí v Python nejsou přemístitelný, aby každý vývojáři pracují na aplikaci bude vytvářet své vlastní místně.

Zkontrolujte, že použít stejnou verzi aplikace Python vybrané pro webovou aplikaci (v runtime.txt nebo nastavení aplikace zásuvné webovou aplikaci na portálu Azure).

Pro Python 2.7:

    python -m virtualenv env

Pro Python 3.4:

    python -m venv env
nebo pyvenv Obálka

Nainstalujte všechny externí balíčky vyžaduje vaše aplikace. Soubor requirements.txt kořenové úložiště slouží k instalaci balíčků ve vašem prostředí virtuální:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru

Samostatné aplikace v části vývojového serveru pomocí následujícího příkazu:

    env/bin/python app.py

Na konzole se zobrazí adresu URL a přijímá port serveru:

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Potom otevřete webový prohlížeč na tuto adresu URL.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Proveďte změny

Teď můžete experimentovat změnou zdrojů aplikace a/nebo šablony.

Po otestování změny potvrďte je do úložiště libovolná:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace další balíčků

Závislosti za Python a lahev na může mít vaše aplikace.

Další balíčků pomocí pip můžete nainstalovat. Instalace Azure SDK Python, které vám umožňuje přístup k Azure úložiště, bus služby a další služby Azure, zadejte například:

    env/bin/pip install azure

Zkontrolujte, že aktualizace requirements.txt:

    env/bin/pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení Azure

Spustit nasazení, použít změny pro Azure:

    git push azure master

Zobrazí se výstup skript pro nasazení včetně virtuální prostředí vytváření instalace balení, vytvoření web.config.

Přejděte na adresu URL Azure zobrazíte provedené změny.


## <a name="troubleshooting---package-installation"></a>Poradce při potížích – instalační balíček

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Poradce při potížích – virtuální prostředí

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="next-steps"></a>Další kroky

Tyto odkazy vedou na další informace o lahve a nástroje Python for Visual Studio: 
 
- [Lahev na si přečtěte následující dokumentaci]
- [Python Tools for Visual Studio si přečtěte následující dokumentaci]

Informace o použití úložiště tabulek Azure a MongoDB:

- [Lahev na a MongoDB na Azure s Python Tools for Visual Studio]
- [Lahev na a úložiště tabulek Azure na Azure s Python Tools for Visual Studio]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Lahev na a MongoDB na Azure s Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md
[Lahev na a Azure Table Storage na Azure s Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Azure SDK Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Libovolná pro Windows]: http://msysgit.github.io/
[GitHub pro Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python 2.2 Tools for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio si přečtěte následující dokumentaci]: http://aka.ms/ptvsdocs 
[Lahev na si přečtěte následující dokumentaci]: http://bottlepy.org/docs/dev/index.html
 
