Některé balíčky nemusí nainstalovat pomocí pip při spuštění aplikace v Azure.  Jednoduše je možné, že není k dispozici na Index balíčku Python balíček.  Je možné, že není potřeba kompilátoru (kompilátoru není k dispozici v počítači spuštěná web appu v aplikaci služby Azure).

V tomto oddílu se podíváme na způsobech řešení tohoto problému.

### <a name="request-wheels"></a>Žádost o wheels (kola)

Vlastníkovi balíčku požadavek kola umožňující balíček by měl obrátit instalační balíček vyžaduje kompilátoru.

S poslední dostupnost [Kompilátoru Microsoft Visual C++ pro Python 2.7][]je teď jednodušší k vytváření balíčků s nativní kód pro Python 2.7.

### <a name="build-wheels-requires-windows"></a>Vytvoření kola (vyžaduje Windows)

Poznámka: Pokud chcete použít tuto možnost, zkontrolujte, že zpracováním balíček pomocí prostředí Python, která odpovídá platformy/architektura/verze, která se použije ve web appu v aplikaci služby Azure (Windows/32-bit/2.7 nebo 3.4).

Pokud balení nejde nainstalovat, protože se musí kompilátoru, můžete nainstalovat kompilátoru na místním počítači a vytvoření kolo balíčku, které pak vložíte v úložišti.

: Mac a Linux Pokud nemáte přístup k počítači Windows, uvidí [vytvořit virtuální počítač systém Windows][] pro vytvoření virtuálního počítače na Azure.  Můžete ji použijete k vytváření kola, je přidat do úložiště a pokud chcete zrušit OM. 

Pro Python 2.7 můžete nainstalovat [Microsoft Visual C++ kompilátoru pro Python 2.7][].

Python 3.4 můžete nainstalovat [Microsoft Visual C++ 2010 Express][].

Vytvoření kola, musíte mít balíček ozubeným kolečkem:

    env\scripts\pip install wheel

Které budete používat `pip wheel` sestavíte závislost:

    env\scripts\pip wheel azure==0.8.4

Tím vytvoříte soubor .whl ve složce \wheelhouse.  Přidání souborů složku a ozubeným kolečkem \wheelhouse do úložiště.

Úprava requirements.txt přidat `--find-links` možnost nahoře. Toto říká pip můžete vyhledávat přesné shody v místní složce před přechodem do indexu balíčku python.

    --find-links wheelhouse
    azure==0.8.4

Pokud chcete zahrnout všechny závislosti mezi úkoly ve složce \wheelhouse a nepoužívat index balíčku python vůbec, můžete vynutit pip přeskočit index balíčku přidáním `--no-index` do horní části vaší requirements.txt.

    --no-index

### <a name="customize-installation"></a>Přizpůsobení instalace

Je možné upravit skriptu nasazení a instalace sady v prostředí virtuální použití alternativní instalační služby, jako například snadné\_nainstalovat.  V tématu deploy.cmd příklad, který je změněna na komentář.  Ujistěte se, že takové balíčky nejsou uvedené v requirements.txt pip zabráníte jejich instalace.

Přidáte tento skript pro nasazení:

    env\scripts\easy_install somepackage

Taky možná budete moct používat snadné\_nainstalovat k instalaci exe instalačního programu (některé jsou zip kompatibilní, tak snadno\_je instalace podporuje).  Přidání instalační program do úložiště a vyvolat snadné\_nainstalovat předáním cestu spustitelný soubor.

Přidáte tento skript pro nasazení:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Zahrnout virtuální prostředí v úložišti (vyžaduje Windows)

Poznámka: Pokud chcete použít tuto možnost, zkontrolujte, že pomocí prostředí virtuální, která odpovídá platformy/architektura/verze, která se použije ve web appu v aplikaci služby Azure (Windows/32-bit/2.7 nebo 3.4).

Jestliže zahrnete virtuální prostředí v úložišti, můžete zabránit skript pro nasazení způsobem virtuální prostředí management na Azure vytvořením prázdný soubor:

    .skipPythonDeployment

Doporučujeme odstranit existující prostředí virtuální v aplikaci, aby přebývající soubory ze kdy byl virtuální prostředí spravovány automaticky.


[Vytvoření virtuálního počítače s Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ kompilátoru pro Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
