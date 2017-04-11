<properties
   pageTitle="Azure Active Directory verze 2.0 ověřování knihoven | Microsoft Azure"
   description="Knihoven kompatibilní klienta a serveru middleware knihoven a související knihovny, zdroje a ukázky odkazy koncového bodu verze 2.0 Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory verze 2.0 ověřování knihoven
Koncový bod verze 2.0 Azure Active Directory (Azure AD) podporuje protokoly OAuth 2.0 a OpenID připojení 1.0 standardní. Můžete používat různé společnostmi Microsoft a jinými organizacemi s koncovým verze 2.0.

Při vytváření aplikace, která používá koncový bod verze 2.0, doporučujeme, abyste pomocí knihoven, které jsou napsal odborníky domény Protocol (protokol), kteří sledují metodiky životního cyklu vývoje zabezpečení (SDL), jako [je následovaný Microsoft][Microsoft-SDL]. Pokud se rozhodnete ruky kód podpora protokoly, doporučujeme sledovat SDL metodologie a zaměřit otázky bezpečnosti ve vlastnostech standardy pro každý protokol.

## <a name="types-of-libraries"></a>Typy knihoven

Verze 2.0 Azure AD spolupracuje dva typy knihoven:

- **Knihoven klienta**. Nativní klienty a servery pomocí knihoven klienta tokeny přístupu pro volání prostředku, například Microsoft Graph.
- **Server middleware knihoven**. Webové aplikace pomocí knihoven middleware serveru pro přihlášení uživatele. Rozhraní API webových pomocí knihoven middleware serveru pro ověřování klíčovými slovy, která jsou odeslány nativní klienty nebo jiné servery.

## <a name="library-support"></a>Podpora knihovny
Protože standardy knihovny můžete zvolit, když použijete koncový bod verze 2.0, je důležité vědět, kam se obrátit pro podporu. Informace o problémech a poslat žádost o funkci v kódu knihovny obraťte se na vlastníka knihovny. Informace o problémech a poslat žádost o funkci při provádění straně služby protokolu od společnosti Microsoft.

Knihovny se dvě kategorie podpory:

- **Podporované společností Microsoft**. Microsoft poskytuje opravách u těchto knihoven a je provedena SDL pečlivostí na těchto knihoven.
- **Kompatibilní**. Společnost Microsoft v základní scénáře testováno těchto knihoven a potvrdit, že fungují s koncovým verze 2.0. Společnost Microsoft neposkytuje opravy těchto knihoven a nebyla mít přehled o těchto knihoven. Problémy a poslat žádost o funkci budou přesměrovány knihovna zdrojový otevřít projekt.

Seznam knihoven, které spolupracují s koncový bod verze 2.0 najdete v tématu další oddíly v tomto článku.

## <a name="microsoft-supported-client-libraries"></a>Knihovny Microsoft podporované klienta
| Platformy| Název knihovny| Ke stažení | Zdrojový kód | Ukázka |
| :-: | :-: | :-: | :-: | :-: |
| .NET pro Windows Store, Xamarin | Microsoft Authentication Library (MSAL) pro .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL pro .NET (GitHub)][ClientLib-NET-Repo] | [Windows desktop native client – ukázka][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js modulu Plug-In | [Passport Azure AD (npm)][ClientLib-Node-Lib] | [Passport Azure AD (GitHub)][ClientLib-Node-Repo] | Již brzy |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Podporované společností Microsoft serveru middleware knihoven
| Platformy| Název knihovny| Ke stažení | Zdrojový kód | Ukázka |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID připojení Middleware technologie ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana projektu (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Ukázka webové aplikace][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuth nosný Middleware technologie ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana projektu (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Ukázka webového rozhraní API][ServerLib-Net4-Owin-Oauth-Sample] |
| Základní .NET | OWIN OpenID připojení Middleware pro .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [Zabezpečení technologie ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Ukázka webové aplikace][ServerLib-NetCore-Owin-Oidc-Sample] |
| Základní .NET | OWIN OAuth nosný Middleware pro .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [Zabezpečení technologie ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Již brzy |
| Node.js | Microsoft Azure Active Directory Passport.js modulu plug-in | [Passport Azure AD (npm)][ServerLib-Node-Lib] | [Passport Azure AD (GitHub)][ServerLib-Node-Repo] | [Ukázka webové aplikace][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Knihoven kompatibilní klienta
| Platformy| Název knihovny | Testované verze | Zdrojový kód | Ukázka |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Ukázka nativní aplikaci](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Ukázka nativní aplikaci](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Již brzy |
| Python baňky | [Baňky OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Baňky OAuthlib](https://github.com/lepture/flask-oauthlib) | Již brzy |
| Skutečné | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Již brzy |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Knihovny middleware kompatibilní serveru.
Již brzy

## <a name="related-content"></a>Související obsah
Další informace o koncový bod Azure AD verze 2.0 najdete v článku [Přehled verze 2.0 modelu aplikace Azure AD][AAD-App-Model-V2-Overview].

Jak pomoct při Upřesnit a přizpůsobovat naše obsah, použijte funkci Disqus komentáře na konci tohoto článku k poskytnutí zpětné vazby.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
