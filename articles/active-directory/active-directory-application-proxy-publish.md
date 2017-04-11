<properties
    pageTitle="Publikování aplikací s proxy server Azure AD aplikace | Microsoft Azure"
    description="Publikování aplikací místní do cloudu pomocí proxy server Azure AD aplikace."
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
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikování aplikací pomocí proxy server Azure AD aplikace

Proxy server Azure AD aplikace vám pomáhá podporovat vzdálené pracovníků publikováním místní aplikací k nim získat přístup přes internet. Tato bod byste měli už mít [povolený Proxy aplikace Azure klasické portálu](active-directory-application-proxy-enable.md). Tento článek vás provede kroky budete publikovat aplikace, které používáte v místní síti a poskytnutí zabezpečeného vzdáleného přístupu z mimo vaši síť. Po dokončení tohoto článku se budete chtít nakonfigurovat aplikaci přizpůsobených informací nebo požadavkům na zabezpečení.

> [AZURE.NOTE] Proxy aplikace je funkce, která je dostupná jenom v případě, že jste upgradovali na Premium nebo základní verzi Azure Active Directory. Další informace najdete v tématu [edicí Azure Active Directory](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Publikování aplikace pomocí Průvodce

1. Přihlaste se jako správce [Azure klasické portálu](https://manage.windowsazure.com/).
2. Přejděte ke službě Active Directory a vyberte složku, kde jste povolili aplikaci Proxy.

    ![Služba Active Directory - ikonu](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klikněte na kartu **aplikace** a potom klikněte na tlačítko **Přidat** v dolní části obrazovky

    ![Přidat aplikaci](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Vyberte **publikovat aplikace, která bude přístupný z mimo vaši síť**.

    ![Publikování aplikace, která bude přístupný z mimo vaši síť](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Zadejte následující informace o aplikaci:

    - **Název**: popisný název aplikace. Musí být v rámci adresáře jedinečná.
    - **Interní adresu URL**: na adresu, kterou konektoru aplikace Proxy používá pro přístup k aplikaci z uvnitř privátní sítě. Zadejte konkrétní cestu na serveru back-end publikovat při nepublikované zbytek serveru. Tímto způsobem můžete publikovat různých webů na stejný server a udělit každý z nich svůj vlastní název a přístup pravidla.

        > [AZURE.TIP] Pokud publikujete cesty, zkontrolujte, že zahrnuje všechny potřebné obrázky, skripty a stylů pro aplikace. Třeba když aplikace na https://yourapp/app a používá obrázky c:\ https://yourapp/media, pak by měl publikujete https://yourapp/ jako cestu.

    - **Metoda používající**: jak aplikace Proxy ověří uživatele před udělovala přístup k aplikaci. V rozevírací nabídce vyberte jednu z možností.

        - Azure Active Directory: Proxy aplikace přesměruje uživatelům přihlaste Azure AD, které ověří pro adresář a aplikace odpovídající oprávnění.
        - Předávací: Uživatelé nemají ověření pro přístup k aplikaci.

    ![Vlastnosti aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Dokončete průvodce klikněte na značku zaškrtnutí v dolní části obrazovky. Aplikace se teď definuje v Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Přiřazení uživatelů a skupin do aplikace

Aby uživatelů pro přístup k publikované aplikaci musíte přiřadit jednotlivě nebo ve skupinách. (Nezapomeňte přiřadit přístup, příliš.) Při této akci musí mít licenci pro základní Azure nebo vyšší každý uživatel. Jednotlivě nebo skupinám můžete přiřadit licence. Další informace najdete v tématu [přiřazení uživatelů do aplikace](active-directory-applications-guiding-developers-assigning-users.md) . 

K aplikacím, které vyžadují předběžné ověření udělí oprávnění k používání aplikace. K aplikacím, které nevyžadují předběžné ověřování uživatelů lze pořád přiřadit k aplikaci tak, aby se zobrazil v jejich seznamu aplikací, jako je Moje aplikace.

1. Po dokončení průvodce přidat aplikaci, zobrazí se stránka rychlý Start aplikace. Pokud chcete spravovat, kdo má přístup k aplikaci, vyberte **Uživatelé a skupiny**.

    ![Úvodní Proxy aplikací přiřadit uživatelům - snímek](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Vyhledejte určité skupiny v adresáři nebo zobrazení všech uživatelů. Pokud chcete zobrazit výsledky hledání, klikněte na značku zaškrtnutí.

    ![Vyhledejte skupinám nebo uživatelům - snímek](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Vyberte všechny uživatele nebo skupinu, kterou chcete přiřadit k této aplikaci a klikněte na **přiřadit**. Zobrazí se výzva k potvrzení akce.

> [AZURE.NOTE] Aplikace integrované ověřování systému Windows můžete přiřadit pouze uživatelé a skupiny, které se synchronizují z Active Directory vaší místní. Uživatelé, kteří Přihlaste se pomocí účtu Microsoft a hostů nelze přiřadit k aplikacím publikovaných pomocí Azure Active Directory aplikace Proxy. Ujistěte se, přihlaste se pomocí přihlašovacích údajů, které jsou součástí tu samou doménu aplikace, které publikujete uživatelů.

## <a name="test-your-published-application"></a>Testovat publikované aplikace

Po publikování aplikace testujete ji tak, že přejdete na adresu URL, kterou jste publikovali. Ujistěte se, že je můžete používat, že vykreslí správně a že všechno funguje očekávaným způsobem. Pokud máte potíže s nebo objeví chybová zpráva, zkuste [Průvodce pro řešení](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurace aplikace

Můžete změnit publikované aplikace nebo Upřesnit možnosti na stránce Konfigurovat nastavení. Na této stránce můžete přizpůsobit aplikace Změna názvu a loga nahrávání. Můžete spravovat pravidla přístupu jako předběžné ověření metoda nebo vícefaktorové ověřování.

![Upřesnit](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Po publikování aplikací pomocí Azure Active Directory aplikace proxy server, objeví se v seznamu aplikací v Azure AD a je tam můžete spravovat.

Pokud vypnete Proxy aplikace služby po publikování aplikací, jsou už přístupné z mimo privátní sítě. Tato aplikace neodstraní.

Zobrazení aplikace a ujistěte se, že je dostupný, poklikejte na název aplikace. Pokud je zakázaná služba Proxy aplikace a aplikace není k dispozici, se zobrazí zpráva s upozorněním v horní části obrazovky.

Odstranění aplikace, vyberte aplikaci v seznamu a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

- [Publikování aplikací pomocí vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)
- [Práce s aplikacemi přehled pohledávek](active-directory-application-proxy-claims-aware-apps.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
