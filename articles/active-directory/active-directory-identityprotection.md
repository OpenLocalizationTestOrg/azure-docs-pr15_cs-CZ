<properties
    pageTitle="Ochrana Identity Azure Active Directory | Microsoft Azure"
    description="Zjistěte, jak Ochrana Azure AD Identity umožňují omezit možnost mohl využívat hostují identity nebo zařízení a k zajištění identitu nebo zařízení, která byla dříve podezřelou nebo známo, že být."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Ochrana Identity Azure Active Directory 

Azure Active Directory Identity Protection je funkce edition P2 Azure AD Premium, která obsahuje konsolidované zobrazení do rizika událostí a potenciální chyby došlo k ovlivnění identit vaší organizace. Microsoft byla myší deset let aplikace zabezpečení cloudové identit a s ochranou Azure AD Identity Microsoft je zpřístupnění tyto stejné systémů ochrany podnikoví uživatelé. Ochrana identity využívá existující Azure AD společnosti odchylky zjišťování možnosti (dostupné prostřednictvím Azure AD neobvyklých aktivity sestavy) a uvádí nové typy události rizika, které rozpozná odchylky v reálném čase.



##<a name="getting-started"></a>Začínáme

Většina porušení zabezpečení se provede po útočníků získat přístup k prostředí krádeže identity uživatele. Útočníků se stále efektivní využití narušením třetích stran a pomocí útoky typu phishing sofistikované. Jakmile se zlými úmysly získá přístup k i nízkou privilegovaných uživatelský účet, je poměrně jednoduchá pro ně k získání přístupu k prostředkům důležité společnosti prostřednictvím boční pohyb. Proto je nutné zamknout všechny identit a při ohroženo identitu včasným zabránit hostují identity je překročen. 

Zjišťování hostují identity je snadno úkol. Naštěstí můžou pomoct ochrana Identity: ochrana Identity pomocí algoritmů výukové adaptivní počítače a heuristiku zjistí odchylky a rizik události, které mohou poukazovat, aby byly ohroženo identitu.
 
Pomocí těchto dat, ochrana Identity vygeneruje sestavy a výstrahy, které umožňují prozkoumat tyto rizika události a odpovídající remediation nebo omezení rizik akce.
 
Ale Azure Active Directory Identity Protection více než nástroji sledování a vytváření sestav. Na základě rizika událostí, ochrana Identity vypočítá úroveň rizika uživatelů pro každého uživatele, umožňují konfigurovat založeny rizika zásady ochrany automaticky identit vaší organizace.  Tyto zásady založené na rizika, kromě ovládacích prvků podmíněného přístupu poskytované Azure Active Directory a EMS, můžete automaticky blokovat nebo nabízejí adaptivní remediation akce, které obsahují resetování hesla a prosazování vícefaktorové ověřování.  

####<a name="explore-identity-protections-capabilities"></a>Prozkoumejte možnosti ochrana Identity 

**Zjišťování rizik událostí a riziková účty:**  

- Zjištění 6 typy událostí rizika pomocí počítače učení a heuristické pravidel 

- Výpočet úrovně rizika uživatele

- Poskytování vlastní doporučení zlepšit celkový postoje zabezpečení zvýrazněním chyby



**Zjišťování rizik události:**

- Odeslání oznámení událostí rizika

- Zjišťování rizik události pomocí příslušných a kontextové informace

- Poskytují základní pracovních postupů můžete sledovat vyšetřování

- Poskytují snadný přístup k akcím remediation například resetování hesla



**Podmíněné přístupu na základě rizika zásady:**

- Zásady zmírnit riziková přihlášení pomocí blokování přihlášení nebo vyžadování výzvy vícefaktorové ověřování.

- Zásady pro blok nebo zabezpečené riziková uživatelských účtů

- Zásady mají uživatelé zaregistrujte vícefaktorové ověřování


## <a name="detection-and-risk"></a>Zjišťování a rizika

### <a name="risk-events"></a>Riziko události

