<properties
    pageTitle="Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory aplikacím | Microsoft Azure"
    description="Azure Active Directory můžete automaticky zřizování uživatelů a skupin na aplikaci nebo identity úložiště, které je přední stěnou pomocí webové služby pomocí rozhraní definované ve specifikaci SCIM Protocol (protokol)"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace

##<a name="overview"></a>Základní informace

Azure Active Directory můžete automaticky zřizování uživatelů a skupin na aplikaci nebo identity úložiště, které je přední stěnou pomocí webové služby pomocí rozhraní podle [Specifikace protokolu SCIM 2.0](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory můžete odesílat žádosti o vytvářet, upravovat a odstraňovat přiřazených uživatelů a skupin na této webové služby, které můžete pak přeložit tyto požadavky na operace po úložišti identity cílové. 

![][1]
*Obrázek: Vytváření ze služby Azure Active Directory úložiště identit prostřednictvím webové služby*

Tuto funkci lze použít ve spojení s možností "[přenést vlastní aplikaci](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" v Azure AD povolit jednotného přihlašování a Automatická uživatele zřizování pro aplikace, které poskytují nebo jsou přední stěnou tak, že SCIM webové služby.

Existují dva případy použití pro SCIM v Azure Active Directory:

* **Zřizování uživatelů a skupin k aplikacím, které podporují SCIM** - aplikace, které podporují SCIM 2.0 použít OAuth nosný tokeny pro ověřování fungovat s Azure AD pole.

* **Sestavte zřizovací řešení pro aplikace, které podporují jiných založené na rozhraní API pro zřízení** - není SCIM aplikací, můžete vytvořit SCIM koncový bod překládat mezi Azure AD SCIM koncového bodu a něco jiného, co rozhraní API aplikace podporuje pro zřizování uživatelů.  Na podporu ve vývoji SCIM koncový bod, nabízíme rozhraní příkazového řádku knihoven spolu s ukázek kódu, které ukazují, jak poskytnout SCIM koncového bodu a přeložení SCIM zprávy.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Zřizování uživatelů a skupin k aplikacím, které podporují SCIM

Azure Active Directory lze nastavit automaticky přiřazené zřizování uživatelů a skupin aplikacemi implementující Web [systému správy identit doménami 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) služby a přijmout OAuth nosný tokeny pro ověřování. V rámci specifikaci SCIM 2.0 aplikací musí být splněné tyto požadavky:

* Vytvoření uživatelů nebo skupin podle bodu 3.3 protokol SCIM podporuje.  

* Úpravy uživatele nebo skupiny s žádostí o opravy podle bodu 3.5.2 protokol SCIM podporuje.  

* Podporuje načítání známé zdroj podle bodu 3.4.1 SCIM protokolu.  

*  Podporuje dotazování uživatele nebo skupiny, jako počet oddílů 3.4.2 SCIM protokolu.  Ve výchozím nastavení jsou uživatelé zpracovávat pomocí externalId a skupiny se zadal dotaz displayName.  

* Podporuje dotaz na uživatelské ID a správcem podle bodu 3.4.2 SCIM protokolu.  

* Podporuje dotazování skupin podle ID a člena podle bodu 3.4.2 SCIM protokolu.  

* Přijme OAuth nosný tokeny se tak mohli ověřovat podle bodu 2.1 SCIM protokolu.

Měli byste se poskytovatele aplikace nebo si přečtěte následující dokumentaci pro příkazy funkce pro kompatibilitu s těmito požadavky poskytovatele aplikace.
 
###<a name="getting-started"></a>Začínáme

Aplikace, které podporují profil SCIM jsme je popsali výše můžete být připojeni pro službu Azure Active Directory v galerii aplikace Azure AD pomocí funkce "vlastní" aplikace. Po připojení Azure AD se spustí proces synchronizace každých 5 minut tam, kde dotazů aplikace SCIM koncový bod pro uživatelé a skupiny a vytvoří nebo upraví podle podrobnosti o přiřazení.

**Připojení aplikace, která podporuje SCIM:**

1.  Ve webovém prohlížeči spusťte portálu Microsoft Azure správy na https://manage.windowsazure.com.
2.  Přejděte na **služby Active Directory > adresář > [svůj adresář] > aplikací**a vyberte **Přidat > Přidat aplikaci z Galerie**.
3.  Vyberte kartu **vlastní** na levé straně, zadejte název aplikace a klepněte na ikonu zaškrtnutí objekt aplikace vytvořte.

![][2]

4.  Na obrazovce výsledné tlačítko druhého **zřízení účtu konfigurovat** .
5.  V poli **Zřizování koncový bod URL** zadejte adresu URL koncový bod SCIM aplikace.
6.  Vyžaduje-li koncový bod SCIM nosný token OAuth od vydavatele než Azure AD, zkopírujte do pole **Ověření tokenu (volitelné)** požadované nosný token OAuth. Je toto pole je prázdné a potom Azure AD bude obsahovat nosný token OAuth vystaveného Azure AD pomocí každý požadavek. Aplikace, které používají Azure AD jako poskytovatele idenity můžete ověřit Azure AD-Vystavitel token.
7.  Klikněte na tlačítko **Další**a klikněte na tlačítko **Spustit Test** mít Azure Active Directory pokus o připojení k SCIM koncového bodu. Pokud se nezdaří pokusy, zobrazí se diagnostické informace.  
8.  Pokud pokusy o připojení k aplikaci proběhl úspěšně, klikněte na tlačítko **Další** na zbývající obrazovky a klepněte na tlačítko **Dokončit** zavřete dialogové okno.
9.  Na obrazovce výsledné tlačítko třetí **Přiřadit účty** . Ve výsledném části Uživatelé a skupiny přiřadíte uživatelů nebo skupin, do kterých chcete poskytování aplikace.
10. Jakmile uživatelům a skupinám jsou udělovány, klikněte na kartu **Konfigurace** panelu v horní části obrazovky.
11. V části **Zřízení účtu**potvrďte, že je nastaven stav na. 
12. V části **Nástroje**kliknutím Zahájení debat zřizovací proces **restartováním zřízení účtu** .

Všimněte si, že 5 až 10 minut může uplynutí procesu zřizovací začne odeslání žádosti o SCIM koncový bod.  Přehled pokusy o připojení je k dispozici karta řídicí panel aplikace a sestavy zřizovací aktivity a chyby zřizování si můžete stáhnout z Záložka sestavy v adresáři.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Vytváření vlastních zřizování řešení pro všechny aplikace

Vytvořením SCIM webové služby, které rozhraní se službou Azure Active Directory, můžete povolit jednoho uživatele přihlašování a Automatická zřizování libovolných aplikace, který poskytuje ZBÝVAJÍCÍ nebo SOAP uživatelské rozhraní API pro zřízení.

Tady je, jak to funguje:

1.  Azure AD poskytuje s názvem [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)běžné knihovnu infrastruktury jazyk. Systém doplňky a vývojáři slouží k vytvoření a nasazení koncový bod na základě SCIM webové služby lze připojit Azure AD k libovolné aplikaci identity úložiště tuto knihovnu.
2.  Mapování jsou součástí webová služba schématu standardizovaným uživateli přiřadit schématu uživatele a protokol požadovaný aplikací.
3.  Adresa URL koncového bodu registraci v Azure AD jako součást vlastní aplikaci v galerii aplikace.
4.  Uživatelům a skupinám jsou udělovány k této aplikaci v Azure AD. Po přiřazení jsou vloženy do fronty k synchronizaci pro cílovou aplikaci. Synchronizace proces zpracování fronty spustí každých 5 minut.

###<a name="code-samples"></a>Ukázky

Usnadnit, sadu [kódu ukázky](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) jsou za předpokladu, že, vytvořte koncový bod SCIM webové služby a ukazují automatické vytváření. Jeden vzorek tvoří poskytovatele udržuje souboru s řádky s hodnotami oddělenými čárkami představující uživatelé a skupiny.  Druhý je poskytovatele, který pracuje na službě Amazon webové služby identit a správu přístupu.  

**Zjistit předpoklady pro**

* Visual Studio 2013 nebo novější
* [Azure SDK pro .NET](https://azure.microsoft.com/downloads/)
* Počítače se systémem Windows, který podporuje ASP.NET framework 4.5 má být použit jako koncový bod SCIM. Tento počítač musí být přístupné z cloudu
* [Předplatné Azure pomocí zkušební verzi nebo licencované verze Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Ukázka Amazon AWS vyžaduje knihovny z [AWS sady nástrojů Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Naleznete v souboru README zahrnutý v sadě vzorku další informace

###<a name="getting-started"></a>Začínáme

Nejjednodušší způsob, jak implementovat SCIM koncový bod, který může přijímat zřizovací požadavky z Azure AD je sestavovat a nasazovat ukázka kódu, který výstupy zřizování uživatelů do souboru textový soubor s oddělovači (CSV).

**Vytvoření koncového bodu SCIM vzorku:**

1.  Stáhnout balíček kód ukázkové na [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Rozbalení balíčku a jeho umístění na počítači se systémem Windows na místo, třeba C:\AzureAD-BYOA-Provisioning-Samples\.
3.  V této složce spuštění FileProvisioningAgent řešení ve Visual Studiu.
4.  Vyberte **Nástroje > Správce balíčku knihovny > Správce balíčků konzoly**a příkazy pod projektu FileProvisioningAgent řešení odkazy:

    Instalační balíček Instalační balíček Microsoft.SystemForCrossDomainIdentityManagement Microsoft.IdentityModel.Clients.ActiveDirectory instalační balíček Instalační balíček Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Vytvoření projektu FileProvisioningAgent.
6.  Spusťte aplikaci příkazového řádku v systému Windows (jako správce) a pomocí příkazu **cd** přejděte v adresáři složky **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** .
7.  Spusťte příkaz pod nahrazení < adresy ip > IP nebo domény název počítače se systémem Windows.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  V systému Windows klikněte v části **Nastavení systému Windows > síť a Internet nastavení**, zaškrtněte **Brána Firewall systému Windows > Upřesnit nastavení**a vytvořte **Příchozí pravidla** , které umožňuje příchozí přístup k portu 9000.
9.  Pokud je počítač Windows za směrovačem, směrovači muset nakonfigurovat pro přístup k překládání mezi jeho port 9000, který je vystaven na Internetu a port 9000 počítače Windows. Toto je nutný pro Azure AD být mít přístup k této koncového bodu v cloudu.


**Registrace koncový bod SCIM vzorku v Azure AD:**

1.  Ve webovém prohlížeči spusťte portálu Microsoft Azure správy na https://manage.windowsazure.com.
2.  Přejděte na **služby Active Directory > adresář > [svůj adresář] > aplikací**a vyberte **Přidat > Přidat aplikaci z Galerie**.
3.  Vyberte kartu **vlastní** na levé straně, zadejte název, třeba "SCIM Test aplikaci" a klikněte na ikonu zaškrtnutí objekt aplikace vytvořte. Všimněte si, že objekt aplikace vytvořen úmyslu představují cílovou aplikaci, kterou by vytváření a implementace jednotného přihlašování pro a nejen SCIM koncový bod.

![][2]

4.  Na obrazovce výsledné tlačítko druhého **zřízení účtu konfigurovat** .
5.  V dialogovém okně zadejte adresu URL a port koncového bodu SCIM zobrazenou po Internetu. To bude něco jako http://testmachine.contoso.com:9000 nebo http://<ip-address>:9000/, kde je < ip adresa > Internetu zveřejněným IP adres.  
6.  Klikněte na tlačítko **Další**a klikněte na tlačítko **Spustit Test** mít Azure Active Directory pokus o připojení k SCIM koncový bod. Pokud se nezdaří pokusy, zobrazí se diagnostické informace.  
7.  Pokud pokusy o připojení k webové službě úspěšné, klikněte na tlačítko **Další** na zbývající obrazovky a klepněte na tlačítko **Dokončit** zavřete dialogové okno.
8.  Na obrazovce výsledné tlačítko třetí **Přiřadit účty** . Ve výsledném části Uživatelé a skupiny přiřadíte uživatelů nebo skupin, do kterých chcete poskytování aplikace.
9.  Jakmile uživatelům a skupinám jsou udělovány, klikněte na kartu **Konfigurace** panelu v horní části obrazovky.
10. V části **Zřízení účtu**potvrďte, že je nastaven stav na. 
11. V části **Nástroje**kliknutím Zahájení debat zřizovací proces **restartováním zřízení účtu** .

Všimněte si, že 5 až 10 minut může uplynutí procesu zřizovací začne odeslání žádosti o SCIM koncový bod.  Přehled pokusy o připojení je k dispozici karta řídicí panel aplikace a sestavy zřizovací aktivity a chyby zřizování si můžete stáhnout z v adresáři kartu sestavy.

Posledním krokem při ověřování vzorku je otevřít soubor TargetFile.csv ve složce \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug na počítači se systémem Windows. Po spuštění procesu zřizovací ukazuje tento soubor podrobné informace o všech přiřazených a zřízení uživatelé a skupiny.

###<a name="development-libraries"></a>Vývojové knihoven

Vyvinout vlastní webová služba, která odpovídá specifikaci SCIM, nejprve seznamte s následující knihovny poskytované společností Microsoft pomáhají urychlit proces vývoje: 

**1:**  Společné knihovny Language Infrastructure nabízeny pro použití s jazyky založené na této infrastruktury, například C#.  Jednu z těchto knihoven [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)deklaruje rozhraní, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, vidíte na obrázku dole.  Vývojář pomocí knihoven implementovat rozhraní s předmětu, který může být uvedené, obecně jako poskytovatele služby.  Knihovny umožňují vývojáři snadno nasazení webové služby, které odpovídá specifikaci SCIM, buď hostované v rámci Internetové informační služby nebo libovolného spustitelný sestavení běžné infrastruktury jazyk.  Požadavky na této webové službě bude převeden na volání na poskytovatele metody, které by naprogramován tak, že vývojář k ovládání pro některé identity úložiště.    

![][3]

**2:** [Rutiny směrování Express](http://expressjs.com/guide/routing.html) jsou k dispozici pro analýzu node.js žádost o objekty představující volat (podle specifikaci SCIM), provedené node.js webové služby.     

###<a name="building-a-custom-scim-endpoint"></a>Vytváření vlastních SCIM koncový bod

Použití knihoven jsme je popsali výše, vývojáři pomocí těchto knihoven můžete hostovat svých služeb do libovolné spustitelný běžné Language Infrastructure sestavení nebo ve Internetové informační služby.  Tady je ukázka kód hostitelské službě v rámci spustitelný na adrese http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Je důležité mít na paměti, že tuto službu musí mít HTTP adresa a server ověřovací certifikát z nich kořenová certifikační autorita je jedním z následujících akcí: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ověřovací certifikát serveru vázat port na Windows host pomocí nástroje prostředí sítě, třeba takto: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Tady zadaná hodnota argumentu certhash je Miniatura certifikátu, zatímco zadaná hodnota argumentu ID aplikace je libovolného globálně jedinečný identifikátor.  

K hostování služby v rámci Internetové informační služby, by vývojář vytvářet sestavení běžné Language Infrastructure kód knihovny s třídu s názvem při spuštění v výchozí obor názvů sestavení.  Tady je ukázka blok předmětu: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Zpracování koncový bod ověřování

Požadavky z Azure Active Directory obsahovat nosný token OAuth 2.0.   Jiné služby, která přijímá žádosti by měl ověřit Vystavitel jako Azure Active Directory jménem očekávané Azure Active Directory klienta, přístup k grafu webové služby Azure Active Directory.  V tokenu Vystavitel je určen deklaraci iss jako "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  V tomto příkladu základní adresu hodnota deklarace https://sts.windows.net, identifikuje Azure Active Directory jako vydavatel, když segmentu relativní adresu cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, je jedinečný identifikátor klienta služby Azure Active Directory, jejímž jménem byl vydán tokenu.  Pokud token byl vydán pro přístup k grafu webové služby Azure Active Directory, identifikátor tuto službu 00000002-0000-0000-c000-000000000000, by měl být na hodnotu tokenu oblast deklarace.  

Vývojáři použití knihoven běžné Language Infrastructure poskytované společností Microsoft pro vytváření služby SCIM může ověřovat žádosti o služby Azure Active Directory pomocí balíček Microsoft.Owin.Security.ActiveDirectory provedením těchto kroků: 

**1:**  U poskytovatele implementace vlastnost Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tím, že ho vrátit způsob, jak se jmenovat pokaždé, když běží služba: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Přidejte následující kód pro příslušný způsob mít jakékoliv žádosti o se koncové body služby ověřeny jako opatřených token vydané Azure Active Directory jménem pro přístup k grafu webové služby Azure Active Directory zadaný klienta: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Uživatele a skupiny schématu

Azure Active Directory lze vytvořit dva typy zdrojů k webovým službám SCIM.  Tyto typy zdrojů jsou uživatelé a skupiny.  

Prostředků pro uživatele jsou označeny identifikátor schématu urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, která je součástí tohoto specifikace protokolu: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Výchozí mapování atributy uživatelům Azure Active Directory atributy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User zdroje je k dispozici v tabulce 1, pod.  

Skupina zdroje jsou označeny identifikátor schématu http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabulka 2 ukazuje, výchozí mapování atributy skupiny v Azure Active Directory atributů http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group zdrojů.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabulka 1: Výchozí mapování atributu uživatele

| Azure Active Directory uživatele | název urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktivní |
| displayName | displayName |
| TelephoneNumber faxu | .value phoneNumbers [typ eq "faxu"] |
| givenName (jméno) | name.givenName |
| Funkce | Název |
| pošta | .value e-mailů [typ eq "práce"] |
| mailNickname | externalId |
| Správce | Správce |
| mobilní | .value phoneNumbers [typ eq "mobilní"] |
| Objektu | ID |
| PSČ | .postalCode adresy [typ eq "práce"] |
| proxy adresy | e-mailů [Zadejte eq "jiné"]. Hodnota |
| fyzické doručení OfficeName | adresy [Zadejte eq "jiné"]. Formátovaný |
| streetAddress | .streetAddress adresy [typ eq "práce"] |
| Surname (příjmení) | name.familyName |
| telefonní číslo | .value phoneNumbers [typ eq "práce"] |
| uživatel PrincipalName | uživatelské jméno |


###<a name="table-2-default-group-attribute-mapping"></a>Tabulka 2: Výchozí skupiny atribut mapování

| Azure skupiny služby Active Directory | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| pošta | .value e-mailů [typ eq "práce"] |
| mailNickname | displayName |
| Členové | Členové |
| Objektu | ID |
| proxyAddresses | e-mailů [Zadejte eq "jiné"]. Hodnota |


##<a name="user-provisioning-and-de-provisioning"></a>Zřizování uživatelů a zrušte pro zřízení

Obrázek ukazuje, zprávy, že Azure Active Directory odešle SCIM služby Správa životního cyklu uživatele v jiné identity obsahují.  Diagram také ukazuje, jak bude SCIM služba implementovaná pomocí knihoven běžné Language Infrastructure poskytované společností Microsoft pro vytváření těchto služeb přeložena tyto požadavky do volání metod poskytovatele.  

![][4]
*Obrázek: Zřizování uživatelů a zrušte zřizování sekvence*

**1:**  Azure Active Directory dotazu služby pro uživatele s hodnotě atributu externalId odpovídající hodnotě atributu mailNickname uživatele v Azure Active Directory.  Dotaz se vyjádřený žádost Hypertext Transfer Protocol tohoto typu, ve kterém je jyoung ukázkou mailNickname uživatele v Azure Active Directory: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Pokud službu tvořily původní použití knihoven běžné Language Infrastructure poskytované společností Microsoft k provedení SCIM services, bude žádost převést na volání metody dotazu poskytovatele.  Tady je podpis tato metoda: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Tady je definice rozhraní Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

V případě výše uvedené ukázkové dotaz pro uživatele se dané hodnoty pro atribut externalId hodnoty argumentů předán metodu dotaz bude následujícím způsobem: 

* Parametry. AlternateFilters.Count: 1
* Parametry. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* Parametry. AlternateFilters.ElementAt(0). Porovnávací operátor: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. ID žádosti"] 

**2:**  Pokud odpověď na dotaz a služby pro uživatele s hodnotě atributu externalId odpovídající hodnotě atributu mailNickname uživatele v Azure Active Directory nevrátí všichni uživatelé, pak Azure Active Directory bude požadovat, aby služba zřízení uživatele odpovídající jedné v Azure Active Directory.  Tady je příklad žádost: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Běžné Language Infrastructure knihoven poskytované společností Microsoft k provedení SCIM services by přeložit žádosti do hovoru na metodu vytvořit poskytovatele služby.  Metoda Create má Tenhle podpis: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

V případě žádosti o zřízení uživatele bude hodnota argumentu zdroje instanci Microsoft.SystemForCrossDomainIdentityManagement. Třídy Core2EnterpriseUser definované v knihovně Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Pokud zřízení uživatel úspěšně a pak provádění metodu očekává vrátíte instanci Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser předmětu, s hodnotou vlastnost identifikátor nastavenou na jedinečný identifikátor nově zřízení uživatele.  

**3:**  Aktualizovat uživatele známé úložiště identity přední stěnou tak, že SCIM, bude Azure Active Directory postupovat vyžádat aktuální stav uživatele služby s žádostí o zpráva: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

Ve službě vytvořené pomocí knihoven běžné Language Infrastructure poskytované společností Microsoft k provedení SCIM services bude žádost převést na volání metody načíst poskytovatele služby.  Tady je podpis způsob získání: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

V případě předchozího příkladu žádost o k načtení aktuální stav uživatele hodnoty vlastnosti objektu k dispozici jako hodnota argumentu parametry budou následujícím způsobem: 

* Identifikátor: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Pokud atribut odkaz se aktualizují, pak Azure Active Directory bude dotazu služeb a zjistit, zda aktuální hodnotu atributu odkaz v úložišti identity přední stěnou službou už odpovídá hodnotě atributu v Azure Active Directory.  V případě uživatelů je atribut pouze z nich bude aktuální hodnotu dotazovaném tímto způsobem atribut správce.  Tady je příklad žádost o zjistěte, jestli má atribut Správce objektu určitého uživatele aktuálně určitou hodnotu: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Hodnota parametru dotazu atributy id, označuje, pokud existuje objektu uživatele, která splňuje zadané jako hodnota parametru dotazu filtr výrazu a potom službu očekává odpoví urn: ietf:params:scim:schemas:core:2.0:User nebo urn: ietf:params:scim:schemas:extension:enterprise:2.0:User zdrojů, včetně pouze hodnoty atributu id tohoto zdroje.  Samozřejmě, je hodnota atributu id známé žadateli předán – je součástí hodnota parametru dotazu filtr; má s žádostí o jeho účel skutečně požadavek na minimální číselné zdroje, aby neodpovídající pro výraz filter jako indikaci též takový objekt existuje.   

Pokud službu tvořily původní použití knihoven běžné Language Infrastructure poskytované společností Microsoft k provedení SCIM services, bude žádost převést na volání metody dotazu poskytovatele.  Hodnota vlastnosti objektu k dispozici jako hodnota argumentu parametrů bude následujícím způsobem: 

* Parametry. AlternateFilters.Count: 2
* Parametry. AlternateFilters.ElementAt(x). AttributePath: "identifikátor"
* Parametry. AlternateFilters.ElementAt(x). Porovnávací operátor: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parametry. AlternateFilters.ElementAt(y). AttributePath: "správce"
* Parametry. AlternateFilters.ElementAt(y). Porovnávací operátor: ComparisonOperator.Equals
* Parametry. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* Parametry. RequestedAttributePaths.ElementAt(0): "identifikátor"
* Parametry. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Tady hodnota argumentu index x může být 0 a 1, může být hodnota index y nebo může být hodnota x 1 a hodnota y může být 0, v závislosti na pořadí výrazy filtru parametr dotazu.   

**5:**  Tady je příklad žádost o služby Azure Active Directory do služby SCIM aktualizovat uživatele: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Knihovny Microsoft Common Language Infrastructure k provedení SCIM services by přeložit žádost do hovoru na metodu aktualizace poskytovatele služby.  Tady je podpis tato metoda: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



V případě výše uvedené příklady žádost o aktualizovat uživatele bude mít objekt uvedený jako hodnota argumentu oprava těchto nemovitostí s hodnotou: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest jako PatchRequest2). Operations.Count: 1
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Název operace: OperationName.Add
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "správce"
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Odkaz: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Hodnota: 2819c223-7f76-453a-919d-413861904646

**6:**  Zrušit zřízení uživatele z úložiště identity přední stěnou službou SCIM, bude Azure Active Directory pošlete žádost o zpráva: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Pokud službu tvořily původní použití knihoven běžné Language Infrastructure poskytované společností Microsoft k provedení SCIM services, bude žádost převést na volání metodu Delete poskytovatele služby.   Tento způsob zahrnuje podpis: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Objekt uvedený jako hodnota argumentu resourceIdentifier bude mít tyto nemovitostí s hodnotou v případě předchozí příklad požadavek zrušte zřízení uživatele: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Zřizování skupiny a zrušte pro zřízení

Obrázek ukazuje, že Azure Active Directory odešle SCIM služby Správa životního cyklu skupiny v jiné identity zprávy obsahují.  Tyto zprávy liší od zprávy, které se týkají uživatelům třemi způsoby: 

* Schéma prostředku skupiny budou označeny jako http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Požadavky na získání skupiny bude stanovit, že atribut členy vyloučit z libovolného zdroje podle odpověď na požadavek.  
* Požadavky na zjistěte, jestli má odkaz atribut určitou hodnotu, budou žádosti o atributu členy.  

![][5]
*Obrázek: Zřizování skupiny a zrušte zřizování sekvence*

##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Automatizace uživatele zřizování/zrušení zajišťování SaaS aplikace](active-directory-saas-app-provisioning.md)
- [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
- [Psaní výrazů pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Definování rozsahu filtry pro zřizování uživatelů](active-directory-saas-scoping-filters.md)
- [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
