<properties 
    pageTitle="Kurz: Azure Active Directory integrace s TimeOffManager | Microsoft Azure" 
    description="Naučte se používat TimeOffManager s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Kurz: Azure Active Directory integrace s TimeOffManager
  
Cílem tohoto kurzu je integrace Azure a TimeOffManager zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   TimeOffManager jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti TimeOffManager (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili TimeOffManager.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro TimeOffManager
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Scénář")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>Povolení integrace aplikací pro TimeOffManager
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace TimeOffManager.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace TimeOffManager, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **TimeOffManager**.

    ![Galerie aplikace] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Galerie aplikace")

7.  V podokně výsledků vyberte **TimeOffManager**a potom klikněte na **Dokončit** přidat aplikaci.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit TimeOffManager s účtem v Azure AD pomocí federace na základě SAML protokolu.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi TimeOffManager.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **TimeOffManager** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k TimeOffManager** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **TimeOffManager odpověď URL** zadejte adresu URL svého TimeOffManager AssertionConsumerService (například: "*Příklad: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id = IC34216*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "Konfigurace aplikace URL")

    Získat adresu URL odpověď z TimeOffManager jednotné přihlašování nastavení stránky.

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Nastavení jednotného přihlašování")

4.  Na stránce **konfigurovat jednotné přihlašování v TimeOffManager** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti TimeOffManager jako správce.

6.  Přejděte na **účtu \> účet možnosti \> jedním přihlašování nastavení**.

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Nastavení jednotného přihlašování")

7.  V části **Nastavení jednotného přihlašování** proveďte následující kroky:

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Nastavení jednotného přihlašování")

    na.  Vytvoření souboru **kódování Base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    b.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte do textového pole **Certifikátu X.509** celý certifikát.
    
    c.  Na portálu na **konfigurovat jednotné přihlašování v TimeOffManager** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Idp vydavatel** .
    
    d.  Na portálu na **konfigurovat jednotné přihlašování v TimeOffManager** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL IdP koncového bodu** .
    
    e.  Jako **Vynutit SAML**vyberte **Ne**.
    

    f.  Jako **Automatické vytváření uživatelům**vyberte **Ano**.
    
    g.  Na portálu na **konfigurovat jednotné přihlašování v TimeOffManager** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    
    h.  Klikněte na **Uložit změny**.

8.  Na portálu na **konfigurovat jednotné přihlašování v TimeOffManager** dialogové okno stránce Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Konfigurace jednotného přihlašování")

9.  V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Atributy")

10. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![SAML tokenu atributy] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "SAML tokenu atributy")

  	|Název atributu|Hodnota atributu|
  	|---|---|
  	|E-mailu|User.Mail|
  	|Jméno|User.givenName|
  	|Příjmení|User.Surname|

    na.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.

    b.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.

    c.  **Hodnota atributu** do textového pole vyberte hodnotu atributu zobrazí pro tento řádek.

    d.  Klikněte na **dokončení**.

11. Klikněte na **použít změny**.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit TimeOffManager, musí být zřízení k TimeOffManager.  
TimeOffManager podporuje jenom čas zřizování uživatelů. Existuje žádné úkol za vás.  
Uživatelé se automaticky přidají při prvním přihlášení pomocí jednotného přihlášení na.

>[AZURE.NOTE] Můžete použít jakékoli další TimeOffManager uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou TimeOffManager k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Přiřazení uživatelů k TimeOffManager, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **TimeOffManager** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
