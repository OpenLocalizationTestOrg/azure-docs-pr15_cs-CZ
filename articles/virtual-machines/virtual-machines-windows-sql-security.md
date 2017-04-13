<properties
    pageTitle="Otázky bezpečnosti pro systém SQL Server v Azure | Microsoft Azure"
    description="Toto téma se vztahuje k prostředkům vytvořená pomocí klasické nasazení modelu a poskytuje obecné pokyny pro zajištění SQL serveru ve Azure virtuálního počítače."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Otázky bezpečnosti pro systém SQL Server ve virtuálních počítačích Azure
 
Toto téma obsahuje celkové pravidel zabezpečení, které pomáhají stanovit zabezpečený přístup k instance serveru SQL Server Azure OM. Zajistit lepší ochranu na vaší databáze instance serveru SQL Server v Azure doporučujeme však implementace postupy zabezpečení tradiční místního kromě zabezpečení doporučené postupy pro Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Další informace o zabezpečení postupy SQL serveru najdete v článku [SQL Server 2008 R2 doporučené postupy pro zabezpečení – provozní a úkoly správy](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure v souladu s několika právní odvětví a standardy, které umožňují vytvářet řešení kompatibilní se serverem SQL Server spuštěné v počítači virtuální. Informace o dodržování s Azure najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/).

Toto je seznam zabezpečení doporučení, které je potřeba zvážit při konfiguraci a připojení k instanci systému SQL Server ve OM Azure.

## <a name="considerations-for-managing-accounts"></a>Co zvážit při správě účtů:

- Vytvořte účet jedinečných oprávnění místního správce, který není s názvem **Správce**.

- Pomocí složitých silných hesel pro všechny účty. Další informace o tom, jak vytvořit silné heslo, najdete v článku [Tipy pro vytváření silných hesel](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) .

- Ve výchozím nastavení Azure slouží k výběru ověřování systému Windows během instalace SQL serveru virtuálního počítače. Proto zakázat přihlášení **správce systému** a přiřazené heslo nastavením. Jsme doporučujeme by měl být **správce systému** přihlášení nebudou používat nebo povolené. Následující seznam uvádí alternativní strategie v případě potřeby přihlášení SQL:
    - Vytvořte účet SQL, který je členem členem.
    - Pokud je nutné použít přihlašovací **správce systému** , povolit přihlášení ji přejmenovat a přiřaďte nové.
    - Obě možnosti výše uvedených vyžadují změnu režim ověřování **serveru SQL Server**a režimu ověřování systému Windows. Další informace najdete v tématu [Změna režimu ověřování serveru](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Důležité informace týkající se zabezpečení připojení Azure virtuálního počítače:

- Zvažte použití [Azure virtuální sítě](../virtual-network/virtual-networks-overview.md) ke správě virtuálních počítačích místo veřejného RDP porty.

- [Skupina zabezpečení síti](../virtual-network/virtual-networks-nsg.md) (NSG) umožňuje povolit nebo odepřít do virtuálního počítače v síti. Pokud chcete použít NSG a už máte koncový bod ACL na místě, odstraňte nejdříve koncový bod ACL. Informace o tom, jak to udělat najdete v tématu [Správa seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Pokud používáte koncové body, pokud nepoužijete je odeberte všechny koncové body v počítači virtuální. Pokyny k používání ACL s koncové body v tématu [Správa ACL na koncový bod](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Povolte šifrovaného připojení možnost pro instanci databázový stroj SQL Server ve virtuálních počítačích Azure. Konfigurace instance serveru SQL pomocí certifikátu s podpisem. Další informace najdete v tématu [Povolení šifrované připojení k databázový stroj](https://msdn.microsoft.com/library/ms191192.aspx) a [Syntaxi připojovacího řetězce](https://msdn.microsoft.com/library/ms254500.aspx).

- Pokud virtuálních počítačích mají přístup jenom z konkrétní síti, použijte na omezení přístupu k určité IP adresy nebo podsítí brána Windows Firewall.

## <a name="next-steps"></a>Další kroky

Pokud vás zajímají také doporučené postupy kolem výkonu, najdete v článku [Výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).
