<properties
    pageTitle="Azure Active Directory B2C: Konfigurace služby Facebook | Microsoft Azure"
    description="Poskytnutí registrace přihlašování a zákazníkům s účty služby Facebook v aplikacích zabezpečeným službou Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Registrace a poskytněte přihlášení uživatelé s oprávněními Facebooku

## <a name="create-a-facebook-application"></a>Vytvořit aplikaci pro Facebook

Jako poskytovatele identity v Azure Active Directory (Azure AD) B2C použít Facebook, musíte vytvořit aplikaci pro Facebook a poskytne ji správné parametry. Potřebujete Facebookový účet, můžete to udělat. Pokud nemáte jeden, dostanete na [https://www.facebook.com/](https://www.facebook.com/).

1. Přejděte na web [služby Facebook pro vývojáře](https://developers.facebook.com/) a přihlaste se pomocí svých přihlašovacích údajů účtu Facebooku.
2. Pokud jste to ještě neudělali, musíte zaregistrovat vývojář Facebook. K tomuto účelu klikněte na **zaregistrovat** (v pravém horním rohu stránky), přijměte zásady společnosti Facebook a dokončete kroky registrace.
3. Kliknutím na **Moje aplikace** a potom klikněte na **Přidat novou aplikaci**. Zvolte **webu** jako platformu a potom klikněte na **Přeskočit a vytvořte ID aplikace**.

    ![Facebooku – přidat nové aplikace](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebooku – přidat novou aplikaci – Web](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebooku – vytvoření ID aplikace](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Ve formuláři zadejte **Zobrazované jméno**, **E-mailu kontaktu**s platnou, příslušné **kategorie**a klikněte na **Vytvořit ID aplikace**. Při této akci musí přijmout zásady platformy Facebook a proveďte kontrolu zabezpečení online.

    ![Facebooku – vytvoření nové ID aplikace](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. V levém navigačním panelu klikněte na **Nastavení** .
6. Klikněte na tlačítko **+ Přidat platformy** a pak vyberte **Web**.

    ![Facebooku – nastavení](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebookový – nastavení – Web](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Zadejte [https://login.microsoftonline.com/](https://login.microsoftonline.com/) do pole **Adresa URL webu** a potom klikněte na **Uložit změny**.

    ![Facebooku – adresu URL webu](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Zkopírujte hodnotu **ID aplikace**. Klikněte na **prezentace** a zkopírujte hodnotu **Tajná aplikace**. Budete potřebovat oba ke konfiguraci služby Facebook jako zprostředkovatelem identit ve vašem klientovi. **Tajná aplikace** je důležité zabezpečení pověření.

    ![Facebooku – ID aplikace a aplikace tajná](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Klikněte na tlačítko **+ Přidat produktu** v levém navigačním panelu a potom na tlačítko **Začínáme** vedle **Facebook přihlášení**.

    ![Facebooku – přihlašovací Facebooku](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v poli **platné OAuth přesměrovat URI** v části **Nastavení OAuth klienta** . **{Klienta}** nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com). Klikněte na **Uložit změny** v dolní části stránky.

    ![Facebooku – OAuth přesměrování URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Aby bylo možné použít tak, že Azure AD B2C aplikace Facebook, musíte ho zpřístupnit veřejnosti. Lze provést kliknutím na **Revize aplikace** v levém navigačním panelu a tím, že přepínačem v horní části stránky na hodnotu **Ano** a **potvrďte**kliknutím na.

    ![Facebooku – veřejné aplikace](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurace služby Facebook jako zprostředkovatelem identit ve vašem klientovi

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Na zásuvné funkce B2C klikněte na **Zprostředkovatelé identit jiní**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte popisný **název** pro konfiguraci zprostředkovatele identit. Zadejte například "FB".
5. Klikněte na **Typ zprostředkovatele identit**, vyberte **Facebook**a klikněte na **OK**.
6. Klikněte na **Nastavení této zprostředkovatele identit** a zadejte ID aplikace a aplikace tajná (aplikace Facebook, které jste dříve vytvořili) v **ID klienta** a **tajná klienta** polí v tomto pořadí.
7. Klikněte na tlačítko **OK**a potom klikněte na **vytvořit** a uložit konfiguraci služby Facebook.
