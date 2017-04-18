<properties 
   pageTitle="Konfigurace připojení VPN čárky webu brány na virtuální sítě pomocí Správce prostředků nasazení modelu | Microsoft Azure"
   description="Zabezpečené připojení k síti virtuální Azure vytvořením brány připojení VPN čárky webu."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurace připojení čárky webu k VNet pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický - Azure portálu](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfigurace čárky webu (P2S) umožňuje vytvářet zabezpečené připojení z jednotlivých klientského počítače k síti virtuální. Připojení P2S je užitečná, když se chcete připojit k VNet ze vzdáleného místa, jako je třeba z domovské nebo konference, nebo když stačí jenom pár klienti, které je potřeba připojení k síti virtuální. 

Připojení k čárky serveru nevyžadují zařízení virtuální privátní sítě nebo veřejné IP adresu pro práci. Vytvoření připojení VPN spuštěním připojení z klientského počítače. Další informace o připojení k bodu na serveru podívejte se na [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) a [plánování a návrh](vpn-gateway-plan-design.md). 

Tento článek vás provede vytvoříte VNet s připojení čárky webu v modelu nasazení Správce prostředků pomocí Powershellu.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely nasazení a metody pro P2S připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka obsahuje dva modely nasazení a k dispozici nasazení metody konfigurace P2S. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Základní pracovního postupu 

![Bodu na--diagram webů] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "bod webu")

V tomto scénáři vytvoříte virtuální sítě s připojením čárky webu. Pokyny vám taky pomůže generovat certifikátech, které jsou potřeba pro tuto konfiguraci. Připojení P2S se skládá z následujících položek: VNet A s Brána VPN, soubor .cer kořenového certifikátu (veřejný klíč), certifikát klienta a konfiguraci sítě VPN na straně klienta. 

