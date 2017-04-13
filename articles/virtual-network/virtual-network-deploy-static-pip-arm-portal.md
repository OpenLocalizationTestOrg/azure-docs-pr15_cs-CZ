<properties 
   pageTitle="Nasazení OM pomocí statické IP veřejné na portálu Azure správce prostředků | Microsoft Azure"
   description="Naučte se nasadit VMs pomocí statické IP veřejné na portálu zure ve Správci zdrojů"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Nasazení OM statické veřejnou IP pomocí portálu Azure

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Vytvoření virtuálního počítače pomocí statické IP veřejné 

Pokud chcete vytvořit virtuálního počítače s statické veřejnou IP adresu na portálu Azure, postupujte následujícím způsobem.

1. V prohlížeči přejděte na [portál Azure](https://portal.azure.com) a v případě potřeby Přihlaste se pomocí účtu Azure.
2. V levém horním rohu na portálu, klikněte na **Nový**>>**Výpočet**>**Windows serveru 2012 R2 Datacentra**.
3. V seznamu **Vyberte nasazení modelu** vyberte **Správce** a klikněte na **vytvořit**.
4. V zásuvné **Základy** zadejte OM informace, jak je ukázáno v následujícím příkladu a potom klikněte na **OK**.

    ![Azure portál – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. V zásuvné **Zvolte velikost** klikněte na **Standardní A1** , jak je ukázáno v následujícím příkladu a potom na tlačítko **Vybrat**.

    ![Azure portálu - výběr velikosti](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. V **Nastavení** zásuvné klikněte na **veřejnou IP adresu**a potom v zásuvné **vytvořit veřejnou IP adresu** v části **přiřazení**, klikněte na **statický** jak je ukázáno v následujícím příkladu. A potom klikněte na **OK**.

    ![Azure portálu - vytvořit veřejnou IP adresu](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. V **Nastavení** zásuvné klikněte na **OK**.
8. Zkontrolujte zásuvné **Souhrn** , jak je ukázáno v následujícím příkladu a klikněte na tlačítko **OK**.

    ![Azure portálu - vytvořit veřejnou IP adresu](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Všimněte si nových dlaždice v řídicím panelu.

    ![Azure portálu - vytvořit veřejnou IP adresu](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Po vytvoření OM zásuvné **Nastavení** se zobrazí, jak je ukázáno v následujícím příkladu

    ![Azure portálu - vytvořit veřejnou IP adresu](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)