<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Clever | Microsoft Azure" 
    description="Naučte se používat Clever s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Kurz: Azure Active Directory integrace s Clever

Cílem tohoto kurzu je zobrazit integrace Azure a Clever. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Inteligentní klienta

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu inteligentní společnosti (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Clever.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Clever
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-clever-tutorial/IC798977.png "Scénář")
##<a name="enabling-the-application-integration-for-clever"></a>Povolení integrace aplikací pro Clever

Cílem tento oddíl je přehledu jak povolit integraci aplikace Clever.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Clever, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-clever-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-clever-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Clever**.

    ![Galerie aplikace] (./media/active-directory-saas-clever-tutorial/IC798978.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Clever**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Inteligentní] (./media/active-directory-saas-clever-tutorial/IC798979.png "Inteligentní")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Clever s účtem v Azure AD pomocí federace podle protokolu SAML.  
Inteligentní aplikace očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![Atributy] (./media/active-directory-saas-clever-tutorial/IC798980.png "Atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Clever** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clever-tutorial/IC784682.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Clever** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clever-tutorial/IC798981.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Inteligentní znaménko na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace inteligentní (například: *https://clever.com/in/azsandbox*) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-clever-tutorial/IC798982.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Clever** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clever-tutorial/IC798983.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu inteligentní společnosti.

6.  Na panelu nástrojů klikněte na **Rychlé přihlásit**.

    ![Rychlé přihlášení] (./media/active-directory-saas-clever-tutorial/IC798984.png "Rychlé přihlášení")

7.  Na stránce **Rychlé přihlášení** proveďte následující kroky:

    ![Rychlé přihlášení] (./media/active-directory-saas-clever-tutorial/IC798985.png "Rychlé přihlášení")

    1.  Zadejte **Přihlašovací adresa URL**.  

        >[AZURE.NOTE] **Přihlašovací adresa URL** je vlastní hodnoty.
Skutečné hodnoty můžete získat od týmu podpory inteligentní.

    2.  **Systém Identity**vyberte **služby AD FS**.
    3.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clever-tutorial/IC798986.png "Konfigurace jednotného přihlašování")

9.  V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-clever-tutorial/IC795920.png "Atributy")

10. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![SAML tokenu atributy] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML tokenu atributy")

  	|Název atributu|Hodnota atributu|
  	|---|---|
  	|clever.student.credentials.District\_uživatelské jméno|User.userPrincipalName|

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.
    3.  **Hodnota atributu** do textového pole vyberte hodnotu atributu zobrazí pro tento řádek.
    4.  Klikněte na **dokončení**.

11. Klikněte na **použít změny**.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Clever, musí být zřízení do Clever.  
V případě Clever zřizování se ručně naplánovaný úkol, kterou je potřeba provést inteligentní technická podpora.

>[AZURE.NOTE] Můžete použít jakékoli další nástroje vytváření inteligentní uživatelského účtu nebo rozhraní API poskytovanou Clever k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Přiřazení uživatelů k Clever, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Clever **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-clever-tutorial/IC798987.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-clever-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
