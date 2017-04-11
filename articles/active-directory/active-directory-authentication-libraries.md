<properties
   pageTitle="Azure Active Directory Authentication knihoven | Microsoft Azure"
   description="Azure AD Authentication Library (ADAL) vývojářům klientské aplikace umožňuje snadno ověřování uživatelů do cloudu nebo místní služby Active Directory (AD) a získat přístup tokenů zabezpečení rozhraní API volání."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory Authentication knihoven

Ověřování Azure AD knihovny (ADAL) umožňuje vývojářům klientské aplikace snadno ověřování uživatelů do cloudu nebo místní služby Active Directory (AD) a získat přístup tokenů zabezpečení volání rozhraní API. ADAL má řadu funkcí, které nastavení ověřování jednodušší pro vývojáře, například asynchronní podporu, která dokáže nahradit tokenu mezipaměti, které ukládá tokeny přístupu a aktualizace tokeny automatické tokenu aktualizace, když skončí platnost přístupový token a token aktualizace je k dispozici a další. Zpracováním většina složitost ADAL vývojář zaměřit na logiku ve své aplikaci a snadno zabezpečení prostředků aniž byste byli odborník na zabezpečení.

ADAL je k dispozici na různých platformách.

|Platformy|Název balíčku|Klient Server|Ke stažení|Zdrojový kód|Dokumentace a vzorky|
|---|---|---|---|---|---|
|.NET klienta pro Windows Store, UWP Xamarin iOS a Android|Služby Active Directory Authentication Library (ADAL) pro .NET v3 |Klient|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL pro .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Si přečtěte následující dokumentaci](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET klienta pro Windows Store, Windows Phone 8.1 |Služby Active Directory Authentication Library (ADAL) pro .NET v2 |Klient|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL pro .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Si přečtěte následující dokumentaci](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Služby Active Directory Authentication Library (ADAL) JavaScript|Klient|[ADAL pro JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL pro JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Ukázka: [SinglePageApp DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X, iOS|Služby Active Directory Authentication Library (ADAL) pro cíle C|Klient|[ADAL pro cíle C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL pro cíle C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Ukázka: [NativeClient iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Služby Active Directory Authentication Library (ADAL) pro Android|Klient|[ADAL pro Android (centrálním úložišti)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL pro Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Ukázka: [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Služby Active Directory Authentication Library (ADAL) pro Node.js|Klient|[ADAL pro Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL pro Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Ukázka: [WebAPI Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport middleware ověřování pro uzel|Klient|[Azure Active Directory pas pro Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory pro Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Služby Active Directory Authentication Library (ADAL) jazyka Java|Klient|[ADAL pro Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL pro Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Rozšíření protokolu identity pro rozhraní Microsoft .NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Rozšíření modelu identity Azure AD pro .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON Web tokenu rutinu pro rozhraní Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Rozšíření modelu identit Azure AD pro .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Middleware OWIN, který umožňuje aplikaci technologii společnosti Microsoft pro ověřování.|Server|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|Middleware OWIN, která umožňuje aplikaci pro účely ověření OpenIDConnect.|Server|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Ukázka: [Web Appu DotNet OpenIDConnecty (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN middleware umožňující aplikaci pro účely ověření WS Federation.|Server|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Ukázka: [Web Appu DotNet WSFederation (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Scénáře

Tady jsou tři obvyklé scénáře, ve kterých se dá použít ADAL pro ověřování.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Ověřování uživatelů klientské aplikace vzdálené zdroji

V tomto scénáři je klientů, jako je aplikace WPF, která potřebuje přístup k prostředku vzdálené zajištěná Azure AD, například webového rozhraní API. Tahle má Azure předplatné věděli, jak vyvolat rozhraní API podřízený web a zná Azure AD klienta, který používá webového rozhraní API. Výsledkem je může použít ADAL usnadnit ověřování s Azure AD pomocí plně delegování zkušenosti ověřování pro ADAL nebo výslovně zpracování přihlašovací údaje uživatele. ADAL umožňuje snadno ověřuje uživatele, získáte od Azure AD přístupový token a aktualizace token a pomocí přístupový token proveďte požaduje do webového rozhraní API.

Příklad kód, který ukazuje tento scénář pomocí ověřování Azure AD najdete v článku [Nativní WPF klientské aplikace k rozhraní API webových](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Ověřování aplikace Server vzdálené zdroji

V tomto scénáři je spuštěné na serveru, který potřebuje přístup k prostředku vzdálené zajištěná Azure AD, například webového rozhraní API aplikace. Tahle má Azure předplatné věděli, jak vyvolat podřízené služby a ví, že používá klienta Azure AD rozhraní API webových. Výsledkem je může použít ADAL usnadnit ověřování s Azure AD pomocí explicitně zpracování přihlašovacích údajů aplikace. ADAL umožňuje snadno načítání token z Azure AD pomocí přihlašovacích údajů aplikace klienta a pomocí token proveďte požaduje do webového rozhraní API. ADAL zpracuje také Správa životnost přístupový token ukládáním ho a obnovení podle potřeby. Ukázka kódu, který ukazuje tento scénář najdete v článku [Aplikace konzoly k rozhraní API webových](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Ověřování aplikace Server jménem uživatele pro přístup k vzdálené zdroje

V tomto scénáři je spuštěné na serveru, který potřebuje přístup k prostředku vzdálené zajištěná Azure AD, například webového rozhraní API aplikace. Žádost nutné, aby udělali jménem uživatele v Azure AD. Tahle má Azure předplatné věděli, jak vyvolat podřízenými webového rozhraní API a zná Azure AD tenant služba používá. Po ověření uživatele k webové aplikaci aplikace můžete získat kód se tak mohli ověřovat pro uživatele z Azure AD. Webová aplikace můžete používat ADAL získat přístupový token a aktualizace token jménem uživatele pomocí se tak mohli ověřovat kód a klient jména a hesla k aplikaci z Azure AD. Když webové aplikace je k dispozici přístupový token, může webového rozhraní API zavolat do tokenu vypršení platnosti. Když skončí platnost tokenu, webové aplikace pomocí ADAL zjištění nových přístupový token pomocí aktualizace tokenu, která byla dříve přijata.


## <a name="see-also"></a>Viz taky

[Příručka pro vývojáře služby Azure Active Directory](active-directory-developers-guide.md)

[Scénáře ověřování pro službu Azure Active directory](active-directory-authentication-scenarios.md)

[Ukázky Azure Active Directory](active-directory-code-samples.md)
