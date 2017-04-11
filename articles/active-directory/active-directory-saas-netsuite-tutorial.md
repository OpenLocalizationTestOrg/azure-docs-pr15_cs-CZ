<properties
    pageTitle="Kurz: Azure Active Directory integrace s NetSuite | Microsoft Azure"
    description="Naučte se používat NetSuite s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Kurz: Jak integrovat NetSuite s Azure Active Directory

Tento kurz vám ukáže, jak připojit prostředí NetSuite služby Azure Active Directory (Azure AD). Naučíte se, jak nakonfigurovat jednotného přihlašování k NetSuite, jak povolit zřizování automatické uživatele a jak přiřadit uživatelům přístup k NetSuite. 

##<a name="prerequisites"></a>Zjistit předpoklady pro

1. S přístupem k Azure Active Directory přes [Azure klasické portál](https://manage.windowsazure.com), musíte musíte nejdřív platné Azure předplatné.

2. Musíte mít přístup k předplatnému [NetSuite](http://www.netsuite.com/portal/home.shtml) správce. Mohli byste použít bezplatnou zkušební účet buď služby.

##<a name="step-1-add-netsuite-to-your-directory"></a>Krok 1: Přidání NetSuite do adresáře

1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![V levém navigačním podokně vyberte služby Active Directory.][0]

2. V seznamu **adresáře** vyberte adresář, který byste chtěli přidat NetSuite k.

3. Klikněte na **aplikace** v horní nabídce.

    ![Klikněte na aplikace.][1]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Kliknutím na Přidat přidejte novou aplikaci.][2]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Klikněte na Přidat aplikaci z galerie.][3]

6. Do **vyhledávacího pole**zadejte **NetSuite**. Potom vyberte **NetSuite** výsledky a klikněte na **Dokončit** přidat aplikaci.

    ![Přidání NetSuite.][4]

7. Teď byste měli vidět rychlé úvodní stránku pro NetSuite:

    ![NetSuite na úvodní stránku v Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Krok 2: Povolení jednotné přihlašování

1. V Azure AD na úvodní stránce NetSuite klikněte na tlačítko **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlašování tlačítka][6]

2. Otevře se dialogové okno a zobrazí se obrazovka s dotazem "Jakým způsobem uživatelé přihlásit k NetSuite?" Vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Vyberte Azure AD jednotné přihlášení][7]

    > [AZURE.NOTE] Další informace o různých jednoho přihlašování možností, [Po kliknutí sem](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Na stránce **Konfigurovat nastavení aplikace** , do pole **Adresa URL odpověď** zadejte svoji adresu URL klienta NetSuite pomocí jedné z následujících formátů:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Zadejte do pole Adresa URL klienta][8]

4. Na stránce **konfigurovat jednotné přihlašování v NetSuite** klikněte na **Stáhnout metadat**a uložte soubor certifikátu místně na vašem počítači.

    ![Stáhněte si metadata.][9]

5. Otevření na nové kartě v prohlížeči a přihlaste se k webu společnosti Netsuite jako správce.

6. Na panelu nástrojů v horní části stránky klikněte na **Nastavení**a potom klikněte na **Správce instalace**.

    ![Přejděte na správce instalace][10]

7. V seznamu **Úkoly nastavení** vyberte **Integrace**.

    ![Přejděte na integraci][11]

8. V části **Správa ověřování** knihovny.tlačítko **SAML jednotné přihlašování**

    ![Přejděte na SAML jednotné přihlašování][12]

9. Na stránce **Nastavení SAML** proveďte následující kroky:

    - V Azure Active Directory zkopírujte hodnotu **Adresa URL vzdálené přihlášení** a vložit ho do pole **Přihlašovací stránku poskytovatelů identit** v NetSuite.

        ![Stránka nastavení SAML.][13]

    - V NetSuite vyberte **Primární metody ověřování**.

    - Pro pole s názvem **Metadat zprostředkovatele identit SAMLV2**vyberte **Nahrát soubor IDP Metadata**. Klikněte na **Procházet** a nahrajte soubor metadat, který jste stáhli v kroku #4.

        ![Nahrání metadata][16]

    - Klikněte na **Odeslat**.

9. V Azure AD zaškrtněte políčko potvrzení jednotné přihlašování povolit certifikát, který jste nahráli NetSuite. Klepněte na tlačítko **Další**.

    ![Zaškrtněte políčko potvrzení][14]

10. Na poslední stránce dialogu zadejte Pokud chcete dostávat e-mailová oznámení chyb a upozornění související s údržbu tento přihlašování jednotné v e-mailovou adresu. 

    ![Zadejte e-mailové adresy.][15]

11. Klikněte na **Dokončit** zavřete dialogové okno. Potom klikněte na **atributy** v horní části stránky.

    ![Klikněte na atributy.][17]

