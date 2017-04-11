<properties 
    pageTitle="Rozhraní API správy klíčů koncepty" 
    description="Informace o rozhraní API, produktů, role, skupiny a jiných základních koncepcí rozhraní API správy." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Co je Správa rozhraní API?

Správa rozhraní API pomáhá organizace publikovat rozhraní API pro externí, partnera a interní vývojáři odemknout potenciál svých dat a služeb. Firmy všude, kde se pokud chcete rozšířit svou činnost jako digitální platformy, vytváření nové kanály, hledání noví zákazníci a řízení hlubší engagement s existující. Rozhraní API Správa poskytuje schopností core zajistit úspěšná rozhraní API program v bráně vývojář zapojení, obchodní přehledy, analýzy, zabezpečení a ochranu.

Následující videu získáte přehled o Azure rozhraní API Management a naučte se používat rozhraní API správy přidáte řadu funkcí k API, včetně řízení přístupu, omezení rychlosti, sledování, protokolování událostí a odpověď ukládání do mezipaměti, s minimálními práce na druhé straně.

> [AZURE.VIDEO azure-api-management-overview]

Použití rozhraní API správy, správci vytvářet rozhraní API. Každý rozhraní API se skládá z jedné nebo několika operací a každý rozhraní API lze přidat jeden nebo víc produktů. Použití rozhraní API, vývojáři přihlášení k odběru produktu, který obsahuje tento rozhraní API a pak můžete volat rozhraní API operace vyměřené poplatky za jeho všech použití zásad, které mohou být platná.

Toto téma obsahuje přehled rozhraní API správy klíčů koncepty.

