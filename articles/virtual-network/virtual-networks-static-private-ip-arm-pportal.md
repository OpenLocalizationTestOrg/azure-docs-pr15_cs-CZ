<properties 
   pageTitle="Jak nastavit statické IP soukromé v režimu ARM pomocí portálu Azure | Microsoft Azure"
   description="Principy soukromé IP adresy (poklesu) a jak mají ovládat v režimu ARM pomocí portálu Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Jak nastavit statická soukromé IP adresu na portálu Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [Spravovat soukromé IP adresa v modelu klasické nasazení](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ukázka kroků očekávat jednoduché prostředí vytvořen. Pokud chcete spustit postup, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovacím prostředí podle [vytvořit vnet](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Postup vytvoření virtuálního počítače pro účely testování soukromé statické IP adresy

Pomocí portálu Azure se nedá nastavit statická soukromé IP adresu při vytváření OM v režimu nasazení Správce prostředků. Je nutné nejprve vytvořit OM, vložíte jej nastavit soukromé IP statická.

Pokud chcete vytvořit OM s názvem *DNS01* v *FrontEnd* podsítě VNet s názvem *TestVNet*, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Nový** > **Výpočet** > **Windows serveru 2012 R2 datacentru**, oznámení, že už zobrazuje **Správce prostředků**seznamu **Vyberte model nasazení** a klikněte na **vytvořit**, jak je vidět na následujícím obrázku.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. V zásuvné **Základy** zadejte název OM vytvořit (*DNS01* v našem případě), místního účtu správce a heslo, jak je vidět na následujícím obrázku.

    ![Základní informace o zásuvné](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Ujistěte se, že **umístění** vybrané *Centrální USA*, a pak klikněte na **Vybrat stávající** pod **pole Skupina zdroje**a pak znova, klikněte na **pole Skupina zdroje** klikněte *TestRG*a klikněte na tlačítko **OK**.

    ![Základní informace o zásuvné](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. V zásuvné **Zvolit rozměry** vyberte **A1 standardní**a potom klikněte na **Výběr**.

    ![Zvolte velikost zásuvné](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. V **Nastavení** zásuvné zkontrolujte nastavená následující vlastnosti jsou nastaveny pomocí níže uvedených hodnot a klikněte na **OK**.

    -**Úložiště účtu**: *vnetstorage*
    - **Sítě**: *TestVNet*
    - **Podsítě**: *FrontEnd*

    ![Zvolte velikost zásuvné](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. V zásuvné **Souhrn** klikněte na **OK**. Všimněte si dlaždici níže zobrazené v řídicím panelu.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak získat statické soukromé informace o IP adrese pro virtuálního počítače

Statická soukromé informace o IP adresu pro OM vytvořená pomocí výše uvedené kroky zobrazíte provedení kroků.

1. Z portálu Microsoft Azure Azure, klikněte na **Procházet vše** > **virtuálních počítačích** > **DNS01** > **všechna nastavení** > **Síťová rozhraní** a potom klikněte na pouze rozhraní sítě uvedené.

    ![Nasazení dlaždice OM](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. V zásuvné **rozhraní sítě** , klikněte na **všechna nastavení** > **IP adresy** a Všimněte si hodnoty **přiřazení** a **IP adresu** .

    ![Nasazení dlaždice OM](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak přidat statické soukromé IP adresu do existující OM
Pokud chcete přidat statické IP adresu soukromé OM vytvořené pomocí výše uvedené kroky, postupujte následujícím způsobem:

1. **IP adresy** zásuvné nahoře klikněte na **statický** v části **přiřazení**.
2. Zadejte *192.168.1.101* **IP**adresa a klikněte na **Uložit**.

    ![Vytvoření OM Azure portálu](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Pokud po kliknutí na příkaz **Uložit** zjistíte, zda přiřazení pořád nastavena **dynamické**, znamená to, že na IP adresu, kterou jste zadali se už používá. Vyzkoušejte jinou IP adresu.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak odebrat soukromé statickou IP adresu z virtuálního počítače
Z OM vytvořili nad odebrat soukromé statickou IP adresu, postupujte podle kroku dole.
    
1. Na zásuvné **IP adresy** nahoře klikněte v části **přiřazení** **dynamické** a klikněte na tlačítko **Uložit**.

## <a name="next-steps"></a>Další kroky

- Další informace o [Rezervovaná veřejnou IP](virtual-networks-reserved-public-ip.md) adresách.
- Další informace o adresách [úrovni instance veřejnou IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Použijte [User REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).