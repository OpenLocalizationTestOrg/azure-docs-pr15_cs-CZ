<properties
    pageTitle="Azure AD Xamarin Začínáme | Microsoft Azure"
    description="Jak vytvořit Xamarin aplikaci, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integrace Azure AD do aplikace Xamarin

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin umožňuje napsat mobilních aplikací v jazyce C#, mohlo by umožnit spuštění v iOS, Android a Windows (mobilní zařízení a počítače). Pokud vytváříte aplikace pomocí Xamarin, Azure AD umožňuje rychle a jednoduše k ověření uživatelů s jejich účty služby Active Directory. Umožňuje taky aplikaci bezpečnému používání všechny webového rozhraní API chráněny Azure AD, například rozhraní API Office 365 nebo rozhraní API Azure.

Xamarin aplikací, které se potřebují dostat k chráněným prostředkům Azure AD poskytuje Active Directory Authentication Library nebo ADAL. ADAL je jediný v životnost slouží k Usnadněte aplikace získat přístup tokeny. Která ukazuje, jak snadno, tady jsme budete vytvářet "Adresáře osobou hledající" aplikace, která:

-   Běží na iOS, Android, počítač s Windows, Windows Phone a Windows Store.
- Pro ověřování uživatelů a získat tokeny pro rozhraní API Azure AD grafu používá jediné portable knihovny tříd (PCL)
-   Hledá v adresáři pro uživatele s dané UPN.

Vytvoření dokončení aplikace fungovat, musíte:

2. Nastavení prostředí vývoj Xamarin
2. Zaregistrujte aplikace v Azure AD.
3. Instalace a konfigurace ADAL.
5. Pomocí ADAL tokenů z Azure AD.

