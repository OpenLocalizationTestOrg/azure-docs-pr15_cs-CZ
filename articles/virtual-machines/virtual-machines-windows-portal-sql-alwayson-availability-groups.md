<properties
    pageTitle="Konfigurace vždy na skupiny dostupnosti v Azure OM automaticky – správce"
    description="Vytvoření skupiny dostupnost vždy na s Azure virtuálních počítačích v režimu správce prostředků Azure. Tento kurz primárně používá uživatelského rozhraní automaticky vytvořit celé řešení."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Konfigurace vždy na skupiny dostupnosti v Azure OM automaticky – správce

> [AZURE.SELECTOR]
- [Správce prostředků: šablony](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Správce prostředků: ruční](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasický: uživatelské rozhraní](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasický: prostředí PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Tento kurz začátku do konce ukazuje, jak vytvořit skupinu dostupnost serveru SQL Server s správce prostředků Azure virtuálních počítačích. Kurz používá ke konfiguraci šablony Azure listy. Kontrola výchozího nastavení, zadejte požadované nastavení a aktualizujte listy v portálu podle projděte si tento kurz.

Na konci kurzu SQL Server dostupnost skupiny řešení v Azure bude obsahovat tyto prvky:

- Virtuální sítě obsahující více podsítí, včetně front-end a back-end podsítě

- Dvě domény řadiče domény Active Directory (AD)

- Dva SQL Server VMs používaný podsítě back-end a připojení k doméně AD

- Shluk WSFC 3 uzel s modelem kvora Většina uzlů

- Skupiny dostupnosti s dvěma replikami synchronní potvrdit dostupnost databáze

Následující obrázek je grafické znázornění řešení.

![Testování laboratorní architektura pro AG v Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Všechny zdroje v tomto řešení patří do skupiny jeden zdroj.

Tento kurz předpokládá takto:

- Už máte účet Azure. Pokud nemáte jeden [zkušební účet](http://azure.microsoft.com/pricing/free-trial/).

- Znáte zřízení OM Server SQL z Galerie virtuálního počítače pomocí grafického uživatelského rozhraní. Další informace najdete v tématu [zřizování virtuálního počítače serveru SQL Server na Azure](virtual-machines-windows-portal-sql-server-provision.md)

- Už máte dobré porozumění dostupnost skupiny. Další informace najdete v tématu [vždy na skupiny dostupnosti (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Pokud vás zajímají pomocí dostupnost skupin služby SharePoint, taky v tématu [Konfigurace SQL Server 2012 vždy na skupiny dostupnost pro SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

V tomto kurzu se pomocí portálu Azure:

- Vyberte šabloně vždy na z portálu

- Zkontrolujte nastavení šablony a upravte několik nastavení prostředí

- Sledování Azure tak, jak se vytvoří celé prostředí

- Připojte jeden řadiče domény a klikněte na jednu z serverů SQL

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Zřízení obrázku z Galerie

Azure poskytuje Galerie aplikace celé řešení. Abyste mohli vyhledejte šablonu:

1.  Přihlaste se k portálu Azure pomocí svého účtu.
1.  Na kartě Azure portálu **+ Nový.** Na portálu se otevře nový zásuvné.
1.  Na nové hledání zásuvné **AlwaysOn**.
![Najděte šablonu AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Ve výsledcích hledání najděte **SQL Server AlwaysOn obrázku**.
![Šablona AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Na **Vyberte model nasazení** zvolte **Správce**.

### <a name="basics"></a>Základní informace

Klikněte na **základní informace** a nakonfigurujte tyto možnosti:

- **Správce uživatelské jméno** je uživatelský účet s oprávněními správce domény a členem členem role serveru SQL Server na obou instance serveru SQL Server. Tento kurz dosáhnete **DomainAdmin**.

- **Heslo** je heslo k účtu správce domény. Pomocí složitých hesla. Potvrďte heslo.

- **Předplatné** je předplatné, vám bude účtovat Azure provádět všechny používaný pro skupinu dostupnost zdroje. Pokud váš účet obsahuje víc předplatných, můžete si nastavit jiné předplatné.

- **Pole Skupina zdroje** je název pro skupinu všechny Azure zdroje vytvořil tento kurz se do kterých patříte. Tento kurz dosáhnete **SQL-HA-RG**. Další informace najdete v tématu (Přehled Správce prostředků Azure) [zdroje overview.md / # zdrojů – skupiny].

- **Umístění** je Azure oblasti, kde bude vytvořen materiály pro účely tohoto návodu. Vyberte Azure oblast hostovat infrastrukturu.

Tady je zásuvné **Základy** bude vypadat takto:

![Základní informace](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Klikněte na **OK**.

### <a name="domain-and-network-settings"></a>Nastavení sítě a domény

Tato šablona Azure Galerie vytvoří novou doménu s nové řadiče domény. Také vytvoří novou síť a dvě podsítí. Šablona neumožňuje vytváření servery v stávající doménu nebo virtuální sítě. Dalším krokem je pro nastavení domény a sítě.

Na zásuvné **Nastavení sítě a doménu** zkontrolujte přednastavených hodnot pro nastavení domény a sítě:

- **Název kořenové domény struktury** je název domény, který bude sloužit k AD doméně, která bude hostitelem clusteru. Kurz dosáhnete **contoso.com**.

- **Virtuální síťový název** je název sítě pro Azure virtuální sítě. Tento kurz dosáhnete **autohaVNET**.

- **Název domény řadiče podsítě** stejný název část virtuální sítě, který je hostitelem řadiče domény. Tento kurz dosáhnete **podsítě-1**. Tento podsítě budou používat adresu předponu **10.0.0.0/24**.

- **Název serveru SQL Server podsítě** stejný název část virtuální sítě hosts serverů SQL a soubor sdílet monitorovací. Tento kurz dosáhnete **podsítě-2**. Tento podsítě budou používat adresu předponu **10.0.1.0/26**.

Další informace o virtuální sítě [Azure najdete v článku Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md).  

**Nastavení domény a sítě** by měl vypadat takto:

![Nastavení sítě a domény](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

V případě potřeby můžete změnit tyto hodnoty. Pro účely tohoto návodu používáme přednastavených hodnot.

- Zkontrolujte nastavení a klikněte na **OK**.

###<a name="availability-group-settings"></a>nastavení skupiny dostupnosti

Na **Nastavení skupiny dostupnost** zkontrolujte přednastavených hodnot pro skupinu dostupnost a posluchače.

- **Název skupiny dostupnosti** je název skupinový zdroje pro skupiny dostupnosti. Tento kurz dosáhnete **Contoso-ag**.

- **název skupiny posluchače dostupnost** používá clusteru a vyrovnávání zatížení vnitřní. Připojení klientů k SQL serveru můžete použít tento název se připojit k odpovídající otevřené databázi. Tento kurz dosáhnete **Contoso posluchače**.

-  **dostupnost skupiny posluchače port** Určuje, že TCP port serveru SQL Server posluchače použije. Pro účely tohoto návodu použijte výchozí port **1433**.

V případě potřeby můžete změnit tyto hodnoty. Pro účely tohoto návodu pomocí přednastavených hodnot.  

![nastavení skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Klikněte na **OK**.

###<a name="vm-size-storage-settings"></a>OM velikost úložiště nastavení

Na **velikost OM, úložiště nastavení** zvolte velikost virtuálního počítače SQL serveru a zkontrolujte další nastavení.

- SQL Server virtuálního počítače odpovídá **velikost** Azure virtuálního počítače pro servery SQL. Vyberte vhodné pro vaše pracovní zátěž virtuálního počítače velikost. Pokud vytváříte použít toto prostředí pro kurz **DS2**. U pracovního vytížení výrobní vyberte velikost virtuálního počítače, který podporuje pracovní zátěž. Mnoho pracovního vytížení výrobní budou vyžadovat **DS4** nebo větší. Šablona bude vytvářet dvěma virtuálních počítačích této velikosti a nainstalovat SQL Server na každý z nich. Další informace najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure nainstaluje Enterprise Edition SQL Server. Náklady závisí na edici a velikost virtuálního počítače. Podrobné informace o aktuální náklady najdete v článku [ceny virtuálních počítačích](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- Domény řadiče domény virtuálního počítače odpovídá **velikost** virtuálního počítače pro řadiče domény. Tento kurz dosáhnete **D2**.

- Monitorovací sdílet soubor virtuálního počítače odpovídá **velikost** virtuálního počítače pro kopií souboru. Tento kurz dosáhnete **A1**.

- **Úložiště SQL účet** je název účtu úložiště pro ukládání data SQL serveru a operační systém discích. Tento kurz dosáhnete **alwaysonsql01**.

- **Účet Datacentrum úložiště** je název účtu úložiště u řadiče domény. Tento kurz dosáhnete **alwaysondc01**.

- **Data SQL serveru disk velikost** v TB je velikost disku serveru SQL Server data v TB. Zadejte číslo od 1 až 4. Toto je velikost dat disku, který se připojí ke každému SQL serveru. Tento kurz dosáhnete **1**.

- **Optimalizace úložiště** nastaví konkrétní úložiště nastavení pro SQL Server virtuálních počítačích v závislosti na typu zátěží na projektu. Všechny servery SQL v tomto scénáři pomocí premium úložiště Azure diskové mezipaměti hostitele nastavení jen pro čtení. Kromě toho můžete optimalizovat nastavení SQL serveru pro pracovní zátěž výběrem jedné z těchto tří nastavení:

    - **Obecné pracovní zátěž** nastaví žádné zvláštní nastavení

    - **Zpracování transakční** nastaví příznak trasování. 1117 a 1118

    - **Datové sklady** nastaví příznak trasování. 1117 a 610

Tento kurz dosáhnete **Obecné zátěží na projektu**.

![Nastavení velikosti úložiště OM](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Zkontrolujte nastavení a klikněte na **OK**.

####<a name="a-note-about-storage"></a>Upozornění o úložiště

Další optimalizace, závisí na velikosti discích data SQL serveru. Pro každý terabajt disku dat Azure přidá další úložiště premium 1 TB (SSD). Když server vyžaduje 2 TB nebo další, šabloně vytvoří fondu úložiště na každém SQL serveru. Fondu úložiště je formulář virtualizace úložiště, kde není nakonfigurován více disků poskytují vyšší výkon odolnost proti chybám a výkonu.  Šablona potom vytvoří prostoru úložiště ve fondu úložiště a nabídne vám to jako jednu datovou operační systém. Šablona označí tohoto disku jako disku dat pro systém SQL Server. Šablona vyladění fondu úložiště pro systém SQL Server s tímto nastavením:

- Velikost prokládání je nastavení proložení virtuální disk. Transakční úloh to je nastavena na 64 KB. Nastavení pro data skladování pracovního vytížení je 256 KB.

- Odolnost proti chybám je jednoduché (bez odolnost proti chybám).

>[AZURE.NOTE] Azure premium úložiště je místně nadbytečné a pořád tři kopie data v jedné oblasti, vyšší odolnost proti chybám v fondu úložiště není potřeba.

- Počet sloupců – výsledek je počet disků ve fondu úložiště.

Další informace o ukládání místa a úložiště fondů najdete tady:

- [Přehled prostor úložiště](http://technet.microsoft.com/library/hh831739.aspx).

- [Zálohování serveru a fondu úložiště](http://technet.microsoft.com/library/dn390929.aspx)

Další informace o doporučených postupech konfigurace systému SQL Server najdete v článku [výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>Nastavení serveru SQL

V **Nastavení systému SQL Server** zkontrolujte a upravte předponu názvu SQL Server OM verze serveru SQL Server, účtu služby SQL Server a heslo a SQL automatické opravy plán údržby.

- **SQL Server název Připojit znak před** slouží k vytvoření názvu pro každý SQL Server. Tento kurz dosáhnete **Contoso-ag**. Název serveru SQL Server bude *Contoso-ag-0* a *Contoso-ag-1*.

- **Verze serveru SQL Server** je verze systému SQL Server. Tento kurz dosáhnete **2014 SQL serveru**. Můžete taky **SQL Server 2012** nebo **SQL serveru 2016**.

- **Uživatelské jméno účtu služby SQL Server** je název domény účet služby SQL Server. Tento kurz dosáhnete **sqlservice**.

- **Heslo** je heslo k účtu služby SQL Server.  Pomocí složitých hesla. Potvrďte heslo.

- **Automatické opravy SQL plán údržby** identifikuje den v týdnu, že Azure automaticky oprava serverů SQL. Tento kurz zadejte **neděli**.

- Čas oblasti Azure **SQL automatické opravy údržbu počáteční hodina** je, když začne se automatické opravy.

>[AZURE.NOTE]Rozdělit okno oprav pro každou OM je celý o jednu hodinu. Pouze v jednom počítači virtuální je opravené najednou, aby se přerušení služby.

![Nastavení serveru SQL](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Zkontrolujte nastavení a klikněte na **OK**.

###<a name="summary"></a>Souhrn

Na souhrnné stránce ověří Azure nastavení. Můžete taky stažení šablony. Prohlédněte si na souhrn. Klikněte na **OK**.

###<a name="buy"></a>Koupit

Tento konečný zásuvné obsahuje **podmínky použití**a **zásady ochrany osobních údajů**. Prohlédněte si tyto informace. Až budete připraveni Azure k zahájení vytváření virtuálních počítačích a všechny ostatní požadované prostředky pro skupinu dostupnost, klikněte na **vytvořit**.

Portál Azure vytvoří skupina zdroje a všechny zdroje.

##<a name="monitor-deployment"></a>Sledování nasazení

Sledujte průběh nasazení z portálu Microsoft Azure. Ikona představující nasazení se automaticky Připne Azure portálu řídicího panelu.

![Azure řídicího panelu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Připojení k serveru SQL Server

Jsou spuštěné nové instance serveru SQL Server na virtuálních počítačích, které nemají připojení k Internetu. Ale řadiče domény mají internetové připojení. Než budete moct připojit k serverům SQL pomocí vzdálené plochy, první RDP na jeden z řadiče domény. Z řadiče domény otevřete druhý RDP k serveru SQL Server.

S verzí na primární domény řadiče domény postupujte takto:

1.  Z Azure portálu řídicího panelu velmi má úspěšnou nasazení.

1.  Klikněte na **zdroje**.

1.  V zásuvné **zdrojů** klikněte na **ad primární datacentrum** , což je název počítače virtuálního počítače primárního řadiče domény.

1.  Na zásuvné **ad primární datacentrum** klikněte na **Připojit**. Prohlížeče zobrazí dotaz, jestli chcete k otevření nebo uložení připojení ke vzdálené objekt. Klikněte na **Otevřít**.
![Připojení k Datacentrum](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Připojení ke vzdálené ploše** zobrazovat varování, že aplikace publisher toto připojení ke vzdálené nerozpoznala. Klikněte na **Připojit**.

1.  Zabezpečení systému Windows vás vyzve k zadejte svoje přihlašovací údaje pro připojení k IP adrese primární domény řadiče domény. Klikněte na tlačítko **použít jiný účet**. **Uživatelské jméno** zadejte **contoso\DomainAdmin**. Toto je účet, který jste se rozhodli pro uživatelské jméno správce. Pomocí složitých hesla, že jste zvolili při konfiguraci šablony.

1.  **Vzdálená plocha** může upozornění, že kvůli problémům s certifikátem zabezpečení nelze ověřit vzdálený počítač. Zobrazí se název certifikátu zabezpečení. Pokud jste postupovali kurzu název bude **ad primární dc.contoso.com**. Klikněte na **Ano**.

Teď jste připojeni k primární domény řadiče domény. S verzí na serveru SQL Server postupujte takto:

1.  U řadiče domény otevřete **Připojení ke vzdálené ploše**.

1.  **Počítač**zadejte název jednoho z serverů SQL. Tento kurz zadejte **0 SQL Server**.

1.  Pomocí stejného uživatelského účtu a heslo, které jste použili k RDP řadiče domény.

Teď jste připojeni s RDP k serveru SQL Server. Můžete otevřít SQL Server management studiuo, připojení k výchozí instanci systému SQL Server a ověřte, jestli ve že skupině availabilty nakonfigurovaný.


