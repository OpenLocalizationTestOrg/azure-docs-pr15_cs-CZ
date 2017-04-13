<properties 
   pageTitle="Vytvořit podepsaný certifikáty virtuální sítě čárky webu křížové místní připojení pomocí makecert | Microsoft Azure"
   description="Tento článek obsahuje kroky pro použití makecert vytvořit podepsaný certifikátů na Windows 10."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Práce s automatickým podpisem certifikáty pro připojení k bodu na serveru

Tento článek pomáhá vytvoření certifikátu podepsaného svým držitelem pomocí **makecert**a z něj generovat certifikátů. Kroky jsou určena makecert na Windows 10. Vytvoření certifikátů, které jsou kompatibilní s připojením k P2S ověřena MakeCert. 

Pro P2S připojení, je použití řešení certifikát organizace, jak zajistit o vystavení klienta certifikáty s běžné formát hodnoty název upřednostňovaná metoda certifikátů 'name@yourdomain.com', místo formátu název_domény\uživatelské_jméno NetBIOS.

Pokud nemáte řešení pro organizace, je nutné P2S klientům umožnit připojení k síti virtuální certifikátu podepsaného svým držitelem. Jsme vědět, že byly změněny makecert, ale je pořád platná metoda pro vytvoření podepsaného certifikáty, které jsou kompatibilní s připojením k P2S. Pracujeme na jiném řešení pro vytvoření podepsaného certifikáty, ale v tuto chvíli makecert je upřednostňovaná metoda.

## <a name="create-a-self-signed-certificate"></a>Vytvoření certifikátu podepsaného svým držitelem

MakeCert je jedním ze způsobů vytvoření certifikátu podepsaného svým držitelem. Podle těchto kroků vás provede jednotlivými vytvoření certifikátu podepsaného svým držitelem pomocí makecert. Tento postup nejsou specifické nasazení modelu. Platí pro správce zdrojů a klasické.

### <a name="to-create-a-self-signed-certificate"></a>Vytvoření certifikátu podepsaného svým držitelem

