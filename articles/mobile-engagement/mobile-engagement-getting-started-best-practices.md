<properties 
    pageTitle="Azure mobilní zapojení příručku Začínáme s doporučené postupy"
    description="Získání příručce Začínáme pro zapojení Mobile Azure a osvědčené postupy pro rychlého připojení" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure mobilní zapojení - Příručka Začínáme s doporučené postupy

## <a name="overview"></a>Základní informace

**Mobilní obrazovky je velmi stránka přeplněná mezera:** V 2013 studie nalezené že průměr mobilního zařízení měli 27 nainstalované aplikace. Uživatelé obvykle stráveného 30 hodin za měsíc jejich aplikace. Většina tentokrát uplynul na sociální sítě a herní (asi 20 hodin). Podle 2014 Android market dosáhl miliónů 1,5 aplikací pro uživatele můžete vybírat. Apple storu obsažené miliónů 1.2 aplikace. Používání mobilní aplikace stále roste jako vývojáři soutěží v tomto Kvetoucí market. 

Průměrná mobilní uživatel bude instalace a odinstalace aplikace s vysokou četnost v závislosti na Změna zájmy a dojde k v aplikaci. Abyste mohli určit úspěch aplikace z něj stal důležité vědět větší kontrolu nad kolik uživatelé nainstalovat aplikaci. Je důležité vědět, jak užitečné je aplikace a pokud trendem použití změny. Důležité se stanou na následující otázky:

- Jsou uživatelé začátek zobrazíte aplikace uninteresting nebo zastaralé? 
- Kolik uživatelů se zastavily vůbec používat aplikaci? 
- Jsou v aplikaci nákup nejoblíbenější směrem nahoru nebo dolů?
- Selhává uživatelů k dokončení práce toků kvůli problémům s aplikací nebo chybějící potřebné? 
- Může uchováváte aplikace užitečné a relevantními značkami stisknutím svěže obsah se základním uživatele? 
- By toto nové obsah shodovat s pro všechny uživatele nebo zaměření na uživatele segmentů na základě chování v aplikaci? 
 
Odpovědi na otázky vypadat přibližně takto může pomoct rozšířit životnost a výnosů z aplikace. Můžete taky pomáhají definovat a zachovat základním uživatele. 

Média související aplikace zpravidla část nejvyšší uchovávání informací mezi uživateli. Jeden to proto, že budou pořád poskytují svěže obsahu uživatelům. Nejdříve možné přijetí užitečné nabízená oznámení na uživatelský segment může mít vysoké dopad na uchovávání informací aplikace. 

Program Azure Mobile zapojení usnadňující rozšířit životnost a uchování aplikace poskytnutím metodu shromáždění a analyzovat podrobné informace o použití aplikace. Průvodce vám pomůže klasifikaci základní podle chování uživatele a vytvoření fokusem kampaní aby poskytly nabízená oznámení a zprávy v aplikaci segmentů identifikované uživatele. Klíčové ukazatele výkonu (KPI) změřit, jak aktivní uživatelé se s různými aspekty aplikace. Azure zapojení Mobile poskytuje metody potřebujete zjistit tyto klíčové ukazatele výkonu. Je dobré zvýšit poskytuje infrastrukturu potřebujete víc zapojili pomocí mobilní aplikace návratnost investic (Návratnost). 

K využívejte zapojení Mobile Azure, budete muset začínat plán dobře navržená engagement. Váš plán vám pomůže identifikovat podrobného data, která byste potřebovali moct segmentech základním uživatele. Můžou být založená na chování a dojde k v aplikaci. U vašeho plánu úspěšné, je doporučený postup jasně určit klíčový ukazatel výkonu, který spočítá cíle aplikace. S ukazateli výkonu vymazat definované můžete snadno vložit nezbytné logiky aplikace shromažďovat graf nebo barev data, která se používají k analýze a vyhodnotíte klíčových ukazatelů výkonu. Toto téma je nejlepší praktické cvičení Průvodce pro definování klíčových ukazatelů výkonu, které budete používat s plánem můžete zapojit. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Krok 1: Definování klíčových ukazatelů výkonu modelu Tip na šířku


