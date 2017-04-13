<properties
   pageTitle="Konfigurace připojení brány čárky webu VPN k síti virtuální Azure pomocí portálu klasické | Microsoft Azure"
   description="Zabezpečené připojení k síti virtuální Azure vytvořením brány připojení VPN čárky webu."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfigurace připojení čárky webu k VNet na klasické portálu

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický - Azure portálu](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klasický – klasické portálu](vpn-gateway-point-to-site-create.md)

Konfigurace čárky webu (P2S) umožňuje vytvářet zabezpečené připojení z jednotlivých klientského počítače k síti virtuální. Připojení P2S je užitečná, když se chcete připojit k VNet ze vzdáleného místa, jako je třeba z domovské nebo konference, nebo když stačí jenom pár klienti, které je potřeba připojení k síti virtuální.

Tento článek vás provede vytvoříte VNet s připojení čárky webu v části **klasické nasazení modelu** pomocí **klasické portálu**.

Připojení k čárky serveru nevyžadují zařízení virtuální privátní sítě nebo veřejné IP adresu pro práci. Vytvoření připojení VPN spuštěním připojení z klientského počítače. Další informace o připojení k bodu na serveru podívejte se na [Nejčastější dotazy týkající se VPN brány](vpn-gateway-vpn-faq.md#point-to-site-connections) a [plánování a návrh](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely nasazení a metody pro P2S připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka obsahuje dva modely nasazení a k dispozici nasazení metody konfigurace P2S. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Základní pracovního postupu

![Bodu na--diagram webů] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "bod webu")
 
Podle těchto kroků vás provede jednotlivými kroky k vytvoření zabezpečené čárky webu připojení k síti virtuální. 

Konfigurace připojení čárky webu je rozdělené podle čtyř oddílů. Pořadí, ve kterém můžete konfigurovat následujícími částmi je důležité. Není kroků nebo můžete přejít.

- **Část 1** Vytvořte virtuální sítě a Brána VPN.
- **Část 2** Vytvoření certifikátů pro ověřování a jejich nahrání.
- **Část 3** Export a nainstalovat certifikáty klienta.
- **Část 4** Konfigurace klienta VPN.

## <a name="vnetvpn"></a>Část 1 - vytvořit virtuální sítě a Brána VPN

### <a name="part-1-create-a-virtual-network"></a>Část 1: Vytvoření virtuální sítě

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com/). Tento postup používat portál klasické není portálu Azure. V současné době nejde vytvořit P2S připojení s protokolem portálu Azure.

2. V levém dolním rohu obrazovky klikněte na **Nový**. V navigačním podokně klikněte na **Síť služby**a potom klikněte na **Virtuální sítě**. Klikněte na **Vytvořit vlastní** spustíte Průvodce konfigurací.

3. Na stránce **Podrobnosti virtuální sítě** zadejte tyto informace a potom klikněte na další šipku v pravém dolním.
    - **Název**: název virtuální sítě. Například "VNet1". To je název, který budete odkazujete při nasazení VMs tento VNet.
    - **Umístění**: umístění přímo souvisí s fyzické místo (oblast), kde se má zdroje (VMs) jsou uloženy. Pokud budete potřebovat VMs nasazené tento virtuální sítě fyzicky umístit východoasijských USA, například vyberte uvedeném místě. Není možné změnit oblasti přidružené k síti virtuální po jeho vytvoření.

4. Na stránce **servery DNS a připojením VPN** zadejte tyto informace a potom klikněte na další šipku v pravém dolním.
    - **Servery DNS**: Zadejte název serveru DNS a IP adresu nebo vyberte dříve registrované DNS server z místní nabídky. Toto nastavení serveru DNS nevytvoří. Umožňuje uživateli určit servery DNS, které chcete použít pro překlad pro tento virtuální sítě. Pokud chcete používat službu Azure výchozí název rozlišení, nezadáte v této části.
    - **Konfigurace čárky webu VPN**: Zaškrtněte políčko.

5. Na stránce **Připojení čárky webu** zadejte rozsah IP adres, ze kterého klienty VPN dostanou IP adresy když připojení. Existuje několik pravidla, které můžete zadat rozsahy adres. Je třeba ověřit, zda oblast, kterou zadáte nepřekrývá pomocí kterékoli z oblastí nachází ve vaší místní síti.

6. Zadejte následující informace a potom klikněte na Další šipky.
 - **Adresní prostor**: Zahrnout počáteční IP a CIDR (počet adresa).
 - **Přidat adresní prostor**: přidejte adresní prostor jenom v případě, že je nutný pro návrh sítě.

7. Na stránce **Virtuální adresu mezery sítě** zadejte adresu oblast, kterou chcete použít pro síť virtuální. Toto jsou dynamické IP adresy (POKLESU) přiřazené VMs a ostatních rolí instancí nasazené tento virtuální sítě.<br><br>Je důležité zejména vybrat oblast nepřekrývá pomocí kterékoli z oblastí, které se používají v místní síti. Musíte koordinaci s správce sítě, který může být nutné doložka mimo rozsah IP adres z místního adresní prostor sítě použít pro síť virtuální.

