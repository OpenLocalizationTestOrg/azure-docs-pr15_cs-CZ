<properties 
    pageTitle="Vytvořit Web App v aplikaci služby Azure pomocí Azure SDK jazyka Java" 
    description="Naučte se vytvářet Web App na aplikaci služby Azure programově Azure SDK pomocí jazyka Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Vytvořit Web App v aplikaci služby Azure pomocí Azure SDK jazyka Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Základní informace

Tento návod ukazuje, jak vytvořit Azure SDK pro aplikaci Java, která vytvoří do webových aplikací služby [Azure aplikace][]a potom nasazení aplikace do ní. Se skládá ze dvou částí:

- Část 1 ukazuje, jak můžete vytvořit aplikaci Java, která vytvoří web appu.
- Část 2 ukazuje, jak vytvořit jednoduchý JSP "Ahoj světe" aplikace a pak použijte FTP klienta nasazení kódu pro aplikaci služby.


## <a name="prerequisites"></a>Zjistit předpoklady pro

### <a name="software-installations"></a>Instalace softwaru

Kód AzureWebDemo aplikace v tomto článku napsané pomocí Azure Java sady SDK 0.7.0, které si můžete nainstalovat pomocí [Webové platformy][] (WebPI). Navíc nezapomeňte použít nejnovější verzi [Azure sada nástrojů pro Eclipse][]. Po instalaci SDK aktualizovat závislosti v projektu zatmění spuštěním **Aktualizovat rejstřík** v **Maven úložištích**a potom znova přidejte nejnovější verzi obalu v okně **závislosti** . Kliknutím můžete ověřit verzi softwaru nainstalovaných v zatmění **Nápověda > podrobné informace o instalaci**; potřebujete alespoň následující verze:

- Balíček pro knihovny Microsoft Azure Java 0.7.0.20150309
- Zatmění integrovaném vývojovém prostředí pro vývojáře í Java 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Vytváření a konfigurace cloudové zdroje na Azure

Před zahájením tohoto postupu, budete muset máte aktivní předplatné Azure a nastavit výchozí Active Directory (AD) na Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Vytvoření v Azure Active Directory (AD)

Pokud už nemáte Active Directory (AD) předplatného Azure, přihlaste se k [Azure klasické portál][] pomocí svého účtu Microsoft. Pokud máte víc předplatných, klikněte na **předplatná** a vyberte výchozí adresář pro předplatné, které chcete použít pro tento projekt. Klikněte na tlačítko **použít** přepněte do zobrazení, které předplatné.

1. V nabídce vlevo vyberte **Služby Active Directory** . **Na nový > adresáře > vytvořit vlastní**.

2. Do pole **Přidat adresář**vyberte **Vytvořit nový adresář**.

3. Do pole **název**zadejte název adresáře.

4. V **doméně**zadejte název domény. Toto je název základní domény, který je součástí ve výchozím nastavení se do adresáře má formát `<domain_name>.onmicrosoft.com`. Můžete nazvěte ji na základě názvu adresáře nebo jiný název domény, kterou vlastníte. Později můžete přidat jiný název domény, který už vaše organizace používá.

5. **Země nebo oblast**vyberte národní prostředí.

Další informace o AD najdete v článku [Co je Azure AD adresáře][]?


### <a name="create-a-management-certificate-for-azure"></a>Vytvoření certifikátu správy pro Azure

Azure SDK jazyka Java používají Správa certifikátů k ověření Azure předplatná. Toto jsou certifikáty x.509v3, které používáte k ověření klientská aplikace, která používá rozhraní API služby správy k následným akcím jménem vlastník předplatného pro správu předplatného zdroje.

Kód v tomto postupu používá certifikátu podepsaného svým držitelem ověření s Azure. Tento postup musíte vytvořit certifikát a předem nahrát na [Azure klasické portálu][] . Tento postup zahrnuje následující kroky:

- Vytvořit soubor PFX představující klientský certifikát a uložit, abyste ji místně.
- Ze souboru PFX generovat certifikát management (soubor CER).
- Nahrání souboru CER k předplatnému Azure.
- Soubor PFX převeďte JKS, protože Java používá tento formát ověření pomocí certifikáty.
- Napište aplikace ověřovací kód, který odkazuje na místní JKS soubor.