Správně definování klíčových ukazatelů výkonu může být obtížné úkolů k dokončení. Aplikace určený pro jinou odvětví mít vlastní zvláštnosti a cílů. Toto jsou, obvykle splést postup. Chcete-li této situaci předejít, cílů a klíčových ukazatelů výkonu by měl rozdělit do tří hlavních kategorií: **obchodní** **zapojení**a **technické**. Toto je označujeme **Tip modelu**.

Dobrý plán většinou tvoří cílů klíčové ukazatele výkonu, které změřit úspěšných pokusů v žádném z těchto kategorií modelu tip.


#### <a name="business-kpis"></a>Obchodní klíčové ukazatele výkonu

Klíčové ukazatele výkonu firmy by měly být nejjednodušší součástí vytvářet. Jste pravděpodobně již definované v některých formuláře plánu mobilní aplikace. Tyto klíčové ukazatele výkonu obecně míra výnosů a usnadní návratnost investic se aplikace. Následující seznam uvádí několik příklad firmy klíčové ukazatele výkonu, které vám mohou pomoci vás při definování ukazatele výkonu:

- Média firmy klíčové ukazatele výkonu
    - Kliknutí na počet služby Active Directory
    - Číslo stránky návštěvy na uživatele
    - Číslo aktuálního předplatného
- Herní firmy klíčové ukazatele výkonu
    - Počet nákupů ve aplikace
    - Průměrný výnos na uživatele (ARPU)
    - Čas strávený relace
    - Dny přehrát a aktuální herní úrovni
- Klíčové ukazatele výkonu obchodování Business
    - Použít aplikaci dnů
    - Průměrný výnos na uživatele (ARPU)
    - Průměrná částka v košíku při platbě na pokladně
    - Kategorie produktů pro většinu zobrazení a nákupu
- Banky a pojištění firmy klíčové ukazatele výkonu
    - Počet účtů
    - Funkce aktivován
    - Stránky nabídky navštívili
    - Upozornění na kliknutí nebo aktivaci      



#### <a name="engagement-kpis"></a>Zapojení klíčové ukazatele výkonu

Zapojení KPI je ukazatel výkonu změřit zapojení uživatelů. Trendy v této oblasti zjistit, uchovávání informací aplikace. Tady je několik příklad ukazatele výkonu pro tento typ klíčového ukazatele výkonu:

- Aktivní uživatelé v posledních 7 dnech.
- Neaktivní uživatele počet různých hodnot za posledních 7 dnů
- Počet uživatelů, kteří nepoužili aplikaci 30 dní.  

Některé zjevných vnější faktory může ovlivnit indikátory v této oblasti. Můžete třeba zvážit mobilního zařízení s uživatelem je vždy. Může nebo nemusí být pravdivé. Herní aplikace může mají mít vyšší použití kolem svátky, kdy hráče může přehrát další při mimo kancelář nebo mimo škole.   

Dobře definované klíčové ukazatele výkonu v této kategorii potřebné informace vyhledat změřit vztah mezi aplikace a zákazníky.



#### <a name="technical-kpis"></a>Technická klíčové ukazatele výkonu

Ukazatele výkonu v této kategorii, která vám pomůže určit Pokud aplikace se chová správně, je zablokovaný nebo k selhání. Indikátory můžete změřit stav aplikace a určení použitelnosti problémů, které můžou zabránit uživatelům v používání aplikace. Informace shromážděné u této kategorie může taky obsahovat výkonu informace, které mohou být významné marketingu týmy. Data může být také užitečné pro řešení potíží s tak, že IT a podpora týmy k identifikaci unreported chyby. 
 
Tady je pár příkladů technické klíčových ukazatelů výkonu:

- Informace o výjimce neošetřené nebo zpracování a počtu 
- Časové razítko pro poslední havárie
- Kliknutí na tlačítko poslední nebo navštívili poslední stránky
- Využití paměti aplikace
- Aplikace snímků
- Verze operačního systému, který běží v aplikaci
- Verze aplikace

Definování tyto klíčové ukazatele výkonu vám usnadní měření výkonu aplikace a zdůraznit potenciální chyby. Toto pole Indikátory by měl snížit čas strávený při opravě pro zákazníky. Může taky pomáhají určit uživatelský segment, který jste narazili určité problémy. Tento uživatel segmentace slouží k vytvoření kampaní předvádění oznámení o dostupných opravy a potenciální propagace pomáhá při obnovení spokojenost zákazníků. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook cvičení 1: Vytvoření řídicího panelu klíčového ukazatele výkonu

