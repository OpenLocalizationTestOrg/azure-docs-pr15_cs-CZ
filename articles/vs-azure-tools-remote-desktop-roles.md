<properties 
   pageTitle="Pomocí vzdálené plochy s Azure role | Microsoft Azure"
   description="Pomocí vzdálené plochy s Azure role"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Pomocí vzdálené plochy s Azure role

Pomocí Azure SDK a Vzdálená plocha, dostanete se k Azure rolí a virtuálních počítačích, které jsou hostované pomocí Azure. Ve Visual Studiu můžete nakonfigurovat vzdálené plochy z Azure projektu. Povolit vzdálená plocha, musíte vytvořit pracovní projekt, který obsahuje jednu nebo více rolí a pak publikovat Azure.

>[AZURE.IMPORTANT] Měli byste přistupovat Azure role pro řešení potíží nebo vývoje pouze. Každé virtuálního počítače účel spuštění určitou roli v aplikaci Azure nespouštět jiné klientské aplikace. Pokud chcete používat Azure hostovat virtuální počítač, který můžete použít pro účely, najdete v článku přístup k Azure virtuálních počítačích z Průzkumníka serveru.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Povolit a používat Vzdálená plocha pro Azure Role

1. V Průzkumníku otevření místní nabídky pro váš projekt a potom vyberte **Publikovat**.

    Zobrazí se průvodce **Publikovat Azure aplikace** .

    ![Příkaz pro projekt cloudové služby publikování](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. V dolní části stránky **Nastavení publikování Microsoft Azure** v průvodci vyberte **Povolit vzdálená plocha** pro všechny zaškrtávací políčko role. 

    Zobrazí se dialogové okno **Konfigurace vzdálené plochy** .

1. V dolní části dialogu **Konfigurace vzdálené plochy** klikněte na tlačítko **Další možnosti** . 
 
    Zobrazí se rozevírací seznam, který umožňuje vytvořit nebo vybrat certifikát tak, aby informace o přihlašovacích údajích dá zašifrovat při připojení přes Vzdálená plocha.

1. V rozevíracím seznamu vyberte ** &lt;vytvořit >**, nebo si vyberte některý ze stávajících ze seznamu. 

    Pokud se rozhodnete stávající certifikát, přeskočte následující kroky.

    >[AZURE.NOTE] Certifikáty, které potřebujete pro připojení ke vzdálené ploše se liší od certifikáty, které používáte pro jiné Azure operace. Vzdálený přístup certifikát musí mít privátním klíčem.

    Zobrazí se dialogové okno **Vytvořit certifikát** .

    1. Zadejte popisný název pro nový certifikát a potom klikněte na tlačítko **OK** . Do pole rozevíracího seznamu se zobrazí nový certifikát.

    1. V dialogovém okně **Konfigurace vzdálené plochy** zadání uživatelského jména a hesla.
    
        Nelze použít existující účet. Správce není zadejte uživatelské jméno pro nový účet.

        >[AZURE.NOTE] Pokud heslo nesplňuje požadavky na složitost, zobrazí se červená ikona vedle textového pole heslo. Heslo musí obsahovat písmena velká, malá písmena a čísla nebo symboly.

    1. Zvolte datum účet vyprší a po které připojení ke vzdálené ploše, budou blokovány.

    1. Poté, co jste uvedli všechny požadované informace, klikněte na tlačítko **OK** .
    
        Několik nastavení, která povolení vzdáleného přístupu služby se přidají do .cscfg a .csdef soubory.

1. V průvodci **Nastavení publikování Microsoft Azure** klikněte na tlačítko **OK** , až budete připravení publikovat cloudové služby.

    Pokud si nejste připraveni publikovat, klikněte na tlačítko **Zrušit** . Konfigurace nastavení se uloží a publikováním cloudové služby později.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Připojení k roli Azure pomocí vzdálené plochy

Po publikování cloudové služby na Azure, můžete k přihlášení do virtuálních počítačích, která hostuje Azure Průzkumník serveru. 

1. V okně Průzkumník serveru rozbalte uzel **Azure** a potom rozbalte uzel pro Cloudová služba a jeden z jeho role zobrazíte seznam instancí.

1. Otevření místní nabídky pro instance uzel a vyberte **Připojit pomocí vzdálené plochy**.

    ![Připojení přes Vzdálená plocha](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Zadejte uživatelské jméno a heslo, které jste vytvořili v předchozích krocích. Teď jste přihlášeni vzdálenou relaci.


