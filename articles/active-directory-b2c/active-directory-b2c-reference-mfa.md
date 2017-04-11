<properties
    pageTitle="Azure Active Directory B2C: Vícefaktorové ověřování | Microsoft Azure"
    description="Jak povolit Vícefaktorové ověřování aplikace spotř webových zajištěná Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Povolení Vícefaktorové ověřování spotř webových aplikacích

Azure Active Directory (Azure AD) B2C integrována přímo [Azure Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication.md) tak, že můžete přiřadit vrstvě druhý cenného papíru registrace přihlašování a prostředí v spotř webových aplikacích. A to aniž byste museli psát jedním řádkem kódu. Momentálně podporujeme ověření telefonního hovoru a text zprávy. Pokud jste už vytvořili registrace přihlašování a zásady, můžete pořád povolit Vícefaktorové ověřování.

> [AZURE.NOTE]
Vícefaktorové ověřování může být užitečné také při vytváření registrace přihlašování a zásady, nejenom úpravou existujícího zásady.

Tato funkce pomáhá aplikace zpracování scénářů, třeba takto:

- Nevyžadují Vícefaktorové ověřování pro přístup k jedné aplikace, ale nevyžadují ho pro přístup k nějaký jiný. Například spotřebitele přihlásit do aplikace automatického pojištění pod svým účtem sociální nebo místní, ale musí ověřit telefonního čísla, před přístup k aplikaci domácí pojištění registrované ve stejném adresáři.
- Nevyžadují Vícefaktorové ověřování pro přístup k aplikaci obecně, ale nevyžadují ho pro přístup k citlivých částí v něm obsažené. Například příjemce se přihlásit k aplikaci bankovní s sociální nebo místního účtu a zůstatek účtu zaškrtněte, ale musí ověřit telefonní číslo před pokusem o elektronický převod.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Upravit registrace zásady Povolit Vícefaktorové ověřování

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **Registrace zásady**.
3. Klikněte na registrace zásad (například "B2C_1_SiUp") a otevřete ho.
4. Klikněte na **vícefaktorové ověřování** a změnit **Stav** na **zapnuto**. Klikněte na **OK**.
5. Klepněte na tlačítko **Uložit** v horní části zásuvné.

Můžete použít funkci "Spustit" na zásady pro ověření prostředí pro uživatele. Zkontrolujte následující položky:

Spotřebitelský účet získá vytvořené v adresáři před nastane Vícefaktorové ověřování. V průběhu kroku příjemce požádáni o zadejte své telefonní číslo a ověřit. Pokud bude ověření úspěšné, telefonní číslo je připojen k spotřebitelského účtu pro pozdější použití. I když spotřebitele zruší nebo vynechává, kterými váš kolega můžete požádáni o ověření telefonního čísla znovu během další přihlašovací (s podporou Multi-Factor Authentication).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Upravit zásady přihlášení povolte Vícefaktorové ověřování

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **Sign in Zásady**.
3. Klikněte na svoje přihlašovací zásady (například "B2C_1_SiIn") a otevřete ho. Klikněte na **Upravit** v horní části zásuvné.
4. Klikněte na **vícefaktorové ověřování** a změnit **Stav** na **zapnuto**. Klikněte na **OK**.
5. Klepněte na tlačítko **Uložit** v horní části zásuvné.

Můžete použít funkci "Spustit" na zásady pro ověření prostředí pro uživatele. Zkontrolujte následující položky:

Když spotřebitele přihlásí (pomocí účtu sociální nebo místní), pokud ověřenou telefonní číslo je připojen k spotřebitelský účet, kterými váš kolega se zobrazí dotaz, kvůli ověření ho. Pokud žádná telefonní číslo je připojen, příjemce požádáni o zadáte a ověřit. Na úspěšném ověření se telefonní číslo připojuje k spotřebitelského účtu pro pozdější použití.

## <a name="multi-factor-authentication-on-other-policies"></a>Vícefaktorové ověřování na jiné zásady

Jak je popsáno registrace & přihlašovací zásad výše uvedené, je také možné povolit vícefaktorové ověřování na registrace nebo přihlašovací zásady a heslo resetovat zásady. Bude k dispozici brzy bude k dispozici pro úpravy zásady profilu.
