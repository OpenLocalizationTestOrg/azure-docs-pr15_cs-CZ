<properties 
    pageTitle="Kurz: Azure Active Directory integrace s ScreenSteps | Microsoft Azure" 
    description="Naučte se používat ScreenSteps s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Kurz: Azure Active Directory integrace s ScreenSteps
  
Cílem tohoto kurzu je zobrazit integrace Azure a ScreenSteps.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   ScreenSteps klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti ScreenSteps (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili ScreenSteps.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro ScreenSteps
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scénář")
##<a name="enabling-the-application-integration-for-screensteps"></a>Povolení integrace aplikací pro ScreenSteps
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace ScreenSteps.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ScreenSteps, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ScreenSteps**.

    ![Galerie aplikace] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ScreenSteps**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ScreenSteps s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **ScreenSteps** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ScreenSteps** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **ScreenSteps Sign v adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. ScreenSteps.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v ScreenSteps** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti ScreenSteps jako správce.

6.  Klikněte na **Správa účtů**.

    ![Správa účtů] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Správa účtů")

7.  Klikněte na **vzdálené ověření**.

    ![Vzdálené ověřování] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Vzdálené ověřování")

8.  Klikněte na **Vytvořit ověřování koncového bodu**.

    ![Vzdálené ověřování] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Vzdálené ověřování")

9.  V části **Vytvoření koncového ověřování** proveďte následující kroky:

    ![Vytvoření koncového bodu ověřování] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Vytvoření koncového bodu ověřování")

    1.  Do textového pole **název** zadejte název.
    2.  V seznamu **režim** vyberte **SAML**.
    3.  Klikněte na **vytvořit**.

10. V části **Ověřování koncovém** proveďte následující kroky:

    ![Koncový bod vzdálené ověřování] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Koncový bod vzdálené ověřování")

    1.  Na portálu na stránce **konfigurovat jednotné přihlašování v ScreenSteps** Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL vzdálené přihlášení** .
    2.  Na portálu na stránce **konfigurovat jednotné přihlašování v ScreenSteps** Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **odhlaste se adresa URL** .
    3.  Klikněte na tlačítko **Zvolit soubor**a pak nahrajte stažený certifikát.
    4.  Klikněte na **Aktualizovat**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit **ScreenSteps**, musí být zřízení do **ScreenSteps**.  
V případě **ScreenSteps**zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Zřízení uživatelského účtu ScreenSteps, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **ScreenSteps** .

2.  Klikněte na **Správa účtů**.

    ![Správa účtů] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Správa účtů")

3.  Klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Uživatelé")

4.  Klikněte na **Vytvořit uživatele**.

    ![Všichni uživatelé] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Všichni uživatelé")

5.  V seznamu **Role uživatele** vyberte role pro vaše uživatele.

6.  V části Role uživatele zadejte**"křestního jména**, **Příjmení**, **e-mailu**, **přihlášení**, **heslo** a **Potvrzení hesla"**platného AAD účtu, který chcete poskytování do související textová pole.

    ![Nového uživatele] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Nového uživatele")

7.  V části skupiny vyberte **Ověřování skupiny SAML ()**a potom klikněte na **Vytvořit uživatele**.

    ![Skupiny] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Skupiny")

>[AZURE.NOTE]Můžete použít jakékoli další ScreenSteps uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ScreenSteps k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Přiřazení uživatelů k ScreenSteps, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **ScreenSteps **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Přiřazení uživatelů] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Přiřazení uživatelů")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).