<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit požadavky na správu obsahu | Microsoft Azure"
    description="Poskytuje přehled o zjištění požadavky na správu obsahu vašim potřebám. Obvykle Pokud má uživatel své zařízení má mít taky více přihlašovací údaje, které bude Alternující podle předpisů rozhraní aplikace, kterou používá. Je důležité k odlišení, jaký obsah byl vytvořen pomocí osobní údaje a těch, které jsou vytvořené pomocí podnikové přihlašovacích údajů. Řešení totožnost by měla chcete provést interakci s cloudové služby bezproblémovou práci poskytovat koncového uživatele při jeho utajení a zvýšit ochranu proti únik data."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Určit požadavky na správu obsahu pro vaše hybridní identity řešení

Principy požadavky na správu obsahu pro svoji firmu přímo ovlivní vaše rozhodnutí o řešení totožnost hybridní používat. Šíření víc zařízeních a schopnost uživatelů přenést svoje zařízení ([BYOD](http://aka.ms/byodcg)) společnost musí chránit vlastních dat, ale je také třeba zachovat soukromí uživatele. Obvykle Pokud má uživatel své zařízení má mít taky více přihlašovací údaje, které bude Alternující podle předpisů rozhraní aplikace, kterou používá. Je důležité k odlišení, jaký obsah byl vytvořen pomocí osobní údaje a těch, které jsou vytvořené pomocí podnikové přihlašovacích údajů. Řešení totožnost by měla chcete provést interakci s cloudové služby bezproblémovou práci poskytovat koncového uživatele při jeho utajení a zvýšit ochranu proti únik data. 

Řešení totožnost bude využít podle různých technické ovládacích prvků za účelem zajištění správy obsahu, jak je znázorněno na následujícím obrázku:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Ovládací prvky zabezpečení, které bude využívání systému správy identit**

Požadavky na správu obsahu bude obecně využít systému správy identit v těchto oblastech:

- Ochrana osobních údajů: identifikace uživatele, který vlastní zdroje a použitím příslušných ovládacích prvků k zachování integrity.
- Klasifikace dat: označte uživatele nebo skupinu a úroveň přístupu k objektu podle jeho klasifikace. 
- Ochrana dat únik: ovládací prvky zabezpečení zodpovědný za ochrana dat chcete-li předejít únik potřebovat chcete provést interakci s systému identity k ověření identity uživatele. To je důležité pro audit účelu záznam.

>[AZURE.NOTE]
Přečtěte si [klasifikace dat pro připravenost ke cloudové](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) Další informace o doporučených postupech a pokyny pro klasifikaci data.

Při plánování řešení totožnost hybridní zajistit, že jsou podle požadavky organizace odpovědět na následující otázky:

- Má vaše společnost ovládací prvky zabezpečení na místě k jejímu vynucení dat ochrany osobních údajů?
 - Pokud ano, tyto ovládací prvky zabezpečení budou moct integrace s řešení identit hybridní, které se chystáte přijmout?
- Má ve vaší organizaci používat klasifikace dat?
 - Pokud ano, je aktuální řešení integrovat s řešení identit hybridní, které se chystáte přijmout?
- Vaše společnost aktuálně neobsahují žádné řešení pro únik dat? 
 - Pokud ano, je aktuální řešení integrovat s řešení identit hybridní, které se chystáte přijmout?
- Vaše společnost musím auditování přístupu k prostředkům?
 - Pokud ano, jaký typ zdrojů?
 - Pokud ano, je nutné úroveň informace?
 - Pokud ano, kde protokol auditování musí být umístěny? V místním i v cloudu?
- Vaše společnost musím šifrování e-mailů, které obsahují citlivá data (SSNs, čísla kreditních karet, atd.)?
- Vaše společnost musím zašifrovat všechny dokumenty/obsah nasdílel partnerů pro externí podnikové?
- Vynucení podnikových zásad na určité typy e-mailů vyžaduje vaše společnost (bez Odpovědět všem proveďte nepřesměrovávat)?
 
>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Definování strategie ochrana dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přesměrovány na tlačítko Možnosti a výhody a nevýhody jednotlivých možností.  Tak, že máte zodpovězené otázek, kterou vybereme kterou nejlepší možnost se hodí vyhovovaly potřebuje.


## <a name="next-steps"></a>Další kroky
[Určit požadavky na řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
