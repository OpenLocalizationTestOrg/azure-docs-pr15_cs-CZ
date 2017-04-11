<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Brightspace tak, že Desire2Learn | Microsoft Azure" 
    description="Naučte se používat Brightspace tak, že Desire2Learn s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Kurz: Azure Active Directory integrace s Brightspace tak, že Desire2Learn

Cílem tohoto kurzu je zobrazit integrace Azure a Brightspace tak, že Desire2Learn.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Brightspace tak, že Desire2Learn jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili Brightspace tak, že Desire2Learn bude moct jednotného přihlášení do aplikace v Brightspace Desire2Learn společnosti (service provider, které iniciuje přihlášení) nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Brightspace tak, že Desire2Learn
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scénář")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Povolení integrace aplikací pro Brightspace tak, že Desire2Learn

Cílem tento oddíl je přehledu jak povolit integraci aplikace Brightspace tak, že Desire2Learn.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Brightspace tak, že Desire2Learn, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Brightspace tak, že Desire2Learn**.

    ![Galerie Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Galerie Apllication")

7.  V podokně výsledků vyberte **Brightspace tak, že Desire2Learn**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Brightspace tak, že Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace tak, že Desire2Learn")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Brightspace tak, že Desire2Learn s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Brightspace tak, že Desire2Learn** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Brightspace tak, že Desire2Learn** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Konfigurace aplikace URL")

    1.  **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané uživatelů a přihlaste se do **Brightspace tak, že Desire2Learn** (například: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v Brightspace tak, že Desire2Learn** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a pak uložit metadata v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat vaší Brightspace tak, že tým podpory Desire2Learn.

    >[AZURE.NOTE] Brightspace tak, že tým podpory Desire2Learn musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Brightspace tak, že Desire2Learn, budou musí být zřízení do Brightspace tak, že Desire2Learn.  
V případě Brightspace tak, že Desire2Learn je potřeba vytvořit tak, že vaše Brightspace tak, že tým podpory Desire2Learn uživatelských účtů.

>[AZURE.NOTE] Můžete použít jiné Brightspace pomocí nástroje pro tvorbu Desire2Learn uživatelského účtu nebo rozhraní API poskytovanou Brightspace tak, že Desire2Learn poskytování služby Azure Active Directory uživatelských účtů.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Přiřazení uživatelů k Brightspace tak, že Desire2Learn, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Brightspace tak, že Desire2Learn **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
