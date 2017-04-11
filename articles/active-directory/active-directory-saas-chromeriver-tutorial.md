<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Chromeriver | Microsoft Azure" 
    description="Naučte se používat Chromeriver s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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


#<a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>Kurz: Azure Active Directory integrace s Chromeriver

Cílem tohoto kurzu je zobrazit integrace Azure a Chromeriver.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Chromeriver jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Chromeriver (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Chromeriver.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Chromeriver
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-chromeriver-tutorial/IC802755.png "Scénář")
##<a name="enabling-the-application-integration-for-chromeriver"></a>Povolení integrace aplikací pro Chromeriver

Cílem tento oddíl je přehledu jak povolit integraci aplikace Chromeriver.

###<a name="to-enable-the-application-integration-for-chromeriver-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Chromeriver, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-chromeriver-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-chromeriver-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-chromeriver-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-chromeriver-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Chromeriver**.

    ![Galerie aplikace] (./media/active-directory-saas-chromeriver-tutorial/IC802756.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Chromeriver**a potom klikněte na **Dokončit** přidat aplikaci.
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Chromeriver s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Chromeriver** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-chromeriver-tutorial/IC802757.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Chromeriver** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-chromeriver-tutorial/IC802758.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-chromeriver-tutorial/IC802759.png "Konfigurace nastavení aplikace")

    1.  **Adresa URL odpověď** do textového pole, zadejte Chromeriver **AssertionConsumerService URL** (například: *https://qa-app.chromeriver.com/login/sso/saml/consume?customerId=911*).  

        >[AZURE.NOTE] Tato hodnota můžete získat od týmu podpory Chromeriver.

    2.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v Chromeriver** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-chromeriver-tutorial/IC802760.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat svému týmu podpory Chromeriver.

    >[AZURE.NOTE]Váš tým podpory Chromeriver musí udělejte skutečné konfigurace jednotného přihlašování.  
    Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-chromeriver-tutorial/IC802761.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Chromeriver, musí být zřízení do Chromeriver.  
V případě Chromeriver je potřeba vytvořit tak, že váš tým podpory Chromeriver uživatelských účtů.

>[AZURE.NOTE] Můžete použít jakékoli další Chromeriver uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Chromeriver poskytování služby Azure Active Directory uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-chromeriver-perform-the-following-steps"></a>Přiřazení uživatelů k Chromeriver, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Chromeriver **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-chromeriver-tutorial/IC802762.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-chromeriver-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
