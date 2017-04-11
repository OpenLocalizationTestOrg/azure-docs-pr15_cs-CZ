<properties
    pageTitle="Azure Active Directory B2C: Konfigurace účtu Microsoft | Microsoft Azure"
    description="Poskytnutí registrace přihlašování a zákazníkům s účtem Microsoft aplikace, které jsou chráněny Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Registrace a poskytněte přihlášení uživatelé s účty Microsoft

## <a name="create-a-microsoft-account-application"></a>Vytvoření aplikace účtu Microsoft

Používat účet Microsoft jako zprostředkovatelem identit v Azure Active Directory (Azure AD) B2C, potřebujete k vytvoření účtu aplikace Microsoft a poskytne ji správné parametry. Je třeba účet Microsoft, můžete to udělat. Pokud nemáte jeden, dostanete na [https://www.live.com/](https://www.live.com/).

1. Přejděte na [Portál registrace aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovací údaje účtu Microsoft.
2. Klikněte na **Přidat aplikaci**.

    ![Microsoft účet – přidání nové aplikace](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Zadejte **název** aplikace a klikněte na **vytvořit aplikaci**.

    ![Účet Microsoft - název aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Zkopírujte hodnotu **Id aplikace**. Budete potřebovat můžete konfigurovat účet Microsoft jako zprostředkovatelem identit ve vašem klientovi.

    ![Účet Microsoft - Id aplikace](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Klikněte na **Přidat platformě** a zvolte **Web**.

    ![Microsoft účet – přidání platformy](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Účtu Microsoft – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v poli **Přesměrovat URI** . **{Klienta}** nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com).

    ![Účet Microsoft - přesměrování adresy URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. V části **Tajemství aplikace** klikněte na **Generovat nové heslo** . Zkopírujte nové heslo na obrazovce. Budete potřebovat můžete konfigurovat účet Microsoft jako zprostředkovatelem identit ve vašem klientovi. Toto heslo je důležité zabezpečení pověření.

    ![Microsoft účet - generovat nové heslo](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Účet Microsoft - nového hesla](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Zaškrtněte políčko, že **podporují Live SDK** v části **Upřesnit možnosti** . Klikněte na **Uložit**.

    ![Účet Microsoft - podpora Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfigurace účtu Microsoft jako zprostředkovatelem identit ve vašem klientovi

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Na zásuvné funkce B2C klikněte na **Zprostředkovatelé identit jiní**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte popisný **název** pro konfiguraci zprostředkovatele identit. Zadejte například "MSA".
5. Klikněte na **Typ zprostředkovatele identit**, vyberte **účet Microsoft**a klikněte na **OK**.
6. Klikněte na **Nastavení této zprostředkovatele identit** a zadejte Id aplikace a heslo účtu aplikace Microsoft, která jste dříve vytvořili.
7. Klikněte na tlačítko **OK** a potom klikněte na **vytvořit** a uložit konfigurace účtu Microsoft.
