<properties
   pageTitle="Vytvořit posluchače AlwaysOn availabilty skupiny pro systém SQL Server ve virtuálních počítačích Azure"
   description="Podrobné pokyny k vytváření posluchače pro skupinu availabilty AlwaysOn pro systém SQL Server ve virtuálních počítačích Azure"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Konfigurace vyrovnávání zatížení interní dostupnost skupiny AlwaysOn v Azure

Toto téma vysvětluje, jak vytvořit Vyrovnávání zatížení interní dostupnost skupiny AlwaysOn serveru SQL Azure virtuálních počítačích spuštěné v modelu správce zdrojů. Skupiny dostupnosti AlwaysOn vyžaduje vyrovnávání zatížení při instance serveru SQL Server na virtuálních počítačích Azure. Vyrovnávání zatížení ukládá IP adresu pro posluchače dostupnost skupiny. Pokud skupiny dostupnosti zahrnuje vícenásobné oblastí, musí každou oblast Vyrovnávání zatížení.

K provedení této úlohy musíte mít skupiny dostupnosti SQL Server AlwaysOn nasazený na Azure virtuálních počítačích v modelu správce zdrojů. Obě virtuálních počítačích SQL serveru musí patřit k stejnou sadu dostupnosti. [Šablona aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) můžete automaticky vytvořit skupiny dostupnosti AlwaysOn ve Správci Azure zdroje. Tato šablona automaticky vytvoří Vyrovnávání zatížení interní za vás. 

