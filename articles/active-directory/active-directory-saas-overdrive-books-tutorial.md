<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Overdrive knihy | Microsoft Azure" 
    description="Naučte se používat Overdrive publikace s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Kurz: Azure Active Directory integrace s Overdrive knihy
  
Cílem tohoto kurzu je zobrazit integrace Azure a OverDrive.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   OverDrive jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili OverDrive bude moct jednotného přihlášení do aplikace na webu společnosti OverDrive (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro OverDrive
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scénář")
##<a name="enabling-the-application-integration-for-overdrive"></a>Povolení integrace aplikací pro OverDrive
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace OverDrive.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Postup povolení integrace aplikace pro OverDrive, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **OverDrive**.

    ![Galerie aplikace] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Galerie aplikace")

7.  V podokně výsledků vyberte **OverDrive**a potom klikněte na **Dokončit** přidat aplikaci.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit OverDrive s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **OverDrive** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Povolení jednotné přihlašování")

2.  Na stránce **jak chcete uživatelům přihlásit k OverDrive** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **OverDrive znak v adresa URL** zadejte adresu URL pomocí následujícího vzorce "*http://mslibrarytest.libraryreserve.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Konfigurace aplikace URL")

4.  Na **konfigurovat jednotné přihlašování v OverDrive** stránce pro stažení souboru metadat a pošlete ji týmu podpory OverDrive.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Konfigurace jednotného přihlašování")

    >[AZURE.NOTE]Tým podpory OverDrive nakonfiguruje jednotného přihlašování pro vás a odešle vám oznámení po dokončení konfigurace.

5.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování OverDrive.  
Pokud přiřazený uživatel se pokusí přihlásit OverDrive, OverDrive účet se automaticky vytvoří v případě potřeby.

>[AZURE.NOTE]Můžete použít jakékoli další OverDrive uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou OverDrive k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Přiřazení uživatelů k OverDrive, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **OverDrive **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).