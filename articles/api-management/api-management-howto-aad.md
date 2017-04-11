<properties 
    pageTitle="Jak povolit vývojář účty správy rozhraní API Azure pomocí služby Azure Active Directory" 
    description="Zjistěte, jak povolit uživatele služby Azure Active Directory v části Správa API." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Jak povolit vývojář účty správy rozhraní API Azure pomocí služby Azure Active Directory


## <a name="overview"></a>Základní informace
Tato příručka, uvidíte, jak povolit přístup k portálu pro vývojáře pro všechny uživatele v jedné nebo více Azure Active adresáře. Tato příručka, taky uvidíte, jak ke správě skupin služby Azure Active Directory uživatele přidáním externích skupiny, které obsahují uživatelům Azure Active Directory.

>Postupujte podle pokynů v této příručce nejdřív nemáte Azure Active Directory, ve kterém chcete vytvořit aplikaci.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Jak povolit vývojář účty služby Azure Active Directory

Začněte tím, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

V nabídce **Rozhraní API správy** na levé straně klikněte na **zabezpečení** a klikněte na **Externí identity**.

![Externí identity][api-management-security-external-identities]

Klikněte na **Azure Active Directory**. Poznamenejte **Přesměrování adresy URL** a zaměňte na Azure Active Directory v portálu klasické Azure.

![Externí identity][api-management-security-aad-new]

Klikněte na tlačítko **Přidat** k vytvoření nové aplikace služby Azure Active Directory a zvolte **Přidat aplikaci, které vyvíjí mé organizaci**.

![Přidat novou aplikaci služby Azure Active Directory][api-management-new-aad-application-menu]

Zadejte název aplikace, vyberte **webovou aplikaci a rozhraní API webových**a kliknutím na tlačítko Další.

![Novou aplikaci služby Azure Active Directory][api-management-new-aad-application-1]

**Přihlašovací adresa URL**zadejte adresu URL přihlašování portálu vývojář. V tomto příkladu je **Adresa URL přihlašování** `https://aad03.portal.current.int-azure-api.net/signin`. 

Adresu **URL ID aplikace**zadejte doménu výchozí nebo vlastní doménu pro službu Azure Active Directory a přidat jedinečný řetězec do ní. V tomto příkladu je výchozí doména **https://contoso5api.onmicrosoft.com** použita s příponou **/api** zadali.

![Nové vlastnosti aplikace služby Azure Active Directory][api-management-new-aad-application-2]

Klikněte na tlačítko Kontrola uložte a vytvořit novou aplikaci a přejděte na kartu **Konfigurovat** nakonfigurovat nové aplikace.

![Vytvořena nová aplikace služby Azure Active Directory][api-management-new-aad-app-created]

Pokud jsou více Azure Active adresářů se dá použít pro tuto aplikaci, klepněte na tlačítko **Ano** pro **aplikace více klienta**. Výchozí hodnota je **č**.

![Aplikace je více klienta][api-management-aad-app-multi-tenant]

Z části **Azure Active Directory** na kartě **Externí identit** v portálu Publisheru zkopírujte **Přesměrování adresy URL** a vložit ho do textového pole **URL odpověď** . 

![Adresa URL odpověď][api-management-aad-reply-url]

Přejděte dolů na kartě konfigurovat, vyberte rozevírací seznam **Oprávnění aplikace** a zaškrtněte políčko **data adresářů pro čtení**.

![Oprávnění aplikace][api-management-aad-app-permissions]

Vyberte rozevírací seznam **Oprávnění delegáta** a kontrola **Povolit přihlašování a číst profily uživatelů**.

![Delegovaná oprávnění][api-management-aad-delegated-permissions]

>Další informace o aplikaci a delegované oprávnění najdete v tématu [přístup k rozhraní API grafu][].

Zkopírujte **Kód klienta** do schránky.

![Id klienta][api-management-aad-app-client-id]

Přepněte se zpátky do portálu Publisheru a vložte do pole **Id klienta** zkopírovali z konfigurace aplikace služby Azure Active Directory.

![Id klienta][api-management-client-id]

Přepněte se zpátky do konfiguraci služby Azure Active Directory a klikněte na **vyberte dobu trvání** rozevírací seznam v části **klíče** a určení intervalu. V tomto příkladě se používá **1 rok** .

![Klíč][api-management-aad-key-before-save]

Klepněte na tlačítko **Uložit** uložte konfiguraci a zobrazení klíče. Klíč zkopírování do schránky.

>Poznamenejte si tento klíč. Po zavření okna konfigurace Azure Active Directory nelze se znovu klávesu.

![Klíč][api-management-aad-key-after-save]

Přepněte se zpátky do portálu Publisheru a vložte klávesu do textového pole **Tajná klienta** .

![Tajná klienta][api-management-client-secret]

