<properties
   pageTitle="Konfigurace připojení brány čárky webu VPN k síti virtuální Azure pomocí portálu Azure | Microsoft Azure"
   description="Zabezpečené připojení k síti virtuální Azure vytvořením čárky webu Navažte spojení brány pomocí portálu Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurace připojení čárky webu k VNet pomocí portálu Azure

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický - Azure portálu](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Tento článek vás provede vytvoříte VNet s připojení čárky webu v modelu klasické nasazení pomocí portálu Azure. Konfigurace čárky webu (P2S) umožňuje vytvářet zabezpečené připojení z jednotlivých klientského počítače k síti virtuální. Připojení P2S je užitečná, když se chcete připojit k VNet ze vzdáleného místa, jako je třeba z domovské nebo konference. Nebo pokud máte jenom několik klienti, které je potřeba připojení k síti virtuální.

Připojení k čárky serveru nevyžadují zařízení virtuální privátní sítě nebo veřejné IP adresu pro práci. Vytvoření připojení VPN spuštěním připojení z klientského počítače. Další informace o připojení k bodu na serveru podívejte se na [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) a [O brány VPN](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely nasazení a metody pro P2S připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka obsahuje dva modely nasazení a k dispozici nasazení metody konfigurace P2S. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Základní pracovního postupu 

![Bodu na--diagram webů] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "bod webu")


V následujících částech vás provede jednotlivými kroky k vytvoření zabezpečené čárky webu připojení k síti virtuální. 

1. Vytvořit virtuální sítě a Brána VPN
2. Vytvoření certifikátů
3. Nahrajte soubor .cer
4. Vytvoření balíčku konfigurace klienta VPN
5. Konfigurace klientského počítače
6. Připojení k Azure

### <a name="example-settings"></a>Příklad nastavení

Můžete použít následující příklady nastavení:

- **Název: VNet1**
- **Adresa místa: 192.168.0.0/16**
- **Podsítě název: FrontEnd**
- **Rozsah adres podsítí: 192.168.1.0/24**
- **Předplatné:** Pokud máte víc předplatných, ověřte, že používáte správná.
- **Pole Skupina zdroje: TestRG**
- **Umístění: Východní USA**
- **Typ připojení: čárky webu**
- **Adresního prostoru klienta: 172.16.201.0/24**. Klienti VPN, která se připojují k VNet pomocí tohoto připojení čárky webu přijímat k IP adrese z fondu zadaný.
- **GatewaySubnet: 192.168.200.0/24**. Podsítě brány musí používat název "GatewaySubnet".
- **Velikost:** Vyberte brány SKU, který chcete použít.
- **Směrování typ: dynamické**

## <a name="vnetvpn"></a>Část 1 - vytvořit virtuální sítě a Brána VPN

### <a name="createvnet"></a>Část 1: Vytvoření virtuální sítě

Pokud ještě nemáte virtuální sítě, vytvořte ho. Snímky obrazovek jsou uvedeny příklady. Ujistěte se, že nahradit hodnoty vlastní. Vytvoření VNet pomocí portálu Azure, pomocí následujících kroků: 

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby Přihlaste se pomocí účtu Azure.

2. Klikněte na **Nový**. Do pole **Hledat Tržiště** text "Virtuální síť". Vyhledejte **Virtuální sítě** ze seznamu vrácené a klepnutím otevřete zásuvné **Virtuální sítě** .

    ![Vyhledejte zásuvné virtuální sítě] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Vyhledejte zásuvné virtuální sítě")

3. V dolní části zásuvné virtuální sítě ze seznamu **Vyberte nasazení modelu** vyberte **klasické**a potom klikněte na **vytvořit**.

    ![Vyberte model nasazení] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Vyberte model nasazení")

4. Na zásuvné **vytvořit virtuální síť** nakonfigurujte nastavení VNet. V tomto zásuvné budete přidávat první adresní prostor a oblasti jednoho podsítě adresu. Po vytvoření VNet se vraťte a přidání dalších podsítí a adresa mezery.

    ![Vytvořit virtuální sítě zásuvné] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Vytvořit virtuální sítě zásuvné")

5. Ověřte, že **předplatné** je ten správný. Předplatné můžete změnit pomocí rozevíracího seznamu.

6. Klikněte na **pole Skupina zdroje** a buď vyberte existující skupiny zdrojů nebo vytvořte novou tak, že zadáte název nové skupiny prostředků. Pokud vytváříte novou skupinu, zadejte název skupiny zdroje podle hodnot plánované konfigurace. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Potom vyberte nastavení **umístění** pro váš VNet. Umístění bude určovat, kde se bude nacházet prostředky, které můžete nasadit do této VNet.

8. Vyberte **Připnout na řídicí panel** , pokud chcete, aby mohli snáz našli váš VNet na řídicím panelu a potom klikněte na **vytvořit**.
    
    ![Připnout do řídicího panelu] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Připnout do řídicího panelu")


9. Po kliknutí na příkaz Vytvořit, zobrazí se na dlaždici na řídicím panelu, které budou obsahovat průběh vašeho VNet. Na dlaždici se mění VNet se vytváří.

    ![Vytváření virtuálních sítí dlaždice] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Vytváření virtuálních sítí dlaždice")

10. Po vytvoření virtuální sítě přidáním IP adresu serveru DNS pro zpracování překlad. Otevřete nastavení virtuální sítě, klikněte na položku servery DNS a přidejte IP adresu serveru DNS, který chcete použít. Toto nastavení nevytvoří nový DNS server. Je potřeba přidat server DNS, který zdroje můžete komunikovat.

Po vytvoření virtuální sítě uvidíte, že **vytvořeno** zařazený do kategorie **Stav** na stránce sítí Azure klasické portálu.

### <a name="gateway"></a>Část 2: Vytvoření podsítí brány a dynamické směrování brány

V tomto kroku vytvoříte podsítí brány a dynamické směrování brány. Na portálu Azure pro klasické nasazení modelu vytváření podsítě brány a brány lze provést pomocí stejného konfigurace listy.

1. Na portálu přejděte virtuální sítě, u kterého chcete vytvořit brány.

2. Na zásuvné virtuální sítě, na zásuvné **Přehled** v části připojení VPN klikněte na **brány**.

    ![Vytvoření brány po kliknutí sem] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Vytvoření brány po kliknutí sem")


3. Na **Nové připojení VPN** zásuvné vyberte **bodu web**.

    ![Typ P2S připojení] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "Typ P2S připojení")

