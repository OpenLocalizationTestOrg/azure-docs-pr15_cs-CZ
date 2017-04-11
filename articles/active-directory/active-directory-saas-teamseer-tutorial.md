<properties 
    pageTitle="Kurz: Azure Active Directory integrace s TeamSeer | Microsoft Azure" 
    description="Naučte se používat TeamSeer s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Kurz: Azure Active Directory integrace s TeamSeer
  
Cílem tohoto kurzu je zobrazit integrace Azure a TeamSeer.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   TeamSeer klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti TeamSeer (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili TeamSeer.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro TeamSeer
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Scénář")

##<a name="enabling-the-application-integration-for-teamseer"></a>Povolení integrace aplikací pro TeamSeer
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace TeamSeer.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace TeamSeer, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **TeamSeer**.

    ![Galerie aplikace] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Galerie aplikace")

7.  V podokně výsledků vyberte **TeamSeer**a potom klikněte na **Dokončit** přidat aplikaci.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit TeamSeer s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **TeamSeer** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k TeamSeer** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **TeamSeer Sign v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*http://www.teamseer.com/companyid*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v TeamSeer** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti TeamSeer jako správce.

6.  Přejděte na **Správce HR**.

    ![HR správce] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR správce")

7.  Klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Nastavení")

8.  Klikněte na **nastavit SAML poskytovatele podrobnosti**.

    ![Nastavení SAML] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "Nastavení SAML")

9.  V části Podrobnosti poskytovatele SAML proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "Nastavení SAML")

    1.  Na portálu na **konfigurovat jednotné přihlašování v TeamSeer** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Adresa URL** .
    2.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **Certifikát veřejného IdP** .

10. Dokončete konfiguraci poskytovatele SAML proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "Nastavení SAML")

    1.  Do **Otestujte e-mailové adresy**zadejte e-mailovou adresu uživatele test.
    2.  **Vydavatel** do textového pole zadejte adresu URL Vystavitel poskytovatele služeb.
    3.  Klikněte na **Uložit**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit TeamSeer, musí být zřízení do ShiftPlanning.  
V případě TeamSeer zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **TeamSeer** jako správce.

2.  Proveďte následující kroky:

    ![HR správce] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR správce")

    1.  Přejděte na **HR správce \> uživatelé**.
    2.  Klikněte na tlačítko **Spustit Průvodce nového uživatele**.

3.  V části **Podrobnosti uživatele** proveďte následující kroky:

    ![Podrobné informace o uživateli] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Podrobné informace o uživateli")

    1.  Zadejte **křestního jména**, **Příjmení**, **uživatelské jméno (e-mailová adresa)** platného AAD účtu, který chcete poskytování do související textová pole.
    2.  Klikněte na tlačítko **Další**.

4.  Postupujte podle pokynů na obrazovce pro přidání nového uživatele a klikněte na tlačítko **Dokončit**.

>[AZURE.NOTE] Můžete použít jakékoli další TeamSeer uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou TeamSeer zřízení Azure AD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Přiřazení uživatelů k TeamSeer, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **TeamSeer **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).