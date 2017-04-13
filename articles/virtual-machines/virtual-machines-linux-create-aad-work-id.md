<properties
   pageTitle="Vytvoření pracovního nebo školního identity v AAD | Microsoft Azure"
   description="Naučte se vytvářet pracovní nebo školní identity v Azure Active Directory pro práci s virtuálních počítačích Linux."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Vytváření pracovní nebo školní identity v Azure Active Directory pro práci s Linux VMs

Pokud jste vytvořili osobní účet Azure nebo osobní web MSDN pro předplatné a vytvořený účet Azure umožní využít výhod MSDN Azure přeplatky – používá identity *účet Microsoft* vytvořit. Mnoho funkcí skvělé z Azure – [šablony skupina zdroje](../azure-resource-manager/resource-group-overview.md) je příkladem – vyžadují pracovního nebo školního účtu (identitu spravuje Azure Active Directory) pro práci. Postupujte podle těchto pokynů k vytvoření nového pracovního nebo školního účtu, protože se naštěstí jedna nejlepší, co o svůj osobní účet Azure, že je součástí výchozí Azure Active Directory doménu, kterou můžete použít k vytvoření nového pracovního nebo školního účtu, využívající funkcemi Azure, které vyžadují ho.

Však posledních změn umožňují správu předplatného u všech ostatních účet Azure pomocí `azure login` interaktivní přihlášení metody popsané [v tomto poli](../xplat-cli-connect.md). Můžete použít buď tímto způsobem, nebo můžete postupovat podle pokynů, které následují. Můžete také [vytvořit pracovní nebo školní identity v Azure Active Directory pomocí Windows VMs](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