Po dokončení tohoto postupu certifikát CER bude nacházet v Azure předplatné a certifikát JKS budou uloženy na místní jednotku pevného disku. Další informace o osvědčení o řízení najdete v článku [vytvořte a uložte certifikát správy Azure][].


#### <a name="create-a-certificate"></a>Vytvoření certifikátu

Pokud chcete vytvořit vlastní certifikát podepsaný svým držitelem, spusťte konzolu příkaz v operačním systému a následující příkazy.

> **Poznámka:**  Počítač, na kterém spustíte tento příkaz může mít JDK nainstalovaný. Cesta k keytool taky, závisí na umístění, ve kterém instalovat JDK. Další informace najdete v tématu [spolu s nástroji pro správu certifikát (keytool)][] v online dokumenty Java.

Aby se vytvořil soubor .pfx:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Aby se vytvořil soubor .cer:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

kde je:

- `<java-install-dir>`je cesta k adresáři, ve kterém jste nainstalovali Java.
- `<keystore-id>`je identifikátor keystore položky (například `AzureRemoteAccess`).
- `<cert-store-dir>`Cesta k v adresáři, ve kterém chcete uložit certifikáty (například `C:/Certificates`).
- `<cert-file-name>`je název soubor certifikátu (například `AzureWebDemoCert`).
- `<password>`je heslo, které zvolíte možnost Zamknout certifikátu; musí být alespoň na úrovni 6 znaků. Žádné heslo, můžete zadat, i když se nedoporučuje.
- `<dname>`Rozlišující název X.500 přiřazené alias a slouží jako pole Vystavitel a předmět v certifikátu podepsaného svým držitelem.

Další informace najdete v tématu [vytvořte a uložte certifikát správy Azure][].


#### <a name="upload-the-certificate"></a>Nahrání certifikátu

Pokud chcete odeslat certifikát podepsaný svým držitelem Azure, přejděte na stránku **Nastavení** portálu klasické a potom klikněte na kartu **Správa certifikátů** . Klikněte na **Uložit** v dolní části stránky a přejděte do umístění souboru CER, kterou jste vytvořili.


#### <a name="convert-the-pfx-file-into-jks"></a>Převedení souboru PFX na JKS

Ve Windows příkazového řádku (systém jako správce), disk cd k adresáři obsahující certifikáty a spusťte tento příkaz, kde `<java-install-dir>` je adresář, ve kterém je nainstalovaná Java ve vašem počítači:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Po zobrazení výzvy zadejte heslo keystore cíl; to bude heslo k souboru JKS.

2. Po výzvě zadejte heslo keystore zdroje. To je heslo, které jste zadali PFX souboru.

Dvě hesla nemusí být stejný. Žádné heslo, můžete zadat, i když se nedoporučuje.


## <a name="build-a-web-app-creation-application"></a>Vytvoření aplikace vytváření Web Appu

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Vytvoření pracovního prostoru zatmění a Maven projektu

V tomto oddílu vytvořit pracovní prostor a Maven projektu pro webovou aplikaci vytváření aplikaci s názvem AzureWebDemo.

1. Vytvoření nového projektu Maven. Klikněte na **Soubor > Nový > Maven projektu**. **Nový projekt Maven**vyberte **vytvořit jednoduchý projekt** a **použít výchozí pracovní prostor umístění**.

2. Na druhé stránce **Nový projekt Maven**zadejte následující údaje:

    - ID skupiny:`com.<username>.azure.webdemo`
    - ID artefaktů: AzureWebDemo
    - Verze: 0.0.1-SNAPSHOT
    - Balení: sklenice
    - Název: AzureWebDemo

    Klikněte na **Dokončit**.

3. Otevřete soubor pom.xml nový projekt v Průzkumník projektu. Vyberte kartu **závislosti** . Je to nový projekt, bez balíčků najdete ještě.

4. Otevřete zobrazení Maven úložištích. **Klikněte na okno > Zobrazit zobrazení > Další > Maven > Maven úložištích** a klikněte na **OK**. Zobrazení **Maven úložištích** se zobrazí v dolní části integrovaném vývojovém prostředí.

5. Otevřete **Globální úložištích**, klikněte pravým tlačítkem myši **centrálním** úložišti a vyberte **Znovu vytvořit Index**.

    ![][1]
    
    Tento krok může trvat několik minut podle rychlosti připojení. Když znovu sestaví index, byste měli vidět balíčků Microsoft Azure v **centrálním** úložišti Maven.