Pokud chcete, můžete [ručně nakonfigurovat skupiny dostupnosti AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Toto téma vyžaduje, že skupin dostupnosti jsou již nakonfigurovány.  

Příbuzná témata, patří:

 - [Konfigurace skupiny dostupnosti AlwaysOn v Azure OM (grafické)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Konfigurace připojení VNet VNet přes správce prostředků Azure a prostředí PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Postup

Podle rozbor tohoto dokumentu bude vytváření a konfigurace vyrovnávání zatížení na portálu Azure. Po dokončení, které si nakonfigurujete clusteru používat na IP adresu z vyrovnávání zatížení může být pro posluchače AlwaysOn dostupnost skupiny.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Vytváření a konfigurace služby Vyrovnávání zatížení na portálu Azure

V této části úkolu provedete následující kroky Azure portálu:

1. Vytvoření Vyrovnávání zatížení a konfigurace IP adresa

1. Konfigurace fondu back-end

1. Vytvoření zkušební 

1. Nastavení pravidel pro vyrovnávání zatížení

>[AZURE.NOTE] Pokud serverů SQL skupiny různých zdrojů a oblastí, provedete některé z těchto kroků dvakrát, jednou v každé pole Skupina zdroje.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. vytváření Vyrovnávání zatížení a konfigurace IP adresa

Cílem prvního kroku je vytvořit Vyrovnávání zatížení. Na portálu Azure otevřete skupina zdroje, který obsahuje virtuálních počítačích SQL serveru. Ve skupinovém rámečku zdroj klikněte na **Přidat**.

- Hledat **Služba Vyrovnávání zatížení**. Ve výsledcích hledání vyberte **Vyrovnávání zatížení**, který je publikován společností **Microsoft**.

- Na zásuvné **Vyrovnávání zatížení** klikněte na **vytvořit**.

- Na **vytvořit služba Vyrovnávání zatížení**, nakonfigurujte Vyrovnávání zatížení takto:

| Nastavení | Hodnota |
| ----- | ----- |
| **Jméno** | Název text představující Vyrovnávání zatížení. Například **sqlLB**. |
| **Schéma** | **Vnitřní** |
| **Virtuální sítě** | Zvolte virtuální síť, která je do servery SQL.   |
| **Podsítě**  | Zvolte podsítě, která je do servery SQL. |
| **Předplatné** | Pokud máte víc předplatných, může se zobrazit tato pole. Vyberte předplatné, které chcete přidružit k tomuto prostředku. Je obvykle stejné obsahoval jako všechny zdroje pro skupiny dostupnosti.  |
| **Pole Skupina zdroje** | Zvolte skupina zdroje, které jsou v serverů SQL. | 
| **Umístění** | Zvolte Azure umístění, které jsou servery SQL v. |

- Klikněte na **vytvořit**. 

Azure vytvoří Vyrovnávání zatížení, které jste nakonfigurovali nad. Vyrovnávání zatížení patří do určité sítě, podsítě, skupina zdroje a umístění. Po dokončení Azure zkontrolujte nastavení Vyrovnávání zatížení v Azure. 

Teď nakonfigurujte IP adresu Vyrovnávání zatížení.  

- Na zásuvné **Nastavení** Vyrovnávání zatížení klikněte na **IP adresu**. **IP adresy** zásuvné ukáže, že to Vyrovnávání zatížení soukromé ve stejné síti virtuální jako serverech SQL. 

- Nastavení: 

| Nastavení | Hodnota |
| ----- | ----- |
| **Podsítě** | Zvolte podsítě, která je do servery SQL. |
| **Přiřazení** | **Statická** |
| **IP adresa** | Zadejte nepoužitý virtuální IP adresu z podsítě.  |

- Uložení nastavení.

Vyrovnávání zatížení má nyní k IP adrese. Záznam tuto IP adresu. Tuto IP adresu použije při vytváření posluchače na clusteru. V skript Powershellu dál v tomto článku použít tuto adresu pro `$ILBIP` proměnné.



## <a name="2-configure-the-backend-pool"></a>2. Konfigurace fondu back-end

Dalším krokem je vytvoření fondu back-end adres. Azure hovorů back-end adresu fondu *back-end fondu*. V tomto případě fondu back-end je adresy dvou SQL serveru ve skupině dostupnosti. 

- Ve skupině zdrojů klikněte na Vyrovnávání zatížení, který jste vytvořili. 

- Na **Nastavení**klikněte na **back-end fondů**.

- Na **fondy adres back-end**klikněte na **Přidat** k vytvoření fondu adres back-end. 

- **Fond back-end přidat** v části **název**zadejte název fondu back-end.

- V části **virtuálních počítačích** klikněte na tlačítko **+ Přidat virtuálního počítače**. 

- V části **Zvolit virtuálních počítačích** klepněte na položku **sady dostupnost** a určení sady dostupnosti, že virtuálních počítačích SQL Server do kterých patříte.

- Po zvolení sadu dostupnost, klikněte na **Zvolit virtuálních počítačích**. Klikněte na dvě virtuálních počítačích hostujících instance serveru SQL Server ve skupině dostupnosti. Klikněte na **Výběr**. 

- Klikněte na **OK** zavřete zásuvné moduly pro **Výběr virtuálních počítačích**a **fondu back-end přidat**. 

Azure aktualizovat nastavení fondu back-end adres. Dostupnost nastavení se fond dva servery SQL.

## <a name="3-create-a-probe"></a>3. zkušební vytvořit

Dalším krokem je vytvoření zkušební. Zkušební definuje, jak bude Azure ověřit serverů SQL, který má aktuálně posluchače dostupnost skupiny. Azure bude probe služeb na základě IP adresy portu, které definujete při vytváření zkušební.

- Na zásuvné **Nastavení** Vyrovnávání zatížení klikněte na **sond**. 

- Na zásuvné **sond** klikněte na **Přidat**.

- Konfigurace zkušební na zásuvné **zkušební přidat** . Konfigurace zkušební pomocí následující hodnoty:

| Nastavení | Hodnota |
| ----- | ----- |
| **Jméno** | Název text představující zkušební. Například **SQLAlwaysOnEndPointProbe**. |
| **Protocol (protokol)** | **TCP** |
| **Port** | Může použít jakékoli dostupné port. Například *59999*.    |
| **Interval**  | *5* | 
| **Chybná prahová hodnota**  | *2* | 

- Klikněte na **OK**. 

>[AZURE.NOTE] Zkontrolujte portu, které zadáte otevřít v bráně firewall serveru i SQL serveru. Obou serverů vyžadují pravidlo pro příchozí připojení pro port TCP, který používáte. Další informace najdete v článku [Přidat nebo upravit pravidlo bránu Firewall](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure vytvoří zkušební. Azure budou používat zkušební otestovat, který SQL Server má posluchače pro skupiny dostupnosti.

## <a name="4-set-the-load-balancing-rules"></a>4. nastavení pravidel pro vyrovnávání zatížení

Nastavení pravidel pro vyrovnávání zatížení. Pravidla Vyrovnávání zatížení nakonfigurovat tak, jak Vyrovnávání zatížení přesměrovává přenosy na serverech SQL. Pro tento Vyrovnávání zatížení bude povolit přímé serveru zpáteční, protože pouze jedna ze dvou servery SQL někdy vlastní prostředku dostupnost skupiny posluchače najednou.

- Na zásuvné **Nastavení** Vyrovnávání zatížení klikněte na **načíst vyrovnávání pravidla**. 

- Na zásuvné **Vyrovnávání zatížení pravidla** klikněte na **Přidat**.

- **Přidat načíst vyrovnávání pravidla** zásuvné používá ke konfiguraci pravidlo pro vyrovnávání zatížení. Použijte následující nastavení: 

| Nastavení | Hodnota |
| ----- | ----- |
| **Jméno** | Název text představující pravidla pro vyrovnávání zatížení. Například **SQLAlwaysOnEndPointListener**. |
| **Protocol (protokol)** | **TCP** |
| **Port** | *1433*   |
| **Port back-end** | *1433*. Všimněte si, že to bude možné neaktivní, protože toto pravidlo používá **Plovoucí IP (zpáteční přímé server)**.   |
| **Probe** | Použijte název, který jste vytvořili zkušební pro tento Vyrovnávání zatížení. |
| **Perzistence relace**  | **Žádná** | 
| **Nečinnosti (v minutách)**  | *4* | 
| **Plovoucí IP (zpáteční přímé server)**  | **Povoleno** | 

 >[AZURE.NOTE] Může být potřeba se posunout dolů na zásuvné zobrazíte všechny možnosti.

- Klikněte na **OK**. 

- Azure nakonfiguruje pravidlo pro vyrovnávání zatížení. Vyrovnávání zatížení je teď nakonfigurované provoz směrovat na serveru SQL Server, který je hostitelem posluchače pro skupiny dostupnosti. 

Skupina zdroje v tomto okamžiku má při vyrovnávání zatížení, připojení k oba počítače SQL serveru. Vyrovnávání zatížení taky obsahuje IP adresu pro posluchače skupiny dostupnosti AlwaysOn serveru SQL, takže některý z počítače můžete odpovědět na žádosti o dostupnosti skupiny.

>[AZURE.NOTE] Pokud jsou servery SQL ve dvou samostatných oblastech, opakujte kroky v jiné oblasti. Každou oblast vyžaduje vyrovnávání zatížení. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurace clusteru používat IP adresu Vyrovnávání zatížení 

Dalším krokem je ke konfiguraci posluchače na clusteru a přenést posluchače online. K tomu, postupujte takto: 

1. Vytvořit skupiny posluchače dostupnosti na překlopení obrázku 

1. Přenést posluchače online

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. Vytvořte skupinu posluchače dostupnosti na překlopení obrázku

V tomto kroku je vytvořit ručně posluchače skupiny dostupnost ve Správci překlopení obrázku a SQL Server Management Studio (SSMS).

- Připojení k Azure virtuální počítač, který je hostitelem primární otevřené pomocí RDP. 

- Otevřete správce.

- Vyberte uzel **sítí** a poznamenejte si síťový název obrázku. Tento název se použije v `$ClusterNetworkName` proměnných v skript Powershellu.

- Rozbalte název obrázku a potom klikněte na položku **role**.

- V podokně **role** klikněte pravým tlačítkem myši na název skupiny dostupnosti a pak vyberte **Přidat zdroje** > **Klientský přístupový bod**.

- Do pole **název** vytvořit název pro tento nový posluchače, a pak dvakrát klikněte na **Další** a potom klikněte na **Dokončit**. Nezahájí posluchače nebo online zdroje v tomto okamžiku.

 >[AZURE.NOTE] Název nového posluchače je název sítě, využívající aplikací pro připojení k databázím ve skupině dostupnost SQL serveru.

- Klikněte na kartu **zdroje** a potom rozbalte položku klientský přístupový bod jste právě vytvořili. Klikněte pravým tlačítkem myši IP zdroje a klikněte na vlastnosti. Zapište si název IP adresu. Použijete tento název v `$IPResourceName` proměnných v skript Powershellu.

- V části **IP adresa** klikněte na **Statické IP adresy** a nastavte statickou IP adresu použít stejnou adresu, která jste použili při nastavování IP adresu Vyrovnávání zatížení na portálu Azure. Povolit NetBIOS pro tuto adresu a klikněte na **OK**. Opakujte tento krok pro každý zdroj IP Pokud řešení zahrnuje víc VNets Azure. 

- Na uzel obrázku, který je aktuálně hostitelem primární otevřené otevřete zvýšenými ISE PowerShell a vložte následující příkazy do nový skript.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Aktualizace proměnných a spustit skript Powershellu pro nastavení IP adresa a port pro nové posluchače.

 >[AZURE.NOTE] Pokud jsou servery SQL v samostatných oblastech, budete muset spustit skript Powershellu dvakrát. První použití clusteru síťový název, název zdroje IP clusteru a načtěte vyrovnávání IP adresu z první skupina zdroje. Ještě jednou pomocí obrázku síťový název, název zdroje IP clusteru a načíst vyrovnávání IP adrese z druhé skupina zdroje.

Obrázku se má dostupnost skupiny posluchače zdroj.

## <a name="2-bring-the-listener-online"></a>2. přenést posluchače online

S tímto dostupnost skupiny posluchače zdrojem nakonfigurovali můžete si přenést posluchače online tak, aby aplikace můžete připojit k databázím ve skupině dostupnost s posluchače.

- Přejděte zpět do správce. Rozbalte položku **role** a zobrazíte skupiny dostupnosti. Na kartě **zdroje** klikněte pravým tlačítkem myši na název posluchače a klikněte na **Vlastnosti**.

- Klikněte na kartu **závislosti** . Pokud existuje více zdrojů uvedené, ověřte, zda IP adresy nebo Ne a závislosti. Klikněte na **OK**.

- Klikněte pravým tlačítkem myši na název posluchače a klikněte na **Přepnout do režimu Online**.


- Když posluchače je online, na kartě **zdroje** , klikněte pravým tlačítkem skupinu dostupnost a klikněte na **Vlastnosti**.

- Vytvoření závislosti na posluchače název zdroje (nikoli na IP adresu zdrojů název). Klikněte na **OK**.


- Spuštění SQL Server Management Studio a připojte k primární otevřené.


- Přejděte na **Maximum AlwaysOn dostupnost** | **skupiny dostupnosti** | **posluchače skupiny dostupnosti**. 


- Teď byste měli vidět posluchače název, který jste vytvořili v správce. Klikněte pravým tlačítkem myši na název posluchače a klikněte na **Vlastnosti**.


- V poli **Port** zadejte číslo portu pro posluchače dostupnost skupiny pomocí $EndpointPort jste dříve použili (1433 bylo výchozí nastavení), klikněte na tlačítko **OK**.

Nyní máte skupiny dostupnosti SQL Server AlwaysOn Azure virtuálních počítačích spuštění v režimu Správce zdrojů. 

## <a name="test-the-connection-to-the-listener"></a>Zkontrolujte připojení k webu posluchače

Chcete-li otestovat připojení:

1. RDP k serveru SQL, který je ve stejném virtuální síť, ale nejste vlastníkem otevřené. Může to být SQL Server ve clusteru.

1. Pomocí nástroje **sqlcmd** test připojení. Například následující skript vytvoří **sqlcmd** připojení k primární otevřené prostřednictvím posluchače pomocí ověřování Windows:

        sqlcmd -S <listenerName> -E

Připojení SQLCMD automaticky připojila k ten instanci služby SQL Server hostuje primární otevřené. 

## <a name="guidelines-and-limitations"></a>Pokyny a omezení

Poznámka: následující pokyny pro posluchače skupiny dostupnosti v Azure pomocí interních služba Vyrovnávání zatížení:

- Jedinou skupinu posluchače interní dostupnosti je podporována za cloudové služby, protože posluchače nakonfigurovaný tak, aby Vyrovnávání zatížení, a to není jedinou Vyrovnávání zatížení vnitřní. Ale je možné vytvořit zdrojový externí posluchače. 

- S Vyrovnávání zatížení vnitřní přístup jenom posluchače z ve stejné síti virtuální.
 
