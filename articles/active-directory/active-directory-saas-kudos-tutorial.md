<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Kudos | Microsoft Azure" 
    description="Naučte se používat Kudos s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Kurz: Azure Active Directory integrace s Kudos
  
Cílem tohoto kurzu je zobrazit integrace Azure a Kudos.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Kudos klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Kudos (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Kudos.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Kudos
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Scénář")
##<a name="enabling-the-application-integration-for-kudos"></a>Povolení integrace aplikací pro Kudos
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Kudos.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Kudos, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Kudos**.

    ![Galerie aplikace] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Kudos**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Kudos] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Kudos s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Kudos** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Kudos** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Kudos na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*https://company.kudosnow.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Kudos** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Kudos jako správce.

6.  V nabídce na horní klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Nastavení")

7.  Klikněte na **Integrace \> jednotné přihlašování**.

8.  V části **jednotné přihlašování** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Jednotné přihlašování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Kudos** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **na adresa URL přihlašovací** .
    2.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP]
        Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikátu**
    4.  Na portálu na **konfigurovat jednotné přihlašování v Kudos** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Odhlášení na adresu URL** .
    5.  **Vaše adresa URL Kudos** do textového pole zadejte název vaší společnosti.
    6.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Kudos, musí být zřízení do Kudos.  
V případě Kudos zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Kudos** jako správce.

2.  V nabídce na horní klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Nastavení")

3.  Klikněte na **Správce uživatelů**.

4.  Klikněte na kartu **uživatelů** a potom klikněte na **Přidat uživatele**.

    ![Správce uživatelů] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Správce uživatelů")

5.  V části **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Přidání uživatele")

    1.  Zadejte **jméno**, **Příjmení**, **e-mailu** a dalších podrobností platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na **Vytvořit uživatele**.

>[AZURE.NOTE]Můžete použít jakékoli další Kudos uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Kudos k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Přiřazení uživatelů k Kudos, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Kudos **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).