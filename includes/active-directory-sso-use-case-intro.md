Organizace používáte další [jako služba (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikací produktivity protože technologii cloudu a nástrojů se stávají přístupnější. Narůstající velikostí počet SaaS aplikace, bude náročné správcům spravovat účty a přístupových práv a odstranění uživatelům nezapomeňte různých hesel. Správa tyto aplikace jednotlivě vytvoří navíc práce a je méně bezpečné.


- Zaměstnanců, kteří mají ke sledování mnoho hesel mají použít zabezpečené méně metody si zapamatovat, zapíšete hesla nebo použití stejné hesel u více účtů.

- Při příchodu nového zaměstnance nebo jednu ponechá všechny svoje účty musí být jednotlivě zřízení nebo zrušit zřízení.

- Navíc zaměstnanců může apps začít používat SaaS pro svou práci bez opakovaného IT, což znamená vytvářejí svých účtů v systémech, které nejsou schválené správce IT a nejsou sledování.  

Řešení pro všechny tyto úkoly je jednotné přihlašování (SSO). Nejjednodušší způsob, jak spravovat víc aplikace a uživatelům poskytnout jednotné přihlašování setkat i v případě je. Azure Active Directory (Azure AD) poskytuje robustní řešení jednotného přihlašování a obsahuje mnoho dostupných předem integrovaných aplikací, s výukové programy pro správce, můžete rychle nastavit nové aplikace a spuštění zřizování uživatelů.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Jak Azure Active Directory integrovat aplikace?  

Azure AD umožňuje integraci aplikace a zřizování účty. Lze to až jednu ze dvou přístupů.

- Pokud aplikace nebude předem integrované v aplikaci Galerie, můžete procházet tento portál a nastavit aplikace a konfigurace umožňuje přihlašování. Pro nějakou aplikaci Galerie mohli rovnou začít podle pokynů jednoduché podrobné prezentovány v galerii aplikace a na portálu Azure povolit jednotného přihlašování.

- Nejsou-li aplikaci v galerii, můžete pořád nastavit většiny aplikací v Azure AD jako vlastní aplikace. Při této akci musí trochu víc odborných ke konfiguraci. Můžete přidat libovolné aplikaci, která podporuje SAML 2.0 jako federované aplikace nebo aplikace, která má HTML na přihlašovací stránku jako jednotného přihlašování k aplikaci heslo.

V případě, kde uživatelé vytvořili svých účtů SaaS aplikace pro, které nejsou spravuje IT, [Zjišťování aplikace cloudu](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) nástroj poskytuje řešení. Tento nástroj sleduje přenosy web identifikovat, se kterými aplikacemi použily v rámci organizace a počet lidí přes každý z nich. IT mohou použít tyto informace se dozvíte, jaké aplikace uživatelů upřednostňujete a rozhodnout, které chcete integrovat do Azure AD sso.  

Při integraci aplikace do Azure AD můžete namapovat identity založení aplikací uživatelů jejich Azure AD identity.  
