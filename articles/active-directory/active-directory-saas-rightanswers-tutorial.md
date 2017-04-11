<properties 
    pageTitle="Kurz: Azure Active Directory integrace s RightAnswers | Microsoft Azure" 
    description="Naučte se používat RightAnswers s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Kurz: Azure Active Directory integrace s RightAnswers
  
Cílem tohoto kurzu je zobrazit integrace Azure a RightAnswers. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   RightAnswers jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili RightAnswers.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro RightAnswers
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scénář")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Povolení integrace aplikací pro RightAnswers
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace RightAnswers.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace RightAnswers, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **RightAnswers**.

    ![Galerie aplikace] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Galerie aplikace")

7.  V podokně výsledků vyberte **RightAnswers**a potom klikněte na **Dokončit** přidat aplikaci.
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit RightAnswers s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **RightAnswers** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k RightAnswers** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** **Odhlásit na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace RightAnswers (například: *https://fortify.rightanswers.com/portal/ss/*) a potom na tlačítko **Další**.

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Konfigurace nastavení aplikace")

4.  Na stránce **konfigurovat jednotné přihlašování v RightAnswers** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat svému týmu podpory RightAnswers.

    >[AZURE.NOTE] Váš tým podpory RightAnswers musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit RightAnswers, musí být zřízení do RightAnswers.  
V případě RightAnswers zřizování je automatické úkolu.  
Existuje žádné úkol za vás.
  
Automatické vytvoření uživatelů v případě potřeby při prvním jednoho přihlašování pokusu o.

>[AZURE.NOTE]Můžete použít jakékoli další RightAnswers uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou RightAnswers k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Přiřazení uživatelů k RightAnswers, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **RightAnswers **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).