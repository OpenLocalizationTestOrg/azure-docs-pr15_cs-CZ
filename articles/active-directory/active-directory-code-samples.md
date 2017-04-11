<properties
   pageTitle="Ukázky Azure Active Directory | Microsoft Azure"
   description="Index Azure Active Directory ukázky uspořádaný podle scénáře."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Ukázky Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) slouží k přidání a tak mohli ověřovat webových aplikací a webového rozhraní API. Tato část slouží k propojení ukázek kódu, které ukazují, jak se to dělá a fragmenty kódu, které lze použít v aplikacích. Na stránce ukázka kód najdete podrobné čtení-mi témata, která se s požadavky, instalace a nastavení. A kód je komentář vám pomůže pochopit kritické oddíly.

Základní scénáře pro každý typ vzorku, najdete v tématu ověření scénáře pro Azure AD.

Přispívat na naše ukázky na GitHub: [Microsoft Azure Active Directory ukázky a si přečtěte následující dokumentaci](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webový prohlížeč webovou aplikaci

Tyto příklady ukazují, jak psát webové aplikace, který směruje prohlížeč přihlašovat je Azure AD.



| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| C# / .NET | [Web Appu OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Ověřování uživatelů z klienta Azure AD pomocí OpenID připojení (ASP.Net OpenID připojení OWIN middleware).
| C# / .NET | [Web Appu MultiTenant OpenIdConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Více klienta, kterého .NET MVC webovou aplikaci používající OpenID připojení (ASP.Net OpenID připojení OWIN middleware) pro ověřování uživatelů z víc klientů Azure AD.
| C# / .NET | [Web Appu WSFederation DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Umožňuje WS federace (ASP.Net WS Federation OWIN middleware) ověřování uživatelů z Azure AD klienta.






## <a name="single-page-application-spa"></a>Použití jedné stránky (zabezpečené ověřování HESLA)

Tento příklad ukazuje, jak psát zabezpečená s Azure AD aplikace jedné stránky.  

| Jazyk nebo platforma | Ukázka | Popis
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Použijte ADAL pro JavaScript a Azure AD k zabezpečení aplikace založené na AngularJS jedné stránky implementovaná s ASP.NET webového rozhraní API back-end.


## <a name="native-application-to-web-api"></a>Nativní aplikaci webového rozhraní API

Tyto příklady kódu ukazují, jak vytvářet native client – aplikace, které volají webového rozhraní API, které jsou chráněny Azure AD. Používají [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [OAuth 2.0 v Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| JavaScript | [NativeClient MultiTarget Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Umožňuje vytvářet Apache Cordova aplikace, která volá webového rozhraní API a používá Azure AD pro ověřování ADAL modul plug-in pro Apache Cordova.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | .NET WPF aplikace, která volá webového rozhraní API, který je zabezpečená pomocí Azure AD.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Aplikace pro Windows Store, která volá webového rozhraní API, který je zabezpečená s Azure AD.
| C# / .NET | [NativeClient WebAPI MultiTenant WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Aplikace pro Windows Store volání více klienta webového rozhraní API, který je zabezpečená s Azure AD.
| C# / .NET | [WebAPI OnBehalfOf DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Nativní klientské aplikace, která volá webového rozhraní API, která získá token jménem původní uživatele a potom na základě tokenu volání jiné webového rozhraní API.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Aplikace pro Windows Store pro Windows Phone 8.1, volající webového rozhraní API zajištěná Azure AD.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | IOS aplikace, která volá webového rozhraní API, který vyžaduje Azure AD pro ověřování.
| C# / .NET | [WebAPI ManuallyValidateJwt DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Nativní klientská aplikace, která obsahuje logiku zpracuje token JWT v webového rozhraní API, místo použití OWIN middleware.
| C# / Xamarin | [NativeClient Xamarin Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Xamarin vazbu k nativní Azure AD ověřování knihovny (ADAL) pro knihovnu Android.
| C# / Xamarin | [NativeClient Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Xamarin vazbu k nativní Azure AD ověřování knihovny (ADAL) pro iOS.
| C# / Xamarin | [NativeClient MultiTarget DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Xamarin projekt, který používá pět platformy a volá webového rozhraní API zajištěná Azure AD.
| C# / .NET | [NativeClient Headless DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Nativní aplikaci, která provádí interaktivním ověřování a volá webového rozhraní API, který je zajištěná Azure AD.



## <a name="web-application-to-web-api"></a>Webová aplikace na Web rozhraní API

Tyto příklady kódu zobrazit použití [OAuth 2.0 v Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) pro vytváření webových aplikací, které volají webového rozhraní API, které jsou chráněny Azure AD.

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| C# / .NET | [Web Appu WebAPI OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Zavolejte na linku webového rozhraní API s oprávněními uživatele přihlášení se změnami.
|  C# / .NET | [Web Appu – WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Zavolejte webového rozhraní API pomocí oprávnění aplikace.
| C# / .NET | [Web Appu – WebAPI-OAuth2 – identity uživatele – Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Abyste mohli zavolat webového rozhraní API přidáte se tak mohli ověřovat pomocí [OAuth 2.0 v Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) stávající webovou aplikaci.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Nastavte si rozhraní REST API služba, která je integrována s Azure AD pro rozhraní API zámek. Obsahuje Node.js server pomocí rozhraní API webových.
| C# / .NET | [Web Appu – WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Více klienta MVC webovou aplikaci používající OpenID připojení (ASP.Net OpenID připojení OWIN middleware) pro ověřování uživatelů z Azure AD klienta. Používá se tak mohli ověřovat kódy pro vyvolání rozhraní API grafu.

## <a name="server-or-daemon-application-to-web-api"></a>Server nebo démon aplikací webového rozhraní API

Tyto příklady kódu ukazují, jak vytvářet aplikace démon nebo na serveru, která získává zdrojů z webového rozhraní API pomocí [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [OAuth 2.0 v Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| C# / .NET | [Démon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Aplikace konzoly volá webového rozhraní API. Klient credential je hesla.
| C# / .NET | [Démon CertificateCredential DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Konzoly aplikace, která volá webového rozhraní API. Klient credential je certifikát.


## <a name="calling-azure-ad-graph-api"></a>Volání rozhraní API Azure AD grafu

Tato ukázka kódu ukazují, jak vytvářet aplikace, které volají Azure AD grafu rozhraní API pro čtení a zápis dat adresáře.

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| Java | [Web Appu GraphAPI Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Webovou aplikaci používající rozhraní API grafu pro přístup k datům Azure AD adresář.
| PHP | [Web Appu GraphAPI PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Webovou aplikaci používající rozhraní API grafu pro přístup k datům Azure AD adresář.
| C# / .NET | [Web Appu GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Webovou aplikaci používající rozhraní API grafu pro přístup k datům Azure AD adresář.
| C# / .NET | [ConsoleApp GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Tato aplikace konzoly ukazuje běžné pro čtení a zápis volání rozhraní API grafu a ukazuje, jak spustit přiřazení licencí uživatelů a aktualizovat uživatele miniaturu fotografie a odkazy.
| C# / .NET | [ConsoleApp GraphAPI DiffQuery DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Konzoly aplikace, která používá rozdílné dotazu v rozhraní API grafu k získání pravidelných změn objektů uživatele v klientovi Azure AD.
| C# / .NET | [Web Appu GraphAPI DirectoryExtensions DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Aplikace MVC používá rozhraní API graf dotazy ke generování organizační diagram jednoduchého společnosti.
| PHP | [Web Appu GraphAPI DirectoryExtensions PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Aplikace PHP, která volá rozhraní API grafu zaregistrovat rozšíření a pak si projděte témata, aktualizovat a odstraňovat hodnoty v atributu rozšíření.


## <a name="authorization"></a>Povolení

Tyto příklady kódu předvedení použití Azure AD pro povolení.

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| C# / .NET | [Web Appu GroupClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Provedení řízení přístupu na základě rolí (RBAC) v aplikaci, která je integrována s Azure AD pomocí deklarace skupiny Azure Active Directory.
| C# / .NET | [Web Appu RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Provedení řízení přístupu na základě rolí (RBAC) v aplikaci, která je integrována s Azure AD pomocí role aplikací Azure Active Directory.


## <a name="legacy-walkthroughs"></a>Starší verze návody

Tyto kurzy technologii trochu starší, ale pořád pravděpodobně zajímavé.

| Jazyk/platformy | Ukázka | Popis
| ----------------- | ------ | -----------
| C# / .NET | [Na základě rolí a na základě ACL se tak mohli ověřovat v aplikaci Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | V aplikaci, která je integrována s Azure AD proveďte ověřování na základě rolí (RBAC) a na základě ACL.
| C# / .NET |  [AAL – Windows Store nejde služba REST – ověření](http://go.microsoft.com/fwlink/?LinkId=330605) |  [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (dřív to bylo vrstev AAL) pro Windows Store Beta slouží k přidání funkce ověření uživatele do aplikace pro Windows Store.
| C# / .NET | [Ověřování ADAL – nativní aplikaci a služba REST - s AAD pomocí dialogového okna prohlížeče](http://go.microsoft.com/fwlink/?LinkId=259814) |  Umožňuje přidávat funkce ověřování uživatelů ke klientovi WPF [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) .
| C# / .NET | [Ověřování ADAL – nativní aplikaci a služba REST - s ACS pomocí dialogového okna prohlížeče](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Slouží k přidání možností ověřování uživatelů ke klientovi WPF [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) a [2.0 služby Řízení přístupu (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) .
| C# / .NET | [ADAL - ověřování na serveru](http://go.microsoft.com/fwlink/?LinkId=259816) | Umožňuje zabezpečená volání služby ze strany proces serveru REST API MVC4 webové služby [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) .
| C# / .NET | [Přidání přihlašování do webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfigurace aplikace .NET provádět web jednotného přihlašování oproti adresáři organizace Azure AD.
| C# / .NET | [Vývoj více klienta webových aplikací s Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Použití Azure AD přidáte jednotného přihlašování a možnosti přístupu k adresáři jednu žádost .NET pro práci mezi více organizacemi.
JAVA | [Ukázka aplikace Java pro Azure AD graf rozhraní API](http://go.microsoft.com/fwlink/?LinkId=263969) | Použití rozhraní API grafu pro přístup k datům adresáře z Azure AD.
PHP | [Ukázka aplikace PHP Azure AD grafu rozhraní API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Použití rozhraní API grafu pro přístup k datům adresáře z Azure AD.
| C# / .NET | [Ukázka aplikace Azure AD grafu rozhraní API](http://go.microsoft.com/fwlink/?LinkID=262648) | Použití rozhraní API graf pro přístup k datům adresáře z Azure AD.
| C# / .NET | [Ukázka aplikace rozdílné dotazu Azure AD grafu](http://go.microsoft.com/fwlink/?LinkId=275398) | Pomocí rozdílné dotazu v rozhraní API grafu pravidelných změn objektů uživatele v klientovi Azure AD.
| C# / .NET | [Ukázka aplikace pro integraci více klienta cloudu žádost o Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Integrace aplikace více klienta do Azure AD.
| C# / .NET | [Zabezpečení aplikace pro Windows Store a webová služba REST Azure AD pomocí](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Vytvoření jednoduchého webového rozhraní API zdroje a klientské aplikace pro Windows Store Azure AD pomocí a [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Pomocí grafu rozhraní API k vytvoření dotazu Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Nakonfigurujte aplikaci rozhraní Microsoft .NET pro přístup k datům z adresáře klienta služby Azure AD pomocí rozhraní API Azure AD grafu.

## <a name="see-also"></a>Viz taky

##### <a name="other-resources"></a>Další zdroje

[Příručka pro vývojáře služby Azure Active Directory](active-directory-developers-guide.md)

[Azure AD grafu rozhraní API koncepční a funkce pro odkazy](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD grafu Pomocník rozhraní API pro knihovnu](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