Jak začít, [můžete si stáhnout kostry projektu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Každý je řešení Visual Studio 2013. Taky musíte tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace. Pokud ještě nemáte klienta, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. nastavení Xamarin vývojové prostředí*
Protože tento kurz zahrnuje projekty pro iOS, Android a Windows, musíte mít Visual Studia a Xamarin společně. K vytvoření nezbytné prostředí, postupujte podle pokynů dokončení na [Nastavení a instalace for Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) na webu MSDN. Tyto pokyny zahrnout materiál, který můžete zkontrolovat zobrazíte další informace o Xamarin během čekání pro instalační dokončete. 

Po dokončení nastavení potřebné, otevřete řešení ve Visual Studiu začít pracovat. Bude hledat šest projekty: pět platformu projekty a jeden Přenosná knihovna tříd, která budou sdíleny všechny platformy`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. zaregistrovat aplikaci osobou hledající adresáře*
Povolit aplikaci získat tokenů, Nejdřív musíte zaregistrovat v Azure AD klienta a udělit oprávnění pro přístup k rozhraní API Azure AD grafu:

-   Přihlaste se k [portálu Správa Azure](https://manage.windowsazure.com)
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte novou **Nativní klientské aplikaci**.
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přesměrovat Uri** tvoří kombinace schéma a řetězec využívající Azure AD vrátíte token odpovědi. Zadejte určité hodnotě, například `http://DirectorySearcher`.
-   Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor. Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě **Konfigurovat** .
- Taky na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací". Aplikace "Azure Active Directory" přidáte oprávnění k **adresáři přístup vaše organizace** ve skupinovém rámečku **Delegovat oprávnění**. To vám umožní aplikace k vytvoření dotazu grafu rozhraní API pro uživatele.

## <a name="2-install--configure-adal"></a>*2. instalace a konfigurace ADAL*
Teď, když máte aplikace v Azure AD, můžete nainstalovat ADAL a zadejte kód týkající se identity. Aby ADAL mohli komunikovat s Azure AD budete muset poskytnout některé informace o aplikaci registrace.
-   Začněte tím, že přidáte ADAL pro všechny projekty v řešení pomocí konzole balíčku správce.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Byste měli zaznamenat dvěma odkazům knihovny se přidají do každého projektu - PCL část ADAL a část platformu.

-   V projectu DirectorySearcherLib otevřete `DirectorySearcher.cs`. Změňte hodnoty člena třídy tak, aby odrážely hodnoty, které vkládáte do portálu Azure. Pokaždé, když se použije ADAL kódu odkazovat tyto hodnoty.
    -   `tenant` Je domain Azure AD klienta, například contoso.onmicrosoft.com
    -   `clientId` Je clientId aplikace, kterou jste zkopírovali z portálu.
    - `returnUri` Je redirectUri jste zadali v portálu, například `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. pomocí ADAL tokenů z AAD*
*Almost* všechny aplikace na ověřování logiky spočívá v `DirectorySearcher.SearchByAlias(...)`. Stačí na projektech specifické pro platformu je předat kontextové parametr `DirectorySearcher` PCL.

- Nejdřív otevřete `DirectorySearcher.cs` a přidejte nové parametr `SearchByAlias(...)` metody. `IPlatformParameters`je kontextové parametr, které je zahrnuta specifické pro platformu objekty, které ADAL je potřeba provést ověření.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Pak inicializace `AuthenticationContext` -ADAL uživatele primární předmětu. Toto je místo, kam předáte ADAL souřadnice potřebuje komunikovat s Azure AD. Zavolejte `AcquireTokenAsync(...)`, který přijme `IPlatformParameters` objektu a vyvolá tok ověřování nutné vracet token do aplikace.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`nejdřív pokusí vrátit token pro požadovaný zdroj (API graf v tomto případě) bez dotázání uživatele zadejte své přihlašovací údaje (přes ukládání do mezipaměti nebo aktualizaci staré tokeny). Pouze v případě potřeby ji stránce se zobrazí uživateli znaménko Azure AD před získáním požadované token.


- Můžete pak připojíte přístupový token požadavek na rozhraní API grafu se tak mohli ověřovat záhlaví:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

To je vše pro `DirectorySearcher` PCL a aplikace je týkající se identity kódu.  Zbývá volání `SearchByAlias(...)` způsob zobrazení jednotlivých platformy a kde potřeby přidat kód pro správně zpracování životním cyklu uživatelského rozhraní.

####<a name="android"></a>Android:
- V `MainActivity.cs`, přidejte volání `SearchByAlias(...)` u tlačítka klikněte na rutinu:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Budete taky muset přepsat `OnActivityResult` životního cyklu metoda přesměrovat žádné ověřování přesměruje zpátky na příslušný způsob.  ADAL poskytuje metodu Pomocníka pro tuto v Androidu:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Počítač s Windows:
- V `MainWindow.xaml.cs`, jednoduše zavolání na `SearchByAlias(...)` předání `WindowInteropHelper` v na plochu `PlatformParameters` objektu:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- V `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` objekt jednoduše trvá odkaz na řadiče zobrazení:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Univerzální Windows:
- V systému Windows univerzální otevřete `MainPage.xaml.cs` a implementace `Search` metodu, která vyhledává helper ve sdíleném projektu aktualizovat uživatelské rozhraní podle potřeby.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Blahopřejeme! Nyní máte pracovní Xamarin aplikace, která obsahuje možnost ověřování uživatelů a bezpečné volat rozhraní API webových pomocí OAuth 2.0 napříč pěti různých platformách. Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé. Spusťte aplikaci DirectorySearcher a přihlášení pomocí jedné z těchto uživatelů. Hledání podle jejich Přípony ostatním uživatelům.

ADAL usnadňuje začlenit běžné identity funkce do aplikace. Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další. Opravdu potřebujete stačí jedno volání rozhraní API `authContext.AcquireToken*(…)`.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Teď můžete přesunout na další identity scénářů. Můžete zkusit:

[Zabezpečení .NET webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
