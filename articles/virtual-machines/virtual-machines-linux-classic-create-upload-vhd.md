<properties
    pageTitle="Vytvořte a uložte virtuálního pevného disku Linux | Microsoft Azure"
    description="Vytvořte a uložte Azure virtuální pevný disk (virtuální pevný disk) s klasické nasazení modelu, který obsahuje operační systém Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Vytváření a nahrávání virtuální pevný Disk, která obsahuje Linux operační systém

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Můžete také [Nahrát obrázek vlastní disku pomocí Správce prostředků Azure](virtual-machines-linux-upload-vhd.md).

V tomto článku se dozvíte, jak vytvořit a nahrát virtuální pevný disk (virtuální pevný disk), můžete ho jako vlastní obrázek vytváření virtuálních počítačích v Azure. Naučte se připravit operační systém, abyste ho mohli použít k vytvoření více virtuálních počítačích založené na tomto obrázku. 

>  [AZURE.NOTE] Pokud máte chvíli, Pomozte nám zlepšit dokumentace OM Linux Azure pomocí tohoto [rychlého průzkumu](https://aka.ms/linuxdocsurvey) vašeho prostředí. Každý odpovědí pomáhá pomáhají vaši práci.

## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte následující položky:

- **Operační systém Linux nainstalovaných v souboru VHD** - jste nainstalovali [potvrzeného Azure Linux rozdělení](virtual-machines-linux-endorsed-distros.md) (nebo v tématu [informace o - potvrzeného distribuce](virtual-machines-linux-create-upload-generic.md)) virtuální disk ve formátu virtuální pevný disk. Jak vytvořit virtuální pevný disk a OM existují více nástroje:
    - Nainstalujte a nakonfigurujte [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), přičemž je nutné použít virtuální pevný disk jako formát obrázku. V případě potřeby můžete [Převést obrázek](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.
    - Můžete taky použít pro Hyper-V [na Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Novější formát VHDX není podporován v Azure. Když vytvoříte virtuálního počítače, zadejte virtuální pevný disk jako formát. V případě potřeby můžete převést VHDX disků virtuální pevný disk pomocí [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Kromě toho Azure podporuje nahrávání dynamické VHD, takže budete muset převést takové disků statické VHD před nahráním. Pomocí nástrojů například [Azure virtuální pevný disk nástroje pro přechod](https://github.com/Microsoft/azure-vhd-utils-for-go) k převodu dynamických disků během nahrávání Azure.

- **Rozhraní příkazového řádku azure** - nainstalovat nejnovější [rozhraní příkazového řádku Azure](../virtual-machines-command-line-tools.md) nahrát virtuální pevný disk.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Krok 1: Příprava nahrát obrázek

Azure podporuje různých Linux rozdělení (viz [Potvrzeného distribuce](virtual-machines-linux-endorsed-distros.md)). V následujících článcích vás jak připravit různých Linux distribuce, které jsou podporovány na Azure. Po dokončení kroků v následující vodítka zpět tohoto prostoru až budete mít soubor virtuálního pevného disku, který je připravená k odeslání do Azure:

- **[Na základě centOS rozdělení](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Červené klobouk Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Se systémem Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Jiné – bez potvrzeného rozdělení](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Azure platformu SLA platí pro virtuálních počítačích Linux operační systém jenom při použití jednoho z schválené rozdělení se podrobnosti o konfiguraci podle "Podporované verze" [Linux na Azure-Endorsed rozdělení](virtual-machines-linux-endorsed-distros.md). Všechny rozdělení Linux v galerii Azure obrázek jsou schválené rozdělení se požadovaná konfigurace.

Zobrazit **[Poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** obecnější tipy na obrázky Linux Příprava Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Krok 2: Příprava připojení k Azure

Ujistěte se, používáte Azure rozhraní příkazového řádku v modelu klasické nasazení (`azure config mode asm`), pak se přihlaste ke svému účtu:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Krok 3: Odeslání obrázku do Azure

Je třeba úložiště účtu, a nahrajte soubor do virtuálního pevného disku. Buď můžete vybrat existující úložiště klienta nebo [vytvořte nový účet](../storage/storage-create-storage-account.md).

K nahrání obrázek pomocí následujícího příkazu použijte Azure rozhraní příkazového řádku:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

V předchozím příkladu:

- **BlobStorageURL** je adresa URL úložiště účet, který budete používat
- **YourImagesFolder** je kontejner v úložišti objektů blob místo, kam chcete ukládat obrázky
- **VHDName** je popisek, který se zobrazí v portálu pro identifikaci virtuální pevný disk.
- **PathToVHDFile** je úplnou cestu a název souboru VHD ve vašem počítači.

Na následujícím obrázku je příklad dokončeno:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Krok 4: Vytvoření virtuálního počítače z obrázku
Vytvoření OM pomocí `azure vm create` stejným způsobem jako běžný OM. Zadejte název udělili obrázek v předchozím kroku. V následujícím příkladu používáme **UbuntuLTS** obrázek názvu v předchozím kroku:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Pokud chcete vytvořit vlastní VMs, zadejte vlastní uživatelské jméno + heslo, umístění, DNS jméno a název obrázku.

## <a name="next-steps"></a>Další kroky

Další informace najdete v článku Principy [Azure rozhraní příkazového řádku pro modelu Azure klasické nasazení](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
