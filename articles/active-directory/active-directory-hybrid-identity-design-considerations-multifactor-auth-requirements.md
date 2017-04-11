<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit požadavky vícefaktorové ověřování"
    description="Řízení přístupu podmíněné zkontroluje Azure Active Directory určité podmínky, které jste vybrali při ověřování uživatele a před umožněním přístup k aplikaci. Když jsou splněné tyto podmínky, uživatel ověření a povolený přístup k aplikaci."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Určit vícefaktorové ověřování požadavky pro vaše hybridní identity řešení

Tato světě mobilita uživatelům přístup k datům a aplikacím v cloudu a z libovolného zařízení zabezpečení těchto informací stal prvořadý.  Každý den je nový nadpis o porušení zabezpečení.  I když není nijak zaručené proti taková porušení, vícefaktorové ověřování poskytuje další úroveň zabezpečení a ochrana před tyto narušením.
Začněte tím, že vyhodnocení požadavky organizace pro vícefaktorové ověřování. To znamená co je organizace pokusu o zabezpečení.  Toto hodnocení je třeba definovat technické požadavky na nastavení a povolení organizace uživatelů vícefaktorové ověřování.

>[AZURE.NOTE]
Pokud si nejste obeznámení s MFA a akce, velmi doporučujeme, přečtěte si článek [Co je Azure Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md) předchozí pokračovat ve čtení v této části.

Zkontrolujte, že odpovězte následující:

- Vaše společnost pokouší zabezpečení aplikace Microsoft? 
- Jak jsou publikovány tyto aplikace?
- Poskytuje vaší společnosti vzdálený přístup k povolení zaměstnance, kteří mají přístup k místní aplikace?

Pokud ano, jaký typ vzdáleného přístupu? Potřebujete zjistit, kde uživatelé, kteří mají přístup k těmto aplikacím budou umístěné. Toto hodnocení je jiné důležitým krokem k definování strategie velkými vícefaktorové ověřování. Ujistěte se, odpovězte na následující otázky:

- Kde jsou uživatelů, který bude nacházet?
- Budou lze umístit kamkoliv?
- Má vaše společnost stanovit omezení podle umístění uživatele?

Jakmile pochopíte tyto požadavky, je důležité také vyhodnocení požadavky na uživatele pro vícefaktorové ověřování. Toto hodnocení je důležité, protože se definice požadavků pro zavádění vícefaktorové ověřování. Ujistěte se, odpovězte na následující otázky:

- Znají uživatelů vícefaktorové ověřování?
- Některé používá vyzváni k další ověřování?  
 - Pokud ano, vždycky, když pocházející z externí sítě nebo přístupu k určité aplikací nebo jiných podmínek?
- Uživatelé budou vyžadovat školení o tom, jak nastavit a implementace vícefaktorové ověřování?
- Jaké uvedené hlavní příklady situací, která vaší společnosti chcete povolit vícefaktorové ověřování pro uživatele?

Po nalezení odpovědí předchozí, budou moct porozumět tomu, pokud jsou tu uvedené vícefaktorové ověřování už implementovaná místní. Toto hodnocení je třeba definovat technické požadavky na nastavení a povolení organizace uživatelů vícefaktorové ověřování. Ujistěte se, odpovězte na následující otázky:

- Vaše společnost musím chránit privilegovaných účty s MFA?
- Vaše společnost musím povolit MFA pro některé aplikace dodržování předpisů důvodů?
- Vaše firma musím povolit MFA pro všechny uživatele měl tato aplikace nebo pouze správci?
- Potřebujete máte MFA vždy povolen nebo jenom při přihlášení uživatelé mimo vaší podnikové sítě?


## <a name="next-steps"></a>Další kroky
[Definování strategii zavádění hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
