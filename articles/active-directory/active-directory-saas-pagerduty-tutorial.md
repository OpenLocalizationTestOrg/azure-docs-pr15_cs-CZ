<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Pagerduty | Microsoft Azure" 
    description="Naučte se používat Pagerduty s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Kurz: Azure Active Directory integrace s Pagerduty
  
Cílem tohoto kurzu je zobrazit integrace Azure a Pagerduty.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Pagerduty klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Pagerduty (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Pagerduty.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Pagerduty
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scénář")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Povolení integrace aplikací pro Pagerduty
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Pagerduty.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Pagerduty, proveďte následující kroky:

1.  Na portálu Správa Azure v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Pagerduty**.

    ![Galerie aplikace] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Pagerduty**a potom klikněte na **Dokončit** přidat aplikaci.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Pagerduty s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Pagerduty** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Pagerduty** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Pagerduty Sign v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Pagerduty.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace url] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Konfigurace aplikace url")

4.  Na stránce **konfigurovat jednotné přihlašování v Pagerduty** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Pagerduty jako správce.

6.  V nabídce nahoře klikněte na **Nastavení účtu**.

    ![Nastavení účtu] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Nastavení účtu")

7.  Klikněte na **jednotné přihlašování**.

    ![Jednotné přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Jednotné přihlašování")

8.  Na stránce povolit jednotné přihlašování (SSO) proveďte následující kroky:

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Povolení jednotné přihlašování")

    1.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikátu**
    3.  Na portálu na **konfigurovat jednotné přihlašování v Pagerduty** dialog stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v Pagerduty** dialog stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    5.  Vyberte **Zapnout jednotného přihlašování**.
    6.  Klikněte na **Uložit změny**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Pagerduty, musí být zřízení do Pagerduty.  
V případě Pagerduty zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Pagerduty** .

2.  V nabídce na horní klikněte na **uživatele**.

3.  Klikněte na **Přidat uživatele**.

    ![Přidání uživatelů] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Přidání uživatelů")

4.  V dialogu **Pozvat váš tým** zadejte požadované **jméno a příjmení** a **e-mailovou** adresu Azure AD uživatele, kterého chcete zřízení, klikněte na tlačítko **Přidat**a potom klikněte na **Odeslat vyzývá**.

    ![Pozvat váš tým] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Pozvat váš tým")

    >[AZURE.NOTE] Všechny přidané uživatelé dostanou pozvání k vytvoření účtu PagerDuty.

>[AZURE.NOTE] Můžete použít jakékoli další Pagerduty uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Pagerduty k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Přiřazení uživatelů k Pagerduty, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Pagerduty **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).