6. V **závislosti**klikněte na **Přidat**. Ve **Skupině Enter ID...** zadejte `azure-management`. Vyberte balíčky pro základní Správa a Správa aplikací služby Web Apps:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Poznámka:** Pokud aktualizujete závislosti po novou verzi, budete muset znovu přidejte všechny závislostí v tomto seznamu.
    > Po klikněte na tlačítko **Přidat** a vyberte každý závislosti, zobrazí se nové verze číslo v seznamu **závislosti** .

Klikněte na **OK**. Azure balíčků zobrazí v seznamu **závislosti** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Psaní kódu jazyka Java vytvořit Web Appu tak, že zavoláte Azure SDK

Dále napište kód, který volá rozhraní API v Azure SDK Java vytvořit aplikaci služby web appu.

1. Vytvoření tříd jazyka Java má být hlavní vstupní bod kódu. V okně Průzkumník projektu klikněte pravým tlačítkem myši na uzel projektu a vyberte **Nový > třídy**.

2. V **Nové tříd jazyka Java**název třídy `WebCreator` a zaškrtněte políčko **Veřejné statické void hlavní** . Výběr by měl vypadat takto:

    ![][2]

3. Klikněte na **Dokončit**. Soubor WebCreator.java se zobrazí v Průzkumník projektu.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Volání Azure rozhraní API pro vytvoření aplikace pro Web App služby


#### <a name="add-necessary-imports"></a>Přidejte nezbytné importy

V WebCreator.java přidejte následující importy; Tyto importy zpřístupnit tříd v knihovnách správy pro využívání rozhraní API Azure:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definování hlavní vstupní bod třídy

Vzhledem k tomu, že má aplikace AzureWebDemo účel vytvořit webovou aplikaci služby aplikace, název hlavní třída pro tuto aplikaci `WebAppCreator`. Tato třída poskytuje kód hlavní vstupní bod, který volá API Správa služby Azure vytvořit web appu.

Přidejte následující definice parametrů pro web app a webspace. Je třeba zadat vlastní Azure předplatné ID a informace o certifikátu.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

kde je:

- `<subscription-id>`je ID Azure předplatného, ve kterém chcete vytvořit zdroj.
- `<certificate-store-path>`je cesty a názvu souboru JKS v adresáři úložiště místní certifikát. Například `C:/Certificates/CertificateName.jks` Linux a `C:\Certificates\CertificateName.jks` pro Windows.
- `<certificate-password>`je heslo, které jste zadali při vytvoření JKS certifikát.
- `webAppName`může být název, jaká jste; Tento postup používá název `WebDemoWebApp`. Úplný název domény je `webAppName` s `domainName` přidaným, tak v tomto případě úplné je doména `webdemowebapp.azurewebsites.net`.
- `domainName`stanovit uvedené výše.
- `webSpaceName`musí mít některou z hodnot definovaných ve třídě [WebSpaceNames][] .
- `appServicePlanName`stanovit uvedené výše.

> **Poznámka:** Při každém spuštění této aplikace, budete muset změnit hodnotu `webAppName` a `appServicePlanName` (nebo odstranit web appu na portálu Azure) před spuštěním aplikaci znova. Spuštění jinak se nepovede, protože v Azure již existuje stejné zdroje.


#### <a name="define-the-web-creation-method"></a>Určení metody vytváření webu

Dále určete způsob, jak vytvořit web appu. Tento způsob `createWebApp`, určuje parametry web app a webspace. Také vytvoří a konfiguruje klienta Správa aplikací služby Web Apps, který je definován [WebSiteManagementClient][] objektu. Správa klienta je klíčová k vytváření webových aplikací Web Apps. Poskytuje RESTful webové služby, které umožňují aplikacím ke správě webových aplikací (provádění operací, jako je vytvoření, aktualizace a odstranění) tak, že zavoláte rozhraní API pro správu služby.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Kód bude výstupní HTTP stav odpovědi označující úspěšném nebo neúspěšném řešení a pokud je úspěšná, bude výstupní název vytvořený web appu.


#### <a name="define-the-main-method"></a>Určení metody main()

Zadejte kód metoda main(), který volá createWebApp() vytvořit web appu.

