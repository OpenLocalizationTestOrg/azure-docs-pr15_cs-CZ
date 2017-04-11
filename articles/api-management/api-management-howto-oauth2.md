<properties 
    pageTitle="Jak povolit vývojář účty pomocí OAuth 2.0 správy rozhraní API Azure" 
    description="Zjistěte, jak povolit uživatelů pomocí OAuth 2.0 správy rozhraní API." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Jak povolit vývojář účty pomocí OAuth 2.0 správy rozhraní API Azure

Mnoho rozhraní API domovské stránce podpory [OAuth 2.0](http://oauth.net/2/) zabezpečené rozhraní API a ověřte, že pouze platné uživatelé mají přístup jenom přístupem zdroje, ke kterým máte nárok. Abyste mohli používat Správa rozhraní API Azure interaktivní vývojář konzoly s neznámé rozhraní API, službu můžete nakonfigurovat instanci služby pro práci s vaší OAuth 2.0 povolené rozhraní API.

## <a name="prerequisites"> </a>Požadavky

Tato příručka, se dozvíte, jak nakonfigurovat vaše instanci služby správy rozhraní API pro účely ověření OAuth 2.0 vývojář účty, ale není ukazují, jak konfigurovat poskytovatele OAuth 2.0. Konfigurace pro každého poskytovatele OAuth 2.0 se liší, přestože kroky jsou podobné a požadovaných informací použít v konfiguraci OAuth 2.0 v instanci služby správy rozhraní API je stejný. Toto téma ukazuje příklady použití Azure Active Directory jako poskytovatele OAuth 2.0.

>[AZURE.NOTE] Další informace o konfiguraci OAuth 2.0 pomocí služby Azure Active Directory najdete v článku [Web Appu GraphAPI DotNet][] vzorku.

## <a name="step1"> </a>Konfigurace serveru se tak mohli ověřovat OAuth 2.0 správy rozhraní API

Začněte tím, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>[AZURE.NOTE] Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

V nabídce **Rozhraní API správy** na levé straně klikněte na **zabezpečení** , klikněte **OAuth 2.0**a potom klikněte na **Přidat se tak mohli ověřovat serveru**.

![OAuth 2.0][api-management-oauth2]

Po kliknutí na tlačítko **Přidat se tak mohli ověřovat server**, se zobrazí formulář nové serveru se tak mohli ověřovat.

![Nový server][api-management-oauth2-server-1]

V polích **název** a **Popis** zadejte název a popis. 

>[AZURE.NOTE] Tato pole slouží k identifikaci serveru se tak mohli ověřovat OAuth 2.0 v aktuální instanci služby správy rozhraní API a jejich hodnoty nepochází z server OAuth 2.0.

Zadejte adresu **URL stránky registrace klienta**. Tato stránka je místo, kam můžete vytvářet a spravovat svoje účty uživatelů a se liší v závislosti na poskytovatele OAuth 2.0 používá. **Adresa URL stránky registrace klienta** bodů na stránku, která uživatelům slouží k vytváření a konfigurace svých účtů pro OAuth 2.0 poskytovatelům, kteří podporují Správa uživatelských účtů. V některých organizacích nevytvářejte konfigurace a použití této funkce i v případě poskytovatele OAuth 2.0 podporuje. Pokud váš poskytovatel OAuth 2.0 nemá Správa uživatelských účtů nakonfigurovali, zadejte URL zástupného symbolu tady třeba adresu URL vaší společnosti nebo adresy URL jako `https://placeholder.contoso.com`.

Následující části formuláře obsahuje **kód se tak mohli ověřovat udělit typy** **Adresa URL se tak mohli ověřovat koncového bodu**a **Metoda žádost o povolení** nastavení.

![Nový server][api-management-oauth2-server-2]

Zadejte **kód se tak mohli ověřovat udělit typy** kontrolou požadované typy. Ve výchozím nastavení je zadán **se tak mohli ověřovat kód** .

Zadejte adresu **URL koncový bod se tak mohli ověřovat**. Pro službu Azure Active Directory, tato adresa URL bude vypadat podobně jako následující adresu URL, kde `<client_id>` nahrazen id klienta, který identifikuje aplikace na server OAuth 2.0.

    https://login.windows.net/<client_id>/oauth2/authorize

**Metoda žádost o povolení** Určuje, jak se žádost o ověření odesílá na server OAuth 2.0. Ve výchozím nastavení **NAČÍST** zaškrtnuto.

Dál je, kde jsou uvedeny **Adresa URL Token koncového bodu**, **metody ověřování klienta**, **přístupový token odesílání metoda**a **výchozí obor** .

![Nový server][api-management-oauth2-server-3]

Pro server Azure Active Directory OAuth 2.0 **Token adresa URL koncového bodu** mají následující formát, kde `<APPID>` má formát `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Výchozí nastavení metod **ověřování klienta** je **základní**a **přístupový token odesílání způsob** je **záhlaví se tak mohli ověřovat**. Tyto hodnoty jsou nakonfigurované v této části formuláře, spolu s **výchozí obor**.

V části **přihlašovací údaje klienta** obsahuje **ID klienta** a **tajná klienta**, které jsou získány během vytváření a konfigurace serveru OAuth 2.0. Po byla vyplněná **ID klienta** a **tajná klienta** , je generováno **redirect_uri** **se tak mohli ověřovat kód** . Tento identifikátor URI se používá ke konfiguraci adresy URL odpovědi v konfiguraci server OAuth 2.0.

![Nový server][api-management-oauth2-server-4]

Pokud **se tak mohli ověřovat kód udělit typy** nastavena na **heslo vlastníka zdroje**, v části **přihlašovací údaje heslo vlastníka zdroje** se používá k určení tyto přihlašovací údaje, jinak můžete ponechat prázdné.

![Nový server][api-management-oauth2-server-5]

Po dokončení formuláři klikněte na **Uložit** uložte konfigurace serveru se tak mohli ověřovat rozhraní API Management OAuth 2.0. Po uložení konfigurace serveru můžete nakonfigurovat rozhraní API pro použití této konfigurace uvedeno v následující části.

## <a name="step2"> </a>Konfigurace rozhraní API používat ověření uživatele OAuth 2.0

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** , klikněte na název požadovaného rozhraní API, klikněte na **zabezpečení**a potom zaškrtněte políčko u **OAuth 2.0**.

![Ověření uživatele][api-management-user-authorization]

Vyberte požadovaný **serveru se tak mohli ověřovat** z rozevíracího seznamu a klikněte na **Uložit**.

![Ověření uživatele][api-management-user-authorization-save]

## <a name="step3"> </a>Otestujte ověření uživatele OAuth 2.0 v portálu pro vývojáře

Jakmile již nakonfigurovány serveru se tak mohli ověřovat OAuth 2.0 a nakonfigurovali API použít tento server, ji můžete otestovat tak, že přejdete na portálu pro vývojáře a volání rozhraní API.  Klikněte na **portál pro vývojáře** v nabídce horní doprava.

![Karta Vývojář v portálu][api-management-developer-portal-menu]

Klikněte na **rozhraní API** v nabídce začátek a vyberte **Rozhraní API ozvěnu**.

![Rozhraní API ozvěnu][api-management-apis-echo-api]

>[AZURE.NOTE] Pokud máte jenom jednu rozhraní API nakonfigurované nebo viditelné ke svému účtu, kliknutím na rozhraní API přejdete přímo na operace pro tuto rozhraní API.

Vyberte **Získat zdroje** operace, klikněte na **Otevřít konzoly**a vyberte z rozevíracího seznamu **se tak mohli ověřovat kód** .

![Otevřené konzoly][api-management-open-console]

Vybrán **se tak mohli ověřovat kód** , se zobrazí automaticky otevírané okno se přihlašovacím formuláři poskytovatele OAuth 2.0. V tomto příkladu je přihlašovacím formuláři poskytovanou Azure Active Directory.

>[AZURE.NOTE] Pokud máte automaticky otevíraných oken zakázané zobrazí se výzva k povolení prohlížeč. Po povolení je, vyberte na **autorizační kód** znovu a přihlašovacím formuláři se zobrazí.

![Přihlásit se][api-management-oauth2-signin]

Jakmile jste se přihlásili, jsou vyplněné **záhlaví požadavků** `Authorization : Bearer` záhlaví, které povolují žádost.

![Žádost o token záhlaví][api-management-request-header-token]

V tomto okamžiku můžete nakonfigurovat požadované hodnoty pro zbývající parametry a odešlete žádost. 

## <a name="next-steps"></a>Další kroky

Další informace o používání OAuth 2.0 a rozhraní API správy najdete v článku následující video a doprovodným [článek](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[Web Appu GraphAPI DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

