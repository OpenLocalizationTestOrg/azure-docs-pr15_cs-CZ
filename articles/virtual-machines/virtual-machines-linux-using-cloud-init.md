<properties
    pageTitle="Přizpůsobení Linux OM při vytváření pomocí cloudu inicializace | Microsoft Azure"
    description="Přizpůsobení Linux OM při vytváření pomocí inicializace cloudu."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Přizpůsobení Linux OM při vytváření pomocí inicializace cloudu

Tento článek popisuje jak vytvořit skript cloudu inicializace se dají hostname, balíčků nainstalovanou aktualizací a spravovat uživatelské účty.  Inicializace cloudu skripty se označují jako při vytváření OM z Azure rozhraní příkazového řádku.  V článku vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure rozhraní příkazového řádku](../xplat-cli-install.md) se přihlásili `azure login`.

- Správce prostředků Azure režimu rozhraní příkazového řádku Azure _musí být v_ `azure config mode arm`.

## <a name="quick-commands"></a>Rychlé příkazy

Vytvořte cloudu init.txt skript, který nastaví název hostitele, aktualizuje všechny balíčků a přidá uživatele sudo Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Vytvoření skupiny zdrojů a spuštění VMs do.

```bash
azure group create myResourceGroup westus
```

Vytvoření Linux OM pomocí inicializace cloudu ji nakonfigurovat během spuštění.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Podrobné informace

### <a name="introduction"></a>Úvod

Při spuštění nové OM Linux se zobrazují standardní Linux OM není nic uvedeno přizpůsobený nebo není připraveno k vašim potřebám. [Cloud inicializace](https://cloudinit.readthedocs.org) je standardní způsob, jak při spuštění pro první vloží skript nebo konfigurace nastavení do této OM Linux.

Na Azure, existují tři různé způsoby měnit na Linux OM, jako je používaný nebo spuštění.

- Vloží skriptů v cloudu – spuštění.
- Vloží skriptů pomocí Azure [VMAccess rozšíření](virtual-machines-linux-using-vmaccess-extension.md).
- Šablony aplikace Azure pomocí inicializace cloudu.
- Šablony aplikace Azure pomocí [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Chcete-li vloží skripty kdykoli po spuštění:

- SSH přímo spustit příkazy
- Vloží skriptů pomocí Azure [VMAccess rozšíření](virtual-machines-linux-using-vmaccess-extension.md)imperativně nebo v šabloně aplikace Azure
- Nástroje pro správu konfigurace jako Ansible, sůl, Chef a Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Inicializace cloudu dostupnost na Azure OM-vytvořit obrázek aliasy:

| Alias     | Aplikace Publisher | Nabídka        | SKU         | Verze | Inicializace cloudu |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | nejnovější  | Ne         |
| Jádro operačního systému    | Jádro operačního systému    | Jádro operačního systému       | Stabilní      | nejnovější  | Ano        |
| Debian    | credativ  | Debian       | 8           | nejnovější  | Ne         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | nejnovější  | Ne         |
| RHEL      | RedHat    | RHEL         | 7.2         | nejnovější  | Ne         |
| UbuntuLTS | Kanonický | UbuntuServer | 14.04.4-LTS | nejnovější  | Ano        |

Microsoft pracuje s partneři get cloud inicializace nezahrnou a práci s obrázky, které poskytují Azure.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Přidání skriptu cloudu inicializace k vytvoření OM pomocí rozhraní příkazového řádku Azure

Spuštění skript cloudu inicializace při vytváření virtuálního počítače v Azure zadejte název souboru inicializace cloudu pomocí rozhraní příkazového řádku Azure `--custom-data` přepnout.

Vytvoření skupiny zdrojů a spuštění VMs do.

```bash
azure group create myResourceGroup westus
```

Vytvoření Linux OM pomocí inicializace cloudu ji nakonfigurovat během spuštění.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Vytváření skriptu cloudu inicializace hostname Linux OM

Nejjednodušší a nejdůležitější nastavení pro všechny Linux OM bude název hostitele. Jsme můžete snadno nastavte inicializace cloudu pomocí tohoto skriptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Příklad cloudu inicializace skript s názvem `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Během počáteční spouštění OM tento skript cloudu inicializace nastaví název hostitele `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Přihlášení a ověřte hostname nové OM.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Vytváření skriptu cloudu inicializace aktualizovat Linux

Zabezpečení chcete, aby se systémem Ubuntu OM aktualizace při prvním spuštění.  Použití cloudu inicializace, kterou jsme to udělat pomocí skriptu sledovat, v závislosti na používaném rozdělení Linux.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Příklad mraků inicializace skriptu `cloud_config_apt_upgrade.txt` Debian řady

```bash
#cloud-config
apt_upgrade: true
```

Po spuštění Linux obsahuje všechny nainstalované balíčky jsou aktualizovány pomocí `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Přihlášení a ověřte, budou aktualizovány všechny balíčků.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Vytváření mraků inicializace skriptu pro přidání uživatele do Linux

Jednou z první kroky na všechny nové OM Linux je přidání uživatele pro sebe nebo eliminování možnosti použití `root`. SSH klíče jsou nejvhodnější pro zabezpečení a pro zvýšení použitelnosti a se přidají do `~/.ssh/authorized_keys` soubor pomocí tohoto skriptu inicializace cloudu.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Příklad cloudu inicializace skriptu `cloud_config_add_users.txt` Debian řady

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Po spuštění Linux obsahuje všechny uvedené uživatele vytvořené a přidané do skupiny sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Přihlášení a ověřte nově vytvořený uživatele.

```bash
ssh myVM
cat /etc/group
```

Výstup

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Další kroky

Inicializace cloudu se standardní způsobů upravit Linux OM při spuštění. Azure má i rozšíření OM, které umožňují úpravy vaší LinuxVM při spuštění nebo je spuštěná. Můžete třeba Azure VMAccessExtension resetovat informace SSH nebo uživateli, je spuštěná OM. S cloudu inicializace které jsou potřeba restartování počítače, aby vám resetoval heslo.

[O funkcích a rozšíření virtuálního počítače](virtual-machines-linux-extensions-features.md)

[Správa uživatelů, SSH a zaškrtněte nebo oprava disků na Azure Linux VMs s příponou VMAccess](virtual-machines-linux-using-vmaccess-extension.md)
