<properties 
    pageTitle="Konfigurace zabezpečení rozdělit sloučení | Microsoft Azure" 
    description="Nastavení x409 certifikáty pro šifrování" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Konfigurace zabezpečení rozdělit hromadné korespondence  

Používat službu rozdělit/sloučení, je třeba správně nakonfigurovat zabezpečení. Služba není součástí funkci pružná měřítko databázi Microsoft Azure SQL. Další informace najdete v tématu [pružná rozdělit měřítko a sloučit kurz služby](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurace certifikátů

Certifikáty jsou nakonfigurované dvěma způsoby. 

1. [Konfigurace certifikátu SSL](#To-Configure-the-SSL#Certificate)
2. [Konfigurace certifikátů](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Získání certifikátů

Z veřejné certifikační autority (CA) nebo z [Služba Windows certifikát](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx)můžete získat certifikáty. Toto jsou upřednostňované způsoby získat certifikáty.

Pokud nejsou k dispozici tyto možnosti, můžete vytvořit **podepsaný certifikáty**.
 
## <a name="tools-to-generate-certificates"></a>Nástroje pro vytvoření certifikátů

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [Pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Spuštění nástroje

* Z vývojář příkazovém řádku pro aplikaci Visual Studia, najdete v článku [Příkazový řádek Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) 

    Pokud máte nainstalované, přejdete takto:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Získání WDK z [Windows 8.1: stažení sady a nástroje](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Konfigurace certifikátu SSL
Certifikát SSL je potřeba k šifrování komunikace a ověřování serveru. Zvolte většina příslušné následující tři scénáře a provedení všech kroků:

### <a name="create-a-new-self-signed-certificate"></a>Vytvoření nového podepsaného svým držitelem

1.    [Vytvoření certifikátu podepsaného svým držitelem](#Create-a-Self-Signed-Certificate)
2.    [Vytvoření souboru PFX pro SSL certifikát podepsaný svým držitelem](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Nahrání certifikátu SSL do cloudu služby](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Aktualizace certifikátu SSL v souboru konfigurace služby](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Import SSL certifikační autorita](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Použití stávající certifikát z úložiště certifikátů
1. [Exportujte certifikát SSL z úložiště certifikátů](#Export-SSL-Certificate-From-Certificate-Store)
2. [Nahrání certifikátu SSL do cloudu služby](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Aktualizace certifikátu SSL v souboru konfigurace služby](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Použití stávající certifikát v souboru PFX

1. [Nahrání certifikátu SSL do cloudu služby](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Aktualizace certifikátu SSL v souboru konfigurace služby](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Konfigurace certifikátů
Klientské certifikáty jsou nutné k ověření žádosti o služby. Zvolte většina příslušné následující tři scénáře a provedení všech kroků:

### <a name="turn-off-client-certificates"></a>Vypnutí certifikátů
1.    [Vypnutí ověřování na základě certifikát klienta](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Nové certifikáty podepsaného klienta
1.    [Vytvoření podepsaného certifikační autorita](#Create-a-Self-Signed-Certification-Authority)
2.    [Nahrání certifikační do cloudu služby](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Aktualizace certifikační v souboru konfigurace služby](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Certifikáty klienta](#Issue-Client-Certificates)
5.    [Vytvoření soubory PFX pro certifikátů](#Create-PFX-files-for-Client-Certificates)
6.    [Import certifikátu klienta](#Import-Client-Certificate)
7.    [Kopírování Thumbprints certifikát klienta](#Copy-Client-Certificate-Thumbprints)
8.    [Konfigurace povolené klientů v souboru konfigurace služby](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Použití existující klientských certifikátů
1.    [Najděte veřejný klíč](#Find-CA-Public Key)
2.    [Nahrání certifikační do cloudu služby](#Upload-CA-certificate-to-cloud-service)
3.    [Aktualizace certifikační v souboru konfigurace služby](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kopírování Thumbprints certifikát klienta](#Copy-Client-Certificate-Thumbprints)
5.    [Konfigurace povolené klientů v souboru konfigurace služby](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Konfigurace klienta Kontrola odvolání certifikátů](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Povolené IP adresy

Přístup k koncové body služby může být omezený na konkrétní rozsahy IP adres.

## <a name="to-configure-encryption-for-the-store"></a>Konfigurace šifrování úložiště

Certifikát je potřeba k šifrování přihlašovacích údajů, které jsou uložené v úložišti metadat. Zvolte většina příslušné následující tři scénáře a provedení všech kroků:

### <a name="use-a-new-self-signed-certificate"></a>Použijte nový certifikát podepsaný svým držitelem

1.     [Vytvoření certifikátu podepsaného svým držitelem](#Create-a-Self-Signed-Certificate)
2.     [Vytvoření souboru PFX pro šifrování certifikát podepsaný svým držitelem](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Nahrání šifrovacím certifikátu za účelem cloudových služeb](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Aktualizovat šifrovací certifikát v souboru konfigurace služby](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Použití stávající certifikát z úložiště certifikátů

1.     [Exportujte certifikát šifrování z úložiště certifikátů](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Nahrání šifrovacím certifikátu za účelem cloudových služeb](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Aktualizovat šifrovací certifikát v souboru konfigurace služby](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Použití stávající certifikát v souboru PFX

1.     [Nahrání šifrovacím certifikátu za účelem cloudových služeb](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Aktualizovat šifrovací certifikát v souboru konfigurace služby](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Výchozí konfigurace

Výchozí konfigurace zakáže veškerý přístup k koncový bod HTTP. Toto je doporučené nastavení, protože požadavky na tyto koncové body může provádět citlivé informace jako přihlašovací údaje pro databázi.
Výchozí konfigurace vám umožní všechny HTTPS koncový bod. Toto nastavení může být omezený dál.

### <a name="changing-the-configuration"></a>Změna konfigurace

Skupiny pravidla pro řízení přístupu, které se vztahují k a koncový bod nejsou nakonfigurováni v **<EndpointAcls>** oddíl v **souboru konfigurace služby**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Pravidla ve skupině ovládací prvek pro access nejsou nakonfigurováni v <AccessControl name=""> část konfiguračního souboru služby. 

Formát jsou vysvětleny v síti ACL si přečtěte následující dokumentaci.
Například umožňuje pouze IP adresy v oblasti 100.100.0.0 k 100.100.255.255 pro přístup k koncový bod HTTPS pravidel by vypadal takhle:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Odmítnutí ochrana služby

Existují dva různé mechanismy podporované ke zjištění a útokům odmítnutí služby:

*    Omezit počet souběžně prováděné žádosti na jeden vzdáleného hostitele (ve výchozím nastavení vypnuto)
*    Omezit sazba přístupu za vzdáleného hostitele (na ve výchozím nastavení)

Jsou založeny na funkce popsána v dynamické IP zabezpečení ve službě IIS. Při změně této konfiguraci mějte na paměti následující skutečnosti:

* Chování proxy servery a překládání adres zařízení přes informace vzdáleným hostitelem
* Každý požadavek všechny zdroje v roli web je považován za (například načítání skripty, obrázky a další)

## <a name="restricting-number-of-concurrent-accesses"></a>Omezení počtu souběžné přístup

Nastavení, které slouží ke konfiguraci toto chování jsou:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Změna DynamicIpRestrictionDenyByConcurrentRequests povolte ochranu true (pravda).

## <a name="restricting-rate-of-access"></a>Omezení sazba aplikace access

Nastavení, které slouží ke konfiguraci toto chování jsou:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Konfigurace odpověď na požadavek odepření

Toto nastavení nakonfiguruje odpověď na požadavek zakázané:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Naleznete v dokumentaci k dynamické IP zabezpečení ve službě IIS dalších podporovaných hodnot.

## <a name="operations-for-configuring-service-certificates"></a>Operace pro konfiguraci služby certifikátů
Toto téma je pouze pro referenci. Postupujte podle kroků konfigurace uvedených v:

* Konfigurace certifikátu SSL
* Konfigurace certifikátů

## <a name="create-a-self-signed-certificate"></a>Vytvoření certifikátu podepsaného svým držitelem
Spuštění:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Chcete-li přizpůsobit:

*    -n s adresy URL služby. Zástupné znaky ("CN = * .cloudapp .net") a jsou podporovány alternativní názvy ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").
*    e - s datum vypršení platnosti certifikátu vytvoření silného hesla a určit po zobrazení výzvy.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Vytvoření souboru PFX pro certifikátu podepsaného svým držitelem SSL

Spuštění:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Zadejte heslo a potom soubor vyexportujte certifikátu pomocí těchto možností:
* Ano, exportovat privátním klíčem
* Export všechny rozšířené vlastnosti

## <a name="export-ssl-certificate-from-certificate-store"></a>Exportujte certifikát SSL z úložiště certifikátů

* Vyhledání certifikátů
* Klikněte na akce -> všechny úkoly -> Exportovat.
* Exportujte certifikát do. Soubor PFX tyto možnosti:
    * Ano, exportovat privátním klíčem
    * Pokud je to možné zahrnout všechny certifikáty na cestě *: exportují se všechny rozšířené vlastnosti

## <a name="upload-ssl-certificate-to-cloud-service"></a>Nahrání certifikátu SSL do cloudové služby

Nahrání certifikátu s existující nebo generovat. Soubor PFX s dvojice klíčů SSL:

* Zadejte heslo ochrany osobních údajů klíče

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Aktualizace certifikátu SSL v souboru konfigurace služby

Aktualizace Miniatura přínosu následující nastavení v souboru konfigurace služby s miniaturou certifikát nahráli cloudové služby:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Import SSL certifikační autorita

Tyto kroky ve všech účtu/počítač, který bude komunikovat pomocí služby:

* Poklikejte. Soubor CER v Průzkumníku Windows
* V dialogovém okně Certifikát klikněte na nainstalovat certifikát...
* Import certifikátu do úložiště důvěryhodných kořenových certifikátů

## <a name="turn-off-client-certificate-based-authentication"></a>Vypnutí ověřování na základě certifikát klienta

Podporované jenom klient certifikát ověřování a jeho zakázání vám umožní veřejné přístupu pro koncové body služby, pokud jsou jiné mechanismy platná (například Microsoft Azure virtuální sítě).

Změna nastavení NEPRAVDA v souboru konfigurace služby vypnout funkci:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

V nastavení certifikátu CA, zkopírujte stejné Miniatura jako certifikát SSL:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Vytvoření podepsaného certifikační autorita
Provést následující postup vytvoření certifikátu podepsaného svým držitelem má fungovat jako certifikační autorita:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Chcete-li přizpůsobit ho

*    e - s data vypršení platnosti certifikátu


## <a name="find-ca-public-key"></a>Najděte veřejný klíč

Všechny certifikáty klienta musí byl vydán certifikační autoritou důvěryhodné službou. Najděte veřejný klíč pro certifikační autority, která vystavila klienta certifikáty, které budou sloužit k ověření k tomu ho nahrajte do cloudové služby.

Pokud soubor s veřejným klíčem není k dispozici, exportujte z úložiště certifikátů:

* Vyhledání certifikátů
    * Vyhledejte certifikát klienta vydán stejnou certifikační autoritou
* Poklepejte na certifikát.
* Vyberte kartu cestě v dialogovém okně certifikát.
* Poklikejte na položku certifikační Autority na cestě.
* Pořizování poznámek vlastnosti certifikátu.
* Zavřete dialogové okno **certifikát** .
* Vyhledání certifikátů
    * Hledání CA výše.
* Klikněte na akce -> všechny úkoly -> Exportovat.
* Exportujte certifikát do. CER tyto možnosti:
    * **Ne, nebudou exportovány privátním klíčem**
    * Pokud je to možné zahrňte všechny certifikáty na cestě.
    * Exportujte všechny rozšířené vlastnosti.

## <a name="upload-ca-certificate-to-cloud-service"></a>Nahrání certifikační cloudové služby

Nahrání certifikátu s existující nebo generovat. Soubor CER s veřejným klíčem certifikační Autority.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Aktualizace certifikační v souboru konfigurace služby

Aktualizace Miniatura přínosu následující nastavení v souboru konfigurace služby s miniaturou certifikát nahráli cloudové služby:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Aktualizace přínosu použité toto nastavení se stejným Miniatura:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Certifikáty klienta

Jednotlivých oprávnění pro přístup ke službě by měla být klienta osvědčení pro his/hers výhradním použít a zvolit, že his/hers vlastníkem silného hesla k ochraně privátním klíčem. 

Ve stejném počítači místo, kam bylo certifikátu podepsaného svým držitelem CA generovaných a uložené se provedou podle těchto kroků:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Přizpůsobení:

* n - s na kód pro klienta, který bude ověřena certifikát
* e - s datum vypršení platnosti certifikátu
* MyID.pvk a MyID.cer s jedinečné názvy souborů pro tento certifikát klienta

Tento příkaz vás vyzve k zadání hesla vytvořili a potom použít jednou. Použijte silné heslo.

## <a name="create-pfx-files-for-client-certificates"></a>Vytvoření certifikátů soubory PFX pro klienta

Pro každý vygenerovaných klientský certifikát provést:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Přizpůsobení:

    MyID.pvk and MyID.cer with the filename for the client certificate

Zadejte heslo a potom soubor vyexportujte certifikátu pomocí těchto možností:

* Ano, exportovat privátním klíčem
* Export všechny rozšířené vlastnosti
* Jednotlivé Komu tento certifikát vystaven měli zvolit heslo exportu

## <a name="import-client-certificate"></a>Import certifikátu klienta

Jednotlivé uživatele, pro kterého byla vydána certifikát klienta importujte pár klíčů v počítačích, které jeho pomocí komunikovat pomocí služby:

* Poklikejte. Soubor PFX v Průzkumníku Windows
* Import do osobní úložiště certifikátů s alespoň tuto možnost:
    * Zahrnout všechny rozšířené vlastnosti zaškrtnuté políčko

## <a name="copy-client-certificate-thumbprints"></a>Kopírování thumbprints certifikát klienta
Jednotlivé uživatele, pro kterého byla vydána certifikát klienta musíte tyto kroky za účelem získání Miniatura z his/hers certifikát, který bude přidán do souboru konfigurace služby:
* Spuštění certmgr.exe
* Vyberte kartu Osobní
* Poklikejte na klienta certifikát, který chcete použít pro ověřování
* V dialogovém okně certifikát, který se otevře vyberte kartu Podrobnosti
* Ujistěte se, že zobrazit zobrazuje všechny
* Vyberte pole s názvem Miniatura v seznamu
* Zkopírujte hodnotu Miniatura **Odstranit neviditelné znaků kódu Unicode před první číslice** odstranit všechny mezery

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Konfigurace klientů povolených v souboru konfigurace služby

Aktualizace hodnota následující nastavení v souboru konfigurace služby s hodnotami oddělenými čárkou seznam thumbprints certifikátů povolený přístup ke službě:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurace klienta Kontrola odvolání certifikátů

Ve výchozím nastavení nekontroluje s certifikační autority stav odvolání certifikátů klienta. Zapnout kontroly, pokud certifikační autority, které certifikáty klienta podporuje tyto kontroly, změňte následující nastavení s některou z hodnot podle výčet X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Vytvoření souboru PFX podepsaného šifrování certifikátů

Pro certifikát šifrování provést:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Přizpůsobení:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Zadejte heslo a potom soubor vyexportujte certifikátu pomocí těchto možností:
*    Ano, exportovat privátním klíčem
*    Export všechny rozšířené vlastnosti
*    Při uložení certifikátu do cloudové služby, budete potřebovat heslo.

## <a name="export-encryption-certificate-from-certificate-store"></a>Exportujte certifikát šifrování z úložiště certifikátů

*    Vyhledání certifikátů
*    Klikněte na akce -> všechny úkoly -> Exportovat.
*    Exportujte certifikát do. Soubor PFX tyto možnosti: 
  *    Ano, exportovat privátním klíčem
  *    Pokud je to možné zahrnout všechny certifikáty na cestě 
*    Export všechny rozšířené vlastnosti

## <a name="upload-encryption-certificate-to-cloud-service"></a>Nahrání šifrovacím certifikátu do cloudové služby

Nahrání certifikátu s existující nebo generovat. Soubor PFX s pár klíče šifrování:

* Zadejte heslo ochrany osobních údajů klíče

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Aktualizovat šifrovací certifikát v souboru konfigurace služby

Aktualizace Miniatura přínosu následující nastavení v souboru konfigurace služby s miniaturou certifikát nahráli cloudové služby:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Společná operace certifikátu

* Konfigurace certifikátu SSL
* Konfigurace certifikátů

## <a name="find-certificate"></a>Vyhledání certifikátů

Postupujte podle těchto kroků:

1. Spusťte mmc.exe.
2. Soubor -> Přidat nebo odebrat modulu Snap-in...
3. Vyberte položku **certifikáty**.
4. Klikněte na **Přidat**.
5. Zvolte umístění úložiště certifikátů.
6. Klikněte na **Dokončit**.
7. Klikněte na **OK**.
8. Rozbalte položku **certifikáty**.
9. Rozbalte uzel úložiště certifikátů.
10. Rozbalte podřízeného uzlu certifikát.
11. V seznamu vyberte certifikát.

## <a name="export-certificate"></a>Exportujte certifikát
Podle pokynů **Průvodce exportem certifikátu**:

1. Klikněte na tlačítko **Další**.
2. Vyberte **Ano**, pak **Exportovat privátním klíčem**.
3. Klikněte na tlačítko **Další**.
4. Vyberte požadované výstupní formát souboru.
5. Zaškrtněte požadované možnosti.
6. Zaškrtněte políčko **heslo**.
7. Zadejte silné heslo a potvrďte ho.
8. Klikněte na tlačítko **Další**.
9. Zadejte nebo vyberte název souboru místa uložení certifikátu (použít. Rozšíření PFX).
10. Klikněte na tlačítko **Další**.
11. Klikněte na **Dokončit**.
12. Klikněte na **OK**.

## <a name="import-certificate"></a>Import certifikátu

V Průvodci importem certifikát:

1. Vyberte umístění úložiště.

    * Výběr **Aktuálního uživatele** -li pouze procesů spuštěných v rámci aktuálního uživatele bude přístup ke službě
    * Vyberte **Místní počítač** , pokud ostatní procesy v tomto počítači bude přístup ke službě
2. Klikněte na tlačítko **Další**.
3. Pokud importovat ze souboru, potvrďte cesta k souboru.
4. Pokud importovat. Soubor PFX:
    1.     Zadejte heslo ochrana privátním klíčem
    2.     Vyberte možnosti importu
5.     Vyberte "Místo" certifikáty v úložišti následující
6.     Klikněte na **Procházet**.
7.     Vyberte požadované úložiště.
8.     Klikněte na **Dokončit**.
       
    * Pokud byla vybrána úložišti Důvěryhodné kořenové certifikační autority, klikněte na **Ano**.
9.     Klikněte na **OK** na všechna okna dialogového okna.

## <a name="upload-certificate"></a>Nahrání certifikátu

Na [portálu Azure](https://portal.azure.com/)

1. Vyberte **Cloudovým službám**.
2. Vyberte cloudovou službu.
3. V nabídce horním klepněte na položku **certifikáty**.
4. Na dolním panelu klikněte na **Odeslat**.
5. Vyberte soubor certifikátu.
6. Pokud je. PFX soubor, zadejte heslo pro privátním klíčem.
7. Po dokončení, zkopírujte miniaturu certifikátu z novou položku v seznamu.

## <a name="other-security-considerations"></a>Otázky bezpečnosti pro ostatní
 
Nastavení SSL popsaných v tomto dokumentu šifrovat komunikaci mezi službou a klientů při použití koncový bod HTTPS. Co je důležité od přihlašovací údaje pro přístup k databázi a případně i ostatní citlivé informace jsou obsaženy v komunikace. Uvědomte, že služba ukládá vnitřní stav, včetně přihlašovací údaje v jeho vnitřní tabulek v databázi Microsoft Azure SQL, která jste zadali pro úložiště metadat v předplatném Microsoft Azure. Databáze byla definována jako součást následující nastavení v souboru konfigurace služby (. Soubor CSCFG): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Jsou šifrovány přihlašovacím údajům uloženým v databázi. Ale osvědčený, zajistěte, aby web a pracovní role nasazení služeb uchovávat aktuální a bezpečné jako budou mít přístup k databázi metadat a certifikát používaný k šifrování a dešifrování uložené přihlašovací údaje. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
