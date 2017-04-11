<properties 
    pageTitle="Kurz: Azure Active Directory integrace s ShiftPlanning | Microsoft Azure" 
    description="Naučte se používat ShiftPlanning s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-shiftplanning"></a>Kurz: Azure Active Directory integrace s ShiftPlanning
  
Cílem tohoto kurzu je zobrazit integrace Azure a ShiftPlanning.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   ShiftPlanning jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti ShiftPlanning (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili ShiftPlanning.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro ShiftPlanning
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scénář")
##<a name="enabling-the-application-integration-for-shiftplanning"></a>Povolení integrace aplikací pro ShiftPlanning
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace ShiftPlanning.

###<a name="to-enable-the-application-integration-for-shiftplanning-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ShiftPlanning, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ShiftPlanning**.

    ![Galerie aplikace] (./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ShiftPlanning**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ShiftPlanning] (./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ShiftPlanning s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **ShiftPlanning** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ShiftPlanning** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Sign ShiftPlanning na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://company.shiftplanning.com/includes/saml/*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v ShiftPlanning** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti **ShiftPlanning** jako správce.

6.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Správce")

7.  V části **Integrace**klikněte na **Jednotné přihlašování**.

    ![Jednotné přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Jednotné přihlašování")

8.  V části **Jednotného přihlašování** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Jednotné přihlašování")

    1.  Vyberte **SAML povolené**.
    2.  Zaškrtněte políčko **Povolit přihlášení heslo**
    3.  Na portálu na **konfigurovat jednotné přihlašování v ShiftPlanning** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **URL Vystavitel SAML** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v ShiftPlanning** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL vzdálené odhlásit** .
    5.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    6.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikátu**
    7.  Klikněte na **Uložit nastavení**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit ShiftPlanning, musí být zřízení do ShiftPlanning.  
V případě ShiftPlanning zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **ShiftPlanning** jako správce.

2.  Klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Správce")

3.  Klikněte na **zaměstnance**.

    ![Zaměstnanců] (./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Zaměstnanců")

4.  V části **Akce**klikněte na **Přidat zaměstnance**.

    ![Přidání zaměstnanců] (./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Přidání zaměstnanců")

5.  V části **Přidat zaměstnanců** proveďte následující kroky:

    ![Uložení zaměstnanců] (./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Uložení zaměstnanců")

    1.  Zadejte **jméno**, **Příjmení** a **e-mailu** platný AAD účet, který chcete vytvořit do související textová pole.
    2.  Klepněte na tlačítko **Uložit zaměstnanců**.

>[AZURE.NOTE]Můžete použít jakékoli další ShiftPlanning uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ShiftPlanning k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-shiftplanning-perform-the-following-steps"></a>Přiřazení uživatelů k ShiftPlanning, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **ShiftPlanning **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).