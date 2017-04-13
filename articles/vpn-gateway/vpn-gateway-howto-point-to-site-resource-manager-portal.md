<properties 
   pageTitle="Konfigurace připojení VPN čárky webu brány virtuální sítě pomocí Správce prostředků nasazení modelu a portálu Azure | Microsoft Azure"
   description="Zabezpečené připojení k síti virtuální Azure vytvořením čárky webu Navažte spojení brány pomocí Správce zdrojů a portálu Azure."
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
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurace připojení čárky webu k VNet pomocí portálu Azure

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický - Azure portálu](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfigurace čárky webu (P2S) umožňuje vytvářet zabezpečené připojení z jednotlivých klientského počítače k síti virtuální. Připojení P2S je užitečná, když se chcete připojit k VNet ze vzdáleného místa, jako je třeba z domovské nebo konference, nebo když stačí jenom pár klienti, které je potřeba připojení k síti virtuální. 

Připojení k čárky serveru nevyžadují zařízení virtuální privátní sítě nebo veřejné IP adresu pro práci. Vytvoření připojení VPN spuštěním připojení z klientského počítače. Další informace o připojení k bodu na serveru podívejte se na [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) a [plánování a návrh](vpn-gateway-plan-design.md).

Tento článek vás provede vytvoříte VNet s připojení čárky webu v modelu nasazení Správce prostředků pomocí portálu Azure.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely nasazení a metody pro P2S připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka obsahuje dva modely nasazení a k dispozici nasazení metody konfigurace P2S. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Základní pracovního postupu 

![Bodu na--diagram webů] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "bod webu")

### <a name="example"></a>Příklady hodnot

- **Název: VNet1**
- **Adresa místa: 192.168.0.0/16**<br>V tomto příkladu budeme používat pouze jeden adresní prostor. Mít víc než jednu mezeru adresu pro vaše VNet.
- **Podsítě název: FrontEnd**
- **Rozsah adres podsítí: 192.168.1.0/24**
- **Předplatné:** Pokud máte víc předplatných, ověřte, že používáte správná.
- **Pole Skupina zdroje: TestRG**
- **Umístění: Východní USA**
- **GatewaySubnet: 192.168.200.0/24**
- **Název brány virtuální sítě: VNet1GW**
- **Typ brány: virtuální privátní sítě**
- **Typ VPN: na základě směrování**
- **Veřejnou IP adresu: VNet1GWpip**
- **Typ připojení: čárky webu**
- **Fond adresa klienta: 172.16.201.0/24**<br>Klienti VPN, která se připojují k VNet pomocí tohoto připojení čárky webu přijímat k IP adrese z fondu adresu klienta.

## <a name="before-beginning"></a>Před zahájením

- Zkontrolujte, jestli máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Část 1 – vytvořit virtuální sítě

