<properties
    pageTitle="Poradce při potížích připojení SSH do virtuálního počítače | Microsoft Azure"
    description="Řešení problémů s například 'SSH připojení se nepovedlo"nebo" SSH odmítnuto ' pro Azure OM systémem Linux."
    keywords="SSH připojení odmítnuto, ssh chyba, azure ssh, SSH připojení se nezdařila."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Poradce při potížích s SSH připojení k OM Linux Azure, nepovede, chyby, nebo odmítnutí
Existuje několik různých důvodů, že dojde k chybě zabezpečené prostředí (SSH), chyby připojení SSH nebo SSH odmítnuta při pokusu o připojení k počítači virtuální Linux (OM). Tento článek vám pomůže najít a opravit problémy. Pomocí portálu Azure Azure rozhraní příkazového řádku, nebo číslo linky přístup OM Linux s řešeními problémů a řešení problémů s připojením.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [webu MSDN Azure a přetečení zásobníku fóra](http://azure.microsoft.com/support/forums/). Můžete taky můžete poslat případ Azure podpory. Přejděte na [Web podpory Azure](http://azure.microsoft.com/support/options/) a vyberte, **využijte možnost získání podpory**. Informace o používání podporují Azure přečtěte si [Microsoft Azure podporují časté otázky](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Rychlé kroky
Po každém kroku Poradce při potížích zkuste se znovu připojit angličtině.

1. Obnovení konfigurace SSH.
2. Obnovení pověření pro uživatele.
3. Ověřte, zda že pravidla [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) povolení SSH komunikace.
    - Pro povolení komunikace SSH (ve výchozím nastavení TCP port 22) musí existovat pravidlo skupiny zabezpečení sítě.
    - Nelze použít přesměrování portu / mapování bez použití vyrovnávání zatížení Azure.
4. Kontrola [stavu OM zdroje](../resource-health/resource-health-overview.md). 
    - Ujistěte se, že OM sestavy jako správný.
    - Pokud máte spouštěcí diagnostiky povoleno, zkontrolujte že OM není zpráv o chybách spouštění v protokolech.
5. Restartujte OM.
6. Přeinstalujte OM.

Pokračujte ve čtení pro podrobnější řešení potíží a vysvětlivky.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>K dispozici metody řešit problémy s připojením SSH

Můžete obnovit přihlašovací údaje nebo SSH konfigurace pomocí jedné z těchto způsobů:

- [Azure portál](#using-the-azure-portal) - skvělé, když potřebujete rychle obnovit konfigurace SSH nebo SSH klíče a nemáte nainstalované Azure nástroje.
- [Příkazy rozhraní příkazového řádku azure](#using-the-azure-cli) – Pokud jste již do příkazového řádku rychle obnovit konfigurace SSH nebo přihlašovací údaje.
- [Rozšíření azure VMAccessForLinux](#using-the-vmaccess-extension) – vytvoření a opětovné použití json definicemi resetovat SSH uživatele nebo konfigurace přihlašovacích údajů.

Po každém kroku Poradce při potížích připojte se k vaší OM znovu. Pokud se stále nemůžete připojit, zkuste dalším krokem.


## <a name="using-the-azure-portal"></a>Pomocí portálu Azure
Portál Azure obsahuje rychlý způsob, jak obnovit přihlašovací údaje pro uživatele nebo konfigurace SSH bez instalace nástrojů ve vašem počítači.

Vyberte svůj OM Azure portálu. Přejděte dolů do části **Podpora + Poradce při potížích** a vyberte **resetování hesla** jako v následujícím příkladu:

![Konfigurace SSH resetovat nebo přihlašovací údaje na portálu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Obnovení konfigurace SSH
Jako první krok, vyberte `Reset SSH configuration only` z rozevírací nabídky **režim** jako předchozí snímek klikněte na tlačítko **Obnovit** . Po dokončení tuto akci pokusili vaší OM znovu.

### <a name="reset-ssh-credentials-for-a-user"></a>Obnovení SSH pro ni přihlašovací údaje uživatele
Obnovení pověření stávajícímu uživateli, na výběr tyhle možnosti `Reset SSH public key` nebo `Reset password` z rozevírací nabídky **režimu** jako předchozí snímek. Zadejte uživatelské jméno a SSH klíč nebo nové heslo a pak klikněte na tlačítko **Obnovit** .

Můžete taky vytvořit uživatele s oprávněními sudo OM z této nabídky. Zadejte nové uživatelské jméno a heslo přidružené nebo klávesa SSH a potom klikněte na tlačítko **Obnovit** .


## <a name="using-the-azure-cli"></a>Použití Azure rozhraní příkazového řádku
Pokud jste to ještě neudělali, [nainstalujte Azure rozhraní příkazového řádku a připojte k předplatnému Azure](../xplat-cli-install.md). Nezapomeňte pomocí Správce prostředků režimu takto:

```
azure config mode arm
```

Pokud jste vytvořili a nahrané vlastní Linux disku obrázku, ujistěte se, [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) verze 2.0.5 nebo novější je. Pro VMs vytvořené pomocí Galerie obrázků je tento přístup rozšíření již nainstalovali a nakonfigurovali za vás.

### <a name="reset-ssh-configuration"></a>Obnovení SSH konfigurace
Konfigurace SSHD samotné může nesprávné nebo ve službě došlo k chybě. Můžete obnovit SSHD aby zkontrolovala, jestli že platí konfigurace SSH samotné. Obnovení SSHD by měl být prvním krokem ukládaly.

Následující příklad obnoví SSHD na OM s názvem `myVM` ve skupině zdroje s názvem `myResourceGroup`. Použijte vlastní názvy skupin OM a zdroje následujícím způsobem:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Obnovení SSH pro ni přihlašovací údaje uživatele
Pokud se zobrazí SSHD budou fungovat správně, je resetování hesla pro uživatele třetím osobám. Následující příklad obnoví pro ni přihlašovací údaje `myUsername` hodnotu zadanou v `myPassword`, na OM s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Pokud ověřování SSH klíčů, můžete obnovit SSH klíč pro daného uživatele. Následující příklad aktualizuje klávesu SSH uložené v `~/.ssh/azure_id_rsa.pub` pro uživatele s názvem `myUsername`, na OM s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>S příponou VMAccess
Rozšíření Access OM Linux čte ve formátu json soubor, který definuje akce provádět. Tyto akce zahrnutí obnovením SSHD, obnovení SSH klíče a přidáte uživatele. Dál používat rozhraní příkazového řádku Azure volání koncovku VMAccess, ale můžete znovu použít soubory json napříč několika VMs podle potřeby. Tento přístup umožňuje vytvářet úložiště json soubory, které lze klepněte volat pro danou scénáře.

### <a name="reset-sshd"></a>Obnovení SSHD
Vytvoření souboru s názvem `PrivateConf.json` následující obsahu:

```bash
{  
    "reset_ssh":"True"
}
```

Pomocí rozhraní příkazového řádku Azure, potom zavolejte `VMAccessForLinux` rozšíření obnovit připojení SSHD zadáním souboru json. Následující příklad obnoví SSHD na OM s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Obnovení SSH pro ni přihlašovací údaje uživatele
Pokud se zobrazí SSHD budou fungovat správně, můžete obnovit přihlašovací údaje pro uživatele třetím osobám. K resetování hesla pro uživatele, vytvořte do souboru nazvaného `PrivateConf.json`. Následující příklad obnoví pro ni přihlašovací údaje `myUsername` hodnotu zadanou v `myPassword`. Zadejte následující řádky do svého `PrivateConf.json` souboru, vlastními hodnotami:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Nebo pokud chcete obnovit SSH klíč pro uživatele, nejprve vytvořit do souboru nazvaného `PrivateConf.json`. Následující příklad obnoví pro ni přihlašovací údaje `myUsername` hodnotu zadanou v `myPassword`, na OM s názvem `myVM` v `myResourceGroup`. Zadejte následující řádky do svého `PrivateConf.json` souboru, vlastními hodnotami:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Po vytvoření souboru json, použijte Azure rozhraní příkazového řádku pro volání `VMAccessForLinux` rozšíření obnovení pověření SSH uživatele zadáním souboru json. Následující příklad obnoví přihlašovací údaje na OM s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Restartujte virtuálního počítače
Pokud máte obnovit přihlašovací údaje pro konfiguraci a uživatel SSH nebo došlo k chybě při tom, můžete zkusit restartování OM adresu základní výpočetním problémy.

### <a name="azure-portal"></a>Azure portálu
Restartujte OM pomocí portálu Azure, vyberte svůj OM a klikněte na ***Restartujte** tlačítko jako v následujícím příkladu:

![Restartujte virtuálního počítače na portálu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku
Následující příklad restartuje OM s názvem `myVM` ve skupině zdroje s názvem `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Přeinstalujte virtuálního počítače
Můžete nasadit virtuálního počítače do jiného uzlu v Azure, která může opravit problémy související sociální sítě. Informace o opětovné nasazení virtuálního počítače najdete v tématu [přeinstalujte virtuální počítač nový Azure uzel](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Po dokončení operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidružené k virtuální počítač bude aktualizován.

### <a name="azure-portal"></a>Azure portálu
Pokud chcete přeinstalujte OM pomocí portálu Azure, vyberte OM a posuňte se dolů do části **Podpora + Poradce při potížích** . Klikněte na tlačítko **přeinstalujte** jako v následujícím příkladu:

![Přeinstalujte OM na portálu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku
Následující příklad znovu nasadí OM s názvem `myVM` ve skupině zdroje s názvem `myResourceGroup`. Použijte vlastní hodnoty následujícím způsobem:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs vytvořený s využitím nasazení modelu Klasický

Vyzkoušejte tento postup řešením nejčastější chyby připojení SSH pro VMs, které jsou vytvořené pomocí klasické nasazení modelu. Po každém kroku zkuste se znovu připojit angličtině.

- Obnovte vzdálený přístup z [Azure portálu](https://portal.azure.com). Na portálu Azure vyberte svůj OM a klikněte na tlačítko **Obnovit vzdálené …** .

- Restartujte OM. Na [portálu Azure](https://portal.azure.com)vyberte svůj OM a klikněte na tlačítko **Restartovat** .

    – NEBO –

    Na [Azure klasické portál](https://manage.windowsazure.com), vyberte **virtuálních počítačích** > **instance** > **Restartovat**.

- Přeinstalujte OM nový Azure uzel. Informace o tom, jak přeinstalujte virtuálního počítače najdete v tématu [přeinstalujte virtuální počítač nový Azure uzel](virtual-machines-windows-redeploy-to-new-node.md).

    Po dokončení operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidružené k virtuální počítač bude aktualizován.

- Postupujte podle pokynů v článku [Jak obnovit heslo nebo SSH na základě Linux virtuálních počítačích](virtual-machines-linux-classic-reset-access.md) :
    - Resetování hesla nebo klávesa SSH.
    - Vytvoření uživatelského účtu _sudo_ .
    - Obnovení konfigurace SSH.

- Kontrola stavu zdroje OM naleznete některé problémy s platformu.<br>
  Vyberte OM a posuňte se dolů **Nastavení** > **Kontrola stavu**.


## <a name="additional-resources"></a>Další zdroje informací

- Pokud jste pořád nemůžete soubory SSH vaší v angličtině po po kroků, najdete v článku [podrobné návody na řešení potíží](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) zobrazíte další kroky pro problém vyřešit.

- Další informace o řešení potíží s aplikací access najdete v tématu [Poradce při potížích s přístupu k spuštění aplikace v Azure virtuálního počítače](virtual-machines-linux-troubleshoot-app-connection.md)

- Další informace o řešení potíží s virtuálních počítačích, které jsou vytvořené pomocí modelu klasické nasazení najdete v článku [Jak obnovit heslo nebo SSH na základě Linux virtuálních počítačích](virtual-machines-linux-classic-reset-access.md).
