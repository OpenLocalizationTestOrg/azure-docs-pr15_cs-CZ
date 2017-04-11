<properties
    pageTitle="Glosář ochranu Identity Azure Active Directory | Microsoft Azure"
    description="Glosář ochranu Identity Azure Active Directory"
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení, Glosář"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Glosář ochranu Identity Azure Active Directory 


### <a name="at-risk-user"></a>Riziko (uživatel)  
Uživatel se jedna nebo více aktivní rizika události. 

### <a name="atypical-sign-in-location"></a>Umístění atypické přihlášení   
Přihlašovací z zeměpisné polohy, která není typické pro určitého uživatele, podobně jako uživatele nebo klienta.

### <a name="azure-ad-identity-protection"></a>Ochrana Azure AD Identity    
Zabezpečení modul Azure Active Directory, která nabízí do rizika událostí a potenciální chyby došlo k ovlivnění identit organizace.

### <a name="conditional-access"></a>Podmíněný přístup  
Zásady zabezpečení přístupu k prostředkům. Pravidla podmíněného přístupu uložené v Azure Active Directory a jsou vyhodnoceny Azure AD před udělení přístupu ke zdroji.  Příklady pravidel zahrnout omezení přístupu na základě umístění uživatele, metody ověřování uživatele nebo stavu zařízení.

### <a name="credentials"></a>Přihlašovací údaje     
Informace, které obsahuje identifikační a doklad identifikace, které se používá k získání přístupu k místním a síťových prostředků. Příklady přihlašovací údaje jsou uživatelská jména a hesla, čipové karty a certifikáty.

### <a name="event"></a>Události   
Záznam aktivity v Azure Active Directory.

### <a name="false-positive-risk-event"></a>Falešně pozitivní (riziková událost) 
Stav události rizika nastavit ručně ochrana Identity uživatelem, uveďte, že riziková událost byla zkoumat a byla nesprávně označena jako riziková událost.

### <a name="identity"></a>Identity    
Osobu či entitu, musí být ověřený pomocí ověřování na základě kritérií, například hesla nebo certifikát.

### <a name="identity-risk-event"></a>Identita riziková událost     
AAD událost označená příznakem jako neobvyklých ochranou identit a oznámit, že byla ohroženo identitu.

### <a name="ignored-risk-event"></a>Ignorovat (riziková událost)    
Stav události rizika ručně nastavit tak, že uživatel ochrana Identity označující, že je zavřený riziková událost bez udělali remediation.

### <a name="impossible-travel-from-atypical-locations"></a>Možné cestovní z atypické umístění   
Riziková událost spouštěný-li dva přihlášení pro jednoho uživatele, kde jedna z nich je z atypické umístění přihlašovací a je čas mezi sign in kratší než minimální čas, který by fyzicky projít mezi těmto místům.  

###<a name="investigation"></a>Vyšetřování    
Proces kontroly činnosti, protokoly a další relevantní informace související s riziková událost se rozhodnout, jestli jsou remediation nebo omezení rizik kroků potřeby porozumět tomu, pokud a identity byl ohroženo a principy používání hostují identity.

### <a name="leaked-credentials"></a>Prozrazený přihlašovací údaje  

Riziková událost spouštěný najdou aktuální přihlašovací údaje uživatele (uživatelské jméno a heslo) veřejně poslal tmavě webu tak, že naše výzkumných.

### <a name="mitigation"></a>Omezení rizik  
Akci, kterou chcete omezení nebo vyloučení možnost se zlými úmysly zneužití hostují identity nebo zařízení bez obnovení identity nebo zařízení bezpečí stavu. Omezení rizik nevyřeší předchozí události riziko spojené s identity nebo zařízení.

### <a name="multi-factor-authentication"></a>Vícefaktorové ověřování 
Metoda ověřování, která vyžaduje dva nebo více metody ověřování, které mohou obsahovat něco uživatel má, například certifikát; něco uživatel zná, třeba jména uživatelů, hesel nebo průchodu věty; pole fyzicky atributy, jako jsou například Miniatura; osobní atributy a, jako jsou osobní podpis.

### <a name="offline-detection"></a>Offline zjišťování   
Rozpoznání odchylky a hodnocení rizik události, jako je pokus o přihlášení po faktoriál pro událost už uskutečněných.

### <a name="policy-condition"></a>Zásady podmínku    
Součástí zásad zabezpečení, které definují entit (skupin, uživatele, aplikace, platformy zařízení, stavy zařízení, rozsahy IP adres, typy klientů) součástí zásad nebo vyloučit z něho.

### <a name="policy-rule"></a>Pravidlo zásad     
Část zásady zabezpečení, které popisuje okolnosti, které by aktivace zásady a akce při aktivaci zásadu.

### <a name="prevention"></a>Ochrana před  
Akci, kterou chcete zabránit poškození organizaci prostřednictvím zneužívání identity nebo zařízení podezřelou nebo vědět, abyste mohli být. Ochrana před akce zabezpečení zařízení nebo identity a nevyřeší předchozí události rizika.

### <a name="privileged-user"></a>Oprávněními (uživatel)
Uživatel, který v době riziková událost, měl oprávnění správců trvalé nebo dočasné jeden nebo více zdrojů v Azure Active Directory, například globální správce, správce fakturace, Správce služeb, Správce uživatelů a hesla správce. 

###<a name="real-time"></a>V reálném čase    
V tématu zjišťování v reálném čase.

###<a name="real-time-detection"></a>Data v reálném zjišťování  
Zjišťování odchylky a hodnocení rizik události, jako je přihlášení pokus o před začátkem zvláštní události, bude moct pokračovat.

