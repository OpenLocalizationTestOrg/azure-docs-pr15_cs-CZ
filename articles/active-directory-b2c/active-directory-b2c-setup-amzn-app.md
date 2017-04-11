<properties
    pageTitle="Azure Active Directory B2C: Konfigurace Amazon | Microsoft Azure"
    description="Poskytnutí registrace přihlašování a zákazníkům s účty Amazon v aplikacích zabezpečeným službou Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Registrace a poskytněte přihlášení uživatelé s oprávněními Amazon

## <a name="create-an-amazon-application"></a>Vytvoření aplikace Amazon

Použít Amazon jako zprostředkovatelem identit v B2C Azure Active Directory (Azure AD), musíte vytvořit aplikaci Amazon a poskytne ji správné parametry. Musíte mít účet Amazon akce. Pokud nemáte jeden, dostanete na [http://www.amazon.com/](http://www.amazon.com/).

1. Přejděte na [Centrum pro vývojáře Amazon](https://login.amazon.com/) a přihlaste se pomocí svých přihlašovacích údajů Amazon účtu.
2. Pokud jste to ještě neudělali, klikněte na **Zaregistrovat**, postupujte podle pokynů registrace vývojář a přijměte zásadu.
3. Klikněte na **zaregistrovat nové aplikace**.

    ![Registrace nové aplikace na webu Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Zadejte informace o aplikaci (**název**, **Popis**a **Adresu URL oznámení ochrany osobních údajů**) a klikněte na tlačítko **Uložit**.

    ![Poskytování informací aplikace pro registraci nové aplikace na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. V části **Nastavení Web** zkopírujte hodnoty **ID klienta** a **Tajné klienta**. (Potřebujete klikněte na tlačítko **Zobrazit tajné** zobrazíte.) Je nutné mít, aby konfigurovat Amazon jako zprostředkovatelem identit ve vašem klientovi. Klikněte na **Upravit** v dolní části. **Tajná klienta** je důležité zabezpečení pověření.

    ![Poskytuje ID klienta a tajné klienta pro nové aplikace na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Zadejte `https://login.microsoftonline.com` v poli **Povolené typů JavaScript** a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v poli **Povolené vrátit adresy URL** . **{Klienta}** nahraďte názvem vašeho klienta (například contoso.onmicrosoft.com). Klikněte na **Uložit**. **{Klienta}** hodnotu malá a velká písmena.

    ![Poskytování JavaScript typů a vrácená adres URL pro nové aplikace na Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurace Amazon jako zprostředkovatelem identit ve vašem klientovi

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Na zásuvné funkce B2C klikněte na **Zprostředkovatelé identit jiní**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte popisný **název** pro konfiguraci zprostředkovatele identit. Zadejte například "Amzn".
5. Klikněte na **Typ zprostředkovatele identit**, vyberte **Amazon**a klikněte na **OK**.
6. Klikněte na **Nastavení této zprostředkovatele identit** a zadejte ID klienta a tajná klientské aplikace Amazon, který jste vytvořili.
7. Klikněte na tlačítko **OK** a potom klikněte na **vytvořit** a uložit konfiguraci Amazon.
