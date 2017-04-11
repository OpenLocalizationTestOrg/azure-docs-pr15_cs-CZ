<properties 
    pageTitle="Kurz: Azure Active Directory integrace s SmarterU | Microsoft Azure" 
    description="Naučte se používat SmarterU s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Kurz: Azure Active Directory integrace s SmarterU
  
Cílem tohoto kurzu je zobrazit integrace Azure a SmarterU.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   SmarterU klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti SmarterU (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili SmarterU.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro SmarterU
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scénář")

##<a name="enabling-the-application-integration-for-smarteru"></a>Povolení integrace aplikací pro SmarterU
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace SmarterU.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace SmarterU, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **SmarterU**.

    ![Fallery aplikace] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Fallery aplikace")

7.  V podokně výsledků vyberte **SmarterU**a potom klikněte na **Dokončit** přidat aplikaci.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit SmarterU s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **SmarterU** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k SmarterU** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Konfigurace jednotného přihlašování")

3.  V **konfigurovat jednotné přihlašování v SmarterU** stránky, pokud si chcete stáhnout metadata, klepněte na tlačítko **Stáhnout metadata**, a potom datový soubor, místně jako **c:\\SmarterUMetaData.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Konfigurace jednotného přihlašování")

4.  V okně různých webového prohlížeče Přihlaste se k webu společnosti SmarterU jako správce.

5.  Na panelu nástrojů na horní klikněte na **Nastavení účtu**.

    ![Nastavení účtu] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Nastavení účtu")

6.  Na stránce konfigurace účtu proveďte následující kroky:

    ![Povolení externích] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Povolení externích")

    1.  Zaškrtněte políčko **Povolit externí se tak mohli ověřovat**.
    2.  V části **Hlavní ovládací prvek Login** vyberte kartu **SmarterU** .
    3.  V části **Výchozí přihlášení uživatele** vyberte kartu **SmarterU** .
    4.  Zaškrtněte políčko **Povolit Okta**.
    5.  Kopírování obsahu metadat stažený soubor a potom je vložte do textového pole **Metadat Okta** .
    6.  Klikněte na **Uložit**.

7.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit SmarterU, musí být zřízení do SmarterU.  
V případě SmarterU zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **SmarterU** .

2.  Přejděte na **Uživatelé**.

3.  V oddílu uživatelských proveďte následující kroky:

    ![Nového uživatele] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Nového uživatele")

    1.  Klikněte na tlačítko **+ uživatele**.
    2.  Zadejte hodnoty související atributu uživatelský účet Azure AD do následujících textová pole: **primární e-mailu**, **ID zaměstnance**, **heslo**, **ověřit heslo**, **příslušné jméno**, **Příjmení**.
    3.  Klikněte na **aktivní**.
    4.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další SmarterU uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou SmarterU k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Přiřazení uživatelů k SmarterU, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **SmarterU **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).