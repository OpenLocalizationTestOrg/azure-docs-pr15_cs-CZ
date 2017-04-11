<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Zoho hromadné | Microsoft Azure" 
    description="Naučte se používat Zoho pošta s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Kurz: Azure Active Directory integrace s Zoho pošty
  
Cílem tohoto kurzu je zobrazit integrace Azure a Zoho pošta.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Hromadné Zoho klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Zoho pošta (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené Zoho pošta.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací Zoho pošty
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scénář")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Povolení integrace aplikací Zoho pošty
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Zoho pošty.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace Zoho pošty, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Zoho pošta**.

    ![Galerie aplikace] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Zoho pošta**a pak klikněte na **Dokončit** přidat aplikaci.

    ![Zoho pošty] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho pošty")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Zoho pošty s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Zoho poštovní** aplikaci integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Zoho pošta** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Konfigurace aplikace URL")

    na. V textovém poli **Zoho hromadné znaménko na adresu URL** zadejte adresu URL pomocí následujícího vzorce:`http://<company name>.ZohoMail.com`

    b. Klikněte na tlačítko **Další**.


4.  Na stránce **konfigurovat jednotné přihlašování v Zoho pošta** klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti Zoho hromadné jako správce.

6.  Otevřete **Ovládací panely**.

    ![Ovládací panely] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Ovládací panely")

7.  Klikněte na kartu **SAML ověřování** .

    ![SAML ověřování] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML ověřování")

8.  V části **Podrobnosti ověřování SAML** proveďte následující kroky:

    ![Podrobnosti o SAML ověřování] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Podrobnosti o SAML ověřování")

    1.  Na portálu na **konfigurovat jednotné přihlašování u Zoho poštu** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování u Zoho poštu** dialogové okno stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v Zoho hromadné** dialogové okno stránce Azure klasické zkopírujte hodnotu **Heslo změnit adresu URL** a potom je vložte do textového pole **Heslo změnit adresu URL** .
    4.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **PublicKey** .
    6.  **Algoritmus**vyberte **RSA**.
    7.  Klikněte na **OK**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit do Zoho e-mailu, musí být zřízení do Zoho e-mailu.  
V případě Zoho hromadné zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Zoho hromadné** jako správce.

2.  Přejděte na **Ovládací panely \> pošta a dokumenty**.

3.  Přejděte na **Podrobné informace o uživateli \> přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Přidání uživatele")

4.  V dialogovém okně **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Přidání uživatele")

    1.  Zadejte **Křestní jméno**, **Příjmení**, **E-mailu ID**a **heslo** platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na **OK**.  

        >[AZURE.NOTE] Majitele účtu služby Azure Active Directory, dostanou e-mailu s odkazem pro potvrzení účtu před z něj stal aktivní.

>[AZURE.NOTE] Můžete použít jakékoli další Zoho hromadné uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Zoho hromadné k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Přiřazení uživatelů do Zoho pošty, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce **Zoho poštovní **aplikaci integrace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).