<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Kintone | Microsoft Azure" 
    description="Naučte se používat Kintone s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Kurz: Azure Active Directory integrace s Kintone
  
Cílem tohoto kurzu je zobrazit integrace Azure a Kintone.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Kintone jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Kintone (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Kintone.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Kintone
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Scénář")
##<a name="enabling-the-application-integration-for-kintone"></a>Povolení integrace aplikací pro Kintone
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Kintone.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Kintone, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Kintone**.

    ![Galerie aplikace] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Kintone**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Kintone s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Kintone** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Kintone** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Kintone na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://company.kintone.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Kintone** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti **Kintone** jako správce.

6.  Klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Nastavení")

7.  Klikněte na **Uživatelé a správa systému**.

    ![Uživatelé a správa systému] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Uživatelé a správa systému")

8.  V části **systému správy \> zabezpečení** klikněte na **přihlásit**.

    ![Přihlášení] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Přihlášení")

9.  Zaškrtněte políčko **Povolit SAML ověřování**.

    ![SAML ověřování] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML ověřování")

10. V části ověřování SAML proveďte následující kroky:

    ![SAML ověřování] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML ověřování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Kintone** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v Kintone** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    3.  Klikněte na tlačítko **Procházet** k nahrání stažený certifikát.
    4.  Klikněte na **Uložit**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Kintone, musí být zřízení do Kintone.  
V případě Kintone zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Kintone** jako správce.

2.  Klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Nastavení")

3.  Klikněte na **Uživatelé a správa systému**.

    ![Uživatel a správa systému] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Uživatel a správa systému")

4.  V části **Správa uživatelů**na **oddělení a uživatele**.

    ![Oddělení a uživatelů] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Oddělení a uživatelů")

5.  Klikněte na **Nový uživatel**.

    ![Noví uživatelé] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Noví uživatelé")

6.  V části **Nový uživatel** proveďte následující kroky:

    ![Noví uživatelé] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Noví uživatelé")

    1.  Zadejte **Zobrazované jméno**, **Přihlašovací jméno**, **Nové heslo**, **Potvrďte heslo**, **E-mailové adresy** a další podrobnosti platného AAD účtu, který chcete poskytování do související texboxes.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další Kintone uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Kintone k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Přiřazení uživatelů k Kintone, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Kintone **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).