Pokud vytváříte tuto konfiguraci jako cvičení, můžete odkázat na [Příklady hodnot](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. přidat další adresní prostor a podsítí

Můžete přiřadit další adresní prostor a podsítí vaší VNet po jeho vytvoření.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. Vytvoření podsítě brány

Před připojením virtuální sítě brány, musíte nejdřív vytvořit podsítě brány virtuální sítě, ke kterému chcete připojit. Pokud je to možné je vhodné vytvořit podsítě brány pomocí CIDR blokem /28 nebo /27 k poskytují dostatek IP adresy tak, aby zahrnoval další konfiguraci budoucí požadavky.

Snímky obrazovek v této části jsou uvedeny jako příklad odkazu. Ujistěte se, pomocí služby rozsah GatewaySubnet adres, které odpovídá požadované hodnoty pro konfiguraci.

#### <a name="to-create-a-gateway-subnet"></a>Vytvoření podsítě brány

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Zadejte server DNS (volitelné)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Část 2 – vytvořit virtuální sítě brány

Bod webu připojení vyžadují následující nastavení:

- Typ brány: virtuální privátní sítě
- Typ VPN: na základě směrování

### <a name="to-create-a-virtual-network-gateway"></a>K vytvoření virtuální sítě brány

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Část 3 – generovat certifikátů

Certifikáty jsou používány Azure k ověřování klientů virtuální privátní sítě VPN čárky webu. Jak Base 64 zakódovaný soubor .cer X.509 z kořenového certifikátu generovaného při řešení certifikát enterprise nebo certifikát podepsaný svým držitelem kořenového exportu dat certifikát veřejného (ne privátním klíčem). Potom importujete data veřejné certifikátu z kořenového certifikátu do Azure. Navíc budete muset generovat certifikát klient z kořenového certifikátu pro klienty. Každého klienta, který potřebují připojit k virtuální sítě pomocí připojení P2S musíte mít nainstalovaný certifikát klienta, který vygenerované z kořenového certifikátu.

### <a name="getcer"></a>1. soubor .cer kořenového certifikátu získejte

Pokud používáte řešení enterprise, můžete použít existující řetěz certifikátů. Pokud nepoužíváte licenci enterprise řešení CA, můžete vytvořit certifikát podepsaný svým držitelem kořenové. Jednou z možností pro vytvoření certifikátu podepsaného svým držitelem je makecert.

- Pokud používáte systém certifikátů enterprise, získejte soubor .cer kořenového certifikátu, kterou chcete použít. 

- Pokud nepoužíváte řešení certifikát enterprise, budete muset generovat certifikát podepsaný svým držitelem kořenového. Postup v systému Windows 10 můžete odkázat při [práci se vlastních kořenových certifikátů pro konfigurace čárky webu](vpn-gateway-certificates-point-to-site.md).

1. Soubor .cer z certifikátu zobrazíte otevřete **text certmgr.msc** a vyhledejte kořenového certifikátu. Klikněte pravým tlačítkem certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem**.

2. V průvodci klikněte na **Další**, vyberte **Ne, nebudou exportovány privátním klíčem**a klikněte na tlačítko **Další**.

3. Na stránce **Formát souboru pro Export** vyberte **základu-64 formát X.509 (. CER).** Potom klikněte na **Další**. 

4. **Soubor k exportu**, **přejděte** do umístění, do kterého chcete exportovat certifikát. **Název souboru**zadejte název souboru certifikát. Klepněte na tlačítko **Další**.

5. Kliknutím na tlačítko **Dokončit** exportujte certifikát.

### <a name="generateclientcert"></a>2. generovat certifikát klienta

Můžete buď generovat jedinečné certifikát pro každého klienta, který propojí nebo na více klientů můžete použít stejný certifikát. Výhodou generování jedinečné certifikátů je možnost zrušit certifikát jednu v případě potřeby. Jinak Pokud všichni používají stejný certifikát klienta a zjistíte, že potřebujete odvolat certifikát pro jednoho klienta, musíte se generovat a instalovat nové certifikáty pro všechny klienty, kteří používají tento certifikát ověření.

- Pokud používáte řešení certifikát enterprise, generovat certifikát klienta s běžné formát hodnoty název 'name@yourdomain.com', místo formátu "název_domény\uživatelské_jméno". 

- Pokud používáte certifikátu podepsaného svým držitelem, najdete v článku [práce s vlastních kořenových certifikátů pro bod webu konfigurace](vpn-gateway-certificates-point-to-site.md) generovat certifikát klienta.

### <a name="exportclientcert"></a>3. exportujte certifikát klienta

Certifikát klienta požaduje ověření. Po generování certifikát klienta, exportujte. Certifikát klienta, který exportujete nainstaluje později v každém klientském počítači.

1. Chcete-li exportovat certifikát klienta, můžete použít *příkaz certmgr.msc*. Klikněte pravým tlačítkem myši klienta certifikát, který chcete exportovat, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**.
2. Exportujte certifikát klienta soukromým klíčem. Toto je soubor *.pfx* . Zkontrolujte, že záznam nebo heslo (klíč), kterou jste nastavili pro tento certifikát.

## <a name="addresspool"></a>Část 4 – přidání fondu adresu klienta

1. Po vytvoření virtuální sítě brány přejděte do části **Nastavení** zásuvné brány virtuální sítě. V části **Nastavení** klikněte na položku **bodu webu konfigurace** otevřete zásuvné **Konfigurace** .

    ![přejděte na zásuvné webu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "přejděte na zásuvné webu")

2. **Fond adres** je IP adres ze kterých dostanou klientům, která se připojují IP adresu. Přidejte adresu fondu a klikněte na tlačítko **Uložit**.

    ![fond adresu klienta] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "fond adresu klienta")

## <a name="uploadfile"></a>Část 5 – nahrávat soubor .cer kořenového certifikátu

Po vytvoření brány můžete na Azure nahrát soubor .cer kořenových certifikátů. Můžete nahrávat soubory až 20 kořenových certifikátů. Není soukromý klíč pro kořenového certifikátu do nahrajete Azure. Po nahrání soubor .cer Azure pomocí ho ověřování klientů, kteří se připojují k virtuální sítě.

1. Přejděte na konfigurace **bodu webu** zásuvné. V části **kořenového certifikátu** tento zásuvné přidáte soubor .cer.

    ![přejděte na zásuvné webu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "přejděte na zásuvné webu")

2. Ujistěte se, exportovat kořenového certifikátu, protože Base 64 zakódovaný soubor X.509 (.cer). Potřebujete exportovat ve formátu, takže můžete otevřít certifikát v textovém editoru.
3. Otevřete certifikát v textovém editoru, jako je Poznámkový blok. Zkopírujte pouze v následující části:

    ![data certifikátu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "data certifikátu")

4. Vložte data certifikátu do části **Veřejných certifikát dat** na portálu. Umístění název certifikátu do pole **název** a potom klikněte na **Uložit**. Můžete přidat až 20 kořenových certifikátů.

    ![nahrání certifikátu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "nahrání certifikátu")

## <a name="clientconfig"></a>Část 6 – stáhnout a nainstalovat klienta VPN konfigurace

