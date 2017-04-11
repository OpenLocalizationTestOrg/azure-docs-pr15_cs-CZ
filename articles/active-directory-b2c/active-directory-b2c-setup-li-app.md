<properties
    pageTitle="Azure Active Directory B2C: Konfigurace LinkedIn | Microsoft Azure"
    description="Poskytnutí registrace přihlašování a zákazníkům s účty LinkedIn na vaše aplikace, které jsou chráněny Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Registrace a poskytněte přihlašovací spotřebitele k účtům LinkedIn

## <a name="create-a-linkedin-application"></a>Vytvoření aplikace LinkedIn

Jako poskytovatele identity v Azure Active Directory (Azure AD) B2C použít LinkedIn, potřebujete k vytvoření aplikace LinkedIn a poskytne ji správné parametry. Potřebujete LinkedIn účet, můžete to udělat. Pokud nemáte jeden, dostanete na [https://www.linkedin.com/](https://www.linkedin.com/).

1. Přejděte na [Web vývojáři LinkedIn](https://www.developer.linkedin.com/) a přihlaste se pomocí svých přihlašovacích údajů LinkedIn účet.
2. Kliknutím na **Moje aplikace** v horní nabídek a potom klikněte na **Vytvořit aplikaci**.

    ![LinkedIn – novou aplikaci](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Ve formuláři **vytvořit novou aplikaci** zadejte příslušné informace (**Název společnosti**, **název**, **Popis**, aplikace a **Adresa URL loga, **Použijte aplikaci**, **Adresa URL webu**, **E-mailová adresa** **Telefon do zaměstnání**)**.
4. Souhlasíte s **LinkedIn rozhraní API podmínky použití** a klikněte na **Odeslat**.

    ![LinkedIn - Register aplikace](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Zkopírujte hodnoty **ID klienta** a **Tajné klienta**. (Najdete je v části **Ověřování klíče**.) Budete potřebovat oba konfigurovat LinkedIn jako zprostředkovatelem identit ve vašem klientovi.

    >[AZURE.NOTE] **Tajná klienta** je důležité zabezpečení pověření.

6. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v poli **Oprávnění přesměrování adresy URL** (pod **OAuth 2.0**). **{Klienta}** nahraďte názvem vašeho klienta (například contoso.onmicrosoft.com). Klikněte na tlačítko **Přidat**a potom klikněte na **Aktualizovat**. **{Klienta}** hodnotu malá a velká písmena.

    ![LinkedIn – instalace aplikace](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurace LinkedIn jako zprostředkovatelem identit ve vašem klientovi

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Na zásuvné funkce B2C klikněte na **Zprostředkovatelé identit jiní**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte popisný **název** pro konfiguraci zprostředkovatele identit. Zadejte například "LI".
5. Klikněte na **Typ zprostředkovatele identit**, vyberte **LinkedIn**a klikněte na **OK**.
6. Klikněte na **Nastavení této zprostředkovatele identit** a zadejte ID klienta a tajná klientské aplikace LinkedIn, který jste vytvořili.
7. Klikněte na **OK** a potom klikněte na **vytvořit** a uložit konfiguraci LinkedIn.
