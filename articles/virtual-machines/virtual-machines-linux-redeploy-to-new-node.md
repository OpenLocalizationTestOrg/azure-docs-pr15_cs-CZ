<properties 
    pageTitle="Přeinstalujte Linux virtuálních počítačích | Microsoft Azure" 
    description="Popisuje, jak přeinstalujte Linux virtuálních počítačích zmírnit problémy s připojením SSH." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Přeinstalujte virtuální počítač nový Azure uzel

Pokud máte byla protilehlé potíže poradce při potížích SSH nebo aplikací k Azure virtuálního počítače (OM), znovu nasazení OM vám mohou pomoci. Když přeinstalujte virtuálního počítače slouží k přesunutí OM nový uzel v rámci Azure infrastruktury a zajišťuje ho znovu zapněte možnosti konfigurace a přidružené prostředky. Tento článek ukazuje, jak přeinstalujte OM pomocí rozhraní příkazového řádku Azure nebo portálu Azure.

> [AZURE.NOTE] Po přeinstalujte virtuálního počítače dočasné disku budou ztracena a dynamické IP adresy přidružené k rozhraní virtuální sítě se automaticky aktualizují. 


## <a name="using-azure-cli"></a>Použití Azure rozhraní příkazového řádku

Zkontrolujte, jestli máte [Nejnovější rozhraní příkazového řádku Azure, které jsou nainstalované](../xplat-cli-install.md) v počítači a jste v režimu správce prostředků (`azure config mode arm`).

Pomocí následujícího příkazu rozhraní příkazového řádku Azure přeinstalujte virtuálního počítače:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Stav změnit OM můžete zobrazit, jak prochází procesu opětovném nasazení. `PowerState` Z OM, půjde "Spustila" aktualizace, potom spusťte a nakonec "spuštěné" jako prochází proces opětovného nasazení nového hostitele. Kontrola stavu VMs v rámci skupiny zdroje s:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením k vaší OM, můžete najít nápovědu pro [potíže s připojením k SSH](virtual-machines-linux-troubleshoot-ssh-connection.md) nebo [SSH podrobné návody na řešení potíží](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Pokud nemáte přístup ke spuštění aplikace v vaší OM, si můžete přečíst [řešíte potíže s aplikací](virtual-machines-linux-troubleshoot-app-connection.md).