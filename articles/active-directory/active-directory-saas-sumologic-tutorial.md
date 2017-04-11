<properties 
    pageTitle="Kurz: Azure Active Directory integrace s SumoLogic | Microsoft Azure" 
    description="Naučte se používat SumoLogic s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Kurz: Azure Active Directory integrace s SumoLogic
  
Cílem tohoto kurzu je zobrazit integrace Azure a SumoLogic.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   SumoLogic klienta
  
Po dokončení tohoto kurzu, uživatelům Azure AD jste přiřadili SumoLogicwill být moct jednoho přihlásit do aplikace na vaše SumoLogic společnosti webu (service provider, které iniciuje přihlášení) nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro SumoLogic
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scénář")

##<a name="enabling-the-application-integration-for-sumologic"></a>Povolení integrace aplikací pro SumoLogic
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace SumoLogic.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace SumoLogic, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **sumologic**.

    ![Galerie aplikace] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Galerie aplikace")

7.  V podokně výsledků vyberte **SumoLogic**a potom klikněte na **Dokončit** přidat aplikaci.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit SumoLogic s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 vaší SumoLogictenant.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **SumoLogic** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k SumoLogic** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **SumoLogic znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. SumoLogic.com*"a potom na tlačítko **Další**.

    ![Konfigurace aoo URL] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfigurace aoo URL")

4.  Na stránce **konfigurovat jednotné přihlašování v SumoLogic** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti SumoLogic jako správce.

6.  Přejděte na **Správa \> zabezpečení**.

    ![Správa] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Správa")

7.  Klikněte na tlačítko **SAML**.

    ![Nastavení globální zabezpečení] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Nastavení globální zabezpečení")

8.  Ze seznamu **Vyberte konfiguraci nebo vytvořte nový účet** vyberte **Azure AD**a potom klikněte na **Konfigurovat**.

    ![Konfigurace SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Konfigurace SAML 2.0")

9.  V dialogovém okně **Konfigurovat SAML 2.0** proveďte následující kroky:

    ![Konfigurace SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Konfigurace SAML 2.0")

    1.  Do textového pole **Název konfigurace** zadejte **Azure AD**.
    2.  Vyberte **režim ladění**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v SumoLogic** dialog stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vystavitel** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v SumoLogic** dialog stránce Azure klasické zkopírujte její hodnotu **Adresa URL požadovat ověření** a potom je vložte do textového pole **Adresa URL požádat o Authn** .
    5.  Vytvoření souboru **kódování Base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte do textového pole **Certifikátu X.509** celý certifikát.
    7.  Jako **E-mailu atribut**vyberte **použít SAML předmět**.
    8.  Vyberte **SP, které iniciuje konfigurace přihlášení**.
    9.  **Cesta k přihlášení** do textového pole zadejte **Azure**.
    10. Klikněte na **Uložit**.

10. Na portálu na **konfigurovat jednotné přihlašování v SumoLogic** dialog stránce Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit SumoLogic, musí být zřízení k SumoLogic.  
V případě SumoLogic zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **SumoLogic** .

2.  Přejděte na **Správa \> uživatelé**.

    ![Uživatelé] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Uživatelé")

3.  Klikněte na **Přidat**.

    ![Uživatelé] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Uživatelé")

4.  V dialogovém okně **Nový uživatel** proveďte následující kroky:

    ![Nového uživatele] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Nového uživatele")

    1.  Zadejte související informace Azure AD účet, který chcete k poskytování do textová pole **křestního jména**, **Příjmení** a **e-mailu** .
    2.  Vyberte roli.
    3.  **Stav**vyberte **aktivní**.
    4.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další SumoLogic uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou SumoLogic k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Přiřazení uživatelů k SumoLogic, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **SumoLogic** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).