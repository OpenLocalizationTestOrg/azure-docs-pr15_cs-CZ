<properties
    pageTitle="Instalace rozhraní Azure příkazového řádku | Microsoft Azure"
    description="Instalace Azure rozhraní příkazového řádku (rozhraní příkazového řádku) for Mac, Linux a Windows začít používat služby Azure"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Instalace Azure rozhraní příkazového řádku

> [AZURE.SELECTOR]
- [Prostředí PowerShell](powershell-install-configure.md)
- [Azure rozhraní příkazového řádku](xplat-cli-install.md)

Rychle nainstalujte Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku) použít sadu otevřít zdroj založený na prostředí příkazy pro vytváření a Správa zdrojů v Microsoft Azure. Máte několik možností nainstalovat tyto různé platformy nástroje v počítači: 

* **balíček npm** – spuštění npm (Správce balíčků JavaScriptu) nainstalovat nejnovější Azure rozhraní příkazového řádku na Linux rozdělení nebo s operačním systémem. Vyžaduje node.js a npm ve vašem počítači.
* **Instalační program** – stažení an, chyby instalační program pro snadné instalaci na Mac a Windows.
* **Kontejner docker** - začít používat nejnovější rozhraní příkazového řádku v kontejneru Docker připravené k použití. Vyžaduje Docker host ve vašem počítači.
    
Další možnosti a pozadí najdete v článku project úložiště na [GitHub](https://github.com/azure/azure-xplat-cli). 

Po rozhraní příkazového řádku Azure nainstalovali, [připojte ho k předplatnému Azure](xplat-cli-connect.md) a spusťte **azure** příkazy z příkazového řádku rozhraní (flám terminálu, příkazový řádek a tak dál) pro práci s Azure zdroje.



## <a name="option-1-install-an-npm-package"></a>Možnost 1: Instalace balíčku npm

Při instalaci rozhraní příkazového řádku z balíčku npm, ujistěte se, jste stáhli a nainstalovali [nejnovější Node.js a npm](https://nodejs.org/en/download/package-manager/). Spusťte **instalaci npm** nainstalovat azure rozhraní příkazového řádku: 

    npm install -g azure-cli

Na Linux distribuce možná muset používat **sudo** úspěšně spustit příkaz __npm__ následujícím způsobem:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Pokud potřebujete nainstalovat nebo aktualizovat Node.js a npm na Linux rozdělení nebo operační systém, doporučujeme nainstalovat nejnovější verzi Node.js l (4.x). Pokud používáte starší verzi, může se zobrazit chyby při instalaci. 

V případě potřeby stáhnout nejnovější Linux [vkládání souboru] [ linux-installer] balíčku npm místně. Nainstalujte balíček stažené npm takto (na Linux distribuce možná budete muset používat **sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Možnost 2: Použití instalačního programu

Pokud používáte počítač Mac a Windows, jsou k dispozici ke stažení následující instalační rozhraní příkazového řádku:

* [Instalační služba systému Mac OS X][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]Ve Windows můžete také stáhnout [Webové platformy](https://go.microsoft.com/?linkid=9828653) instalaci rozhraní příkazového řádku. Instalační služba poskytuje možnost nainstalovat další SDK Azure a nástroje příkazového řádku po instalaci rozhraní příkazového řádku. 


## <a name="option-3-use-a-docker-container"></a>Možnost 3: Použití Docker kontejneru

Pokud jste nastavili počítače jako hostitel [Docker](https://docs.docker.com/engine/understanding-docker/) , můžete spustit nejnovější Azure rozhraní příkazového řádku v kontejneru Docker. Spusťte tento příkaz (na Linux distribuce možná budete muset používat **sudo**):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Spuštění příkazů rozhraní příkazového řádku Azure
Po instalaci používat rozhraní příkazového řádku Azure příkaz **azure** z příkazového řádku uživatelského rozhraní (flám terminálu, příkazový řádek a tak dál). Například pro spuštění příkazu nápovědy, zadejte tento příkaz:

```
azure help
```
> [AZURE.NOTE]Na některých distribuce Linux může dojít k chybě podobné `/usr/bin/env: ‘node’: No such file or directory`. Tato chyba se berou poslední zařízení Node.js instalována ve /usr/bin/nodejs. Vyřešíte ho vytvořte symbolického odkazu na /usr/bin/node spuštěním tento příkaz:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Zobrazit verzi Azure rozhraní příkazového řádku jste si nainstalovali, zadejte tento příkaz:

```
azure --version
```

Teď jste připraveni! Přístup k příkazu rozhraní příkazového řádku pro práci s vlastními prostředky, [připojit k předplatnému Azure z Azure rozhraní příkazového řádku](xplat-cli-connect.md).

>[AZURE.NOTE] Při prvním použití rozhraní příkazového řádku Azure, zobrazí se zpráva s dotazem, jestli chcete umožnit společnosti Microsoft shromažďování informací použití. Účast je nepovinného. Pokud se rozhodnete chcete tohoto programu zúčastnit, můžete kdykoli ukončit spuštěním `azure telemetry --disable`. Pokud chcete povolit účast kdykoli, spusťte `azure telemetry --enable`.


## <a name="update-the-cli"></a>Aktualizace rozhraní příkazového řádku

Společnost Microsoft často vydává aktualizované verze Azure rozhraní příkazového řádku. Přeinstalace rozhraní příkazového řádku pomocí instalačního programu operačního systému nebo spuštění nejnovější Docker kontejner. Nebo pokud máte Node.js nejnovějšími npm nainstalovaný, aktualizujte zadáním následujícího (na Linux distribuce možná budete muset používat **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Povolení doplňování tabulátorů

Karta dokončení příkazů rozhraní příkazového řádku je podporovaný pro Mac a Linux.

Aby se v zsh, spusťte:

```
echo '. <(azure --completion)' >> .zshrc
```

Povolit v flám, spusťte:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Další kroky 

* [Připojení z rozhraní příkazového řádku k předplatnému Azure](xplat-cli-connect.md) můžete vytvořit a spravovat Azure zdroje.

* Další informace o rozhraní příkazového řádku Azure, stáhněte si zdrojového kódu, hlášení problémů nebo přispívat do projektu, navštivte [GitHub úložiště pro rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).

* Pokud máte dotazy k používání rozhraní příkazového řádku Azure nebo Azure, navštivte [Fóra pro Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Pokud chcete, můžete zkusit také na základě Python [Azure rozhraní příkazového řádku 2.0 náhled](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