Při definování strategie marketingové klíčových ukazatelů výkonu by měly prezentovat zobrazení všech cílů hlavní. Měly by být jasně definované datových bodů, které vám umožní získat informace o důležitých informací ke sledování aplikace a chování koncový uživatel.

Vytvoření klíčového ukazatele výkonu řídicí panel, který obsahuje pod informace

1.  Jaké jsou klíčových ukazatelů výkonu aplikace?
2.  Jaké datové body bude umožňuje znázornit jednotlivých klíčových ukazatelů výkonu?
3.  Umístění tato data pro aplikaci (tedy obrazovku nastavení, systém...)
4.  Můžete přehrávat sekvenci využití tohoto klíčového ukazatele výkonu

Používáte list **Builder klíčového ukazatele výkonu** v naší [Médií Playbook šablony] [ Media Playbook link] příklady a pokyny.



## <a name="step-2-your-engagement-program"></a>Krok 2: Aplikaci můžete zapojit


Program skvělé mobilní zapojení považovat za hlavní součástí aplikace. To opravdu by měla obsahovat skvělé úvodní program, který spustí pro uživatele během první dny využití aplikace. To může mít velmi pozitivní vliv na zaměstnání a uchování aplikace. Studie projevili, že většina uživatelů nepoužívali aplikace prvních pár dnů po instalaci. Chcete-li snažit zahájit nebo při uživatel přesto věnovaných aplikace brzy překročit řidiče k jízdě úrok zákazníkům očekávání. Zkontrolujte, že budete prezentovat hodnoty klíče a výhody aplikace pro zákazníky. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Nabízená oznámení jsou nejvhodnější přístup k začátku závazky s uživateli mobilního zařízení. Však skvělé opatrnost Pokud rozdělíte uživatelé nabízených oznámení. Vzhledem k tomu, když uživatel zdá se zobrazují nevyžádané pošty nebo uninteresting oznámení, může mít vliv na vážně. Několikrát kliknout může uživatel aplikaci nikdy se vrátíte odstranit. Uživatel by měl příjem vysoce přizpůsobených hodnotu v aplikaci místo obecný nevyžádané pošty.

Po nastavení uživatelů aktivně provozují, pak aplikace zapojení pomůže jednotky další aspekty aplikace.

Můžete například nastavení kampaně požadující aktivní uživatelé postup ohodnocení aplikace. Vzhledem k tomu uživatelský segment je většina aktivní a má většina zkušenosti s vámi aplikace, očekáváte, aby co nejpřesnější hodnocení. Až budete mít hodnocení vysoká aplikace, může pomoci jednotky nahoru organický – stáhnout aplikaci i snížit nové náklady pořízení zákazníka.



#### <a name="engagement-sequence"></a>Zapojení sekvence


Globální Program zapojení zahrnuje různé zapojení řady. Každý posloupnost cílem je dosáhnout několik cílů.


###### <a name="life-push-sequence"></a>Životnost nabízených sekvence


Cíle posloupnost nabízených životnost se liší podle toho životním cyklu přijetí uživatele s aplikací. Určitému uživateli může být nové neaktivní nebo velmi aktivní. V různých fází životního cyklu zapojení mohou uživatelé využít svěže obsahu ve formě tipy a odkazů dokumentaci. 

Například vytvoří nový uživatel potřebovat nápovědy začíná určený k aplikaci případně těžit z nového uživatele pobídky podobně jako tento při prvním spuštění aplikace...

*"Rádi jste palubě! Nezapomeňte se přihlásit k získání vaší 1st měsíc volné."*


###### <a name="behavioral-push-sequence"></a>Chování nabízených sekvence

Posloupnost chování nabízených chce zvýšit využití podle uživatele chování shromažďované aplikace.  

Velmi aktivních uživatelů v aplikaci football virtuálních můžou využívat zapojení se následující nabízená oznámení...

*"Jan jsou ventilátory vážně football! Log in to naše Fotbalových oddíl a win bezplatné přístup k SuperBowl!"*


###### <a name="alerting-push-sequence"></a>Výstrahy nabízených sekvence

