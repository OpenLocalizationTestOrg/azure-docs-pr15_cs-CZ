<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Boomi | Microsoft Azure" 
    description="Naučte se používat Boomi s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Kurz: Azure Active Directory integrace s Boomi

Cílem tohoto kurzu je zobrazit integrace Azure a Boomi.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Boomi jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Boomi (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Boomi.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Boomi
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Scénář")
##<a name="enabling-the-application-integration-for-boomi"></a>Povolení integrace aplikací pro Boomi

Cílem tento oddíl je přehledu jak povolit integraci aplikace Boomi.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Boomi, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Boomi**.

    ![Galerie aplikace] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Boomi**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Boomi s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Boomi** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Boomi** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **URL odpověď Boomi** zadejte **Boomi AtomSphere přihlašovací adresa URL** (například: "*https://platform.boomi.com/sso/AccountName/saml*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Boomi** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Boomi jako správce.

6.  Na panelu nástrojů na horní klikněte na název vaší společnosti a pak **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Nastavení")

7.  Klikněte na **Možnosti jednotné přihlašování**.

    ![Možnosti jednotné přihlašování] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Možnosti jednotné přihlašování")

8.  V části **Možnosti přihlašování** proveďte následující kroky:

    ![Možnosti přihlášení] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Možnosti přihlášení")

    1.  Zaškrtněte políčko **Povolit SAML jednotného přihlašování**.
    2.  Klikněte na **Import**nahrát stažený certifikátu.
    3.  Na portálu na **konfigurovat jednotné přihlašování v Boomi** dialogové okno stránce Azure klasický zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .
    4.  Je **Federace Id umístění**vyberte položku **federace Id NameID prvku předmět**.
    5.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Boomi, musí být zřízení do Boomi.  
V případě Boomi zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Boomi** jako správce.

2.  Přejděte na **Správa uživatelů \> uživatelé**.

    ![Uživatelé] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Uživatelé")

3.  Klikněte na **Přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Přidání uživatele")

4.  Na stránce dialogové okno **Přidat role uživatelů** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Přidání uživatele")

    1.  Zadejte **jméno**, **Příjmení** a **e-mailu** platný AAD účet, který chcete vytvořit do související textová pole.
    2.  Klikněte na OK.

>[AZURE.NOTE] Můžete použít jakékoli další Boomi uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Boomi k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Přiřazení uživatelů k Boomi, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Boomi **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
