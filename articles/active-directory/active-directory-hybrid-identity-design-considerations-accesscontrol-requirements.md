
<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit požadavky na řízení přístupu | Microsoft Azure"
    description="Popisuje identit a identifikační požadavků na zdroje pro uživatele v hybridním prostředí přístup."
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

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Určit požadavky na řízení přístupu pro vaše hybridní identity řešení
Při organizace je návrhu jejich řešení totožnost hybridní lze také použít tuto příležitost kontroloval požadavků na zdroje, které se chystáte být k dispozici pro uživatele přístup. Přístup k datům mezi všechny čtyři pilíře identity, které jsou:

- Správa
- Ověřování
- Povolení
- Sestavy auditování

V částech, které sleduje převezme a tak mohli ověřovat ve víc informací, správu a auditování jsou součástí životním cyklu identity hybridní. Přečtěte si [úlohy správy identit hybridní zjistit](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) Další informace o těchto možnostech.

>[AZURE.NOTE]
Další informace o každém z těchto pilíře číst [Čtyři pilíře z Identity – Správa identit v stáří hybridní IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) .

## <a name="authentication-and-authorization"></a>A mohli ověřovat
Existují různé scénáře a tak mohli ověřovat, podobnému sledu bude mít určitý požadavky, které se musí splňovat řešení identit hybridní, které společnosti přejdete na přijmout. Scénáře týkající se komunikace Business to Business (B2B) můžete přidat další ověřovací kód pro správce IT vzhledem k tomu, budou se muset zajistit, že metodu a tak mohli ověřovat používanou organizaci mohli komunikovat s jejich obchodní partnery. Během procesu návrhu a tak mohli ověřovat požadavky Ujistěte se, že jsou odpovědi na následující otázky:

- Bude vaše organizace ověřování a povolte pouze uživatelé c:\ jejich systému správy identit?
 - Jsou všechny plány B2B scénářích?
 - Pokud ano, už znáte protokoly (SAML OAuth, pomocí protokolu Kerberos, tokeny a certifikáty) se použije k propojení obou firmy?
- Znamená řešení identit hybridní, které se chystáte přijmout podpory protokoly?

Jiné důležité zvážit je místo, kam úložiště ověřování, které se použije uživatelům a partnerům budou umístěné a pro správu modelu se nemusí používat. Zvažte následující dvě základní možnosti:
- Centralizované: v tomto modelu přihlašovací údaje uživatele, zásady a správu může být centralizované místních i v cloudu.
- Hybridní: v tomto modelu přihlašovací údaje uživatele, zásady a správu budou centralizované místním prostředím a replikovanou v cloudu.

Jaký model organizaci přijme, se budou lišit podle jejich obchodní požadavky, který chcete odpovědět na následující otázky k identifikaci místo, kam se bude nacházet systému správy identit a režimem správy používat:

- Má mít vaše organizace aktuálně Správa identit místní?
 - Nastavení Ano znamená, že plánujete v jednoduchosti je?
 - Jsou všechny nařízení nebo dodržování předpisů požadavky, že vaše organizace musí odpovídat této vyžaduje kde se má nacházet systému správy identit?
- Vaše organizace používá jednotného přihlašování pro aplikace nachází místní nebo v cloudu?
 - Pokud ano, přijetí model identit hybridní vliv tento proces?

## <a name="access-control"></a>Řízení přístupu
Zatímco a tak mohli ověřovat jsou základní prvky povolení přístupu k podnikové dat prostřednictvím ověření uživatele, je také třeba nastavit úroveň přístupu, kterou tito uživatelé budou mít a úroveň přístupu správci bude mít nad prostředky, které jsou správa. Řešení totožnost hybridní musí být mohli dát podrobného přístup ke zdrojům, delegování a řízení přístupu základní role. Ujistěte se, že jsou o řízení přístupu zodpovězené následující otázky:

- Má vaše společnost více uživatelů s vyššími oprávněními oprávnění ke správě systému identity?
 - Pokud ano, musí každý uživatel přístup stejnou úroveň přístupu?
- Vaše firma musel přístup delegáta, aby uživatelé ke správě určitého zdroje?
 - Pokud ano, jak často tohoto chování?
- Vaše firma musel integrace možnosti řízení přístupu k mezi místním prostředím a cloudové zdroje?
- Vaše společnost jsou potřeba pro omezení přístupu k prostředkům podle určitých podmínek?
- Má mít vaše společnost libovolné aplikaci, která potřebuje vlastní řízení přístupu k několik zdrojů informací?
 - Pokud ano, kde se tyto aplikace nacházejí (místní nebo v cloudu)?
 - Pokud ano, kde se můžou být cílové zdrojů nachází (místní nebo v cloudu)?

>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Definování strategie ochranu dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přesměrovány na tlačítko Možnosti a výhody a nevýhody jednotlivých možností.  Odpovědí na tyto otázky vyberete volba nejlepší vyhovuje potřebám vaší organizace.

## <a name="next-steps"></a>Další kroky

[Určit incidentu požadavky](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