Uživatelé ocení relevantní informace na své zájmy zaměření. Sekvenci nabízená oznámení zlepšuje zapojení tak, že podle zájmy upozornění, že uživatel jasně ukazuje. Může to být explicitní, když uživatel vybere svoje zájmy v aplikaci. Ji může taky stanovit, implicitně na základě dat shromážděných při interakci uživatele s aplikací.

Například uživatele aplikace obchodování může pravidelně si koupit konkrétní značku kávy, které jste zaznamenali s obchodní klíčového ukazatele výkonu. Následující oznámení můžete být díky dokonalejšímu využití tohoto uživatele s aplikací.
 
*"Hi společnost Wes, mezi oblíbené značky kávy bude na prodej 25 % vypnutí seznamu první týden roku 2015 dne. Můžete vážíme jako zákazníka a chtěli Přesvědčte se, zda jste byli vědomi."*

###### <a name="rentention-push-sequence"></a>Rentention nabízených sekvence

Tato řada cílem je uchovávání uživatelů pomocí oznámení kampaně opakujících nabízených jednotka běžná zvykněte atraktivních s aplikací. To můžete zvýšit uchovávání informací aplikace Pokud uživatel má interakce. 

Například související aplikace sports může se zobrazit následující nabízená oznámení týdenní podle Oblíbené týmy uživatele:

*"Umožňující win 200 bodů, najdete v článku hlasování zda Yankees New York bude win jejich hra tento týden před Ostrava modrá Jays!"*


#### <a name="the-3w-approach"></a>3W přístup

Zvládnutí sekvence různých nabízených vám pomůže zapojit s koncovým uživatelům. Ale potřebujete 3W možnost používat pro individuální nastavení vám oznámení. Přístup 3W by měl adresu, která co a kdy u jednotlivých oznámení. Pokud jasně splníte tyto tři otázky můžete oznámení by měl správně zaměřit pro zapojení.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Kdo: Uživatel segmentech, který bude přijímat zprávy

Předání oznámení uživatelům považovat za kanálu velmi důvěrné komunikace. Zkontrolujte, jestli oznámení cíl odešlete uživatelský segment jsou i s rozsahem zájmů, které uživatelský segment. Oznámení nesprávně směrované je velmi pravděpodobné mít negativní vliv na uživatele. Může to považují za spam vést k aplikaci odinstalovat. 

Použijte kombinaci funkcí specifických kritérií technické a chování při definování uživatele segmentů dostanou oznámení. Jednoduchý příklad definování uživatelský segment může být podobný následující příkaz:

"Všichni uživatelé, kteří spuštění mobilní aplikaci pro první čas třemi dny a navštívili na přihlašovací stránku dvakrát bez skutečného dokončení přihlášení".
 
Tento příkaz vám pomůže identifikovat data, která by bylo nutné shromáždit podporuje konkrétním scénáři.


###### <a name="what-the-message-that-you-will-send"></a>Co: Bude odesílané zprávy

**Tón**

Ve vaší závazky použít tónu, která odpovídá vaší Segmentovaný uživatelů. Toto je jasné spolehlivých způsobů, jak komunikovat s vaší koncových uživatelů a propagace uživatele zájem o svoji aplikaci. 

**Přesměrování**

Nabízená oznámení se dá použít pro více než otevření aplikace. Pokud oznámení nabízí kontext například vysílání zprávy nebo podpora produktu, toto oznámení může hloubkové odkaz přímo ke správnému obsahu v rámci aplikace. K podpoře to, musíte vytvořit adresu URL schéma chcete, aby aplikace Správa přesměrování. Když pracujete na zapojení sekvence, je důležitým krokem, nesmí být zapomněli.

Přesměrování dá ovládat taky pro jiné systémy. Například adresa URL akce je možné přesměrovat koncoví uživatelé mnoho jiných systémů, včetně následujících:

- Na webu
- Poštovní schránka se už nastavení e-mailu
- Pole ovládací prvek služby SMS
- Telefonické připojení služby
- Přímo do aplikace obsahují hodnocení aplikace. 

Nabízí mnoho příležitostí zapojit koncových uživatelů a vytvářet pravidla pro automatické zlepšit hudebního vystoupení.


**Formát a obsah**

Různé typy a formáty nabízená oznámení:

