#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Jak používat nástroje Azure příkazového řádku pro Mac a Linux

Tato příručka popisuje, jak používat nástroje Azure příkazového řádku pro Mac a Linux můžete vytvořit a spravovat služby Azure. Scénáře nichž se uplatní zahrnují **Instalace nástrojů**, **Import nastavení publikování**, **vytváření a správě Azure webů**a **vytváření a správě Azure virtuálních počítačích**. Dokumentace podrobné informace najdete v tématu [nástroj služby Azure příkazového řádku pro Mac a Linux si přečtěte následující dokumentaci][reference-docs]. 

##<a name="table-of-contents"></a>Obsahy
* [Jaké nástroje Azure příkazového řádku pro Mac a Linux](#Overview)
* [Jak nainstalovat nástroje Azure příkazového řádku pro Mac a Linux](#Download)
* [Jak vytvořit účet Azure](#CreateAccount)
* [Stažení a importovat nastavení publikování](#Account)
* [Jak vytvořit a spravovat webový server Azure](#WebSites)
* [Jak vytvořit a spravovat virtuální počítač Azure](#VMs)


<h2><a id="Overview"></a>Jaké nástroje Azure příkazového řádku pro Mac a Linux</h2>

Nástroje Azure příkazového řádku pro Mac a Linux jsou sady nástrojů příkazového řádku pro nasazení a správu Azure služby.
 
Podporované úkoly patřit následující úkoly:

* Importujte nastavení publikování.
* Vytvářet a spravovat weby Azure.
* Vytváření a správa Azure virtuálních počítačích.

Úplný seznam podporovaných příkazů, zadejte `azure -help` příkazového řádku po instalaci nástrojů, nebo si přečtěte [následující dokumentaci pro odkaz][reference-docs].

<h2><a id="Download">Jak nainstalovat nástroje Azure příkazového řádku pro Mac a Linux</a></h2>

Následující seznam obsahuje informace o instalaci nástroje příkazového řádku v závislosti na operačním systému:

* **Mac**: stáhnout [Instalační služby Azure SDK][mac-installer]. Otevřete soubor stažený .pkg a postupujte podle instalace se zobrazí výzva.

* **Linux**: nainstalujte nejnovější verzi [Node.js] [ nodejs-org] (viz [Instalace Node.js přes Správce balíčků][install-node-linux]), spusťte tento příkaz:

        npm install azure-cli -g

    **Poznámka**: budete muset se zvýšenými oprávněními spusťte tento příkaz:

        sudo npm install azure-cli -g

* **Windows**: Spusťte instalační program Winows (souboru MSI), který najdete tady: [Azure příkazového řádku nástroje][windows-installer].


Chcete-li otestovat instalaci, zadejte `azure` na příkazovém řádku. Pokud instalace podaří, zobrazí se seznam všech dostupných `azure` příkazy.

<h2><a id="CreateAccount"></a>Jak vytvořit účet Azure</h2>

Použití nástroje Azure příkazového řádku pro Mac a Linux, budete potřebovat účet Azure.

Otevřete webový prohlížeč a přejděte k [http://www.windowsazure.com] [ windowsazuredotcom] a klikněte na **bezplatnou zkušební verzi** v pravém horním rohu.

![Webu Azure][Azure Web Site]

Postupujte podle pokynů pro vytvoření účtu.

<h2><a id="Account"></a>Stažení a importovat nastavení publikování</h2>

Nejdřív budete muset nejdřív stáhnout a importovat vaše nastavení publikování. To vám umožní při použití nástroje pro vytváření a správy služby Azure. Pokud si chcete stáhnout vaše nastavení publikování, použijte `account download` příkaz:

    azure account download

Tím otevřete výchozí prohlížeč a objeví se výzva k přihlášení k portálu pro správu. Po přihlášení, vaše `.publishsettings` stažení souboru. Poznamenejte si místo, kam se tento soubor uložen.

Pak importovat `.publishsettings` soubor spuštěním následujícího příkazu, nahrazení `{path to .publishsettings file}` s cestou k vaší `.publishsettings` souboru:

    azure account import {path to .publishsettings file}

Můžete odebrat všechny informace uložené <code>import</code> příkazu pomocí <code>account clear</code> příkaz:

    azure account clear

Chcete-li zobrazit seznam možností pro `account` použijte příkazy, `-help` možnost:

    azure account -help

Po importu svého nastavení publikování, byste měli odstranit `.publishsettings` soubor z bezpečnostních důvodů.

> [AZURE.NOTE] Při importu nastavení publikování, přihlašovací údaje pro přístup k předplatného Azure jsou uloženy uvnitř vaší `user` složky. Vaše `user` složky chráněn operačního systému. Ale, doporučujeme provést další kroky k šifrování vaše `user` složky. Můžete to udělat takto:    
> 
> - Ve Windows můžete změnit vlastnosti složky nebo pomocí nástroje BitLocker.
> - Na Macu zapněte FileVault ke složce.
> - Na systémem Ubuntu použijte funkci adresáře šifrovaný Domů. Další distribuce Linux nabízejí odpovídajících funkcí.

Teď jste připraveni k vytvoření a správa Azure weby a Azure virtuálních počítačích.  

<h2><a id="WebSites"></a>Jak vytvořit a spravovat web na Azure</h2>

###<a name="create-a-website"></a>Vytvoření webu

Azure webu, nejdřív vytvořit prázdný adresář s názvem `MySite` a přejděte do adresáře.

Spusťte tento příkaz:

    azure site create MySite --git

Výstup tento příkaz bude obsahovat výchozí adresu URL pro nově vytvořený web. `--git` Možnost umožňuje použít libovolná publikovat na váš web vytvořením libovolná úložištích v adresáři místní aplikace a v datovém centru váš web. Všimněte si, že u složky se už úložišti libovolná příkazu přidá nový vzdáleného do existující úložiště ukazující do úložiště v datovém centru váš web.

Všimněte si, že můžete spustit `azure site create` příkazu pomocí kterékoli z těchto možností:

* `--location [location name]`. Tato možnost umožňuje určit umístění datovém centru, ve kterém je vytvořen vašeho webu (například "západní NAS"). Vynecháme-li tuto možnost, budou promted a zvolte umístění.
* `--hostname [custom host name]`. Tato možnost umožňuje zadat vlastní název hostitele pro váš web.

Pak můžete přidat obsah do adresáře webu. Používat normální libovolná tok (`git add`, `git commit`) potvrďte svůj obsah. Pomocí následujícího příkazu Libovolná nabízená obsahu webu Azure: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Nastavení publikování z GitHub

Nastavení nepřetržitý publikování z úložiště GitHub, můžete `--GitHub` možnost při vytváření webu:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Pokud máte místní klonovat úložišti GitHub nebo pokud máte úložiště s jednoho vzdáleného odkazu do úložiště GitHub, tento příkaz automaticky publikovat kód v úložišti GitHub na svůj web. Od změny posune GitHub úložiště bude automaticky publikován webu.

Nastavení publikování z GitHub výchozí větev používají při větvi předlohy. Chcete-li zadat jinou větev, spusťte následující příkaz z místní úložiště:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Konfigurace nastavení aplikace

Nastavení aplikace jsou klíč dvojice, které jsou k dispozici aplikaci za běhu. Při nastavení pro web na Azure, aplikace nastavení hodnoty přepíší nastavení klíčem se definují nastavení(Web.config)) vašeho webu. Aplikace Node.js a PHP jsou k dispozici jako proměnné nastavení aplikace. Následující příklad ukazuje, jak nastavit pár klíče hodnoty:

    azure site config add <key>=<value> 

Chcete-li zobrazit seznam všech dvojice klíč/hodnota, použijte následující:

    azure site config list 

Nebo pokud znáte klávesu a chcete zobrazit hodnotu, můžete:

    azure site config get <key> 

Pokud chcete změnit hodnotu existující klíč musíte nejprve zrušit zaškrtnutí existující klíč a pak ho znovu přidejte. Příkaz Vymazat je:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Seznam a zobrazit weby

Seznam vaše weby, použijte tento příkaz:

    azure site list

Podrobné informace o webu, použijte `site show` příkaz. Následující příklad ukazuje podrobnosti o `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Zastavení, spusťte nebo opětovné spuštění na webu

Zastavit, zahájení nebo restartujte webu se `site stop`, `site start`, nebo `site restart` příkazy:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Odstranění webu

Nakonec můžete odstranit web s `site delete` příkaz:

    azure site delete MySite

Poznámka: Pokud používáte některou z výše příkazy aplikace do složky, kde jste spustili `site create`, není nutné zadat název webu `MySite` jako poslední parametr.

Pokud chcete zobrazit úplný seznam `site` pomocí příkazů `-help` možnost:

    azure site -help 

<h2><a id="VMs"></a>Jak vytvořit a spravovat virtuální počítač Azure</h2>

Virtuální počítač Azure se vytvoří z virtuálního počítače obrázku (soubor VHD), který zadáte nebo, která je dostupná v galerii obrázek. Pokud chcete zobrazit obrázky, které jsou k dispozici, použijte `vm image list` příkaz:

    azure vm image list

Zřízení a spuštění virtuálního počítače z jednoho z dostupných obrázků se `vm create` příkaz. Následující příklad ukazuje, jak vytvořit virtuální počítač Linux (s názvem `myVM`) od obrázků v galerii obrázek (CentOS 6.2). Kořenové uživatelské jméno a heslo pro virtuální počítač `myusername` a `Mypassw0rd` v tomto pořadí. (Všimněte si, že `--location` parametr určuje datovém centru, ve kterém je vytvořen virtuální počítač. Pokud nezadáte `--location` parametr, zobrazí se výzva pro výběr místa konání.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Zvažte předávání `--ssh` příznak (Linux) nebo `--rdp` příznak (Windows) `vm create` povolení vzdáleného připojení, virtuální počítač nově vytvořený.

Pokud byste raději zřízení virtuálního počítače z vlastního obrázku, můžete vytvořit obrázek ze souboru VHD s `vm image create` příkaz a potom použijte `vm create` příkaz zřízení virtuální počítač. Následující příklad ukazuje, jak vytvořit obrázek Linux (s názvem `myImage`) ze souboru místní VHD. ( `--location` Parametr určuje data, ve kterém je obrázek uložen.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Místo abyste vytvářeli obrázek z místního VHD, můžete vytvořit obrázek z VHD uložených v úložišti objektů Blob Azure. Lze provést pomocí `blob-url` parametr:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Po vytvoření obrázku, můžete vytvořit virtuální počítač z obrázku pomocí `vm create`. Následující příkaz vytvoří virtuální počítač s názvem `myVM` z obrázku vytvořili nad (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Poté, co jste zřízení virtuálního počítače, můžete vytvořit koncové body umožňuje vzdálený přístup k počítači virtuální (například). V následujícím příkladu `vm create endpoint` příkaz otevření externí port 22 a místní port 22 na `myVM`:

    azure vm endpoint create myVM 22 22

Podrobné informace o virtuální počítač (včetně IP adresu, název DNS a informace koncového bodu) jaké jde dosáhnout pomocí `vm show` příkaz:

    azure vm show myVM

Vypnutí spustit, nebo restartujte virtuální počítač, použijte jeden z následujících příkazů:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

A nakonec OM, můžete odstranit `vm delete` příkaz:

    azure vm delete myVM

Úplný seznam příkazů pro vytváření a správu virtuálních počítačích, použít `-h` možnost:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

