<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Vytvoření webové aplikace, která obsahuje registrace, přihlásit a pomocí Azure Active Directory B2C resetování hesla."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C: Registrace & přihlašovací v aplikaci technologie ASP.NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Pomocí B2C Azure Active Directory (Azure AD) můžete přidat výkonné identity samoobslužné funkce správy do webové aplikace v krátkém chvilku. Tento článek popisuje jak vytvořit webovou aplikaci ASP.NET obsahující uživatele registrace, přihlásit a resetování hesel. Aplikace se zahrnují podporu registrace přihlašování a pomocí uživatelské jméno nebo e-mailu které využívají účty sociálních třeba Facebook a Google.

Tento kurz se liší od [naše jiné .NET základy práce na webu](active-directory-b2c-devquickstarts-web-dotnet.md) , v tom, že používá k [registraci nebo přihlášení zásad](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) k poskytnutí registrace uživatele a přihlášení pomocí jedné tlačítka místo dvě (jedno pro registrace a jeden pro přihlášení).  V nutshell přihlášení nahoru nebo přihlásit zásad umožňuje přihlášení k aplikaci existujícího účtu by mít jednu nebo vytvořte nový účet, pokud je jejich prvním pomocí aplikace.

## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient. Adresář je kontejner pro všechny uživatele aplikace, skupiny a další.  Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat v této příručce.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikace ve vašem adresáři B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).  Nezapomeňte:

- Zahrňte **webové aplikace/webového rozhraní API** aplikace.
- Zadejte `https://localhost:44316/` jako **přesměrovat URI**. Je výchozí adresu URL Tato ukázka kódu.
- Zkopírujte dolů **ID aplikace** přiřazená aplikace.  Budete ho později potřebovat.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tento příklad kód obsahuje dvěma prostředími identit: **Registrace & přihlašovací**a **Resetování hesel**.  Je potřeba vytvořit jednu zásadu každého typu způsobem popsaným v [článku Přehled zásad](active-directory-b2c-reference-policies.md). Když vytvoříte dva zásad, nezapomeňte:

- Zvolte **Uživatelské ID registrace** nebo **e-mailu registrace** zásuvné poskytovatelů identit.
- Zvolte **zobrazované jméno** a dalších registrace atributů registrace & přihlašovací zásad.
- Zvolte deklaraci **zobrazované jméno** jako aplikace deklaraci identity v každé zásady. Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. Je potřeba názvy zásad později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření dvě zásady budete připraveni k vytvoření aplikace.

## <a name="download-the-code-and-configure-authentication"></a>Stáhněte si tento kód a konfigurace ověřování

Kód pro tento ukázkový [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Hotový ukázkový je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) nebo `complete` větev stejném úložišti.

Po stažení ukázkový kód otevřete soubor SLN Visual Studio můžete začít.

Aplikace informuje uživatele o s Azure AD B2C tak, že požadavky na ověření HTTP, které určují zásad, který chce provést jako součást žádost. Knihovna OWIN společnosti Microsoft .NET webových aplikací slouží k odeslat OpenID připojení požadavky ověřování, spustit zásad, spravovat relace uživatelů a další.