8. Zadejte následující informace a klikněte na zaškrtnutí začít vytvářet virtuální sítě.
 - **Adresní prostor**: Přidat interní rozsah IP adres, který chcete použít pro tento virtuální sítě, včetně spuštění IP a počtu. Je důležité vybrat oblast nepřekrývá pomocí kterékoli z oblastí, které se používají v místní síti. 
 - **Přidat podsítě**: dalších podsítí nejsou povinná, ale můžete chtít vytvořit samostatné podsítě pro VMs, které budou mít statické POKLES. Nebo můžete chtít mít vaše VMs v podsítě, která je oddělená od ostatních instancí role.
 - **Přidat brány podsítě**: podsítě brány je nutný pro funkce VPN čárky webu. Kliknutím vložíte podsítě brány. Podsítě brány se používá pouze pro brány virtuální sítě.

9. Po vytvoření virtuální sítě uvidíte, že **vytvořeno** zařazený do kategorie **Stav** na stránce sítí Azure klasické portálu. Po vytvoření virtuální sítě můžete vytvořit dynamické směrování brány.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Část 2: Vytvoření dynamické směrování brány

Typ brány musí být nakonfigurované jako dynamické. Statická směrování brány něm něco nefunguje s touto funkcí.

1. Na portálu na stránce **sítí** Azure klasické klikněte na virtuální sítě, který jste vytvořili a přejděte na stránku **řídicího panelu** .

2. Klikněte na položku **Vytvořit brány**, nachází v dolní části stránky **řídicího panelu** . Zobrazí se zpráva s dotazem **se dá k vytvoření brány pro virtuální sítě "VNet1"**. Kliknutím na tlačítko **Ano** začít vytvářet brány. Může trvat asi 15 minut bránu vytvořit.

## <a name="generate"></a>Část 2 – generovat a nahrajte certifikátů

Certifikáty slouží k ověřování klientů virtuální privátní sítě VPN čárky webu. Můžete použít kořenového certifikátu generovaného při řešení certifikát enterprise nebo pomocí certifikátu podepsaného svým držitelem. Nahrajte až 20 kořenových certifikátů na Azure. Po nahrání soubor .cer Azure umožňuje informace obsažené v něm ověření klienti, které máte nainstalován certifikát klienta. Z stejný certifikát, který představuje soubor .cer musíte vytvořit certifikát klienta.

V této části bude postupujte takto:

- Získejte soubor .cer kořenového certifikátu. Může to být buď certifikátu podepsaného svým držitelem nebo můžete použít systému enterprise certifikát.
- Nahrajte soubor .cer Azure.
- Vytvoření certifikátů.

### <a name="root"></a>Část 1: Získání soubor .cer kořenového certifikátu

