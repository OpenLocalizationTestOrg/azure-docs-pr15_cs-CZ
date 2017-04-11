<properties 
   pageTitle="Konfigurace DNS mezi dvěma Azure virtuální sítěmi | Microsoft Azure" 
   description="Další informace o konfiguraci sítě VPN a domény překlad mezi dvěma virtuální sítích a jak nakonfigurovat geo replikace HBase." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Konfigurace DNS mezi dvěma Azure virtuálních sítí

> [AZURE.SELECTOR]
- [Konfigurace připojení k síti VPN](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurace DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurace HBase replikace](hdinsight-hbase-geo-replication.md) 


Naučte se přidat a nakonfigurovat serverů DNS Azure virtuální sítě pro zpracování překlad v rámci a virtuální sítích.

Tento kurz je druhá část [řady] [ hdinsight-hbase-geo-replication] týkající se vytváření geo replikace HBase:

- [Konfigurace připojení k síti VPN mezi dvěma virtuální sítěmi][hdinsight-hbase-geo-replication-vnet]
- Konfigurace DNS pro virtuálních sítí (Tento kurz)
- [Konfigurace HBase geo replikace][hdinsight-hbase-geo-replication]


Následující obrázek znázorňuje dva virtuálních sítí jste vytvořili v [článku Konfigurace připojení k síti VPN mezi dvěma virtuální sítěmi][hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase replikace virtuální síťového diagramu][img-vnet-diagram]

