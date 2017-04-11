<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Workday | Microsoft Azure" 
    description="Informace o použití Workday s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Kurz: Azure Active Directory integrace s Workday
  
Cílem tohoto kurzu je zobrazit integrace Azure a pracovní den. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Ke klientovi Workday
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Workday
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Konfigurace zřizování uživatelů

![Scénář] (./media/active-directory-saas-workday-tutorial/IC782919.png "Scénář")

##<a name="enabling-the-application-integration-for-workday"></a>Povolení integrace aplikací pro Workday
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace služby Salesforce.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Postup povolení integrace aplikace pro Workday, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-workday-tutorial/IC700994.png "Aplikace")

4.  Otevřete **Aplikaci Galerie**, klikněte na **Přidat aplikaci**a potom klikněte na **Přidat aplikaci pro svoji organizaci používat**.

    ![Co chcete udělat?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Co chcete udělat?")

5.  Do **vyhledávacího pole**zadejte **pracovního dne**.

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701021.png "WORKDAY")

6.  V podokně výsledků vyberte **Workday**a potom klikněte na **Dokončit** přidat aplikaci.

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701022.png "WORKDAY")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Workday s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit certifikát zakódovaný základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na stránce integrace **Workday** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC782920.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Workday** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC782921.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-workday-tutorial/IC782957.png "Konfigurace aplikace URL")

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané uživatelů se přihlásit k Workday pomocí následujícího vzorce:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  V textovém poli **URL odpovědět pracovního dne** zadejte adresu URL odpovědět Workday pomocí následujícího vzorce:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Adresa URL odpověď musí mít domény (například: www, wd2, wd3, wd3 impl, wd5, wd5 impl). 
    >Použití nějak "*http://www.myworkday.com*" funguje, ale nikoli "*http://myworkday.com*". 
 
4.  Na stránce **konfigurovat jednotné přihlašování v Workday** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC782922.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Workday jako správce.

6.  Přejděte na **nabídka \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Přejděte na **stránku Správa účtu**.

    ![Správa účtu] (./media/active-directory-saas-workday-tutorial/IC782924.png "Správa účtu")

8.  Přejděte na **Upravit nastavení klienta – zabezpečení**.

    ![Úprava klienta zabezpečení] (./media/active-directory-saas-workday-tutorial/IC782925.png "Úprava klienta zabezpečení")

9.  V části **Přesměrování adresy URL** proveďte následující kroky:

    ![Přesměrování adresy URL] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Přesměrování adresy URL")

    na. Klikněte na **Přidat řádek**.

    b. V textovém poli **Přihlášení přesměrování adresy URL** a textové pole **Adresa URL přesměrování mobilního** zadejte adresu **URL klienta Workday** jste zadali na stránce **Konfigurace adresa URL aplikace** klasické portálu Azure.
    
    c. Na portálu na **konfigurovat jednotné přihlašování v Workday** dialogové okno stránce Azure klasické zkopírujte **Jedné adresy URL služby Sign-Out**a potom je vložte do textového pole **Adresa URL přesměrování odhlásit** .

    d.  Do textového pole **prostředí** zadejte název prostředí.  


    >[AZURE.NOTE] Hodnota atributu prostředí je svázané se hodnota adresu klienta:
    >
    >-   Pokud název domény klienta Workday URL začíná impl (například: *https://impl.workday.com/\<klienta\>/login-saml2.htmld*), **prostředí** atributu musí být nastavena na implementaci.
    >-   Pokud název domény začíná něco jiného, budete muset kontaktovat Workday získání odpovídající hodnoty **prostředí** .

10. V části **Nastavení SAML** proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-workday-tutorial/IC782926.png "Nastavení SAML")

    na.  Zaškrtněte políčko **Povolit SAML ověřování**.

    b.  Klikněte na **Přidat řádek**.

11. V části poskytovatelů identit SAML proveďte následující kroky:

    ![Zprostředkovatele identit SAML] (./media/active-directory-saas-workday-tutorial/IC7829271.png "Zprostředkovatele identit SAML")

    na. Do textového pole název poskytovatele Identity zadejte název poskytovatele (například: *SPInitiatedSSO*).

    b. Na portálu na **konfigurovat jednotné přihlašování v Workday** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **Vydavatel** .

    c. Zaškrtněte políčko **Povolit Workday Initialted odhlásit**.

    d. Na portálu na **konfigurovat jednotné přihlašování v Workday** dialogové okno stránce Azure klasické zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Adresa URL požádat o odhlásit** .


    e. Klikněte na **Certifikát veřejného klíč identita poskytovatele**a potom klikněte na **vytvořit**. 

    ![Vytvoření] (./media/active-directory-saas-workday-tutorial/IC782928.png "Vytvoření")

    f. Klikněte na **vytvořit x509 veřejným klíčem**. 
        
    ![Vytvoření] (./media/active-directory-saas-workday-tutorial/IC782929.png "Vytvoření")


1. V části **Zobrazení x509 veřejným klíčem** proveďte následující kroky: 

    ![Zobrazení x509 veřejný klíč] (./media/active-directory-saas-workday-tutorial/IC782930.png "Zobrazení x509 veřejný klíč") 

    na. Do textového pole **název** zadejte název vašeho certifikátu (například: *OOP\_SP*).
        
    b. Do textového pole **Platné od** zadejte platný atribut hodnotou certifikátu.
    
    c.  **Platnost do** textového pole zadejte platný hodnotě atributu vašeho certifikátu.
        
    >[AZURE.NOTE] Můžete získat od data a platné datum, od stažený certifikát poklepáním platná. Data jsou uvedené v části na kartu **Podrobnosti** .

    d. Vytvoření souboru **kódování Base-64** z stažený certifikát.  

    >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    e.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku a zkopírujte obsah od něj.
    
    f.  Do textového pole **certifikát** vložení obsahu schránky.
    
    g.  Klikněte na **OK**.

12.  Proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "Konfigurace jednotného přihlašování")

    na.  Povolit **x509 soukromý klíč pár**.

    b.  Do textového pole **ID poskytovatelem služby** zadejte **http://www.workday.com**.

    c.  Vyberte **Povolit SP spuštěná SAML ověřování**.

    d.  Na portálu na **konfigurovat jednotné přihlašování v Workday** dialogové okno stránce Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Adresy URL služby Jednotné přihlašování IdP** .
     
    e. Vyberte **není Uprostřed zúžené požadavek inicializovaný SP ověření**.

    f. Jako **Požádat o podpis metody ověřování**vyberte **SHA256**. 
        
    ![Ověřování požádat o podpis metody] (./media/active-directory-saas-workday-tutorial/IC782932.png "Ověřování požádat o podpis metody") 
 
    g. Klikněte na **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Na portálu na stránce **konfigurovat jednotné přihlašování v Workday** Azure klasické klikněte na **Další**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC782934.png "Konfigurace jednotného přihlašování")

13. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Konfigurace jednotného přihlašování")



##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Získat testovací uživatelské jméno zřízení do pracovního dne, budete muset kontaktovat tým podpory pracovního dne.  
Workday tým podpory pro vytvoření uživatele.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Přiřazení uživatelů k Workday, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Workday **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-workday-tutorial/IC782935.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-workday-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).