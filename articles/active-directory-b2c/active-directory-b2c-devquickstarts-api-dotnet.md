<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Postup vytvoření rozhraní API webových .NET pomocí Azure Active Directory B2C, zabezpečené ověřování pomocí tokeny aplikace access OAuth 2.0."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Sestavení .NET webového rozhraní API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S B2C Azure Active Directory (Azure AD) můžete zabezpečení webového rozhraní API pomocí tokeny aplikace access OAuth 2.0. Těchto tokenů povolit klientské aplikace, které používají Azure AD B2C ověření rozhraní API. Tento článek popisuje, jak vytvořit API "seznam úkolů".NET modelu zobrazení řadiče MVC (), které umožňuje uživatelům CRUD úkoly. Na webu rozhraní API je zabezpečená pomocí Azure AD B2C a umožňuje pouze ověřené uživatele ke správě seznamu úkolů.

## <a name="create-an-azure-ad-b2c-directory"></a>Vytvoření Azure AD B2C adresáře

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient. Adresář je kontejner pro všechny uživatele aplikace, skupiny a další. Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat v této příručce.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikaci pro v adresáři vašeho B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Nezapomeňte:

- Připojte k **web appu** nebo **webového rozhraní API** aplikace.
- Použití **přesměrovat identifikátor URI** `https://localhost:44316/` pro web app. Toto je výchozí umístění webový klient aplikace pro tato ukázka kódu.
- Zkopírujte **ID aplikace** přiřazená aplikace. Je potřeba ho později.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Klient v tomto příkladu obsahuje tři identity prostředí: registrace, přihlaste se a upravit profil. Musíte se k vytvoření zásad pro každý typ způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Při vytváření tři zásady nezapomeňte:

- Zvolte **Uživatelské ID registrace** nebo **e-mailu registrace** zásuvné poskytovatelů identit.
- Zvolte **zobrazované jméno** a dalších registrace atributů registrace zásad.
- Klikněte na **zobrazované jméno** a **ID objektu** deklarované jako deklarace aplikace pro každého zásadu. Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. Musíte mít tyto zásady názvy později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po úspěšném vytvoření tři zásad, budete chtít vytvořit aplikace.

## <a name="download-the-code"></a>Stáhněte si tento kód

Kód pro tento kurz [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Dokončené aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) nebo `complete` větev stejném úložišti.

Po stažení ukázkový kód otevřete soubor SLN Visual Studio můžete začít. Řešení soubor obsahuje dva projekty: `TaskWebApp` a `TaskService`. `TaskWebApp`jsou webové aplikace MVC, interakci uživatele s. `TaskService`je v aplikaci back-end webového rozhraní API obsahujícího seznam úkolů každého uživatele.

## <a name="configure-the-task-web-app"></a>Konfigurace úkolu web appu

Při interakci uživatele s `TaskWebApp`, klienta odesílá žádosti Azure AD a získává zpět klíčovými slovy, která mohou sloužit k volání `TaskService` webového rozhraní API. Přihlaste se uživatele a získat tokenů, je třeba zadat `TaskWebApp` některé informace o aplikaci. V `TaskWebApp` otevřeného projekt `web.config` soubor v kořenovém projektu a nahrazení hodnot v `<appSettings>` oddíl.  Můžete ponechat `AadInstance`, `RedirectUri`, a `TaskServiceUrl` hodnoty jako-je.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Tento článek se nezabývá stavební `TaskWebApp` klienta.  Naučte se vytvářet webové aplikace pomocí Azure AD B2C, najdete v článku [naší kurz .NET webové aplikace](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Zabezpečené rozhraní API

Když budete mít klienta, který volá rozhraní API jménem uživatele, můžete zabezpečit `TaskService` pomocí tokeny nosný OAuth 2.0. Vaše rozhraní API můžete přijmout a ověřte tokeny pomocí rozhraní otevřít Web společnosti Microsoft pro knihovnu .NET (OWIN).

### <a name="install-owin"></a>Instalace OWIN
Začněte instalací OWIN OAuth ověřování kanálu:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Zadejte podrobnosti o B2C
Otevřít `web.config` soubor v kořenovém adresáři `TaskService` project a nahrazení hodnot v `<appSettings>` oddíl. Tyto hodnoty se používají v knihovně rozhraní API a OWIN.  Můžete ponechat `AadInstance` hodnotu beze změny.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Přidání třídy spuštění OWIN
Přidání třídu OWIN spuštění k `TaskService` projektu s názvem `Startup.cs`.  Klikněte pravým tlačítkem myši na projektu, vyberte **Přidat** a **Nová položka**a vyhledejte OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurace ověřování OAuth 2.0
Otevřete soubor `App_Start\Startup.Auth.cs`a implementace `ConfigureAuth(...)` metodu:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Zabezpečené Správce úloh
Po konfiguraci aplikace ověřování OAuth 2.0 můžete zabezpečeného webového rozhraní API přidáním `[Authorize]` značku Správce úloh. Toto je řadiče certifikovaného místo pro manipulaci s seznam všech úkolů, je třeba zabezpečit celý řadiče na úrovni třídy. Můžete také přidat `[Authorize]` značky na jednotlivé akce pro podrobnější řízení.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Získat informace o uživateli z tokenu
`TasksController`v databázi, kde každý úkol má přidružená uživatele, který "vlastní" úkol: ukládá úkoly. Vlastník je určen uživatelské **ID objektu**. (Toto je důvod, proč byste potřebovali přidáte ID objektu jako aplikace deklaraci identity v celém zásadami.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec, vytvoření a spuštění obou `TaskWebApp` a `TaskService`. Zaregistrujte si aplikaci pomocí-mailovou adresu nebo uživatelské jméno. Vytvořit některé úkoly uživatele ze seznamu úkolů a Všimněte si, jak jsou trvalé v rozhraní API i po ukončení a opětovném spuštění klienta.

## <a name="edit-your-policies"></a>Úprava vaší zásad

Po zajištěna rozhraní API pomocí Azure AD B2C můžete experimentovat s zásady vaše aplikace a zobrazit efekty (nebo chybí z nich odvozená) na rozhraní API. Můžete pracovat s deklarace aplikace v zásady a změňte uživatelské informace, které jsou k dispozici v webového rozhraní API. Nároky, které přidáte budou k dispozici .NET MVC webového rozhraní API v `ClaimsPrincipal` objektu, jak je popsáno dříve v tomto článku.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
