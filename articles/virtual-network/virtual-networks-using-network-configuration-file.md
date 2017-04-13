<properties 
    pageTitle="Konfigurace virtuální sítě pomocí konfiguračního souboru sítě" 
    description="Pokyny pro export a import souboru konfigurace sítě k portálu Správa Azure k vytvoření nebo úprava virtuální sítě. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Konfigurace virtuální sítě pomocí konfiguračního souboru sítě

Virtuální sítě (VNet) můžete nakonfigurovat tak, že na portálu Správa Azure nebo pomocí konfiguračního souboru sítě.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Vytváření a úpravy souboru konfigurace sítě 
Nejjednodušší způsob, jak vytvářet konfiguračního souboru sítě je export nastavení sítě z existující konfigurace virtuální sítě, změňte soubor obsahující nastavení, které chcete konfigurovat virtuálních sítí.

Upravit soubor konfigurace sítě, můžete jednoduše otevřete soubor, proveďte požadované změny a potom uložit. Použití editoru *xml* ke změnám souboru konfigurace sítě. 

Úzce řiďte se pokyny pro [Nastavení sítě konfiguračního souboru schématu](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure byly použity podsítě, která obsahuje něco používaný jako **používá**. Když podsítě se používá, nelze změnit. Před změnou, přesuňte všechno, co jste nainstalovali podsítě jiné podsítě, která se nemění.   Najdete v článku [Přesunutí OM nebo instanci rolí jiné podsítě](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Export a import nastavení virtuální sítě pomocí portálu pro správu  
Můžete import a export nastavení konfigurace sítě obsažených v souboru konfigurace sítě pomocí prostředí PowerShell nebo portálu pro správu. Následující pokyny vám pomůže, export a import pomocí portálu pro správu. 

### <a name="to-export-your-network-settings"></a>Export nastavení sítě
Při exportu, všechna nastavení virtuálních sítí ve vašem předplatném zapíše do XML souboru. 

1. Přihlaste se k **portálu Správa**.
2. Na portálu správy v dolní části stránky **sítě** klikněte na **Exportovat**. 
3. V okně **Export konfigurace sítě** ověřte, zda jste vybrali předplatného, u kterého chcete exportovat nastavení sítě. Potom klikněte na zaškrtnutí v pravé dolní. 
4. Po zobrazení výzvy, uložte soubor *NetworkConfig.xml* do umístění podle svého výběru.


### <a name="to-import-your-network-settings"></a>Import nastavení sítě

1. Na **Portálu Správa**, v navigačním podokně na levém dolním rohu klepněte na **Nový**.
2. Klikněte na **síť služby** -> **virtuální sítě** -> **Import konfigurace**.
3. Na stránce **Importovat soubor konfigurace sítě** najděte soubor konfigurace sítě a potom klikněte na **Další** šipky.
4. Na stránce **vytváření sítě** zobrazí se informace na obrazovce s které části konfiguraci sítě se změny nebo vytvoření. Pokud pro vás správný vypadají změny, klikněte na zaškrtnutí a přejděte na aktualizovat nebo vytvořit virtuální sítě. 