Chcete-li začít přidejte do projektu pomocí konzole Visual Studio balíčku Správce balíčků OWIN middleware NuGet.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Potom otevřete `web.config` souborů v kořenovém projektu a zadejte hodnoty konfigurace vaše aplikace v `<appSettings>` oddíl nahrazení hodnot menších než vlastní.  Můžete ponechat `ida:RedirectUri` a `ida:AadInstance` hodnoty, beze změny.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Dále přidejte třídy spuštění OWIN projektu s názvem `Startup.cs`. Klikněte pravým tlačítkem myši na projektu, vyberte **Přidat** a **Nová položka**a vyhledejte "OWIN." Změna deklaraci třídy na `public partial class Startup`. Součást tohoto třídy jsme implementovaná za vás v jiném souboru. OWIN middleware vyvolá `Configuration(...)` metoda při spuštění aplikace. Tato metoda zavolání na `ConfigureAuth(...)`, kde můžete určit ověřování aplikace.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(...)` metody.  Parametry zadáte v `OpenIdConnectAuthenticationOptions` sloužit jako souřadnice aplikaci komunikovat s Azure AD. Budete potřebovat nastavit ověření souborů cookie. Middleware OpenID připojení používá ke správě uživatelských relací, mimo jiné soubory cookie.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Odeslání žádosti o ověřování Azure AD
Aplikace je teď správně nakonfigurované komunikovat s Azure AD B2C pomocí ověřování protokolem OpenID připojení.  OWIN má stará všechny podrobnosti o vytvoření zprávy o ověřování, ověřování tokenů z Azure AD a udržování relace uživatele.  Zbývá zahajte toku každého uživatele.

Když uživatel vybere **přihlášení** nebo **Zapomněli jste heslo?** ve web appu akce přidružené vyvolání v `Controllers\AccountController.cs`. V obou případech můžete použít předdefinované metody OWIN aktivovat správné zásad:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Během provádění registrace nebo přihlášení zásad, klikněte na možnost má uživatel **Zapomněli jste heslo?** odkaz.  V tomto případě se Azure AD B2C odeslat aplikace pro konkrétní chybová zpráva, že ji spustit heslo resetovat zásad.  Zachycení tuto chybu v `Startup.Auth.cs` pomocí `AuthenticationFailed` upozornění:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Kromě explicitně vyvolání zásadu, můžete pomocí `[Authorize]` značku v seznamu svého zařízení, které se provedou zásadu, pokud není přihlášení uživatele. Otevřít `Controllers\HomeController.cs` a přidejte `[Authorize]` značku řadiče deklarované.  OWIN vybereme poslední nakonfigurované zásady, kdy `[Authorize]` přístupů značku.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Můžete taky OWIN uživatele z aplikace odhlásit. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Zobrazit informace o uživatelích
Při ověřování uživatelů pomocí připojení OpenID Azure AD vrátí ID token aplikace, která obsahuje **deklarované**. Toto jsou tvrzení o uživateli. Nároky slouží k přizpůsobení aplikace.  

Otevřít `Controllers\HomeController.cs` soubor. Můžete přistupovat k uživatele deklarací řadiči prostřednictvím `ClaimsPrincipal.Current` objekt zabezpečení.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Přístup k deklarace, které aplikace přijímá stejným způsobem.  Seznam všech deklarací přijaté aplikace je k dispozici pro vás na stránce **deklarace** .

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec můžete vytvořit a spustit aplikaci. Zaregistrujte si aplikaci pomocí e-mailovou adresu nebo uživatelské jméno. Odhlaste se a přihlaste se zpět stejným uživatelským. Upravte profil uživatele. Odhlaste se a zaregistrovat jako jiného uživatele. Všimněte si, že informace zobrazené na kartě **deklarací** odpovídá informace, které jste nakonfigurovali ve vaší zásady.

## <a name="add-social-idps"></a>Přidání sociální IDPs

Aplikaci v současné době podporuje pouze uživatel registrace přihlašování a pomocí **místních účtů**. Toto jsou uložené v adresáři B2C účty, které používají uživatelské jméno a heslo. Pomocí Azure AD B2C můžete přidat podpory pro ostatní **Zprostředkovatelé identit jiní** (IDPs) beze změny jednotlivých kódu.

Chcete aplikaci přidat sociální IDPs, začněte podrobných pokynů v těchto článcích. Pro každý IDP, který chcete podporu budete muset registraci aplikace v režimu a získat ID klienta.

- [Nastavení služby Facebook jako IDP](active-directory-b2c-setup-fb-app.md)
- [Nastavení Google jako IDP](active-directory-b2c-setup-goog-app.md)
- [Nastavení Amazon jako IDP](active-directory-b2c-setup-amzn-app.md)
- [Nastavení LinkedIn jako IDP](active-directory-b2c-setup-li-app.md)

Po přidání Zprostředkovatelé identit jiní do adresáře B2C potřebujete upravit každou tři zásad zahrnout nové IDPs způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md). Po uložení vaše zásady znova spusťte aplikaci.  Měla by se zobrazit nová IDPs přidá ve formě přihlašovací a dojde k registrace možnosti v žádném z vaší identity.

Můžete experimentovat se zásadami a sledovat vliv na aplikaci vzorku. Přidání nebo odebrání IDPs, pracovat s deklarace aplikace nebo změnit atributy registrace. Vyzkoušejte až najdete v článku jak shromáždit zásad, požadavky ověřování a OWIN.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Ho můžete zkopírovat z GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
