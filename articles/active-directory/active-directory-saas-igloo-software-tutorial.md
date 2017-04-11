<properties 
    pageTitle="Kurz: Azure Active Directory integrace Igloo softwarem | Microsoft Azure" 
    description="Naučte se používat Igloo Software s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Kurz: Azure Active Directory integrace Igloo softwarem
  
Cílem tohoto kurzu je zobrazit integrace Azure a Igloo softwaru.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   [Igloo Software](http://www.igloosoftware.com/) jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Igloo Software (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené Igloo Software.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací Igloo softwaru
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scénář")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Povolení integrace aplikací Igloo softwaru
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Igloo softwaru.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Igloo softwaru, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Igloo Software**.

    ![Galerie aplikace] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Igloo Software**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Igloo softwaru s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi centrální plochy.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Igloo Software** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k softwaru Igloo** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD jednotné přihlášení] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD jednotné přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Igloo Software znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://company.igloocommunities.com/?signin*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Igloo softwaru** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu Igloo softwaru společnosti.

6.  Otevřete **Ovládací panely**.

    ![Ovládací panely] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Ovládací panely")

7.  Na kartě **členství** klikněte na **Symbol v nastavení**.

    ![V dialogovém okně Nastavení podpisu] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "V dialogovém okně Nastavení podpisu")

8.  V části SAML konfigurace klikněte na **Konfigurovat SAML ověření**.

    ![Konfigurace SAML] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "Konfigurace SAML")

9.  V části **Obecné konfigurace** proveďte následující kroky:

    ![Obecné konfigurace] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Obecné konfigurace")

    1.  Do textového pole **Název připojení** zadejte vlastní název konfiguraci.
    2.  Na portálu na **konfigurovat jednotné přihlašování v článku Software Igloo** dialog stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **IdP přihlašovací adresa URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v článku Software Igloo** dialog stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **URL odhlášení IdP** .
    4.  Jako **odpověď odhlásit a typ žádosti HTTP**vyberte **Publikovat**.
    5.  Vytvoření textového souboru z stažený certifikát.
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Odebrání řádku prvního a posledního řádku z verze souboru text vašeho certifikátu, zkopírujte zbývajícího textu certifikát a potom je vložte do textového pole **Certifikát veřejného** .

10. **Konfigurace ověřování a odpovědí**proveďte následující kroky:

    ![Konfigurace ověřování a odpovědi] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Konfigurace ověřování a odpovědi")

    1.  Jako **Poskytovatele Identity**vyberte **Microsoft služby AD FS**.
    2.  Jako **Identifikátor URI typu**vyberte **E-mailovou adresu**.
    3.  **Atribut e-mailu** do textového pole zadejte **emailaddress**.
    4.  Do textového pole **Atribut křestní jméno** zadejte **givenName (jméno)**.
    5.  **Poslední atribut název** do textového pole zadejte **Surname (příjmení)**.

11. Preform následující kroky a dokončete konfiguraci:

    ![Vytvoření uživatele na přihlašovací] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Vytvoření uživatele na přihlašovací")

    1.  **Vytvoření uživatele na přihlašovací**vyberte **vytvořit nového uživatele do vašeho webu při přihlášení**.
    2.  Jako, **Přihlaste se nastavení**vyberte **tlačítko SAML pomocí na obrazovce "Přihlásit"**.
    3.  Klikněte na **Uložit**.

12. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Igloo softwaru.  
Při přiřazené uživatel se pokusí přihlásit Igloo Software pomocí panelu přístup, Igloo Software kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky Igloo softwarem.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Přiřazení uživatelů k Igloo Software, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce **Igloo Software **aplikace integrace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).