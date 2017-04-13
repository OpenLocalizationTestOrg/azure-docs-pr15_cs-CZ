<properties
    pageTitle="Vytvořte a uložte vlastní obrázek Linux | Microsoft Azure"
    description="Vytvořte a uložte virtuální pevný disk (virtuální pevný disk) na Azure s obrázkem na vlastní Linux pomocí nasazení modelu správce prostředků."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Ukládat a vytvářet OM Linux z disku vlastní obrázek

V tomto článku se dozvíte, jak ukládat virtuální pevný disk (virtuální pevný disk) Azure pomocí Správce prostředků nasazení modelu a vytvářet Linux VMs z tento vlastní obrázek. Tato funkce umožňuje instalace a konfigurace Linux distro k vašim požadavkům a potom použijte tento virtuální pevný disk a rychle vytvořte Azure virtuálních počítačích (VMs).

## <a name="quick-commands"></a>Rychlé příkazy
Pokud potřebujete rychle provést úkol tyto údaje oddíl základu příkazy k nahrání OM na Azure. Podrobnější informace a kontext pro každý krok najdete zbytek dokumentu a [od tady](#requirements).

Ujistěte se, jestli máte [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) přihlášení a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.

Vytvoření skupiny zdrojů. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUs` umístění:

```bash
azure group create myResourceGroup --location "WestUS"
```

Vytvoření účtu úložiště pro uložení virtuální discích. Následující příklad vytvoří úložiště účet s názvem `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Seznam přístupových kláves pro váš účet úložiště. Poznamenejte si `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Vytvoření kontejneru v rámci vašeho účtu úložiště pomocí klávesy úložiště, kterou jste získali. Následující příklad vytvoří kontejneru s názvem `myimages` úložiště klíčových hodnot z `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Nakonec Nahrajte svůj virtuální pevný disk na kontejner, který jste vytvořili. Místní cestu k vaší virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Teď můžete vytvářet virtuálního počítače z nahraný virtuální disk [pomocí šablony správce prostředků](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Můžete také rozhraní příkazového řádku zadáním URI na disk (`--image-urn`). Následující příklad vytvoří OM s názvem `myVM` pomocí virtuální disk jste předtím nahráli:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Účet cílového úložiště musí být stejné jako místo, kam jste nahráli virtuální disku. Bude potřeba zadat nebo odpovědí zobrazí výzvu pro všechny další parametry vyžadované `azure vm create` příkazu například virtuální sítě, veřejnou IP adresu, uživatelské jméno a SSH klíče. Další informace o [dostupných parametrů správce prostředků rozhraní příkazového řádku](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Požadavky
Podle těchto kroků, je potřeba k provedení:

- **Operační systém Linux nainstalovaných v souboru VHD** - instalace [potvrzeného Azure Linux rozdělení](virtual-machines-linux-endorsed-distros.md) (nebo nevidím [informace o pro - potvrzeného distribuce](virtual-machines-linux-create-upload-generic.md)) virtuální disk ve formátu virtuální pevný disk. Jak vytvořit virtuální pevný disk a OM existují více nástroje:
    - Nainstalujte a nakonfigurujte [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), přičemž je nutné použít virtuální pevný disk jako formát obrázku. V případě potřeby můžete [Převést obrázek](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.
    - Můžete taky použít pro Hyper-V [na Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Novější formát VHDX není podporován v Azure. Když vytvoříte virtuálního počítače, zadejte virtuální pevný disk jako formát. V případě potřeby můžete převést VHDX disků virtuální pevný disk pomocí [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Kromě toho Azure podporuje nahrávání dynamické VHD, takže budete muset převést takové disků statické VHD před nahráním. Pomocí nástrojů například [Azure virtuální pevný disk nástroje pro přechod](https://github.com/Microsoft/azure-vhd-utils-for-go) k převodu dynamických disků během nahrávání Azure.

- VMs vytvořená z vlastní obrázek musí být umístěny ve stejném účtu úložiště jako samotný obraz
    - Vytvoření účtu úložiště a kontejner pro uložení vaší vlastní obrázek a vytvořené VMs
    - Po vytvoření všechny VMs můžete bezpečně odstranit obrázek

Ujistěte se, jestli máte [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) přihlášení a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Příprava nahrát obrázek

Azure podporuje různých Linux rozdělení (viz [Potvrzeného distribuce](virtual-machines-linux-endorsed-distros.md)). Jak připravit různých Linux distribuce, které jsou podporovány na Azure vás provede v následujících článcích:

- **[Na základě centOS rozdělení](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Červené klobouk Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Se systémem Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Jiné – bez potvrzeného rozdělení](virtual-machines-linux-create-upload-generic.md)**

Zobrazit **[Poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** obecnější tipy na obrázky Linux Příprava Azure.

> [AZURE.NOTE] [Azure platformu SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí pro VMs spuštěný Linux pouze schválené rozdělení se používá s podrobnostmi konfigurace podle "Podporované verze" [Linux na Azure-Endorsed rozdělení](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Skupiny zdrojů logicky spojit všechny Azure zdroje pro podporu virtuálních počítačích, například virtuální sítě a úložiště. Další informace o [skupinách Azure prostředků tady](../azure-resource-manager/resource-group-overview.md). Před nahráním obrázek vlastní disku a vytváření VMs, musíte nejdřív vytvořit skupinu zdrojů. 

Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` umístění:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště
VMs uložená jako objekty BLOB stránky v rámci účtu úložiště. Další informace o [úložišti objektů blob Azure tady](../storage/storage-introduction.md#blob-storage). Vytvoření účtu úložiště pro vaše vlastní bitové a VMs. Všechny VMs vytvářených z disku vlastní obrázek musí být ve stejném účtu úložiště jako tento obrázek.

Následující příklad vytvoří úložiště účet s názvem `mystorageaccount` ve skupině prostředků dřív vytvořili:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Seznam úložiště účtu klíče
Azure vygeneruje dva 512-bit přístupových kláves pro každý účet úložiště. Tyto přístupových kláves používají při ověření účtu úložiště, jako je třeba provádět operace zápisu. Další informace o [správě přístup k základnímu úložišti tady](../storage/storage-create-storage-account.md#manage-your-storage-account). Zobrazení přístupových kláves z verze s `azure storage account keys list` příkaz.

Zobrazení přístupových kláves úložiště účtu, který jste vytvořili:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Výstup je podobný:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Poznamenejte si `key1` jako použije Pokud chcete provést interakci s účtem úložiště v dalším krokům.

## <a name="create-a-storage-container"></a>Vytvoření kontejneru úložiště
Stejným způsobem, kterou vytvoříte různé adresářích logicky uspořádat do místního systému souborů vytvoříte kontejnery v rámci účtu úložiště k uspořádání virtuálních disků a obrázky. Účet úložiště může obsahovat libovolný počet kontejnery. 

Následující příklad vytvoří kontejneru s názvem `myimages`, zadání přístupová klávesa získáte v předchozím kroku (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Nahrání virtuální pevný disk
Teď můžete skutečně nahrát obrázek vlastní disku. Jako u všech virtuální disků používaný VMs, můžete nahrát a uložit vlastní disku obrázek jako objektů blob stránky.

Určení přístupová klávesa, kontejner, který jste vytvořili v předchozím kroku a cestu k obrázku vlastní disk na místním počítači:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Vytvoření OM z vlastní obrázek
Při vytváření VMs z disku vlastní obrázek zadejte identifikátor URI obrázek disku. Zajištění shody účtu cílového úložiště kde je uložený obrázek vlastní disku. Můžete vytvořit svůj OM pomocí rozhraní příkazového řádku Azure nebo správce prostředků JSON šablony.


### <a name="create-a-vm-using-the-azure-cli"></a>Vytvoření OM pomocí rozhraní příkazového řádku Azure
Zadáte `--image-urn` parametr s `azure vm create` příkaz tak, aby ukazovaly na obrázek vlastní disku. Zkontrolujte, že je `--storage-account-name` odpovídá účtu úložiště, kde je uložený obrázek vlastní disku. Není potřeba používat kontejneru stejný jako obrázek vlastní disku pro uložení vaší VMs. Nezapomeňte vytvořit další kontejnery stejným způsobem jako v předchozích krocích před nahráním vlastní disku obrázky.

Následující příklad vytvoří OM s názvem `myVM` z vlastní disku obrázku:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Pořád budete muset zadat, nebo odpovědí zobrazí výzvu pro všechny další parametry vyžadované `azure vm create` příkazu například virtuální sítě, veřejnou IP adresu, uživatelské jméno a SSH klíče. Další informace o [dostupných parametrů správce prostředků rozhraní příkazového řádku](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Vytvoření OM pomocí šablony JSON
Azure šablony správce prostředků jsou JavaScript Object Notation (JSON) soubory, které definují prostředí, které chcete vytvořit. Šablony jsou rozdělené podle různých zdrojů poskytovatelů například výpočetním nebo k síti. Můžete použít existující šablony nebo napsat svůj vlastní. Přečtěte si další informace o [použití Správce zdrojů a šablony](../azure-resource-manager/resource-group-overview.md).

V rámci `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzel, který obsahuje údaje o konfiguraci vašeho OM. Jsou dva hlavní parametry upravit `image` a `vhd` URI, přejděte na obrázek vlastní disku a nové OM virtuální disk. Následujícím obrázku je příklad JSON pro používání vlastní bitové.

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Můžete použít [Toto existující šablony OM z vlastního obrázku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo informace o [vytváření vlastních šablon správce prostředků Azure](../resource-group-authoring-templates.md). 

Až budete mít nakonfigurované šablony, vytvoříte vaší VMs pomocí `azure group deployment create` příkaz. Určení URI JSON šablony pomocí `--template-uri` parametr:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Pokud máte otevřený soubor JSON místně z počítače, můžete `--template-file` parametr místo:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Další kroky
Po připravit a odeslat vlastní virtuální disk, můžete získat další informace o [použití Správce zdrojů a šablony](../azure-resource-manager/resource-group-overview.md). Můžete také přidat [data disk](virtual-machines-linux-add-disk.md) do nové VMs. Pokud máte spuštěné na vaše VMs, které potřebujete pro přístup k, ujistěte se, otevřete [porty a protokoly koncové body](virtual-machines-linux-nsg-quickstart.md).