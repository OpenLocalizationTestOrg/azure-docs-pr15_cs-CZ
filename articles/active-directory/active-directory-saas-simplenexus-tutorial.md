<properties 
    pageTitle="Kurz: Azure Active Directory integrace s SimpleNexus | Microsoft Azure" 
    description="Naučte se používat SimpleNexus s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Kurz: Azure Active Directory integrace s SimpleNexus
  
Cílem tohoto kurzu je zobrazit integrace Azure a SimpleNexus.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   SimpleNexus jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti SimpleNexus (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili SimpleNexus.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro SimpleNexus
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scénář")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Povolení integrace aplikací pro SimpleNexus
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace SimpleNexus.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace SimpleNexus, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **jednoduchý nexus –**.

    ![Galerie aplikace] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Galerie aplikace")

7.  V podokně výsledků vyberte **SimpleNexus**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Jednoduchý nexus –] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Jednoduchý nexus –")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit SimpleNexus s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **SimpleNexus** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k SimpleNexus** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **SimpleNexus znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://simplenexus.com/CompanyName\_přihlášení*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v SimpleNexus** klikněte na tlačítko **Stáhnout metadat**a nastavíte přesměrování soubor metadat pro tým podpory SimpleNexus.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Konfigurace jednotného přihlašování")

    >[AZURE.NOTE] Jednotné přihlašování, musí být povolené tým podpory SimpleNexus.

5.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit SimpleNexus, musí být zřízení do SimpleNexus.  
V případě SimpleNexus zřizování se ručně naplánovaný úkol provedly správce klienta.

>[AZURE.NOTE] Můžete použít jakékoli další SimpleNexus uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou SimpleNexus k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Přiřazení uživatelů k SimpleNexus, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **SimpleNexus **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).