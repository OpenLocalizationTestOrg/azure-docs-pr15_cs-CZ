<properties
    pageTitle="Různé způsoby vytváření Linux OM | Microsoft Azure"
    description="Přečtěte si způsobů, jak vytvořit virtuální počítač Linux na Azure, včetně odkazů na nástroje a výukové programy pro obou metod."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Různé způsoby vytváření Linux virtuálního počítače v Azure

Máte možnost v Azure vytvořit virtuální počítač Linux (OM) pomocí nástrojů a pracovní postupy pro vás pohodlnější. Tento článek shrnuje tato rozdíly a příklady pro vytváření Linux VMs.


## <a name="azure-cli"></a>Azure rozhraní příkazového řádku 

Rozhraní příkazového řádku Azure je k dispozici na platformách prostřednictvím balíčku npm, pokud distro balíčků nebo Docker kontejner. Další informace o [tom, jak nainstalovat a nakonfigurovat Azure rozhraní příkazového řádku](../xplat-cli-install.md). Následující kurzy jsou uvedeny příklady použití Azure rozhraní příkazového řádku. V každé článku najdete další informace o příkazech rychlého rozhraní příkazového řádku vidíte:

- [Vytvoření OM Linux z Azure rozhraní příkazového řádku pro vývojáře a test](virtual-machines-linux-quick-create-cli.md)
    - Následující příklad vytvoří OM jádro operačního systému pomocí veřejný klíč s názvem `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Vytvoření zabezpečené OM Linux pomocí Azure šablony](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Následující příklad vytvoří OM pomocí šablony ukládají na GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Vytvoření dokončení Linux prostředí pomocí rozhraní příkazového řádku Azure](virtual-machines-linux-create-cli-complete.md)
    - Vytvořit Vyrovnávání zatížení a více VMs sady dostupnost naformátovat.

- [Přidání disku Linux OM](virtual-machines-linux-add-disk.md)
    - Následující příklad přidává disk 5Gb existující OM s názvem `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portálu

[Azure portál](https://portal.azure.com) můžete rychle vytvořit virtuálního počítače, protože nic nainstalovat na počítač. Umožňuje vytvořit OM Azure portálu:

- [Vytvoření Linux OM pomocí portálu Azure](virtual-machines-linux-quick-create-portal.md) 
- [Připojte disk pomocí portálu Azure](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Obrázek možnosti a operační systém
Při vytváření virtuálního počítače, zvolte obrázek závislosti na operačním systému, který chcete spustit. Azure a partnerům nabízí mnoho obrázky, které patří nástroje předinstalované a aplikací. Nebo odešlete jednu z vlastní obrázky (viz [následující část](#use-your-own-image)).

### <a name="azure-images"></a>Azure obrázky
Použití `azure vm image` příkazy rozhraní příkazového řádku pro najdete v článku co je k dispozici Publisheru, distro vydání a sestavení.

Seznam dostupných vydavatelé následujícím způsobem:

```bash
azure vm image list-publishers --location WestUS
```

Seznam dostupných produktů (nabídka) pro dané publisher následujícím způsobem:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Seznam dostupných skladové jednotky (distro vydáních) dané nabídku následujícím způsobem:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Seznam všech dostupných obrázky pro spočívá v dané verzi:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Další příklady procházení a používání dostupné obrázků najdete v článku [obrázek a vyberte Azure virtuálního počítače obrázky s Azure rozhraní příkazového řádku](virtual-machines-linux-cli-ps-findimage.md).

`azure vm quick-create` a `azure vm create` příkazy mají aliasy můžete rychle zobrazit nejčastěji používaných distros a jejich nejnovější verze. Použití aliasů je často rychlejší než určující Publisheru, nabídky, SKU a verze pokaždé, když vytvoříte virtuálního počítače:

| Alias     | Aplikace Publisher | Nabídka        | SKU         | Verze |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | nejnovější  |
| Jádro operačního systému    | Jádro operačního systému    | Jádro operačního systému       | Stabilní      | nejnovější  |
| Debian    | credativ  | Debian       | 8           | nejnovější  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | nejnovější  |
| RHEL      | RedHat    | RHEL         | 7.2         | nejnovější  |
| SLES      | SLES      | SLES         | 12 SP1      | nejnovější  |
| UbuntuLTS | Kanonický | UbuntuServer | 14.04.4-LTS | nejnovější  |

### <a name="use-your-own-image"></a>Použití vlastní obrázek

Pokud požadujete zvláštní úpravy, můžete obrázek na základě existující OM Azure *pomocí* tohoto OM. Můžete taky nahrát obrázek vytvořený místní. Další informace o podporovaných distros a jak používat vlastní obrázky najdete v následujících článcích:

- [Azure potvrzeného rozdělení](virtual-machines-linux-endorsed-distros.md)

- [Informace o-potvrzeného rozdělení](virtual-machines-linux-create-upload-generic.md)

- [Jak zachytit Linux virtuálního počítače jako správce prostředků šablony](virtual-machines-linux-capture-image.md).
    - Rychlé zahájení příklad příkazy k zaznamenání existující OM:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Další kroky

- Z [portálu](virtual-machines-linux-quick-create-portal.md), pomocí [rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md)nebo pomocí [Správce prostředků Azure šablony](virtual-machines-linux-cli-deploy-templates.md)vytvořte OM Linux.

- Po vytvoření Linux OM [Přidat data disk](virtual-machines-linux-add-disk.md).

- Rychlé kroky k [resetování hesla nebo SSH klíče a Správa uživatelů](virtual-machines-linux-using-vmaccess-extension.md)
