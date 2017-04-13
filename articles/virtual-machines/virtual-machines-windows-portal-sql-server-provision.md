<properties
    pageTitle="Zřízení SQL Server virtuálního počítače | Microsoft Azure"
    description="Vytvoření a připojte k počítači virtuálního serveru SQL Server ve Azure na portálu. Tento kurz používá režim správce prostředků."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Zřízení virtuálního počítače serveru SQL Server Azure portálu

> [AZURE.SELECTOR]
- [Portál](virtual-machines-windows-portal-sql-server-provision.md)
- [Prostředí PowerShell](virtual-machines-windows-ps-sql-create.md)

Tento kurz začátku do konce ukazuje, jak vytvořit virtuální počítač serverem SQL pomocí portálu Azure.

Galerie Azure virtuálního počítače (OM) obsahuje několik obrázků, které obsahují Microsoft SQL Server. Několika kliknutími můžete vybírat z SQL OM obrázky z galerie a zřízení v prostředí Azure.

V tomto kurzu udělejte toto:

- [Výběr SQL OM obrázek z Galerie](#select-a-sql-vm-image-from-the-gallery)
- [Konfigurace a vytvořte OM](#configure-the-vm)
- [Otevřete OM Vzdálená plocha](#open-the-vm-with-remote-desktop)
- [Připojení k serveru SQL Server vzdáleně](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Výběr SQL OM obrázek z Galerie

1. Přihlaste se k [portálu Azure](https://portal.azure.com) pomocí svého účtu.

    >[AZURE.NOTE] Pokud nemáte účet Azure, navštěvujte blog o [Azure bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

1. Na portálu Azure klikněte na **Nový**. Na portálu otevře **Nový** zásuvné. Jsou zdroje OM SQL Server ve **virtuálních počítačích** skupině tržiště.

1. V zásuvné **Nový** klikněte na **virtuálních počítačích**.

1. Pokud chcete zobrazit všechny dostupné obrázky, klikněte na **Zobrazit všechny** na zásuvné **virtuálních počítačích** .

    ![Zásuvné Azure virtuálních počítačích](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. V části **databázový server**klikněte na **Serveru SQL Server**. Může být potřeba se posunout dolů vyhledejte **databázový server**. Prohlédněte dostupné šablony SQL serveru.

    ![Galerie virtuálního počítače SQL obrázky](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Každá šablona identifikuje verze serveru SQL Server a operačního systému. Vyberte jednu z těchto obrázků ze seznamu. Podívejte se na zásuvné podrobnosti, s popisem obrázek virtuálního počítače.

    >[AZURE.NOTE] Obrázky SQL OM zahrnout do ceny za minutu OM vytvoříte licencování nákladů pro systém SQL Server. Je další možnost přenést svůj vlastní licence (BYOL) a platovou jenom pro OM. Názvy obrázek jsou předponou {BYOL}. Další informace o tuto možnost najdete v článku [Začínáme se serverem SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).

1. V části **Vyberte model nasazení**zkontrolujte, že je zaškrtnuté **Správce prostředků** . Správce prostředků je doporučené nasazení modelu doplňku pro nové virtuálních počítačích. Klikněte na **vytvořit**.

    ![Vytvoření OM SQL pomocí Správce prostředků](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Konfigurace OM
Existuje pět listy konfigurace systému SQL Server virtuálního počítače.

| Krok               | Popis                          |
|---------------------|-------------------------------|
| **Základní informace**              | [Základní nastavení](#1-configure-basic-settings)      |
| **Velikost**                | [Výběr velikosti virtuálního počítače](#2-choose-virtual-machine-size)   |
| **Nastavení**            | [Konfigurace volitelné funkce](#3-configure-optional-features)   |
| **Nastavení serveru SQL** | [Konfigurace nastavení serveru SQL](#4-configure-sql-server-settings) |
| **Souhrn**             | [Prohlédněte si souhrn](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. základní nastavení konfigurace
Na zásuvné **základní informace o** poskytnutí následujících údajů:

* Zadejte jedinečný **název**virtuální počítač.
* Zadejte **uživatelské jméno** místního účtu správce na OM. Tento účet se taky přidá do **členem** role serveru SQL Server.
* Zadání silné **heslo**.
* Pokud máte víc předplatných, ověřte správnost předplatné pro novou OM.
* V dialogovém okně **pole Skupina zdroje** zadejte název nové skupiny prostředků. Můžete taky použít existující zdroj skupiny klikněte na **Vybrat stávající**. Skupina zdroje je kolekce související materiály v Azure (virtuálních počítačích úložiště účty, virtuálních sítí, atd.).

    >[AZURE.NOTE] Použití nové skupiny prostředků je užitečné, pokud jsou jenom testování nebo získání informací o nasazení serveru SQL Server v Azure. Po dokončení se test odstraňte zdroje a automaticky odstraní OM a všechny zdroje přidružený k této skupině zdrojů. Další informace o skupinách prostředků najdete v článku [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

* Vyberte **umístění** pro toto nasazení.
* Klikněte na **OK** uložte nastavení.

    ![Základní informace o zásuvné SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Zvolte velikost virtuálního počítače
V kroku **velikost** v zásuvné **Zvolte velikost** zvolte velikost virtuálního počítače. Zásuvné původně zobrazí doporučené počítače velikosti založené na šabloně, kterou jste vybrali. Vypočte taky měsíční náklady spustit OM.

![Možnosti formátu OM SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Pro výrobní úloh doporučujeme vybrat velikostí virtuální počítač, který podporuje [Premium úložiště](../storage/storage-premium-storage.md). Pokud není potřeba této úrovni výkonu, použijte tlačítko **Zobrazit vše** , které zobrazuje všechny možnosti velikost počítače. Například pro vývoj nebo testovacím prostředí používáte zmenšené počítače.

>[AZURE.NOTE] Další informace o virtuální počítač velikosti zobrazíte [velikosti virtuálních počítačích](virtual-machines-windows-sizes.md). Důležité informace o velikosti SQL Server OM najdete v článku [výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

Zvolte velikosti vašeho počítače a potom klikněte na **Výběr**.

## <a name="3-configure-optional-features"></a>3. volitelné funkce nakonfigurovat
Na zásuvné **Nastavení** konfigurace Azure úložiště, sítě a sledování virtuálního počítače.

- Určete v části **úložiště** **disku napište** standardní nebo Premium (SSD). Úložiště Premium doporučujeme pracovního vytížení výroby.

>[AZURE.NOTE] Pokud vyberete Premium (SSD) pro počítač velikost, která nepodporuje Premium úložiště, automaticky změní velikosti vašeho počítače.  

- V části **účet úložiště**můžete přijmout název účtu automaticky zřizování úložiště. Můžete taky kliknout na **úložiště účtu** , a zvolte existujícího účtu a nakonfigurujte typ účtu úložiště. Ve výchozím nastavení Azure vytvoří nový účet úložiště s místně nadbytečné úložiště. Další informace o možnostech úložiště najdete v článku [replikace Azure úložiště](../storage/storage-redundancy.md).

- V části **síť**můžete přijmout automaticky vyplněnými hodnotami. Můžete taky kliknout na každou funkcí ruční konfigurace **virtuální sítě**, **podsítě**, **veřejnou IP adresu**a **Skupiny zabezpečení sítě**. Pro účely tohoto kurzu nechte výchozí hodnoty.

- Azure umožňuje **Sledování** ve výchozím nastavení se stejným účtem úložiště určenou pro OM. Můžete změnit následující nastavení.

- Ve skupinovém rámečku, **Nastavte dostupnost**určete sady dostupnosti. Pro účely tohoto kurzu můžete vybrat **možnost žádné**. Pokud budete chtít nastavit SQL AlwaysOn dostupnost skupiny, konfigurace dostupnosti předejít opětovnému vytvoření virtuálního počítače.  Další informace najdete v tématu [Správa dostupnosti virtuálních počítačích](virtual-machines-windows-manage-availability.md).

Po dokončení konfigurace nastavení, klikněte na **OK**.

## <a name="4-configure-sql-server-settings"></a>4. konfigurovat nastavení serveru SQL
Na zásuvné **Nastavení serveru SQL Server** konfigurovat nastavení a optimalizace pro systém SQL Server. Nastavení, které můžete konfigurovat pro systém SQL Server patří.

| Nastavení               |
|---------------------|
| [Připojení](#connectivity)              |
| [Ověřování](#authentication)                |
| [Konfigurace úložiště](#storage-configuration)            |
| [Automatické opravy](#automated-patching) |
| [Automatické zálohování](#automated-backup)             |
| [Integrace Azure klíčové trezoru](#azure-key-vault-integration)             |
| [R služby](#r-services) |

### <a name="connectivity"></a>Připojení
V části **připojení k SQL**zadejte typ přístupu, kterou chcete instanci systému SQL Server na tento OM. Pro účely tohoto kurzu vyberte **veřejné (internet)** aby umožňoval připojení k serveru SQL Server z počítače nebo služby na Internetu. Tato možnost vybrána Azure automaticky nakonfiguruje brány firewall a skupiny zabezpečení sítě přenosy na port 1433.  

![Možnosti připojení k SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Připojení k serveru SQL Server prostřednictvím Internetu, je potřeba povolit ověřování serveru SQL Server, který je popsaný v další části.

>[AZURE.NOTE] Je možné přidat další omezení pro komunikaci síti vaší OM SQL Server. Lze provést po vytvoření OM při úpravě skupiny zabezpečení sítě. Další informace najdete v tématu [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md)

Pokud chcete povolit připojení k databázový stroj prostřednictvím Internetu, zvolte jednu z následujících možností:

- **Místní (uvnitř pouze OM)** povolit připojení k serveru SQL pouze z OM.
- **Soukromé (v rámci virtuální síť)** aby umožňoval připojení z počítače nebo služby ve stejné síti virtuální SQL Server.

>[AZURE.NOTE] Obrázek virtuálního počítače pro SQL Server Express edition neumožňuje automaticky protokol TCP/IP. To platí to i v případě veřejných a soukromých možností připojení. Pro edici Express musíte pomocí Správce konfigurace systému SQL Server [protokol TCP/IP můžete ručně povolit](#configure-sql-server-to-listen-on-the-tcp-protocol) po vytvoření OM.

Obecně zvýšení zabezpečení výběrem nejvíce omezující připojení, který umožňuje nefunguje. Ale všechny možnosti zabezpečitelné pomocí pravidla skupiny zabezpečení sítě a ověření SQL/Windows.

Výchozí **port** 1433. Můžete použít jiné číslo portu.
Další informace najdete v tématu [připojit k SQL serveru virtuálního počítače (Správce zdrojů) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Ověřování
Pokud požadujete ověřování serveru SQL Server, klepněte na **Povolit** **ověřování serveru SQL**.

![Ověřování serveru SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Pokud máte v plánu pro přístup k serveru SQL Server přes internet (tedy možnost připojení k veřejné službě), je nutné povolit tady ověřování serveru SQL. Veřejný přístup k serveru SQL Server vyžaduje použití ověřování serveru SQL.

Pokud povolíte ověřování serveru SQL Server, zadejte **uživatelské jméno** a **heslo**. Uživatelské jméno je nakonfigurování jako členem **členem** pevnou role serveru a přihlašovací ověřování serveru SQL Server. Další informace o režimy ověřování najdete v článku [Volba režim ověřování](http://msdn.microsoft.com/library/ms144284.aspx) .

Pokud nepovolíte ověřování serveru SQL Server, můžete pomocí místního účtu správce na OM se připojit k instanci systému SQL Server.

### <a name="storage-configuration"></a>Konfigurace úložiště
Klikněte na **úložiště konfigurace** můžete určit požadavky na úložiště.

![Konfigurace úložiště SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Pokud vyberete možnost standardní úložiště, tato možnost není k dispozici. Automatické ukládání optimalizace je dostupný jenom u Premium úložiště.

Požadavky na můžete zadat jako vstupní a výstupní operace za sekundu (procesorů) výkon MB/s a velikost celkové úložiště. Konfigurace tyto hodnoty pomocí stupnice posuvné. Na portálu automaticky vypočítá počet disků tyto požadavkům.

Ve výchozím nastavení Azure optimalizuje úložiště pro 5000 procesorů, 200 MB a 1 TB úložiště. Můžete změnit nastavení úložiště podle zátěží na projektu. V části **úložiště optimalizovaná pro**vyberte jednu z následujících možností:

- **Obecné** je ve výchozím nastavení a podporuje většina úloh.
- Zpracování **transakční** Optimalizuje úložný prostor pro tradiční databáze OLTP úloh.
- **Datové sklady** Optimalizuje úložný prostor pro vytváření sestav a analýze úloh.

>[AZURE.NOTE] Horní omezení posuvníků lišit podle toho velikost vybraný virtuální počítač.

### <a name="automated-patching"></a>Automatické opravy
**Automatické opravy** aktivované ve výchozím nastavení. Automatická oprava umožňuje Azure automaticky opravovat SQL Server a operačního systému. Zadejte den v týdnu, čas a doba trvání údržby. Azure provede opravy chyb v tomto okně údržbu. Okno plán údržby používá národní prostředí OM přihlášení. Pokud nechcete, aby Azure automaticky opravovat SQL Server a operačního systému, klikněte na **Zakázat**.  

![SQL automatické opravy](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Další informace najdete v tématu [Automatické opravy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatické zálohování
Povolte automatické zálohování databází pro všechny databáze v části **Automatické zálohování**. Automatické zálohování je ve výchozím nastavení vypnutá.

Pokud zapnete automatické zálohování SQL, můžete nakonfigurovat následující:

- Doba uchovávání informací (dní) pro zálohování
- Úložiště účet, který chcete použít pro zálohování
- Šifrování a hesla pro zálohování

Šifrování zálohování, klikněte na **Povolit**. Zadejte **heslo**. Azure vytvoří certifikát šifrování zálohy a používá zadaného hesla k ochraně certifikátu.

![SQL automatické zálohování](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Další informace najdete v tématu [Automatické zálohování pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure integrace trezoru klíč
Ukládat tajemství zabezpečení v Azure šifrování, klikněte na **Azure klíčové trezoru integrace** a klikněte na **Povolit**.

![Integrace SQL Azure klíčové trezoru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

V následující tabulce jsou uvedeny parametrů požadovaných nakonfigurovat integraci trezoru klíč Azure.

|PARAMETR|POPIS|PŘÍKLAD|
|----------|----------|-------|
|**Adresa URL klíčové trezoru** |Umístění klíčové trezoru.|https://contosokeyvault.Vault.Azure.NET/ |
|**Hlavní název** |Azure Active Directory hlavního názvu služby. Tento název se taky nazývá ID klienta.  |fde2b411 33d 5-4e11-af04eb07b669ccf2|
| **Hlavní tajná**|Azure Active Directory služby základní tajná. Tento tajná se taky nazývá tajná klienta. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Název přihlašovacích údajů**|**Název pověření**: integrace AKV vytvoří pověření v rámci SQL serveru, umožní OM mít taky přístup k klíčové trezoru. Vyberte název pro tento přihlašovací údaje.| mycred1|

Další informace najdete v tématu [Konfigurace integrace Azure klíč pro systém SQL Server na Azure VMs trezoru](virtual-machines-windows-ps-sql-keyvault.md).

Po dokončení konfigurace serveru SQL Server nastavení klikněte na **OK**.

### <a name="r-services"></a>R služby
Pro SQL Server 2016 Enterprise edition máte možnost Povolit [R služby SQL Server](https://msdn.microsoft.com/library/mt604845.aspx). Umožňuje pokročilé technologie pro analýzu pomocí služby SQL Server 2016. Zaškrtněte políčko **Povolit** na zásuvné **Nastavení serveru SQL** .

![Povolení služeb SQL Server R](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Možnost pro povolení služby R je vypnutá, pro SQL Server obrázky, které nejsou 2016 Enterprise edition.

## <a name="5-review-the-summary"></a>5. Zkontrolujte souhrnné
Na **souhrnné** zásuvné zkontrolujte souhrnné a klikněte na **OK** vytvořte SQL Server, pole Skupina zdroje a určeným pro tento OM zdroje.

Můžete sledovat nasazení z portálu Microsoft azure. Tlačítko **oznámení** v horní části obrazovky se zobrazí základní stav nasazení.

>[AZURE.NOTE] Abyste měli přehled o na časy nasazení, můžu používaný SQL OM oblasti východoasijských USA s výchozím nastavením. Toto zkušební nasazení trvala celkem 26 minut. Ale může dojít rychlejší nebo pomaleji nasazení čas založený na vaší oblasti a vybrali nastavení.

## <a name="open-the-vm-with-remote-desktop"></a>Otevřete OM Vzdálená plocha

Připojení k virtuálního počítače pomocí vzdálené plochy pomocí následujících kroků:

1. Po OM Azure, se zobrazí ikona OM Azure řídicího panelu. Můžete taky nalezne pomocí procházení existující virtuálních počítačích. Klikněte na novém počítači virtuální SQL. **Virtuální počítač** zásuvné zobrazí podrobnosti o virtuálního počítače.
1. V horní části zásuvné **virtuálního počítače** klikněte na **Připojit**.
1. V prohlížeči soubory ke stažení souboru RDP pro OM. Otevřete soubor RDP.
    ![Vzdálená plocha OM SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Připojení ke vzdálené ploše s upozorněním, že aplikace publisher toto připojení ke vzdálené nerozpoznala. Klikněte na **Připojit** k pokračovat.
1. V dialogovém okně **Zabezpečení systému Windows** klikněte na **použít jiný účet**.
1. Pro typ **uživatelské jméno** ** \<uživatelské jméno >**, kde <user name> je uživatelské jméno, které jste zadali při konfiguraci OM. Budete muset přidat počáteční zpětné lomítko před názvem.
1. Zadejte **heslo** , která odrážejí dříve konfigurované pro tento OM a potom klikněte na **OK** se připojte.
1. Pokud jiný dialogové okno **Připojení ke vzdálené ploše** zeptá, jestli se chcete připojit, klikněte na **Ano**.

Po připojení k serveru SQL Server virtuálního počítače můžete spustit SQL Server Management Studio a připojit pomocí ověřování Windows pomocí svých přihlašovacích údajů místního správce. Pokud jste povolili ověřování serveru SQL Server, můžete také připojit pomocí ověřování serveru SQL pomocí SQL přihlašovací jméno a heslo, které jste nakonfigurovali během vytváření.

Přístup k počítači umožňuje přímo změnit počítače a nastavení serveru SQL Server svým požadavkům. Například může konfigurovat nastavení brány firewall nebo změnili jste nastavení konfigurace systému SQL Server.

## <a name="connect-to-sql-server-remotely"></a>Připojení k serveru SQL Server vzdáleně

V tomto kurzu budeme vybraná **veřejné** přístupu virtuálního počítače a **Ověřování serveru SQL Server**. Automaticky se nakonfigurováno virtuálního počítače, aby umožňoval připojení SQL serveru z libovolného klienta přes internet (za předpokladu, že mají správné přihlašovací SQL).

>[AZURE.NOTE] Pokud jste nezaškrtli políčko veřejné během vytváření, dodatečné kroky potřebné pro přístup k instanci systému SQL Server přes internet. Další informace najdete v tématu [připojení k SQL serveru virtuálního počítače v](virtual-machines-windows-sql-connect.md).

Následující části ukazují, jak připojit k instanci systému SQL Server na vaše OM z jiného počítače přes internet.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky
Další informace o použití serveru SQL Server Azure najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md) a [Nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).

Videa přehled serveru SQL Server na virtuálních počítačích Azure podívejte se na [Azure OM je nejlepší platformu pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Prozkoumání Naučná stezka](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro systém SQL Server na virtuálních počítačích Azure.
