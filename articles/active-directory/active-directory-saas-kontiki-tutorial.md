<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Kontiki | Microsoft Azure" 
    description="Naučte se používat Kontiki s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Kurz: Azure Active Directory integrace s Kontiki
  
Cílem tohoto kurzu je zobrazit integrace Azure a Kontiki.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Kontiki jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Kontiki (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Kontiki.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Kontiki
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Scénář")
##<a name="enabling-the-application-integration-for-kontiki"></a>Povolení integrace aplikací pro Kontiki
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Kontiki.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Kontiki, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Kontiki**.

    ![Galerie aplikace] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Kontiki**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Kontiki s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Kontiki** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Konfigurace jednoho přihlášení")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Kontiki** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Kontiki na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k Kontiki (například: "*https://company.mc.eval.kontiki.com/*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Kontiki** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Konfigurace jednoho přihlášení")

5.  Odešlete metadatafile Kontiki tým podpory.

    >[AZURE.NOTE] Konfigurace přihlašování musí vykonat Kontiki tým podpory. Zobrazí se vám oznámení hned po dokončení konfigurace.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Konfigurace jednoho přihlášení")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Kontiki.  
Při přiřazený uživatel se pokusí přihlásit Kontiki pomocí panelu aplikace access, Kontiki kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Kontiki.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Přiřazení uživatelů k Kontiki, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Kontiki **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).