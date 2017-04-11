<properties 
    pageTitle="Kurz: Azure Active Directory integrace s popisem trasy na Microsoft | Microsoft Azure" 
    description="Naučte se používat tras na Microsoft s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Kurz: Azure Active Directory integrace s popisem trasy na Microsoft

Cílem tohoto kurzu je zobrazit integrace služby Azure Active Directory a tras na Microsoft.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Pokyny v předplatném Microsoft

Pokud v předplatném Microsoft ještě nemáte federované směrech, e-mailové žádosti o s "*service@DirectionsOnMicrosoft.com*".

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí jednotného přihlašování Azure Active Directory uživatele, které jste přiřadili směrech Microsoft.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací směrech na Microsoft
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scénář")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Povolení integrace aplikací směrech na Microsoft

Cílem tento oddíl je přehledu jak povolit integraci aplikace jak Microsoft.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Postup povolení integrace aplikace jak Microsoft, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **směrech Microsoft**.

    ![Galerie aplikace] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galerie aplikace")

7.  V podokně výsledků vyberte **tras na Microsoft**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Scénář] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scénář")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit tras na Microsoft s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **tras na Microsoft** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k směrech Microsoft** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD Singel přihlašování] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** odhlásit na adresu URL do textového pole zadejte **https://www.directionsonmicrosoft.com/user/login**a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v směrech Microsoft** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Konfigurace jednotného přihlašování")

5.  Odeslat soubor metadat podle pokynů na tým podpory společnosti Microsoft (*service@DirectionsOnMicrosoft.com*). Pokud chcete povolit tras na tým podpory společnosti Microsoft vyhledejte členství ve federovaných webu, musí být údajů o vaší společnosti v e-mailu.

    >[AZURE.NOTE] Jednotné přihlašování směrech na Microsoft je potřeba povolit podle pokynů na tým podpory společnosti Microsoft.
Zobrazí se oznámení a při jednotného přihlašování je povolená.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Neexistuje žádné akce položky konfigurace uživatele zřizování tras na Microsoft.  
Při přiřazené uživatel se pokusí přihlásit směrech Microsoft pomocí panelu přístup, směrech Microsoft kontroluje, zda uživatel vyskytuje. Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky podle pokynů na Microsoft.
##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Přiřazení uživatelů k směrech Microsoft, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **směrech Microsoft **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ano")
