<properties 
    pageTitle="Kurz: Azure Active Directory integrace s BlueJeans | Microsoft Azure" 
    description="Naučte se používat BlueJeans s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Kurz: Azure AD integrace s BlueJeans

Cílem tohoto kurzu je zobrazit integrace Azure a BlueJeans.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   BlueJeans jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti BlueJeans (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili BlueJeans.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro BlueJeans
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scénář")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Povolení integrace aplikací pro BlueJeans

Cílem tento oddíl je přehledu jak povolit integraci aplikace BlueJeans.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace BlueJeans, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **BlueJeans**.

    ![Galerie aplikace] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Galerie aplikace")

7.  V podokně výsledků vyberte **BlueJeans**a potom klikněte na **Dokončit** přidat aplikaci.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit BlueJeans s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **BlueJeans** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k BlueJeans** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit BlueJeans na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://company.BlueJeans.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v BlueJeans** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti **BlueJeans** jako správce.

6.  Přejděte na **správce \> nastavení skupiny \> zabezpečení**.

    ![Správce] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Správce")

7.  V části **zabezpečení** proveďte následující kroky:

    ![Jednotné přihlašování k SAML] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "Jednotné přihlašování k SAML")

    1.  Vyberte **jednotné přihlašování k SAML**.
    2.  Zaškrtněte políčko **Povolit automatické vytváření**.

8.  Přesunutí na pomocí následujícího postupu:

    ![Cesta certifikátu] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Cesta certifikátu")

    1.  Klikněte na **Zvolit soubor**a pak nahrajte stažený certifikát.
    2.  Na portálu na **konfigurovat jednotné přihlašování v BlueJeans** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v BlueJeans** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Heslo změnit adresu URL** a potom je vložte do textového pole **Heslo změnit adresu URL** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v BlueJeans** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .

9.  Přesunutí na pomocí následujícího postupu:

    ![Uložení změn] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Uložení změn")

    1.  Do textového pole **id uživatele** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Do textového pole **e-mailu** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Klikněte na **Uložit změny**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit BlueJeans, musí být zřízení do BlueJeans.  
V případě BlueJeans zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **BlueJeans** jako správce.

2.  Přejděte na **správce \> Správa uživatelů \> přidat uživatele**.

    ![Správce] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Správce")

    >[AZURE.IMPORTANT] Na kartě **Přidat uživatele** je k dispozici pouze, na **kartu zabezpečení** **Povolit automatické vytváření** není zaškrtnuté.

3.  V části **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Přidání uživatele")

    1.  Zadejte **BlueJeans uživatelské jméno**, **e-mailovou adresu**, **ID schůzky BlueJeans**, **Moderátora heslo**, **Celé jméno**, **společnosti** platného AAD účtu, který chcete vytvořit do související textová pole.
    2.  Klikněte na **Přidat uživatele**.

>[AZURE.NOTE] Můžete použít jakékoli další BlueJeans uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou BlueJeans k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Přiřazení uživatelů k BlueJeans, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **BlueJeans **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
