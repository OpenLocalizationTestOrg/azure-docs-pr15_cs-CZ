<properties
    pageTitle="Komunita nástroje pro správu služby Azure správce prostředků Azure migrace"
    description="Tento článek obsahuje nástroje, které jste obdrželi komunita pro pomoc s migrací IaaS zdrojů z Správa služby Azure do zásobníku správce prostředků Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Komunita nástroje pro správu služby Azure správce prostředků Azure migrace

Tento článek obsahuje nástroje, které jste obdrželi komunita pro pomoc s migrací IaaS zdrojů z Správa služby Azure do zásobníku správce prostředků Azure.

>[AZURE.NOTE]Tyto nástroje úředně nepodporuje Microsoft Support. Proto jsou otevřeny, ji na Github a jsme spokojení přijmout PRs opravy nebo další scénáře. A nahlaste problém, použijte funkci Github problémy.
>
> Migrace pomocí těchto nástrojích způsobí prostoje klasické virtuálního počítače. Pokud hledáte migrace podporované platformy, navštěvujte blog o 
>
>- [Podporované platformy migrace zdrojů IaaS od klasického správce prostředků Azure zásobníku](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Technická hloubkové postupy platformy podporované migrace z klasického správci zdrojů Azure](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migraci zdrojů IaaS z klasické Azure zdroje správci pomocí prostředí PowerShell Azure](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Toto je modulu skript Powershellu pro migraci vaší **jednoho** virtuální Stroj z Správa služby Azure zásobníku do zásobníku správce prostředků Azure. 

[Odkaz na nástroj dokumentace](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz je další možnost migrovat úplná sada IaaS Správa služby Azure zdrojů do Azure správce prostředků IaaS zdroje. Migrace může dojít v rámci stejného předplatného nebo mezi různých předplatných a typy předplatného (ex: CSP předplatná).

[Odkaz na nástroj dokumentace](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)