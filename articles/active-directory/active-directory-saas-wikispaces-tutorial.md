<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Wikispaces | Microsoft Azure" 
    description="Naučte se používat Wikispaces s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Kurz: Azure Active Directory integrace s Wikispaces
  
Cílem tohoto kurzu je zobrazit integrace Azure a Wikispaces.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Wikispaces jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Wikispaces (služby poskytovatelem, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Wikispaces.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Wikispaces
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Povolení integrace aplikací pro Wikispaces
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Wikispaces.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Wikispaces, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Wikispaces**.

    ![Galerie aplikace] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Wikispaces**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Wikispaces s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Wikispaces** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Wikispaces** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Wikispaces na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*http://company.wikispaces.net*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Wikispaces** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Konfigurace jednotného přihlašování")

5.  Odešlete metadatafile Wikispaces tým podpory.

    >[AZURE.NOTE] Konfigurace přihlašování musí vykonat Wikispaces tým podpory. Zobrazí se vám oznámení hned po dokončení konfigurace.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Wikispaces, musí být zřízení do Wikispaces.  
V případě Wikispaces zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Wikispaces** jako správce.

2.  Přejděte na **Členové**.

    ![Členové] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Členové")

3.  Klikněte na **Pozvat uživatele**.

    ![Pozvat osoby] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Pozvat osoby")

4.  V části **Pozvat ostatní** proveďte následující kroky:

    ![Pozvat osoby] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Pozvat osoby")

    1.  Zadejte **uživatelská jména nebo e-mailovou adresu** platného AAD účtu, který chcete ustanovení do související textová pole.
    2.  Klikněte na **Odeslat**.  

        >[AZURE.NOTE] Majitele účtu služby Azure Active Directory obdrží e-mailu a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Wikispaces uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Wikispaces k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Přiřazení uživatelů k Wikispaces, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Wikispaces **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).