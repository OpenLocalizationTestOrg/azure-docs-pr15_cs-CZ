<properties
    pageTitle="Připojení k SQL serveru virtuálního počítače (Správce zdrojů) | Microsoft Azure"
    description="Zjistěte, jak se připojit k serveru SQL Server spuštěné v počítači virtuální v Azure. Toto téma používá modelu klasické nasazení. Scénáře lišit v závislosti na konfiguraci sítě a umístění klienta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Připojení k SQL serveru virtuálního počítače na Azure (Správce zdrojů)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-connect.md)
- [Klasický](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Základní informace

Toto téma popisuje, jak se připojit k instanci serveru SQL Azure virtuální počítač se systémem. Popisuje některé [scénáře obecné připojení](#connection-scenarios) a potom obsahuje [Podrobný postup konfigurace systému SQL Server konektivitu OM Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu. Klasický verzi tohoto článku zobrazíte najdete v článku [připojení k SQL serveru virtuálního počítače na klasické Azure](virtual-machines-windows-classic-sql-connect.md).

Pokud se dají přednost úplné walk-through zřizování a připojení, přečtěte si článek [zřizování SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scénáře připojení

Způsob, jakým klienta spojí se serverem SQL virtuálního počítače se systémem se liší v závislosti na umístění klienta a konfiguraci počítače a sítě. Podobnému sledu patří:

- [Připojení k serveru SQL Server přes internet](#connect-to-sql-server-over-the-internet)
- [Připojení k serveru SQL Server ve stejné síti virtuální](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Připojení k serveru SQL Server přes Internet

Pokud chcete připojit k serveru SQL Server databázový stroj z Internetu, existuje několik kroků potřebných, například konfigurace brány firewall, ověřování serveru SQL povolení a konfigurace sítě skupiny zabezpečení máte pravidlo skupiny zabezpečení sítě přenosy protokolu TCP na port 1433.

Pokud používáte portálu zřízení obrázek virtuálního počítače SQL serveru se správcem zdrojů, takto dělají můžete po výběru **veřejné** možnost připojení k SQL:

![Veřejné připojení možnost SQL během vytváření](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Jestli to není při vytváření, pak můžete ručně nakonfigurovat SQL serveru a virtuálních počítačích pomocí následujících [kroků v tomto článku ruční konfigurace připojení](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Obrázek virtuálního počítače pro SQL Server Express edition neumožňuje automaticky protokol TCP/IP. Pro edici Express musíte pomocí Správce konfigurace systému SQL Server [protokol TCP/IP můžete ručně povolit](#configure-sql-server-to-listen-on-the-tcp-protocol) po vytvoření OM.

Po dokončení, můžete všechny klient s přístupem k Internetu zadáním veřejnou IP adresu virtuální počítač nebo na popisek DNS přiřazená tuto IP adresu připojit k instanci systému SQL Server. Pokud port serveru SQL Server je 1433, nepotřebujete zadat v připojovacím řetězci.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

I když připojení pro klienty umožňuje přes internet, to neznamená kdokoliv můžete připojit k serveru SQL. Mimo musí klientů správné uživatelské jméno a heslo. Pro větší zabezpečení můžete vyhnout známý port 1433. Například pokud konfigurace systému SQL Server poslouchat port 1 500 a zřídit správné brány firewall a pravidla skupiny zabezpečení sítě, můžete se připojit přidáním číslo portu název serveru jako v následujícím příkladu:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Je důležité si všimněte si, že použijete tento postup komunikovat s SQL Server, všechny odchozí dat z Azure datacentra podléhá normální [ceny převodů výstupní data](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Připojení k serveru SQL Server ve stejné síti virtuální

[Virtuální sítě](../virtual-network/virtual-networks-overview.md) umožňuje další scénáře. Můžete se připojit VMs ve stejné síti virtuální, i když tyto VMs existovat v jiné skupiny prostředků. A s [VPN k webu](../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architektura, ke kterému je připojen VMs s místní sítě a počítačů.

Virtuální sítí umožňuje připojit Azure VMs k doméně. To je jediný způsob, jak pomocí ověřování Windows na serveru SQL Server. Ostatních případech připojení vyžadují ověřování serveru SQL pomocí uživatelského jména a hesla.

Pokud používáte portálu zřízení obrázek virtuálního počítače SQL serveru se správcem zdrojů, jsou pravidla správné brány firewall pro komunikaci virtuální síti se systémem instalační program, když vyberete **soukromé** možnost připojení k SQL. Jestli to není při vytváření, pak můžete ručně nakonfigurovat SQL serveru a virtuálních počítačích pomocí následujících [kroků v tomto článku ruční konfigurace připojení](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Ale pokud se chystáte konfigurace doménovém prostředí a ověřování systému Windows, nemusíte Pokud chcete konfigurovat ověřování serveru SQL a přihlášení postupujte podle pokynů v tomto článku. Můžete také nemusí ke konfiguraci sítě skupiny zabezpečení pravidla pro přístup přes internet.

>[AZURE.NOTE] Obrázek virtuálního počítače pro SQL Server Express edition neumožňuje automaticky protokol TCP/IP. Pro edici Express musíte pomocí Správce konfigurace systému SQL Server [protokol TCP/IP můžete ručně povolit](#configure-sql-server-to-listen-on-the-tcp-protocol) po vytvoření OM.

Za předpokladu, že jste nakonfigurovali DNS v síti virtuální, můžete tím, že zadáte název počítače SQL Server OM v připojovacím řetězci připojit k instanci systému SQL Server. Následující příklad také předpokládá ověřování systému Windows taky nakonfigurovaná a že uživatel má udělený přístup k instanci systému SQL Server.

    "Server=mysqlvm;Integrated Security=true"

Všimněte si, že v tomto případě lze zadat IP adresu OM.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Postup ruční konfigurace připojení k SQL serveru v Azure OM

Následující postup ukazuje, jak lze ručně nastavit připojení k instanci systému SQL Server a volitelně připojte se přes internet pomocí služby SQL Server Management Studio (SSMS). Všimněte si, že mnoho z těchto kroků se dělají můžete po výběru požadovaných možností připojení k serveru SQL Server na portálu.

Abyste se mohli připojit k instanci systému SQL Server z jiného OM nebo Internetu, je nutné provést následující úkoly podle popisu v následujících částech:

- [Otevřete porty TCP v bráně firewall systému Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurace systému SQL Server poslouchat protokolu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurace systému SQL Server pro kombinovaný režim ověřování](#configure-sql-server-for-mixed-mode-authentication)
- [Vytvořte přihlášení ověřování serveru SQL Server](#create-sql-server-authentication-logins)
- [Konfigurace příchozí pravidla skupiny zabezpečení sítě](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Konfigurace DNS štítku na veřejnou IP adresu](#configure-a-dns-label-for-the-public-ip-address)
- [Připojení k databázový stroj z jiného počítače](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky

Zřízení pokyny spolu s takto připojení najdete v tématu [zřizování SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Prozkoumání Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro systém SQL Server na virtuálních počítačích Azure.

Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
