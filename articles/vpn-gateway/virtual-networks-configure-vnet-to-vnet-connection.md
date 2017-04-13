<properties
   pageTitle="Konfigurace připojení VNet VNet pro klasické nasazení modelu | Microsoft Azure"
   description="Jak se připojit Azure virtuálních sítí spolupráce pomocí prostředí PowerShell a Azure klasické portálu."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Konfigurace připojení VNet VNet pro klasické nasazení modelu

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasický – klasické portálu](virtual-networks-configure-vnet-to-vnet-connection.md)



Tento článek vás provede kroky k vytvoření a připojení přes virtuální sítě spolupráce pomocí klasické nasazení modelu (označovaná taky jako správce služby správy). Následující postup používá portálu Azure klasické při tvorbě VNets a bran a prostředí PowerShell který nakonfiguruje připojení VNet VNet. Na portálu nelze nakonfiguruje připojení.

![VNet VNet připojení diagramu](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modely nasazení a metody pro VNet VNet připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace VNet VNet. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Informace o připojení VNet VNet

Připojení k jiné síti virtuální virtuální sítě je podobný připojení k umístění webu místní síti virtuální (VNet VNet). Oba typy připojení se k zajištění zabezpečené tunelem pomocí IPsec/IKE používají VPN brány. 

V různých předplatných a různé regiony a může být VNets, ke kterému se můžete připojit. Kombinování VNet VNet komunikaci s konfigurací více webu. Toto oprávnění umožňuje vytvořit topologie sítě kombinující křížově místní připojení s připojením mezi virtuální sítě.


### <a name="why-connect-virtual-networks"></a>Proč připojit virtuální sítě?

Můžete chtít připojit virtuální sítě z těchto důvodů:

- **Křížové oblast geo redundance a geo informací o stavu**
    - Můžete nastavit vlastní geo replikace a synchronizace s zabezpečené připojení nemusíte přecházet prostřednictvím internetového koncové body.
    - Vyrovnávání zatížení Azure a Microsoft nebo clusterů technologii třetích stran můžete nastavit vysoce dostupné úlohu s geo redundance napříč několika Azure oblastí. Příkladem důležité je nastavení SQL vždy na skupinám dostupnost šíření napříč několika Azure oblastí.

- **Místní aplikací vícevrstvého s silných izolace okraj**
    - Ve stejné oblasti můžete nastavit vícevrstvého aplikací s více VNets připojení společně s silných izolace a zabezpečená komunikace mezi osy.

- **Křížové předplatného, komunikace mezi organizace v Azure**
    - Pokud máte víc předplatných Azure, můžete se připojit pracovního vytížení z různých předplatných společně bezpečně mezi virtuální sítě.
    - Pro podniky nebo poskytovatele služeb můžete povolit komunikaci mezi organizace s zabezpečené technologie VPN v Azure.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>VNet VNet nejčastější dotazy týkající se klasické VNets

- Virtuální sítě může být ve stejném nebo jiném předplatná.

- Virtuální sítě může být ve stejném nebo jiném Azure oblasti (umístění).

- Do cloudové služby nebo k načtení vyrovnávání koncového bodu nemůže zahrnovat virtuální sítích, i když jsou připojené společně.

- Připojení více virtuální sítí společně nevyžaduje všechna VPN zařízení.

- VNet VNet podporuje připojení přes Azure virtuální sítě. Ho nepodporuje připojení virtuálních počítačích nebo cloudové služby, které nejsou nasazeny virtuální sítě.

- VNet VNet vyžaduje dynamické směrování brány. Azure statické směrování brány nejsou podporované.

- Připojení k síti virtuální slouží současně s více webů VPN. Existuje maximálně 10 tunelů VPN brány virtuální sítě VPN připojení dalších virtuální sítě nebo místní weby.

- Nesmí překrývat adresu mezery virtuální sítí a místních webů místní síti. Překrývající se adresa mezery způsobí vytváření virtuálních sítí a nahrávání souborů konfigurace netcfg selhání.

- Nadbytečné tunelů mezi dvěma virtuálních sítí nejsou podporované.

- Všechny tunelů VPN pro VNet, včetně P2S VPN, sdílet dostupné šířky pásma pro Brána VPN a provozu brány stejné VPN SLA v Azure.

- Přenosy VNet VNet prochází přes Azure páteřní síti.


## <a name="step1"></a>Krok 1: plánování rozsahy IP adres

Je důležité rozhodnout, oblasti, které se používá ke konfiguraci virtuální sítě. V této konfiguraci musíte žádná z oblastí VNet překrývat sebou, nebo pomocí kterékoli z místní sítě, ke kterým se připojit k.

Následující tabulka zobrazuje příklady jak definovat vaše VNets. Použití rozsahů jako vodítko pouze. Poznamenejte si oblastí virtuálních sítí. Tyto informace jsou potřeba pro pozdější kroky.

**Příklad nastavení**

|Virtuální sítě  |Adresní prostor               |Oblast      |Připojení k webu místní síti|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Západní USA     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Japonsko východ  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Krok 2 – Vytvoření VNet1

V tomto kroku vytvoříme VNet1. Pokud chcete použít některé příklady, nezapomeňte nahradit vlastní hodnoty. Pokud vaše VNet už existuje, nemusíte tento krok. Ale budete muset ověřte, zda rozsahy IP adres nepřekrývaly s oblastí pro druhý VNet nebo pomocí kterékoli z jiných VNets, ke kterému se chcete připojit.

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com). V tomto článku jsme používat portál klasické, protože některá nastavení požadovaná konfigurace ještě nejsou k dispozici v Azure portálu.

