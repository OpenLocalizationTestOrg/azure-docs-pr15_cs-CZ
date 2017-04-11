<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Egnyte | Microsoft Azure" 
    description="Naučte se používat Egnyte s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Kurz: Azure Active Directory integrace s Egnyte
  
Cílem tohoto kurzu je zobrazit integrace Azure a Egnyte.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Egnyte jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD přiřazené Egnyte budou tito uživatelé moct jednotného přihlášení do aplikace na webu společnosti Egnyte (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md)
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Egnyte
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scénář")
##<a name="enabling-the-application-integration-for-egnyte"></a>Povolení integrace aplikací pro Egnyte
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Egnyte.

###<a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Egnyte, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-egnyte-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-egnyte-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-egnyte-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-egnyte-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **egnyte**.

    ![Galerie aplikace] (./media/active-directory-saas-egnyte-tutorial/IC787813.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Egnyte**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Egnyte] (./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Egnyte s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Egnyte** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-egnyte-tutorial/IC787815.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Egnyte** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-egnyte-tutorial/IC787816.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Egnyte znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://company.egnyte.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-egnyte-tutorial/IC787817.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Egnyte** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-egnyte-tutorial/IC787818.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Egnyte jako správce.

6.  Klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-egnyte-tutorial/IC787819.png "Nastavení")

7.  V nabídce klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-egnyte-tutorial/IC787820.png "Nastavení")

8.  Klikněte na kartu **Konfigurace** a potom klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-egnyte-tutorial/IC787821.png "Zabezpečení")

9.  V části **Ověřování přihlašování** proveďte následující kroky:

    ![Jednotné přihlašování ověřování] (./media/active-directory-saas-egnyte-tutorial/IC787822.png "Jednotné přihlašování ověřování")

    1.  Jako **jeden přihlášení**vyberte **SAML 2.0**.
    2.  Jako **poskytovatele Identity**vyberte **AzureAD**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v Egnyte** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v Egnyte** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **ID entity poskytovatele Identity** .
    5.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **certifikátů zprostředkovatele identit** .
    7.  Jako **Výchozí mapování uživatele**vyberte **e-mailovou adresu**.
    8.  Jako **hodnotu použití Vystavitel specifické pro doménu**vyberte **Zakázáno**.
    9.  Klikněte na **Uložit**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-egnyte-tutorial/IC787823.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Egnyte, musí být zřízení do Egnyte.  
V případě Egnyte zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti Egnyte **Egnyte** jako správce.

2.  Přejděte na **Nastavení \> uživatelé a skupiny**.

3.  Klikněte na **Přidat nového uživatele**a potom vyberte typ uživatele, kterého chcete přidat.

    ![Uživatelé] (./media/active-directory-saas-egnyte-tutorial/IC787824.png "Uživatelé")

4.  V části **Nový uživatel standardní** proveďte následující kroky:

    ![Nové standardní uživatele] (./media/active-directory-saas-egnyte-tutorial/IC787825.png "Nové standardní uživatele")

    1.  Zadejte **e-mailu**, **uživatelské jméno** a další podrobnosti platný účet Azure Active Directory, který chcete poskytování.
    2.  Klikněte na **Uložit**.

    >[AZURE.NOTE] Majitele účtu služby Azure Active Directory, dostanou e-mailových oznámení.

>[AZURE.NOTE] Můžete použít jakékoli další Egnyte uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Egnyte k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>Přiřazení uživatelů k Egnyte, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Egnyte **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-egnyte-tutorial/IC787826.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-egnyte-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).