Riziko události jsou události, které byly označena jako podezřelé ochranou identit a označuje, že identitu může mít byla ohroženo. Úplný seznam události rizika najdete v článku [typy rizika události zjištěné ochranou Azure Active Directory Identity](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Úroveň rizika

Úroveň rizika riziková událost údaj (vysoké, střední nebo nízká) závažnosti riziková událost. Úroveň rizika pomáhá uživatelům ochrana Identity nastavit jejich priority akce, které musíte provést Pokud chcete snížit riziko uživatelům ve své organizaci. Závažnosti riziková událost představuje: Zkontrolujte sílu signálu jako prognostických narušení zabezpečení identity s množství hluku, která obvykle seznámí. 

- **Vysoký**: vysoké spolehlivosti a vysoký závažnosti riziková událost. Tyto události, které jsou silných indikátory, které ohroženo identita uživatele a všechny uživatelské účty vliv mají remediated okamžitě.

- **Střední**: vysoké závažnosti, ale nižší spolehlivosti riziková událost, nebo naopak. Tyto události, které jsou potenciálně nebezpečné a měli remediated vliv na všechny uživatelské účty.

- **Nízká**: nízké spolehlivosti a nízké závažnosti riziková událost. Tato událost nevyžadují okamžitou akci, ale v kombinaci s dalších událostí rizika můžete obdržet silných označení, že je ohroženo identitě. 


![Úroveň rizika] (./media/active-directory-identityprotection/01.png "Úroveň rizika")

 

Riziko události buď podle **v reálném čase**, nebo v po zpracování po provedení riziková událost už umístěte (offline). Aktuálně většina rizika událostí v ochrana Identity jsou vyhodnocované offline a zobrazí v ochrana Identity hodin 2-4 při instalaci. Když Vyhodnocená v v reálném čase, v reálném čase rizikové událostí se zobrazí v konzole ochranu Identity 5 až 10 minut.

Několik starších klientů aktuálně nepodporují zjišťování událostí v reálném čase rizika a ochrana před. Přihlášení z těchto klientů v důsledku toho nelze zjistit nebo zabránit v reálném čase.


## <a name="investigation"></a>Vyšetřování
Cesty přes ochrana Identity obvykle začíná na řídicím panelu ochrana Identity. 

![Remediation] (./media/active-directory-identityprotection/1000.png "Remediation")

Řídicí panel vám umožňuje přístup k:
 
- Sestavy například **Uživatelé příznakem pro rizika**, **rizika události** a **chyby**
- Nastavení, například konfigurace **Zásad zabezpečení**, **oznámení** a **Registrace vícefaktorové ověřování**
 

Je to obvykle výchozí bod pro vyšetřování, což je proces kontroly činnosti, protokoly a další relevantní informace související s riziková událost se rozhodnout, jestli jsou nutné remediation nebo omezení rizik kroky, a jak identitu byl ohroženo a principy používání hostují identity.

Můžete propojují vašich aktivit vyšetřování k [oznámení](active-directory-identityprotection-notifications.md) Azure Active Directory Protection odešle na e-mailu.

Následující části obsahují podrobnosti a kroky, které se vztahují k vyšetřování.  



## <a name="what-is-a-user-risk-level"></a>Co je úroveň rizika uživatele?

Úroveň rizika uživatelů údaj (vysoké, střední nebo nízká) pravděpodobnosti, aby byly ohroženo identita uživatele. To je vypočítána na základě události rizika uživatele, které jsou přidružené k identita uživatele. 

Stav riziková událost je **aktivní** nebo **uzavřená**. Pouze události rizika, které jsou **aktivní** přispět k výpočtu rizika uživatele. 

Úroveň rizika uživatelů spočítána pomocí následujících vstupů:

- Aktivní rizikové události ovlivňující ochranu uživatele
- Rizika stupeň tyto události 
- Jestli jste byli přesměrováni akcích remediation 

![Uživatel rizik] (./media/active-directory-identityprotection/1001.png "Uživatel rizik")



Můžete použít úrovně rizika uživatele k vytvoření zásad podmíněného přístupu blokování riziková uživatelů z přihlášení nebo vynutit, aby bezpečně změnit svoje heslo. 


## <a name="closing-risk-events-manually"></a>Zavření rizikové události ručně

Ve většině případů bude trvat nápravné akce například hesla resetovat automaticky zavřete rizikové události. Však to nemusí být vždy možné.  
Toto je, například v případě, že pokud:

- Uživatel s událostmi aktivní rizika odstranil
- Vyšetřování zjistí, že nahlášených riziková událost provést legitimní uživatel

Protože události rizika, které jsou **aktivní** přispívat do výpočtu rizika uživatele, bude pravděpodobně nutné ručně snížit úroveň rizika ukončením rizik události ručně.  
V průběhu vyšetřování můžete provést některou z těchto akcí můžete změnit stav riziková událost:

![Akce] (./media/active-directory-identityprotection/34.png "Akce")

- **Řešení** – Pokud po šetřit riziková událost, jako když jste akce odpovídající remediation mimo ochrana Identity a si myslíte, že riziková událost by měly považovat za zavřeli, použít označení události vyřešený. Přeložit události se nastavit stav riziková událost na Uzavřeno a riziková událost se už přispívání do riziko uživatele.

- **Označit jako falešně pozitivní** – v některých případech může prošetřit riziková událost a seznamte se s, aby byl nesprávně označena jako riziková. Snížit počet takové výskytů označením riziková událost jako falešně pozitivní. To vám pomůže počítače výukové algoritmů zlepšit klasifikace podobných událostí v budoucnu. Stav falešně pozitivní události, je **uzavřená** a už přispívají k riziko uživatele.

- **Ignorovat** – Pokud nebyly pořízené všech akcí remediation, ale chcete riziková událost, aby vás odebrala ze seznamu aktivní, můžete označit riziková událost ignorovat a stav události vám zavřený. Ignorované události není přispět k riziko uživatele. Tuto možnost, bude použito pouze neobvyklé okolnosti. 

- **Opětovná aktivace** - události rizika, které byly ručně ukončená (výběrem možnosti **řešení** **falešně pozitivní**a **Ignorovat**) můžete znovu aktivovat, nastavení stav události zpátky na **aktivní**. Události opětovně rizika přispívat uživatelské úrovně výpočtu rizika. Riziko události zavřený prostřednictvím remediation (jako bezpečné pro vytvoření nového hesla) nelze znovu aktivovat. 




**Otevřete dialogové okno související konfigurace**:

1. Na **ochranu Azure AD Identity** zásuvné klikněte v části **zjistit** **rizikové události**.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1002.png "Resetování hesla ručně")

