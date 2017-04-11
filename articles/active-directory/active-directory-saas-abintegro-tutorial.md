<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Abintegro | Microsoft Azure" 
    description="Naučte se používat Abintegro s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-abintegro"></a>Kurz: Azure Active Directory integrace s Abintegro

Cílem tohoto kurzu je zobrazit integrace Azure a Abintegro.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Abintegro jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Abintegro (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Abintegro.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Abintegro
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-abintegro-tutorial/IC790076.png "Scénář")
##<a name="enabling-the-application-integration-for-abintegro"></a>Povolení integrace aplikací pro Abintegro

Cílem tento oddíl je přehledu jak povolit integraci aplikace Abintegro.

###<a name="to-enable-the-application-integration-for-abintegro-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Abintegro, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-abintegro-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-abintegro-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-abintegro-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-abintegro-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **abintegro**.

    ![Galerie aplikace] (./media/active-directory-saas-abintegro-tutorial/IC790077.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Abintegro**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Abintegro] (./media/active-directory-saas-abintegro-tutorial/IC790078.png "Abintegro")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Abintegro s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Abintegro** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-abintegro-tutorial/IC790079.png "Konfigurace jednoho přihlášení")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Abintegro** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-abintegro-tutorial/IC790080.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Abintegro na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k Abintegro (například: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-abintegro-tutorial/IC790081.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Abintegro** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-abintegro-tutorial/IC790082.png "Konfigurace jednoho přihlášení")

5.  Odešlete metadatafile Abintegro tým podpory.

    >[AZURE.NOTE] Konfigurace přihlašování musí vykonat Abintegro tým podpory. Zobrazí se vám oznámení hned po dokončení konfigurace.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-abintegro-tutorial/IC790083.png "Konfigurace jednoho přihlášení")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Neexistuje žádné akce položky konfigurace uživatele zřizování Abintegro.  
Při přiřazené uživatel se pokusí přihlásit Abintegro pomocí panelu přístup, Abintegro kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Abintegro.
##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-abintegro-perform-the-following-steps"></a>Přiřazení uživatelů k Abintegro, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Abintegro **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-abintegro-tutorial/IC790084.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-abintegro-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
