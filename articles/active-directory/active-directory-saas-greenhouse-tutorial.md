<properties 
    pageTitle="Kurz: Azure Active Directory integrace s skleníkových | Microsoft Azure" 
    description="Naučte se používat skleníkových s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Kurz: Azure Active Directory integrace s skleníkových
  
Cílem tohoto kurzu je zobrazit integrace Azure a skleníkových.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Skleníkových jednoho přihlašování předplatné
  
Po dokončení tohoto kurzu, které jste přiřadili skleníkových uživatelům Azure AD bude moct jednotného přihlášení do aplikace na webu společnosti skleníkových (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro skleníkových
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scénář")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Povolení integrace aplikací pro skleníkových
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace skleníkových.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace skleníkových, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **skleníkových**.

    ![Galerie aplikace] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Galerie aplikace")

7.  V podokně výsledků vyberte **skleníkových**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Skleníkových] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Skleníkových")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit skleníkových s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **skleníkových** aplikace integrace Azure klasický klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k skleníkových** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://company.greenhouse.io*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v skleníkových** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Konfigurace jednotného přihlašování")

5.  Přepošlete tento soubor metadat pro tým podpory skleníkových.

    >[AZURE.NOTE] Jednotné přihlašování musí povolit tak, že tým podpory skleníkových.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit skleníkových, musí být zřízení do skleníkových.  
V případě skleníkových zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **skleníkových** jako správce.

2.  V nabídce na horní klikněte na **Konfigurovat**a klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Uživatelé")

3.  Klikněte na **nové uživatele**.

    ![Nového uživatele] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Nového uživatele")

4.  V části **Přidat nového uživatele** proveďte následující kroky:

    ![Přidání nového uživatele] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Přidání nového uživatele")

    1.  **Enter uživatele e-mailů** do textového pole zadejte e-mailovou adresu platný účet Azure Active Directory, který chcete poskytování.
    2.  Klikněte na **Uložit**.
        
        >[AZURE.NOTE] Na majitele účtu služby Azure Active Directory, dostanou e-mailu a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další skleníkových uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou skleníkových k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Přiřazení uživatelů k skleníkových, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **skleníkových **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).