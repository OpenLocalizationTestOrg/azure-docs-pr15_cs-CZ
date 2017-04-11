<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Benefitsolver | Microsoft Azure"
    description="Naučte se používat Benefitsolver s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Kurz: Azure Active Directory integrace s Benefitsolver

Cílem tohoto kurzu je zobrazit integrace Azure a Benefitsolver.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Benefitsolver jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Benefitsolver.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Benefitsolver
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scénář")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Povolení integrace aplikací pro Benefitsolver

Cílem tento oddíl je přehledu jak povolit integraci aplikace Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Benefitsolver, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Benefitsolver**.

    ![Galerie aplikace] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Benefitsolver**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Benefitsolver s účtem v Azure AD pomocí federace na základě SAML protokolu.  
Aplikace Benefitsolver očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![Atributy] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Benefitsolver** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Benefitsolver** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Konfigurace nastavení aplikace")

    1.  **Přihlaste se na adresu URL** do textového pole zadejte **http://azure.benefitsolver.com**.
    2.  **Adresa URL odpověď** do textového pole zadejte **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Klikněte na tlačítko **Další**.

4.  Na stránce **konfigurovat jednotné přihlašování v Benefitsolver** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat svému týmu podpory Benefitsolver.

    >[AZURE.NOTE] Váš tým podpory Benefitsolver musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurace jednotného přihlašování")

7.  V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Atributy")

8.  Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![Atributy] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atributy")

  	|Název atributu|Hodnota atributu|
  	|---|---|
  	|ClientID|Potřebujete k získání tuto hodnotu od týmu podpory Benefitsolver.|
  	|ClientKey|Potřebujete k získání tuto hodnotu z Benefitsolver týmu podpory.|
  	|LogoutURL|Potřebujete k získání tuto hodnotu od týmu podpory Benefitsolver.|
  	|Číslo zaměstnance|Potřebujete k získání tuto hodnotu od týmu podpory Benefitsolver.|

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.
    3.  **Hodnota atributu** do textového pole vyberte hodnotu atributu zobrazí pro tento řádek.
    4.  Klikněte na **dokončení**.

9.  Klikněte na **použít změny**.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Benefitsolver, musí být zřízení do Benefitsolver.  
V případě Benefitsolver je dat zaměstnanců aplikaci vyplněné prostřednictvím sčítání souboru z počítače HRIS (obvykle je každou noc).  

>[AZURE.NOTE] Můžete použít jakékoli další Benefitsolver uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Benefitsolver k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Přiřazení uživatelů k Benefitsolver, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Benefitsolver **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