Pokud používáte systém certifikátů enterprise, získejte soubor .cer kořenového certifikátu, kterou chcete použít. V [části 3](#createclientcert)generovat certifikáty klient z kořenového certifikátu.

Pokud nepoužíváte řešení certifikát pro organizace, musíte generovat certifikát podepsaný svým držitelem kořenového. Postup v systému Windows 10 můžete odkázat při [práci se vlastních kořenových certifikátů pro konfigurace čárky webu](vpn-gateway-certificates-point-to-site.md). Tento článek vás provede pomocí makecert generovat certifikátu podepsaného svým držitelem a potom soubor vyexportujte soubor .cer.

### <a name="upload"></a>Část 2: Nahrát soubor .cer kořenového certifikátu k portálu Azure klasické

Přidání důvěryhodného certifikátu do Azure. Když přidáte soubor kódováním Base 64 X.509 (.cer) Azure, vás vyzve Azure s informacemi o důvěryhodnosti kořenového certifikátu představující požadovaný soubor.

1. Azure klasické portálu na stránce **certifikáty** virtuální sítě klikněte na **Odeslat kořenového certifikátu**.

2. Na stránce **Odeslání certifikát** vyhledejte pro .cer kořenového certifikátu a klikněte na zaškrtnutí.

### <a name="createclientcert"></a>Část 3: Generovat certifikát klienta

Pak generovat certifikáty klienta. Můžete buď generovat jedinečné certifikát pro každého klienta, který propojí nebo na více klientů můžete použít stejný certifikát. Výhodou generování jedinečné certifikátů je možnost zrušit certifikát jednu v případě potřeby. Jinak Pokud všichni používají stejný certifikát klienta a zjistíte, že potřebujete odvolat certifikát pro jednoho klienta, musíte se generovat a instalovat nové certifikáty pro všechny klienty, kteří používají certifikát ověření.

- Pokud používáte řešení certifikát enterprise, generovat certifikát klienta s běžné formát hodnoty název 'name@yourdomain.com', spíše než formátování NetBIOS "Doména\uživatelské jméno". 

- Pokud používáte certifikátu podepsaného svým držitelem, najdete v článku [práce s vlastních kořenových certifikátů pro bod webu konfigurace](vpn-gateway-certificates-point-to-site.md) generovat certifikát klienta.

## <a name="installclientcert"></a>Část 3 – Export a nainstalujte certifikát klienta

Nainstalujte certifikát klienta na každém počítači, který chcete připojit k virtuální sítě. Certifikát klienta požaduje ověření. Můžete automatizovat instalace certifikátu klienta nebo můžete nainstalovat ručně. Následující kroky vám pomohou při exportu a ruční instalace certifikátu klienta.

1. Chcete-li exportovat certifikát klienta, můžete použít *příkaz certmgr.msc*. Klikněte pravým tlačítkem myši klienta certifikát, který chcete exportovat, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**.
2. Exportujte certifikát klienta soukromým klíčem. Toto je soubor *.pfx* . Zkontrolujte, že záznam nebo heslo (klíč), kterou jste nastavili pro tento certifikát.
3. Zkopírujte soubor *.pfx* klientského počítače. V klientském počítači poklikejte na soubor *.pfx* případně ji nainstalujte. Zadejte heslo při požadavku. Neměňte umístění instalace.

## <a name="vpnclientconfig"></a>Část 4 – konfigurace klienta VPN

Připojení k virtuální sítě, bude potřeba nakonfigurovat klienta VPN. Klientský počítač vyžaduje certifikát klienta a VELKÁ2 konfigurace klienta VPN než budete moct připojit. Konfigurace klienta VPN, proveďte následující kroky v pořadí.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Část 1: Vytvoření balíčku konfigurace klienta VPN

1. Azure klasické portálu na stránku **řídicího panelu** virtuální sítě přejděte na první pohled dával nabídku v pravém rohu. Seznam operačních systémů klienta, které podporují najdete v tématu připojení [bodu-webu](vpn-gateway-vpn-faq.md#point-to-site-connections) část Nejčastější dotazy týkající se VPN brány. Balíček klienta VPN obsahuje informace o konfiguraci ke konfiguraci sítě VPN klientský software integrovaná v systému Windows. Balíček nenainstaluje další software. Nastavení jsou specifické pro virtuální sítě, ke které se chcete připojit.<br><br>Vyberte Stáhnout balíček, který odpovídá operačního systému klienta, na kterém budou nainstalovaná:
 - Pro 32bitové klienty vyberte **Stáhnout balíček pro 32bitovou verzi klienta VPN**.
 - 64bitová verze klientů vyberte **Stáhnout balíček pro 64bitovou verzi klienta VPN**.

2. Trvá několik minut vytvořit balíček klienta. Po dokončení balíček, můžete si stáhnout soubor. Stahovaného souboru *.exe* mohou být bezpečně uložena ve vašem počítači.

3. Po generovat a stáhnout balíček klienta VPN z portálu Microsoft Azure klasické, můžete nainstalovat balíček klienta klientského počítače, ze kterého se chcete připojit k síti virtuální. Pokud nebudete chtít balíček klienta VPN nainstalovat do více počítačů klienta, ujistěte se, že každé mají také nainstalován certifikát klienta.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Část 2: Nainstalovat VPN konfigurace na straně klienta

1. Zkopírujte soubor konfigurace místně do počítače, na kterém chcete připojení k síti virtuální a poklikejte na souboru .exe. 

2. Po nainstaloval balíček, můžete začít připojení VPN. Konfigurace balíček není podepsán společností Microsoft. Může chcete podepsat balíček pomocí služby podpisový vaší organizace nebo přihlásit sami pomocí [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Je v pořádku použil funkci balíček bez přihlášení. Ale pokud balíček není podepsán, upozornění se zobrazí po instalaci balíček.

3. V klientském počítači přejděte na **Nastavení sítě** a klepněte na položku **VPN**. Zobrazí se připojení uvedené. Zobrazí název virtuální sítě, že se připojí k a bude vypadat nějak takto: 

    ![Klienta VPN] (./media/vpn-gateway-point-to-site-create/vpn.png "Klienta VPN")


### <a name="part-3-connect-to-azure"></a>Část 3: Připojení k Azure

1. Připojení k vaší VNet v klientském počítači přejděte na připojení VPN a najděte připojení VPN, které jste vytvořili. Je název stejný název jako virtuální sítě. Klikněte na **Připojit**. Zobrazí se zpráva se může zobrazit, které odkazuje na pomocí certifikátu. V takovém případě klikněte na **pokračovat** a použít zvýšenými oprávněními. 

2. Na stránce Stav **připojení** klikněte na **Připojit** k zahájení připojení. Pokud se zobrazí na obrazovce **Vyberte certifikát** , zkontrolujte, že klientský certifikát zobrazující té, která chcete použít pro připojení. Pokud není, použít na šipku rozevíracího seznamu vyberte správný certifikát a klikněte na **OK**.

    ![Klient 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Připojení klienta VPN")

3. Připojení k teď možné stanovit.

    ![Klienta VPN 3] (./media/vpn-gateway-point-to-site-create/connected.png "Připojení klienta VPN 2")

### <a name="part-4-verify-the-vpn-connection"></a>Část 4: Ověření připojení VPN

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

Pokud budete potřebovat další informace o virtuálních sítí, najdete na stránce [Virtuální dokumentaci sítě](https://azure.microsoft.com/documentation/services/virtual-network/) .
