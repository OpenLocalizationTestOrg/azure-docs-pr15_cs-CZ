<properties
    pageTitle="Připojení k SQL serveru virtuálního počítače (klasický) | Microsoft Azure"
    description="Zjistěte, jak se připojit k serveru SQL Server spuštěné v počítači virtuální v Azure. Toto téma používá modelu klasické nasazení. Scénáře lišit v závislosti na konfiguraci sítě a umístění klienta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Připojení k SQL serveru virtuálního počítače na Azure (klasické nasazení)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-sql-connect.md)
- [Klasický](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Základní informace

Toto téma popisuje, jak se připojit k instanci serveru SQL Azure virtuální počítač se systémem. Popisuje některé [scénáře obecné připojení](#connection-scenarios) a potom obsahuje [Podrobný postup konfigurace systému SQL Server konektivitu OM Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Pokud používáte VMs správce prostředků, najdete v článku [připojení k SQL serveru virtuálního počítače na Azure pomocí Správce prostředků](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scénáře připojení

Způsob, jakým klienta spojí se serverem SQL virtuálního počítače se systémem se liší v závislosti na umístění klienta a konfiguraci počítače a sítě. Podobnému sledu patří:

- [Připojení k serveru SQL Server ve stejném cloudové služby](#connect-to-sql-server-in-the-same-cloud-service)
- [Připojení k serveru SQL Server přes internet](#connect-to-sql-server-over-the-internet)
- [Připojení k serveru SQL Server ve stejné síti virtuální](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Než se připojíte pomocí kterékoli z těchto postupů, postupujte podle [kroků v tomto článku Konfigurace připojení](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Připojení k serveru SQL Server ve stejném cloudové služby

Více virtuálních počítačích lze vytvořit ve stejném cloudové služby. Tento scénář virtuálních počítačích, najdete v tématu [jak připojit virtuálních počítačích s virtuální sítě nebo cloudové služby](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Tento scénář je při pokusu klienta na jednom počítači virtuální připojení k SQL serveru na jiném počítači virtuální ve stejném cloudové služby.

V tomto případě se můžete připojit pomocí OM **název** (viz také jako **Název počítače** nebo **název hostitele** na portálu). Toto je název, který jste zadali OM při vytváření. Například pokud název SQL OM **mysqlvm**klienta OM ve stejném cloudové služby použít následující připojovací řetězec pro připojení:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Připojení k serveru SQL Server přes Internet

Pokud chcete připojit k serveru SQL Server databázový stroj z Internetu, musíte vytvořit virtuální počítač koncový bod pro příchozí TCP komunikace. Tento krok Azure konfigurace směrovat příchozí TCP port umožnění datových přenosů do portu TCP přístupný virtuální počítač.

Připojení přes internet, je nutné použít název DNS OM a číslo portu koncový bod OM (nakonfigurované dál v tomto článku). Najděte název DNS, přejděte na portál Azure a vyberte **virtuálních počítačích (klasické)**. Vyberte virtuálního počítače. **Název DNS** se zobrazuje v části **Přehled** .

Zvažte například klasické virtuálního počítače s názvem **mysqlvm** s názvem DNS **mysqlvm7777.cloudapp.net** a OM koncového bodu **57500**. Za předpokladu, že správně nakonfigurovaném připojení, následující připojovací řetězec může se virtuální počítač všude na Internetu:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

I když připojení pro klienty umožňuje přes internet, to neznamená kdokoliv můžete připojit k serveru SQL. Mimo musí klientů správné uživatelské jméno a heslo. Pro lepší zabezpečení nebudete používat známý port 1433 koncového bodu veřejné virtuálního počítače. A pokud je to možné, můžete do ní přidat ACL na koncový bod omezení přenosy pouze klientům umožnit. Pokyny k používání ACL s koncové body v tématu [Správa ACL na koncový bod](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Je důležité si všimněte si, že použijete tento postup komunikovat s SQL Server, všechny odchozí dat z Azure datacentra podléhá normální [ceny převodů výstupní data](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Připojení k serveru SQL Server ve stejné síti virtuální

[Virtuální sítě](../virtual-network/virtual-networks-overview.md) umožňuje další scénáře. Můžete se připojit VMs ve stejné síti virtuální, i když jsou tyto VMs v jiné cloudové služby. A s [VPN k webu](../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architektura, ke kterému je připojen VMs s místní sítě a počítačů.

Virtuální sítí umožňuje připojit Azure VMs k doméně. To je jediný způsob, jak pomocí ověřování Windows na serveru SQL Server. Ostatních případech připojení vyžadují ověřování serveru SQL pomocí uživatelského jména a hesla.

Pokud přecházíte konfigurace doménovém prostředí a ověřování systému Windows, není nutné Pokud chcete konfigurovat veřejné koncový bod nebo ověřování serveru SQL a přihlášení postupujte podle pokynů v tomto článku. V tomto scénáři můžete připojit k instanci systému SQL Server zadáním názvu SQL Server OM v připojovacím řetězci. Následující příklad předpokládá ověřování systému Windows taky nakonfigurovaná a že uživatel má udělený přístup k instanci systému SQL Server.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Postup konfigurace připojení k serveru SQL Azure OM

Podle těchto kroků ukazují, jak se připojit k instanci systému SQL Server přes internet pomocí služby SQL Server Management Studio (SSMS). Však stejný postup platí pro uskutečnění usnadnění pro aplikace spuštění obou místní nebo v Azure virtuálního počítače SQL serveru.

Abyste se mohli připojit k instanci systému SQL Server z jiného OM nebo Internetu, je nutné provést následující úkoly podle popisu v následujících částech:

- [Vytvoření koncového bodu TCP virtuálního počítače](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Otevřete porty TCP v bráně firewall systému Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurace systému SQL Server poslouchat protokolu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurace systému SQL Server pro kombinovaný režim ověřování](#configure-sql-server-for-mixed-mode-authentication)
- [Vytvořte přihlášení ověřování serveru SQL Server](#create-sql-server-authentication-logins)
- [Zjištění názvu DNS virtuálního počítače](#determine-the-dns-name-of-the-virtual-machine)
- [Připojení k databázový stroj z jiného počítače](#connect-to-the-database-engine-from-another-computer)

Připojení cesta je shrnout tak, že na následujícím obrázku:

![Připojení k serveru SQL Server virtuálního počítače](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Další kroky

Posluchače by měl implementace také plánování používání skupin AlwaysOn dostupnost pro dostupnost a obnovení. Databáze klienti připojují posluchače namísto přímo jedna instance serveru SQL Server. Posluchače směruje klientů primární otevřené ve skupině dostupnosti. Další informace najdete v tématu [Konfigurace posluchače ILB pro skupiny dostupnosti AlwaysOn v Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Je důležité si můžete všechny zabezpečení doporučené postupy pro systém SQL Server Azure virtuální počítač se systémem. Další informace najdete v tématu [Otázky bezpečnosti pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-security.md).

[Prozkoumání Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro systém SQL Server na virtuálních počítačích Azure. 

Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
