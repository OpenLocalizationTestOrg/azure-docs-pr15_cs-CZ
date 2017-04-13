<properties
    pageTitle="Instalace Python a SDK - Azure"
    description="Zjistěte, jak nainstalovat Python a SDK pomocí služby Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Instalace Python a SDK

Python je snadné v systému Windows a přijat předinstalovaná [Flám pro systém Windows](https://msdn.microsoft.com/commandline/wsl/about), Mac a Linux. Tato příručka vás provede instalaci a příprava počítače k použití s Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Co je v Python Azure SDK?

Azure SDK Python obsahuje prvky, které umožňují vývoj, nasazení a správu Python žádosti o Azure. Konkrétně SDK Azure pro Python patří:

* **Knihovny správy**. Tato knihovna tříd poskytují rozhraní Správa Azure zdroje, jako je třeba úložiště účty virtuálních počítačích.

* **Knihovny Runtime**. Tato knihovna tříd poskytují rozhraní pro přístup k Azure funkce, jako jsou služby pro ukládání a bus.

## <a name="which-python-and-which-version-to-use"></a>Které Python a kterou verzi budu používat

Jsou k dispozici několik provedeních Python tlumočníky – jako příklad lze uvést:

* CPython – standardní a nejčastěji používaná Python video interpreter
* PyPy – rychlé a požadavkům alternativní implementaci CPython
* IronPython – video interpreter Python, která poběží na .net/CLR
* Jython – video interpreter Python, která poběží na virtuálního počítače Java

**CPython** v2.7 nebo v3.3 + PyPy 5.4.0 testováno a podporované pro Python Azure SDK.

## <a name="where-to-get-python"></a>Kde získat Python?

Chcete-li získat CPython několika způsoby:

* Přímo z [www.python.org][]
* Z renomovanou distro ATP [www.continuum.io][], [www.enthought.com][] [www.activestate.com][]
* Vytvořit ze zdroje.

Pokud máte konkrétní důvod, doporučujeme nejdřív dvou možností.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalace SDK ve Windows, Linux a MacOS (pouze knihoven klienta)

Pokud už máte Python nainstalovaný, můžete nainstalovat balíku všechny knihoven klienta v prostředí Python 3.3 + nebo existující 2.7 Python pip. Balíčky to bude stáhnout z [Python balíčku Index][] (PyPI).

Může být nutné práva správce:

- Linux a MacOS, použít `sudo` příkazu: `sudo pip install azure-mgmt-compute`.
- Systém Windows: jako správce otevřete prostředí PowerShell/příkazový řádek

Můžete nainstalovat jednotlivě každé knihovny pro každou službu Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Náhled balíčků je možné nainstalovat pomocí `--pre` příznak:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Můžete taky nainstalovat sadu Azure knihoven v jediném řádku, který používá `azure` meta balíčku. Protože ne všechny balíčků v tomto meta balíčku jsou publikované jako stálé ještě `azure` meta balíčku je stále ve verzi preview. Však core balíčků z kód kvality/dokončení perspektiv považovat za "stabilní" v současnosti
- to bude úředně označen jako takové synchronní v jiných jazycích co nejdříve. Na jakékoli další hlavními změnami jsme nejsou plánování do té doby.

Protože je verzi preview, je potřeba použít `--pre` příznaku:

```console
   $ pip install --pre azure
```
   
nebo přímo

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Získání další balíčků

[Python balíčku Index][] (PyPI) obsahuje propracovaných výběru Python knihoven.  Pokud jste se rozhodli nainstalovat Distro, budete mít už většina zajímavé posunuto různých scénářích z vývoj webu do technické výpočetních.


## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio

[Python Tools for Visual Studio][] (PTVS) je modul plug-in free/OSS společnosti Microsoft, které změní a plnohodnotný integrovaném vývojovém Python prostředí:

![How-k-nainstalovat python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Použití PTVS vynechán, doporučuje se ale jako nabízí podpory Python a Web projektu/řešení, ladění, vytváření profilů, interaktivní okně Úprava šablony a technologie Intellisense.

PTVS také usnadňuje nasadit na Microsoft Azure s podporou pro nasazení [Cloudovými službami][] a [weby][].

PTVS funguje s vaší stávající Visual Studio 2013 nebo instalace 2015.  Dokumentace, stahování a diskuse najdete v tématu [Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scénáře Linux a MacOS

Linux nebo MacOS, hlavní Azure scénáře, které jsou podporované:

1. Využívající služby Azure pomocí knihoven klienta pro Python

2. Spuštění aplikace v Linux OM

3. Vývoj a publikování webů Azure pomocí libovolná

První scénář umožňuje vytváření působivých webových aplikací, které využít výhod možností Azure PaaS například [úložiště objektů blob][] [úložiště fronty][] [úložiště tabulek][] atd prostřednictvím Pythonic obálky pro rozhraní REST API Azure. Tyto funguje stejně jako ve Windows, Mac a Linux.  Můžete taky těchto knihoven klienta z počítače místní rozvoj nebo Linux OM spuštěna Azure.

Scénář OM jednoduše spusťte Linux OM podle svého výběru (se systémem Ubuntu, CentOS, Suse) a spustit a spravovat který se vám líbí.  Jako příklad můžete spustit [IPython][] REPL/Poznámkový blok v počítači Windows nebo Mac a Linux a Linux nebo modul IPython spuštěna Azure OM použití více procesorů Windows přejděte v prohlížeči. V tématu kurz [IPython Poznámkový blok na Azure][] Další informace.

Informace o nastavení Linux OM najdete v článku kurz [Vytvoření virtuálního počítače systém Linux][] .

Libovolná nasazení můžete vyvíjet aplikace webu Python a publikování na web Azure z žádný operační systém.  Po stisknutí úložiště Azure, vytvoří automaticky prostředí virtuální a pip instalovat požadovaných balíčky.

Další informace o vývoje a publikování Azure webů naleznete v tématu kurzy [Vytváření webů s Django][], [Vytváření webů s lahev na][]a [Vytváření webů s baňky][]. Další obecné informace o použití vyhovujících zápisu WSGI framework najdete v článku [Konfigurace Python se Azure weby][].


## <a name="additional-software-and-resources"></a>Další Software a prostředky:

* [Azure SDK Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Úřední Azure vzorky Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Poměrně analýzy Python rozdělení][]
* [Enthought Python rozdělení][]
* [ActiveState Python rozdělení][]
* [SciPy - sady matematickém Python knihoven][]
* [NumPy – knihovně číslovky pro Python][]
* [Django Project – zdokonaleným web framework/cm][]
* [IPython - což Upřesnit REPL/Poznámkový blok pro Python][]
* [IPython poznámkového bloku na Azure][]
* [Python Tools for Visual Studio na GitHub][]
* [Středisko pro vývojáře Python](/develop/python/)

[Poměrně analýzy Python rozdělení]: http://continuum.io
[Enthought Python rozdělení]: http://www.enthought.com
[ActiveState Python rozdělení]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - sady matematickém Python knihoven]: http://www.scipy.org
[NumPy – knihovně číslovky pro Python]: http://www.numpy.org
[Django Project – zdokonaleným web framework/cm]: http://www.djangoproject.com
[IPython - což Upřesnit REPL/Poznámkový blok pro Python]: http://ipython.org
[IPython]: http://ipython.org
[Poznámkový blok IPython na Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Weby]: web-sites-python-ptvs-django-mysql.md
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio na GitHub]: https://github.com/microsoft/ptvs
[Python balíček indexu]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Vytváření virtuálních počítač Linux]: virtual-machines-linux-quick-create-cli.md
[Vytváření webů s Django]: web-sites-python-create-deploy-django-app.md
[Vytváření webů s lahev na]: web-sites-python-create-deploy-bottle-app.md
[Vytváření webů s baňky]: web-sites-python-create-deploy-flask-app.md
[Konfigurace Python se Azure weby]: web-sites-python-configure.md
[úložiště tabulek]: storage-python-how-to-use-table-storage.md
[úložiště fronty]: storage-python-how-to-use-queue-storage.md
[úložiště objektů BLOB]: storage-python-how-to-use-blob-storage.md
