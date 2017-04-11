<properties
    pageTitle="Azure AD .NET Začínáme | Microsoft Azure"
    description="Postup vytvoření .NET MVC webového rozhraní API, které lze integrovat s Azure AD pro a tak mohli ověřovat."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Ochrana rozhraní API webových používání nosný tokenů z Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Pokud vytváříte aplikace, která zajišťuje přístup k chráněným je třeba vědět, jak tyto materiály zabránit bude vyplacena neoprávněně přístupu k prostředkům.
Azure AD umožňuje rychle a jednoduše k ochraně webového rozhraní API OAuth nosný 2.0 přístup tokeny pomocí jenom několik řádků kódu.

Ve webových aplikacích Asp.NET lze provést pomocí společnosti Microsoft provádění součástí .NET Framework 4.5 middleware OWIN komunity řízené úsilím.  Tady použijeme OWIN k vytvoření "Se seznamem úkolů" webového rozhraní API:
-   Určí, které rozhraní API není zamčený.
-   Ověří, aby obsahoval volání rozhraní API webových platné tokenu přístup.

K tomuto účelu budete muset:

1. Registrace aplikace s Azure AD
2. Nastavení aplikace na používání OWIN ověřování kanálu.
3. Konfigurace klientské aplikaci zavolat na k proveďte seznamu rozhraní API webových

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Každý je řešení Visual Studio 2013.  Taky musíte tenanta Azure AD ve kterých se registruje aplikace.  Pokud žádný nemáte už, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. registrace aplikace s Azure AD*
Zajistit aplikace Nejdřív musíte vytvořit aplikaci pro vašeho klienta a poskytnout Azure AD několik důležité informace.

-   Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com)
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte nové **webové aplikace a/nebo WebAPI**.
    -   **Název** aplikace popisuje aplikace koncovým uživatelům.  Zadejte "seznam služby".
    -   **Přesměrovat Uri** tvoří kombinace schéma a řetězec využívající Azure AD by vrátit všechny tokeny aplikace požadované. Zadejte `https://localhost:44321/` pro tuto hodnotu.
-   Po dokončení zápisu, přejděte na kartu **Konfigurovat** a vyhledejte **Aplikaci Identifikátor URI** pole.  Zadejte identifikátor klienta specifické pro tuto hodnotu, například`https://contoso.onmicrosoft.com/TodoListService`
- Uložte konfiguraci.  Ponechání otevřeného portálu: Můžete taky muset zaregistrovat klientské aplikace brzy.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. nastavení aplikace pro použití kanálu OWIN ověřování*

Zaregistrování jste aplikaci s Azure AD, budete muset nastavit aplikace komunikovat s Azure AD za účelem ověření pravosti příchozí žádosti a tokeny.

-   Chcete-li začít otevřete řešení a přidejte balíčků OWIN middleware NuGet TodoListService projektu v konzole balíčku správce.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Přidání třídu OWIN spuštění TodoListService projektu s názvem `Startup.cs`.  Pravý tlačítkem myši na projektu--> **Přidat** --> vyhledejte "OWIN"-->**Nová položka** .  OWIN middleware vyvolá `Configuration(…)` metoda při spuštění aplikace.
-   Změna deklaraci třídy na `public partial class Startup` -jsme už implementovali součástí tohoto třídy za vás v jiném souboru.  V `Configuration(…)` metody, používat volání ConfgureAuth(...) nastavení ověřování pro web app.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(…)` metody.  Parametry zadáte v `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bude sloužit jako souřadnice aplikaci komunikovat s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Teď můžete použít `[Authorize]` atributy můžete chránit řadiče a akce s JWT nosný ověřování.  Vylepšení vzhledu `Controllers\TodoListController.cs` třídy slovem udělit oprávnění.  Tímto způsobem bude muset uživateli přihlášení před přístupem na této stránce.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Když oprávněnými volající úspěšně vyvolá jednu z `TodoListController` rozhraní API, akce potřebovat přístup k informacím o volajícím.  OWIN poskytuje přístup k deklarací uvnitř nosný token prostřednictvím `ClaimsPrincpal` objektu.  
- Běžné požadavku na web rozhraní API je ověření "obory" účastní token – zajistíte tak, že koncový uživatel souhlasí s o oprávněních požadovaných pro přístup ke službě seznamu úkol:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Nakonec otevřete `web.config` souborů v kořenové složce TodoListService projektu a zadejte hodnoty konfigurace v `<appSettings>` oddíl.
  - `ida:Tenant` Je název vašeho klienta Azure AD, například "contoso.onmicrosoft.com".
  - Vaše `ida:Audience` je URI ID aplikace aplikace, která jste zadali v portálu Azure.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. konfigurace klientské aplikace a spuštění služby*
Chcete-li zobrazit seznam služby úkol v akci, budete muset nakonfiguruje klient seznamu úkol, můžete získat tokenů z AAD a volat na služby.

- Přejděte zpátky na [Portálu Správa Azure](https://manage.windowsazure.com)
- Vytvoření nové aplikace ve vašem klientovi Azure AD a vyberte **Nativní klientské aplikace** v výsledný dotaz.
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   Zadejte `http://TodoListClient/` pro hodnotu **Přesměrovat Uri** .
- Po dokončení zápisu AAD přiřadí aplikace jedinečné **Id klienta**. Musíte mít tuto hodnotu v dalším krokům tak zkopírujte ji z karty konfigurovat.
- Taky na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací". Klikněte na "Přidat aplikaci." Vyberte v rozevíracím seznamu "Zobrazit" "Všechny aplikace" a klikněte na horní značku zaškrtnutí. Vyhledejte a klikněte na vaše se seznam služeb a klikněte na značku zaškrtnutí dole na Přidat aplikaci. Vyberte "Aplikace Access se seznam služeb" z rozevíracího seznamu "Delegovat oprávnění" a konfiguraci uložte.


- Ve Visual Studiu otevřete `App.config` v TodoListClient projektu a zadejte hodnoty konfigurace v `<appSettings>` oddíl.
  - `ida:Tenant` Je název vašeho klienta Azure AD, například "contoso.onmicrosoft.com".
  - Vaše `ida:ClientId` ID aplikace jste zkopírovali z portálu Microsoft Azure.
  - Vaše `todo:TodoListResourceId` je URI ID aplikace udělat seznam s aplikace, která jste zadali v portálu Azure.

Nakonec vyčistit, vytvoření a spuštění jednotlivé projekty!  Pokud jste to ještě neudělali, teď je potřeba vytvořit nového uživatele ve vašem klientovi s *. doménu onmicrosoft.com.  Přihlaste se ke klientovi se seznamem s tímto uživatelem a přidat některé úkoly uživatele do seznamu úkolů.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Teď přejdete k další příklady další identity.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
