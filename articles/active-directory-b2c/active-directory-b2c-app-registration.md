<properties
    pageTitle="Azure Active Directory B2C: Registrace aplikace | Microsoft Azure"
    description="Jak si zaregistrovat aplikace s Azure Active Directory B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrace aplikace

## <a name="prerequisite"></a>Předpoklady

Vytvářet aplikace, která přijímá spotř registrace přihlašování a, musíte nejdřív pro registraci aplikace s klientem Azure Active Directory B2C. Pokud potřebujete vlastního klienta pomocí kroků uvedených v tématu [Vytvoření Azure AD B2C klienta](active-directory-b2c-get-started.md). Po provedení všech kroků v tomto článku se budete mít zásuvné funkce B2C připnuté vaší Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Přejděte na zásuvné funkce B2C

Pokud máte zásuvné funkce B2C připnuté vaší Startboard, zobrazí se zásuvné hned, jak se přihlásit na [portál Azure](https://portal.azure.com/) jako globální správce klienta B2C.

Zásuvné se dostanete kliknutím na **Procházet** a **Azure AD B2C** v levém navigačním podokně na [Azure portálu](https://portal.azure.com/).

> [AZURE.IMPORTANT] Musíte být globální správce tenanta B2C být mít přístup k funkcím zásuvné B2C. Globální správce z jiných klienta nebo uživatele z libovolné klienta k němu nemají přístup.  Přepnutí na vašeho klienta B2C pomocí klienta přepínač v pravém horním rohu portálu Azure.

## <a name="register-an-application"></a>Registrace aplikace

1. Na zásuvné funkce B2C na portálu Azure klikněte na **aplikace**.
2. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
3. Zadejte **název** aplikace, která bude popisovat aplikace zákazníkům. Je třeba zadat "Contoso B2C aplikace".
4. Pokud vytváříte aplikace založené na webu, zapíná a vypíná **Zahrnout v prohlížeči / webového rozhraní API** přepnout na hodnotu **Ano**. **Adresy URL odpovědět** jsou koncové body, kde Azure AD B2C vrátí klíčovými žádosti o aplikaci. Zadejte například `https://localhost:44321/`. Pokud webové aplikace se také volání některých webového rozhraní API zajištěná Azure AD B2C, je vhodné **Aplikace tajné** taky můžete vytvořit kliknutím na tlačítko **Generování klíče** .

    > [AZURE.NOTE] **Tajná aplikace** je důležité zabezpečení pověření a by měl být zabezpečený řádně podporovat.

5. Pokud vytváříte mobilní aplikace, zapíná a vypíná přepínačem **native client – zahrnout** na hodnotu **Ano**. Zkopírujte dolů výchozí **Přesměrovat URI** , který se automaticky vytvoří.
6. Klikněte na **vytvořit** k registraci aplikace.
7. Klepněte na aplikaci, kterou jste právě vytvořili a zkopírujte dolů globálně jedinečné **ID klienta aplikace** , které budete používat později v kódu.

> [AZURE.IMPORTANT] Aplikace vytvořené v zásuvné funkce B2C potřeba spravovat ve stejném umístění. Pokud upravíte B2C aplikací pomocí prostředí PowerShell nebo jiného portálu se stanou nepodporované a pravděpodobně nefunguje v Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Vytvoření aplikace rychlým startem

Teď, když máte aplikaci registrovaný u Azure AD B2C, musíte udělat jednu z našich výukové programy pro rychlého nastavení a zprovoznění. Tady je pár doporučení:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
