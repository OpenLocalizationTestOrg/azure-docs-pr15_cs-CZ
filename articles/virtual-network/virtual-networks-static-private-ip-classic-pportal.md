<properties 
   pageTitle="Jak nastavit statické IP soukromé klasický režim na portálu Azure | Microsoft Azure"
   description="Principy statické IP adresy soukromé a jak mají ovládat v klasického režimu pomocí portálu Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Jak nastavit statická soukromé IP adresu (klasické) na portálu Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete taky [Spravovat statické soukromé IP adresu v modelu nasazení Správce prostředků](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ukázka kroků očekávat jednoduché prostředí vytvořen. Pokud chcete spustit postup, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovacím prostředí podle [vytvořit vnet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Určení statické IP adresu soukromé při vytváření virtuálního počítače
Pokud chcete vytvořit OM s názvem *DNS01* v *FrontEnd* podsítě VNet s názvem *TestVNet* statické soukromé IP *192.168.1.101*, postupujte následujícím způsobem:

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Nový** > **Výpočet** > **Windows serveru 2012 R2 datacentru**, oznámení, že již zobrazuje **klasické**seznamu **Vyberte model nasazení** a klikněte na **vytvořit**.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. V zásuvné **Vytvořit OM** zadejte název OM vytvořit (*DNS01* v našem případě), místního účtu správce a heslo.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Klikněte na položku **Volitelná konfigurace** > **sítě** > **Virtuální sítě**a potom klikněte na **TestVNet**. Pokud **TestVNet** není k dispozici, ujistěte se, používáte umístění *Centrální USA* a vytvořili testovacím prostředí uvedeno na začátku tohoto článku.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. V **síti** zásuvné Ujistěte se, že podsítě aktuálně vybrané *FrontEnd*, a pak klikněte na **IP adresy**, klikněte v části **přiřazení IP adresa** klikněte na **statický**a zadejte *192.168.1.101* **IP** adresu jak je vidět níže.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Klikněte na tlačítko **OK** v zásuvné **IP adresy** a pak klikněte na tlačítko **OK** v **síti** zásuvné a klikněte na **OK** v zásuvné **volitelné konfigurace** .
7. V zásuvné **Vytvořit OM** klikněte na **vytvořit**. Všimněte si dlaždici níže zobrazené v řídicím panelu.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak získat statické soukromé informace o IP adrese pro virtuálního počítače

Statická soukromé informace o IP adresu pro OM vytvořená pomocí výše uvedené kroky zobrazíte provedení kroků.

1. Z portálu Microsoft Azure Azure, klikněte na **Procházet vše** > **virtuálních počítačích (klasické)** > **DNS01** > **všechna nastavení** > **IP adresy** a oznámení přiřazení IP adresy a IP adres jak je vidět níže.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak odebrat soukromé statickou IP adresu z virtuálního počítače
Z OM vytvořili nad odebrat soukromé statickou IP adresu, postupujte následujícím způsobem.
    
1. Z **adresy IP** zásuvné nahoře klikněte na **dynamické** napravo od **přiřazení IP adresy**, a pak klikněte na tlačítko **Uložit**a klikněte na **Ano**.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak přidat statické soukromé IP adresu do existující OM
Pokud chcete přidat statické IP adresu soukromé OM vytvořené pomocí výše uvedené kroky, postupujte následujícím způsobem:

1. **IP adresy** zásuvné nahoře klikněte na **statický** napravo od **přiřazení IP adres**.
2. Zadejte *192.168.1.101* **IP adresa**a pak klikněte na **Uložit**a klikněte na **Ano**.

## <a name="next-steps"></a>Další kroky

- Další informace o [Rezervovaná veřejnou IP](virtual-networks-reserved-public-ip.md) adresách.
- Další informace o adresách [úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Použijte [User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).