2. V seznamu **rizika události** klikněte na rizika.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1003.png "Resetování hesla ručně")

2. Na zásuvné rizika klikněte pravým tlačítkem myši uživatele.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1004.png "Resetování hesla ručně")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Ruční zavřením všech událostí rizika pro uživatele

Místo ručně zavírání rizikové událostí pro uživatele jednotlivě, Azure Active Directory Identity Protection také poskytuje metodu zavřete všechny události pro uživatele jediným kliknutím.


![Akce] (./media/active-directory-identityprotection/2222.png "Akce")

Když kliknete na **Zavřít všechny události**, všechny události uzavřeny a příslušného uživatele už není riziko.



## <a name="remediating-user-risk-events"></a>Stav uživatelské rizika události

Remediation je akci, kterou chcete zabezpečené identitu nebo zařízení, která byla dříve podezřelou nebo známo, že být. Akce remediation obnoví identity nebo zařízení bezpečných stav a odstraňuje předchozí události riziko spojené s identity nebo zařízení.

Chcete-li nápravě události rizika uživatele, můžete:

- Provedení hesla obnovíte ručně nápravě uživatelské rizika události 

- Konfigurace uživatelských zásad zabezpečení rizika zmírnit nebo automaticky nápravě uživatelské rizika události

- Znovu obrázek infikovaných zařízení  


### <a name="manual-secure-password-reset"></a>Ruční zabezpečené heslo resetovat

Zabezpečené heslo resetovat je efektivní remediation pro mnoho událostí rizika a při provádění automaticky zavře tyto rizika události přepočítá úroveň rizika uživatelů. Můžete na řídicím panelu ochrana Identity zahajte resetování hesla pro riziková uživatele. 

Dialogové okno související dvěma způsoby různých resetování hesla:

**Resetování hesla** – vyberte **vyžadovat, aby uživatel resetování hesla** a povolte tak uživateli automatické obnovení, pokud uživatel zaregistroval pro vícefaktorové ověřování. Při jeho dalším přihlášení uživatel povinné úspěšně vyřešit výzvu vícefaktorové ověřování a potom muset změnit heslo. Tato možnost není dostupná, pokud je uživatelský účet není registrovaný vícefaktorové ověřování.

