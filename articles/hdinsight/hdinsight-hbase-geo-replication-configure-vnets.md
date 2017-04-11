<properties 
   pageTitle="Konfigurace připojení VPN mezi dvěma virtuální sítěmi | Microsoft Azure" 
   description="Další informace o konfiguraci sítě VPN a domény překlad mezi dvěma Azure virtuálních sítí a jak nakonfigurovat geo replikace HBase." 
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

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Konfigurace připojení VPN mezi dvěma Azure virtuálních sítí  

> [AZURE.SELECTOR]
- [Konfigurace připojení k síti VPN](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurace DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurace HBase replikace](hdinsight-hbase-geo-replication.md) 

Připojení k webu Azure virtuální sítě VPN brány používá k poskytování zabezpečené tunelem pomocí Ipsec/IKE. VNets může být v různých oblastech a různých předplatných. Můžete dokonce zkombinujete VNet VNet komunikaci s konfigurací více webu. Existuje několik důvodů, proč pro VNet VNet připojení:

- Křížové oblast geo redundance a geo informací o stavu 
- Místní aplikací vícevrstvého s silných izolace okraj 
- Křížové předplatného, komunikace mezi organizace v Azure

Další informace najdete v tématu [Konfigurace VNet VNet připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Zobrazíte je na video:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Tento kurz je součástí [řady] [ hdinsight-hbase-replication] týkající se vytváření geo replikace HBase. 

- Konfigurace připojení k síti VPN mezi dvěma virtuální sítěmi (Tento kurz)
- [Konfigurace DNS pro virtuální sítě][hdinsight-hbase-geo-replication-dns]
- [Konfigurace HBase geo replikace][hdinsight-hbase-geo-replication]

Následující obrázek znázorňuje dva virtuální sítě, ke kterým vytvoříte v tomto kurzu:

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


>[AZURE.NOTE] Služby Azure názvy a názvy virtuální počítač musí být jedinečná. Název použitý v tomto kurzu se Contoso-[název Azure Service/OM]-[EU / USA]. Například Contoso-VNet-EU je Azure virtuální síť v datovém centru severní Evropy; Contoso-DNS-US je DNS server OM ve východoasijských USA datacentra. Musíte vytvoříte vlastní názvy.
 

##<a name="create-two-azure-vnets"></a>Vytvoření dvou VNets Azure



**Vytvořit virtuální sítě s názvem Contoso VNet EU v severní Evropě**

1.  Přihlaste se k [Azure klasické portál][azure-portal].
2.  Klikněte na **Nový** **SÍŤOVÝCH služeb**, **virtuální sítě**, **vytvořit vlastní**.
3.  Zadejte:

    - **Název**: Contoso-VNet-EU
    - **Umístění**: severní Evropě

        Tento kurz používá datacentrech severní Evropy a východoasijských USA. Můžete použít vlastní datacentrech.
4.  Zadejte:

    - **DNS SERVER**: (nechejte pole prázdné) 
    
        Budete potřebovat serveru DNS pro překlad v rámci virtuální sítí. Další informace týkající se použití Azure-za předpokladu, že překlad a kdy použít serveru DNS najdete v článku [Vyřešení DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Pokyny pro nastavení překlad mezi VNets najdete v tématu [Konfigurace DNS mezi dvěma Azure virtuální sítěmi][hdinsight-hbase-dns].
  
    - **Konfigurace VPN čárky webu**: (zaškrtnuté)

        Bod webu nevztahuje se na tento scénář.

    - **Konfigurace sítě VPN na webu**: (zaškrtnuté)
    
        Připojení VPN k webu Azure virtuální sítě nakonfiguruje ve východoasijských USA datacentra.
5.  Zadejte:

    -   **Spuštění IP adresu místa**: 10.1.0.0
    -   **Adresa místa CIDR**: / 16
    -   **IP adres podsítí-1 spuštění**: 10.1.0.0
    -   **CIDR podsítě-1**: / 24

    Adresní prostor nemůže překrývat s virtuální sítě USA.  

**Vytvořit virtuální sítě s názvem Contoso VNet EU v Evropě západní**

- Opakujte postup poslední se na tyto hodnoty:

    - **Název**: Contoso-VNet US
    - **Umístění**: východoasijských USA
     
    - **DNS SERVER**: (nechejte pole prázdné)
    - **Konfigurace VPN čárky webu**: (zaškrtnuté)
    - **Konfigurace sítě VPN na webu**: (nezaškrtnutá)
     
    - **Spuštění IP adresu místa**: 10.2.0.0
    - **Adresa místa CIDR**: / 16
    - **IP adres podsítí-1 počáteční**: 10.2.0.0
    - **CIDR podsítě-1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Konfigurace připojení VPN mezi dvěma VNets

###<a name="create-local-networks"></a>Vytvoření místní sítě

Při vytváření VNet VNet konfiguraci musíte nastavit každý VNet k identifikaci vzájemně jako webem místní síti. V této části nakonfigurujete každý VNet jako místní síti. Místní sítě sdílet stejné prostorech adres IP s odpovídajícím VNet.

![Konfiguraci Azure VPN k webu – azure místní sítě][img-vnet-lnet-diagram]


**Vytvoření místní síti s názvem Contoso-LNet-Evropské unie odpovídající adresní prostor síť Contoso – VNet EU**

1. Z portálu Microsoft klasické Azure klikněte na **Nový** **Síťové služby**, **Virtuální sítě**, **Přidejte místní síti**.
3. Zadejte:

    - **Název**: Contoso-LNet-EU
    - **Virtuální privátní sítě zařízení IP adresa**: 192.168.0.1 (tuto adresu se aktualizují později)

        Obvykle byste měli použijete skutečné externí IP adresu pro zařízení VPN. Pro VNet VNet konfigurací použijete VPN brány IP adresu. Vzhledem k tomu, že jste ještě nevytvořili bran VPN pro dva VNets, zadejte IP adresu arbitary a vraťte na opravný nástroj fix it.
4.  Zadejte:

    - **Počáteční IP adresu místa:** 10.1.0.0
    - **CIDR místo adresy:** /16
    
    To musí odpovídat přesně oblast, kterou jste zadali dříve Contoso VNet EU.

**Vytvoření místní síti s názvem Contoso-LNet – cz odpovídající adresní prostor síť Contoso-VNet cz**

- Opakujte postup poslední pomocí následujících parametrů:

    - **Název**: Contoso-LNet US
    - **Virtuální privátní sítě zařízení IP adresa**: 192.168.0.1 (tuto adresu se aktualizují později)
     
    - **Spuštění IP adresu místa**: 10.2.0.0
    - **Adresa místa CIDR**: / 16


###<a name="create-vpn-gateways"></a>Vytvoření brány VPN

V této konfiguraci tvoří dvě části. Nejdřív konfigurace připojení k webu VNet k místní síti a vytvořte dynamické směrování VPN. VNet VNet vyžaduje Azure VPN bran s dynamické směrování VPN. Azure statický směrování virtuální privátní sítě nejsou podporované.

**Konfigurace připojení k webu Contoso VNet EU pro Contoso-LNet-US**

1.  Z portálu klasické Azure klikněte v levém podokně **sítě**
2.  Klikněte na **Contoso VNet EU**.
3.  Klikněte na kartu **CONFIGUE** .
4.  Podívejte **se připojit k místní síti**.
5.  V **Místní síti**vyberte **Contoso-LNet-US**.
6.  Klikněte na **Přidat brány podsítě** v části mezery adresa virtuální sítě.
7.  Klikněte na **Uložit**.
8.  Kliknutím na **OK** potvrďte.


**Vytvoření brány virtuální privátní sítě Contoso VNet EU**

1.  Z portálu Microsoft klasické Azure klikněte na kartu **řídicího PANELU** .
4.  Klikněte na **Vytvořit brány** na konec stránky a potom klikněte na **Dynamické směrování**.
5.  Klikněte na **Ano** potvrďte. Všimněte si grafiku brány na stránce se změní na žlutá a vytvoření brány se objeví. Obvykle to trvá asi 15 minut brány na vytvořit.

    Jakmile se stav brány změní na připojení, uvidí IP adresu pro každou bránu na řídicím panelu. Poznamenejte si IP adresu, která odpovídá pro každou VNet starat pozor, abyste kombinovat. Toto jsou IP adresy, které se použije při úpravě IP adresy zástupný symbol pro zařízení VPN v místní sítě.

6.  Vytvořte kopii **Brány IP adresu**. Můžete ji používat ke konfiguraci VPN brány IP adresu pro Contoso EU VNet v následující části.

**Vytvoření brány virtuální privátní sítě Contoso VNet EU**

- Opakujte poslední dvě postup pro nastavení připojení k webu Contoso-VNet-US Contoso LNet EU a vytvořit Brána VPN pro Contoso-Vnet-US. Až budete hotovi, bude mít VPN brány IP adresu pro Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Nastavení IP adres pro místní sítě VPN zařízení
V části poslední vytvoření brány VPN u každé VNets. Jste získali IP adresy bran VPN. Teď můžete přejít zpět na nastavení místní sítě VPN zařízení IP adres.

**Konfigurace VPN zařízení IP adresu pro Contoso LNet EU** 

1.  Z portálu Microsoft klasické Azure klikněte v levém podokně **sítě** .
2.  Na začátek klikněte na položku **Místní sítě** .
3.  Klikněte na **Contoso LNet EU**a potom klikněte na **Upravit** v dolní části.
4.  Aktualizaci **VPN zařízení IP adresy**.  Toto je adresa, kterou dostanete z karty řídicího PANELU Contoso VNET EU.
5.  Klikněte pravým tlačítkem.
6.  Klikněte na tlačítko Zkontrolovat.

**Konfigurace VPN zařízení IP adresu pro Contoso LNet USA** 

- Opakujte poslední postup pro nastavení sítě VPN zařízení IP adresu pro Contoso-LNet-US.

###<a name="set-vnet-gateway-keys"></a>Nastavte VNet klíče brány

Vnet brány musí používat sdílený klíč k ověření připojení mezi virtuální sítě. Klávesy se nedají konfigurovat z portálu klasické Azure. Musíte pomocí prostředí PowerShell a .NET SDK.

**Chcete-li nastavit klíče**

1. Na počítači otevřete **ISE v prostředí Windows PowerShell** nebo konzoly Windows PowerShell.
2. Aktualizace parametrů v tento skript sledovat a spuštění:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Zkontrolujte připojení VPN 

Všechny VMs používaný VNets, můžete použít bez vizuální virtuální síťový diagram stránky řídicího panelu VNet na portálu klasické Azure Pokud chcete zkontrolovat stav připojení:

![HDInsight HBase replikace virtuální sítě VPN stavu připojení][img-vpn-status]
  



##<a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili, jak nakonfigurovat připojení VPN mezi dvěma Azure virtuálních sítí. Další články dvou v řadě zabývat těmito oblastmi:

- [Konfigurace DNS mezi dvěma Azure virtuálních sítí][hdinsight-hbase-geo-replication-dns]
- [Konfigurace HBase geo replikace][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
