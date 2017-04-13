<properties
   pageTitle="Připojení pomocí Správce prostředků nasazení modelu a portálu Azure VNets | Microsoft Azure"
   description="Vytvořte si připojení VPN brány mezi VNets přes správce prostředků a portálu Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfigurace připojení VNet VNet pomocí portálu Azure

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasický – klasické portálu](virtual-networks-configure-vnet-to-vnet-connection.md)


Tento článek vás provede kroky k vytvoření připojení mezi VNets v modelu nasazení Správce prostředků přes VPN brány a portálu Azure.

Při použití portálu Azure připojení virtuální sítě VNets musí být v rámci stejného předplatného. Pokud virtuální sítě nacházejí v různých předplatných, můžete je stále připojit pomocí [prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) kroků.

![v2v diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modely nasazení a metody pro VNet VNet připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace VNet VNet. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Prozkoumávání VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Informace o připojení VNet VNet

Připojení k jiné síti virtuální virtuální sítě je podobný připojení VNet k umístění webu místní (VNet VNet). Oba typy připojení se k zajištění zabezpečené tunelem pomocí IPsec/IKE používají Azure VPN brány. VNets, ke kterému se můžete připojit může být v různých oblastech nebo v různých předplatných.

Můžete dokonce kombinovat VNet VNet komunikace s konfigurací více webu. Toto oprávnění umožňuje vytvořit topologie sítě, které kombinují křížově místní připojení s připojením mezi virtuální sítě, jak je znázorněno na následujícím obrázku:

![Informace o připojení] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informace o připojení")

### <a name="why-connect-virtual-networks"></a>Proč připojit virtuální sítě?

Můžete chtít připojit virtuální sítě z těchto důvodů:

- **Křížové oblast geo redundance a geo informací o stavu**
    - Můžete nastavit vlastní geo replikace a synchronizace s zabezpečené připojení nemusíte přecházet prostřednictvím internetového koncové body.
    - Správce přenosy Azure a vyrovnávání zatížení můžete nastavit vysoce dostupné úlohu s geo redundance napříč několika Azure oblastí. Příkladem důležité je nastavení SQL vždy na skupinám dostupnost šíření napříč několika Azure oblastí.

- **Místní aplikací vícevrstvého s izolace nebo administrativní hranice**
    - Ve stejné oblasti můžete nastavit vícevrstvého aplikací s více virtuální sítí propojeny kvůli izolace nebo požadavky pro správu.

Další informace o připojení VNet VNet najdete v tématu [VNet VNet – nejčastější dotazy](#faq) na konci tohoto článku.

### <a name="values"></a>Příklad nastavení

Pokud chcete použít tento postup jako cvičení, můžete hodnoty konfigurace vzorku. Pro účely příkladu budeme používat více mezer adresu pro každou VNet. Konfigurace VNet VNet však nevyžadují několika mezer adresu.

**Hodnoty pro TestVNet1:**

- Název VNet: TestVNet1
- Adresa místa: 10.11.0.0/16
    - Podsítě název: FrontEnd
    - Rozsah adres podsítí: 10.11.0.0/24
- Pole Skupina zdroje: TestRG1
- Umístění: Východní USA
- Adresa místa: 10.12.0.0/16
    - Podsítě název: back-end
    - Rozsah adres podsítí: 10.12.0.0/24
- Název brány podsítě: GatewaySubnet (tím, že se automatické vyplňování na portálu)
    - Rozsah adres podsítí brány: 10.11.255.0/27
- DNS Server: Použití IP adresu serveru DNS
- Název brány virtuální sítě: TestVNet1GW
- Typ brány: virtuální privátní sítě
- Typ VPN: na základě směrování
- SKU: Vyberte SKU brány, který chcete použít
- Veřejnou IP adresu název: TestVNet1GWIP
- Připojení hodnoty:
    - Název: TestVNet1toTestVNet4
    - Sdílený klíč: můžete vytvořit sdílený klíč sami. V tomto příkladu budeme používat abc123. Důležité je, že po vytvoření připojení mezi VNets hodnota musí odpovídat.

**Hodnoty pro TestVNet4:**

- Název VNet: TestVNet4
- Adresa místa: 10.41.0.0/16
    - Podsítě název: FrontEnd
    - Rozsah adres podsítí: 10.41.0.0/24
- Pole Skupina zdroje: TestRG1
- Umístění: Západní USA
- Adresa místa: 10.42.0.0/16
    - Podsítě název: back-end
    - Rozsah adres podsítí: 10.42.0.0/24
- Název GatewaySubnet: GatewaySubnet (tím, že se automatické vyplňování na portálu)
    - Rozsah adres GatewaySubnet: 10.41.255.0/27
- DNS Server: Použití IP adresu serveru DNS
- Název brány virtuální sítě: TestVNet4GW
- Typ brány: virtuální privátní sítě
- Typ VPN: na základě směrování
- SKU: Vyberte SKU brány, který chcete použít
- Veřejnou IP adresu název: TestVNet4GWIP
- Připojení hodnoty:
    - Název: TestVNet4toTestVNet1
    - Sdílený klíč: můžete vytvořit sdílený klíč sami. V tomto příkladu budeme používat abc123. Důležité je, že po vytvoření připojení mezi VNets hodnota musí odpovídat.


## <a name="CreatVNet"></a>1. vytváření a konfigurace TestVNet1

Pokud už máte VNet, ověřte, jestli jsou kompatibilní s návrhu brány VPN nastavení. Věnujte zvláštní pozornost podsítí, které může překrývat v jiných sítích. Pokud máte překrývající se podsítí, připojíte se nebude fungovat správně. Pokud vaše VNet nakonfigurovaný na správné nastavení, můžete začít kroků uvedených v části [Specify serveru DNS](#dns) .

### <a name="to-create-a-virtual-network"></a>Chcete-li vytvořit virtuální sítě

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. přidat další adresní prostor a podsítí

Můžete přidat další adresní prostor a podsítí po vytvoření vaší VNet.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Vytvoření podsítě brány

Před připojením virtuální sítě brány, musíte nejdřív vytvořit podsítě brány virtuální sítě, ke kterému chcete připojit. Pokud je to možné je vhodné vytvořit podsítě brány pomocí CIDR blokem /28 nebo /27 k poskytují dostatek IP adresy tak, aby zahrnoval další konfiguraci budoucí požadavky.

Pokud vytváříte tuto konfiguraci jako cvičení, podívejte se do tyto [příklady nastavení](#values) při vytváření vaší podsítě brány.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Vytvoření podsítě brány

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Zadejte server DNS (volitelné)

Pokud chcete mít překlad virtuálních počítačích nasazené na vaše VNets, měli byste určit serveru DNS.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. Vytvoření brány pro virtuální sítě

V tomto kroku vytvoříte brány virtuální sítě pro vaše VNet. Tento krok může trvat až 45 minut. Pokud vytváříte tuto konfiguraci jako cvičení, můžete odkázat na [příklady nastavení](#values).

### <a name="to-create-a-virtual-network-gateway"></a>K vytvoření virtuální sítě brány

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. vytváření a konfigurace TestVNet4

Po konfiguraci TestVNet1 vytvoříte TestVNet4 s opakováním předchozích kroků nahrazení hodnoty budou z TestVNet4. Není potřeba počkat, až brány virtuální sítě pro TestVNet1 proběhla vytváření před konfigurací TestVNet4. Pokud používáte vlastní hodnoty, ujistěte se, že adresa mezery nepřekrývaly pomocí kterékoli z VNets, který chcete připojit.


## <a name="TestVNet1Connection"></a>7. připojení TestVNet1 nakonfigurovat

Když bran virtuální sítě pro TestVNet1 a TestVNet4 dokončili, můžete vytvořit virtuální sítě brány připojení. V této části vytvoříte připojení z VNet1 k VNet4.

1. **Všechny zdroje**přejděte na brány virtuální sítě pro vaše VNet. Například **TestVNet1GW**. Klikněte na tlačítko **TestVNet1GW** otevřete zásuvné brány virtuální sítě.

    ![Připojení zásuvné] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Připojení zásuvné")

2. Klikněte na tlačítko **+ Přidat** otevřete zásuvné **Přidat připojení** .

3. Na zásuvné **připojení přidat** do pole Název zadejte název připojení. Například **TestVNet1toTestVNet4**.

    ![Název připojení] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Název připojení")

4. **Typ připojení**. rozevíracím seznamu vyberte **VNet VNet** .

5. Hodnota pole **první brány virtuální sítě** se automaticky vyplní protože vytváříte toto připojení z brány zadaný virtuální sítě.

6. V poli **druhého virtuální sítě brány** je virtuální sítě brány, který chcete vytvořit připojení k VNet. Klikněte na **zvolit jiné brány virtuální sítě** zásuvné **Zvolte virtuální sítě brány** .

    ![Přidání připojení] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Přidání připojení")

7. Zobrazení brány virtuální sítě, které jsou uvedené v tomto zásuvné. Všimněte si, že jsou uvedeny pouze bran virtuální sítě, které jsou ve vašem předplatném. Pokud chcete připojit k Brána virtuální sítě, která není součástí vašeho předplatného, stiskněte klávesovou zkratku [článek Powershellu](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klikněte na položku Brána virtuální sítě, který chcete připojit.
 
9. V poli **sdílené klíč** zadejte sdílený klíč pro připojení. Můžete vygenerovat nebo vytvoření tohoto klíče. Připojení k webu klíč, který používáte by být přesně stejné pro zařízení s místním a virtuální síťové připojení brány. Koncept se podobá tady, s výjimkou, spíše než připojení prostřednictvím sítě VPN zařízení, se připojujete k bráně pro jiné virtuální sítě.

    ![Sdílené klíč] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Sdílené klíč")

10. Klikněte na **OK** v dolní části zásuvné uložte provedené změny.

## <a name="TestVNet4Connection"></a>8. připojení TestVNet4 nakonfigurovat

Dále vytvořte připojení z TestVNet4 TestVNet1. Použití stejným způsobem, jaký jste použili k vytvoření připojení z TestVNet1 TestVNet4. Ujistěte se, pomocí klávesy stejné sdílené.

## <a name="VerifyConnection"></a>9. připojení k ověření

Ověřte připojení. Pro každou virtuální sítě bránu postupujte takto:

1. Vyhledejte zásuvné brány virtuální sítě. Například **TestVNet4GW**. 
2. Na zásuvné brány virtuální sítě klikněte na **připojení** zobrazíte zásuvné připojení brány virtuální sítě.

Podívejte se připojení a zkontrolujte stav. Po vytvoření připojení uvidíte **byl úspěšný** a **Připojit** jako hodnoty stavu.

![Úspěšně] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Úspěšně")

Poklikejte na každou připojení samostatně zobrazíte další informace o připojení.

![Základy] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Základy")

## <a name="faq"></a>Nejčastější dotazy týkající se VNet VNet

Zobrazení podrobností o nejčastější dotazy týkající se další informace o připojení VNet VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Další kroky

Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
