<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Sprinklr | Microsoft Azure" 
    description="Naučte se používat Sprinklr s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Kurz: Azure Active Directory integrace s Sprinklr
  
Cílem tohoto kurzu je integrace Azure a Sprinklr zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Sprinklr klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Sprinklr (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Sprinklr.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Sprinklr
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scénář")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Povolení integrace aplikací pro Sprinklr
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Sprinklr.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Sprinklr, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Sprinklr**.

    ![Galerie aplikace] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Sprinklr**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Sprinklr s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Sprinklr** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Sprinklr** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Sprinklr znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. sprinklr.com*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Sprinklr** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Sprinklr jako správce.

6.  Přejděte na **Správa \> nastavení**.

    ![Správa] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Správa")

7.  Přejděte na **Správa partnera \> jednotného přihlášení** v levém podokně.

    ![Správa partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Správa partnera")

8.  Klikněte na tlačítko **+ Doplňky jednotného přihlášení**.

    ![Jeden znak ie] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Jeden znak ie")

9.  Na stránce **Jednotné přihlašování** proveďte následující kroky:

    ![Jeden znak ie] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Jeden znak ie")

    1.  Do textového pole **název** zadejte název pro konfiguraci vašeho (například: *WAADSSOTest*).
    2.  Vyberte **Povolit**.
    3.  Vyberte **Použijte nový certifikát jednotné přihlašování**.
    4.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    5.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **Certifikát poskytovatele Identity**
    6.  Na portálu na **konfigurovat jednotné přihlašování v Sprinklr** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **Entity Id** .
    7.  Na portálu na **konfigurovat jednotné přihlašování v Sprinklr** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .
    8.  Na portálu na **konfigurovat jednotné přihlašování v Sprinklr** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlášení poskytovatele Identity** .
    9.  Jako **Typ SAML uživatelské ID**, vyberte **výraz obsahuje uživatele "uživatelské jméno s sprinklr.com**.
    10. Je **SAML umístění ID uživatele**vyberte položku **ID uživatele v elementu identifikátor název příkazu předmět**.
    11. Zavřete **Uložit**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pro AAD aby uživatelé mohli přihlásit musí být zřízení pro přístup do aplikace Sprinklr.  
Tato část popisuje, jak vytvořit AAD uživatelských účtů uvnitř Sprinklr.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Zřízení uživatelský účet v Sprinklr, proveďte následující kroky:

1.  Přihlaste se k webu společnosti Sprinklr jako správce.

2.  Přejděte na **Správa \> nastavení**.

    ![Správa] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Správa")

3.  Přejděte na **Správa klienta \> uživatelé** v levém podokně.

    ![Nastavení] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Nastavení")

4.  Klikněte na **Přidat uživatele**.

    ![Nastavení] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Nastavení")

5.  V dialogovém okně **Upravit uživatele** proveďte následující kroky:

    ![Úpravy uživatele] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Úpravy uživatele")

    1.  V **e-mailů**, **křestního jména** a **Příjmení** textová pole, zadejte informace uživatelský účet Azure AD chcete poskytování.
    2.  Vyberte **zakázat heslo**.
    3.  Vyberte požadovaný **jazyk**.
    4.  Vyberte **Typ uživatele**.
    5.  Klikněte na **Aktualizovat**.

    >[AZURE.IMPORTANT] **Heslo zakázané** musí být zaškrtnuté povolení uživatele k přihlášení pomocí poskytovatele identit.

6.  Přejděte k **roli**a pak proveďte následující kroky:

    ![Role partnera] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Role partnera")

    1.  V seznamu **globální** vyberte **všechny\_oprávnění**.
    2.  Klikněte na **Aktualizovat**.

>[AZURE.NOTE] Můžete použít jakékoli další Sprinklr uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Sprinklr zřízení Azure AD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Přiřazení uživatelů k Sprinklr, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Sprinklr **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).