<properties 
    pageTitle="Kurz: Azure Active Directory integrace s jarní CM | Microsoft Azure" 
    description="Naučte se používat jarní CM s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Kurz: Azure Active Directory integrace s jarní CM
  
Cílem tohoto kurzu je ukazují, jak nastavit jednotné přihlašování mezi Azure Active Directory a SpringCM.
  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   SpringCM jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure Active Directory uživatele, které jste přiřadili SpringCM budou moct jednotné přihlašování pomocí panelu AAD přístup.

1.  Povolení integrace aplikací pro SpringCM
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scénář")

##<a name="enabling-the-application-integration-for-springcm"></a>Povolení integrace aplikací pro SpringCM
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace SpringCM.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace SpringCM, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **SpringCM**.

    ![Galerie aplikace] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galerie aplikace")

7.  V podokně výsledků vyberte **SpringCM**a potom klikněte na **Dokončit** přidat aplikaci.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Tato část popisuje, jak povolit uživatelům ověřit SpringCM s účtem v Azure Active Directory federation podle protokolu SAML pomocí.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **SpringCM** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k SpringCM** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit SpringCM na adresu URL** do textového pole zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci SpringCM a klikněte na tlačítko **Další**. 

    Adresa URL aplikace je svoji adresu URL SpringCM klienta (například: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Konfigurace aplikace URL] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v SpringCM** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně do svého počítače.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Konfigurace jednoho přihlášení")

5.  V okně různých webového prohlížeče Přihlaste se k vašemu webu společnosti **SpringCM** jako správce.

6.  V nabídce v horní klikněte na **Přejít na**, klikněte na **Předvolby**a potom v části **Předvolby účtu** klikněte na **Jednotné přihlašování SAML**.

    ![Jednotné přihlašování SAML] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "Jednotné přihlašování SAML")

7.  V části Konfigurace zprostředkovatele identit proveďte následující kroky:

    ![Konfigurace zprostředkovatele identit] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Konfigurace zprostředkovatele identit")

    1.  Pokud chcete odeslat stažený certifikát Azure Active Directory, klikněte na **Vyberte Vystavitel certifikátu** nebo **Změnit Vystavitel certifikátu**.
    2.  Na portálu na stránce **konfigurovat jednotné přihlašování v SpringCM** Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vystavitel** .
    3.  Na portálu na stránce **konfigurovat jednotné přihlašování v SpringCM** Azure klasické zkopírujte její hodnotu **Singel adresy URL přihlašování služby** a potom je vložte do textového pole **Koncový bod, které iniciuje poskytovatele služeb (SP)** .
    4.  Jako **SAML povoleno**zaškrtněte políčko **Povolit**.
    5.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Konfigurace jednoho přihlášení")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure Active Directory do SpringCM přihlásit, musí být zřízení do SpringCM.  
V případě SpringCM zřizování se ručně naplánovaný úkol.

>[AZURE.NOTE] Další podrobnosti najdete v tématu [Vytvoření a úprava SpringCM uživatele](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Zřízení uživatelského účtu SpringCM, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **SpringCM** jako správce.

2.  Klikněte na **Přejít na**a potom klikněte na **Adresář**.

    ![Vytvoření uživatele] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Vytvoření uživatele")

3.  Klikněte na **Vytvořit uživatele**.

4.  Vyberte **roli**.

5.  Zaškrtněte políčko **Odeslat e-mailu aktivace**.

6.  Zadejte jméno, příjmení a e-mailovou adresu platné Azure Active Directory uživatelského účtu, který chcete ustanovení do související textová pole.

7.  Přidejte uživatele do **skupiny zabezpečení**.

8.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další SpringCM uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou SpringCM k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Přiřazení uživatelů k SpringCM, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **SpringCM** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).




