<properties 
    pageTitle="Kurz: Azure Active Directory integrace s FreshService | Microsoft Azure" 
    description="Naučte se používat FreshService s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Kurz: Azure Active Directory integrace s FreshService
  
Cílem tohoto kurzu je zobrazit integrace Azure a FreshService.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   FreshService jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili FreshService.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro FreshService
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Scénář")
##<a name="enabling-the-application-integration-for-freshservice"></a>Povolení integrace aplikací pro FreshService
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace FreshService.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace FreshService, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **FreshService**.

    ![Galerie aplikace] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Galerie aplikace")

7.  V podokně výsledků vyberte **FreshService**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit FreshService s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro FreshService vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **FreshService** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k FreshService** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit FreshService na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Freshdesk (například: "*http://democompany.freshservice.com/*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v FreshService** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti FreshService jako správce.

6.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Správce")

7.  **Portál zákazníka**klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Zabezpečení")

8.  V části **zabezpečení** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Jednotné přihlašování")

    1.  Přepnutí **OnON jednotného přihlášení**.
    2.  Vyberte **SAML přihlašování**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v FreshService** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **SAML přihlašovací adresa URL** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v FreshService** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    5.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikátu zabezpečení** .
    
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit FreshService, musí být zřízení do FreshService.  
V případě FreshService zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **FreshService** jako správce.

2.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Správce")

3.  V části **Správa uživatelů** na **žadatele**.

    ![Žadatele] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Žadatele")

4.  Klikněte na **Nový odesílatele**.

    ![Nové žadatele] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Nové žadatele")

5.  V části **Nový žadateli** proveďte následující kroky:

    ![Nové odesílatele] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Nové odesílatele")

    1.  Uzavřít **křestního jména** a **e-mailu** atributy platný účet Azure Active Directory, který chcete k poskytování související textová pole.
    2.  Klikněte na **Uložit**.

    >[AZURE.NOTE] Majitele účtu služby Azure Active Directory pošle e-mail a odkaz na před se aktivuje, potvrďte účet

>[AZURE.NOTE] Můžete použít jakékoli další FreshService uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou FreshService k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Přiřazení uživatelů k FreshService, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **FreshService **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).