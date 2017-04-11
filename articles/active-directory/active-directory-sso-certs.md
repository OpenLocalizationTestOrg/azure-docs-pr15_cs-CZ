<properties
    pageTitle="Jak spravovat federace certifikáty v Azure AD | Microsoft Azure"
    description="Zjistěte, jak upravit datum vypršení platnosti pro federaci certifikáty a způsobu jejich obnovení certifikáty, které brzy vyprší platnost."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Správa certifikátů federované jednotné přihlašování v Azure Active Directory

Tento článek popisuje běžné otázky týkající se potvrzení, že Azure Active Directory vytvoří za účelem vytvoření federované jednotné přihlašování (SSO) k aplikacím SaaS.

Tento článek platí jenom k aplikacím, které jsou nakonfigurované používání **Azure AD jednotné přihlašování**, jak je vidět v následujícím příkladu:

![Azure AD jednotné přihlášení](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Jak upravit datum vypršení platnosti certifikátu federace

Ve výchozím nastavení jsou nastavené vyprší po dvou letech certifikáty. Zvolte jiné platnosti certifikátu podle následujících pokynů. Součástí snímky obrazovek pomocí služby Salesforce z příkladu, ale tento postup můžete použít k aplikaci federované SaaS.

1. V Azure Active Directory na stránce Rychlý Start aplikace, klikněte na **Konfigurovat jednotného přihlašování**.

    ![Otevřete Průvodce konfigurací jednotné přihlašování.](./media/active-directory-sso-certs/config-sso.png)

2. Vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

3. Zadejte adresu **URL přihlašování** aplikace a zaškrtněte políčko **Konfigurovat certifikát používaný pro federované jednotného přihlašování**. Klepněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Na další stránce vyberte **Generovat nový certifikát**a vyberte, jak dlouho certifikát, který má být platná pro. Klepněte na tlačítko **Další**.

    ![Generovat nový certifikát.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Potom klikněte na **Stáhnout certifikát**. Informace o nahrání certifikátu do určité SaaS aplikaci, klikněte na **Pokyny ke konfiguraci zobrazení**.

    ![Stáhněte si a pak nahrajte certifikátu](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Certifikát nepovolí, dokud nebude zaškrtněte políčko potvrzení v dolní části dialogového okna a potom klepněte na tlačítko Odeslat.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Jak prodloužit předplatné certifikát, který bude brzy vyprší platnost

Obnovení kroky ukázáno v následujícím příkladu by měl v ideálním případě výsledkem žádné významné výpadek služeb pro uživatele. Snímky obrazovek použít v této části funkce služby Salesforce jako příklad, ale tento postup můžete použít k aplikaci federované SaaS.

1. V Azure Active Directory na stránce Rychlý Start aplikace, klikněte na **Konfigurovat jednotného přihlašování**.

    ![Otevřít Průvodce konfigurací jednotné přihlašování](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Na první stránce dialogu **Azure AD jednotné přihlašování** by již být vybrána, takže klikněte na tlačítko **Další**.

3. Na druhé stránce zaškrtněte políčko **Konfigurovat certifikát používaný pro federované jednotné přihlašování**. Klepněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Na další stránce vyberte **Generovat nový certifikát**a vyberte, jak dlouho nový certifikát, který má být platná pro. Klepněte na tlačítko **Další**.

    ![Generovat nový certifikát.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klikněte na **Stáhnout certifikát**. Chcete úspěšně rewnew certifikát, musíte provést následující dva kroky:

    - Nahrajte nový certifikát aplikaci SaaS jednotné přihlašování obrazovku. Zjistěte, jak to udělat pro konkrétní aplikaci SaaS, klikněte na **Pokyny ke konfiguraci zobrazení**.

    - V Azure AD zaškrtněte políčko potvrzení v dolní části dialogu povolit nový certifikát a klikněte na tlačítko **Další** můžou odeslat.

    > [AZURE.IMPORTANT] Jednotné přihlašování k aplikaci přestane být platná okamžiku, kdy některá z těchto dvou kroků dokončit, ale bude povoleno opakujte druhý krok po ukončení. Proto minimalizovat výpadek služeb, prosím připravte k provedení těchto dvou kroků v rámci krátké časový úsek od sebe.

    ![Stáhněte si a pak nahrajte certifikátu](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Poradce při potížích na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)
