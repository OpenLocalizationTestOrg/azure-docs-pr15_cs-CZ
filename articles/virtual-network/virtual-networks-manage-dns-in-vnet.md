<properties 
   pageTitle="Správa servery DNS používané virtuální sítě (VNet)"
   description="Naučte se přidávat a odebírat servery DNS v síti virtuální (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Správa servery DNS používané virtuální sítě (VNet)

Můžete spravovat seznam serverů DNS v VNet v portálu pro správu nebo v souboru konfigurace sítě. Pro každý VNet můžete přidat až 12 serverů DNS. Při určování servery DNS, je důležité ověřit seznam serverů DNS ve správném pořadí ve vašem prostředí. Seznamy serveru DNS nefungují kruhového. Používají se v pořadí, aby byla vyplněná. Pokud první server DNS v seznamu je možné získat přístup na stránku, klient použít tento server DNS bez ohledu na to, zda DNS server funguje správně nebo ne. Pokud chcete změnit pořadí serveru DNS pro vaši virtuální síť, odebrat servery DNS v seznamu a přidejte zpět v pořadí, ve kterém chcete.

>[AZURE.WARNING] Po aktualizaci seznamu DNS, musíte restartovat virtuálních počítačích umístěné v síti virtuální tak, že vystopovat nové nastavení serveru DNS. Virtuálních počítačích bude dál používat aktuální konfigurace, dokud se nerestartuje.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Úprava seznamu serveru DNS pro virtuální sítě pomocí portálu pro správu

1. Přihlaste se k **portálu Správa**.

1. V navigačním podokně klikněte na **sítě**a potom klikněte na název sítě virtuální ve sloupci **název** .

1. Klikněte na **Konfigurovat**.

1. V části **Servery DNS**můžete nakonfigurovat následující:

    - **Zaregistrujte (Přidat) nový DNS server –** Jednoduše napište do polí název a IP adresu. Tím přidáte serveru DNS k síti virtuální servery DNS seznamu a taky registruje DNS server Azure.

    - **Chcete-li přidat serveru DNS, která byla dříve registrované –** Pokud jste už zaregistrovali DNS serveru s Azure, můžete vybírat z předem vyplněné seznamu.

    - **Chcete-li odebrat serveru DNS z virtuální sítě –** Klikněte na X vedle server, ke kterému chcete odebrat. Poznámka: Toto jenom odeberete serveru z tohoto seznamu virtuální sítě. DNS server zůstane registrovaných v Azure vaší virtuálních sítí používat. Chcete-li odstranit serveru DNS u vašeho předplatného, přejděte na stránku **sítí -> serverů DNS** .

    - **Chcete-li změnit pořadí serverů DNS –** Odebrání všech serverů DNS, které jsou uvedené a potom je přidat se změnami v pořadí, ve kterém chcete. Mějte na paměti, že se nejedná seznam kruhového DNS.

    - **Chcete-li přejmenovat DNS server –** Zvýraznění DNS server v seznamu a potom zadejte nový název. Tím se registrace nového serveru DNS v Azure, jakož i ho přidat do seznamu serverů DNS sítě virtuální. Staré DNS server a jeho IP adresa zůstane registrovaných s Azure. Můžete ho odstranit na stránce **DNS servery** Pokud nepoužíváte jiných virtuálních sítí.

1. Klepnutím na tlačítko **Uložit** v dolní části stránky uložte nové konfigurace servery DNS.

1. Restartujte virtuálních počítačích umístěn v síti virtuální povolení získat nové nastavení DNS.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Úprava seznamu DNS server pomocí konfiguračního souboru sítě

Upravovat seznam serverů DNS pomocí konfiguračního souboru sítě, budete nejdřív export nastavení konfigurace z portálu pro správu. Budete potom upravit soubor konfigurace sítě a importujte ho znovu pomocí portálu pro správu. Níže je uceleném popis jednotlivých kroků dokončete tento proces.

1. Virtuální síťová nastavení exportujte do souboru konfigurace sítě. Další informace a postup export nastavení konfigurace sítě najdete v tématu [Export virtuální síťová nastavení pro soubor konfigurace sítě](virtual-networks-using-network-configuration-file.md).

1. Zadejte informace o serveru DNS virtuální sítě. Další informace o zadávání serveru DNS naleznete v tématu [Nastavení serveru DNS v souboru konfigurace virtuální sítě](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Další informace o souborech konfigurace sítě najdete v článku [Azure virtuální schéma konfigurace sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx) a [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md).

1. Importujte souboru konfigurace sítě. Další informace a kroky k importu souboru konfigurace sítě najdete v článku [Import souboru konfigurace sítě](virtual-networks-using-network-configuration-file.md).

1. Restartujte virtuálních počítačích umístěn v síti virtuální povolení získat nové nastavení DNS.
