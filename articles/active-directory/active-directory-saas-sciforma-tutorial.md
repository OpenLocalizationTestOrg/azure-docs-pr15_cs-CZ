<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Sciforma | Microsoft Azure" 
    description="Naučte se používat Sciforma s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-ad-integration-with-sciforma"></a>Kurz: Azure AD integrace s Sciforma
  
Cílem tohoto kurzu je zobrazit integrace Azure a Sciforma.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Sciforma klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Sciforma (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Sciforma.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Sciforma
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-sciforma-tutorial/IC777369.png "Scénář")
##<a name="enabling-the-application-integration-for-sciforma"></a>Povolení integrace aplikací pro Sciforma
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Sciforma.

###<a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Sciforma, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-sciforma-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-sciforma-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-sciforma-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-sciforma-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Sciforma**.

    ![Galerie aplikace] (./media/active-directory-saas-sciforma-tutorial/IC777370.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Sciforma**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Sciforma] (./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Sciforma s účtem v Azure AD pomocí federaci podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Sciforma** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sciforma-tutorial/IC777372.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Sciforma** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sciforma-tutorial/IC777373.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Sciforma znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Sciforma.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-sciforma-tutorial/IC777374.png "Konfigurace aplikace URL")

4.  V **konfigurovat jednotné přihlašování v Sciforma** stránky, pokud si chcete stáhnout metadata, klepněte na tlačítko **Stáhnout metadata**, a potom datový soubor, místně jako **c:\\SciformaMetaData.xml**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sciforma-tutorial/IC777375.png "Konfigurace jednotného přihlašování")

5.  Tento soubor metadat pro Sciforma vpřed tým podpory. Potřeb týmu podpory nakonfiguruje jednotného přihlašování pro vás.

6.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sciforma-tutorial/IC777376.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Sciforma.  
Při přiřazeného uživatel se pokusí přihlásit Sciforma pomocí panelu přístup, Sciforma kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Sciforma.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Přiřazení uživatelů k Sciforma, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Sciforma **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-sciforma-tutorial/IC777377.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-sciforma-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).