Připojení klientů k Azure pomocí P2S musí mít certifikát klienta i balíček konfigurace klienta VPN nainstalovaný. Balíčky konfigurace klienta VPN jsou k dispozici pro klienty Windows. 

Balíček klienta VPN obsahuje informací ke konfiguraci sítě VPN klientský software, která je integrovaná v systému Windows. Konfigurace je specifické pro VPN, který chcete připojit. Balíček nenainstaluje další software. V tématu [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) Další informace.

1. Ke konfiguraci **bodu webu** zásuvné, klikněte na **Stáhnout VPN klientského** otevřete zásuvné **klienta VPN stáhnout** .

    ![Stažení klienta VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Stažení klienta VPN")

2. Vyberte správný balíček pro vašeho klienta a potom klikněte na **Stáhnout**. Pro klienty 64bitová verze vyberte **AMD64**. Pro 32bitové klienty vyberte **x86**.

3. Nainstalujte balíček v klientském počítači. Pokud se zobrazí místní SmartScreen, klikněte na **Další informace**, abyste mohli nainstalovat balíček **přesto spustit** .

4. V klientském počítači přejděte na **Nastavení sítě** a klepněte na položku **VPN**. Zobrazí se připojení uvedené. Zobrazí název virtuální sítě, která se připojí k a vypadá podobně jako tento příklad: 

    ![Klienta VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "Klienta VPN")

## <a name="installclientcert"></a>Část 7 – nainstalovat certifikát klienta

Každý klientský počítač, musíte mít certifikát klienta pro ověření. Při instalaci certifikátu klienta, budete potřebovat heslo, které byly nastavené při exportu certifikát klienta.

1. Zkopírujte soubor .pfx klientského počítače.
2. Poklikejte na soubor .pfx případně ji nainstalujte. Neměňte umístění instalace.

## <a name="connect"></a>Část 8 – připojení k Azure

1. Připojení k vaší VNet v klientském počítači přejděte na připojení VPN a najděte připojení VPN, které jste vytvořili. Je název stejný název jako virtuální sítě. Klikněte na **Připojit**. Zobrazí se zpráva se může zobrazit, které odkazuje na pomocí certifikátu. V takovém případě klikněte na **pokračovat** a použít zvýšenými oprávněními. 

2. Na stránce Stav **připojení** klikněte na **Připojit** k zahájení připojení. Pokud se zobrazí na obrazovce **Vyberte certifikát** , zkontrolujte, že klientský certifikát zobrazující té, která chcete použít pro připojení. Pokud není, použít na šipku rozevíracího seznamu vyberte správný certifikát a klikněte na **OK**.

    ![Klient 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Připojení klienta VPN")

3. Připojení k teď možné stanovit.

    ![Klienta VPN 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Připojení klienta VPN 2")

## <a name="verify"></a>Část 9 – zkontrolujte připojení

1. Ověřte, jestli je vaše připojení VPN aktivní, otevřete příkazový řádek se zvýšenými oprávněními a spusťte *ipconfig/vše*.

2. Zobrazte výsledky. Všimněte si, že na IP adresu, kterou jste dostali jednu z adresy v rámci fond adresu klienta VPN čárky webu, který určenou v konfiguraci. Výsledky by měly být něco podobného:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Přidání nebo odebrání důvěryhodného kořenových certifikátů

Odebrání důvěryhodného kořenového certifikátu z Azure. Při odebrání důvěryhodnou certifikační klientské certifikáty, které byly vytvořeny z kořenového certifikátu už bude moct připojit k Azure pomocí čárky webu. Pokud chcete připojení klientů, budou muset nainstalovat certifikát nového klienta, vygenerovaný z certifikát, který je v Azure důvěryhodné.

Spravovat seznam odvolaných certifikátů ke konfiguraci **bodu webu** zásuvné. Toto je zásuvné, který jste použili k [nahrání kořenových certifikátů](#uploadfile).

## <a name="revokeclient"></a>Spravovat seznam odvolaných certifikátů

Můžete odvolání certifikátů. Seznam odvolaných certifikátů umožňuje selektivně Odepřít připojení čárky webu založené na jednotlivé klientské certifikáty. Pokud odeberete .cer certifikát kořenové z Azure, odebere přístupu pro všechny certifikátů generovaných/přihlášení pomocí odvolaných kořenových certifikátů. Pokud chcete odvolat certifikát konkrétní klienta, ne kořenové, můžete udělat tak. Tímto způsobem, který bude tato aplikace certifikáty, které byly vytvořeny z kořenového certifikátu platit. 

Běžné praktického cvičení je umožňuje spravovat přístup na úrovni týmu nebo organizace, při používání odvolaných certifikátů pro řízení přístupu jemně odstupňovaná k jednotlivým uživatelům kořenového certifikátu.

Spravovat seznam odvolaných certifikátů ke konfiguraci **bodu webu** zásuvné. Toto je zásuvné, který jste použili k [nahrání kořenových certifikátů](#uploadfile).


## <a name="next-steps"></a>Další kroky

Přidání virtuálního počítače k síti virtuální. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .