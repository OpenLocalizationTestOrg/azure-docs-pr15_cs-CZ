<properties
    pageTitle="Nastavení koncových bodů na klasické OM Windows | Microsoft Azure"
    description="Zjistěte, jak nastavit koncové body OM Windows Azure klasické portálu komunikovat s virtuálního počítače Windows Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Jak nastavit koncové body na klasické počítač virtuální Windows v Azure


Všechna okna, které virtuálních počítačích, které jste vytvořili v Azure pomocí klasické nasazení modelu můžete automaticky komunikovat osobní kanálu sítě s jiných virtuálních počítačích ve stejném cloudové služby nebo virtuální sítě. Však počítačů v Internetu nebo jiných virtuální sítích vyžadují koncové body tak, aby příchozí síťové směrovaly do virtuálního počítače. Tento článek je také k dispozici pro [Linux virtuálních počítačích](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Správce prostředků** nasazení modelu koncové body se konfigurují **Sítě skupin zabezpečení (NSGs)**. Další informace najdete v tématu [Povolit externí přístup k vaší OM pomocí portálu Azure] (virtual-machines-windows-nsg-quickstart-portal.md).

Při vytváření virtuálního počítače Windows Azure klasické portálu běžné koncové body stejně jako pro vzdálené plochy a Windows PowerShell Vzdálená obvykle vytvářejí za vás automaticky. Při vytváření virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Další kroky

* Použití rutiny prostředí PowerShell Azure nastavit koncový bod OM, najdete v článku [Přidání AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Správa ACL na koncový bod pomocí rutiny Powershellu Azure, najdete v článku [řízení přístupu Správa seznamů (ACL) pro koncové body pomocí prostředí PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Pokud jste vytvořili v modelu nasazení Správce prostředků virtuálního počítače, můžete [vytvořit skupiny zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-ps.md) přenosy řízení bude v angličtině Azure Powershellu.
