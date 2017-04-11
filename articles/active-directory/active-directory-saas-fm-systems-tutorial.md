<properties 
    pageTitle="Kurz: Azure Active Directory integrace s FM: systémy | Microsoft Azure" 
    description="Naučte se používat FM: systémy službou Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Kurz: Azure Active Directory integrace s FM: systémy
  
Cílem tohoto kurzu je integrace Azure a FM:Systems zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   FM:Systems jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti FM:Systems (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili FM:Systems.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro FM:Systems
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scénář")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Povolení integrace aplikací pro FM:Systems
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace FM:Systems.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace FM:Systems, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **FM:Systems**.

    ![Galerie aplikace] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Galerie aplikace")

7.  V podokně výsledků vyberte **FM:Systems**a potom klikněte na **Dokončit** přidat aplikaci.

    ![FM: systémy] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: systémy")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit FM:Systems s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **FM:Systems** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k FM:Systems** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Konfigurace aplikace URL")

    1.  **Znaménko FM:Systems na adresu URL** do textového pole, zadejte FM:Systems **Odpověď URL** (například: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Tato hodnota se dá dostat z vaší FM: systémy tým podpory.

    2.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v FM:Systems** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a pak uložit metadata v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Konfigurace jednotného přihlašování")

5.  Odeslat soubor stažený metadat pro vaše FM: systémy tým podpory.

    >[AZURE.NOTE] FM: Tým podpory systémy musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit FM:Systems, musí být zřízení do FM:Systems.  
V případě FM:Systems zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  V okně webového prohlížeče Přihlaste se k webu společnosti FM:Systems jako správce.

2.  Přejděte na **systému správy \> Správa zabezpečení \> uživatelé \> seznam uživatelů**.

    ![Správa systému] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Správa systému")

3.  Klikněte na **vytvořit nového uživatele**.

    ![Vytvoření nového uživatele] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Vytvoření nového uživatele")

4.  V části **Vytvořit uživatele** proveďte následující kroky:

    ![Vytvoření uživatele] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Vytvoření uživatele")

    1.  Zadejte uživatelské jméno, heslo a jeho potvrzení, e-mailovou adresu a ID zaměstnance platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na tlačítko **Další**.

>[AZURE.NOTE] Můžete použít jakékoli další FM:Systems uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou FM:Systems k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Přiřazení uživatelů k FM:Systems, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **FM:Systems **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).