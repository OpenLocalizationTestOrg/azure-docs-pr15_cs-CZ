<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Clarizen | Microsoft Azure" 
    description="Naučte se používat Clarizen s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Kurz: Azure Active Directory integrace s Clarizen

Cílem tohoto kurzu je zobrazit integrace Azure a Clarizen.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Clarizen jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Clarizen (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Clarizen.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Clarizen
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scénář")
##<a name="enabling-the-application-integration-for-clarizen"></a>Povolení integrace aplikací pro Clarizen

Cílem tento oddíl je přehledu jak povolit integraci aplikace Clarizen.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Clarizen, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Clarizen**.

    ![Galerie aplikace] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Clarizen**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Clarizen s účtem v Azure AD pomocí federace na základě SAML protokolu.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Clarizen** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Clarizen** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Konfigurace jednotného přihlašování")

3.  Na stránce **konfigurovat jednotné přihlašování v Clarizen** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Konfigurace jednotného přihlašování")

4.  V okně různých webového prohlížeče, přihlaste se k webu společnosti **Clarizen** jako správce (například: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klikněte na své uživatelské jméno a potom klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Nastavení")

6.  Klikněte na kartu **Globálního nastavení** a pak vedle **Federované ověřování**, klikněte na **Upravit**.

    ![Globálních nastavení] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globálních nastavení")

7.  V dialogovém okně **Ověření federované** proveďte následující kroky:

    ![Federované ověřování] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federované ověřování")

    1.  Klikněte na **Uložit** nahrajete stažený certifikát.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Clarizen** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    3.  Na portálu na stránce dialogové okno **Konfigurovat jednoduchým odhlašovací na Clarizen** Azure klasické zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Adresa URL Sign-out** .
    4.  Vyberte **použít příspěvek**.
    5.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Clarizen, musí být zřízení do Clarizen.  
V případě Clarizen zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Clarizen** jako správce.

2.  Klikněte na **lidé**.

    ![Lidé] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Lidé")

3.  Klikněte na **Pozvat uživatele**.

    ![Pozvání uživatelů] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Pozvání uživatelů")

4.  Na stránce dialogu pozvat osoby proveďte následující kroky:

    ![Pozvat osoby] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Pozvat osoby")

    1.  **E-mailu** do textového pole zadejte e-mailovou adresu platný účet Azure Active Directory, který chcete poskytování.
    2.  Klikněte na **pozvánku**.

    >[AZURE.NOTE] Majitele účtu služby Azure Active Directory dostávat e-mailu a potvrďte svůj účet, než se aktivuje, klepnutím na odkaz.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Přiřazení uživatelů k Clarizen, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Clarizen **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
