<properties
    pageTitle="Azure Active Directory hybridní identity navrhování – určit identity požadavky | Microsoft Azure"
    description="Určení obchodních potřeb společnosti, které vás definovat požadavcích na hybridní identity návrh."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Určit identity požadavky pro vaše hybridní identity řešení
Cílem prvního kroku v návrhu řešení identit hybridní je rozhodnout, že splníte požadavky pro firmy organizace, která bude využívání toto řešení.  Hybridní identity spustí podpůrné roli (podporuje všechna ostatní řešení cloudu zadáním ověřování) a přejde na poskytnout nové a zajímavých funkcí, které odemknout nového pracovního vytížení pro uživatele.  Tyto pracovního vytížení a služby, které chcete používat pro uživatele, bude určovat požadavcích na hybridní identity návrh.  Těchto služeb a úloh potřebujete můžete využít hybridní identity obou místních i cloudových.  

Potřebujete projít tyto klíčové aspekty firmy chcete porozumět tomu, co je povinné nyní a co společnosti plány jednotného zasílání zpráv v budoucnosti. Pokud nemáte viditelnost dlouhodobou strategie pro hybridní identity návrhu, je pravděpodobné, že nebudou řešení scalable jako obchodní potřeby zvětšovat a změnit.   T má diagram ukazuje příklad architektura identity hybridní a úloh, které jsou právě odemknout uživatelům. Toto je právě příklad nových možností, které můžete odemknout a doručila ji s strategii identitu pevné hybridní. 
 
