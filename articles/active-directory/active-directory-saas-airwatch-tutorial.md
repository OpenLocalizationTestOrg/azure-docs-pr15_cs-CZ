<properties 
    pageTitle="Kurz: Azure Active Directory integrace s AirWatch | Microsoft Azure" 
    description="Naučte se používat AirWatch s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Kurz: Azure Active Directory integrace s AirWatch

Cílem tohoto kurzu je zobrazit integrace Azure a AirWatch.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   AirWatch jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti AirWatch (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili AirWatch.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro AirWatch
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Povolení integrace aplikací pro AirWatch

Cílem tento oddíl je přehledu jak povolit integraci aplikace AirWatch.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace AirWatch, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **AirWatch**.

    ![Galerie aplikace] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Galerie aplikace")

7.  V podokně výsledků vyberte **AirWatch**a potom klikněte na **Dokončit** přidat aplikaci.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit AirWatch s svůj účet v Azure AD pomocí federace na základě SAML protokolu.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **AirWatch** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k AirWatch** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit AirWatch na adresu URL** do textového pole, zadejte adresu URL používané uživatelů se přihlásit k aplikaci AirWatch (například: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v AirWatch** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti AirWatch jako správce.

6.  V levém navigačním podokně klikněte na **účty**a potom klikněte na **Správce**.

    ![Správci] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Správci")

7.  Rozbalení nabídky **Nastavení** a klikněte na **Adresářovými službami**.

    ![Nastavení] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Nastavení")

8.  Klikněte na kartu **uživatele** v textfield **Base DN** , napište název domény a klikněte na tlačítko **Uložit**.

    ![Uživatel] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Uživatel")

9.  Klikněte na kartu **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. Proveďte následující kroky:

    ![Nahrání] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Nahrání")

    1.  Jako **Typ adresář**vyberte **žádné**.
    2.  Vyberte **použít SAML pro ověřování**.
    3.  K nahrání stažený certifikát, klikněte na **Odeslat**.

11. V části **požádat o** proveďte následující kroky:

    ![Žádost o] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Žádost o")

    1.  Jako **Typ žádosti vazbu**vyberte **Publikovat**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Airwatch** dialogové okno stránce Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Identity poskytovatele jeden znak na adresu URL** .
    3.  Jako **NameID formát**vyberte **E-mailovou adresu**.
    4.  Klikněte na **Uložit**.

12. Znovu klikněte na kartu **uživatele** .

    ![Uživatel] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Uživatel")

13. V části **atribut** proveďte následující kroky:

    ![Atribut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Atribut")

    1.  **Identifikátor objektu** do textového pole zadejte **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Do textového pole **uživatelské jméno** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Do textového pole **Název zobrazení** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Do textového pole **jméno** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  **Příjmení** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  Do textového pole **e-mailu** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Klikněte na **Uložit**.

14. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit AirWatch, musí být zřízení do AirWatch.  
V případě AirWatch zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **AirWatch** jako správce.

2.  V navigačním podokně na levé straně klikněte na **účty**a potom klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Uživatelé")

3.  V nabídce **Uživatelé** klikněte na **Zobrazení seznamu**a potom klikněte na **Přidat \> přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Přidání uživatele")

4.  V dialogovém okně **Přidat / Upravit uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Přidání uživatele")

    1.  Zadejte **uživatelské jméno**, **heslo**, **Potvrďte heslo**, **křestního jména**, **Příjmení**, **E-mailovou adresu** platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další AirWatch uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou AirWatch k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Přiřazení uživatelů k AirWatch, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **AirWatch **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