1. **Oznámení** : umožňuje odesílat zprávy reklamě uživatelům v různých chvíli (mimo aplikaci v aplikaci nebo pokaždé, když).
2. **Hlasování** : možnost shromáždění informací ze koncových uživatelů tak, že je pokládání otázek. Tyto odpovědi budou k dispozici při vytváření kritéria cílové koncovým uživatelům.
3. **Posune data** : umožňuje odeslat binární nebo ve formátu Base 64 datového souboru, který se aktualizovat aplikaci. Informace obsažené v dat nabízených odeslaný do aplikace k přizpůsobení možností uživatelů v aplikaci. Aplikace potřebuje s cílem podporu data ve nabízených data.
4. **Dlaždice (pouze pro Windows Phone)** : povoleno používat Microsoft nabízená oznámení služby (MPNS) k odeslání nativní nabízená oznámení s daty XML (podporované od SDK verze 0.9.0. Konečný datové části pro dlaždice nesmí překročit 32 kilobajtů.). Tato zpráva se zobrazí přímo na dlaždici vaší vývěsky.
5. **Webové zobrazení** : zobrazení webového je místní obsahující obsah webu. Tento automaticky otevírané okno se zobrazí, když koncový uživatel klikl nabízená oznámení o. Zobrazení webového umožňuje mít další interakce s koncovým uživatelem.
 
>[AZURE.NOTE] Ujistěte se, že obsah, který odesíláte jako nabízená oznámení v souladu s platformu odpovídajících (iOS, Android, Windows) pokyny k vývoji aplikací a posílají nabízená oznámení.

 


###### <a name="when-the-timing-of-your-campaign"></a>Kdy: Časování kampaně

Kdy je nejvhodnější doba k aktivaci kampaně aktivaci nabízená oznámení? Pokud se ručního nebo automatické? Ho opakované? Určení správný čas nebo počet_plateb je nutné zapojit uživatelé, kteří mají nejlepších výsledků. Pro každý zapojení pořadí a scénáře, třeba určit, kdy bude ten nejlepší čas odeslání nabízená oznámení. Tady je několik příkladů:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Jestliže odesíláte mnoho oznámení denně, je nutné provést vážně úvahu, že uživatelé mohou vnímat komunikace za nevyžádanou poštu. 

Azure zapojení Mobile obsahuje dva způsoby, jak se tím vyhnout komunikace je považován za nevyžádanou poštu. Nejdřív pomocí pořádku zrno segmentace zajistit, že že nejsou zaměřené uživatelům. Kromě toho Azure Mobile zapojení poskytuje funkce "kvót". Tato funkce můžete omezit oznámení odeslané s kampaní. Příklad nastavení výchozí kvóta 5 pro každý týden zajistí, aby uživatel součástí část uživatelský segment kampaně obdrží upozornění na víc než 5 pro tento týden.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook cvičení 2: Vytvoření aplikace můžete zapojit

Věnujte chvíli sumarizovat cílů a definování kampaní plánujete přehrát pomocí konkrétní řady. Zkontrolujte, že použijete přístup 3W oznámení v kampaní. 

Použijte sešit **Zapojení Program** [Médií Playbook šablony] [ Media Playbook link] příklady a pokyny.


## <a name="step-3-app-integration"></a>Krok 3: Integrace aplikace


#### <a name="create-a-tag-plan"></a>Vytvoření plánu značky

Integrovat Azure Mobile zapojení do aplikace musíte vytvořit plán značku. Plán značku je základního pilíře projektu. Definuje vztah mezi marketingové specifikace průběhu prací aplikace a skutečnou značky dat shromážděných v aplikaci změřit klíčových ukazatelů výkonu. Informuje o jaké analýzy nebude moct vidět na portálu. Také umožňuje definovat segmentů uživatele a odeslat fokusem nabízená oznámení provozovat vaši koncoví uživatelé. Jakmile máte plán značku definované, přidání, že je jednoduchý pomocí SDK Azure Mobile zapojení kód integrovat do aplikace.

Plán značku všechno, co byste neměli označení v aplikaci. By měl obsahovat pouze značky dat, která je součástí strategie mobilní engagement. To bude pravděpodobně různorodého mezi aplikacemi. [Šablona Playbook médií] [ Media Playbook link] za předpokladu, že tak, že pomáhá Azure Mobile zapojení vytvoření plánu značku dané způsobem. Použijte **Značku plán** list jako vodítko k vytvoření plánu značku.

