<properties 
   pageTitle="Jak vytvořit NSGs v režimu ARM pomocí portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet a publikovat NSGs v ARM pomocí portálu Azure"
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

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Jak spravovat NSGs pomocí portálu Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [vytvořit NSGs v modelu klasické nasazení](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Ukázka prostředí PowerShell příkazy dole očekávat jednoduché prostředí vytvořen podle výše uvedených scénář. Pokud budete chtít spustit příkazy tak, jak jsou zobrazeny v tomto dokumentu, nejdřív vytvořit testovacím prostředí nasazením [tuto šablonu](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikněte na **Deploy Azure**, nahraďte výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů na portálu. Postupem použít **RG NSG** jako název, který byl používaný šabloně skupina zdroje.

## <a name="create-the-nsg-frontend-nsg"></a>Vytvoření NSG NSG FrontEnd

Vytvoření **NSG FrontEnd** NSG podle výše uvedených scénáře, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Procházet >** > **skupiny zabezpečení sítě**.

    ![Azure portálu - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. V zásuvné **skupiny zabezpečení sítě** klikněte na **Přidat**.
  
    ![Azure portálu - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. V zásuvné **vytvořit skupinu zabezpečení sítě** vytvořit NSG s názvem *NSG FrontEnd* ve skupině *RG NSG* zdroje a potom klikněte na **vytvořit**.

    ![Azure portálu - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Vytvoření pravidla v existující NSG

Pokud chcete vytvořit pravidla do existující NSG z portálu Microsoft Azure, postupujte následujícím způsobem.

2. Klikněte na **Procházet >** > **skupiny zabezpečení sítě**.

3. V seznamu NSGs, klikněte na **NSG FrontEnd** > **pravidla příchozí zabezpečení**

    ![Azure portálu - NSG FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. V seznamu pravidel **Příchozí zabezpečení**klikněte na **Přidat**.

    ![Azure portálu - přidat pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. V zásuvné **Přidat pravidlo pro příchozí zabezpečení** vytvořit pravidlo s názvem *pravidlo webu* s prioritou *200* umožňují přístup prostřednictvím *protokolu TCP* port *80* všechny v angličtině z libovolného zdroje a klikněte na **OK**. Všimněte si, že většina nastavení výchozích hodnot už.

    ![Azure portál – nastavení pravidel](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Za několik sekund, než se zobrazí v NSG nové pravidlo.

    ![Azure portál – nové pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Opakujte kroky 6 vytvořit pravidlo pro příchozí připojení s názvem *rdp pravidla* s prioritou *250* umožňují přístup prostřednictvím *protokolu TCP* port *3389* všechny v angličtině z libovolného zdroje.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Přidružení NSG podsítě FrontEnd

1. Klikněte na **Procházet >** > **skupiny zdrojů** > **RG NSG**.
2. V zásuvné **RG NSG** kliknutím ****  >  **TestVNet**.

    ![Azure portálu - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. V **Nastavení** zásuvné, klikněte na **podsítí** > **FrontEnd** > **skupiny zabezpečení sítě** > **NSG FrontEnd**.

    ![Azure portál – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. V zásuvné **FrontEnd** klikněte na **Uložit**.

    ![Azure portál – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Vytvoření NSG NSG back-end

Pokud chcete vytvořit **NSG back-end** NSG a přidružit k podsítě **back-end** , postupujte následujícím způsobem.

1. Opakováním kroků v tématu [Vytvoření NSG NSG FrontEnd](#Create-the-NSG-FrontEnd-NSG) k vytvoření NSG s názvem *NSG back-end*
2. Opakováním kroků v tématu [Vytvoření pravidel v existující NSG](#Create-rules-in-an-existing-NSG) vytvořit **příchozí** pravidla v následující tabulce.

  	|Příchozí pravidla|Odchozího pravidla|
  	|---|---|
  	|![Azure portálu - příchozí pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure portálu - odchozího pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Opakujte kroky [přidružení NSG podsítě FrontEnd](#Associate-the-NSG-to-the-FrontEnd-subnet) přidružíte **NSG back-end** NSG podsítě **back-end** .

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [Spravovat existující NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Povolit protokolování](virtual-network-nsg-manage-log.md) pro NSGs.