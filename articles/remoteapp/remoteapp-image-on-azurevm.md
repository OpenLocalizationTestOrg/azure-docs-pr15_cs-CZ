<properties
    pageTitle="Vytvořit obrázek Azure RemoteApp založené na Azure OM | Microsoft Azure"
    description="Naučte se vytvořit obrázek pro Azure RemoteApp pomocí Azure virtuálního počítače."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Vytvoření obrázku Azure RemoteApp založené na Azure virtuálního počítače

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Vytvoření Azure RemoteApp obrázky, (které podržte aplikace, které sdílíte ve vaší kolekci) z Azure virtuálního počítače. Může taky můžete použít obrázek virtuální počítač, který je teď nově přidaná do Galerie obrázků Azure OM, který vyhovuje všem Azure RemoteApp obrázek – můžete tento obrázek OM jako výchozí bod pro vlastní OM, chcete-li. Hledejte ikonu "Windows Server hostitele relací vzdálené plochy" v knihovně.

Existují dva kroky vytvořit vlastní obrázek založené na Azure OM - vytvořit obrázek a pak ho nahrajte z Azure OM knihovny do Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Vytvoření vlastního obrázku založené na Azure OM

Pomocí těchto kroků k vytvoření obrázku založené na Azure OM.

1. Vytvoření Azure virtuálního počítače. Můžete "Windows Server hostitele relací vzdálené plochy" nebo "Windows Server vzdálené plochy relace hostitele s Microsoft Office 365 ProPlus" obrázek z Galerie obrázků Azure virtuálního počítače. Tento obrázek vyhovuje všem Azure RemoteApp šablony obrázek.

    Podrobnosti najdete v tématu [Vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Připojení k OM a nainstalujte a nakonfigurujte aplikace, které chcete sdílet prostřednictvím RemoteApp. Zkontrolujte, že proveďte všechny další konfigurace Windows vyžadované aplikace.

    Další informace najdete v tématu [přihlášení k virtuálního počítače spuštění systému Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Pokud používáte některou z obrázků hostitel vzdálené plochy relace systému Windows Server, je však započítávány ověření skript, který zajistí, že vaše OM splňuje RemoteApp pre-reqs. Skript spustit, poklikejte na **ValidateRemoteAppImage** na stolním počítači. Ujistěte se, že všechny chyby vykázaného skriptem řeší teprve potom přejděte k dalšímu kroku.

4. SYSPREP generalize a zachytit. Zjistěte, [jak zachytit virtuálního počítače Windows chcete použít jako šablonu](../virtual-machines/virtual-machines-windows-classic-capture-image.md) pokyny.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Import obrázku do knihovny obrázků Azure RemoteApp

Obrázek novinky naimportujte Azure RemoteApp pomocí těchto kroků:

1. Na kartě **Obrázky šablon** :
    - Pokud máte žádné existující obrázky, klikněte na **Nahrát nebo importovat obrázek šablony**.
    - Pokud už máte alespoň jeden obrázek, klikněte na **+** přidat nový obrázek.

2. Vyberte knihovnu **importu obrázek z virtuálních počítačích** a klikněte na tlačítko **Další**.

3. Na další stránce vyberte vlastní obrázek ze seznamu a potvrďte, že jste postupovali podle kroků uvedených při vytvoření obrázek. Klikněte na tlačítko **Další**.
4. Zadejte název nového RemoteApp obrázek a vyberte umístění a potom na zaškrtnutí spusťte proces importu.

> [AZURE.NOTE] Importovat obrázky odkudkoli Azure podporované ve virtuálních počítačích Azure kamkoli Azure nepodporuje Azure RemoteApp. V závislosti na umístění importu může trvat až 25.

Teď jste připraveni k vytvoření nové kolekce, buď [cloudu](remoteapp-create-cloud-deployment.md) kolekce nebo [hybridní](remoteapp-create-hybrid-deployment.md)podle svých potřeb.