Nakonec zavolejte `createWebApp` z `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Spusťte aplikaci a ověřte vytvoření webové aplikace

Abyste ověřili, že aplikace běží, klikněte na **Spustit > spustit**. Po dokončení spuštění aplikace byste měli vidět následující výstup v konzole zatmění:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Přihlaste se k portálu Azure klasické a klikněte na **Web Apps**. Nové webové aplikace by se měly v seznamu aplikací Web Apps objevit během pár minut.


## <a name="deploying-an-application-to-the-web-app"></a>Nasazení aplikace pro Web App

Po spuštění AzureWebDemo a vytvoření nové webové aplikace, přihlaste se k portálu klasické, klikněte na **Web Apps**a vyberte **WebDemoWebApp** v seznamu **Aplikací Web Apps** . V aplikaci webové stránky řídicího panelu, klikněte na **Procházet** (nebo klepněte na adresu URL `webdemowebapp.azurewebsites.net`) přejděte k němu. Zobrazí se stránka prázdné zástupný symbol a vzhledem k tomu, že žádný obsah byl publikován na web app zatím.

Další vytvoříte aplikace "Dobrý den" a nasadit na web appu.


### <a name="create-a-jsp-hello-world-application"></a>Vytvoření aplikace JSP Vítáme

#### <a name="create-the-application"></a>Vytvoření aplikace

Předvedení nasazení aplikace na web následující postup ukazuje, jak vytvořit jednoduchý aplikaci "Ahoj světe" Java a odeslat do aplikace služby Web Appu aplikaci vytvořili.

1. Klikněte na **Soubor > Nový > dynamické Web projektu**. Pojmenujte ho `JSPHello`. Chcete-li změnit další nastavení v tomto dialogovém okně nepotřebujete. Klikněte na **Dokončit**.

    ![][3]

2. V aplikaci Project Explorer rozbalte **JSPHello** projekt, klikněte pravým tlačítkem na **obsah webu**a potom klikněte na **Nový > soubor JSP**. V dialogovém okně Nový soubor JSP zadejte název nového souboru `index.jsp`. Klikněte na tlačítko **Další**.

3. V dialogovém okně **Vybrat šablonu JSP** vyberte **Nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.

4. V index.jsp, přidejte tento kód v `<head>` a `<body>` značky oddílů:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Spusťte aplikaci Vítáme v localhost

Před spuštěním tuto aplikaci, musíte nastavit několika vlastností.

1. Klikněte pravým tlačítkem myši na **JSPHello** projekt a vyberte **Vlastnosti**.

2. V dialogovém okně **Vlastnosti** : vyberte **Java sestavit cestu**, vyberte kartu **pořadí a Export** , zkontrolujte **JRE systémová knihovna**a potom klikněte **až** na začátek seznamu.

    ![][4]

3. Také v dialogovém okně **Vlastnosti** : vyberte **Cílový Runtimes** a klikněte na **Nový**.

4. V dialogovém okně **Nové prostředí serveru** vyberte server například **Apache Tomcat 7.0** a klikněte na tlačítko **Další**. V dialogovém okně **Tomcat serveru** nastavit **název** `Apache Tomcat v7.0`a nastavte **Tomcat instalační adresář** na adresář, ve kterém je nainstalovaná verze Tomcat serveru, který chcete použít.

    ![][5]

    Klikněte na **Dokončit**.

5. Potom návrat na stránku **Určené Runtimes** dialogového okna **Vlastnosti** . Vyberte **Apache Tomcat 7.0**a klikněte na **OK**.

    ![][6]

6. V nabídce zatmění **spuštění** klikněte na **Spustit**. V dialogovém okně **Spustit jako** zaškrtněte políčko **Spustit na serveru**. V dialogovém okně **Spustit na serveru** vyberte **Tomcat 7.0 serveru**:

    ![][7]

    Klikněte na **Dokončit**.

7. Při spuštění aplikace byste měli vidět **JSPHello** stránky se zobrazí v okně localhost v zatmění (`http://localhost:8080/JSPHello/`), zobrazí následující zprávu:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Export aplikace jako WAR

Exportujte soubory project web jako souboru archivu (WAR) na webu, můžete ho nasadit na web appu. Následující soubory project web jsou uloženy ve složce obsah webu:

    META-INF
    WEB-INF
    index.jsp