4. **Klient adresní prostor**pro přidání rozsah IP adres. Toto je oblast, ze které klienti VPN, dostanou k IP adrese při připojení. Odstranit oblast vyplní automaticky a přidejte své vlastní.

    ![Adresního prostoru klienta] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Adresního prostoru klienta")

5. Zaškrtněte políčko **vytvořit brány okamžitě** .

    ![Vytvoření brány okamžitě] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Vytvoření brány okamžitě")

6. Klikněte na **konfigurační volitelné brány** otevřete zásuvné **Konfigurace brány** .

    ![Klikněte na položku konfigurace volitelné brány] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Klikněte na položku konfigurace volitelné brány")

7. Klepněte na tlačítko Přidat **brány podsítě** **podsítě konfigurace požadované nastavení** . Když je možné vytvořit brány podsítě nejnižší /29, doporučujeme vytvoření větší podsítě obsahující víc adres tak, že vyberete aspoň /28 nebo /27. Toto nastavení umožňuje dost adresy tak, aby pokryly možné konfigurace další požadované může v budoucnosti.

    >[AZURE.IMPORTANT] Když pracujete s podsítí brány, snažte se přidružení skupinu zabezpečení sítí (NSG) podsítě brány. Přidružení sítě zabezpečení skupinu, do které tento podsítě tuto chybu může způsobovat bránu VPN přestanou fungovat správně. Další informace o skupiny zabezpečení sítě najdete v tématu [Co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)

    ![Přidání GatewaySubnet] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Přidání GatewaySubnet")