##<a name="prerequisites"></a>Zjistit předpoklady pro
Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracoviště se Azure Powershellu**.

    Před spuštěním skriptů Powershellu, zkontrolujte, jestli že jste připojeni k předplatnému Azure pomocí následující rutinu:

        Add-AzureAccount

    Pokud máte víc předplatných Azure, můžete nastavit aktuálního předplatného pomocí následující rutinu:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Dva Azure virtuální sítě s připojení k síti VPN**.  Pokyny najdete v tématu [Konfigurace připojení VPN mezi dvěma Azure virtuální sítěmi][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Služby Azure názvy a názvy virtuální počítač musí být jedinečná. Název použitý v tomto kurzu se Contoso-[název Azure Service/OM]-[EU / USA]. Například Contoso-VNet-EU je Azure virtuální síť v datovém centru severní Evropy; Contoso-DNS-US je DNS server OM v datovém centru východoasijských USA. Musíte vytvoříte vlastní názvy.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Vytvoření Azure virtuálních počítačích má být použit jako servery DNS

**Vytvoření virtuálního počítače v rámci Contoso VNet EU, s názvem Contoso. DNS EU**

1.  Klikněte na **Nový** **Výpočet**, **virtuální počítač**, **Z GALERIE**.
2.  Vyberte **Windows serveru 2012 R2 Datacentra**.
3.  Zadejte:
    - **Název počítače virtuální**: Contoso-DNS EU
    - **Nové uživatelské jméno**: 
    - **Heslo**: 
4.  Zadejte:
    - **CLOUDOVÉ služby**: vytvoření nového cloudové služby
    - **Oblast/SPŘAŽENÍ skupiny/virtuální sítě**: (vyberte Contoso VNet EU)
    - **Virtuální PODSÍTÍ**: podsítě-1
    - **Úložiště účtu**: použití účtu automaticky generované úložiště
    
        Název cloudové služby se budou shodovat s názvem virtuálního počítače. V tomto případě je to Contoso DNS EU. Pro následující virtuálních počítačích se rozhodnete sdělit nám cloudovou službu stejné.  Všechny virtuálních počítačích v části stejné cloudové služby sdílení stejné virtuální sítě a přípona domény.

        Úložiště účet se používá k ukládání souboru s obrázkem virtuálního počítače. 
    - **Koncové body**: (přejděte dolů a vyberte **DNS**) 

Po vytvoření virtuální počítač zjistíte vnitřní IP a externí IP Adrese.

1.  Klikněte na název virtuálního počítače **Evropské unie Contoso DNS**.
2.  Klikněte na **řídicí panel**.
3.  Zapište si:
    - VEŘEJNÉ VIRTUÁLNÍ IP ADRESA
    - VNITŘNÍ IP ADRESA


**Vytvoření virtuálního počítače v rámci Contoso VNet spojené, s názvem Contoso DNS USA** 

- Opakujte stejný postup, který chcete vytvořit virtuální počítač se na tyto hodnoty:
    - NÁZEV počítače virtuální: Contoso-DNS-USA
    - OBLAST/SPŘAŽENÍ skupiny/virtuální sítě: Vyberte Contoso VNET USA
    - VIRTUÁLNÍ PODSÍTÍ: Podsítě-1
    - ÚLOŽIŠTĚ účet: Použití účtu automaticky generované úložiště
    - Koncové body: (vybrat DNS)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Nastavení statické IP adres pro dva virtuálních počítačích

Servery DNS vyžaduje statické IP adres.  Z portálu Microsoft Azure klasické nelze provést tento krok. Použijete Azure Powershellu.

**Konfigurace statickou IP adresu dvou virtuálních počítačích**

1. Otevřete ISE v prostředí Windows PowerShell.
2. Spusťte tyto rutiny.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Název_služby je název služby cloudu. Protože DNS server je první virtuální počítač cloudové služby, název cloudové služby je shodný s názvem virtuálního počítače.

    Možná budete muset aktualizovat název_služby a název shodovat s názvy, které máte.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Přidejte na Server DNS role dvou virtuálních počítačích

**Chcete-li přidat role serveru DNS pro Contoso. DNS EU**

1.  Z portálu Microsoft klasické Azure klikněte na **virtuálních počítačích** na levé straně. 
2.  Klikněte na **Contoso DNS EU**.
3.  Klikněte na **řídicí panel** v horní.
4.  Klikněte na **Připojit** ze spodního okraje a postupujte podle pokynů pro připojení k virtuálního počítače prostřednictvím RDP.
2.  V rámci RDP relace klikněte na tlačítko Windows v levém dolním otevřete na úvodní obrazovce.
3.  Klikněte na dlaždici **Správce serveru** .
4.  Klikněte na **Přidat rolí a funkcí**.
5.  Klikněte na tlačítko **Další**
6.  Vyberte **instalace na základě rolí nebo založeného na funkcích**a klikněte na tlačítko **Další**.
7.  Vyberte virtuálního počítače DNS (musí být zvýrazní už) a klikněte na tlačítko **Další**.
8.  Zaškrtněte políčko **DNS Server**.
9.  Klikněte na **Přidat funkce**a pak klikněte na **pokračovat**.
10. Postup třikrát klepněte na tlačítko **Další** a potom klikněte na **nainstalovat**. 

**Chcete-li přidat role serveru DNS pro Spojené státy Contoso DNS**

- Opakujte postup pro přidání DNS role **Contoso-DNS-US**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Servery DNS přiřadit virtuálních sítí

**K registraci dva servery DNS**

1.  Z portálu Microsoft klasické Azure klikněte na **Nový** **SÍŤOVÝCH služeb**, **Virtuální sítě**, **ZAREGISTROVAT SERVER DNS**.
2.  Zadejte:
    - **Název**: Contoso-DNS EU
    - **IP adresu serveru DNS**: 10.1.0.4 – IP adresa musí odpovídající IP adresu virtuálního počítače serveru DNS.
     
3.  Opakujte proces registrace Contoso DNS spojené s tímto nastavením:
    - **Název**: Contoso-DNS USA
    - **IP adresu serveru DNS**: 10.2.0.4

**Pokud chcete přiřadit dva servery DNS dvě virtuální sítě**

1.  V levém podokně na portálu klasické klepněte na položku **sítě** .
2.  Klikněte na **Contoso VNet EU**.
3.  Klikněte na **KONFIGUROVAT**.
4.  V části **servery dns** vyberte **Evropské unie Contoso DNS** .
5.  Klikněte na **Uložit** v dolní části stránky a klikněte na tlačítko **Ano** potvrďte.
6.  Opakujte proces přiřadit server DNS **Contoso-DNS-US** virtuální sítě **Contoso-VNet-US** .

Všechny virtuálních počítačích, které již byly nasazeny virtuální sítím, je nutné restartovat aktualizovat konfigurace serveru DNS.

**Restartujte virtuálních počítačích**

1. Z portálu Microsoft klasické Azure klikněte na **virtuálních počítačích** na levé straně.
2. Klikněte na **Evropské unie Contoso DNS**.
3. Klikněte na **řídicí panel** v horní.
4. V dolní části klikněte na **RESTARTOVAT** .
5. Opakujte stejný postup restartujte **Contoso-DNS-US**.


##<a name="configure-dns-conditional-forwarders"></a>Konfigurace DNS podmíněné předávání

Server DNS v každé virtuální síti může vyřešit jenom DNS názvy v rámci tohoto virtuální sítě. Potřebujete konfigurovat podmíněné předávání tak, aby ukazovaly na server DNS mezi dvěma účastníky název rozlišení v síti virtuální mezi dvěma účastníky.

Abyste mohli nakonfigurovat podmíněné předávání, je potřeba vědět přípony domén dva servery DNS. DNS přípony se může lišit v závislosti na konfiguraci cloudové služby, kterou jste použili při vytváření virtuálních počítačích. Pro každý používán virtuální sítě přípony DNS musíte přidat podmíněné předávání. 

**K vyhledání domény přípony dva servery DNS**

1. RDP do **Evropské unie Contoso DNS**.
2. Otevřete konzoly Windows PowerShell nebo příkazový řádek.
3. Spuštění **příkazu ipconfig**a zapište si **přípona DNS připojení**.
4. Nezavírejte RDP relace, budete potřebovat. 
5. Opakujte stejný postup zjistěte **přípony specifické pro připojení DNS** **Contoso-DNS-US**.


**Konfigurace DNS pro předávání**
 
1.  Relace RDP **Contoso DNS**EU klikněte na klávesu Windows v levém dolním.
2.  Klikněte na **Nástroje pro správu**.
3.  Klikněte na **DNS**.
4.  V levém podokně rozbalte **kódu oznámení o doručení**, **Contoso DNS EU**.
5.  Zadejte následující informace:
    - **DNS domény**: Zadejte přípony DNS spojené DNS Contoso. Příklad: Contoso-DNS US.b5.internal.cloudapp.net.
    - **IP adresy hlavní servery**: Zadejte 10.2.0.4, což je Contoso-DNS-spojených na IP adresu.
6.  Stiskněte klávesu **ENTER**a potom klikněte na **OK**.  Teď budete moct vyřešit Contoso-DNS-USA na IP adresu z Evropské unie Contoso DNS.
7.  Opakujte postup pro přidání DNS pro předávání ke službě DNS v počítači virtuální Contoso DNS spojené s následující hodnoty:
    - **DNS domény**: Zadejte přípony DNS Contoso DNS EU. 
    - **IP adresy hlavní servery**: Zadejte 10.2.0.4, což je Contoso-DNS-Evropské unie IP adresa.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Otestujte překlad přes virtuální sítě

Teď můžete otestovat překlad názvů virtuální sítích. Ve výchozím nastavení je bránou firewall blokovány ping.  Pomocí nástroje nslookup řešení virtuálních počítačích server DNS (třeba použijete plně kvalifikovaný název domény) v sítích mezi dvěma účastníky.  


##<a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili, jak nakonfigurovat překlad sítích virtuální sítě VPN. Další články dvou v řadě zabývat těmito oblastmi:

- [Konfigurace připojení VPN mezi dvěma Azure virtuálních sítí][hdinsight-hbase-geo-replication-vnet]
- [Konfigurace HBase geo replikace][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