**Povolené klienti** Určuje adresáře, které mají přístup k rozhraní API instanci služby správy rozhraní API. Určení domény instancí služby Azure Active Directory, ke kterým chcete udělit přístup. Vložení znaků newline, mezery nebo čárky můžete oddělit víc domén.

![Povolené klientů][api-management-client-allowed-tenants]

V části **Povolené klienti** lze zadat víc domén. Před všichni uživatelé můžou přihlásit se z jinou doménu než původní doménu, kdy byla registrována aplikace, globální správce jinou doménu musí udělit oprávnění pro aplikaci pro přístup k datům adresář. Udělit oprávnění, globální správce musí přihlásit k aplikaci a klikněte na **přijmout**. V následujícím příkladu `miaoaad.onmicrosoft.com` přidala **Povolených pro klienty** a globální správce z této domény přihlašováním poprvé.

![Oprávnění][api-management-permissions-form]

>Pokud – globální správce se pokusí přihlásit před udělením oprávnění globálního správce, pokus o přihlášení se nezdaří a zobrazí se obrazovka chyby.

Jakmile se požadované konfiguraci není zadán, klikněte na **Uložit**.

![Uložení][api-management-client-allowed-tenants-save]

Až se změny uloží, můžete uživatelům v zadaném Azure Active Directory Přihlaste se k portálu pro vývojáře pomocí následujících kroků v tématu [přihlášení k portálu pro vývojáře pomocí účtu služby Azure Active Directory][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Přidání externích Azure Active Directory skupin

Po povolení přístupu pro uživatele v Azure Active Directory, můžete přidat skupiny služby Azure Active Directory do rozhraní API správy snadněji spravovat přidružení vývojáře ve skupině požadované produkty.

> Abyste mohli konfigurovat externí skupiny Azure Active Directory, Azure Active Directory musí nejdříve možné konfigurovat kartě identit pomocí postupu v předchozí části. 

Externí skupin služby Azure Active Directory se přidají na kartě **zobrazení** produktů, u kterého chcete udělit přístup ke skupině. Klikněte na **produkty**a potom klikněte na název požadovaného produktu.

![Konfigurace produktů][api-management-configure-product]

Přejděte na kartu **zobrazení** a klikněte na **Přidat skupin služby Azure Active Directory**.

![Přidání skupiny][api-management-add-groups]

V rozevíracím seznamu vyberte **Azure Active Directory klienta** a potom zadejte název požadovanou skupinu v seznamu **skupiny** přidat textové pole.

![Vyberte skupinu][api-management-select-group]

Tento název skupiny najdete v seznamu **skupiny** pro služby Azure Active Directory, jak je vidět v následujícím příkladu.

![Seznam skupin služby Azure Active Directory][api-management-aad-groups-list]

Klikněte na **Přidat** do ověřte název skupiny a přidejte skupinu. V tomto příkladu **Contoso 5 vývojáři** vnější skupiny přidá. 

![Přidání skupiny][api-management-aad-group-added]

Klepněte na tlačítko **Uložit** uložte do nového výběru skupiny.

Po konfiguraci skupinu Azure Azure Active Directory z jednoho výrobku je k dispozici ke kontrole na kartě **viditelnost** ostatních produktů v instanci služby správy rozhraní API.

Kontrola a konfigurace vlastností pro vnější skupiny po byly přidány, klikněte na název skupiny na kartě **skupiny** .

![Správa skupin][api-management-groups]

Tady můžete upravit **název** a **Popis** skupiny.

![Úprava skupiny][api-management-edit-group]

Uživatelé z nakonfigurované Azure Active Directory můžete se přihlásit k portálu vývojář a zobrazení a přihlášení k odběru skupiny, ke kterým mají viditelnost postupujte podle pokynů v následující části.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Jak se přihlásit k portálu pro vývojáře pomocí účtu služby Azure Active Directory

Přihlaste se k portálu pro vývojáře pomocí účtu služby Azure Active Directory nakonfigurovány v předchozích částech, otevřete nové okno prohlížeče pomocí **přihlašování adresu URL** z konfigurace aplikace služby Active Directory a klikněte na **Azure Active Directory**.

![Karta Vývojář v portálu][api-management-dev-portal-signin]

Zadejte přihlašovací údaje jednoho uživatele ve vaší Azure Active Directory a klikněte na **přihlásit**.

![Přihlásit se][api-management-aad-signin]

Registrační formulář můžete být vyzváni, pokud je požadována všechny požadované informace. Dokončete registrační formulář a klikněte na **přihlásit**.

![Registrace][api-management-complete-registration]

Uživatel je teď přihlášení k portálu pro vývojáře pro instanci služby správy rozhraní API.

![Registrace je dokončena.][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

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
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Přístup k grafu API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Přihlaste se k portálu pro vývojáře pomocí účtu služby Azure Active Directory]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