**Dočasné heslo** – vyberte **Generovat dočasné heslo** okamžitě platnost současné heslo a vytvářet nové dočasné heslo pro uživatele. Odešlete nové dočasné heslo na alternativní e-mailovou adresu uživatele nebo jeho nadřízeným. Protože dočasné heslo se uživateli zobrazí výzva k Změna hesla při přihlášení.


![Zásady] (./media/active-directory-identityprotection/1005.png "Zásady")


**Otevřete dialogové okno související konfigurace**:

1. Na **ochranu Azure AD Identity** zásuvné klikněte na **Uživatelé příznakem pro rizika**.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1006.png "Resetování hesla ručně")


2. Ze seznamu uživatelé vyberte uživatele s alespoň jeden rizikové události.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1007.png "Resetování hesla ručně")


2. Na zásuvné uživatele klikněte na **Resetovat heslo**.

    ![Resetování hesla ručně] (./media/active-directory-identityprotection/1008.png "Resetování hesla ručně")





## <a name="user-risk-security-policy"></a>Zásady zabezpečení rizika uživatele

Zásady zabezpečení rizika uživatele je podmíněné přístupu, který vyhodnotí úroveň rizika pro určitého uživatele a slouží k použití akce remediation a omezení na základě předdefinovaného podmínky a pravidla.


![Zásady ridk uživatele] (./media/active-directory-identityprotection/1009.png "Zásady ridk uživatele")


Ochrana Azure AD Identity usnadňuje správu omezení rizik a remediation příznakem pro rizika povolením uživatelů:

- Nastavení uživatelů a skupin, do kterých zásady platí pro: 

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1010.png "Zásady ridk uživatele")


- Nastavte uživatele rizika úrovně prahovou hodnotu (minimum, střední nebo vysokou), který spustí zásady: 

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1011.png "Zásady ridk uživatele")


- Nastavení ovládacích prvků vynucené při aktivaci zásady:

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1012.png "Zásady ridk uživatele")


- Přepnutí stavu svoje zásady:

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/403.png "Registrace MFA")


- Zkontrolujte a vyhodnocení dopad změn před aktivace:

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1013.png "Zásady ridk uživatele")


Výběr **horní** prahové hodnoty snižuje počet zásadu aktivaci a minimalizuje vliv na uživatele.
Ale nezahrnuje uživatelé **Minimum** a **Střední** příznakem pro rizika ze zásad, které nemusí zabezpečené identit nebo zařízení, které byly dříve podezřelou nebo známé být.

Při nastavování zásady,

- Vyloučení uživatelů, kteří mohou generovat spoustu pozitivní false (vývojářích, zabezpečení analytici)

- Vyloučení uživatelů v prostředí, kde není praktické povolení zásady (třeba přístup k linku)

- Použití zavedením **vysoké** mezní během počáteční zásad, nebo pokud výzvy prezentací, které koncoví uživatelé musí minimalizovat.

- Mezní hodnota **Minimum** použijte, pokud vaše organizace vyžaduje větší zabezpečení. Výběr mezní hodnoty **Minimum** uvádí další uživatelské přihlašovací stránky, ale zvýšené zabezpečení.

Doporučené výchozí pro většinu organizací jsou je třeba nakonfigurovat pravidlo pro **Střední** mezní narazila poměr použitelnosti a zabezpečení.

Základní informace o souvisejících uživatelské prostředí najdete v článku:

- [Tok obnovení účtu Compromised](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Účet Compromised blokované toku](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Otevřete dialogové okno související konfigurace**:

1. Na **ochranu Azure AD Identity** zásuvné v části **Konfigurace** klikněte na **rizika zásady pro uživatele**.

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1009.png "Zásady ridk uživatele")






## <a name="mitigating-user-risk-events"></a>Zmírnění uživatelské rizika události
Správci můžou nastavit zásady zabezpečení rizika uživatele k blokování uživatelů na přihlásit se v závislosti na úrovni rizika. 

Blokování přihlašování v:
 
- Zabrání vytvoření nové události rizika uživatele pro příslušného uživatele

- Umožňuje správcům ručně nápravě rizika události ovlivňující identita uživatele a v ní obnovit stav zabezpečení



## <a name="what-is-a-sign-in-risk-level"></a>Co je úroveň rizika přihlásit?