V této konfiguraci používáme následující hodnoty. V oddílu [1](#declare) v článku nastavení proměnné. Můžete buď jako walk-through postupujte podle pokynů a použijte hodnoty beze změny je nebo změnit tak, aby odrážela prostředí. 

### <a name="example"></a>Příklady hodnot

- **Název: VNet1**
- **Adresa místa: 192.168.0.0/16** a **10.254.0.0/16**<br>V tomto příkladu použijeme víc adresní prostor znázorňující, že tuto konfiguraci fungovat s několika mezer adresu. Více mezer adresu však není povinný pro tuto konfiguraci.
- **Podsítě název: FrontEnd**
    - **Rozsah adres podsítí: 192.168.1.0/24**
- **Podsítě název: back-end**
    - **Rozsah adres podsítí: 10.254.1.0/24**
- **Podsítě název: GatewaySubnet**<br>Název podsítě *GatewaySubnet* je povinný brány VPN pracovat.
    - **Rozsah adres podsítí: 192.168.200.0/24** 
- **Fond adresu klienta VPN: 172.16.201.0/24**<br>Klienti VPN, která se připojují k VNet pomocí tohoto připojení čárky webu přijímat k IP adrese z fondu adresu klienta VPN.
- **Předplatné:** Pokud máte víc předplatných, ověřte, že používáte správná.
- **Pole Skupina zdroje: TestRG**
- **Umístění: Východní USA**
- **DNS Server: IP adresa** serveru DNS, který chcete použít pro překlad.
- **Název GW: Vnet1GW**
- **Veřejné IP název: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Před zahájením

- Zkontrolujte, jestli máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
- Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell. Když pracujete s Powershellu pro tuto konfiguraci, ujistěte se, že používáte jako správce. 

## <a name="declare"></a>Část 1 - protokolu a nastavení proměnné

V této části Přihlaste se a deklarovat hodnoty použité pro tuto konfiguraci. V ukázkové skripty jsou použity deklarované hodnoty. Změňte hodnoty tak, aby odrážely vlastní prostředí. Nebo můžete použít deklarované hodnoty a projděte si kroky jako cvičení.

1. V konzole prostředí PowerShell přihlaste ke svému účtu Azure. Tato rutina výzvu pro přihlašovací údaje pro váš účet Azure. Po přihlášení se stáhne nastavení účtu tak, aby byly k dispozici na Azure Powershellu.

        Login-AzureRmAccount 

2. Dostaňte seznam předplatné Azure.

        Get-AzureRmSubscription

3. Určení předplatné, ke které chcete použít. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklarujte proměnné, které chcete použít. Použijte v následujícím příkladu zaokrouhlením hodnoty pro vlastní potřeby.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Část 2 – konfigurace VNet 

1. Vytvoření skupiny prostředků.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Vytvořte konfigurace podsítě pro virtuální sítě *FrontEnd*, *back-end*a *GatewaySubnet*názvů. Tyto předpony musí být součástí VNet adresní prostor, který jste deklarované.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Vytvořte virtuální sítě. Server DNS určený by měl být server DNS, který můžete jmen pro zdroje, ke kterému se připojujete. V tomto příkladu jsme použili veřejnou IP adresu. Ujistěte se, pokud chcete použít vlastní hodnoty.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Zadejte proměnné virtuální sítě, který jste vytvořili.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Požádat o dynamicky přiřazené veřejnou IP adresu. Tuto IP adresu je nutné bránu fungovat správně. Pak připojíte bránu IP konfiguraci brány.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Část 3 – certifikátů

Certifikáty jsou používány Azure k ověřování klientů virtuální privátní sítě VPN čárky webu. Jak Base 64 zakódovaný soubor .cer X.509 z kořenového certifikátu generovaného při řešení certifikát enterprise nebo certifikát podepsaný svým držitelem kořenového exportu dat certifikát veřejného (ne privátním klíčem). Potom importujete data veřejné certifikátu z kořenového certifikátu do Azure. Navíc budete muset generovat certifikát klient z kořenového certifikátu pro klienty. Každého klienta, který potřebují připojit k virtuální sítě pomocí připojení P2S musíte mít nainstalovaný certifikát klienta, který vygenerované z kořenového certifikátu.

### <a name="cer"></a>1. soubor .cer kořenového certifikátu získejte

Je třeba přístup k datům veřejné certifikát pro kořenového certifikátu, kterou chcete použít.

- Pokud používáte systém certifikátů enterprise, získejte soubor .cer kořenového certifikátu, kterou chcete použít. 

- Pokud nepoužíváte řešení certifikát enterprise, budete muset generovat certifikát podepsaný svým držitelem kořenového. Postup v systému Windows 10 můžete odkázat při [práci se vlastních kořenových certifikátů pro konfigurace čárky webu](vpn-gateway-certificates-point-to-site.md).


1. Soubor .cer z certifikátu zobrazíte otevřete **text certmgr.msc** a vyhledejte kořenového certifikátu. Klikněte pravým tlačítkem certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem**.

2. V průvodci klikněte na **Další**, vyberte **Ne, nebudou exportovány privátním klíčem**a klikněte na tlačítko **Další**.

3. Na stránce **Formát souboru pro Export** vyberte **základu-64 formát X.509 (. CER).** Potom klikněte na **Další**. 

4. **Soubor k exportu**, **přejděte** do umístění, do kterého chcete exportovat certifikát. **Název souboru**zadejte název souboru certifikát. Klepněte na tlačítko **Další**.

5. Kliknutím na tlačítko **Dokončit** exportujte certifikát.

### <a name="generate"></a>2. generovat certifikát klienta

Pak generovat certifikáty klienta. Můžete buď generovat jedinečné certifikát pro každého klienta, který propojí nebo na více klientů můžete použít stejný certifikát. Výhodou generování jedinečné certifikátů je možnost zrušit certifikát jednu v případě potřeby. Jinak Pokud všichni používají stejný certifikát klienta a zjistíte, že potřebujete odvolat certifikát pro jednoho klienta, musíte se generovat a instalovat nové certifikáty pro všechny klienty, kteří používají certifikát ověření. V každém klientském počítači dál v tomto cvičení jsou nainstalované certifikáty klienta.

- Pokud používáte řešení certifikát enterprise, generovat certifikát klienta s běžné formát hodnoty název 'name@yourdomain.com', spíše než formátování NetBIOS "Doména\uživatelské jméno". 

- Pokud používáte certifikátu podepsaného svým držitelem, najdete v článku [práce s vlastních kořenových certifikátů pro bod webu konfigurace](vpn-gateway-certificates-point-to-site.md) generovat certifikát klienta.

### <a name="exportclientcert"></a>3. exportujte certifikát klienta

Certifikát klienta požaduje ověření. Po generování certifikát klienta, exportujte. Certifikát klienta, který exportujete nainstaluje později v každém klientském počítači.

1. Chcete-li exportovat certifikát klienta, můžete použít *příkaz certmgr.msc*. Klikněte pravým tlačítkem myši klienta certifikát, který chcete exportovat, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**.
2. Exportujte certifikát klienta soukromým klíčem. Toto je soubor *.pfx* . Zkontrolujte, že záznam nebo heslo (klíč), kterou jste nastavili pro tento certifikát.

### <a name="upload"></a>4. nahrát soubor .cer kořenového certifikátu

Deklarujte proměnná název vašeho certifikátu nahraďte hodnotu vlastní:

        $P2SRootCertName = "Mycertificatename.cer"

Přidání dat veřejné certifikát pro kořenového certifikátu do Azure. Můžete nahrávat soubory až 20 kořenových certifikátů. Není soukromý klíč pro kořenového certifikátu do nahrajete Azure. Po nahrání soubor .cer Azure pomocí ho ověřování klientů, kteří se připojují k virtuální sítě. 

Nahraďte cesta k souboru vlastní a potom spusťte rutiny.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Část 4 – vytvoření brány sítě VPN

Konfigurace a vytvořte bránu virtuálních sítí pro vaše VNet. *-GatewayType* musí být **Vpn** a *-VpnType* musí být **RouteBased**. To může trvat až 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Část 5 – Stáhnout balíček konfigurace klienta VPN

Připojení klientů k Azure pomocí P2S musí mít certifikát klienta a balíček konfigurace klienta VPN nainstalovaný. Balíčky konfigurace klienta VPN jsou k dispozici pro klienty Windows. Balíček klienta VPN obsahuje informací ke konfiguraci sítě VPN klientský software, která je integrovaná v systému Windows specifické pro VPN, který chcete připojit k. Balíček nenainstaluje další software. V tématu [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) Další informace.

1. Po vytvoření brány můžete stáhnout balíček konfigurace klienta. V tomto příkladu stáhne balíček pro klienty 64bitová verze. Pokud chcete stáhnout 32bitového klienta, nahraďte 'Amd64' "x86". Můžete taky stáhnete klienta VPN pomocí portálu Azure.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Rutiny prostředí PowerShell vrátí odkaz na adresu URL. Toto je příklad jaké vrácené adresa URL vypadá jako:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Zkopírujte a vložte odkaz, který se vrátí do webového prohlížeče ke stažení balíčku. Nainstalujte balíček v klientském počítači. Pokud se zobrazí místní SmartScreen, klikněte na **Další informace**, abyste mohli nainstalovat balíček **přesto spustit** .

4. V klientském počítači přejděte na **Nastavení sítě** a klepněte na položku **VPN**. Zobrazí se připojení uvedené. Zobrazí název virtuální sítě, která se připojí k a vypadá podobně jako tento příklad: 

    ![Klienta VPN] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "Klienta VPN")