2. V levém dolním rohu obrazovky klikněte na **Nový** > **Síťové služby** > **Virtuální sítě** > **Vytvořit vlastní** spustíte Průvodce konfigurací. Jak přejít pomocí Průvodce přidáte zadanými hodnotami na každé stránce.

### <a name="virtual-network-details"></a>Virtuální nastavení sítě

Na stránce virtuální sítě podrobnosti zadejte následující informace:

  ![Virtuální nastavení sítě](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Název** - název virtuální sítě. Například VNet1.
  - **Umístění** – při vytváření virtuálních sítí, můžete jej přidružit Azure umístění (oblast). Pokud chcete svůj VMs nasazené k síti virtuální fyzicky umístit západní USA, například vyberte uvedeném místě. Není možné změnit umístění přidružené k síti virtuální po jeho vytvoření.

### <a name="dns-servers-and-vpn-connectivity"></a>Servery DNS a připojením VPN

Na stránce servery DNS a připojením VPN zadejte tyto informace a potom klikněte na další šipku v pravém dolním.

  ![Servery DNS a připojením VPN](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **Servery DNS** – zadejte název serveru DNS a IP adresu nebo rozevíracím seznamu vyberte dříve registrované serveru DNS. Toto nastavení serveru DNS nevytvoří. Umožňuje uživateli určit servery DNS, které chcete použít pro překlad pro tento virtuální sítě. Pokud chcete mít překlad mezi virtuálních sítí, budete muset konfigurace serveru DNS, spíše než překládání název, který poskytuje společnost Azure.
- Nevybírejte žádné zaškrtávací políčka P2S nebo S2S připojení. Klikněte na šipku v pravém dolním přejdete na další obrazovce.

### <a name="virtual-network-address-spaces"></a>Virtuální sítě adresu mezer

Na stránce virtuální adresu mezery sítě zadejte adresu oblast, kterou chcete použít pro síť virtuální. Toto jsou dynamické IP adresy (POKLESU) přiřazené VMs a ostatních rolí instancí nasazené tento virtuální sítě. 

Pokud vytváříte VNet, která bude mít taky zajištěný připojení k místní síti, je zejména důležité vyberte požadovaný rozsah, nepřekrývá pomocí kterékoli z oblastí, které se používají v místní síti. V takovém případě budete muset koordinaci s správce sítě. Správce sítě může být nutné doložka mimo rozsah IP adres z místního adresní prostor sítě použít pro svůj VNet.

  ![Virtuální sítě adresu mezery stránky](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Adresní prostor** – včetně spuštění IP adresu Count. Ověřte, že mezery adresu zadanou nepřekrývaly pomocí kterékoli z adresy mezery, ke kterým máte ve vaší místní síti. V tomto příkladu budeme používat 10.1.0.0/16 VNet1.
  - **Přidat podsítě** – včetně spuštění IP adresu Count. Další podsítí nejsou povinná, ale můžete chtít vytvořit samostatné podsítě pro VMs, které budou mít statické POKLES. Nebo můžete chtít mít vaše VMs v podsítě, která je oddělená od ostatních instancí role.
 
**Klikněte na zaškrtnutí** v pravém dolním rohu stránky a virtuální sítě se spustí vytvořit. Až se dokončí, zobrazí se, že "Vytvořili" zařazený do kategorie stav na stránce sítě.

## <a name="step-3---create-vnet2"></a>Krok 3 – vytvoření VNet2

Pak opakujte předchozí kroky k vytvoření jiného VNet. V dalších krocích připojíte dva VNets. Můžete odkázat na [příklady nastavení](#step1) v části Krok 1. Pokud vaše VNet už existuje, nemusíte tento krok. Potřebujete ověřte, že rozsahy IP adres nepřekrývaly pomocí kterékoli z jiných VNets nebo místní sítě, ke kterým se chcete připojit.

## <a name="step-4---add-the-local-network-sites"></a>Krok 4 – přidání webů místní síti

Při vytváření konfigurace VNet VNet budete muset Konfigurace webů místní síti, která se zobrazí na stránce portálu **Místní sítě** . Azure používá nastavení zadané v jednotlivých webů místní síti a zjistěte, jak chcete směrovat přenosy v síti mezi VNets. Zjistíte název, který chcete použít odkázat na každý web místní síti. Je vhodné použít popisný text, vyberte hodnotu z rozevíracího seznamu v dalších krocích.

Například VNet1 připojí k místní síti web, který vytvoříte s názvem "VNet2Local". Nastavení VNet2Local obsahují předpony adres VNet2 a veřejnou IP adresu VNet2 brány. VNet2 se připojí k místní síti webem vytvořená s názvem "VNet1Local", která obsahuje předpony adres VNet1 a veřejnou IP adresu VNet1 brány.

### <a name="localnet"></a>Přidání webu místní síti VNet1Local

1. V levém dolním rohu obrazovky klikněte na **Nový** > **Síťové služby** > **Virtuální sítě** > **Přidat místní síti**.

2. Na stránce **Zadejte podrobnosti místní síti** **název**zadejte název, který má představovat sítě, ke které se chcete připojit. V tomto příkladu "VNet1Local" můžete odkázat na brány a rozsahy IP adres pro VNet1.

3. **Virtuální privátní sítě zařízení IP adresy (nepovinné)**zadejte libovolný platný veřejnou IP adresu. Obvykle byste měli použijete skutečné externí IP adresu pro zařízení VPN. Pro VNet VNet konfigurace použijte veřejnou IP adresu přiřazeného k bráně pro vaše VNet. Ale předpokladu, že jste si vytvořili dosud brány, můžete zadat libovolný platný veřejnou IP adresu jako zástupný symbol. Není nechte toto pole prázdné – je povinný v této konfiguraci. Později vraťte se do těchto nastavení a konfigurace po Azure vzniká s odpovídajícím IP adresy brány. Klikněte na šipku přejdete na další obrazovce.

4. Na stránce **zadání adresu**zadejte IP adresu rozsah a adresa počet pro VNet1. To musí odpovídat přesně oblasti, který je nakonfigurovaný pro VNet1. Azure používá rozsahy IP adres, které určují chcete směrovat přenosy určené pro VNet1. Klikněte na zaškrtnutí vytvořit místní síti.

### <a name="add-the-local-network-site-vnet2local"></a>Přidání webu místní síti VNet2Local

Umožňuje vytvořit web místní síti "VNet2Local" výše uvedené kroky. V případě potřeby můžete odkázat na hodnoty v [příkladu nastavení](#step1) v části Krok 1.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Konfigurace každý VNet tak, aby ukazovaly na místní síti

Každý VNet musí odkazovat na příslušné místní síti, které chcete směrovat přenosy v síti na. 

#### <a name="for-vnet1"></a>Pro VNet1

1. Přejděte na stránku **Konfigurovat** pro virtuální sítě **VNet1**. 
2. Připojení k webu zaškrtněte políčko "Připojit k místní síti" a potom vyberte **VNet2Local** jako místní síti z rozevíracího seznamu. 
3. Uložte nastavení.

#### <a name="for-vnet2"></a>Pro VNet2

1. Přejděte na stránku **Konfigurovat** pro virtuální sítě **VNet2**. 
2. V části připojení k webu zaškrtněte políčko "Připojit k místní síti" a potom vyberte **VNet1Local** z rozevíracího seznamu jako místní síti. 
3. Uložte nastavení.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Krok 5 – Konfigurace brány pro každý VNet

Nakonfigurujte bránu dynamické směrování pro každou virtuální síť. Konfigurace nepodporuje statické směrování brány. Pokud používáte VNets, která byla dříve konfigurované a dynamické směrování brány, který už máte, nemusíte tento krok. Pokud jsou vaše brány statické směrování, musíte je odstraníte a znovu je vytvořte jako dynamické směrování brány. Pokud byste odstranili brány, dostane uvolnění veřejnou IP adresu přiřazenou a potřebujete vrátit a nakonfigurovat některé z místní sítě a VPN zařízení s novou veřejnou IP adresu pro nová brána.

1. Na stránce **sítě** zkontrolujte, že sloupec Stav sítě virtuální **vytvořili**.

2. Ve sloupci **název** klikněte na název svojí virtuální sítě. V tomto příkladu použijeme "VNet1".

3. Na stránce **řídicího panelu** Všimněte si, že tento VNet nemá brány ještě nakonfigurované. Zobrazí se tento stav změnit průběžně kroky pro nastavení vaší brány.

4. V dolní části stránky klikněte na **Vytvořit brány** a **Dynamické směrování**. Pokud chcete systém vás vyzve k potvrzení má brána vytvořili, klikněte na Ano.

    ![Typ brány](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Při vytváření brána Všimněte si grafiku brány na stránce se změní na žlutá a s textem "Vytváření brány". Obvykle to trvá asi 30 minut brány na vytvořit.

6. Opakujte stejný postup pro VNet2. Už není potřeba dokončit před zahájením vytvořte bránu pro vaše VNet první VNet bránu.

7. Pokud stav brány se změní na "Připojení", je zobrazen v řídicím panelu veřejnou IP adresu pro každou bránu. Poznamenejte si IP adresu, která odpovídá pro každou VNet starat pozor, abyste kombinovat. Toto jsou IP adresy, které se používají při úpravě IP adresy zástupný symbol pro zařízení virtuální privátní sítě pro každou místní síti.

## <a name="step-6---edit-the-local-network"></a>Krok 6: upravit místní síti

1. Na stránce **Místní sítě** klikněte na název místní síti název, který chcete upravit a potom klikněte na tlačítko **Upravit** v dolní části stránky. **VPN zařízení IP adresa**zadejte IP adresu brány, který odpovídá VNet. Například pro VNet1Local, vložte do pole IP adresa brány přiřazená VNet1. Klikněte na šipku ve spodní části stránky.

2. Na stránce **zadejte do adresního prostoru** klikněte na zaškrtnutí v pravé dolní bez jakýchkoli změn.

## <a name="step-7---create-the-vpn-connection"></a>Krok 7 – vytvoření připojení k síti VPN

Po dokončení předchozích kroků nastavte předem sdílené klíče IPsec/IKE a vytvořte připojení. Této sadě kroků pomocí prostředí PowerShell a na portálu se nedají konfigurovat. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell Azure. Ujistěte se, pokud si chcete stáhnout nejnovější verzi rutiny pro správu služeb (ko). 

1. Otevřete Windows PowerShell a přihlaste se.

        Add-AzureAccount

2. Vyberte předplatné, vaše VNets uložena v.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Vytvoření připojení. V příkladech Všimněte si, že sdílený klíč přesně stejné. Sdílený klíč musí vždy odpovídat.


    VNet1 VNet2 připojení

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 připojení

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Počkejte připojení inicializace. Jakmile inicializoval brány brány vypadá jako na následujícím obrázku.

    ![Stav brány - připojení](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Další kroky

Přidání virtuálních počítačích virtuální sítí. Najdete v [dokumentaci virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/) Další informace.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