1. Klikněte pravým tlačítkem myši na složku obsah webu a zaškrtněte políčko **Exportovat**.

2. V dialogovém **Exportovat vyberte** klikněte **Web > WAR** soubor a potom klikněte na tlačítko **Další**.

3. V dialogovém okně **Exportovat WAR** vyberte adresáři src v aktuálním projektu a zadejte název souboru WAR na konci. Příklad:

    `<project-path>/JSPHello/src/JSPHello.war`

Další informace o nasazení WAR souborů najdete v článku [Přidání aplikace Java Azure aplikace služby Web Apps](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Nasazení aplikace světě Ahoj pomocí FTP

Vyberte klient FTP v jiných výrobců pro publikování aplikace. Tento postup popisuje dvě možnosti: konzole Kudu integrovaná v Azure; a FileZilla, nástroji Oblíbené s pohodlný a přehledné grafické uživatelského rozhraní.

> **Poznámka:** Azure sada nástrojů pro Eclipse podporuje nasazení úložiště účtů a cloudové služby, ale aktuálně nepodporuje nasazení do webových aplikací. Můžete nasadit účtů úložiště a služby cloudu pomocí projektu nasazení Azure podle popisu v tématu [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale ne na web apps. Použijte jiné metody například FTP nebo GitHub přenášet soubory do webové aplikace.

> **Poznámka:** Nedoporučujeme pomocí protokolu FTP z příkazového řádku systému Windows (příkazového řádku FTP.EXE nástroj, který se dodává se systémem Windows). FTP klientů používajících aktivní FTP, například FTP.EXE, často nebude přes brány firewall. Aktivní FTP určuje interní na základě LAN adresy, ke kterému serveru FTP pravděpodobně nezdaří připojení.

Další informace o nasazení webové aplikace pro aplikaci služby pomocí protokolu FTP naleznete v následujících tématech:

- [Nasazení pomocí nástroje FTP](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Nastavení přihlašovacích údajů nasazení

Zkontrolujte, že spustíte aplikaci **AzureWebDemo** vytvořit web appu. Soubory přenese do tohoto umístění.

1. Přihlaste se k portálu klasické a klikněte na **Web Apps**. Ujistěte se, že v seznamu aplikací pro web se zobrazí **WebDemoWebApp** a ujistěte se, jestli je spuštěný. Klikněte na tlačítko **WebDemoWebApp** otevřete její stránku **řídicího panelu** .

2. Na stránku **řídicího panelu** v části **Rychlé přehledně uspořádané**, klikněte na **Nastavení přihlašovacích údajů nasazení** (pokud už máte nasazení pověření to bude číst **obnovení pověření nasazení**).

    Nasazení přihlašovací údaje jsou přidružené k účtu Microsoft. Potřebujete zadejte uživatelské jméno a heslo, které vám pomohou nasadit pomocí libovolná a FTP. Pomocí těchto přihlašovacích údajů můžete nasadit na libovolný web app na všechna předplatná Azure přidruženého k vašemu účtu Microsoft. Libovolná a FTP přihlašovací údaje nasazení v dialogovém okně zadat a záznam uživatelské jméno a heslo pro budoucí použití.


#### <a name="get-ftp-connection-information"></a>Získat informace o připojení FTP

Použití FTP nasazení soubory aplikace do nově vytvořený web appu, musíte získat informace o připojení. Existují dva způsoby, jak získat informace o připojení. Jedním ze způsobů, je Navštěvujte blog o aplikaci webové stránky **řídicího panelu** ; jiným způsobem je ke stažení webu aplikace publikovat profilu. Profil publikování je soubor XML, který obsahuje informace, například FTP hostitele název a přihlašovací údaje pro váš web apps v aplikaci služby Azure. Můžete uživatelské jméno a heslo pro nasazení na libovolný web app na všechna předplatná přidružené k účtu Azure, nejen tuto položku.

Získat informace o připojení FTP z web appu zásuvné [Azure portálu][]:

1. V části **Základy**najděte a zkopírujte **FTP hostname**. Toto je podobný identifikátor URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. V části **Základy**najděte a zkopírujte **FTP/nasazení uživatelské jméno**. Bude to mít formuláře *webappname\deployment uživatelské jméno*; Příklad `WebDemoWebApp\deployer77`.

Získat informace o připojení FTP z profilu publikování:

1. V zásuvné web appu klikněte na **získat Publikovat profil**. To bude stažení souboru .publishsettings na místní disk.

2. Otevřete soubor .publishsettings v XML editoru nebo v textovém editoru a vyhledejte `<publishProfile>` obsahující prvek `publishMethod="FTP"`. Ho by měl vypadat takto:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Všimněte si, že web appu `publishProfile` mapování nastavení a nastavení správce webu FileZilla takto:

- `publishUrl`je stejné jako **název hostitele FTP**hodnotu, kterou jste nastavili v **Host (hostitel)**.
- `publishMethod="FTP"`znamená, nastavení **protokolu** **FTP - File Transfer Protocol**a **šifrování** **jednoduchým FTP**.
- `userName`a `userPWD` jsou klávesové zkratky pro skutečné uživatelské jméno a heslo hodnoty, které jste zadali při obnovení pověření nasazení. `userName`je stejná jako **nasazení / FTP uživatele**. Mapují na **uživatele** a **hesla** v FileZilla.
- `ftpPassiveMode="True"`znamená, že serveru FTP použita pasivní FTP přenos; na kartě **Nastavení přenosu** vybrána **Pasivní** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurace webové aplikace hostovat aplikace Java

Před publikováním aplikace budete muset změnit nastavení několik tak, aby web appu můžete hostovat aplikace Java.

1. Na portálu klasické přejděte na stránku **řídicího panelu** web appu a klikněte na **Konfigurovat**. Na stránce **Konfigurovat** zadejte následující nastavení.

2. Ve **verzi jazyka Java** výchozí hodnota je **mimo**; Vyberte Java verzi aplikace cílů; například 1.7.0_51. Až to uděláte, taky zkontrolujte, že **Web kontejneru** je nastavená verzi serveru Tomcat.

3. Ve **Výchozích dokumentů**přidejte index.jsp a přesunutí do horní části seznamu. (Výchozí soubor webové aplikace je hostingstart.html.)

4. Klikněte na **Uložit**.


#### <a name="publish-your-application-using-kudu"></a>Publikování aplikace pomocí Kudu

Jedním ze způsobů publikování aplikace je používat konzolu ladění Kudu integrované v Azure. Kudu známý stabilní a konzistentní s aplikací služby Web Apps a Tomcat serveru. Procházením URL následující formulář modelům konzoly pro web app

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Tento postup je umístěn na následující adresu URL; konzole Kudu Přejděte do tohoto umístění:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. V nabídce horní vyberte **ladění konzoly > CMD**.

3. Přejděte do příkazového řádku konzoly `/site/wwwroot` (nebo klikněte na `site`, pak `wwwroot` v zobrazení adresáře v horní části stránky):

    `cd /site/wwwroot`

4. Po určení **verze Java**měli Tomcat serveru vytvořit webapps adresář. Na příkazovém řádku konzoly přejděte do adresáře webapps:

    `mkdir webapps`

    `cd webapps`

5. Přetáhněte JSPHello.war z `<project-path>/JSPHello/src/` a vložíte do zobrazení adresáře Kudu v části `/site/wwwroot/webapps`. Není přetáhněte ho do oblasti "Sem přetáhněte nahrávat a zip", protože Tomcat bude unzip ho.

  ![][8]

Na první JSPHello.war objeví v oblasti adresáře samostatně:

  ![][9]

Ve chvíli (pravděpodobně nižší než 5 minut) Tomcat serveru bude WAR soubor rozbalit do adresáře rozbalené JSPHello. Klikněte na KOŘENOVÉM adresáři zobrazíte zda index.jsp byl rozbaleny a zkopírovat. Pokud ano, přejdete zpět v adresáři webapps, zda byl vytvořen adresáři rozbalené JSPHello. Pokud tyto položky nevidíte, počkejte a opakujte.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Publikování aplikace pomocí FileZilla (volitelné)

Jiný nástroj, který používáte pro publikování aplikace je FileZilla Oblíbené klient FTP třetích stran s pohodlný a přehledné grafické uživatelského rozhraní. Můžete stáhnout a nainstalovat FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) , pokud už nemáte ho. Další informace o použití klienta, najdete v článku [si přečtěte následující dokumentaci FileZilla](https://wiki.filezilla-project.org/Documentation) a tento blogu na [FTP klienti – část 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. V FileZilla, klikněte na **Soubor > Správce webu**.
2. V dialogovém okně **Správce webu** klikněte na **Nový web**. Nový prázdný web FTP se zobrazí v **Vybrat položku** s výzvou k zadání názvu. V tomto procesu nazvěte ji `AzureWebDemo-FTP`.

    Na kartě **Obecné** zadejte následující nastavení:
    - **Host (hostitel):** Zadejte **Název hostitele FTP** , kterou jste zkopírovali z řídicího panelu.
    - **Portu:** (Nechte toto pole prázdné, a to je přenos pasivní serveru určí portu, který chcete použít.)
    - **Protocol (protokol):** FTP File Transfer Protocol
    - **Šifrování:** Použití jednoduchým FTP
    - **Přihlášení typ:** Normální
    - **Uživatele:** Zadejte nasazení / FTP uživatele, který jste zkopírovali z řídicího panelu. Toto je celé FTP uživatelské jméno, které obsahuje formulář *webappname\username*.
    - **Heslo:** Zadejte heslo, které jste zadali při nastavení přihlašovacích údajů nasazení.

    Na kartě **Nastavení přenosu** vyberte **Pasivní**.

3. Klikněte na **Připojit**. Pokud úspěšné, FileZilla na konzole se zobrazí `Status: Connected` zprávy a problém `LIST` příkazu zobrazíte obsah adresáře.

4. Na panelu **místní** webu vyberte ze zdrojového adresáře, ve kterém je umístěn JSPHello.war soubor; cesta bude vypadat podobně jako takto:

    `<project-path>/JSPHello/src/`

5. Na panelu **vzdálený** server vyberte cílovou složku. Budete nasazovat soubor WAR `webapps` adresáře v části kořenový web appu. Přejděte na `/site/wwwroot`, klikněte pravým tlačítkem myši na `wwwroot`a vyberte **vytvořit adresář**. Adresář pojmenovat `webapps` a zadejte tento adresář.

6. Přenos JSPHello.war k `/site/wwwroot/webapps`. Vyberte JSPHello.war v **místní** seznam souborů, klikněte na něj pravým tlačítkem myši a vyberte **Odeslat**. Měli byste vidět se zobrazí v `/site/wwwroot/webapps`.

7. Po zkopírování JSPHello.war k adresáři webapps Tomcat serveru se automaticky Rozbalit (unzip) souborů v souboru WAR. Přestože rozbalování skoro okamžitě začne Tomcat Server, může trvat dlouho doba (případně hodiny) pro soubory, které chcete zobrazit v klientovi FTP.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Spusťte aplikaci Vítáme na Web Appu

1. Po odeslání souboru WAR a ověřit, že Tomcat serveru vytvořil rozbalené `JSPHello` adresáře, procházet a `http://webdemowebapp.azurewebsites.net/JSPHello` spustit aplikaci.

    > **Poznámka:** Pokud kliknete na tlačítko **Procházet** z portálu Microsoft klasické se může zobrazit výchozí webovou stránku sdělují "Tato webová aplikace na základě Java byla úspěšně vytvořena." Možná budete muset aktualizovat webové stránky zobrazit výstup aplikace místo výchozí webovou stránku.

2. Při spuštění aplikace, měli byste vidět na webovou stránku s výstupu následující:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Vyčistit Azure zdroje

Tento postup vytvoří webovou aplikaci aplikaci služby. Můžete se vám nebudou účtovat poplatky pro zdroj, dokud existuje. Pokud chcete dál používat webovou aplikaci pro testování nebo vývoj, měli byste zvážit zastavení nebo jeho odstranění. Do webových aplikací, který má být zastaven pořád být účtovány malých poplatků, ale můžete ho restartovat kdykoli. Odstranění web appu se vymažou všechna data, která jste nahráli do ní.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure aplikace služby]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webové platformy]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure sada nástrojů pro Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klasické portálu]: https://manage.windowsazure.com
[Co je Azure AD adresáře]: http://technet.microsoft.com/library/jj573650.aspx
[Vytvořte a uložte certifikát správy pro Azure]: ../cloud-services/cloud-services-certs-create.md
[Spolu s nástroji pro správu certifikát (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure portálu]: https://portal.azure.com
