<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Gigya | Microsoft Azure" 
    description="Naučte se používat Gigya s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Kurz: Azure Active Directory integrace s Gigya
  
Cílem tohoto kurzu je zobrazit integrace Azure a Gigya.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Gigya jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Gigya (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Gigya.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Gigya
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Konfigurace jednotného přihlašování] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Konfigurace jednotného přihlašování")
##<a name="enabling-the-application-integration-for-gigya"></a>Povolení integrace aplikací pro Gigya
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Gigya.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Gigya, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Gigya**.

    ![Galerie aplikace] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Gigya**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Gigya s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Gigya** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Gigya** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Gigya na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*http://company.gigya.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Gigya** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti Gigya jako správce.

6.  Přejděte na **Nastavení \> SAML přihlášení**a potom klikněte na tlačítko **Přidat** .

    ![SAML přihlášení] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML přihlášení")

7.  V části **Přihlašovací SAML** proveďte následující kroky:

    ![Konfigurace SAML] (./media/active-directory-saas-gigya-tutorial/IC789533.png "Konfigurace SAML")

    1.  Do textového pole **název** zadejte název pro konfiguraci.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Gigya** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vydavatel** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v Gigya** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Jednoho přihlašování adresy URL služby** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v Gigya** dialogové okno stránce Azure klasické zkopírujte hodnotu **Identifikátor formátu jména** a potom je vložte do textového pole **ID formát názvu** .
    5.  Vytvoření souboru **kódování base-64** z stažený certifikát.
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikát** .
    7.  Klikněte na **Uložit nastavení**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Gigya, musí být zřízení do Gigya.  
V případě Gigya zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Gigya** jako správce.

2.  Přejděte na **správce \> spravovat uživatele**a potom klikněte na **Pozvat uživatele**.

    ![Správa uživatelů] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Správa uživatelů")

3.  V dialogu pozvat uživatele proveďte následující kroky:

    ![Pozvání uživatelů] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Pozvání uživatelů")

    1.  **E-mailu** do textového pole zadejte e-mailový alias platný účet Azure Active Directory, který chcete poskytování.
    2.  Klikněte na **Pozvat uživatele**.
    
        >[AZURE.NOTE] Majitele účtu služby Azure Active Directory, dostanou e-mail obsahující odkaz před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Gigya uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Gigya k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Přiřazení uživatelů k Gigya, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Gigya **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).