### <a name="remediated-risk-event"></a>Remediated (riziková událost)     
Stav události rizika automaticky nastavil ochrana Identity označující, že byla riziková událost remediated pomocí standardní nápravné akce pro tento typ riziková událost. Třeba při obnovení hesla uživatele mnoho událostí rizika, které označuje, že byla ohroženo předchozí heslo jsou automaticky remediated.

### <a name="remediation"></a>Remediation     
Akci, kterou chcete zabezpečené identitu nebo zařízení, které byly dříve podezřelé nebo známo, že být. Akce remediation obnoví identity nebo zařízení bezpečných stav a odstraňuje předchozí události rizika související s identity nebo zařízení.

### <a name="resolved-risk-event"></a>Vyřešit (riziková událost)   
Stav události rizika uživatelem ochrana Identity nastavit ručně, uveďte, že uživatel přijal akce odpovídající remediation mimo ochrana Identity a považovat riziková událost zavřít.

###<a name="risk-event-status"></a>Stav události rizika    
Vlastnost riziková událost, označující, zda je aktivní události a pokud zavřeli, důvod zavření.

###<a name="risk-event-type"></a>Typ události rizika  
Kategorie pro riziková událost označující typ odchylky, která způsobila události považuje za nebezpečné.

###<a name="risk-level-risk-event"></a>Úroveň rizika (riziková událost)  
Označení (vysoké, střední nebo nízká) závažnosti riziková událost na pomoc uživatelům ochrana Identity nastavit jejich priority akce přijmou snížit riziko uživatelům ve své organizaci. 

###<a name="risk-level-sign-in"></a>Úroveň rizika (přihlášení) 
Uvedení (vysoké, střední nebo nízká) pravděpodobnost, že pro konkrétní přihlášení, někoho jiného je pokus o použití identita uživatele.

###<a name="risk-level-user-compromise"></a>Úroveň rizika (narušení zabezpečení uživatele) 
Uvedení (vysoké, střední nebo nízká) pravděpodobnost, že byla ohroženo identitu.

### <a name="risk-level-vulnerability"></a>Úroveň rizika (zabezpečení)  
Označení (vysoké, střední nebo nízká) závažnosti chyby na pomoc uživatelům ochrana Identity nastavit jejich priority akce přijmou Pokud chcete snížit riziko uživatelům ve své organizaci.

### <a name="secure-identity"></a>Zabezpečené (identita)   
Proveďte akci remediation jako je změna hesla nebo počítač reimaging obnovení potenciálně hostují identity Nekompromisní stavu.

### <a name="security-policy"></a>Zásady zabezpečení
Sada pravidel a podmínky. Zásady lze použít k entit, jako jsou uživatelé, skupiny, aplikace, zařízení, platformy zařízení, stavy zařízení, rozsahy IP adres a typy klientů Auth2.0. Při zapnuté funkci zásadu, je vyhodnocen kdykoli entita součástí zásad vystaven token zdroje.

### <a name="sign-in-v"></a>Přihlaste se (v) 
Ověřit identitu v Azure Active Directory.

### <a name="sign-in-n"></a>Přihlásit (n) 
Proces nebo akci ověření identity v Azure Active Directory a události, která zachytí tuto operaci.

###<a name="sign-in-from-anonymous-ip-address"></a>Přihlášení z anonymní IP adresy    
Riziková událost spouštěný později úspěšné přihlášení z adresy IP, který byl zjištěn jako anonymní proxy IP adresu.

###<a name="sign-in-from-infected-device"></a>Přihlášení z infikovaných zařízení 
Riziková událost spouštěný při přihlašování v pochází z IP adresy, které se používá pro použití v jedné nebo více hostují zařízení, která jsou aktivně pokusu o komunikaci se serverem bot.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Přihlaste se z adresy IP pomocí podezřelé aktivity 
Riziková událost spouštěný po úspěšné přihlášení z IP adres s vysokou počet neúspěšný pokus o přihlášení přes víc uživatelských účtů přes krátký časový úsek.

###<a name="sign-in-from-unfamiliar-location"></a>Přihlášení z cizí umístění 
Riziková událost spouštěný Pokud se uživatel úspěšně přihlásí z do nového umístění (IP, zeměpisná šířka a délka a ASN).

###<a name="sign-in-risk"></a>Přihlašovací rizika     
V tématu rizika úrovně (přihlášení)

###<a name="sign-in-risk-policy"></a>Zásady přihlašovací rizika  
Podmíněné přístupu, který vyhodnotí riziko specifické přihlašovací a platí mitigations na základě předdefinovaného podmínky a pravidla.

###<a name="user-compromise-risk"></a>Riziko narušení zabezpečení uživatele     
V tématu rizika úrovně (narušení zabezpečení uživatele)

###<a name="user-risk"></a>Riziko pro uživatele    
V tématu rizika úrovně (uživatel narušení zabezpečení).

###<a name="user-risk-policy"></a>Zásady pro uživatele rizika     
Zásady podmíněného přístupu, které byly použity přihlášení a platí mitigations na základě předdefinovaného podmínky a pravidla.

###<a name="users-flagged-for-risk"></a>Uživatelé s příznakem pro rizika   
Uživatelé, kteří mají události rizika, které jsou aktivní nebo remediated

###<a name="vulnerability"></a>Chyba    
Konfigurace nebo stav služby Azure Active Directory, takže adresáři na zneužití nebo hrozeb.


## <a name="see-also"></a>Viz taky

- [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md)
