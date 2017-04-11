<properties
    pageTitle="Azure Active Directory B2C: Google + konfigurace | Microsoft Azure"
    description="Poskytnutí registrace přihlašování a zákazníkům s účty Google + v aplikacích zabezpečeným službou Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Registrace a poskytněte přihlašovací spotřebitele s Google + účty

## <a name="create-a-google-application"></a>Vytvoření aplikace Google +

Použití Google + jako zprostředkovatelem identit v Azure Active Directory (Azure AD) B2C, potřebujete k vytvoření aplikace Google + a poskytne ji správné parametry. Potřebujete účet Google + akce. Pokud nemáte jeden, dostanete na [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Přejděte do [Google vývojáři konzoly](https://console.developers.google.com/) a přihlaste se pomocí svého Google + přihlašovací údaje účtu.
2. Klikněte na **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.

    ![Google + - Začínáme](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - nového projektu](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Klikněte na **Správce rozhraní API** a v levém navigačním panelu klikněte na příkaz **přihlašovací údaje** .
4. Klikněte na kartu **OAuth souhlas obrazovky** nahoře.

    ![Google + – přihlašovací údaje](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Vyberte nebo zadejte platnou **e-mailovou adresu**, zadejte **název produktu**a klikněte na tlačítko **Uložit**.

    ![Google + - OAuth souhlas obrazovky](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Klikněte na **nové přihlašovací údaje** a pak zvolte **ID klienta OAuth**.

    ![Google + - OAuth souhlas obrazovky](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Ve skupinovém rámečku **Typ aplikace**vyberte **Webová aplikace**.

    ![Google + - OAuth souhlas obrazovky](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Zadejte **název** aplikace, zadejte `https://login.microsoftonline.com` v poli **oprávnění JavaScript typů** a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v poli **autorizované přesměrovat URI** . **{Klienta}** nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com). **{Klienta}** hodnotu malá a velká písmena. Klikněte na **vytvořit**.

    ![Google + – vytvoření ID klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Zkopírujte hodnoty **ID klienta** a **tajná klienta**. Budete potřebovat oba konfigurace Google + jako zprostředkovatelem identit ve vašem klientovi. **Tajná klienta** je důležité zabezpečení pověření.

    ![Google + - tajná klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurace služby Google + jako zprostředkovatelem identit ve vašem klientovi

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Na zásuvné funkce B2C klikněte na **Zprostředkovatelé identit jiní**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte popisný **název** pro konfiguraci zprostředkovatele identit. Například zadejte "G +".
5. Klikněte na **Typ zprostředkovatele identit**, vyberte **Google**a klikněte na **OK**.
6. Klikněte na **Nastavení této zprostředkovatele identit** a zadejte ID klienta a tajná klientské aplikace Google +, který jste vytvořili.
7. Klikněte na tlačítko **OK** a potom klikněte na **vytvořit** a uložit konfiguraci Google +.
