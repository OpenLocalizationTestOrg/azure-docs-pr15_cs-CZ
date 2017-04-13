<properties
    pageTitle="Migraci zdrojů IaaS z klasické správci zdrojů Azure pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
    description="Tento článek provede podporované platformy migrace zdrojů z klasického správci zdrojů Azure pomocí rozhraní příkazového řádku Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migraci zdrojů IaaS z klasické správci zdrojů Azure pomocí rozhraní příkazového řádku Azure

Tento postup ukazují, jak migrovat infrastruktury jako zdroje služby (IaaS) z modelu klasické nasazení modelu nasazení Správce prostředků Azure taky pomocí příkazů Azure rozhraní příkazového řádku (rozhraní příkazového řádku). V článku vyžaduje [Rozhraní příkazového řádku Azure](../xplat-cli-install.md).

>[AZURE.NOTE] Jsou všechny operace zde popsané idempotent. Pokud máte problém než s nepodporovanou funkcí nebo Chyba konfigurace, doporučujeme, abyste opakovat připravit, zrušit nebo potvrdit operace. Platformu bude potom akci opakujte.

## <a name="step-1-prepare-for-migration"></a>Krok 1: Příprava migraci

Tady je několik doporučené postupy, které doporučujeme jak zjistit migrujete IaaS zdroje z klasické správci zdrojů:

- Přečtěte si do [seznamu nepodporované konfigurace nebo funkce](virtual-machines-windows-migration-classic-resource-manager.md). Pokud máte virtuálních počítačích, které používají nepodporované konfigurace nebo funkce, doporučujeme čekat funkce/Konfigurace podpory bude upřesněno. Můžete taky můžete odebrat tuto funkci nebo přesunout z této konfigurace pro povolení migrace, pokud vyhovuje vašim potřebám.
-   Pokud máte automatizované skripty, které nasazení infrastrukturu a aplikací dnes, pokusu o vytvoření podobné nastavení testu pomocí tyto skripty pro migraci. Můžete také nastavit ukázkové prostředí pomocí portálu Azure.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Krok 2: Nastavení předplatného a zaregistrovat poskytovatele

Scénáře migrace musíte nastavit prostředí pro obě classic a správce prostředků. [Instalace rozhraní příkazového řádku Azure](../xplat-cli-install.md) a [Vyberte předplatné](../xplat-cli-connect.md).

Přihlaste se k účtu.
    
    azure login

Pomocí následujícího příkazu vyberte Azure předplatného.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Registrace je čas krok ale je třeba udělat jednou před pokusem o migraci. Bez registrace zobrazí se tato chybová zpráva 

>   *BadRequest: Předplatné není registrovaný migraci.* 

Zaregistrovat se zprostředkovatelem zdroje migrace pomocí následujícího příkazu. Všimněte si, že v některých případech tento příkaz vyprší její časový limit. Registrace však nebude úspěšná.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Počkejte prosím pět minut, než registrace k dokončení. Kontrola stavu schválení pomocí následujícího příkazu. Ujistěte se, že je RegistrationState `Registered` před pokračováním.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Nyní přepínat rozhraní příkazového řádku pro `asm` režimu.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Krok 3: Ujistěte se, že máte dost jádra Azure správce prostředků virtuálního počítače v oblasti Azure aktuální nasazení nebo VNET

V tomto kroku budete muset přepnout na `arm` režimu. To udělejte pomocí následujícího příkazu.

```
azure config mode arm
```

Příkaz rozhraní příkazového řádku umožňuje zkontrolovat aktuální velikost jádra, ke kterým máte ve Správci zdrojů Azure. Další informace o kvótách základní, najdete v článku [limity a správce prostředků Azure](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Až budete mít Hotovo ověří tento krok, můžete přejít zpět `asm` režimu.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Krok 4: Možnost 1 - migrace virtuálních počítačích v cloudové službě 

Vytvořte požadovaný seznam cloudovým službám pomocí následujícího příkazu a potom vyberte cloudové služby, kterou chcete migrovat. Poznámka: Pokud VMs ve službě cloud jsou v síti virtuální nebo pokud mají webové či pracovníka role, zobrazí se vám chybová zpráva.

    azure service list

Spusťte tento příkaz zjištění názvu nasazení ke cloudové službě z podrobných výstupu. Ve většině případů název nasazení je shodný s názvem služby cloudu.

    azure service show <serviceName> -vv

Připravte se na virtuálních počítačích ve službě cloud migraci. Máte dvě možnosti můžete vybírat.

Pokud chcete migrovat VMs k vytvoření platformy virtuální síti, použijte následující příkaz.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Pokud chcete migrovat do existující virtuální sítě v modelu nasazení Správce prostředků, použijte následující příkaz.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Po úspěšném připravit si prošli podrobný výstup a získat stav migrace VMs zajistit, aby se v `Prepared` stavu.

    azure vm show <vmName> -vv

Kontrola konfigurace připravené zdrojů pomocí rozhraní příkazového řádku nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte následující příkaz.

    azure service deployment abort-migration <serviceName> <deploymentName>

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Krok 4: Možnost 2 – migrace virtuálních počítačích v síti virtuální

Vyberte virtuální sítě, ke které chcete migrovat. Poznámka: Pokud virtuální sítě obsahuje role webu nebo pracovního nebo VMs s nepodporované konfigurace, zobrazí se vám chybová zpráva funkce ověření.

Pomocí následujícího příkazu vstoupit předplatné virtuální sítě.

    azure network vnet list
    
Výstup bude vypadat nějak takhle:

![Snímek obrazovky s příkazového řádku s názvem síť virtuální zvýrazněná.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Ve výše uvedeném příkladu je **virtualNetworkName** celý název **"Skupiny classicubuntu16 classicubuntu16"**.

Příprava virtuální sítě podle svého výběru migraci pomocí následujícího příkazu.

    azure network vnet prepare-migration <virtualNetworkName>

Kontrola konfigurace pro připravené virtuálních počítačích pomocí rozhraní příkazového řádku nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte následující příkaz.

    azure network vnet abort-migration <virtualNetworkName>

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Krok 5: Migrace účet úložiště

Jakmile dokončíte migraci virtuálních počítačích, doporučujeme, abyste migrace účtu úložiště.

Příprava účtu úložiště migraci pomocí následujícího příkazu

    azure storage account prepare-migration <storageAccountName>

Kontrola konfigurace účtu připravené úložiště pomocí rozhraní příkazového řádku nebo portálu Azure. Pokud nejste připravení migrace a chcete se vrátit na původní stav, použijte následující příkaz.

    azure storage account abort-migration <storageAccountName>

Pokud připravené konfigurační připadá dobrý, můžete vpřed a potvrďte zdrojů pomocí následujícího příkazu.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Další kroky

- [Podporované platformy migrace IaaS zdrojů z klasického správci zdrojů](virtual-machines-windows-migration-classic-resource-manager.md)
- [Technická hloubkové postupy na podporované platformy migrace z klasického správci zdrojů](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
