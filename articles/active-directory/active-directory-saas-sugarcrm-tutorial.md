<properties 
    pageTitle="Kurz: Azure Active Directory integrace integrace s SugarCRM | Microsoft Azure" 
    description="Naučte se používat SugarCRM s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Kurz: Azure Active Directory integrace integrace s SugarCRM
  
Cílem tohoto kurzu je zobrazit integrace Azure a cukru CRM.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Cukru CRM jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili cukru CRM bude moct jednotného přihlášení do aplikace na webu společnosti cukru CRM (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro cukru CRM
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Scénář")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Povolení integrace aplikací pro cukru CRM
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace cukru CRM.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace cukru CRM, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Cukru CRM**.

    ![Galerie aplikace] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Cukru CRM**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Cukru CRM] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Cukru CRM")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit cukru CRM s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi cukru CRM.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Cukru CRM** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k cukru CRM** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Cukru CRM Sign na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace cukru CRM (například: "*http://company.sugarondemand.com*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v cukru CRM** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti cukru CRM jako správce.

6.  Přejděte na **Správce**.

    ![Správce] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Správce")

7.  V části **Správa** klikněte na **Správa hesel**.

    ![Správa] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Správa")

8.  Zaškrtněte políčko **Povolit SAML ověřování**.

    ![Správa] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Správa")

9.  V části **Ověřování SAML** proveďte následující kroky:

    ![SAML ověřování] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML ověřování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v cukru CRM** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v cukru CRM** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL po** .
    3.  Vytvoření souboru **kódování Base-64** z stažený certifikát.

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte do textového pole **Certifikátu X.509** celý certifikát.
    5.  Klikněte na **Uložit**.

10. Na portálu na **konfigurovat jednotné přihlašování v cukru CRM** dialogové okno stránce Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit cukru CRM, musí být zřízení cukru CRM.  
V případě cukru CRM zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Cukru CRM** jako správce.

2.  Přejděte na **Správce**.

    ![Správce] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Správce")

3.  V části **Správa** klikněte na **Správa uživatelů**.

    ![Správa] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Správa")

4.  Přejděte na **Uživatelé \> vytvořit nového uživatele**.

    ![Vytvoření nového uživatele] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Vytvoření nového uživatele")

5.  Na kartě **Profil uživatele** proveďte následující kroky:

    ![Nového uživatele] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Nového uživatele")

    1.  Zadejte uživatelské jméno, příjmení jméno a e-mailové adresa platné Azure Active Directory uživatele do související textová pole.

6.  **Stav**vyberte **aktivní**.

7.  Na kartě heslo proveďte následující kroky:

    ![Nového uživatele] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Nového uživatele")

    1.  Zadejte heslo do související textové pole.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další cukru CRM uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou cukru CRM k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Přiřazení uživatelů k cukru CRM, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Cukru CRM** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).