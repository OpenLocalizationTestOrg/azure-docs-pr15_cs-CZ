<properties
    pageTitle="Nakonfigurovat integraci Azure klíčové trezoru pro systém SQL Server na Azure VMs (Správce zdrojů)"
    description="Zjistěte, jak můžete automatizovat konfigurace systému SQL Server šifrování pro použití s Azure klíč trezoru. Toto téma vysvětluje, jak integrace trezoru klíč Azure pomocí služby SQL Server na virtuálních počítačích vytvořené pomocí Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Nakonfigurovat integraci Azure klíčové trezoru pro systém SQL Server na Azure VMs (Správce zdrojů)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasický](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Základní informace
Existuje více funkce šifrování SQL serveru, jako jsou [průhledné šifrování (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sloupec úrovně šifrování (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx)a [zálohování šifrování](https://msdn.microsoft.com/library/dn449489.aspx). Tyto formuláře šifrování vyžadují, abyste spravovat a ukládat klíče, který používáte pro šifrování. Služby Azure klíč trezoru (AKV) slouží ke zlepšení zabezpečení a správa klávesy v umístění, zabezpečené a snadno dostupné. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje serveru SQL Server pomocí klávesy z trezoru klíč Azure.

Pokud se serverem SQL Server s místním systémem stroje, že jsou [postup, jak lze získat přístup Azure klíč trezoru z počítače místního serveru SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale pro systém SQL Server v Azure VMs, můžete ušetřit čas používáním funkce *Integrace trezoru klíč Azure* .

Při zapnuté funkci tuto funkci, ho automaticky instalace konektoru SQL serveru, nakonfiguruje poskytovatele EKM pro přístup k Azure klíč trezoru a vytvoří pověření, která umožňuje přístup k trezoru. Pokud dívali kroků v tématu dokumentace výše uvedených místního uvidíte, že tato funkce umožňuje automatizovat kroky 2 a 3. Jediné, co by ještě musíte udělat ruční je vytvořit klíčové trezoru a klíče. V ní celý instalaci systému SQL OM automatické. Po dokončení tohoto nastavení této funkce můžete provést příkazů T-SQL s cílem zahájit šifrování databáze nebo zálohy běžným způsobem.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Povolení a konfigurace AKV integrace
Můžete povolit integraci AKV během vytváření nebo ji nakonfigurovat pro existující VMs.

### <a name="new-vms"></a>Nové VMs
Pokud jsou zřizování nový počítač virtuálního serveru SQL Server pomocí Správce prostředků, obsahuje portálu Azure kroku postup povolení integrace Azure klíč trezoru. Azure klíč trezoru funkce je dostupná jenom pro organizace, vývojář a zkušební verze SQL serveru.

![Integrace SQL Azure klíčové trezoru](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Podrobné informace o vytváření najdete v článku [poskytnutí virtuálního počítače serveru SQL Server Azure portálu](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující VMs
Pro existující virtuálních počítačích SQL serveru vyberte virtuálního počítače SQL serveru. Vyberte část zásuvné **Nastavení** **Konfigurace systému SQL Server** .

![Integrace AKV SQL pro existující VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

V zásuvné **Konfigurace systému SQL Server** klikněte na tlačítko **Upravit** v části Integrace automatické trezoru klíče.

![Nakonfigurovat integraci AKV SQL pro existující VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Až budete hotovi, klikněte na tlačítko **OK** v dolní části zásuvné **Konfigurace systému SQL Server** uložte provedené změny.

>[AZURE.NOTE] Můžete taky nakonfigurovat integraci AKV pomocí šablony. Další informace najdete v tématu [Azure rychlý úvod šablony pro integraci Azure klíč trezoru](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