Některé součásti, které jsou součástí architektury hybridní identity![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Určení potřeb podniku
Každý společnosti bude mít odlišné požadavky, i když tyto společnosti jsou součástí odvětví stejné skutečné podniku, který může být různé požadavky. Pořád můžete využít doporučené postupy z odvětví nakonec je však obchodních potřeb společnosti, které vás definovat požadavcích na hybridní identit návrh. 

Ujistěte se, odpovězte na následující otázky k identifikaci potřebám vaší organizace:

- Chcete-li oříznout provozní náklady IT vyhledávání vaše společnost?
- Vaše společnost vyhledávání zabezpečení cloudové aktiva (SaaS aplikace, infrastruktura)?
- Vyhledávání modernizovat vaše IT vaše společnost?
  - Jsou uživatelé mobilních a náročné IT vytvoření výjimky do DMZ umožňuje jiný typ přenosy pro přístup k prostředkům?
  - Má vaše společnost starší verze aplikace potřebné k publikování na tyto moderní uživatele, které nejsou snadno revize?
  - Vaše společnost musím provedení těchto úkolů a zobrazte v části ovládací prvek ve stejnou dobu?
- Vaše společnost vyhledávání zabezpečené identit uživatelů a snížení rizika, tím, že přinese nové nástroje, které využít znalosti jazyků společnosti Microsoft Azure zabezpečení odborné znalosti v místním nasazení?
- Vaše společnost pokouší zbavit dreaded "externí" účty místně nebo je přesunout do cloudu, kde jsou už neaktivní hrozbou, že do místního prostředí?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analýza infrastruktura místních identit
Teď, když máte představu o vaší společnosti obchodní požadavky, budete muset zjistit infrastrukturu místních identit. Toto hodnocení je důležité pro definování technické požadavky integrovat aktuální řešení totožnost systému správy identit cloudu. Ujistěte se, odpovězte na následující otázky:

- K čemu řešení a tak mohli ověřovat má vaše společnost použít místní? 
- Vaše společnost aktuálně nemá žádné místní synchronizační služba služby?
- Má ve vaší organizaci používat všechny poskytovatelů identit třetích stran (IdP)?

Budete potřebovat je potřeba vědět cloudových služeb, které můžou mít vaše společnost. Provádění hodnocení znát aktuální integrace s SaaS, IaaS nebo PaaS modely ve vašem prostředí je velmi důležité. Zkontrolujte, že během hodnocení odpovězte na následující otázky:
- Má vaše společnost veškerá integrace s poskytovateli služeb cloudu?
- Pokud ano, které služby jsou používány?
- Tato integrace právě výrobní nebo je pilotního nasazení?


>[AZURE.NOTE]
Pokud nemáte mapování správné všech svých aplikací a cloudových služeb, můžete použít nástroj zjišťování aplikace cloudu. Tento nástroj může poskytnout přehled o všech organizace business a spotř cloudu aplikace oddělení IT. Který usnadňuje vyhledávání stín IT ve vaší organizaci, včetně podrobností o využití vyhledávání a uživatelům přístup ke cloudové aplikace. Tento nástroj přejděte na [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Vyhodnocení požadavky integrace identity
Pak budete muset zjistit požadavky integrace identity. Toto hodnocení je třeba definovat technické požadavky jak budou uživatelé ověření vzhled organizace stavu a volném čase v cloudu, jak organizace vám umožní se tak mohli ověřovat a co uživatelské prostředí má dělat. Ujistěte se, odpovězte na následující otázky:

- Budou vaší organizaci používat federace, standardní ověření nebo obojí?
- Je federace povinné?  Z důvodu následující:
 - Jednotné přihlašování pomocí protokolu Kerberos
 - Vaše společnost používá místní aplikací (buď součást stran podnikových nebo 3), které používá SAML nebo podobné funkce federace.
 - MFA prostřednictvím čipové karty. RSA SecurID, atd.
 - Pravidla přístupu klienta, která následující otázky:
     1. Můžete blokovat všechny externí přístup k Office 365 podle IP adresa klienta?
     1. Můžete blokovat všechny externí přístup k Office 365, s výjimkou Exchange ActiveSync?
     1. Můžete blokovat všechny externí přístup k Office 365 kromě aplikací založených na prohlížeči (OWA, SPO)
     1. Můžete blokovat všechny externí přístup k Office 365 pro členy skupiny AD určené
- Zabezpečení a auditování pochybnosti
- Již existující investice federované ověřování
- Pro naše doménu v cloudu zadejte název použije naši organizaci?
- Má organizace vlastní doménu?
    1. Je tuto doménu veřejných a snadno zkontrolovat prostřednictvím DNS?
    1. Pokud není, pak máte veřejné domény, která mohou sloužit k registraci alternativní Přípony ve službě Active Directory?
- Shodovaly identifikátory uživatelů pro znázornění cloudu? 
- Má organizace aplikace, které vyžadují integrace se službou cloud services?
- Organizace mít víc domén a budou všechny používat standardní nebo federované ověřování?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Vyhodnocení aplikace, které pracují ve vašem prostředí
Teď máte představu o vaší místní a cloudové infrastruktury, budete muset zjistit aplikace spuštěné v těchto prostředí. Toto hodnocení je třeba definovat technické požadavky integrovat těmito aplikacemi systému správy identit cloudu. Ujistěte se, odpovězte na následující otázky:

- Kde se naše aplikace live?
- Uživatelé budou místní aplikace?  V cloudu? Nebo obě?
- Plánujete prohlédněte existující aplikace úloh a jejich přesunutí do cloudu?
- Plánujete nové aplikace, které bude nacházet buď místně nebo v cloudu, která se bude používat cloudové můžete vyvíjet ověřování?

## <a name="evaluate-user-requirements"></a>Vyhodnocení požadavky na uživatele
Máte taky vyhodnocení požadavky na uživatele. Toto hodnocení je třeba definovat kroky, které je třeba mít na místě a pomoc uživatelům při přechodu do cloudu. Ujistěte se, odpovězte na následující otázky:

- Uživatelé budou aplikací místního?
- Uživatelé budou aplikace v cloudu?
- Jak uživatelé obvykle přihlášení k jejich místního prostředí?
- Jak budou uživatelé přihlašovat do cloudu?

>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Určení incidentu požadavky](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) budou přesměrovány na tlačítko Možnosti a výhody a nevýhody jednotlivých možností.  Tak, že máte zodpovězené otázek, kterou vybereme kterou nejlepší možnost se hodí vyhovovaly potřebuje.

## <a name="next-steps"></a>Další kroky
[Určení požadavky synchronizace adresářů](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
