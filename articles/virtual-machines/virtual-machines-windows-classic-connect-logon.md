<properties
    pageTitle="Přihlásit se k classic Azure VM | Microsoft Azure"
    description="Přihlášení k virtuálnímu počítači systému Windows vytvořené pomocí modelu classic nasazení pomocí portálu Azure klasické."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Přihlaste se k systému Windows virtuálního počítače pomocí portálu Azure classic

V klasické portálu Azure použijte tlačítko **Připojit** ke spuštění relace vzdálené plochy a přihlaste se k systému Windows virtuálního počítače.

Chcete se připojit k Linux VM? V tématu [jak se přihlásit do virtuálního počítače systémem Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Zjistěte, jak [provést tyto kroky, pomocí nového portálu Azure](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video návod

Zde je video návodu kroky v tomto výukovém programu. Zahrnuje také koncové body a veřejné a privátní porty používané pro připojení k systému Windows virtuálního počítače v Azure.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Připojení k virtuálnímu počítači

1. Přihlaste se k portálu Azure klasické.

2. Klepněte na **virtuální počítače**a potom vyberte virtuální počítač.

3. V řádku nabídek v dolní části stránky klepněte na tlačítko **Připojit**.

    ![Přihlaste se k virtuálnímu počítači](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Pokud není k dispozici tlačítko **Připojit** , naleznete v tématu Poradce při potížích s tipy na konci tohoto článku.

## <a name="log-on-to-the-virtual-machine"></a>Přihlaste se k virtuálnímu počítači

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Další kroky

-   Pokud není aktivní tlačítko **Připojit** nebo máte jiné potíže s připojením ke vzdálené ploše, zkuste obnovit konfiguraci. Z řídicího panelu virtuální počítač v části **Získat rychlý přehled**, klepněte na tlačítko **Obnovit vzdálené konfigurace**.
-   Problémy s heslem zkuste jej. Z řídicího panelu virtuální počítač pod **Získat rychlý přehled**, klepněte na tlačítko **Obnovit heslo**.

Pokud tyto tipy nejsou k dispozici nebo nejsou, je třeba, naleznete v tématu [Poradce při potížích připojení ke vzdálené ploše pro systém Windows Azure virtuálního počítače](virtual-machines-windows-troubleshoot-rdp-connection.md). Tento článek provede vás Diagnostika a řešení běžných potíží.


