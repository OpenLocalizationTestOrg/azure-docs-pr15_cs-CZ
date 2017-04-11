<properties
   pageTitle="Správa identit víceklientské aplikací | Microsoft Azure"
   description="Doporučené postupy pro správu ověřování, povolení a identity v víceklientské aplikace."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Správa identit víceklientské aplikací v Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tato řada popisuje doporučené postupy pro multitenancy, při použití Azure AD pro správu ověřování a identity.

Když vytváříte víceklientské aplikace, jednu z první stránky je Správa identit uživatele, protože teď každý uživatel patří do klienta. Příklad:

- Přihlaste se pomocí své organizace přihlašovací údaje uživatele.
- Uživatelé mají mít přístup k jejich organizace dat, ale ne data, která patří jiné klientů.
- Organizace můžete registraci aplikace a potom přiřazování rolí aplikace její členy.

Azure Active Directory (Azure AD) obsahuje některé skvělé funkce, které podporují všech těchto scénářích.

Pro tuto řadu článků, jsme vytvořili také dokončení [začátku do konce implementaci] [ tailspin] víceklientské aplikace. Tyto články obsahovat, co jste se dozvěděli při vytváření aplikace. Začínáme s aplikací, najdete v tématu [spuštění aplikace průzkumy](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Tady je seznam článků v této řadě:

- [Úvod do správy identit víceklientské aplikací](guidance-multitenant-identity-intro.md)
- [Informace o aplikaci Tailspin průzkumy](guidance-multitenant-identity-tailspin.md)
- [Ověření pomocí Azure AD a OpenID připojení](guidance-multitenant-identity-authenticate.md)
- [Práce s identitami založených na deklaraci identity](guidance-multitenant-identity-claims.md)
- [Registrace a klient rychlého připojení](guidance-multitenant-identity-signup.md)
- [Role aplikací](guidance-multitenant-identity-app-roles.md)
- [Povolení na základě rolí a na základě zdroje](guidance-multitenant-identity-authorize.md)
- [Zabezpečení back-end webového rozhraní API](guidance-multitenant-identity-web-api.md)
- [Ukládání do mezipaměti tokeny OAuth2 přístupu](guidance-multitenant-identity-token-cache.md)
- [Federování se službou AD FS zákazníka víceklientské aplikací v Azure](guidance-multitenant-identity-adfs.md)
- [Získání přístupu tokenů z Azure AD pomocí výrazu klienta](guidance-multitenant-identity-client-assertion.md)
- [Pomocí klávesy trezoru chránit tajemství aplikace](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
