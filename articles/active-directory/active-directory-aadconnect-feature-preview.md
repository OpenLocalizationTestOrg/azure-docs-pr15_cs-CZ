<properties
   pageTitle="Azure AD Connect: Funkce v náhledu | Microsoft Azure"
   description="Toto téma popisuje v dalších funkcí podrobných dat, které jsou v náhledu v Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Další informace o funkcích ve verzi preview
Toto téma popisuje funkce nyní ve verzi preview.

## <a name="group-writeback"></a>Skupina zpětného zápisu
Možnost pro skupinu zpětným v volitelné funkce umožní zpětným **Skupin Office 365** do struktury se serverem Exchange nainstalovaný. Toto je skupině, která je vždy standardní v cloudu. Pokud máte místním systémem Exchange, pak je můžete odepsat tyto skupiny do místního nasazení tak mohou uživatelé, kteří mají poštovní schránce Exchange místní odesílání a přijímání e-mailů z těchto skupin.

Další informace o skupin Office 365 a jak je lze používat najdete [tady](http://aka.ms/O365g).

Tato skupina bude vyjádřených jako distribuční skupinu v místní služby AD DS. Místního serveru Exchange musí být na 8 (vydána v březnem 2015) kumulativní aktualizaci Exchange 2013 nebo 2016 Exchange rozpoznatelný tento nový typ skupiny.

**Poznámky během náhledu**

- Atribut knihy address není aktuálně vyplněné v náhledu. Bez Tenhle atribut skupiny se nezobrazí z globálního seznamu adres. Nejjednodušším způsobem k naplnění Tenhle atribut je získáte pomocí rutiny prostředí PowerShell systému Exchange `update-recipient`.
- Pouze strukturami s schématu Exchange jsou platné cíle pro skupiny. Pokud byl zjištěn žádné Exchange, skupina zpětným nebude možné povolit.
- Aktuálně podporuje pouze jeden doménové nasazení organizaci Exchange. Pokud máte víc než jednu organizace místním systémem Exchange, pak budete potřebovat řešení pro místní GALSync pro tyto skupiny zobrazit v dalších doménových.
- Funkce zpětným skupina zpracovává aktuálně skupiny zabezpečení nebo distribuční skupiny.

>[AZURE.NOTE] Předplatné Azure AD Premium je potřebný pro zpětným skupiny.

## <a name="user-writeback"></a>Uživatel zpětného zápisu
> [AZURE.IMPORTANT] Funkce náhledu zpětným uživatele byla odebrána na Azure AD Connect aktualizovat srpen 2015. Pokud jste ho povolili, měli byste zakázat tuto funkci.

## <a name="next-steps"></a>Další kroky
Pokračujte [Vlastní instalace Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