Při definování oddílu značku v listu, třeba specifických. To je velmi důležité, abyste se vyhnuli zapomíná a to. Podrobnosti každé očekávání situace, ve kterém každou značku odešle. Uveďte název aktivity, kde je vložený každou značku. To všechny zahrnuté do **Informative** část listu. List plánu značku by měl být hlavní odkaz test ověření. 

V části **Data, která chcete shromažďovat** měli najít týmu vývoje typy, názvy, hodnoty a další informace o klíč/dvojice povinné pro každou značku, který bude vložen do aplikace.

Doporučujeme revizí značku plán s všechny týmy přidružené k projektu. Proveďte potřebné změny a potvrďte, že všechno, co je jasné, marketingu a vývoj týmy.

**Výkaz práce** listu mohou sloužit k nápovědě průvodce, který každý, kdo v projektu.


#### <a name="data-types"></a>Datové typy

Toto jsou běžné typy dat nepodporuje Azure Mobile Engagement.

###### <a name="devices-and-users"></a>Zařízení a uživatelů

Azure zapojení Mobile identifikuje uživatele tak, že generování jedinečný identifikátor u každého zařízení. Tento identifikátor se nazývá identifikátor zařízení (nebo deviceid). Vygeneruje se tak, že všechny spuštěné na stejné zařízení sdílet stejné identifikátor zařízení.

###### <a name="sessions-and-activities"></a>Relace a aktivity

Relace je jedna instance aplikace spouštíte uživatelem. Relace se nachází od času spuštění aplikace do ukončení.

Aktivita je logické seskupení sadu věci, které aplikace může dělat během relace. Je obvykle konkrétní obrazovky v aplikaci, ale je možné, že něco definované logickou aplikace. Minimálně by měl označení jednotlivých obrazovky nebo aktivitu aplikace. To vám umožní pochopit cesta uživatele.


###### <a name="events"></a>Události

Události se používají k vykazování interakci uživatele s aplikací. Mohou být rychlé akce, jako jsou sdílení obsahu nebo spuštění videa. Označení události vám poskytne kolekce dat, které ukazují, jak uživatelé pracují s aplikací. 

###### <a name="jobs"></a>Úlohy

Práce se používají vykazování akce, které mají po dobu. By je několik příkladů:

- Spuštění volání rozhraní API
- Zobrazí čas služby Active Directory
- Doba trvání úkolů pozadí 
- Doba trvání proces nákupu
- Zobrazení videa


###### <a name="errors"></a>Chyby

Chyby se používají k oznamujte problémy zjištěná aplikace. Například nesprávné uživatelské akce nebo selhání rozhraní API volání.

###### <a name="application-information"></a>Informace o aplikaci

Informace o aplikaci (informace App) slouží k označení data vztahující se k práci uživatelů s aplikací. Je generováno aplikací interakci uživatele s aplikací. 

Pro dané informace app klíč Azure Mobile zapojení pouze uchovává informace o nejnovějších hodnotu (žádný historie). Informace o aplikaci zobrazíte stav aplikace nebo vaši koncoví uživatelé. Příklad protokolu ve stavu nebo skupiny Oblíbené produktu uživatele.

###### <a name="crash-data"></a>Stav dat

Docházet k chybám data automaticky shromážděná chyby Mobile zapojení SDK sestav aplikace není uskutečněných jednotlivými aplikace. Příklad neošetřené výjimce, který bude proveden.


###### <a name="extra-data"></a>Další data

Události, chyby, aktivity a úlohy je možné vylepšovat s parametry. Toto je další informace, které může vývojář stanovit jako specifická data z aplikace. Co je důležité pro definování jemně odstupňovaná segmentace. 

Například hodnotu "článek" značky vám umožní segment koncovým uživatelům podle kdo zobrazit konkrétní článku. Však, který nemusí být dostatečně. Může být lepší Pokud stejnou značku "článek" obsahuje také informace extra například "news_category" v rámci aktivity. To by být užitečné k určení dynamicky kategorii Oblíbené pro uživatele. 

Další informace o hlásí pár klíč/hodnota. V příkladu pro tuto aplikaci média další informace pro "news_category" budou hodnotu pro danou kategorii. Například "sports", "ekonomiky" nebo "politickém".





#### <a name="tag-and-sdk-integration"></a>Integrace značky a SDK 

