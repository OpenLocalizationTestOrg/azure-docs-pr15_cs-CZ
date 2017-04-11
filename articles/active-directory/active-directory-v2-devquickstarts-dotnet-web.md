<properties
    pageTitle="Azure AD verze 2.0 .NET Web Appu | Microsoft Azure"
    description="Pracovní nebo školní účty a k vytváření .NET MVC Web App a přihlásí uživatelů pomocí obou osobní Account společnosti Microsoft."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Přidání přihlášení do aplikace pro web .NET MVC

Koncový bod verze 2.0 rychle přidat ověřování ke svým aplikacím web s podporou obou osobních účtů Microsoft a pracovní nebo školní účty.  Ve webových aplikacích ASP.NET lze provést pomocí součástí .NET Framework 4.5 middleware OWIN společnosti Microsoft.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

 Tady jsme budete vytvořit webovou aplikaci používající OWIN a po přihlášení uživatele, zobrazení některé informace o uživateli podepsat uživatele mimo aplikaci.
 
 ## <a name="download"></a>Ke stažení
Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo klonovat kostra:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Dokončené aplikace je k dispozici na konci kurzu, stejně.

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Kopírovat dolů **Id aplikace** přiřazené k aplikaci, musíte ho brzy bude k dispozici.
- Přidání platformu **webové** aplikace.
- Zadejte správné **Přesměrovat URI**. Identifikátor uri přesměrování označuje Azure AD místo, kam mají být směrovány ověřování odpovědi – je jako výchozí pro tento kurz `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Instalace a konfigurace OWIN ověřování
Tady jsme budete konfigurovat middleware OWIN použít ověřování protokolem OpenID připojení.  OWIN se použijí k problému žádosti o přihlášení a odhlašovací, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

-   Chcete-li začít, otevřete `web.config` soubor v kořenovém projektu a zadejte hodnoty konfigurace vaše aplikace v `<appSettings>` oddíl.
    -   `ida:ClientId` Je **Id aplikace** přiřazená vaši aplikaci distribuovali portálu registrace.
    -   `ida:RedirectUri` Je **Přesměrovat Uri** jste zadali v portálu.

-   Dále přidejte balíčků OWIN middleware NuGet projektu pomocí konzole balíčku správce.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Přidání "OWIN spuštění třídu" projektu s názvem `Startup.cs` pravý tlačítkem myši na projektu--> **Přidat** --> vyhledejte "OWIN"-->**Nová položka** .  OWIN middleware vyvolá `Configuration(...)` metoda při spuštění aplikace.
-   Změna deklaraci třídy na `public partial class Startup` -jsme už implementovali součástí tohoto třídy za vás v jiném souboru.  V `Configuration(...)` metody, používat volání ConfigureAuth(...) nastavení ověřování pro webovou aplikaci  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Otevřete soubor `App_Start\Startup.Auth.cs` a implementace `ConfigureAuth(...)` metody.  Parametry zadáte v `OpenIdConnectAuthenticationOptions` bude sloužit jako souřadnice aplikaci komunikovat s Azure AD.  Budete taky muset nastavení ověření souborů Cookie: middleware OpenID připojení používá soubory cookie pod titulní stránky.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Odeslání žádosti o ověřování
Aplikace je teď správně nakonfigurované komunikovat s koncový bod verze 2.0 ověřování protokolem OpenID připojení.  OWIN má stará všech dobře podrobnosti věnujte zprávy o ověřování, ověřování tokenů z Azure AD a udržování relace uživatele.  Zbývá dát uživatelům umožňuje přihlášení a odhlášení.

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

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Teď, otevřete `Views\Shared\_LoginPartial.cshtml`.  Toto je místo, kam budete se uživateli zobrazí vaše aplikace přihlašovací a odhlašovací odkazy a vytiskněte uživatelské jméno v zobrazení.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
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

## <a name="display-user-information"></a>Zobrazit informace o uživatelích
Při ověřování uživatelů s OpenID připojení koncového bodu verze 2.0 vrátí id_token do aplikace, která obsahuje [nároky](active-directory-v2-tokens.md#id_tokens)nebo tvrzení o uživateli.  K přizpůsobení aplikace můžete použít tyto požadavky:

- Otevřít `Controllers\HomeController.cs` soubor.  Můžete přistupovat k uživatele deklarací řadiči prostřednictvím `ClaimsPrincipal.Current` objekt zabezpečení.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Spuštění

Nakonec vytvoření a spuštění aplikace.   Přihlášení pomocí osobního Account Microsoft nebo pracovního nebo školního účtu a Všimněte si, jak se projeví identita uživatele v horním navigačním panelu.  Nyní máte web app zabezpečený pomocí standardních protokolů, které můžete ověřování uživatelů s jejich osobní a pracovní/školní účty.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako .zip tady](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Další kroky

Teď můžete přesunout na další upřesňující témata.  Můžete zkusit:

[Zabezpečené rozhraní API webových s koncový bod verze 2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Další zdroje informací najdete v tématech:
- [Průvodce pro vývojáře verze 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Značka StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
