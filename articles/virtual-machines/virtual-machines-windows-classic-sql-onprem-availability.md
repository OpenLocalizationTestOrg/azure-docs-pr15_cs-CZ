<properties
    pageTitle="Rozšíření místních vždy o skupinách dostupnost Azure | Microsoft Azure"
    description="Tento kurz využívá zdroje vytvořená pomocí klasické nasazení modelu a popisuje, jak pomocí Průvodce přidání otevřené v SQL Server Management Studio (SSMS) přidat vždy na skupiny dostupnosti otevřené v Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Rozšíření místních vždy o skupinách dostupnost Azure

Přidáním vedlejší repliky představují vždy na skupiny dostupnost dostupnost pro skupiny databáze. Povolit tyto repliky nedaří myši databází, když se nepovede. Kromě toho se mohou sloužit k převzít čtení úloh nebo úlohy zálohování.

Místní dostupnost skupiny můžete rozšířit na Microsoft Azure zřizování jeden nebo více VMs Azure se serverem SQL Server a potom jejich přidávání nastavili jako repliky skupinám dostupnost místní.

Tento kurz se předpokládá, že budete mít takto:

- Aktivní Azure předplatné. Můžete si [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- Existující vždy na dostupnost skupiny místních. Další informace o dostupnosti skupiny zobrazit [Vždy na skupiny dostupnosti](https://msdn.microsoft.com/library/hh510230.aspx).

- Propojení mezi místní síti a Azure virtuální sítě. Další informace o vytváření tento virtuální sítě najdete v článku [Konfigurace VPN k webu Azure klasické portálu](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Přidání průvodce Azure otevřené

V této části se dozvíte, jak pomocí **Průvodce přidáním otevřené Azure** rozšířit řešení vždy na skupiny dostupnosti zahrnout Azure repliky.

1. Z v SQL Server Management Studiuo rozbalte **Vždy na dostupnost** > **Skupiny dostupnosti** > **[název skupiny dostupnost]**.

1. Klikněte pravým tlačítkem myši **Repliky dostupnost**a potom klikněte na **Přidat otevřené**.

1. Ve výchozím nastavení se zobrazí **Přidat otevřené Průvodce dostupnost skupiny** . Klikněte na tlačítko **Další**.  Pokud jste vybrali **Nezobrazovat znovu na této stránce** možnosti v dolní části stránky během předchozí spuštění tohoto průvodce, nezobrazí se tato obrazovka.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Budete požádáni o připojení k ostatními existující sekundární. Kliknete na **připojit...** vedle každého otevřené nebo můžete kliknout na **Připojení všech...** v dolní části obrazovky. Po ověření klikněte na **Další** přejdete na další obrazovce.

1. Na stránce **Zadejte repliky** více karty jsou uvedené v horní části: **repliky**, **koncové body**, **Zálohování předvolby**a **posluchače**. Na kartě **repliky** klikněte na **Přidat... Azure otevřené** spuštění Průvodce přidáním otevřené Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Pokud jste nainstalovali jednu mezeru před vyberte stávající certifikát správy Azure místní úložiště certifikátů Windows. Vyberte nebo zadejte id Azure předplatného, pokud jste použili jednu mezeru před. Klikněte na stáhnout pro stažení a instalace certifikátu správy Azure a stáhněte si seznam předplatných pomocí účtu Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Naplní jednotlivá pole na stránce s hodnotami, které se použijí k vytvoření Azure virtuálního počítače (OM), který bude hostovat otevřené.

  	|Nastavení|Popis|
|---|---|
|**Obrázek**|Vyberte požadovanou kombinaci OS a SQL Server|
|**Velikost OM**|Vyberte požadovanou velikost OM, který nejlíp vyhovuje potřebám vaší organizace|
|**Název OM**|Zadejte jedinečný název pro nový OM. Název musí obsahovat 3 až 15 znaků, můžete obsahovat jenom písmena, číslice a pomlčky a musí začínat písmenem a končí písmeno nebo číslici.|
|**Uživatelské jméno OM**|Zadejte uživatelské jméno, které budou převedeny na účtu správce na OM|
|**Heslo správce OM**|Zadejte heslo pro nový účet|
|**Potvrďte heslo**|Potvrďte heslo nového účtu|
|**Virtuální sítě**|Určení Azure virtuální síť, která by měl použít nové OM. Další informace o virtuálních sítí najdete v článku [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md).|
|**Virtuální podsítě**|Určení podsítě virtuální sítě, která by měl použít nové OM|
|**Domény**|Ověřte správnost předem vyplněné hodnotu pro doménu|
|**Doména uživatelské jméno**|Zadat účet, který je v skupiny místních správců na místní uzlů|
|**Heslo**|Zadejte heslo pro uživatelské jméno domény|

1. Klikněte na **OK** potvrďte nastavení nasazení.

1. Právní podmínky se zobrazí vedle. Přečtěte si a pokud souhlasíte s termíny, klikněte na **OK** .

1. Znovu se zobrazí stránka **Zadejte repliky** . Zkontrolujte nastavení pro nové Azure otevřené na kartách **repliky** **koncové body**a **Zálohování předvolby** . Změnit nastavení požadavkům vaší firmy.  Další informace o parametry obsažené na těchto kartách najdete v článku [Určit repliky stránky (dostupnost Průvodce vytvořením skupiny průvodce nebo přidání otevřené)](https://msdn.microsoft.com/library/hh213088.aspx). Poznámka: nejde vytvořit posluchače pomocí karty posluchače dostupnost skupin, které obsahují Azure repliky. Kromě toho pokud posluchače již byly vytvořeny před spuštěním průvodce, zobrazí se zpráva s upozorněním, že není podporováno v Azure. Podíváme se na postup vytvoření posluchače v části **vytvoření posluchače dostupnost skupiny** .

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Klikněte na tlačítko **Další**.

1. Vyberte požadovanou metodu synchronizace dat, který chcete použít na stránce **Vyberte počáteční synchronizace dat** a klikněte na tlačítko **Další**. Pro většinu scénáře vyberte **Úplné synchronizaci Data**. Další informace o metody synchronizace dat najdete v článku [Vyberte počátečního Data synchronizace stránky (vždy na dostupnost skupiny průvodci)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Prohlédněte si výsledky na stránce **ověření** . Opravte zbývající problémy a v případě potřeby vygenerujte ověření. Klikněte na tlačítko **Další**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Zkontrolujte nastavení na stránce **Souhrn** a potom klikněte na tlačítko **Dokončit**.

1. Zřizovací proces, nebude zahájen. Po úspěšném dokončení průvodce klikněte na **Zavřít** ukončíte průvodce.

>[AZURE.NOTE] Průvodce přidáním otevřené Azure vytvoří soubor protokolu v Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Tento soubor protokolu mohou sloužit k odstraňování problémů s selhalo Azure otevřené nasazení. Pokud se Průvodce nezdaří, provádění všech akcí, všechny předchozí operace vráceny, včetně odstranění zřizování OM.

## <a name="create-an-availability-group-listener"></a>Vytvoření skupiny posluchače dostupnosti

Po vytvoření dostupnost skupiny je vhodné vytvořit posluchače pro klientům připojení replikám. Posluchače směrovat příchozí připojení k primární a sekundární otevřené jen pro čtení. Další informace o posluchače najdete v tématu [Konfigurace posluchače ILB skupin vždy na dostupnost v Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Další kroky

Kromě použití **Průvodce přidáním otevřené Azure** rozšířit vždy na dostupnost skupiny Azure, mohou také přesunete některých úloh systému SQL Server úplně Azure. Začněte tím, najdete v článku [vytváření SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