12. Klikněte na **Přidat uživatele atribut**.

    ![Klepněte na Přidat uživatele atribut.][18]

13. Pole **Název atributu** zadejte v `account`. **Hodnota atributu** pole zadejte ID NetSuite účtu. Pokyny pro vyhledání ID účtu jsou uvedeny níže:

    ![Přidání ID NetSuite účtu.][19]

    - V NetSuite klikněte na **Nastavení** z nabídky horním navigačním panelu.
    - Klikněte v části **Nastavení** levé navigační nabídky, vyberte v části **Integrace** a klikněte na **Předvolby webové služby**.
    - ID účtu NetSuite zkopírujte a vložte ji do pole **Hodnota atributu** v Azure AD.

        ![Získání účtu ID][20]

14. V Azure AD klikněte na **Dokončit** dokončete přidání atribut SAML. V nabídce dole klepněte na **Použít změny** .

    ![Uložte provedené změny.][21]

15. Před uživatelé mohou provádět jednotného přihlašování do NetSuite, musí nejdřív být přiřazena příslušná oprávnění v NetSuite. Postupujte podle pokynů a přiřadit oprávnění.

    - V nabídce horním navigačním panelu klikněte na **Nastavení**a potom klikněte na **Správce instalace**.

        ![Přejděte na správce instalace][10]

    - V nabídce levém navigačním panelu vyberte **Uživatelé/role**klikněte na **Spravovat role**.

        ![Přejděte na Správa rolí][22]

    - Klikněte na **novou roli**.

    - Zadejte **název** pro novou roli je a zaškrtněte políčko **Jednoho přihlašování pouze** .

        ![Pojmenujte novou roli.][23]

    - Klikněte na **Uložit**.

    - V nabídce na horní klikněte na **oprávnění**. Klepněte na **Nastavení**.

        ![Přejděte na oprávnění][24]

    - Vyberte **Set Up SAM jednotného přihlašování**a potom klikněte na **Přidat**.

    - Klikněte na **Uložit**.

    - V nabídce horním navigačním panelu klikněte na **Nastavení**a potom klikněte na **Správce instalace**.

        ![Přejděte na správce instalace][10]

    - V nabídce levém navigačním panelu vyberte **Uživatelé/role**klikněte na **Spravovat uživatele**.

        ![Přejděte na Správa uživatelů][25]

    - Vyberte testovací uživatelské jméno. Klikněte na **Upravit**.

        ![Přejděte na Správa uživatelů][26]

    - V dialogovém okně role vyberte roli, že jste vytvořili a klikněte na **Přidat**.

        ![Přejděte na Správa uživatelů][27]

    - Klikněte na **Uložit**.

19. Testování konfigurace, viz oddíl níž s názvem [Přiřadit uživatelům NetSuite](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Krok 3: Povolit automatické zřizování uživatelů

> [AZURE.NOTE] Ve výchozím nastavení se přidá do kořenové pobočky prostředí NetSuite zřizování uživatelů.

1. V Azure Active Directory na úvodní stránce NetSuite, klikněte na **Konfigurovat zřizování uživatelů**.

    ![Konfigurace zřizování uživatelů][28]

2. V dialogovém okně, které se otevře zadejte svoje přihlašovací údaje správce pro NetSuite a potom klikněte na tlačítko **Další**.

    ![Zadejte svoje přihlašovací údaje správce NetSuite.][29]

3. Na stránce potvrzení zadejte v e-mailové adresy pro příjem oznámení o selhání zajišťování.

    ![Potvrďte.][30]

4. Klikněte na **Dokončit** zavřete dialogové okno.

##<a name="step-4-assign-users-to-netsuite"></a>Krok 4: Přiřazení uživatelů do NetSuite

1. Testování konfigurace, začněte vytvářet nový testovací účet v adresáři.

2. Na stránce NetSuite Snadné spuštění klikněte na tlačítko **Přiřadit uživatele** .

    ![Klikněte na přiřadit uživatelům][31]

3. Vyberte svůj testovací uživatelské jméno a klikněte na tlačítko **přiřadit** v dolní části obrazovky:

 - Pokud jste to ještě povolte a zobrazí se následující výzvy k potvrzení zřizování automatické uživatele:

        ![Confirm the assignment.][32]

 - Pokud jste povolili automatické uživatele zřizování, uvidíte výzva k definování, jaký druh role v NetSuite by měl mít uživatel. Nově zřizování uživatelů objevit ve vašem prostředí NetSuite po několika minutách.

4. Otestujte vaše nastavení jednotného přihlašování otevřít Panel přístup na [https://myapps.microsoft.com](https://myapps.microsoft.com/), přihlaste se k testovací účet a klikněte na **NetSuite**.

##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
