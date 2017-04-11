<properties
   pageTitle="Azure AD Connect synchronizace: rozšíření Directory | Microsoft Azure"
   description="Toto téma popisuje funkce rozšíření adresáře v Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect synchronizace: rozšíření Directory
Rozšíření Directory umožňuje rozšířit schématu v Azure AD pomocí vlastní atributy z místní služby Active Directory. Tato funkce umožňuje vytvářet aplikace LOB jinými atributy můžete pokračovat ve správě místní. Tyto atributy můžete spotřebované prostřednictvím [rozšíření directory Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Aplikace Microsoft Graph](https://graph.microsoft.io/). Zobrazí se k dispozici pomocí [Průzkumníka Azure AD grafu](https://graphexplorer.cloudapp.net) a [Microsoft Graph Průzkumníka](https://graphexplorer2.azurewebsites.net/) atributy.

V současné době spotřebovává bez pracovního vytížení Office 365 tyto atributy.

Konfigurovat další atributy, které chcete synchronizovat v parametru path vlastní nastavení v Průvodci instalací.
![Průvodce rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) instalace zobrazí následující atributy, které jsou platné kandidáty:

- Typy objektů uživatelů a skupin
- Jedinou hodnotu atributy: řetězec logická hodnota, celé, binární.
- S více hodnotami atributy: řetězec, binární.

Seznam atributů čte z mezipaměti vytvořen během instalace Azure AD Connect. Pokud máte rozšířené schématu služby Active Directory s další atributy, [schéma musíte aktualizovat](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) před tyto atributy jsou viditelné.

Objekt můžete mít až 100 rozšíření atributů adresáře. Maximální délka je 250 znaků. Pokud je hodnota atributu delší, bude zkrácen modulem synchronizace.

Během instalace Azure AD Connect registraci aplikace kde jsou k dispozici tyto atributy. Zobrazí se tato aplikace na portálu Azure.  
![Schéma rozšíření aplikace](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Tyto atributy jsou teď dostupné prostřednictvím grafu:  
![Graf](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atributy jsou předponou rozšíření\_{AppClientId}\_. AppClientId má stejné hodnoty pro všechny atributy v adresáři vašeho Azure AD.

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
