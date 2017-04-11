
<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit incidentu rResponse požadavky | Požadavky na Microsoft Azure "
    description="Určení možností sledování a vytváření sestav pro hybridní identity řešení, který lze využít tak, že to, aby udělali kroky k identifikaci a zmírnění potenciální hrozeb"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Určit incidentu požadavky pro vaše hybridní identity řešení

Vysoké nebo střední organizace s největší pravděpodobností budou mít [zabezpečení incidentu odpověď](https://technet.microsoft.com/library/cc700825.aspx) na místě pomůže IT příslušným způsobem provádět akce na úrovni incidentu. Systému správy identit je důležitou součástí při procesu incidentu odpověď, protože mohou sloužit k určení, kdo provést určité akce vůči plánu. Řešení identit hybridní musíte mít poskytují možnosti sledování a vytváření sestav, které lze využít tak, že IT provést akce identifikovat a zmírnění potenciální hrozbou. V plánu typické incidentu máte následující fáze jako součást plánu:

1.  Počáteční hodnocení.
2.  Vyřešení incidentu komunikace.
3.  Ovládací prvek poškození a snížení rizika.
4.  Identifikace co bylo narušení zabezpečení a závažnosti.
5.  Zachování důkaz.
6.  Upozornění příslušné stranám.
7.  Obnovení systému.
8.  Dokumentace.
9.  Hodnocení poškození a náklady.
10. Proces a plán revize.

Během identifikace jaké it narušení zabezpečení a závažnosti fáze, bude potřeba identifikaci systémů, které byly ohroženo, soubory, které máte dostupné a zjistit, zda rozlišuje tyto soubory. Systému identity hybridní by měla splňovat tyto požadavky usnadňující identifikační uživatele, která vyrobila provedené změny. 

## <a name="monitoring-and-reporting"></a>Sledování a vytváření sestav
Opakovaně pokoušeli systému identity taky pomůže ve fázi zhodnocení především pokud systému vytvořila auditování a vytváření sestav funkce. Během počáteční hodnocení správce IT musíte mít k identifikaci podezřelé aktivity nebo systému měli být schopní aktivační událost, kterou automaticky založené na předkonfigurovaná úkolu. Mnoho aktivit znamenat útok možné však v ostatních případech chybně nakonfigurované systém může vést k celá řada falešně pozitivní sady zjišťování průnik. 

Systému správy identit by pomoci správci IT identifikovat a hlásit činnostem podezřelé. Obvykle tyto technické požadavky můžete splňovat všechny systémy sledování a s podřízenosti funkci, která můžete zvýraznit potenciální hrozeb. Pomocí níže uvedených otázkách můžete navrhnout řešení totožnost hybridní při zohlednění pozornost incidentu požadavky:

- Má vaše společnost používá incidentu odpověď zabezpečení na místě?
 - Pokud ano, je aktuální systému správy identit používá jako součást procesu
- Vaše společnost musím identifikovat podezřelé přihlašování pokusy o před uživateli na různých zařízeních?
- Vaše společnost musím zjistit potenciální hostují pověření uživatele?
- Vaše společnost musím auditování přístup a akci uživatele?
- Vaše společnost musím vědět, kdy uživatel resetovat své heslo?

## <a name="policy-enforcement"></a>Vynucení zásad

Během kontroly poškození a fáze snížení rizika je důležité rychle zmenšit skutečné a potenciální účinky útok. Tuto akci, která bude trvat v tuto chvíli může být rozdíl mezi vedlejší a hlavní jednu. Přesné odpověď závisí na vaší organizaci a přírodní útok, který se setkáte. Pokud počáteční hodnocení uzavřena, že byla ohroženo účet, musíte se k jejímu vynucení zásadu blokovat tento účet. To je pouze příklad, kde bude využít systému správy identit. Pomocí níže uvedených otázkách můžete navrhnout řešení totožnost hybridní při vezměte v úvahu, jak budou vynucené zásady reagovat na událost probíhající:

- Vaše společnost má zásady na místě k blokování uživatelů z Accessu k síti v případě potřeby?
 - Pokud ano, aktuálním řešení integrovat systému správy identit hybridní odkazující na přijmout?
- Vyžaduje vaše společnost k jejímu vynucení podmíněného přístupu pro uživatele, kteří jsou v karanténě? 
 
>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Strategie ochranu dat definovat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přesměrovány na tlačítko Možnosti a výhody a nevýhody jednotlivých možností.  Tak, že máte zodpovězené otázek, kterou vybereme kterou nejlepší možnost se hodí vyhovovaly potřebuje.

## <a name="next-steps"></a>Další kroky
[Definování strategie ochrana dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
