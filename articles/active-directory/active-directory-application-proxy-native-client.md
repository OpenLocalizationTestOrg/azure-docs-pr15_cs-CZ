<properties
    pageTitle="Jak povolit publikování native client – aplikací s aplikacemi proxy | Microsoft Azure"
    description="Popisuje, jak povolit native client – aplikace komunikovat s Azure AD aplikace Proxy spojnice k poskytnutí zabezpečeného vzdáleného přístupu ke svým aplikacím místní."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Povolení aplikace native client – chcete provést interakci s proxy aplikací

Azure Active Directory aplikace Proxy se často používá pro publikování prohlížeče aplikací, jako je SharePoint, Outlook Web Access a vlastních aplikací line of business. Jej lze také publikovat native client – aplikace, které se liší od webových aplikací web apps, protože získat nainstalovaný na zařízení. Důvodem je podporou Azure AD Vystavitel klíčovými slovy, která se odesílají ve standardní povolte HTTP záhlaví.

![Vztah mezi koncovým uživatelům Azure Active Directory a publikované aplikace](./media/active-directory-application-proxy-native-client/richclientflow.png)

Publikovat žádost doporučujeme používat Authentication Library Azure AD, který má na starosti všechny problém ověřování a podporuje mnoha prostředích jiného klienta. Proxy aplikace zapadá do [Nativní aplikaci rozhraní API webových scénáře](active-directory-authentication-scenarios.md#native-application-to-web-api). Proces způsoby vypadá takto:

## <a name="step-1-publish-your-application"></a>Krok 1: Publikování aplikace

Publikování aplikace proxy, stejně jako jakékoli jiné aplikace, přiřaďte uživatelům a dát jim premium nebo základní licence. Další informace v tématu [publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Krok 2: Konfigurace aplikace

Konfigurace aplikace nativní následujícím způsobem:

1. Přihlaste se k portálu Azure klasické.
2. Vyberte ikonu služby Active Directory v nabídce nalevo a pak vyberte adresáře.
3. V nabídce začátek klikněte na **aplikace**. Pokud není aplikace byly přidány do vašeho adresáře, tato stránka bude ukazovat jenom na odkaz **Přidat aplikaci** . Klikněte na odkaz nebo také kliknete na tlačítko **Přidat** na panelu s příkazy.
4. Na stránce **Co chcete udělat** klikněte na odkaz **Přidat aplikaci, které vyvíjí naší organizace**.
5. Na stránce **námi o aplikaci** zadejte název aplikace a zvolte **nativní klientské aplikaci**. Klepněte na ikonu šipky pokračovat.
6. Na stránce **informace o aplikaci** **Přesměrovat URI** nativní klientské aplikace a klepněte na tlačítko zaškrtnutí dokončete.

Přidala vaše aplikace a přejdete na úvodní stránku aplikace.

## <a name="step-3-grant-access-to-other-applications"></a>Krok 3: Udělení přístupu jiných aplikací

Povolení nativní aplikaci zveřejněn do jiných aplikací ve vašem adresáři:

1. V nabídce začátek klikněte na **aplikace**, vyberte nový nativní aplikaci a potom klikněte na **Konfigurovat**.
2. Přejděte dolů do části **oprávnění na jiné aplikace** . Klikněte na tlačítko **Přidat aplikaci** a vyberte aplikaci proxy, která chcete udělit přístup nativní aplikaci a klikněte na značku zaškrtnutí v pravém dolním rohu. Z rozevírací nabídky **Delegované oprávnění** vyberte nové oprávnění.

![Oprávnění pro ostatní aplikace snímek – přidat aplikaci](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Krok 4: Úprava Active Directory Authentication Library

Upravte kód nativní aplikaci v rámci ověření z Active Directory ověřování knihovny (ADAL) do patřit následující úkoly:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Proměnné nahrazuje následujícím způsobem:

- **TenantId** najdete v GUID do pole Adresa URL aplikace **konfigurační** stránce ihned po "/ adresář /".
- **Adresa URL frontend** je front-end adresa URL zadaná v aplikaci Proxy a najdete na stránce **Konfigurace** proxy aplikace.
- **Id klienta** nativní aplikaci najdete na stránce **Konfigurovat** nativní aplikaci.
- **Přesměrovat URI nativní aplikaci** najdete na stránce **Konfigurovat** nativní aplikaci.

![Nové nativní aplikaci konfigurace snímek obrazovky stránky](./media/active-directory-application-proxy-native-client/new_native_app.png)

Další informace o toku nativní aplikaci najdete v článku [nativní aplikaci webového rozhraní API](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Viz taky

- [Publikování aplikací pomocí vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)
- [Práce s aplikacemi přehled pohledávek](active-directory-application-proxy-claims-aware-apps.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