## <a name="clientcertificate"></a>Část 6 – nainstalujte certifikát klienta

Každý klientský počítač, musíte mít certifikát klienta pro ověření. Při instalaci certifikátu klienta, budete potřebovat heslo, které byly nastavené při exportu certifikát klienta.

1. Zkopírujte soubor .pfx klientského počítače.
2. Poklikejte na soubor .pfx případně ji nainstalujte. Neměňte umístění instalace.

## <a name="connect"></a>Část 7 – připojení k Azure

1. Připojení k vaší VNet v klientském počítači přejděte na připojení VPN a najděte připojení VPN, které jste vytvořili. Je název stejný název jako virtuální sítě. Klikněte na **Připojit**. Zobrazí se zpráva se může zobrazit, které odkazuje na pomocí certifikátu. V takovém případě klikněte na **pokračovat** a použít zvýšenými oprávněními. 

2. Na stránce Stav **připojení** klikněte na **Připojit** k zahájení připojení. Pokud se zobrazí na obrazovce **Vyberte certifikát** , zkontrolujte, že klientský certifikát zobrazující té, která chcete použít pro připojení. Pokud není, použít na šipku rozevíracího seznamu vyberte správný certifikát a klikněte na **OK**.

    ![Připojení klienta VPN] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Připojení klienta VPN")

