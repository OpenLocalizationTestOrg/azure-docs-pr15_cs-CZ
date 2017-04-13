<properties
    pageTitle="Obnovení přístupu v Azure Linux VMs s příponou VMAccess | Microsoft Azure"
    description="Obnovení přístupu v Azure Linux VMs s příponou VMAccess."
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
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Správa uživatelů, SSH a zaškrtněte nebo oprava disků na Azure Linux VMs s příponou VMAccess

Tento článek popisuje, jak používat rozšíření VMAcesss Azure zkontrolovat nebo opravě disku, resetování uživatelského přístupu, spravovat uživatelské účty a obnovení konfigurace SSHD na Linux. V článku vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure rozhraní příkazového řádku](../xplat-cli-install.md) se přihlásili `azure login`.

- Správce prostředků Azure režimu rozhraní příkazového řádku Azure _musí být v_ `azure config mode arm`.

## <a name="quick-commands"></a>Rychlé příkazy

Používání VMAccess na Linux VMs dvěma způsoby:

- Použití rozhraní příkazového řádku Azure a požadované parametry.
- Použití nezpracovanými JSON soubory, které VMAccess zpracuje a potom chovalo na.

Příkaz Rychlé části budeme používat rozhraní příkazového řádku Azure `azure vm reset-access` metody. V následujících příkladech příkaz nahraďte hodnoty, které obsahují "Příklad" s hodnotami z vlastní prostředí.

## <a name="create-a-resource-group-and-linux-vm"></a>Vytvořte pole Skupina zdroje a Linux OM

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Vytvoření Debian OM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Resetování hesla kořenové

Aby vám resetoval heslo kořenové:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>Klíčové obnovit SSH

Chcete-li obnovit klávesu SSH jiné kořenové uživatele:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Vytvoření uživatele

Vytvoření uživatele:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Odebrání uživatele

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Obnovení SSHD

Chcete-li obnovit SSHD konfigurace:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Podrobné informace

### <a name="vmaccess-defined"></a>VMAccess definované:

Disk na Linux OM je zobrazený chyby. Nějak resetování hesla kořenové Linux OM nebo omylem odstranili privátním klíčem SSH. V takovém případě zpět ve dnech datacentra by potřebujete řídit tam a pak otevřete KVM získat z konzoly serveru. Rozšíření Azure VMAccess Považujte za tento KVM přepínač, který vám umožní přístup k konzole obnovte přístup Linux nebo úrovně údržbě disku.

Podrobné informace budeme dlouhé formulář VMAccess, pracujícího s nezpracovanými JSON soubory.  Tyto soubory VMAccess JSON můžete zkratka z Azure šablon.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Použití VMAccess zaškrtnout nebo opravě disku Linux OM

Použití VMAccess můžete dělat fsck spusťte na disku, klikněte v části OM Linux.  Můžete taky udělat kontrola disku a opravy disku pomocí VMAccess.

Zkontrolujte a opravte disku můžete tento skript VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Použití VMAccess obnovíte Linux přístupu uživatelů

Pokud jste ztratili přístup ke kořenové na Linux OM, můžete spustit skript VMAccess o resetování hesla kořenové.

Aby vám resetoval heslo kořenové, použijte tento skript VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Resetovat klávesu SSH uživatele jiné kořenové, použijte tento skript VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Správa uživatelských účtů na Linux pomocí VMAccess

VMAccess je Python skript, který slouží ke správě uživatelů na Linux OM bez přihlášení a práce s nimi sudo nebo účtu root.

Vytvoření uživatele, použijte tento skript VMAccess:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Chcete-li odstraněním uživatelského účtu, použijte tento skript VMAccess:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Vytvoření nového konfigurace SSHD pomocí VMAccess

Pokud konfiguraci Linux VMs SSHD měnit a zavřete připojení SSH před ověření změn, může být zakázaný ze SSH'ing zpět.  VMAccess lze obnovit konfigurace SSHD známé správné konfigurace bez myši SSH přihlášení.

Chcete-li obnovit konfigurace SSHD pomocí tohoto skriptu VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Spuštění VMAccess skript:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Další kroky

Aktualizace Linux pomocí Azure VMAccess rozšíření je jedním ze způsobů k provádění změn na pracovního OM Linux.  Pomocí nástrojů jako inicializace cloudu a Azure šablony můžete taky změnit Linux OM při spuštění.

[O funkcích a rozšíření virtuálního počítače](virtual-machines-linux-extensions-features.md)

[Vytváření šablon správce prostředků Azure s příponami Linux OM](virtual-machines-linux-extensions-authoring-templates.md)

[Přizpůsobení Linux OM při vytváření pomocí inicializace cloudu](virtual-machines-linux-using-cloud-init.md)
