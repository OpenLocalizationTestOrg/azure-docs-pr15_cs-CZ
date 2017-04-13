<properties 
   pageTitle="Správa NSGs na portálu náhled správce prostředků | Microsoft Azure"
   description="Naučte se spravovat stávajícího NSGs na portálu náhled ve Správci zdrojů"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Správa NSGs pomocí portálu náhledu

> [AZURE.SELECTOR]
- [Portál](virtual-network-manage-nsg-arm-portal.md)
- [Prostředí PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure rozhraní příkazového řádku](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Načtení informací

Můžete zobrazit existující NSGs, načtení pravidel pro existující NSG a zjistěte, jaké prostředky NSG souvisí s.

### <a name="view-existing-nsgs"></a>Zobrazení existujícího NSGs
Pokud chcete zobrazit všechny existující NSGs předplatné, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Procházet >** > **skupiny zabezpečení sítě**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Projít seznam NSGs v zásuvné **skupiny zabezpečení sítě** .

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Zobrazení seznamu NSGs ve skupině **RG NSG** prostředků, postupujte následujícím způsobem. 

1. Klikněte na **skupiny zdrojů >** > **RG NSG** > **...**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. V seznamu zdrojů vyhledejte položky zobrazení ikony NSG, jak je znázorněno níže zásuvné **zdroje** .

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Seznam všech pravidel pro NSG

Pravidla NSG s názvem **NSG FrontEnd**zobrazíte podle následujících kroků. 

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na **pravidla příchozí zabezpečení**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Jak je ukázáno v následujícím příkladu, zobrazí se zásuvné **Příchozí pravidla zabezpečení** .

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Na kartě **Nastavení** klikněte na **pravidla odchozí zabezpečení** zobrazíte odchozího pravidla.

>[AZURE.NOTE] Pokud chcete zobrazit výchozí pravidla, klikněte na ikonu **výchozí pravidla** v horní části zásuvné, která se zobrazí tato pravidla.

### <a name="view-nsgs-associations"></a>Zobrazit NSGs přidružení

Co prostředky, které je NSG **NSG FrontEnd** přidružit zobrazíte podle následujících kroků.

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na **podsítí** zobrazíte jaké podsítí jsou přidružené k NSG.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Na kartě **Nastavení** klikněte na **síť rozhraní** zobrazíte nic jaké přidružený k NSG.

## <a name="manage-rules"></a>Správa pravidel

Můžete přidat pravidla do existující NSG, upravit existující pravidla a odebrat pravidla.

### <a name="add-a-rule"></a>Přidání pravidla

Přidání pravidla umožňují **příchozích** port **443** z jakéhokoli stroje **NSG FrontEnd** NSG, postupujte následujícím způsobem.

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na **pravidla příchozí zabezpečení**.
3. V zásuvné **Příchozí pravidla zabezpečení** klikněte na **Přidat**. Pak v zásuvné **Přidat pravidlo pro příchozí zabezpečení** vyplnit hodnoty, jak je ukázáno v následujícím příkladu a klikněte na **OK**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Po pár sekund Všimněte si nové pravidlo zásuvné **pravidla příchozí zabezpečení** .

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Změnit pravidlo

Chcete-li změnit pravidlo vytvořené nad povolíte příchozí data z **Internetu** , postupujte následujícím způsobem.

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na pravidlo vytvořili výše.
3. V zásuvné **Povolit https** změňte vlastnost **zdroj** , jak je ukázáno v následujícím příkladu a klikněte na tlačítko **Uložit**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Odstranění pravidla

Odstranění pravidla vytvořili výše, postupujte následujícím způsobem.

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na pravidlo vytvořili výše.
3. V zásuvné **Povolit https** klikněte na **Odstranit**a klikněte na **Ano**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Správa přidružení

Můžete přiřadit NSG do podsítí a nic. Můžete taky oddělit NSG z libovolného zdroje, které je přidružené k.

### <a name="associate-an-nsg-to-a-nic"></a>Přidružení NSG na NIC

Chcete-li přidružit **NSG FrontEnd** NSG **TestNICWeb1** NIC, postupujte následujícím způsobem.

1. Na zásuvné **skupiny zabezpečení sítě** , nebo na zásuvné **zdroje** nahoře klikněte **NSG FrontEnd**.
2. Na kartě **Nastavení** klikněte na **síť rozhraní** > **přidružení** > **TestNICWeb1**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Oddělit NSG z NIC

Chcete-li oddělit **NSG FrontEnd** NSG z **TestNICWeb1** NIC, postupujte následujícím způsobem.

1. Z portálu Microsoft Azure, klikněte na **skupiny zdrojů >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. V zásuvné **TestNICWeb1** klikněte na **změnit zabezpečení...**  > **None**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Můžete taky tento zásuvné přidružíte NIC do existující NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Oddělit NSG z podsítě

Chcete-li oddělit **NSG FrontEnd** NSG z podsítě **FrontEnd** , postupujte následujícím způsobem.

1. Z portálu Microsoft Azure, klikněte na **skupiny zdrojů >** > **RG NSG** > **...**  >  **TestVNet**.
2. V **Nastavení** zásuvné, klikněte na **podsítí** > **FrontEnd** > **skupiny zabezpečení sítě** > **žádný**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. V zásuvné **FrontEnd** klikněte na **Uložit**.

![Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Přidružení NSG do podsítě

Pokud chcete znovu přiřadit **NSG FrontEnd** NSG podsítě **FronEnd** , postupujte následujícím způsobem.

1. Z portálu Microsoft Azure, klikněte na **skupiny zdrojů >** > **RG NSG** > **...**  >  **TestVNet**.
2. V **Nastavení** zásuvné, klikněte na **podsítí** > **FrontEnd** > **skupiny zabezpečení sítě** > **NSG FrontEnd**.
3. V zásuvné **FrontEnd** klikněte na **Uložit**.

>[AZURE.NOTE] Můžete také propojit NSG do podsítě z thh NSG **Nastavení** zásuvné.

## <a name="delete-an-nsg"></a>Odstranění NSG

NSG můžete odstranit pouze pokud máte není přidružený k jakémukoli prostředku. Pokud chcete odstranit NSG, postupujte následujícím způsobem.

1. Z portálu Microsoft Azure, klikněte na **skupiny zdrojů >** > **RG NSG** > **...**  >  **NSG FrontEnd**.
2. V **Nastavení** zásuvné klikněte na **síť rozhraní**.
3. Pokud jsou všechny nic uvedený, klikněte NIC a postupujte podle kroku 2 v [Dissociate NSG z NIC](#Dissociate-an-NSG-from-a-NIC).
4. Opakujte krok 3 pro každý NIC.
5. V **Nastavení** zásuvné klikněte na **podsítí**.
6. Pokud jsou uvedené podsítí, klikněte na podsítě a postupujte podle kroků 2 a 3 v [Dissociate NSG z podsítě](#Dissociate-an-NSG-from-a-subnet).
7. Posunuje zleva zásuvné **NSG FrontEnd** klikněte na příkaz **Odstranit** > **Ano**.

[Azure portálu - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Další kroky

- [Povolit protokolování](virtual-network-nsg-manage-log.md) pro NSGs.