3. Připojení k teď možné stanovit.

    ![Připojení] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Připojení")

## <a name="verify"></a>Část 8 – zkontrolujte připojení

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

## <a name="addremovecert"></a>Přidání nebo odebrání důvěryhodného kořenového certifikátu

Certifikáty slouží k ověřování klientů virtuální privátní sítě VPN čárky webu. Následující kroky vám pomohou při přidávání a odebírání kořenových certifikátů. Když přidáte soubor kódováním Base 64 X.509 (.cer) Azure, vás vyzve Azure s informacemi o důvěryhodnosti kořenového certifikátu představující požadovaný soubor. 

Můžete přidat nebo odebrat důvěryhodné kořenové certifikáty, pomocí prostředí PowerShell nebo na portálu Azure. Pokud chcete udělat pomocí portálu Azure, přejděte na vaše **virtuální sítě brány > Nastavení > čárky webu konfigurace > Root certificates**. Podle těchto kroků vás provede jednotlivými těchto úkolů pomocí prostředí PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Přidání důvěryhodného kořenového certifikátu

Můžete přidat až 20 soubory .cer kořenových certifikátů na Azure. Postupujte podle pokynů přidejte kořenového certifikátu.

1. Vytvoření a připravit nového kořenového certifikátu, který přidáte do Azure. Export veřejným klíčem jako Base 64 formát X.509 (. CER) a potom ho otevřete v textovém editoru. Zkopírujte pouze části ukázáno v následujícím příkladu. 
 
    Zkopírujte hodnoty, jak je vidět v následujícím příkladu:

    ![certifikát] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certifikát")
    
2. Zadejte název certifikátu a klíčové informace jako proměnná. Nahraďte informace vlastního, jak je znázorněno následujícím příkladu:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Přidání nového kořenového certifikátu. Vždy možné přidávat jen jeden certifikát.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Můžete ověřit nový certifikát byla přidaná správně pomocí následující rutiny.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Odebrání důvěryhodného kořenového certifikátu

Odebrání důvěryhodného kořenového certifikátu z Azure. Při odebrání důvěryhodnou certifikační klientské certifikáty, které byly vytvořeny z kořenového certifikátu už bude moct připojit k Azure pomocí čárky webu. Pokud chcete připojení klientů, budou muset nainstalovat certifikát nového klienta, vygenerovaný z certifikát, který je v Azure důvěryhodné.

1. Odebrání důvěryhodného kořenového certifikátu, upravte v následujícím příkladu:

    Přidejte proměnné.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Odebrání certifikátu.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Použijte následující rutinu pro ověření, že certifikát byl úspěšně odebrán. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Spravovat seznam odvolaných certifikátů

Můžete odvolání certifikátů. Seznam odvolaných certifikátů umožňuje selektivně Odepřít připojení čárky webu založené na jednotlivé klientské certifikáty. Pokud odeberete .cer certifikát kořenové z Azure, odebere přístupu pro všechny certifikátů generovaných/přihlášení pomocí odvolaných kořenových certifikátů. Pokud chcete odvolat certifikát konkrétní klienta, ne kořenové, můžete udělat tak. Tímto způsobem, který bude tato aplikace certifikáty, které byly vytvořeny z kořenového certifikátu platit.

Běžné praktického cvičení je umožňuje spravovat přístup na úrovni týmu nebo organizace, při používání odvolaných certifikátů pro řízení přístupu jemně odstupňovaná k jednotlivým uživatelům kořenového certifikátu.

### <a name="revoke-a-client-certificate"></a>Odvolání certifikát klienta

1. Pokud potřebujete Miniatura klienta certifikát, který chcete odebrat.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Miniatura přidáte do seznamu odvolaný miniatura.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Ověřte, že miniaturou jsme přidali seznam odvolaných certifikátů. Je třeba přidat jeden Miniatura najednou.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Obnovení certifikát klienta

Obnovení certifikát klienta odebráním miniaturou seznam odvolaných certifikátů.

1.  Miniatura odeberte ze seznamu miniaturu certifikátu odvolaný klienta.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Zaškrtněte, pokud je Miniatura odebere ze seznamu odvolaný.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Další kroky

Přidání virtuálního počítače k síti virtuální. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .