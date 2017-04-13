<properties
    pageTitle="Vytvoření Linux OM pomocí portálu Azure | Microsoft Azure"
    description="Vytvoření Linux OM pomocí portálu Azure."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Vytvoření Linux OM na Azure pomocí portálu


Tento článek popisuje, jak používat [Azure portál](https://portal.azure.com/) k vytvoření virtuálního počítače Linux.

Požadavky na jsou:

- [účet Azure](https://azure.microsoft.com/pricing/free-trial/)

- [SSH veřejných a privátních klíčů souborů](virtual-machines-linux-mac-create-ssh-keys.md)


1. Přihlášení k portálu Azure pomocí své identity pro účet Azure, klikněte na tlačítko **+ Nový** v levém horním rohu:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Klikněte na **virtuálních počítačích** **Marketplace** , pak **Se systémem Ubuntu serveru 14.04 l** **Kategorie vybrané aplikace** obrázky seznamu.  Ověřte dole modelu nasazení `Resource Manager` a potom klikněte na **vytvořit**.

    ![– screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Na stránce **základní informace o** zadejte:
    - Název OM
    - uživatelské jméno pro správce
    - Typ ověřování nastavený na **SSH veřejný klíč**
    - svůj veřejný klíč SSH jako řetězec (z vaší `~/.ssh/` adresář)
    - Zdroj název skupiny nebo vyberte existující skupiny

    a klikněte na **OK** pokračovat a zvolte požadovanou velikost OM; by měl vypadat nějak takto:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Zvolte velikost **DS1** nainstaluje se systémem Ubuntu Premium SSD a klikněte na **Vybrat** ke konfiguraci nastavení.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. Ve skupinovém rámečku **Nastavení**ponechte výchozí nastavení pro ukládání a sítě hodnoty a klikněte na tlačítko **OK** zobrazíte na souhrn.  Oznámení typu disku byl nastaven do Premium SSD výběrem DS1, **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Potvrzení nastavení pro nové OM systémem Ubuntu a klikněte na **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Otevřete portál řídicího panelu a v **síťových rozhraní** zvolte kartu NIC

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Otevření nabídky veřejnou IP adres ve skupinovém rámečku Nastavení NIC

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH do veřejné adresy IP pomocí veřejným klíčem SSH

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Další kroky

Teď jste si vytvořili Linux OM rychle pro účely testování nebo ukázka jako referenci. Vytvoření Linux OM přizpůsobený infrastrukturu, můžete pomocí některého z těchto článků.

- [Vytvoření Linux OM na Azure pomocí šablony](virtual-machines-linux-cli-deploy-templates.md)
- [Vytvoření SSH zabezpečená Linux OM na Azure pomocí šablony](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Vytvoření Linux OM pomocí rozhraní příkazového řádku Azure](virtual-machines-linux-create-cli-complete.md)
