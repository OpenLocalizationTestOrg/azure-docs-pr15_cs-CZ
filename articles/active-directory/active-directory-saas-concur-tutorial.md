<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Concur | Microsoft Azure" 
    description="Naučte se používat Concur s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Kurz: Azure Active Directory integrace s Concur  


Cílem tohoto kurzu je zobrazit integrace Azure a Concur.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Ke klientovi Concur

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Concur
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-concur-tutorial/IC769766.png "Scénář")

>[AZURE.NOTE] Konfigurace předplatného Concur federované SSO prostřednictvím SAML je samostatných úkolů, které musíte kontaktovat Concur provádět.

##<a name="enabling-the-application-integration-for-concur"></a>Povolení integrace aplikací pro Concur

Cílem tento oddíl je přehledu jak povolit integraci aplikace Concur.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Concur, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** , vyberte v adresáři, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-concur-tutorial/IC700994.png "Aplikace")

4.  Otevřete **Aplikaci Galerie**, klikněte na **Přidat aplikaci**a potom klikněte na **Přidat aplikaci pro svoji organizaci používat**.

    ![Co chcete udělat?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Co chcete udělat?")

5.  Do **vyhledávacího pole**zadejte **Concur**.

    ![Galerie aplikace] (./media/active-directory-saas-concur-tutorial/IC721727.png "Galerie aplikace")

6.  V podokně výsledků vyberte **Concur**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vyústit] (./media/active-directory-saas-concur-tutorial/IC721728.png "Vyústit")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Concur s účtem v Azure AD pomocí federace podle protokolu SAML.

>[AZURE.NOTE] Konfigurace předplatného Concur federované SSO prostřednictvím SAML je samostatných úkolů, které musíte kontaktovat Concur provádět.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Concur **aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-concur-tutorial/IC769767.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Concur** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-concur-tutorial/IC769768.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Vyústit znak v adresu URL** do textového pole zadejte svoji concur klienta přihlašovací adresu URL a klikněte na **Další**: 

    ![Konfigurace přihlášení adresy URL] (./media/active-directory-saas-concur-tutorial/IC769769.png "Konfigurace přihlášení adresy URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Concur** proveďte následující kroky.

    ![Konfigurace přihlášení adresy URL] (./media/active-directory-saas-concur-tutorial/IC769770.png "Konfigurace přihlášení adresy URL")

    1.  Klikněte na stáhnout metadata a pak safe datový soubor se svým počítačem.
    2.  Obraťte se na tým podpory Concur pro nastavení jednotného přihlašování pro vašeho klienta.
    3.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .  

    >[AZURE.NOTE] Konfigurace předplatného Concur federované SSO prostřednictvím SAML je samostatných úkolů, které musíte kontaktovat Concur provádět.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Cílem tento oddíl je přehledu povolení zřizování adresářové služby Active Directory pro Concur.

Povolit aplikace služby výdaje na úkol, že má být správné nastavení a používání profil Správce služeb Web. Nepřidávat jednoduše role správce WS do existujícího správce profilu, který použijete pro u & T funkce správy.

Vyústit konzultanty nebo správce klienta musí vytvořit různých profil správce webové služby a správce klienta musí používat profil pro správce služby webové funkce (například povolení aplikace). Tyto profily uchovává odděleně od správce klienta denní u & T správce profilu (profil správce u & T nemají přiřazenou roli WSAdmin).

Při vytváření profilu se dá použít pro povolení aplikace zadejte správce klienta název do pole v profilu uživatele. Tento krok přiřadí vlastnictví profilu. Po vytvoření profilů klienta musíte se přihlásit pomocí tento profil klikněte na tlačítko "*Povolit*" aplikace Partner v rámci nabídky webové služby.

Z těchto důvodů neměli tato akce provést profil, který používají normální u & T správy.

1.  Klient musí být ten, který klepne na tlačítko "*Ano*" v okně dialog, která se zobrazí po povolení aplikace. Tlačítko potvrzuje, že je klient nevadí aplikace Partner přístup ke svým datům, aby se nebo partnera nelze klikněte na tlačítko Ano.
2.  Pokud správce klienta, který povolil aplikace pomocí Správce u & T profilu ponechá společnosti (výsledkem profil je deaktivovat), žádné aplikace povoleno pomocí, aby profil nebudou fungovat, dokud aplikace je povolený k jinému profilu aktivní WS správce. Z tohoto důvodu máte k vytvoření různých správce WS profily.
3.  Pokud správce ponechá společnosti, název přidružené k profil správce WS lze změnit na náhradní správce, pokud budete chtít bez vlivu, že aplikace povolené protože profilu nemusí deaktivovat

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlášení k vašemu tenantovi **Concur** .

2.  V nabídce **Správa** vyberte **Webové služby**.

    ![Concur klienta] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur klienta")

3.  V podokně **Webové služby** na levé straně zvolte **Povolit aplikaci Partner**.

    ![Povolit aplikaci Partner] (./media/active-directory-saas-concur-tutorial/IC721730.png "Povolit partnerů aplikaci")

4.  Ze seznamu **Povolení aplikace** vyberte **Azure Active Directory**a potom klikněte na **Povolit**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Kliknutím na tlačítko **Ano** zavřete dialogové okno **Potvrzení akce** .

    ![Potvrďte] (./media/active-directory-saas-concur-tutorial/IC721732.png "Potvrďte")

6.  Na portálu Azure klasické vyberte ze seznamu aplikací a otevře se stránka dialogového okna **Concur** **Concur** .

7.  Otevřete dialogové okno stránky **Nastavení zřizování uživatele** , klikněte na **Konfigurovat zřizování uživatelů**.

8.  Zadejte uživatelské jméno a heslo správce Concur a klikněte na tlačítko **Další**.

9.  Dokončete konfiguraci na stránce **potvrzení** klikněte na tlačítko **Dokončit** .

Nyní můžete vytvořit testovací účet, až 10 minut a ověřte, že účet synchronizování k Concur.
##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Přiřazení uživatelů k Concur, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Concur **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-concur-tutorial/IC769771.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-concur-tutorial/IC767830.png "Ano")

Teď by měl až 10 minut a ověřte, že účet synchronizování k Concur.

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
