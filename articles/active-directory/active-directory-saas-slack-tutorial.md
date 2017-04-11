<properties 
    pageTitle="Kurz: Azure Active Directory integrace s časovou rezervou | Microsoft Azure" 
    description="Naučte se používat časová rezerva s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Kurz: Azure Active Directory integrace s časová rezerva
  
Cílem tohoto kurzu je zobrazit integrace Azure a časová rezerva.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Časové rezervy jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu časové rezervy společnosti (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené časová rezerva.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro časová rezerva
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-slack-tutorial/IC794980.png "Scénář")

##<a name="enabling-the-application-integration-for-slack"></a>Povolení integrace aplikací pro časová rezerva
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro časové rezervy.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace časová rezerva, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-slack-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-slack-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-slack-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **časová rezerva**.

    ![Galerie aplikace] (./media/active-directory-saas-slack-tutorial/IC794981.png "Galerie aplikace")

7.  V podokně výsledků vyberte **časová rezerva**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Scénář] (./media/active-directory-saas-slack-tutorial/IC796925.png "Scénář")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit časová rezerva s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **časová rezerva** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-slack-tutorial/IC794982.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jak chcete uživatelům přihlásit k časová rezerva** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-slack-tutorial/IC794983.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** do textového pole **Časová rezerva znak v adresa URL** zadejte adresu URL vašeho časové rezervy klienta (například: "*https://azuread.slack.com*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-slack-tutorial/IC794984.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v časová rezerva** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-slack-tutorial/IC794985.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu časové rezervy společnosti.

6.  Přejděte na **společnosti Microsoft Azure AD \> týmu nastavení**.

    ![Nastavení týmu] (./media/active-directory-saas-slack-tutorial/IC794986.png "Nastavení týmu")

7.  V části **Nastavení týmových** klikněte na kartu **ověření** a potom klikněte na **Změnit nastavení**.

    ![Nastavení týmu] (./media/active-directory-saas-slack-tutorial/IC794987.png "Nastavení týmu")

8.  V dialogovém okně **Nastavení ověřování SAML** proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-slack-tutorial/IC794988.png "Nastavení SAML")

    1.  Na portálu na **konfigurovat jednotné přihlašování v časová rezerva** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **SAML 2.0 koncový bod HTTP ()** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v časová rezerva** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vystavitel zprostředkovatele identit** .
    3.  Vytvoření souboru **kódování base-64** z stažený certifikát.
    
        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevřete zakódovaný certifikát základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **Veřejné certifikát** .
    5.  Zrušte zaškrtnutí políčka **Povolit uživatelům měnit své e-mailovou adresu**.
    6.  Vyberte **Povolit uživatelům volbu vlastní uživatelské jméno**.
    7.  Jako **ověřování pro váš tým musí používat**vyberte **je nepovinný krok**.
    8.  Klikněte na tlačítko **Uložit konfiguraci**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-slack-tutorial/IC794989.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit časová rezerva, musí být zřízení do časové rezervy.
  
Existuje žádné akce položky konfigurace uživatele zřizování časová rezerva.  
Pokud přiřazený uživatel se pokusí přihlásit časová rezerva, časová rezerva účet se automaticky vytvoří v případě potřeby.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Přiřazení uživatelů k časová rezerva, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **časová rezerva **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-slack-tutorial/IC794990.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-slack-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).