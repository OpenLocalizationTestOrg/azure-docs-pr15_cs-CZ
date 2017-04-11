<properties 
    pageTitle="Kurz: Azure Active Directory integrace s 15Five | Microsoft Azure" 
    description="Naučte se používat 15Five s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Kurz: Azure Active Directory integrace s 15Five

Cílem tohoto kurzu je zobrazit integrace Azure a 15Five. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   15Five jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti 15Five (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili 15Five.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro 15Five
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-15five-tutorial/IC784667.png "Scénář")
##<a name="enabling-the-application-integration-for-15five"></a>Povolení integrace aplikací pro 15Five

Cílem tento oddíl je přehledu jak povolit integraci aplikace 15Five.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace 15Five, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-15five-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-15five-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-15five-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **15Five**.

    ![Galerie aplikace] (./media/active-directory-saas-15five-tutorial/IC784668.png "Galerie aplikace")

7.  V podokně výsledků vyberte **15Five**a potom klikněte na **Dokončit** přidat aplikaci.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit 15Five s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **15Five** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-15five-tutorial/IC784670.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k 15Five** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-15five-tutorial/IC784671.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **15Five Sign v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://company.15Five.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-15five-tutorial/IC784672.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v 15Five** klikněte na tlačítko **Stáhnout metadat**a nastavíte přesměrování soubor metadat pro tým podpory 15Five.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-15five-tutorial/IC784673.png "Konfigurace jednotného přihlašování")

    >[AZURE.NOTE] Jednotné přihlašování, musí být povolené tým podpory 15Five.

5.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-15five-tutorial/IC784674.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit 15Five, musí být zřízení do 15Five.  
V případě 15Five zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **15Five** jako správce.

2.  Přejděte na **Správa společnosti**.

    ![Správa společnosti] (./media/active-directory-saas-15five-tutorial/IC784675.png "Správa společnosti")

3.  Přejděte na **lidé \> přidejte lidi**.

    ![Lidé] (./media/active-directory-saas-15five-tutorial/IC784676.png "Lidé")

4.  V části Přidat novou osobu proveďte následující kroky:

    ![Přidat novou osobu] (./media/active-directory-saas-15five-tutorial/IC784677.png "Přidat novou osobu")

    1.  Zadejte **jméno**, **Příjmení**, **název**, **e-mailovou adresu** platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na tlačítko **Hotovo**.

    >[AZURE.NOTE] Majitele účtu Azure AD, dostanou e-mailu a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další 15Five uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou 15Five k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Přiřazení uživatelů k 15Five, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **15Five **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-15five-tutorial/IC784678.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-15five-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