Podrobné pokyny pro integraci SDK Azure Mobile zapojení do aplikace postupujte podle dokumentaci [Můžete zapojit SDK integrace](mobile-engagement-windows-store-integrate-engagement.md) na Azure webu. Vyberte cílové platformy na odkazy v horní části této stránky.

Doporučujeme vytváření projektů pro dvěma aplikacemi této technologii postavená Azure Mobile Engagement. Jeden pro vývoj a test pracovní a druhý pro pracovní výroby. Vaše oddělení IT můžete pak převést z pracovní test výroby po úspěšném testování přijetí uživateli.



#### <a name="user-acceptance-testing-uat"></a>Přijetí uživateli testování (UAT)

Testování přijetí uživateli (UAT) se týká jak zajistit, že všechno funguje podle návrhu. Práce toků lze provést a shromáždění všechna požadovaná data podle plánu značku:
 
- Informace o označování by měla být místo podle dokumentovaného AZME koncepty
- Všechny informace, které bude nutné se shromažďují (včetně Další informace o hodnotu aplikace informace)
- Klasifikace odpovídá podle rozložení plánování značky
- Neexistuje žádné duplicitní značky odeslané

Důkladně otestujte všechny typy chování oznámení, které jste vložili v aplikaci

- Oznámení, hlasování, Data posune mimo aplikaci a v aplikaci
- Zobrazení textu nebo Web
- Odznáček aktualizace kategorií



#### <a name="setup"></a>Nastavení

Nastavení Azure Mobile zapojení je velmi jednoduché. V dokumentaci související s uživatelským rozhraním je dostupná na webu Azure Mobile zapojení, [jak se orientovat v uživatelském rozhraní](mobile-engagement-user-interface-home.md).

Doporučujeme, začněte s nastavením správné rolí a členství role pro uživatele projektu. Pomůže vám to spravovat správné přístup k platformu pro všechny uživatele. Vaše role může být:

- Správci
- Vývojáři
- Prohlížeče 

Navíc:
- Zaregistrujte deviceID vyzkoušet na vlastních zařízení.
- Přejděte na nastavení účtu a nastavení časového pásma s grafy a oznámení o doručení čas nastavený pro časové pásmo.
- Přejděte na nastavení aplikace a zaregistrovat "App – informace" potřebujete s koncovým uživatelem cílové dostupné.

Další informace o tom, jak spustit první kampaně nabízená oznámení si, [jak začít používání a správa posune kontaktujte koncovým uživatelům](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Uzavření


Zapojení programy jsou opakované a měli nepřetržitě vylepšíte váš při experimentování s co je nejvhodnější pro aplikace. 

Nejprve při vývoji zkušenosti s zapojení strategie není pokusu o vytvoření efektivní strategii celý globální engagement. Trvat krok za krokem přístup identifikace klíčových ukazatelů výkonu a jak můžete využít je. Zapojení strategie budou jedinečné pro jednotlivé aplikace.

Když jste vytvořili zkušenosti zvažte přidání následující do svých programů můžete zapojit:

- Sledování: Získání uživatelů a pravděpodobně definovat shromažďování dat zdrojů. Shromažďování dat zdrojů jde propojit Azure mobilní Engagement. Umožňuje sledovat výkonů každého zdroje. Tyto informace budou zajímavé maximalizace WIA investice. 

- A a B testování: Toto je důležitou součástí aplikace Engagement. Jednotlivé aplikace má vlastní zvláštnosti. A a B testování, můžete zlepšit aplikaci engagement.

- GEO – umístění: Toto je velký příležitosti značky. Díky tato funkce se dostanete na správném místě a čas. Doporučujeme ověřit, že shromáždíte dost koncových uživatelů chování data před spuštěním použít geo umístění.

- Data nabízených: dat je neviditelné nabízených. Data nabízených umožňuje přizpůsobení aplikace založené na chování koncového uživatele. Například pokud uživatelský segment často požádá maximum Odborný produkty, vlastníkem aplikace můžete poslat nabízených dat, které bude přizpůsobit své domovské stránky s obsahem Odborný maximum.



## <a name="next-steps"></a>Další kroky

- [Vytvořit účet Azure Mobile Engagement](mobile-engagement-create.md).
- Navštivte [Definovat strategie Mobile zapojení](mobile-engagement-define-your-mobile-engagement-strategy.md) zobrazíte další informace o definování strategie Mobile Engagement.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
