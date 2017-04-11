<properties 
    pageTitle="Kurz: Azure Active Directory integrace s EmpCenter | Microsoft Azure" 
    description="Naučte se používat EmpCenter s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Kurz: Azure Active Directory integrace s EmpCenter
  
Cílem tohoto kurzu je zobrazit integrace Azure a EmpCenter.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   EmpCenter jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti EmpCenter (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili EmpCenter.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro EmpCenter
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scénář")
##<a name="enabling-the-application-integration-for-empcenter"></a>Povolení integrace aplikací pro EmpCenter
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace EmpCenter.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace EmpCenter, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **EmpCenter**.

    ![Galerie aplikace] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Galerie aplikace")

7.  V podokně výsledků vyberte **EmpCenter**a potom klikněte na **Dokončit** přidat aplikaci.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit EmpCenter s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **EmpCenter** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k EmpCenter** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Konfigurace nastavení aplikace")

    1.  **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace EmpCenter (například: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v EmpCenter** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat svému týmu podpory EmpCenter.

    >[AZURE.NOTE] Váš tým podpory EmpCenter musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit EmpCenter, musí být zřízení do EmpCenter.  
V případě EmpCenter je potřeba vytvořit tak, že váš tým podpory EmpCenter uživatelských účtů.

>[AZURE.NOTE] Můžete použít jakékoli další EmpCenter uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou EmpCenter poskytování služby Azure Active Directory uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Přiřazení uživatelů k EmpCenter, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **EmpCenter **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).