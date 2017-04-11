<properties
    pageTitle="Nasazení šablon aplikace Visual Studio ve vrstvě Azure | Microsoft Azure"
    description="Informace o nasazení šablon aplikace Visual Studio ve vrstvě Azure."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Nasazení šablon ve vrstvě Azure pomocí aplikace Visual Studio

Pomocí aplikace Visual Studio nasazení šablon správce prostředků Azure do zásobníku Koncepce Azure.

Správce prostředků šablony nasazení a zřízení všechny zdroje pro aplikaci v operaci jediné, koordinovaný.

1.  Otevřete aplikaci Visual Studio 2015 aktualizace 1 pro.

2.  Klikněte na **soubor**, klikněte na **Nový**a v dialogovém okně **Nový projekt** klikněte na **Pole Skupina zdroje Azure**.

3.  Zadejte **název** nového projektu a potom klikněte na **OK**.

4.  V dialogovém okně **Vybrat šablonu Azure** klikněte na **Windows virtuálního počítače**a potom klikněte na **OK**.

  V nové projektu zobrazí se seznam dostupných šablon rozbalením uzel **šablony** v podokně **Průzkumník řešení** .

5.  V podokně **Průzkumník řešení** klikněte pravým tlačítkem myši na název projektu, klikněte na **nasazení**a pak klikněte na **Nový nasazení**.

6.  V dialogovém okně **nasadit do pole Skupina zdroje** v rozevíracím seznamu **předplatné** vyberte předplatné Microsoft Azure vrstvě.

7.  V seznamu **Pole Skupina zdroje** vyberte existující skupiny zdrojů nebo vytvořte nový účet.

8.  V seznamu **zdrojů skupina umístění** vyberte umístění a klikněte na **nasazení**.

9.  V dialogovém okně **Upravit parametry** zadejte hodnoty parametrů (které se liší podle šablony) a potom klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky

[Nasazení šablony s příkazového řádku](azure-stack-deploy-template-command-line.md)