>[AZURE.NOTE] Další informace najdete v tématu [cloudové správy rozhraní API: využití Power rozhraní API](http://j.mp/ms-apim-whitepaper) dokument PDF. Tento úvodní dokument řízení rozhraní API CITO výzkum zahrnuje: 
>
> - Obecné požadavky rozhraní API a problémy
>     - Oddělení rozhraní API a prezentování fasádách
>     - Vývojáři zprovoznění a spuštění rychle
>     - Zabezpečení aplikace access
>     - Technologie pro analýzu a metriky
> - Získání kontrolu a přehled pomocí správy rozhraní API platformy
> - Použití cloudu a místní řešení
> - Správa Azure rozhraní API

## <a name="apis"> </a>Rozhraní API a provoz

Rozhraní API se základem instanci služby správy rozhraní API. Každý rozhraní API představuje sady operace dostupné pro vývojáře. Každý rozhraní API obsahuje odkaz na službu back-end implementující rozhraní API a jeho operace mapy s operacemi implementovaná službou back-end. Operace v části Správa rozhraní API jsou snadnému s publikum nemůže ovládat URL mapování, dotaz a parametry cesty, žádost a odpověď obsahu a ukládání do mezipaměti operace odpověď. Sazba limit kvóty a zásady omezení IP lze rovněž provést na úrovni jednotlivých operace nebo rozhraní API.

Další informace najdete v článku [jak vytvářet rozhraní API][] a [jak si přidat operace rozhraní API][].


## <a name="products"></a> Produkty

Jak vytažená rozhraní API pro vývojáře jsou produkty. Produkty v části Správa rozhraní API máte jednu nebo více rozhraní API a budou nakonfigurována název, popis a podmínky použití. Produkty může být **otevřený** nebo **chráněného**. Chráněný produkty musíte mít předplatné před jejich použitím při otevření produkty můžete použít bez přihlášení k odběru. Když produkt je připraven k použití vývojáři můžou publikovat. Po publikování, může se zobrazit (nebo v případě chráněných produktů, nemáte předplatné) vývojáři. Schválení předplatné je nakonfigurovaný na úrovni produktu a můžete vyžadovat schválení správce nebo být schváleno automatického.

Skupiny slouží ke správě viditelnost produktů pro vývojáře. Produkty udělovat viditelnost skupiny a vývojářů můžete zobrazit a přihlášení k odběru produkty, které jsou viditelné skupiny, do kterých patří. 

Další informace najdete v článku [jak vytvářet a publikovat produktu][] a následující video.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Skupiny

Skupiny slouží ke správě viditelnost produktů pro vývojáře. Správa rozhraní API obsahuje následující skupiny neměnný systému.

-   **Správci** - Azure předplatné správci jsou členy této skupiny. Správci spravovat instancí služby správy rozhraní API vytváření rozhraní API, operace a produkty, které využívají vývojáři.
-   **Vývojáři** - ověřené – portál pro vývojáře, které uživatelé spadají do této skupiny. Vývojáři se zákazníky, kteří vytváření aplikací pomocí svého rozhraní API. Vývojáři mají přístup k portálu pro vývojáře a vytváření aplikací, které volají operace rozhraní API.
-   **Hosté** - uživatelé portálu neověřených vývojář, například potenciální zákazníci návštěva portálu pro vývojáře instance rozhraní API správy spadají do této skupiny. Mohou být uděleny určité jen pro čtení přístup, například možnost zobrazit rozhraní API, ale ne jim zavolá.

Kromě těchto skupin systém správci můžete vytvořit vlastní skupiny nebo [využít externí skupin v klienti přidruženými Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Vlastní a externí skupiny lze vedle skupiny systému v pojmenování vývojáři viditelnost a přístup k rozhraní API produkty. Můžete například vytvořit jednu vlastní skupinu pro vývojáře přidružen konkrétního partnera organizace a aby mu umožnil přístup k rozhraní API z produktu obsahující jenom relevantní rozhraní API. Uživatel může být členem skupiny víc než jedné skupině.

Další informace najdete v článku [jak vytvářet a používat skupiny][].

## <a name="developers"></a> Vývojáři

Vývojáři představují uživatelské účty v instanci služby správy rozhraní API. Vývojáři můžou vytvořené nebo pozváni ke správci nebo můžou zaregistrovat z [portálu vývojář][]. Každý vývojář je členem jednu nebo více skupin a může být přihlášení k odběru produkty, jejichž udělit viditelnost do těchto skupin.

Když vývojáři přihlášení k odběru produktu jsou poskytovány klávesu primárních a sekundárních produktu. Tento používá se při volání do rozhraní API produktu.

Další informace najdete v tématu [Vytvoření nebo pozvání vývojáři][] a [jak můžete přidružit skupiny s vývojáři][].

## <a name="policies"></a> Zásady

Zásady jsou výkonné možností správy rozhraní API, pomocí kterých vydavatele, kterého chcete změnit chování rozhraní API prostřednictvím konfigurace. Zásady jsou kolekce příkazy, které jsou proveden sebou na žádost nebo odpovědi rozhraní API. Převod formátu ze souboru XML a JSON omezení rychlosti volání omezení počtu příchozí hovory od vývojáře patří oblíbené příkazy a mnoho dalších zásad jsou k dispozici.

Zásady výrazů lze použít jako hodnoty atributu nebo textové hodnoty v některém z zásady správy rozhraní API, dokud zásadu určuje jinak. Několik zásad například zásady [řízení toku](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) a [nastavit proměnnou](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) vycházejí zásad výrazů. Další informace najdete v tématu [Upřesnit zásad](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [zásady výrazy](https://msdn.microsoft.com/library/azure/dn910913.aspx)a následující video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Úplný seznam zásady správy rozhraní API najdete v článku Principy [zásad][]. Další informace o použití a konfiguraci zásad najdete v článku [zásady správy rozhraní API][]. Kurz týkající se vytváření produktu s sazba kvóty a zásady najdete v článku [jak vytvořit a nakonfigurovat nastavení rozšířeného produktu][]. Ukázka najdete v článku následující video.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Portál pro vývojáře

Portálu pro vývojáře slouží k vývojáři můžou Další informace o rozhraní API, zobrazení a hovor operace a přihlášení k odběru produkty. Potenciální zákazníci Basic můžou navštívit portálu pro vývojáře zobrazení rozhraní API a operace a zaregistrovat. Adresu URL portálu vývojář se nachází na řídicí panel na klasické portálu Azure pro instanci služby správy rozhraní API.

Přidáním vlastní obsah, přizpůsobení stylu a přidejte svůj branding můžete přizpůsobit vzhled a chování vlastního portálu vývojář.

## <a name="api-management-and-the-api-economy"></a>Správa rozhraní API a rozhraní API ekonomiky

Další informace o rozhraní API správy, podívejte se na následující prezentace z konference Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Karta Vývojář v portálu]: #developer-portal

[Jak vytvořit rozhraní API]: api-management-howto-create-apis.md
[Jak přidat operace rozhraní API]: api-management-howto-add-operations.md
[Jak vytvořit a publikovat k produktu]: api-management-howto-add-products.md
[Vytvoření a používání skupin]: api-management-howto-create-groups.md
[Jak přidružit vývojáři skupiny]: api-management-howto-create-groups.md#associate-group-developer
[Jak vytvořit a nakonfigurovat nastavení rozšířeného produktu]: api-management-howto-product-with-rules.md
[Jak vytvořit nebo pozvání vývojáři]: api-management-howto-create-or-invite-developers.md
[Odkaz na zásad]: api-management-policy-reference.md
[Zásady správy rozhraní API]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
