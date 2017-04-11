<properties 
    pageTitle="Kurz: Azure Active Directory integrace s technickou podporu Jitbit | Microsoft Azure" 
    description="Naučte se používat Jitbit linku s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Kurz: Azure Active Directory integrace s Jitbit technickou podporu
  
Cílem tohoto kurzu je zobrazit integrace Azure a Jitbit technickou podporu.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Jitbit Helpdesk klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti technickou podporu Jitbit (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené Jitbit technickou podporu.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro technickou podporu Jitbit
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scénář")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Povolení integrace aplikací pro technickou podporu Jitbit
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Jitbit technickou podporu.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Postup povolení integrace aplikace pro technickou podporu Jitbit, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Jitbit technickou podporu**.

    ![Galerie aplikace] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Jitbit Helpdesk**a potom klikněte na **Dokončit** přidat aplikaci.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit na technickou podporu Jitbit s účtem v Azure AD pomocí federace podle protokolu SAML. .  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Aby bylo možné konfigurace jednotného přihlašování na technickou podporu Jitbit klientovi, budete muset nejdřív kontaktujte technickou podporu Jitbit technickou podporu na získání tuto funkci povolit.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Technickou podporu Jitbit** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Konfigurace jednotného přihlašování")

2.  Na stránce, **jakým způsobem uživatelé přihlásit k Jitbit technickou podporu** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Jitbit technickou podporu znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Jitbit.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Konfigurace aplikace URL")

4.  Na stránce **Konfigurace jednotného přihlašování na technickou podporu Jitbit** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Jitbit Helpdesk.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče jiný web Přihlaste se jako správce k webu společnosti Jitbit technickou podporu.

6.  Na panelu nástrojů na horní klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Správa")

7.  Klikněte na **Obecné nastavení**.

    ![Uživatelé, společnosti a oprávnění] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Uživatelé, společnosti a oprávnění")

8.  V části **Nastavení ověřování** konfigurace proveďte následující kroky:

    ![Nastavení ověřování] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Nastavení ověřování")

    1.  Vyberte **Povolit SAML 2.0 jednoho požádat o** přihlášení pomocí jedné přihlašování (SSO) **OneLogin**.
    2.  Na portálu na dialog stránce **Konfigurace jednotného přihlašování na technickou podporu Jitbit** Azure klasické zkopírujte hodnotu **poskytovatele služeb (SP), které iniciuje koncového bodu** a potom je vložte do textového pole **Adresa URL koncového bodu** .
    3.  Vytvoření souboru **kódování base-64** z stažený certifikát.
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevření zakódovaný certifikátu základu-64, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikátu**
    5.  Klikněte na **Uložit změny**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Jitbit technickou podporu, musí být zřízení do Jitbit technickou podporu.  
V případě technickou podporu Jitbit zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Jitbit technickou podporu** .

2.  V nabídce nahoře klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Správa")

3.  Klikněte na **uživatele, společnosti a oprávnění**.

    ![Uživatelé, společnosti a oprávnění] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Uživatelé, společnosti a oprávnění")

4.  Klikněte na **Přidat uživatele**.

    ![Přidat uživatele] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Přidat uživatele")

5.  Zadejte data Azure AD účet, který chcete k poskytování do následujících textová pole v části vytvořit: **uživatelské jméno**, **e-mailu**, **jméno**a **Příjmení**

    ![Vytvoření] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Vytvoření")

6.  Klikněte na **vytvořit**.

>[AZURE.NOTE] Můžete použít jakékoli další Jitbit technickou podporu uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Jitbit technickou podporu k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Přiřazení uživatelů k Jitbit technickou podporu, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Jitbit Helpdesk **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).