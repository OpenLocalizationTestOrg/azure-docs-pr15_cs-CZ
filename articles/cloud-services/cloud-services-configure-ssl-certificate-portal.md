<properties 
    pageTitle="Nastavení SSL pro do cloudové služby | Microsoft Azure" 
    description="Zjistěte, jak můžete určit HTTPS koncový bod pro web roli a jak nahrát certifikát SSL k zabezpečení aplikace. Tyto příklady používají portálu Azure." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurace SSL pro aplikace v Azure

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-configure-ssl-certificate-portal.md)
- [Azure klasický portálu](cloud-services-configure-ssl-certificate.md)

Socket Layer šifrování (SSL Secure) je nejčastěji používaná způsob zabezpečení dat posílaných všude na Internetu. Běžné úkol popisuje určení HTTPS koncový bod pro web roli a jak nahrát certifikát SSL k zabezpečení aplikace.

> [AZURE.NOTE] Postupy v tomto úkolu lze použít ke Cloudovým službám Azure; Aplikace služby najdete v článku [Toto](../app-service-web/web-sites-configure-ssl-certificate.md).

Tento úkol používá provozní nasazení; informace o použití pracovní nasazení je k dispozici na konci tohoto tématu.

Přečtěte si [Tento](cloud-services-how-to-create-deploy-portal.md) nejdřív Pokud jste ještě vytvořili do cloudové služby.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Krok 1: Získání certifikát SSL

Abyste mohli nakonfigurovat SSL pro aplikaci, musíte nejdřív Získejte certifikát SSL, který byl podepsán tak, že certifikační autority (CA), důvěryhodných třetí strany, která problémy certifikátů k tomuto účelu. Pokud už nemáte jeden, budete muset pořídit jednu ze společnosti, který prodává certifikáty SSL.

Certifikát musí splňovat následující kritéria pro certifikáty SSL v Azure:

-   Certifikát musí obsahovat privátním klíčem.
-   Certifikát musí být vytvořena pro výměnu klíčů, exportovat do souboru osobních informací Exchange (.pfx).
-   Název subjektu certifikát musí odpovídat doménu použitou pro přístup ke cloudové službě. Nelze Získejte certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net. Vlastní název domény použít, když musíte získat přístup ke službě. Pokud požadujete certifikát od certifikační Autority na certifikát předmět jméno se musí shodovat název vlastní domény používaný pro přístup k aplikaci. Například, pokud váš vlastní název domény je **contoso.com** by požadujete certifikát od certifikační Autority pro * **. contoso.com** nebo * *www.contoso.com**.
-   Certifikát musí používat aspoň 2048bitový šifrování.

Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a používat certifikátu podepsaného svým držitelem. Certifikát podepsaný svým držitelem není ověřeno certifikační Autority a mohli používat v cloudapp.net jako adresa URL webu. Například následující úkolu používá certifikátu podepsaného svým držitelem, ve kterém je běžné název (CN) použitý v certifikát **sslexample.cloudapp.net**.

Další musí obsahovat informace o certifikátu do definice služby a služby konfigurace soubory.

<a name="modify"> </a>
## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Krok 2: Úprava souborů definice a konfigurační služby

Aplikace musí být nakonfigurované pro použít certifikát a musí být přidán koncový bod HTTPS. V důsledku toho definice služby a služby konfigurační soubory třeba aktualizovat.

1.  Ve vašem prostředí vývoj otevřete soubor definice služby (CSDEF), přidat oddíl **certifikáty** v části **WebRole** a obsahují následující informace o certifikátu (a pomocná certifikáty):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    V části **certifikáty** definuje název certifikát, umístění a název úložiště, kde je uložena.
    
    Oprávnění (`permisionLevel` atribut) může být nastavena na jednu z následujících akcí:

  	| Hodnota oprávnění  | Popis |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Výchozí)** Všechny procesy role zpřístupníte privátním klíčem. |
  	| zvýšená          | Pouze zvýšenými procesy zpřístupníte privátním klíčem.|

2.  V souboru definice služby přidáte **InputEndpoint** prvek v části **koncové body** povolit HTTPS:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  V souboru definice služby přidáte **vazbu** prvek v části **weby** . Tato akce přidá HTTPS vazby mapovat koncový bod k vašemu webu:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Všechny požadované změny souboru definice služby dokončili, ale pořád budete muset přidat informace o certifikátu do souboru konfigurace služby.

4.  Služba konfigurační soubor (CSCFG) ServiceConfiguration.Cloud.cscfg, přidáte oddíl **certifikáty** v části **Role** nahrazení Miniatura vyhovovat vidíte na obrázku s informacemi o certifikátu:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Výše uvedený příklad používá **sha1** pro algoritmus miniatury. Zadání odpovídající hodnoty pro vašeho certifikátu Miniatura algoritmus.)

Teď, po aktualizaci definice služby a služby konfigurační soubory balíček nasazení pro odeslání do Azure. Pokud používáte **cspack**, nepoužívejte příznak **/generateConfigurationFile** jako, který přepíše informace o certifikátu, které jste vložili.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Odeslání certifikát

Připojení k portálu a...

1. Vyberte cloudové služby na portálu, vyberte vaše **Cloudové služby**. (Což je v části **všechny zdroje** .) 
    
    ![Publikování cloudové služby](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klepněte na položku **certifikáty**.

    ![Klikněte na ikonu certifikátů](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Uveďte **soubor**, **heslo**a pak klikněte na **Odeslat**.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Krok 4: Připojení k instanci rolí pomocí HTTPS

Teď nasazení je zařídit i v Azure, můžete připojit k němu pomocí HTTPS.
    
1.  Klikněte na adresu **URL webu** otevřete webový prohlížeč.

    ![Klikněte na adresu URL webu](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2.  Ve webovém prohlížeči upravit odkaz na místo **http**použijte **https** a přejděte na stránku.

    >[AZURE.NOTE] Pokud používáte certifikátu podepsaného svým držitelem při procházení HTTPS koncový bod, který máte přidružený k certifikátu podepsaného svým držitelem chyba se může zobrazit certifikát v prohlížeči. Tento problém; pomocí certifikátu podepsáno důvěryhodnou certifikační autoritou odstraňuje. mezitím můžete ignorovat chyby do schránky. (Další možností je přidání certifikátu podepsaného svým držitelem do úložiště certifikátů důvěryhodnou certifikační autority uživatele.)

    ![Náhled stránky](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

    >[AZURE.TIP] Pokud chcete použít protokol SSL pro pracovní nasazení místo produkční nasazení, musíte nejdřív zjištění adresy URL používané pro pracovní nasazení. Jakmile byly nasazeny cloudové služby, adresa URL pro pracovní prostředí, je určený podle **ID nasazení** GUID v tomto formátu:`https://deployment-id.cloudapp.net/`  
      
    >Vytvoření certifikátu s názvem běžné (CN) rovností na základě GUID adresu URL (třeba **328187776e774ceda8fc57609d404462.cloudapp.net**). Umožňuje přidat certifikát fázované cloudové služby na portálu. Zadejte informace o certifikátu CSDEF a CSCFG souborů, opětovnému vytvoření balíčku aplikace a aktualizovat fázované nasazení použít nový balíček.

## <a name="next-steps"></a>Další kroky

* [Obecné konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Spravovat Cloudová služba](cloud-services-how-to-manage-portal.md).
