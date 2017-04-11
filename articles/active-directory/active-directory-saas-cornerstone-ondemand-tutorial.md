<properties 
    pageTitle="Kurz: Azure Active Directory integrace s základního pilíře OnDemand | Microsoft Azure" 
    description="Naučte se používat základního pilíře OnDemand s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Kurz: Azure Active Directory integrace s základního pilíře OnDemand

Cílem tohoto kurzu je zobrazit integrace Azure a základního pilíře OnDemand.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Do základního pilíře OnDemand klienta

Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili základního pilíře OnDemand bude moct jednotného přihlášení do aplikace na webu společnosti základního pilíře OnDemand (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro základního pilíře OnDemand
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scénář")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Povolení integrace aplikací pro základního pilíře OnDemand

Cílem tento oddíl je přehledu jak povolit integraci aplikace základního pilíře OnDemand.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace základního pilíře OnDemand, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **základního pilíře ondemand**.

    ![Galerie aplikace] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Galerie aplikace")

7.  V podokně výsledků vyberte možnost **Základního pilíře OnDemand**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Základního pilíře OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Základního pilíře OnDemand")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit základního pilíře OnDemand s účtem v Azure AD pomocí federaci podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Základního pilíře OnDemand** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k základního pilíře OnDemand** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD jednotné přihlášení] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD jednotné přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Základního pilíře OnDemand Sign v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*http://company.csod.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v základního pilíře OnDemand** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Konfigurace jednotného přihlašování")

5.  Nástroj poslat že tým podpory následující položky, které chcete OnDemand základního pilíře:

    1.  Pokud chcete stažený certifikátu
    2.  Hodnotu **Adresa URL vzdálené přihlášení**
    3.  Hodnotu **Adresa URL vzdálené odhlásit** .

    >[AZURE.NOTE] Jednotné přihlašování, musí být nakonfigurované tak, že tým podpory základního pilíře OnDemand.
Po dokončení konfigurace, zobrazí se vám oznámení od týmu podpory.

6.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit základního pilíře OnDemand, musí být zřízení do základního pilíře OnDemand.  
V případě základního pilíře OnDemand zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Odeslání informací o (například: název, Emial) o Azure AD uživateli, chcete-li poskytování OnDemand základního pilíře tým podpory.

>[AZURE.NOTE] Můžete použít jakékoli další OnDemand základního pilíře uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou základního pilíře OnDemand k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Přiřazení uživatelů k základního pilíře OnDemand, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Základního pilíře OnDemand **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Přiřazení uživatelů] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Přiřazení uživatelů")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
