<properties
    pageTitle="Azure Active Directory B2C: Vlastní atributy | Microsoft Azure"
    description="Použití vlastní atributy v Azure Active Directory B2C shromažďování informací o uživatele"
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

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Umožňuje shromáždit informace o vaší spotřebitele vlastní atributy

Adresáře B2C Azure Active Directory (Azure AD) je součástí vestavěnou sadu informací (atributy): křestní jméno, příjmení, Město, PSČ a další atributy. Každý spotř webových aplikací má však specifickým požadavkům na jaké atributy shromažďovat od spotřebitele. Pomocí Azure AD B2C můžete rozšířit sady atributů uložené v jednotlivých spotřebitelského účtu. Můžete vytvořit vlastní atributy na [Azure portál](https://portal.azure.com/) a použít v registrace zásady, jak je ukázáno v následujícím příkladu. Můžete taky číst a psát tyto atributy pomocí rozhraní [API Azure AD grafu](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Vlastní atributy pomocí [rozšíření schématu adresáře rozhraní API Azure AD grafu](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Vytvořit vlastní atribut

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **atributy uživatele**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Zadejte **název** vlastní atribut (například "ShoeSize") a volitelně **Popis**. Klikněte na **vytvořit**.

    > [AZURE.NOTE]
    Pouze "Řetězec" **Datový typ** momentálně neexistuje.

Vlastní atribut je teď dostupná v seznamu **atributy uživatele**a pro použití v registrace zásady.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Použití vlastní atribut v registrace zásad

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **Registrace zásady**.
3. Klikněte na registrace zásad (například "B2C_1_SiUp") a otevřete ho. Klikněte na **Upravit** v horní části zásuvné.
4. Klikněte na **Registrace atributy** a vyberte vlastní atribut (například "ShoeSize"). Klikněte na **OK**.
5. Klikněte na **deklarace aplikace** a vyberte atribut vlastní. Klikněte na **OK**.
6. Klepněte na tlačítko **Uložit** v horní části zásuvné.

Můžete použít funkci "Spustit" na zásady pro ověření prostředí pro uživatele. Teď by měl v tématu "ShoeSize" v seznamu atributy shromažďované během spotř registrace a podívejte se, jak token odeslaný zpátky do okna aplikace.

## <a name="notes"></a>Poznámky

- Spolu s registrace zásady vlastní atributy lze také v registrace nebo přihlašovací zásady a úpravy zásady profilu.
- Existuje známé omezení vlastní atributy. Vytváří se jenom poprvé, pracovní postup slouží v jakékoli zásady a ne v případě, můžete ho přidat do seznamu **atributy uživatele**.