Úroveň přihlašovací rizika udává (vysoké, střední nebo nízká) pravděpodobnost, že pro určité symbol in, někoho jiného pokouší ověření identity uživatele. Úroveň přihlašovací rizika Vyhodnocená každá její položka při přihlašování v a byly použity rizikové událostí a indikátory zjištěnou v reálném čase u této konkrétní přihlášení. 

## <a name="mitigating-sign-in-risk-events"></a>Zmírnění rizik přihlašovací události 
Omezení rizik je akci, kterou chcete omezit možnost mohl využívat hostují identity nebo zařízení bez obnovení identity nebo zařízení bezpečných stavu. Omezení rizik nevyřeší předchozí události přihlašovací riziko spojené s identity nebo zařízení.

Můžete podmíněného přístupu v ochrana Azure AD Identity automaticky zmírnění rizik přihlašovací události. Pomocí těchto zásad, zvažte rizika stupeň uživatele nebo přihlášení k blokování riziková přihlášení nebo modrým provádět vícefaktorové ověřování. Tyto akce mohou zabránit, aby mohl využívání krádež identity způsobit škodu a může Počkejte chvíli zabezpečené identitě. 


## <a name="sign-in-risk-security-policy"></a>Zásady zabezpečení přihlašovací rizika

Zásady přihlašovací rizika jsou podmíněné přístupu, který vyhodnotí riziko specifické přihlašovací a platí mitigations na základě předdefinovaného podmínky a pravidla.

![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1014.png "Přihlašovací zásad rizika")


Ochrana Azure AD Identity slouží ke správě zmírnění riziková přihlášení povolením:

- Nastavení uživatelů a skupin, do kterých zásady platí pro: 

    ![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1015.png "Přihlašovací zásad rizika")


- Nastavte přihlašovací rizika úrovně prahovou hodnotu (minimum, střední nebo vysokou), který spustí zásady: 

    ![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1016.png "Přihlašovací zásad rizika")


- Nastavení ovládacích prvků vynucené při aktivaci zásady:  

    ![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1017.png "Přihlašovací zásad rizika")
    

- Přepnutí stavu svoje zásady:

    ![Registrace MFA] (./media/active-directory-identityprotection/403.png "Registrace MFA")

- Zkontrolujte a vyhodnocení dopad změn před aktivace: 

    ![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1018.png "Přihlašovací zásad rizika")


### <a name="what-you-need-to-know"></a>Co je potřeba vědět

Můžete nakonfigurovat zásady zabezpečení přihlašovací rizika, které vyžadují vícefaktorové ověřování:

![Přihlašovací zásad rizika] (./media/active-directory-identityprotection/1017.png "Přihlašovací zásad rizika")

Však z bezpečnostních důvodů toto nastavení jde použít pouze pro uživatele, které již byly registrované pro vícefaktorové ověřování. Pokud má požadovat vícefaktorové ověřování je splněny pro uživatele, který ještě není registrovaný u vícefaktorové ověřování, je blokován uživatele. 

Osvědčený Pokud chcete vynutit vícefaktorové ověřování pro riziková přihlášení, postupujte následujícím způsobem:

