<properties
    pageTitle="Služby základní ověřování pro rozhraní API aplikace v aplikaci služby Azure | Microsoft Azure"
    description="Zjistěte, jak chránit aplikace rozhraní API aplikace služby Azure-služeb scénářích."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Služby základní ověřování pro rozhraní API aplikace v aplikaci služby Azure

## <a name="overview"></a>Základní informace

Tento článek vysvětluje, jak používat aplikaci služby ověřování pro *interní* přístup k rozhraní API aplikace. Vnitřní scénář je, kde máte rozhraní API aplikaci, která se mají stát spotřební pouze pomocí vlastního kódu aplikace. Doporučené postupy pro implementaci tento scénář v aplikaci služby je použití Azure AD pro ochranu jen rozhraní API aplikace. Zavolejte chráněné rozhraní API aplikace s nosný token, který získáte z Azure AD zadáním identitu pověření (služby základní). Alternativy k používání Azure AD naleznete v části **Služba Služba ověřování** [Přehled ověřování aplikace služby Azure](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

V tomto článku se dozvíte:

* Jak zamknout aplikaci pro rozhraní API z neověřené Accessu pomocí služby Azure Active Directory (Azure AD).
* Informace o používání chráněných rozhraní API aplikace z rozhraní API aplikace, v prohlížeči nebo mobilní aplikaci pomocí přihlašovacích údajů jistinu (aplikace identita) služby Azure AD. Informace o tom, jak používat z logických aplikace najdete v tématu [Použití vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných použití logických operátorů](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Jak zajistit, že přihlášeného uživatele nelze chráněné rozhraní API aplikace volat z prohlížeče.
* Jak abyste měli jistotu, že chráněné rozhraní API aplikace může vyvolat pouze konkrétní Azure AD služby jistinu.

Tento článek obsahuje dva oddíly:

* V části [konfiguraci služby základní ověřování aplikace služby Azure](#authconfig) uvedený důvod obecně konfigurace ověřování pro nějakou aplikaci rozhraní API a používání chráněných rozhraní API aplikace. V této části platí pro všechny rámce nepodporuje aplikaci služby včetně .NET, Node.js a Java rovnoměrně z vnější.

* Počínaje části [pokračováním výukové programy pro .NET Začínáme –](#tutorialstart) kurz vás provede konfigurace scénář "interní přístupu" pro .NET ukázku aplikace spuštěné v aplikaci služby. 

## <a id="authconfig"></a>Postup při konfiguraci služby základní ověřování v aplikaci služby Azure

Tato část obsahuje obecné pokyny, které se vztahují k aplikaci rozhraní API. Postup konkrétní ukázková aplikace pro .NET seznamu proveďte přejděte k [dalším řadě kurzů .NET rozhraní API aplikace](#tutorialstart).

1. [Azure portál](https://portal.azure.com/), přejděte na zásuvné **Nastavení** rozhraní API aplikace, který chcete zamknout a pak najděte v části **funkce** a klikněte na **ověřování / se tak mohli ověřovat**.

    ![Ověřování/se tak mohli ověřovat Azure portálu](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. V **ověřování / se tak mohli ověřovat** zásuvné, klepněte **na**.

4. V seznamu **akce provedeny, pokud není požadavek ověřit** vyberte **přihlásit pomocí účtu služby Azure Active Directory** .

5. V části **Zprostředkovatelů ověřování**vyberte **Azure Active Directory**.

    ![Ověřování a povolení zásuvné Azure portálu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Konfigurace **Nastavení služby Azure Active Directory** zásuvné vytvořit novou aplikaci Azure AD nebo použití existující aplikaci Azure AD Pokud ještě nemáte, který chcete použít.

    Vnitřní scénáře obvykle zahrnuje aplikace rozhraní API pro volání aplikace rozhraní API. Můžete použít samostatné Azure AD aplikací pro každý rozhraní API aplikace nebo jediným Azure AD aplikace.

    Podrobné pokyny v tomto zásuvné najdete v článku [jak nakonfigurovat aplikaci služby aplikace pomocí služby Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Až skončíte s zásuvné konfigurace poskytovatele ověřování, klikněte na **OK**.

7. V **ověřování / se tak mohli ověřovat** zásuvné, klikněte na tlačítko **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Když to uděláte, aplikaci služby umožňuje pouze žádosti z volající v nakonfigurované Azure AD klienta. Žádný ověřování nebo se tak mohli ověřovat kód je potřeba v zamknutém rozhraní API aplikace. Nosný token předána rozhraní API aplikace spolu s běžně používaných nároky v záhlavích HTTP a si můžete přečíst těchto informací v kódu ověřte, že žádosti se z určitého volající, například objekt zabezpečení služeb.

Tahle funkce ověřování funguje stejným způsobem jako pro všechny jazyky, které služba aplikace podporuje, včetně .NET, Node.js a Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Jak používat aplikaci chráněné rozhraní API

Volající musí poskytovat nosný token Azure AD pomocí rozhraní API volání. K získání nosný token pomocí služby základní přihlašovacích údajů, volající používá Active Directory Authentication Library (ADAL pro [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)nebo [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Chcete-li získat token, kód, který volá ADAL poskytuje ADAL následující informace:

* Název vašeho klienta Azure AD.
* ID klienta a tajná klienta (aplikace klíč) aplikace Azure AD pro spojené s volajícím začnete.
* ID klienta Azure AD aplikace přidružená k chráněné rozhraní API aplikace. (Pokud jediným Azure AD aplikaci používají, je to stejné ID klienta jako pro volající.)

Tyto hodnoty jsou k dispozici na stránkách Azure AD [Azure klasické portálu](https://manage.windowsazure.com/).

Jakmile získal tokenu zahrne ji volající s žádostí o protokolu HTTP se tak mohli ověřovat záhlaví.  Aplikace služby ověří tokenu a umožňuje žádosti o přístup chráněné rozhraní API aplikace.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Jak zabránit rozhraní API aplikace access uživateli ve stejném klientovi

Tokeny nosný pro uživatele ve stejném klientovi jsou považovány za platné chráněné rozhraní API aplikace.  Pokud chcete zajistit, že pouze hlavní název služby upoutat chráněné rozhraní API aplikace, přidejte kód v aplikaci chráněné rozhraní API pro ověřování deklarací následující z tokenu:

* `appid`ID klienta Azure AD aplikace, která souvisí s volajícím začnete by měl být. 
* `oid`(`objectidentifier`) by měl být služby základní ID volajícího. 

Aplikace služby obsahuje také `objectidentifier` převzetí v záhlaví X-MS--JISTINU – ID klienta.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Jak chránit rozhraní API aplikace z přístup pomocí prohlížeče

Pokud nemáte ověřování deklarací v kódu v aplikaci chráněné rozhraní API a použijte samostatný Azure AD aplikace chráněné rozhraní API aplikace, ujistěte se, že aplikace Azure AD odpověď adresa URL je stejná jako základní adresu URL rozhraní API aplikace. Pokud adresa URL odpovědět bodů přímo do aplikace chráněné rozhraní API, uživatelem ve stejném klientovi Azure AD přejděte do aplikace rozhraní API, přihlaste se a úspěšně volání rozhraní API.

## <a id="tutorialstart"></a>Pokračování řadě kurzů .NET rozhraní API aplikace

Pokud sledujete Node.js nebo Java řadě kurzů pro rozhraní API aplikace, přejděte do oddílu [Další kroky](#next-steps) . 

Zbývající část tohoto článku pokračuje řadě kurzů .NET rozhraní API aplikace a předpokládá, že jste dokončili [kurz ověřování pro uživatele](app-service-api-dotnet-user-principal-auth.md) a ukázkové aplikace spuštěné v Azure pomocí ověřování uživatelů povolené.

## <a name="set-up-authentication-in-azure"></a>Nastavení ověřování v Azure

V této části můžete nakonfigurovat aplikaci služby tak, aby pouze požadavků HTTP dovoloval kontaktovat rozhraní API aplikace osy dat jsou ty, které máte platný Azure AD nosný tokeny. 

V následující části konfiguraci aplikace rozhraní API střední úroveň odeslat pověření aplikací Azure AD, vrátit nosný token a nosný token odešlete data osy rozhraní API aplikace. Tento postup je znázorněn v diagramu.

![Služba ověřování diagramu](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Pokud dostanete do potíží při výuková pokynů naleznete v části [Poradce při potížích](#troubleshooting) na konci kurzu. 

1. [Azure portál](https://portal.azure.com/)přejděte na zásuvné **Nastavení** rozhraní API aplikace, kterou jste vytvořili rozhraní API aplikace ToDoListDataAPI (osy dat) a potom klikněte na **Nastavení**.

2. V **Nastavení** zásuvné najděte v části **funkce** a klikněte na **ověřování / se tak mohli ověřovat**.

    ![Ověřování/se tak mohli ověřovat Azure portálu](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. V **ověřování / se tak mohli ověřovat** zásuvné, klepněte **na**.

4. V seznamu **akce provedeny, pokud není požadavek ověřit** vyberte **přihlásit pomocí účtu služby Azure Active Directory**.

    To je nastavení, které se služba aplikace zajistit, která jenom ověřené žádosti o přístup rozhraní API aplikace. Požadavky, které máte platný nosný tokeny aplikaci služby tokeny podél předává rozhraní API aplikace a naplní záhlaví HTTP se běžně používaných deklarací zpřístupnit tyto informace snadněji kódu.

5. V části **Zprostředkovatelů ověřování**klikněte na **Azure Active Directory**.

    ![Ověřování a povolení zásuvné Azure portálu](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. V **Nastavení služby Azure Active Directory** zásuvné klikněte na **Express**.

    S **Express** možnost Azure automaticky vytvořit aplikaci AAD Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nemusíte vytvoření klienta, protože každý účet Azure automaticky nějaký použitý.

7. V části **režim správy**klikněte na **Vytvořit novou aplikaci AD** Pokud není vybrána.

    Na portálu připojí vstupním poli **Vytvořit aplikace** s výchozí hodnotou. Ve výchozím nastavení aplikace Azure AD se stejným názvem jako rozhraní API aplikace. Pokud chcete, můžete zadat jiný název.
    
    ![Nastavení služby Azure AD](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Poznámka**: jako alternativu můžete použít jednu Azure AD aplikace pro volající rozhraní API aplikace a chráněné rozhraní API aplikace. Pokud jste se rozhodli, které alternativní, nemusí vzhledem k tomu, že jste již vytvořili aplikace Azure AD dříve v tomto kurzu ověřování uživatele možnost **Vytvořit novou aplikaci AD** . Pro účely tohoto návodu použijete vašeho oddělení Azure AD žádosti o aplikaci volání rozhraní API a chráněné rozhraní API aplikace.

8. Poznamenejte si hodnotu, která je ve vstupním poli **Vytvořit aplikace** ; budete vyhledat této AAD aplikace na portálu Azure klasické později.

7. Klikněte na **OK**.

10. V **ověřování / se tak mohli ověřovat** zásuvné, klikněte na tlačítko **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Aplikace služby vytvoří aplikace služby Azure Active Directory s **adresou URL přihlašování** a **Odpovědět URL** automaticky nastavená na adresa URL aplikace rozhraní API. Tato hodnota umožňuje uživatelům ve vašem klientovi AAD k přihlášení a přístup k rozhraní API aplikace.

### <a name="verify-that-the-api-app-is-protected"></a>Ověřte, že se po zamknutí rozhraní API aplikace

1. V prohlížeči přejděte na adresu URL rozhraní API aplikace: v zásuvné **rozhraní API aplikace** na portálu Azure, klikněte na odkaz v části **Adresa URL**. 

    Budete přesměrováni na přihlašovací obrazovce, protože neověřené požadavky nejsou povolené kontaktovat rozhraní API aplikace. 

    Pokud váš prohlížeč přejít do rozhraní Swagger, prohlížeče může být připojen – v takovém případě otevřete okno InPrivate nebo okno a přejděte na adresu URL Swagger uživatelského rozhraní.

18. Přihlaste se pomocí přihlašovací údaje uživatele AAD klienta.

    Pokud jste přihlášeni, "úspěšně vytvořené" stránce zobrazená v prohlížeči.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurace ToDoListAPI projektu a získat odeslat token Azure AD

V této části můžete provádět následující úkoly:

* Přidáte kód v aplikaci rozhraní API střední vrstvy, který používá pověření aplikací Azure AD k získání tokenu a odeslat brožuru s žádostí o HTTP rozhraní API aplikace osy dat.
* Přihlašovací údaje, které bude nutné získáte z Azure AD.
* Zadejte přihlašovací údaje do nastavení prostředí runtime služby Azure aplikace v prostředním osy rozhraní API aplikace. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurace ToDoListAPI projektu a získat odeslat token Azure AD

V aplikaci project ToDoListAPI ve Visual Studiu proveďte následující změny.

1. Zrušte komentář kód v souboru *ServicePrincipal.cs* .

    Toto je kód, který používá ADAL pro .NET získat nosný token Azure AD.  Použití několika konfigurační hodnoty, které vytvoříte v prostředí Azure runtime později. Tady je kód: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Poznámka:** Tento kód vyžaduje ADAL .NET NuGet balíčku (Microsoft.IdentityModel.Clients.ActiveDirectory), která je nainstalovaná v projektu. Při vytváření tohoto projektu od začátku, je třeba nainstalovat tento balíček. Tento balíček nenainstaluje automaticky šablonou rozhraní API aplikace nový projekt.

2. V *Zařízení/ToDoListController*zrušte komentář kód v `NewDataAPIClient` metodu, která přidá tokenu HTTP žádosti v záhlaví se tak mohli ověřovat.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Nasazení ToDoListAPI projektu. (Klikněte pravým tlačítkem projekt a potom klikněte na **Publikovat > Publikovat**.)

    Visual Studio nasazuje projektu a otevře prohlížeč základní adresu URL webové aplikace. Tím se zobrazí 403 chybovou stránku, která je normální při pokusu o přejít na rozhraní API webových základní adresu URL z prohlížeče.

4. Zavřete prohlížeč.

### <a name="get-azure-ad-configuration-values"></a>Získání Azure AD konfigurace hodnot

11. V [Azure klasické portál](https://manage.windowsazure.com/)přejděte na **Azure Active Directory**.

12. Na kartě **adresář** klikněte na AAD klienta.

14. Klikněte na **aplikace > aplikace naší společnosti vlastní**a klikněte na značku zaškrtnutí.

15. V seznamu aplikací klepněte na název ten, který Azure, vytvoří se až povoleno ověřování rozhraní API aplikace ToDoListDataAPI (osy dat).

16. Klikněte na kartu **Konfigurovat** .

5. Zkopírujte hodnotu **ID klienta** a uložte jej někam, kterou si můžete jej později. 

8. Na portálu Azure klasické přejděte zpět do seznamu **aplikací vlastní naší společnosti**a klikněte na aplikaci AAD, který jste vytvořili ToDoListAPI rozhraní API aplikace střední úroveň (tu, kterou jste vytvořili v předchozím kurzu, není tu, kterou jste vytvořili v tomto kurzu).

16. Klikněte na kartu **Konfigurovat** .

5. Zkopírujte hodnotu **ID klienta** a uložte jej někam, kterou si můžete jej později.

6. **Klíče**vyberte z rozevíracího seznamu **Vyberte doba trvání** **1 rok** .

6. Klikněte na **Uložit**.

    ![Generování aplikace klíče](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Zkopírujte hodnoty klíče a uložte jej někam, kterou si můžete z později.

    ![Zkopírujte nový klíč aplikace](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Konfigurace nastavení služby Azure AD v prostředí runtime střední úroveň rozhraní API aplikace

1. Přejděte na [portál Azure](https://portal.azure.com/)a potom přejděte na zásuvné **Rozhraní API aplikace** rozhraní API aplikace, který je hostitelem TodoListAPI (střední vrstvy) projektu.

2. Klikněte na **Nastavení > Nastavení aplikace**.

3. V části **Nastavení aplikace** přidejte následující klíče a hodnoty:

  	| **Klíč** | IDA: autorita |
  	|---|---|
  	| **Hodnota** | https://Login.microsoftonline.com/ {vaše jméno klienta Azure AD} |
  	| **Příklad** | https://Login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Klíč** | IDA: ClientId |
  	|---|---|
  	| **Hodnota** | ID klienta, volající aplikace (střední vrstvy - ToDoListAPI) |
  	| **Příklad** | 960adec2-b74a-484A-960adec2-b74a-484A |

  	| **Klíč** | IDA: ClientSecret |
  	|---|---|
  	| **Hodnota** | Klíč aplikace volání aplikace (střední vrstvy - ToDoListAPI) |
  	| **Příklad** | e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |

  	| **Klíč** | IDA: zdrojů |
  	|---|---|
  	| **Hodnota** | ID klienta se nazývá aplikace (osy dat – ToDoListDataAPI) |
  	| **Příklad** | e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |

    **Poznámka**: pro `ida:Resource`, ujistěte se, používáte se nazývá aplikace **ID klienta** a ne jeho **URI ID aplikace**.

    `ida:ClientId`a `ida:Resource` odlišné hodnoty pro účely tohoto návodu vzhledem k tomu, že používáte oddělit applicaations Azure AD pro střední vrstvy a data osy. Pokud jste používali jediné aplikaci Azure AD pro volající rozhraní API aplikace a chráněné rozhraní API aplikace použijete stejné hodnoty v obou `ida:ClientId` a `ida:Resource`.

    Kód používá ConfigurationManager zobrazíte tyto hodnoty, může být uložena v projektu nastavení(Web.config)) nebo zápisy v prostředí Azure runtime. Spuštěná aplikace ASP.NET v aplikaci služby Azure nastavení prostředí automaticky přepíše nastavení z Web.config. Nastavení prostředí jsou obvykle [bezpečnější způsob, jak ukládat citlivé informace ve srovnání s soubor Web.config](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Testování aplikace

1. V prohlížeči přejděte na adresu URL HTTPS AngularJS front-end web appu.

2. Klikněte na kartu **Se seznamem úkolů** a protokolu pomocí přihlašovacích údajů pro uživatele ve vašem klientovi Azure AD. 

4. Přidání položek úkolů můžete ověřit, že aplikace pracuje.

    ![Seznam stránek](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Pokud se aplikace nefunguje očekávaným způsobem, zkontrolujte všechny možnosti, které jste zadali do portálu Azure. Pokud se všechna nastavení zobrazují správně, naleznete v části [Poradce při potížích](#troubleshooting) dál v tomto kurzu.

## <a name="protect-the-api-app-from-browser-access"></a>Zamknout aplikaci rozhraní API z přístup pomocí prohlížeče

Tento kurz, který jste vytvořili samostatný Azure AD žádosti o aplikaci rozhraní API ToDoListDataAPI (osy dat). Jak ukázali jsme si, když aplikace služby vytvoří aplikace AAD, nakonfiguruje aplikaci AAD způsobem, který umožňuje uživateli přejděte na adresu URL rozhraní API aplikace v prohlížeči a přihlaste se. To znamená, že je možné koncový uživatel Azure AD klienta, nikoli pouze služby základní, přístup k rozhraní API. 

Pokud chcete zabránit přístup pomocí prohlížeče bez psaní kódu v aplikaci chráněné rozhraní API, můžete změnit adresu **URL odpovědět** v aplikaci AAD tak, aby se liší od základní adresu URL rozhraní API aplikace. 

### <a name="disable-browser-access"></a>Zakázání přístup pomocí prohlížeče

1. V klasickém portálu **Konfigurovat** ouško AAD aplikace, která byla vytvořená pro TodoListService změňte hodnotu do pole **Adresa URL odpověď** tak, aby byla platná adresa URL, ale ne URL rozhraní API aplikace.
 
2. Klikněte na **Uložit**.

### <a name="verify-browser-access-no-longer-works"></a>Ověřte, jestli už funguje přístup pomocí prohlížeče

Dříve ověřit, že můžete přejít na adresu URL rozhraní API aplikace z prohlížeče po přihlášení pomocí přihlašovacích údajů pro jednotlivé uživatele. V této části ověřte, zda je už není možné. 

1. V novém okně prohlížeče přejděte na adresu URL rozhraní API aplikace znovu.

2. Přihlaste se po zobrazení výzvy k tomu nevyzve.

3. Přihlášení se povede, ale vede k chybová stránka.

    Aplikaci AAD máte nastavený tak, aby uživatelé v klientovi AAD nemůžete přihlásit a přístup k rozhraní API z prohlížeče. Můžete pořád přístup k rozhraní API aplikace pomocí služby základní tokenu kterého můžete ověřit, že přejdete na adresu URL webové aplikace a přidávání dalších položek úkolů.

## <a name="restrict-access-to-a-particular-service-principal"></a>Omezení přístupu k určité službě jistina  

Přímo, volající, který dostane token pro uživatele nebo jistinu služby Azure AD klienta upoutat aplikaci rozhraní API TodoListDataAPI (osy dat). Můžete chtít Ujistěte se, že aplikace rozhraní API osy dat pouze přijme volání v aplikaci TodoListAPI (střední vrstvy) rozhraní API a jenom z konkrétní službu jistina. 

Tato omezení můžete přidat přidáním kódu ověřuje `appid` a `objectidentifier` nároky na příchozí volání.

Pro účely tohoto návodu vložit kód, který ověří ID aplikace a služby základní ID přímo v řadiče akcí.  Alternativy jsou používat vlastní `Authorize` atribut nebo dělat toto ověření spouštěcí řady (například OWIN middleware). Příklad ten naleznete v tématu [Tato ukázková aplikace](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Proveďte následující změny TodoListDataAPI projektu.

2. Otevřete soubor *Controllers/TodoListController.cs* .

3. Zrušte komentář řádky, které nastavil `trustedCallerClientId` a `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Zrušte komentář kód v metodu CheckCallerId. Tento způsob je místo toho na začátku každé metody akce v zařízení. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Přeinstalujte ToDoListDataAPI projekt aplikace služby Azure.

6. V prohlížeči přejděte na adresu URL HTTPS AngularJS front-end web appu na a na domovské stránce klikněte na kartu **Se seznamem úkolů** .

    Aplikace nefunguje, protože se nedaří volání do back-end. Nový kód kontroluje skutečné ID aplikace a objectidentifier, ale ještě nemá správné hodnoty zkontrolovat je proti. Prohlížeč vývojář nástroje konzoly hlásí, že server vrací chybu HTTP 401.

    ![Chyba při vývojář nástroje konzoly](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    V následujících krocích je nakonfigurovat očekávané hodnoty.

8. Pomocí prostředí PowerShell Azure AD získali hodnotu jistiny služby Azure AD aplikace, která jste vytvořili TodoListWebApp projektu.

    na. Další informace o tom, jak nainstalovat Azure PowerShell a připojit se ke svému předplatnému najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).

    b. Zobrazte seznam objektů služby, spustit `Login-AzureRmAccount` příkaz a potom `Get-AzureRmADServicePrincipal` příkaz.

    c. Najděte objektu pro službu jistiny TodoListAPI aplikace a uložte ho do umístění můžete zkopírovat z později.

7. Na portálu Azure přejděte na zásuvné aplikace rozhraní API pro rozhraní API aplikace, které nasazené ToDoListDataAPI projektu.

9. Klikněte na **Nastavení > Nastavení aplikace**.

3. V části **Nastavení aplikace** přidejte následující klíče a hodnoty:

  	| **Klíč** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Hodnota** | Volání aplikace služby základní id |
  	| **Příklad** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Klíč** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Hodnota** | ID klienta volání aplikace – zkopírovali z aplikace TodoListAPI Azure AD |
  	| **Příklad** | 960adec2-b74a-484A-960adec2-b74a-484A |

6. Klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. V prohlížeči vraťte se adresu URL webové aplikace a na domovské stránce klikněte na kartu **Se seznamem úkolů** .

    Tentokrát aplikace funguje očekávaným způsobem, protože aplikace důvěryhodných volající ID a služby základní jsou očekávané hodnoty.

    ![Seznam stránek](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Vytváření projektů úplně od začátku

Dva projekty rozhraní API webových vytvořená pomocí šablony **Azure rozhraní API aplikace** project a nahrazení výchozího hodnoty řadiče řadiči seznam úkolů. Získání Azure AD služby základní tokeny v aplikaci project ToDoListAPI, byl nainstalovaný balíček NuGet [Active Directory Authentication knihovny (ADAL) pro .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) .
 
Informace o tom, jak vytvořit aplikaci jednostránkové AngularJS s rozhraní API webových zadní jako ToDoListAngular najdete v tématu [Hands v laboratoři: sestavení jedné stránce aplikace (zabezpečené ověřování HESLA) pomocí rozhraní API webových ASP.NET a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Další informace o tom, jak přidat Azure AD ověřovací kód najdete v článku [Zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Řešení potíží

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nezaměňujte ToDoListAPI (střední vrstvy) a ToDoListDataAPI (osy dat). Například v tomto kurzu přidáte ověřování aplikace rozhraní API osy dat, **ale klávesu aplikace musí pocházet z Azure AD aplikace, která jste vytvořili střední úroveň rozhraní API aplikace**.

## <a name="next-steps"></a>Další kroky

Toto je poslední kurz v řadě rozhraní API aplikace. 

Další informace o Azure Active Directory najdete v následujících zdrojích.

* [Příručka Azure AD vývojáři](http://aka.ms/aaddev)
* [Azure AD scénáře](http://aka.ms/aadscenarios)
* [Azure AD vzorky](http://aka.ms/aadsamples)

    [Web Appu – WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) vzorek tvoří podobně jako obsah zobrazený v tomto kurzu, ale bez použití aplikace služby ověřování.

Informace o jiných způsobech do kterých se nasadí projekty Visual Studio rozhraní API aplikace Visual Studio pomocí nebo [Automatizace nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) [systému správy zdrojů](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)najdete v článku [jak nasadit aplikaci pro aplikaci služby Azure](../app-service-web/web-sites-deploy.md).
