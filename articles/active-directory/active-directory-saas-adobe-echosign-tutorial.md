<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Adobe EchoSign | Microsoft Azure" 
    description="Zjistěte, jak pomocí Adobe EchoSign Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Kurz: Azure Active Directory integrace s Adobe EchoSign

Cílem tohoto kurzu je zobrazit integrace Azure a Adobe EchoSign.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Adobe EchoSign jednoho podpisu povolené předplatného

Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili Adobe EchoSign bude moct jednotného přihlášení do aplikace na webu společnosti Adobe EchoSign (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integraci aplikace Adobe EchoSign
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scénář")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Povolení integraci aplikace Adobe EchoSign

Cílem tento oddíl je přehledu jak povolit integraci aplikace Adobe EchoSign.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Adobe EchoSign, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Adobe EchoSign**.

    ![Galerie aplikace] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Adobe EchoSign**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Adobe EchoSign s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **Adobe EchoSign** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k aplikaci Adobe EchoSign** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Adobe EchoSign znak na adresu URL** do textového pole zadejte adresu URL pomocí následujícího vzorce "*https://company.echosign.com/*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Adobe EchoSign** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Adobe EchoSign jako správce.

6.  V nabídce v horní klikněte na **účet**a potom v navigačním podokně na levé die, klikněte na **Nastavení SAML** v části **Nastavení účtu**.

    ![Účet] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Účet")

7.  V části Nastavení SAML proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "Nastavení SAML")

    1.  Jako **SAML režimu**vyberte možnost **SAML povinný**.
    2.  Vyberte **Povolit EchoSign účtu správcům Přihlaste se pomocí svých přihlašovacích údajů EchoSign**.
    3.  **Vytváření uživatelských**vyberte **automaticky přidávali uživatelé ověřeno SAML**.

8.  Přesunutí na, provedením následujících kroků:

    ![Nastavení SAML] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "Nastavení SAML")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Adobe EchoSign** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **IdP Entity ID** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v Adobe EchoSign** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **IdP přihlašovací adresa URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v Adobe EchoSign** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **URL IdP odhlásit** .
    4.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **IdP certifikát** 6.  Klikněte na **Uložit změny**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Adobe EchoSign, musí být zřízení do Adobe EchoSign.  
V případě Adobe EchoSign zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Adobe EchoSign** jako správce.

2.  V nabídce v horní klepněte na **účet**a potom v navigačním podokně na levé die, klikněte na **Uživatelé a skupiny**a pak klikněte na **vytvořit nového uživatele**.

    ![Účet] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Účet")

3.  V části **Vytvořit nového uživatele** proveďte následující kroky:

    ![Vytvoření uživatele] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Vytvoření uživatele")

    1.  Zadejte **E-mailovou adresu**, **Křestní jméno** a **Příjmení** platný AAD účet, který chcete vytvořit do související textová pole.
    2.  Klikněte na **Vytvořit uživatele**.

        >[AZURE.NOTE] Majitele účtu služby Azure Active Directory, dostanou e-mail obsahující odkaz před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Adobe EchoSign uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Adobe EchoSign k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Přiřazení uživatelů k Adobe EchoSign, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **Adobe EchoSign **klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
