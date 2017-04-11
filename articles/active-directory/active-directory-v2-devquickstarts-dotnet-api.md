<properties
    pageTitle="Azure AD verze 2.0 .NET webového rozhraní API | Microsoft Azure"
    description="Pracovní nebo školní účty a k vytváření .NET MVC rozhraní Api webových, který přijme tokenů z obou osobní Account Microsoft."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Zabezpečené MVC webového rozhraní API

Službou Azure Active Directory, koncový bod verze 2.0 můžete chránit rozhraní API webových používání tokenů aplikace access [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) povolení uživatele s osobní účet Microsoft i pracovní nebo školní účty, na bezpečně přístup k rozhraní API webových.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

Ve webové rozhraní API lze provést pomocí součástí .NET Framework 4.5 middleware OWIN společnosti Microsoft.  Tady můžete vytvořit "Se seznamem úkolů" MVC rozhraní API webových umožňující klientům vytváření a čtení úkolů ze seznamu úkolů uživatele použijeme OWIN.  Webového rozhraní API ověří, že příchozí žádosti obsahují platné přístupový token a odmítnout všechny požadavky, jejichž není ověření bylo úspěšné na zamknutém směrování.  V tomto příkladu byl vytvořené pomocí aplikace Visual Studio 2015.

## <a name="download"></a>Ke stažení
Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Postup, můžete [Stáhnout v aplikaci kostry jako .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo klonovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Kostry aplikace zahrnuje všechny kód často používaný pro jednoduchého rozhraní API, ale chybí všechny použitelné týkající se identity. Pokud nechcete sledovat, můžete místo toho klonovat nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Kopírovat dolů **Id aplikace** přiřazené k aplikaci, musíte ho brzy bude k dispozici.

Tato řešení visual studio obsahuje také "TodoListClient", což je jednoduchý WPF aplikace.  TodoListClient slouží k ukazují, jak uživatel znaménka se změnami a jak klienta můžete vydat žádosti o rozhraní API webových.  V tomto případě TodoListClient i TodoListService představují stejné aplikace.  Abyste mohli nakonfigurovat TodoListClient, měli byste taky:

- Přidání platformu **mobilní** aplikace.


## <a name="install-owin"></a>Instalace OWIN

Zaregistrování jste aplikaci, musíte nastavit aplikace komunikovat s koncový bod verze 2.0 za účelem ověření pravosti příchozí žádosti a tokeny.

- Chcete-li začít otevřete řešení a přidejte balíčků OWIN middleware NuGet TodoListService projektu v konzole balíčku správce.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfigurace ověřování OAuth

- Přidání třídu OWIN spuštění TodoListService projektu s názvem `Startup.cs`.  Pravý tlačítkem myši na projektu--> **Přidat** --> vyhledejte "OWIN"-->**Nová položka** .  OWIN middleware vyvolá `Configuration(…)` metoda při spuštění aplikace.
- Změna deklaraci třídy na `public partial class Startup` -jsme už implementovali součástí tohoto třídy za vás v jiném souboru.  V `Configuration(…)` metodu, používat volání ConfgureAuth(...) nastavení ověřování pro web app.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Otevřete soubor `App_Start\Startup.Auth.cs` a implementace `ConfigureAuth(…)` metodu, která bude nastavení rozhraní API webových přijmout tokenů z verze 2.0 koncového bodu.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Teď můžete použít `[Authorize]` atributy můžete chránit řadiče a akce s ověřováním nosný OAuth 2.0.  Vylepšení vzhledu `Controllers\TodoListController.cs` třídy slovem udělit oprávnění.  To vynutíte uživatel přihlašovat před přístupem na této stránce.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Když oprávněnými volající úspěšně vyvolá jednu z `TodoListController` rozhraní API, akce potřebovat přístup k informacím o volajícím.  OWIN poskytuje přístup k deklarací uvnitř nosný token prostřednictvím `ClaimsPrincpal` objektu.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Nakonec otevřete `web.config` soubor v kořenové složce TodoListService projektu a zadejte hodnoty konfigurace v `<appSettings>` oddíl.
  - Vaše `ida:Audience` je **Id aplikace** aplikace, které jste zadali v portálu.

## <a name="configure-the-client-app"></a>Konfigurace aplikace klienta
Chcete-li zobrazit seznam služby úkol v akci, budete muset nakonfiguruje klient seznamu úkol, můžete získat tokenů z verze 2.0 koncového bodu a volat na služby.

- V projectu TodoListClient otevřete `App.config` a zadejte hodnoty konfigurace v `<appSettings>` oddíl.
  - Vaše `ida:ClientId` Id aplikace, kterou jste zkopírovali z portálu.

Nakonec vyčistit, vytvoření a spuštění každého projektu.  Teď máte .NET MVC rozhraní API webových, která přijímá tokenů z obou osobní účty Microsoft a pracovní nebo školní účty.  Přihlaste se k TodoListClient a volání webu rozhraní api pro přidání úkolů do seznamu úkolů uživatele.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako .zip tady](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Další kroky
Teď můžete přesunout na další témata.  Můžete zkusit:

[Volání webového rozhraní API z Web Appu >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Další zdroje informací najdete v tématech:
- [Průvodce pro vývojáře verze 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Značka StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