1. Z počítače se systémem Windows 10 stáhněte a nainstalujte [Windows Software Development Kit (SDK) pro Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Po instalaci můžete najít nástroje makecert.exe v části tyto materiály: C:\Program Files (x86) \Windows Kits\10\bin\<arch >. 
        
    Příklad:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Dále vytvořte a nainstalujte certifikát v úložišti osobních certifikátů na vašem počítači. Následující příklad vytvoří odpovídající soubor *.cer* , která jste nahráli do Azure při konfiguraci P2S. Následující příkaz spusťte jako správce. Nahraďte název, který chcete použít pro certifikát *ARMP2SRootCert* a *ARMP2SRootCert.cer* .<br><br>Certifikát bude nacházet v certifikáty – aktuální User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Chcete-li získat veřejný klíč

Jako součást konfiguraci brány VPN pro připojení k čárky serveru odeslán se veřejný klíč pro kořenového certifikátu Azure.

1. Soubor .cer z certifikátu zobrazíte otevřete **text certmgr.msc**. Klikněte pravým tlačítkem certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem**.

2. V průvodci klikněte na **Další**, vyberte **Ne, nebudou exportovány privátním klíčem**a klikněte na tlačítko **Další**.

3. Na stránce **Formát souboru pro Export** vyberte **základu-64 formát X.509 (. CER).** Potom klikněte na **Další**. 

4. **Soubor k exportu**, **přejděte** do umístění, do kterého chcete exportovat certifikát. **Název souboru**zadejte název souboru certifikát. Klepněte na tlačítko **Další**.

5. Kliknutím na tlačítko **Dokončit** exportujte certifikát.

 
### <a name="export-the-self-signed-certificate-optional"></a>Exportujte certifikát podepsaný svým držitelem (volitelné)

Můžete chtít exportujte certifikát podepsaný svým držitelem a bezpečně uložit. Pokud je třeba, můžete později nainstalovat na jiný počítač a generovat další klientské certifikáty nebo jiný soubor .cer exportovat. Zákazníkům s nainstalován certifikát klienta a že je taky nakonfigurovaná s velkými klienta VPN, které můžete nastavení připojení k síti virtuální prostřednictvím P2S. Z tohoto důvodu chcete, abyste měli jistotu, že certifikátů generovaných a nainstalovaný pouze v případě potřeby a bezpečné uložení certifikátu podepsaného svým držitelem.

Export certifikátu podepsaného svým držitelem jako do .pfx, vyberte kořenového certifikátu a používat stejným způsobem popsaným ve [exportovat certifikát klienta](#clientkey) export.

## <a name="create-and-install-client-certificates"></a>Vytvoření a nainstalovat certifikáty klienta

Certifikát podepsaný svým držitelem neinstalovat přímo v klientském počítači. Potřebujete negeneruje klientského certifikátu podepsaného svým držitelem. Můžete pak exportovat a nainstalujte certifikát klienta do klientského počítače. Podle těchto kroků nejsou specifické nasazení modelu. Platí pro správce zdrojů a klasické.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Část 1 - certifikát klienta negeneruje certifikátu podepsaného svým držitelem

Podle těchto kroků vás provede jednotlivými jedním ze způsobů negeneruje certifikátu podepsaného svým držitelem certifikát klienta. Více klientských certifikátů může způsobit z stejný certifikát. Každý klientský certifikát lze pak exportovat a na klientském počítači nainstalovaný. 

1. Na stejném počítači, který jste použili k vytvoření certifikátu podepsaného svým držitelem otevřete příkazový jako správce.

2. V tomto příkladu "ARMP2SRootCert" odkazuje na certifikát podepsaný svým držitelem, kterou jste vygenerovali. 
    - Změnit název podepsaného kořenové složky, které jsou generování klientského certifikátu z *"ARMP2SRootCert"* . 
    - Změňte název chcete vytvořit certifikát klienta je *ClientCertificateName* . 


    Úprava a spusťte vzorku generovat certifikát klienta. Pokud spustíte v následujícím příkladu bez provádění úprav, výsledek bude klienta certifikát s názvem ClientCertificateName v úložišti osobních certifikátů vygenerované z kořenového certifikátu ARMP2SRootCert.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Všechny certifikáty jsou uložené ve vaší "certifikáty – aktuální User\Personal\Certificates" uložit do počítače. Si můžete vygenerovat jako mnoho klientské certifikáty podle potřeby podle tohoto postupu.

### <a name="clientkey"></a>Část 2 – exportujte certifikát klienta

1. Chcete-li exportovat certifikát klienta, otevřete **text certmgr.msc**. Klikněte pravým tlačítkem myši klienta certifikát, který chcete exportovat, klikněte na **Všechny úkoly**a potom klikněte na **Exportovat**. Otevře se **Průvodce exportem**.

2. V průvodci klikněte na **Další**a pak vyberte **Ano, exportovat privátním klíčem**a klikněte na tlačítko **Další**.

3. Na stránce **Export formátu souboru** můžete ponechat výchozí vybrané. Klepněte na tlačítko **Další**. 
 
4. Na stránce **zabezpečení** musí chránit privátním klíčem. Pokud vyberete heslo nepoužít, zkontrolujte, že záznam nebo Zapamatovat heslo, které jste nastavili pro tento certifikát. Klepněte na tlačítko **Další**.

5. **Soubor k exportu**, **přejděte** do umístění, do kterého chcete exportovat certifikát. **Název souboru**zadejte název souboru certifikát. Klepněte na tlačítko **Další**.

6. Kliknutím na tlačítko **Dokončit** exportujte certifikát.  

### <a name="part-3---install-a-client-certificate"></a>Část 3 – nainstalovat certifikát klienta

Každého klienta, který chcete připojit k síti virtuální pomocí připojení čárky webu musí mít nainstalovaný certifikát klienta. Tento certifikát je kromě balíček požadovaná konfigurace VPN. Podle těchto kroků vás provede jednotlivými ruční instalace certifikátu klienta.

1. Najděte a zkopírujte soubor *.pfx* klientský počítač. V klientském počítači poklikejte na soubor *.pfx* nainstalovat. Nechte **Úložiště místo** **Aktuálního**uživatele a potom klikněte na tlačítko **Další**.

2. Nemáte na **souboru** importovat stránky, proveďte požadované změny. Klikněte na tlačítko **Další**.

3. Na stránce **ochranu soukromým klíčem** zadejte heslo pro potvrzení použili jednu, nebo ověřte správnost objekt zabezpečení, který je instalace certifikátu a nakonec klikněte na tlačítko **Další**.

4. Na stránce **Úložiště certifikátů** ponechejte výchozí umístění a klikněte na tlačítko **Další**.

5. Klikněte na **Dokončit**. V **Upozornění zabezpečení** pro instalaci certifikátu klikněte na **Ano**. Teď je bezpečně naimportované certifikát.

## <a name="next-steps"></a>Další kroky

Pokračujte v konfiguraci čárky webu. 

- **Správce prostředků** nasazení modelu postup najdete v tématech [Konfigurovat čárky webu připojení k VNet pomocí Powershellu](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klasický** nasazení modelu postup najdete v tématech [Konfigurovat bodu-server připojení VPN k VNet na klasické portálu](vpn-gateway-point-to-site-create.md).