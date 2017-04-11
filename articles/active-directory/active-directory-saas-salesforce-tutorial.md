<properties
    pageTitle="Kurz: Azure Active Directory integrace s služby Salesforce | Microsoft Azure"
    description="Zjistěte, jak pomocí služby Salesforce Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Kurz: Jak integrace služby Salesforce s Azure Active Directory

Tento kurz vám ukáže, jak připojit prostředí služby Salesforce Azure Active Directory. Naučíte se, jak nakonfigurovat jednotného přihlašování k služby Salesforce, jak povolit zřizování automatické uživatele a jak přiřadit uživatelům přístup k služby Salesforce.

##<a name="prerequisites"></a>Zjistit předpoklady pro

1. S přístupem k Azure Active Directory přes [Azure klasické portál](https://manage.windowsazure.com), musíte musíte nejdřív platné Azure předplatné.

2. Pokud nemáte platného klienta v [Salesforce.com](https://www.salesforce.com/).

> [AZURE.IMPORTANT] Pokud používáte **zkušební** účet Salesforce.com, pak nebude možné nakonfigurovat zřizování automatické uživatele. Zkušební účty nemají potřebnou úroveň přístupu rozhraní API povoleno, dokud jsou zakoupené.
> 
> Toto omezení můžete orientace pomocí [bezplatné vývojář účtu](https://developer.salesforce.com/signup) pro dokončení tohoto kurzu.

Pokud používáte prostředí izolovaného prostoru služby Salesforce, přečtěte si téma [kurz integrace služby Salesforce izolovaného prostoru](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Výukové programy pro video

Tento kurz pomocí níže uvedených videa, které sledujete.

**Videokurz první část: jak povolit jednotné přihlašování**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Druhá část Videokurz: Jak automatizovat zřizování uživatelů**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Krok 1: Přidání služby Salesforce do adresáře

1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![V levém navigačním podokně vyberte služby Active Directory.][0]

2. V seznamu **adresáře** vyberte adresář, který byste chtěli přidat služby Salesforce k.

3. Klikněte na **aplikace** v horní nabídce.

    ![Klikněte na aplikace.][1]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Kliknutím na Přidat přidejte novou aplikaci.][2]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Klikněte na Přidat aplikaci z galerie.][3]

6. Do **vyhledávacího pole**zadejte **služby Salesforce**. Potom vyberte **služby Salesforce** výsledky a klikněte na **Dokončit** přidat aplikaci.

    ![Přidání služby Salesforce.][4]

7. Teď byste měli vidět rychlé úvodní stránku pro služby Salesforce:

    ![Služby Salesforce na úvodní stránku v Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Krok 2: Povolení jednotné přihlašování

1. Před můžete nakonfigurovat jednotné přihlašování, musíte nastavit a nasadit vlastní doménu pro prostředí služby Salesforce. Další informace o tom, jak to udělat najdete v článku [Nastavení domény název](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Na služby Salesforce na úvodní stránce Azure AD klikněte na tlačítko **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlašování tlačítka][6]

3. Otevře se dialogové okno a zobrazí se obrazovka s dotazem, "Jakým způsobem uživatelé přihlásit k služby Salesforce?" Vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Vyberte Azure AD jednotné přihlášení][7]

    > [AZURE.NOTE] Další informace o různých jednoho přihlašování možností, [Po kliknutí sem](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. Na stránce **Konfigurovat nastavení aplikace** vyplňte **Znaménko na adresu URL** tak, že zadáte svoji adresu URL domény služby Salesforce pomocí v tomto formátu:
 - Účet organizace:`https://<domain>.my.salesforce.com`
 - Karta Vývojář v účtu:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Zadejte vaše přihlašovací adresa URL][8]

5. Na stránce **konfigurovat jednotné přihlašování v služby Salesforce** klikněte na **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Stáhněte si certifikát][9]

6. Otevření na nové kartě v prohlížeči a přihlaste se k účtu správce služby Salesforce.

7. V navigačním podokně **Správce** klikněte na **Ovládací prvky zabezpečení** rozbalte části související. Klikněte na **Nastavení jednotného přihlašování**.

    ![Klikněte na nastavení jednotného přihlašování ve skupinovém rámečku ovládací prvky zabezpečení][10]

8. Na stránce **Nastavení jednotného přihlašování** kliknutím na tlačítko **Upravit** .

    ![Klikněte na tlačítko Upravit][11]

    > [AZURE.NOTE] Pokud není možné povolit nastavení jednotného přihlašování pro váš účet služby Salesforce, budete muset kontaktování podpory služby Salesforce prvku za účelem funkce povolenou.

9. Vyberte **Povolit SAML**a klikněte na **Uložit**.

    ![Vyberte SAML povolena][12]

10. Konfigurace SAML jednoho přihlašování nastavení, klikněte na **Nový**.

    ![Vyberte SAML povolena][13]

11. Na stránce **SAML jednoho přihlašování nastavení úpravy** zkontrolujte následující konfigurace:

    ![Snímek obrazovky s konfigurace, které je třeba][14]

 - Pole **název** zadejte do pole popisný název pro tuto konfiguraci. Poskytování hodnotu pro **název** automatické vyplnění textového pole **Název rozhraní API** .

 - V Azure AD zkopírujte hodnotu **Vystavitel URL** a potom je vložte do pole **Vystavitel** v služby Salesforce.

 - Do pole **Entity Id textového pole**napište název domény služby Salesforce pomocí následujícího vzorce:
     - Účet organizace:`https://<domain>.my.salesforce.com`
     - Karta Vývojář v účtu:`https://<domain>-dev-ed.my.salesforce.com`

 - Klikněte na **Procházet** nebo **Zvolte soubor** k otevření dialogu **Zvolte soubor k nahrání** vyberte certifikát služby Salesforce a potom klikněte na **Otevřít** nahrát certifikát.

 - **Typ identit SAML**zaškrtněte políčko **výraz obsahuje uživatelské jméno salesforce.com**.

 - **Umístění identit SAML**vyberte možnost **Identity je v elementu NameIdentifier příkazu předmět**

 - V Azure AD zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do pole **Adresa URL přihlašovací poskytovatele Identity** v služby Salesforce.

 - **Služby, které iniciuje požádat o vazbu poskytovatelem**vyberte **HTTP přesměrování**.

 - Nakonec klikněte na **Uložit** , aby vaše SAML přihlašování nastavení jednotného.

12. V levém navigačním podokně v služby Salesforce klikněte **Domain Management** rozbalte části související a potom klikněte na **My Domain**.

    ![Klikněte na mé doméně][15]

13. Přejděte dolů do části **Konfigurace ověření** a klikněte na tlačítko **Upravit** .

    ![Klikněte na tlačítko Upravit][16]

14. V části **Služba ověřování** vyberte popisný název konfiguraci SAML jednotného přihlašování k a potom klikněte na **Uložit**.

    ![Vyberte konfiguraci jednotné přihlašování][17]

    > [AZURE.NOTE] Pokud vyberete víc ověřovací služba, pak když se uživatelé pokusí zahajte jednotného přihlašování k vašemu prostředí služby Salesforce jim se výzva k vyberte které ověřovací služba chtějí se přihlašujete. Pokud nechcete, aby to bylo možné, pak je vhodné **ponechat všechny ostatní ověřování služby zrušené zaškrtnutí políčka**.

15. V Azure AD zaškrtněte políčko potvrzení jednotné přihlašování povolit certifikát, který jste nahráli do služby Salesforce. Klepněte na tlačítko **Další**.

    ![Zaškrtněte políčko potvrzení][18]

16. Na poslední stránce dialogu zadejte Pokud chcete dostávat e-mailová oznámení chyb a upozornění související s údržbu tento přihlašování jednotné v e-mailovou adresu. 

    ![Zadejte e-mailové adresy.][19]

17. Klikněte na **Dokončit** zavřete dialogové okno. Testování konfigurace, viz následující oddíl [Přiřadit uživatelům služby Salesforce](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Krok 3: Povolit automatické zřizování uživatelů

1. Na stránce Azure AD úvodní služby Salesforce kliknete na tlačítko **Konfigurace zřizování uživatelů** .

    ![Klikněte na tlačítko Konfigurace zřizování uživatelů][20]

2. V dialogovém okně **Konfigurovat zřizování uživatele** zadejte do svého správce služby Salesforce uživatelské jméno a heslo.

    ![Zadejte uživatelské jméno správce nebo heslo][21]

    > [AZURE.NOTE] Konfigurujete provozním prostředí, doporučený postup při vytvoření nového účtu správce v služby Salesforce speciálně pro tento krok. Tento účet musí mít přiřazenou v služby Salesforce profil **Správce systému** .

3. Získat tokenu zabezpečení služby Salesforce, otevřete nové kartě a přihlaste se do jednoho účtu správce služby Salesforce V pravém horním rohu stránky klikněte na své jméno a potom klikněte na **Nastavení**.

    ![Klikněte na své jméno a potom klikněte na nastavení][22]

4. V levém navigačním podokně klikněte na **osobní** rozbalte části související a potom klikněte na **Obnovit moje zabezpečení tokenu**.

    ![Klikněte na své jméno a potom klikněte na nastavení][23]

5. Na stránce **Obnovit moje zabezpečení tokenu** kliknete na tlačítko **Obnovit tokenu zabezpečení** .

    ![Přečtěte si upozornění.][24]

6. Vyhledejte doručené poště e-mail přidružený k tomuto účtu správce. Vyhledejte e-mailu z Salesforce.com obsahující nová tokenu zabezpečení.

7. Zkopírujte tokenu, přejděte do okna Azure AD a vložit ho do pole **Tokenu zabezpečení uživatele** . Klepněte na tlačítko **Další**.

    ![Vložte do tokenu zabezpečení][25]

8. Na stránce potvrzení můžete dostávat e-mailová oznámení pro při vytváření selhání. Klikněte na **Dokončit** zavřete dialogové okno.

    ![Zadejte e-mailové adresy pro příjem oznámení ve formě][26]

##<a name="step-4-assign-users-to-salesforce"></a>Krok 4: Přiřazení uživatelů do služby Salesforce

1. Testování konfigurace, začněte tím, vytvoření nového účtu testovat v adresáři.

2. Na stránce služby Salesforce Snadné spuštění klikněte na tlačítko **Přiřadit uživatele** .

    ![Klikněte na přiřadit uživatelům][27]

3. Vyberte svůj testovací uživatelské jméno a klikněte na tlačítko **přiřadit** v dolní části obrazovky:

 - Pokud jste to ještě povolte a zobrazí se následující výzvy k potvrzení zřizování automatické uživatele:

        ![Confirm the assignment.][28]

 - Pokud jste povolili automatizované zřizování uživatelů, uvidíte výzva k definování, jaký typ služby Salesforce profil by měl mít uživatel. Nově zřizování uživatelů objevit ve vašem prostředí služby Salesforce po několika minutách.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Pokud jsou zřizování prostředí **developer** služby Salesforce, máte hodně omezený počet licencí k dispozici u každého profilu. Proto doporučujeme zřizování uživatelů **Chatter bezplatné uživatelském** profilu, který má k dispozici 4,999 licence.

4. Otestujte vaše nastavení jednotného přihlašování, otevřít Panel přístup na [https://myapps.microsoft.com](https://myapps.microsoft.com/)a přihlaste se k testovací účet a klikněte na **služby Salesforce**.

##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
