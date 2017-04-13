<properties
   pageTitle="Vytvoření Linux OM na Azure pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Vytvoření Linux OM na Azure pomocí rozhraní příkazového řádku."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Vytvoření Linux OM na Azure pomocí rozhraní příkazového řádku

Tento článek popisuje, jak rychle nasazení Linux virtuálního počítače (OM) na Azure pomocí `azure vm quick-create` příkazu v Azure rozhraní příkazového řádku (rozhraní příkazového řádku). `quick-create` Příkaz nasadí OM uvnitř základní zabezpečené infrastruktury, můžete pomocí prototypů nebo testování koncept rychle. V článku vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure rozhraní příkazového řádku](../xplat-cli-install.md) se přihlásili `azure login`.

- Správce prostředků Azure režimu rozhraní příkazového řádku Azure _musí být v_ `azure config mode arm`.

Můžete taky rychle nasadíte Linux OM pomocí [Azure portálu](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Rychlé příkazy

Následující příklad ukazuje, jak nasazení OM jádro operačního systému a připojte kód zabezpečené prostředí (SSH) (váš argumenty mohou lišit):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Podrobné informace

Následující návod obsahuje UbuntuLTS OM právě nasazeném, krok za krokem s vysvětlením provedení každého kroku.

## <a name="vm-quick-create-aliases"></a>OM-vytvořit aliasy

Rychlý způsob, jak vybrat rozdělení je použití rozhraní příkazového řádku Azure aliasy namapované nejběžnější rozdělení s operačním systémem. V následující tabulce jsou uvedeny aliases (z Azure rozhraní příkazového řádku verze 0,10). Všechna nasazení, které používají `quick-create` výchozího stavu a VMs, které jsou podporovaným úložiště jednotce (SSD), která nabízí rychlejší zřizovací přístup pomocí protokolu nebo výkonné disku. (Tyto aliasy představuje miniaturní část dostupné rozdělení na Azure. Vyhledat další obrázky v Azure Marketplace [vyhledáte obrázek v prostředí PowerShell](virtual-machines-linux-cli-ps-findimage.md), [na webu](https://azure.microsoft.com/marketplace/virtual-machines/)nebo [Nahrát vlastní obrázek](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Aplikace Publisher | Nabídka        | SKU         | Verze |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | nejnovější  |
| Jádro operačního systému    | Jádro operačního systému    | Jádro operačního systému       | Stabilní      | nejnovější  |
| Debian    | credativ  | Debian       | 8           | nejnovější  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | nejnovější  |
| RHEL      | Červené klobouk    | RHEL         | 7.2         | nejnovější  |
| UbuntuLTS | Kanonický | Se systémem Ubuntu serveru | 14.04.4-LTS | nejnovější  |

Následující oddíly použití `UbuntuLTS` alias pro možnost **ImageURN** (`-Q`) pro nasazení serveru se systémem Ubuntu 14.04.4 l.

Předchozí `quick-create` příklad pouze vyznačenými `-M` příznak k identifikaci veřejným klíčem SSH nahrát při zakázání SSH hesla, aby se zobrazí výzva k následující argumenty:

- Název skupiny prostředků (libovolný řetězec je obvykle pořádku pro první skupina Azure zdroje)
- Název OM
- umístění (`westus` nebo `westeurope` jsou vhodné výchozí nastavení)
- Linux (jestli chcete, aby Azure vědět, které s operačním systémem chcete)
- uživatelské jméno

Následující příklad určuje všechny hodnoty, takže žádné další s dotazem, je potřeba. To budete mít `~/.ssh/id_rsa.pub` jako ssh-rsa veřejné klíčové soubor ve formátu, funguje stejně jako:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Výstup by měl vypadat následujícím výstupního bloku:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Přihlaste se k nové OM

Přihlaste se k vaší OM pomocí veřejnou IP adresu podle výstupu. Můžete taky použít plně kvalifikovaný název domény (FQDN), které je uvedeno:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Proces přihlášení by měl vypadat nějak následující blok výstup:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Další kroky

`azure vm quick-create` Příkaz je způsob, jak rychle nasadit OM, přihlaste se k prostředí flám a Začínáme pracovat. Však pomocí `vm quick-create` nezískáváte rozsáhlou kontrolu ani znamená umožňují vytvářet složitější prostředí.  Abyste mohli nasadit Linux OM, která používá vlastní nastavení pro infrastrukturu, můžete použít některou z těchto článků:

- [Použití šablony pro správce prostředků Azure k vytvoření určitého nasazení](virtual-machines-linux-cli-deploy-templates.md)
- [Vytvoření vlastní prostředí pro Linux OM přímo pomocí příkazů rozhraní příkazového řádku Azure](virtual-machines-linux-create-cli-complete.md)
- [Vytvoření SSH zabezpečená Linux OM na Azure pomocí šablony](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Můžete taky [použít `docker-machine` Azure ovladač s různými příkazy a rychle vytvořte Linux OM jako hostitel docker](virtual-machines-linux-docker-machine.md).
