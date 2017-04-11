<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Intacct | Microsoft Azure" 
    description="Naučte se používat Intacct s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Kurz: Azure Active Directory integrace s Intacct
  
Cílem tohoto kurzu je zobrazit integrace Azure a Intacct.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Intacct klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Intacct (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Intacct.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Intacct
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Scénář")
##<a name="enabling-the-application-integration-for-intacct"></a>Povolení integrace aplikací pro Intacct
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Intacct, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Intacct**.

    ![Galerie aplikace] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Intacct**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Intacct s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Intacct** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Intacct** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Intacct na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://Intacct.com/company*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Intacct** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Intacct jako správce.

6.  Klikněte na kartu **společnosti** a potom klikněte na **Informace o společnosti**.

    ![Společnosti] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Společnosti")

7.  Klikněte na kartu **zabezpečení** a potom klikněte na **Upravit**.

    ![Zabezpečení] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Zabezpečení")

8.  V části **jednotné přihlašování (SSO)** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Jednotné přihlašování")

    1.  Zaškrtněte políčko **Povolit jednotného přihlášení na**.
    2.  Jako **Typ zprostředkovatele identit**vyberte **SAML 2.0**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v Intacct** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Adresa URL vydavatel** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v Intacct** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    5.  Vytvoření souboru **kódování base-64** z stažený certifikát.
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **certifikátu**
    7.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Intacct, musí být zřízení do Intacct.  
V případě Intacct zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Intacct** .

2.  Klikněte na kartu **společnosti** a potom klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Uživatelé")

3.  Klikněte na kartu **Přidat**

    ![Přidání] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Přidání")

4.  V části **Informace** o uživateli postupujte takto:

    ![Informace o uživatelích] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Informace o uživatelích")

    1.  Zadejte **ID uživatele**, **Příjmení**, **jméno**, **e-mailovou adresu**, **název** a **telefonu** Azure AD účtu, který chcete poskytování do související textová pole.
    2.  Vyberte **oprávnění správce** , který chcete zřízení účtu Azure AD.
    3.  Klikněte na **Uložit**.
        
        >[AZURE.NOTE] Majitele účtu AAD dostávat e-mailu a potvrďte svůj účet, než se aktivuje, klepnutím na odkaz.

>[AZURE.NOTE] Můžete použít jakékoli další Intacct uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Intacct k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Přiřazení uživatelů k Intacct, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Intacct **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).