8. Vyberte brány **velikost**. Toto je brány SKU, který budete používat k vytvoření brány virtuální sítě. Na portálu je **základní**SKU výchozí. Další informace o brány skladové jednotky najdete v článku [O VPN brány nastavení](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![velikost brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Vyberte **Typ směrování** brány. Konfigurace P2S vyžadují typu **dynamické** směrování. Po dokončení konfigurace tento zásuvné, klikněte na tlačítko **OK** .

    ![Konfigurace směrování typ] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Konfigurace směrování typ")

10. Na zásuvné **Nové připojení VPN** klikněte na **OK** v dolní části zásuvné začít vytvářet brány virtuální sítě. To může trvat až 45 minut. 


## <a name="generatecerts"></a>Část 2: vygenerování certifikátů

Certifikáty jsou používány Azure k ověřování klientů virtuální privátní sítě VPN čárky webu. Jak Base 64 zakódovaný soubor .cer X.509 z kořenového certifikátu generovaného při řešení certifikát enterprise nebo certifikát podepsaný svým držitelem kořenového exportu dat certifikát veřejného (ne privátním klíčem). Potom importujete data veřejné certifikátu z kořenového certifikátu do Azure. Navíc budete muset generovat certifikát klient z kořenového certifikátu pro klienty. Každého klienta, který potřebují připojit k virtuální sítě pomocí připojení P2S musíte mít nainstalovaný certifikát klienta, který vygenerované z kořenového certifikátu.

### <a name="cer"></a>Část 1: Získání soubor .cer kořenového certifikátu


Pokud používáte řešení enterprise, můžete použít existující řetěz certifikátů. Pokud nepoužíváte licenci enterprise řešení CA, můžete vytvořit certifikát podepsaný svým držitelem kořenové. Jednou z možností pro vytvoření certifikátu podepsaného svým držitelem je makecert.

- Pokud používáte systém certifikátů enterprise, získejte soubor .cer kořenového certifikátu, kterou chcete použít. 

- Pokud nepoužíváte řešení certifikát enterprise, budete muset generovat certifikát podepsaný svým držitelem kořenového. Postup v systému Windows 10 můžete odkázat při [práci se vlastních kořenových certifikátů pro konfigurace čárky webu](vpn-gateway-certificates-point-to-site.md).

1. Soubor .cer z certifikátu zobrazíte otevřete **text certmgr.msc** a vyhledejte kořenového certifikátu. Klikněte pravým tlačítkem certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem**.

2. V průvodci klikněte na **Další**, vyberte **Ne, nebudou exportovány privátním klíčem**a klikněte na tlačítko **Další**.

3. Na stránce **Formát souboru pro Export** vyberte **základu-64 formát X.509 (. CER).** Potom klikněte na **Další**. 

4. **Soubor k exportu**, **přejděte** do umístění, do kterého chcete exportovat certifikát. **Název souboru**zadejte název souboru certifikát. Klepněte na tlačítko **Další**.

5. Kliknutím na tlačítko **Dokončit** exportujte certifikát.


### <a name="genclientcert"></a>Část 2: Vygenerování certifikát klienta

Můžete buď generovat jedinečné certifikát pro každého klienta, který propojí nebo na více klientů můžete použít stejný certifikát. Výhodou generování jedinečné certifikátů je možnost zrušit certifikát jednu v případě potřeby. Jinak Pokud všichni používají stejný certifikát klienta a zjistíte, že potřebujete odvolat certifikát pro jednoho klienta, musíte se generovat a instalovat nové certifikáty pro všechny klienty, kteří používají tento certifikát ověření.

- Pokud používáte řešení certifikát enterprise, generovat certifikát klienta s běžné formát hodnoty název 'name@yourdomain.com', místo formátu "název_domény\uživatelské_jméno". 

- Pokud používáte certifikátu podepsaného svým držitelem, najdete v článku [práce s vlastních kořenových certifikátů pro bod webu konfigurace](vpn-gateway-certificates-point-to-site.md) generovat certifikát klienta.

### <a name="exportclientcert"></a>Část 3: Exportovat certifikát klienta

Nainstalujte certifikát klienta na každém počítači, který chcete připojit k virtuální sítě. Certifikát klienta požaduje ověření. Můžete automatizovat instalace certifikátu klienta nebo ho můžete nainstalovat ručně. Následující kroky vám pomohou při exportu a ruční instalace certifikátu klienta.

1. Chcete-li exportovat certifikát klienta, můžete použít *příkaz certmgr.msc*. Klikněte pravým tlačítkem myši klienta certifikát, který chcete exportovat, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**.
2. Exportujte certifikát klienta soukromým klíčem. Toto je soubor *.pfx* . Zkontrolujte, že záznam nebo heslo (klíč), kterou jste nastavili pro tento certifikát.

## <a name="upload"></a>Část 3 – nahrát soubor .cer kořenového certifikátu

Po vytvoření brány můžete na Azure nahrát soubor .cer kořenových certifikátů. Můžete nahrávat soubory až 20 kořenových certifikátů. Není soukromý klíč pro kořenového certifikátu do nahrajete Azure. Po nahrání soubor .cer Azure pomocí ho ověřování klientů, kteří se připojují k virtuální sítě.

1. V části **připojení VPN** zásuvné pro vaše VNet klikněte na obrázek **klientům** připojení VPN otevřít **bodu server** zásuvné.

    ![Klienti] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Klienti")

2. Připojení **bodu webu** zásuvné, klikněte na tlačítko **spravovat certifikáty** otevřete zásuvné **certifikáty** .<br>

    ![Certifikáty zásuvné](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Na zásuvné **certifikátů** klikněte na **Nahrát** otevřete zásuvné **Odeslat certifikát** .<br>

    ![Nahrání zásuvné certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Klikněte na složku obrázek procházením vyhledejte soubor .cer. Vyberte soubor a potom klikněte na **OK**. Po aktualizaci stránky zobrazíte nahraný certifikátů na zásuvné **certifikáty** .

    ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Část 4 - generovat balíček konfigurace klienta VPN

Připojení k virtuální sítě, bude potřeba nakonfigurovat klienta VPN. Klientský počítač vyžaduje certifikát klienta a VELKÁ2 balíček konfigurace klienta VPN než budete moct připojit.

Balíček klienta VPN obsahuje informace o konfiguraci ke konfiguraci sítě VPN klientský software integrovaná v systému Windows. Balíček nenainstaluje další software. Nastavení jsou specifické pro virtuální sítě, ke které se chcete připojit. Seznam operačních systémů klienta, které podporují najdete v tématu připojení [bodu-webu](vpn-gateway-vpn-faq.md#point-to-site-connections) část Nejčastější dotazy týkající se VPN brány. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Generování balíček konfigurace klienta VPN

1. Na portálu Azure v zásuvné **základní informace** pro vaší VNet v **sítě VPN**, klikněte na obrázek klienta připojení VPN otevřít **bodu server** zásuvné.
2. V horní části **bodu sítě VPN připojení** zásuvné, klepněte na Stáhnout balíček, který odpovídá operačního systému klienta, na kterém budou nainstalovaná:

 - Pro klienty 64bitovou verzi vyberte **Klienta VPN (64bitová verze)**.
 - Pro 32bitové klienty vyberte **Klienta VPN (32bitový)**.

    ![Stáhnout balíček konfigurace klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Zobrazí se zpráva, že Azure vygenerovala balíček konfigurace klienta VPN pro virtuální sítě. Po několika minutách vytvářejí balíčku a zobrazí se zpráva do místního počítače, aby byly Stáhnout balíček. Uložte soubor balíčku konfigurace. Nainstaluje to každý klientského počítače, který se připojí k virtuální sítě pomocí P2S.


## <a name="clientconfiguration"></a>Oddíl 5: Konfigurace klientského počítače

### <a name="part-1-install-the-client-certificate"></a>Část 1: Nainstalovat certifikát klienta

Každý klientský počítač, musíte mít certifikát klienta pro ověření. Při instalaci certifikátu klienta, budete potřebovat heslo, které byly nastavené při exportu certifikát klienta.

1. Zkopírujte soubor .pfx klientského počítače.
2. Poklikejte na soubor .pfx případně ji nainstalujte. Neměňte umístění instalace.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Část 2: Nainstalovat VPN klientského konfigurace

Za předpokladu, že verze odpovídá architektura pro klienta, můžete použít stejný balíček konfigurace klienta VPN v každém klientském počítači.

1. Zkopírujte soubor konfigurace místně do počítače, na kterém chcete připojení k síti virtuální a poklikejte na souboru .exe. 

2. Po nainstaloval balíček, můžete začít připojení VPN. Konfigurace balíček není podepsán společností Microsoft. Může chcete podepsat balíček pomocí služby podpisový vaší organizace nebo přihlásit sami pomocí [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Je v pořádku použil funkci balíček bez přihlášení. Ale pokud balíček není podepsán, upozornění se zobrazí po instalaci balíček.

3. V klientském počítači přejděte na **Nastavení sítě** a klepněte na položku **VPN**. Zobrazí se připojení uvedené. Zobrazuje název virtuální sítě, že se připojí k a bude vypadat nějak takto: 

    ![Klienta VPN] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "Klient VNet VPN")

## <a name="connect"></a>Oddíl 6 - připojení k Azure

### <a name="connect-to-your-vnet"></a>Připojení k vaší VNet

1. Připojení k vaší VNet v klientském počítači přejděte na připojení VPN a najděte připojení VPN, které jste vytvořili. Je název stejný název jako virtuální sítě. Klikněte na **Připojit**. Zobrazí se zpráva se může zobrazit, které odkazuje na pomocí certifikátu. V takovém případě klikněte na **pokračovat** a použít zvýšenými oprávněními. 

2. Na stránce Stav **připojení** klikněte na **Připojit** k zahájení připojení. Pokud se zobrazí na obrazovce **Vyberte certifikát** , zkontrolujte, že klientský certifikát zobrazující té, která chcete použít pro připojení. Pokud není, použít na šipku rozevíracího seznamu vyberte správný certifikát a klikněte na **OK**.

    ![Připojení] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "Připojení klienta VPN")

3. Připojení k teď možné stanovit.

    ![Založené připojení] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Připojení")

### <a name="verify-the-vpn-connection"></a>Ověřte připojení VPN

1. Ověřte, jestli je vaše připojení VPN aktivní, otevřete příkazový řádek se zvýšenými oprávněními a spusťte *ipconfig/vše*.
2. Zobrazte výsledky. Všimněte si, že na IP adresu, kterou jste dostali adres rozsahu čárky webu připojení adres, že jste zadali při vytváření vašeho VNet. Výsledky by měly být něco podobného:

Příklad:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Další kroky

Virtuálních počítačích můžete přidat k síti virtuální. Zjistěte, [jak vytvořit vlastní virtuálního počítače](../virtual-machines/virtual-machines-windows-classic-createportal.md).