1. Povolte [zásady registrace vícefaktorové ověřování](#multi-factor-authentication-registration-policy) pro ovlivněné uživatele.
2. Vyžadovat ovlivněné uživatele k přihlášení v relaci riziková provádět registrace MFA

Dokončení těchto kroků zajišťuje, že vícefaktorové ověřování je nutný pro funkce riziková přihlašovací. 


### <a name="best-practices"></a>Doporučené postupy

 
Výběr **horní** prahové hodnoty snižuje počet zásadu aktivaci a minimalizuje vliv na uživatele.  
 
Ale nezahrnuje **Minimum** a **Střední** přihlášení příznakem pro rizika ze zásad, které nemusí blokovat mohl z využívání hostují identity. 

Při nastavování zásady, 

- Vyloučení uživatelů, kteří nemají / nesmí obsahovat vícefaktorové ověřování

- Vyloučení uživatelů v prostředí, kde není praktické povolení zásady (třeba přístup k linku)

- Vyloučení uživatelů, kteří mohou generovat spoustu pozitivní false (vývojářích, zabezpečení analytici)

- Použití zavedením **vysoké** mezní během počáteční zásad, nebo pokud výzvy prezentací, které koncoví uživatelé musí minimalizovat.

- Mezní hodnota **Minimum** použijte, pokud vaše organizace vyžaduje větší zabezpečení. Výběr mezní hodnoty **Minimum** uvádí další uživatelské přihlašovací stránky, ale zvýšené zabezpečení.

Doporučené výchozí pro většinu organizací jsou je třeba nakonfigurovat pravidlo pro **Střední** mezní narazila poměr použitelnosti a zabezpečení.

 
Zásady přihlašovací rizika jsou:

- Použije na všechny přenosy prohlížeče a přihlášení pomocí moderní ověřování.
- Není použit pro aplikace, které používají protokoly starší zabezpečení zakázáním koncový bod WS zabezpečení na federované IDP, například služby AD FS.

Stránka **Rizikové událostí** v konzole ochrana Identity obsahuje seznam všech událostí:

- Použití této zásady
- Zkontrolujte aktivity a určit, zda byla akce odpovídající 

Základní informace o souvisejících uživatelské prostředí najdete v článku:

- [Obnovení riziková přihlášení](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Riziková přihlašovací blokované](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Registrace vícefaktorové ověřování během riziková přihlášení](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Otevřete dialogové okno související konfigurace**:

1. Na zásuvné **Ochrana Azure AD Identity** v části **Konfigurace** klikněte na **Sign in zásad rizika**.

    ![Zásady ridk uživatele] (./media/active-directory-identityprotection/1014.png "Zásady ridk uživatele")





## <a name="multi-factor-authentication-registration-policy"></a>Zásady registrace vícefaktorové ověřování

Azure vícefaktorové ověřování je metodu ověření uživatele, která je potřeba použít větší kontrolu nad uživatelské jméno a heslo. Poskytuje druhou vrstvu zabezpečení a uživatel přihlášení, začátky transakce.  
Doporučujeme vyžadovat Azure vícefaktorové ověřování pro přihlášení uživatele, protože ho:

- Poskytuje silné ověřování s celou řadu možností snadno ověření

- Hraje úlohu při přípravě organizaci chránit před a obnovení účtu kompromisům klíče

![Zásady ridk uživatele] (./media/active-directory-identityprotection/1019.png "Zásady ridk uživatele")



Další informace najdete v tématu [Co je Azure Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md)


Ochrana Azure AD Identity slouží ke správě zavádění registrace vícefaktorové ověřování pomocí konfigurace zásad, který umožňuje: 



- Nastavení uživatelů a skupin, do kterých zásady platí pro: 

    ![Zásady MFA] (./media/active-directory-identityprotection/1020.png "Zásady MFA")



- Nastavení ovládacích prvků vynucené při aktivaci zásady:  

    ![Zásady MFA] (./media/active-directory-identityprotection/1021.png "Zásady MFA")


- Přepnutí stavu svoje zásady:

    ![Zásady MFA] (./media/active-directory-identityprotection/403.png "Zásady MFA")

- Zobrazte aktuální stav registrace: 

    ![Zásady MFA] (./media/active-directory-identityprotection/1022.png "Zásady MFA")


Základní informace o souvisejících uživatelské prostředí najdete v článku:

- [Tok registrace vícefaktorové ověřování](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Registrace vícefaktorové ověřování během riziková přihlášení](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Otevřete dialogové okno související konfigurace**:

1. Na **ochranu Azure AD Identity** zásuvné v části **Konfigurace** klikněte na **Registrace vícefaktorové ověřování**.

    ![Zásady MFA] (./media/active-directory-identityprotection/1019.png "Zásady MFA")





## <a name="next-steps"></a>Další kroky

 - [Kanál 9: Azure AD a Identity zobrazit: náhled ochranu Identity](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Typy rizika události zjištěné ochranou Azure Active Directory Identity](active-directory-identityprotection-risk-events-types.md)
 - [Chyby detekovaný ochranou Azure Active Directory Identity](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory Identity Protection oznámení](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory Identity Protection playbook](active-directory-identityprotection-playbook.md)
 - [Glosář služby Azure Active Directory ochrana Identity](active-directory-identityprotection-glossary.md)

 - [Přihlašovací zkušenosti s ochranou Azure AD Identity](active-directory-identityprotection-flows.md)

 - [Povolení ochrany Identity Azure Active Directory](active-directory-identityprotection-enable.md)
 - [Azure Active Directory ochrana Identity – jak zrušit blokování uživatelů](active-directory-identityprotection-unblock-howto.md)

 - [Začínáme s Azure Active Directory Identity Protection a Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


