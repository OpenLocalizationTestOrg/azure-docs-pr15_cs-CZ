<properties
    pageTitle="Azure AD .NET Začínáme | Microsoft Azure"
    description="Jak vytvořit Web App MVC .NET, který lze integrovat s Azure AD pro přihlášení."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>Technologie ASP.NET přihlašovací aplikace a odhlásit se Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD díky rychle a jednoduše využívající Správa identit webovou aplikaci, poskytuje jediné přihlašovací a odhlašovací s jenom několik řádků kódu.  Ve webových aplikacích Asp.NET lze provést pomocí společnosti Microsoft provádění součástí .NET Framework 4.5 middleware OWIN komunity řízené úsilím.  Tady použijeme OWIN na:
-   Uživatel se přihlaste do aplikace používají Azure AD zprostředkovatele identit.
-   Zobrazte některé informace o uživateli.
-   Nejdřív se př uživatele mimo aplikaci.

K tomuto účelu budete muset:

1. Registrace aplikace s Azure AD
2. Nastavení aplikace na používání OWIN ověřování kanálu.
3. Žádosti o přihlášení a odhlašovací vydat Azure AD pomocí OWIN.
4. Vytiskněte dat o uživateli.

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Taky musíte tenanta Azure AD ve kterých se registruje aplikace.  Pokud žádný nemáte už, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. registrace aplikace s Azure AD*
Povolení aplikace pro ověřování uživatelů, Nejdřív musíte zaregistrovat nové aplikace ve vašem klientovi.

- Přihlaste se k portálu Správa Azure.
- V levém navigačním podokně klikněte na **Služby Active Directory**.
- Vyberte klienta, kde se chcete zaregistrovat aplikace.
- Klikněte na kartu **aplikace** a klikněte na Přidat v dolní okraj.
- Postupujte podle pokynů a vytvořte nové **webové aplikace a/nebo WebAPI**.
    - **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přihlašovací adresa URL** je základní adresa URL aplikace.  Osnovu výchozí hodnota je `https://localhost:44320/`.
    - **Identifikátor URI aplikace** je jedinečný identifikátor aplikace.  Názvů, je použít `https://<tenant-domain>/<app-name>`, například`https://contoso.onmicrosoft.com/my-first-aad-app`
- Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě Konfigurovat.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. nastavení aplikace pro použití kanálu OWIN ověřování*
Tady jsme budete konfigurovat middleware OWIN použít ověřování protokolem OpenID připojení.  OWIN se použijí k problému žádosti o přihlášení a odhlašovací, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

-   Chcete-li začít přidejte do projektu pomocí Správce balíčků konzoly balíčků OWIN middleware NuGet.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Přidání třídu OWIN spuštění projektu s názvem `Startup.cs` pravý tlačítkem myši na projektu--> **Přidat** --> vyhledejte "OWIN"-->**Nová položka** .  OWIN middleware vyvolá `Configuration(...)` metoda při spuštění aplikace.
-   Změna deklaraci třídy na `public partial class Startup` -jsme už implementovali součástí tohoto třídy za vás v jiném souboru.  V `Configuration(...)` metody, používat volání ConfgureAuth(...) nastavení ověřování pro webovou aplikaci  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otevřete soubor `App_Start\Startup.Auth.cs` a implementace `ConfigureAuth(...)` metody.  Parametry zadáte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice aplikaci komunikovat s Azure AD.  Budete taky muset nastavení ověření souborů Cookie: middleware OpenID připojení používá soubory cookie pod titulní stránky.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Nakonec otevřete `web.config` souborů v kořenovém projektu a zadejte hodnoty konfigurace v `<appSettings>` oddíl.
    -   Vaše aplikace `ida:ClientId` je Guid jste zkopírovali z portálu Microsoft Azure v kroku 1.
    -   `ida:Tenant` Je název vašeho Azure AD klienta, například "contoso.onmicrosoft.com".
    -   Vaše `ida:PostLogoutRedirectUri` označuje Azure AD místo, kam by měl uživatel přesměrován po úspěšném dokončení odhlašovací žádosti.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. pomocí OWIN žádosti o přihlášení a odhlašovací vydat Azure AD*
Aplikace je teď správně nakonfigurované komunikovat s Azure AD pomocí ověřování protokolem OpenID připojení.  OWIN má stará všech dobře podrobnosti věnujte zprávy o ověřování, ověřování tokenů z Azure AD a udržování relace uživatele.  Zbývá dát uživatelům umožňuje přihlášení a odhlášení.

- Můžete povolit značky v zařízení vyžadovat tento uživatel přihlásí před přístupem na určitou stránku.  Otevřít `Controllers\HomeController.cs`a přidat `[Authorize]` značku řadiče o.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Můžete taky OWIN přímo vydat požadavky ověřování z vašeho kódu.  Otevřít `Controllers\AccountController.cs`.  V SignIn() a SignOut() akce zadejte ověřovací kód programu OpenID připojení a odhlašovací žádosti o v tomto pořadí.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Teď, otevřete `Views\Shared\_LoginPartial.cshtml`.  Toto je místo, kam budete se uživateli zobrazí vaše aplikace přihlašovací a odhlašovací odkazy a vytiskněte uživatelské jméno v zobrazení.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="4--display-user-information"></a>*4. informace o uživateli zobrazení*
Při ověřování uživatelů s OpenID připojit, vrátí Azure AD id_token aplikace, která obsahuje "nároky" nebo tvrzení o uživateli.  K přizpůsobení aplikace můžete použít tyto požadavky:

- Otevřít `Controllers\HomeController.cs` soubor.  Můžete přistupovat k uživatele deklarací řadiči prostřednictvím `ClaimsPrincipal.Current` objekt zabezpečení.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Nakonec vytvoření a spuštění aplikace.  Pokud jste to ještě neudělali, teď je potřeba vytvořit nového uživatele ve vašem klientovi s *. doménu onmicrosoft.com.  Přihlaste se pomocí tohoto uživatele a Všimněte si, jak se projeví identita uživatele v horním navigačním panelu.  Odhlásit a znovu se přihlaste jako jiným uživatelem ve vašem klientovi.  Pokud jste úplně zejména ambiciózní, zaregistrovat a spusťte jiné instance tohoto aplikace (plus pár vlastní clientId) a jednotného přihlašování programu kukátko najdete v článku akce.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [není uvedený tady](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Teď můžete přesunout na další upřesňující témata.  Můžete zkusit:

[Zabezpečeného webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
