<properties 
    pageTitle="Kurz: Azure Active Directory integrace s AppDynamics | Microsoft Azure" 
    description="Naučte se používat AppDynamics s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Kurz: Azure Active Directory integrace s AppDynamics

Cílem tohoto kurzu je zobrazit integrace Azure a AppDynamics. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   AppDynamics jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti AppDynamics (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili AppDynamics.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro AppDynamics
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scénář")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Povolení integrace aplikací pro AppDynamics

Cílem tento oddíl je přehledu jak povolit integraci aplikace AppDynamics.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace AppDynamics, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **AppDynamics**.

    ![Galerie aplikace] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Galerie aplikace")

7.  V podokně výsledků vyberte **AppDynamics**a potom klikněte na **Dokončit** přidat aplikaci.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit AppDynamics s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **AppDynamics** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Konfigurace jednoho přihlášení")

2.  Na stránce **jakým způsobem uživatelé přihlásit k AppDynamics** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit AppDynamics na adresu URL** do textového pole zadejte svoji adresu URL používané uživatelů k přihlašování k AppDynamics ("*https://companyname.saas.appdynamics.com*") a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v AppDynamics** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Konfigurace jednoho přihlášení")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti AppDynamics jako správce.

6.  Na panelu nástrojů na horní klikněte na **Nastavení**a potom klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Správa")

7.  Klikněte na kartu **Zprostředkovatele ověřování** .

    ![Zprostředkovatel ověřování] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Zprostředkovatel ověřování")

8.  V části **Ověřování poskytovatele** proveďte následující kroky:

    ![Konfigurace SAML] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "Konfigurace SAML")

    1.  Jako **Poskytovatele ověřování**vyberte **SAML**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v AppDynamics** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v AppDynamics** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    4.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **certifikátu**
    6.  Klikněte na **Uložit**.
        ![Uložení] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Uložení")

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Konfigurace jednoho přihlášení")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit AppDynamics, musí být zřízení do AppDynamics.  
V případě AppDynamics zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k webu společnosti AppDynamics jako správce.

2.  Přejděte na **Uživatelé**a potom klikněte na **+** otevřete dialogové okno **Vytvořit uživatele** .

    ![Uživatelé] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Uživatelé")

3.  V části **Vytvořit uživatele** proveďte následující kroky:

    ![Vytvoření uživatele] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Vytvoření uživatele")

    1.  Zadejte **uživatelské jméno**, **jméno**, **e-mailu**, **Nové heslo**, **Opakujte nové heslo** platný AAD účet, který chcete poskytování do související textová pole.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další AppDynamics uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou AppDynamics zřízení Azure AD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Přiřazení uživatelů k AppDynamics, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **AppDynamics **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
