<properties 
    pageTitle="Kurz: Azure Active Directory integrace s InsideView | Microsoft Azure" 
    description="Naučte se používat InsideView s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Kurz: Azure Active Directory integrace s InsideView
  
Cílem tohoto kurzu je zobrazit integrace Azure a InsideView.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   InsideView klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti InsideView (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili InsideView.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro InsideView
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Scénář")
##<a name="enabling-the-application-integration-for-insideview"></a>Povolení integrace aplikací pro InsideView
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace InsideView.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace InsideView, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **InsideView**.

    ![Galerie aplikace] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Galerie aplikace")

7.  V podokně výsledků vyberte **InsideView**a potom klikněte na **Dokončit** přidat aplikaci.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit InsideView s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **InsideView** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Konfigurace jednoho přihlášení")

2.  Na stránce **jakým způsobem uživatelé přihlásit k InsideView** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **URL InsideView odpověď** , zadejte adresu URL svého InsideView jednotné přihlašování (například: `https://my.insideview.com/iv/<STS Name>/login.iv`) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v InsideView** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Konfigurace jednoho přihlášení")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti InsideView jako správce.

6.  Na panelu nástrojů na horní klikněte na **Správce** **SingleSignOn nastavení**a klikněte na **Přidat SAML**.

    ![SAML jednotné přihlašování nastavení] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML jednotné přihlašování nastavení")

7.  V části **Přidat nové SAML** proveďte následující kroky:

    ![Přidání nového SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Přidání nového SAML")

    1.  Do textového pole **STS název** zadejte název pro konfiguraci.
    2.  Na portálu na **konfigurovat jednotné přihlašování v InsideView** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Poskytovatele služeb (SP), které iniciuje koncového bodu** a potom je vložte do textového pole **SamlP/WS Fed Unsolicated koncového bodu** .
    3.  Vytvoření souboru **kódování base-64** z stažený certifikát.
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **STS certifikátu**
    5.  Do textového pole **Crm uživatelské Id mapování** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  **Mapování Crm e-mailu** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  **Mapování Crm křestní jméno** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  **Mapování Crm příjmení** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Konfigurace jednoho přihlášení")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit InsideView, musí být zřízení do InsideView.  
V případě InsideView zřizování se ručně naplánovaný úkol.
  
Uživatelé nebo kontaktů vytvořených v InsideView, kontaktujte správce úspěchu zákazníků nebo posíláte e-maily**support@insideview.com**

>[AZURE.NOTE] Můžete použít jakékoli další InsideView uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou InsideView zřízení Azure AD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Přiřazení uživatelů k InsideView, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **InsideView **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).