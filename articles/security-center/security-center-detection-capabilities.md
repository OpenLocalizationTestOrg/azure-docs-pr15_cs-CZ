<properties
   pageTitle="Funkce rozpoznávání v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu umožňuje vysvětlit, jak fungují funkcím zjišťování Azure Centrum zabezpečení."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Funkce rozpoznávání Centrum zabezpečení Azure
Tento dokument popisuje možnosti rozšířeného vyhledávání Centrum zabezpečení Azure, které vám pomůže identifikovat aktivní hrozeb zaměření na svých prostředcích Microsoft Azure a nabízí další informace potřebné k rychle reagovat.

> [AZURE.NOTE] Rozšířené zjišťování jsou dostupné ve standardní osy z Azure Centrum zabezpečení. Neexistuje bezplatnou zkušební verzi 90 dnů. Můžete upgradovat z výběru ceny osy v [Zásady zabezpečení](security-center-policies.md). Navštivte [Centrum zabezpečení stránky](https://azure.microsoft.com/pricing/details/security-center/) zobrazíte další informace o cenách. 


## <a name="responding-to-todays-threats"></a>Odpověď na aktuální hrozeb
Byly významné změny v orientace na šířku hrozbou, že za poslední let výrazné 20. V minulosti společnosti obvykle jenom dosáhl starat o poškození vzhledu webu útočníků jednotlivé, kteří se stali převážně zájem vidět "co může přehrávají". Dnešní útočníků jsou mnohem víc sofistikované a uspořádat. Mají často stanovených cílů finanční výkazy strategic. Budou mít taky zajištěný další materiály k dispozici, jak mohou být financovány podle státní stavech nebo organizované zločinu.

Tento přístup vedla ke špičkovým jádrem úroveň odbornosti v pořadí útočník. Už se zajímá poškození vzhledu webu. Jsou Teď máte zájem o odcizením informace, účetnictví a soukromé dat – všechno můžete používají generovat finančního na trh otevřít nebo můžete využít konkrétní obchodní, politických nebo vojenské pozici. Ještě víc o než tyto útočníků s finanční cílem je útočníků, kteří porušení sítí k jeho poškození infrastrukturu a lidé.

Odpověď organizace často nasadit různých čárky řešení, která zaměřit na ochrana obvod enterprise nebo koncové body vyhledáním útoku známé podpisy. Tato řešení směřuje ke generování velkého množství zhoršeným věrností upozornění, které vyžadují, analytik zabezpečení a protřídit prozkoumat. Většina organizací chybět znalosti požadované odpovídat na tyto upozornění – tolik přejít unaddressed a čas.  Mezitím útočníků vývojem jejich způsobů podkopat mnoha ochranu založené na podpis a [Přizpůsobit cloudu prostředí](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nové přístupy podporují rychleji identifikovat vznikající hrozeb a urychlení zjišťování a odpověď. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Jak Centrum zabezpečení Azure rozpozná a odpoví na rizika

Microsoft zabezpečení výzkumných jsou neustále pozor hrozeb. Mají přístup k rozsáhlou sadu telemetrie získané od společnosti Microsoft globální stavu a volném čase v cloudu a místní. Tento sahající wide a různorodého kolekce datové sady umožňuje zjišťovat nové útok trendů a charakteristik přes jeho místního příjemce a enterprise produktů, jakož i její online služby společnosti Microsoft. V důsledku toho Centrum zabezpečení můžete rychle aktualizovat jeho rozpoznávání algoritmů jako útočníků uvolněte nové a stále sofistikované zneužití. Tento postup vám pomáhá udržet krok s rychlé přesunutí nebezpečné prostředí. 

Centrum zabezpečení zjišťování hrozbou, že funguje tak, že automaticky shromažďování informací o zabezpečení z Azure prostředků, sítě a připojené partnerských řešení. Analyzuje tyto informace, často korelace informace z různých zdrojů, k identifikaci hrozeb. Výstrahy zabezpečení se přednostně spolu s doporučení ohledně toho, jak nápravě hrozbou, že v Centru zabezpečení.

![Shromažďování dat Centrum zabezpečení a prezentace](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Centrum zabezpečení využívá analytics pokročilým zabezpečením, který úplně dokonalejší postupů založených na podpis. Změnám v velkých dat a [výukových počítače](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologie jsou využít k vyhodnocení události napříč celou cloudu struktury – zjišťování hrozeb, které by bylo možné zjistit ruční přístupy a předpovídání vývoj útoky. Zahrnout tyto analýzy zabezpečení: 

- **Integrované hrozbou intelligence**: vyhledá známé objekty actor chybná využitím globální hrozbou intelligence z produkty společnosti Microsoft a služby, Jednotková služby Microsoft digitální zločinů (DCU), Microsoft zabezpečení odpověď Center (Nejčastější) a externí kanálů.
- **Chování analýzy**: slouží k použití známých vzorků pro Objevte nebezpečného chování. 
- **Zjišťování odchylky**: pomocí statistické vytváření profilů vytváří historických směrný plán. Upozornění na odchylek z založení směrné plány, které odpovídají potenciální útoku.


### <a name="threat-intelligence"></a>Riziko měřítka
Společnost Microsoft nemá velmi významně pomoci počtu globální hrozbou měřítka. Telemetrie toky v z několika zdrojů, jako jsou Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft digitální zločinů jednotky (DCU) a Microsoft zabezpečení odpověď Center (Nejčastější). Výzkumných také dostávat informace intelligence hrozbou, že, který je sdílen mezi poskytovateli služeb hlavní cloudu a přihlásí k kanálů intelligence hrozbou, že od třetích stran. Centrum zabezpečení Azure můžete použít tyto informace upozorňuje na rizika z známé chybná účastníky. Několik příkladů:

- **Odchozí komunikaci škodlivým IP adresu**: odchozí přenosy známé botnet nebo darknet pravděpodobně označuje, že ohroženo zdroj a mohl ho pokusu o spuštění příkazy na tato data systému nebo exfiltrate. Centrum zabezpečení Azure porovná v síti ke společnosti Microsoft globální hrozbou, že databázi a zobrazí upozornění zjistí komunikace škodlivým IP adresu.

## <a name="behavioral-analytics"></a>Chování analýzy

Chování analýzy je postup, který analyzuje a porovnává data do kolekce známé vzorků. Tyto vzorce jsou však není jednoduché podpisy. Jsou určeny prostřednictvím složité počítače výukové algoritmech zaevidují do rozsáhlé datové sady. Budou jsou také určena prostřednictvím opatrní analýzy škodlivým chování expert analytici. Centrum zabezpečení Azure můžete použít chování analýzy identifikovat hostují zdroje založené na analýzy virtuálního počítače protokoly, virtuální sítě zařízení protokoly, struktury protokoly, výpisy a jiných zdrojů. 

Kromě toho je korelace s jiných signály ke kontrole pro podporu důkaz rozšířených kampaně. Tento korelace pomáhá při zjišťování události, které jsou v souladu s založení indikátory narušení zabezpečení. Několik příkladů:

- **Spuštění procesu Suspicious**: útočníků využívat několik technik pro spuštění škodlivým softwarem bez zjišťování. Například mohl může pojmenovat malware stejné jako legitimní systémové soubory, ale tyto soubory umístit do jiného umístění, použijte název, který je velmi podobný benign souboru nebo masky true příponu. Centrum zabezpečení modelů procesů chování a monitory procesu spuštění zjišťování odlehlé hodnoty jako jsou například.  
- **Skryté malwaru a využívání pokusí**: Propracovaného malware je možné obejít tradiční antimalware výrobky podle nikdy psaní na disk nebo šifrování softwarové součásti uložených na disku.  Však takové malware lze zjistit analýzy paměti při malware, ponechte trasování v paměti funkce. Když dojde k chybě software, výpis zaznamenává část paměti v době k chybě.  Analýzou paměti v výpis Centrum zabezpečení Azure dokáže rozpoznat postupy používané k zneužití chyb v software, otvírat důvěrné údaje a tajně přetrvávají se změnami hostují počítač bez vlivu na výkon počítače.
- **Boční pohybu a interní reconnaissance**: uchovávat v nedůvěryhodné síť a vyhledejte/sklizně důležitých dat, útočníků často pokus o přesunutí laterálně z hostují počítače ostatním uživatelům ve stejné síti. Centrum zabezpečení sleduje aktivity obrázku a přihlášení k Seznamte se s neúspěšných pokusech o rozbalte jeho stupátko v síti, například vzdáleného příkazu spuštění sítě zjišťování a výčet účtů.
- **Skriptů Powershellu**: prostředí PowerShell používá útočníků na cílovém virtuálních počítačích pro jiné účely provést škodlivému kódu. Centrum zabezpečení vyhodnotí prostředí PowerShell aktivity důkaz podezřelé aktivity. 
- **Odchozí útoky**: útočníků často obrázku cloudové zdroje s cílem použití tyto materiály k připojení další útoky. Hostují virtuálních počítačích, například asi zvyklí spuštění přímým útokům proti jiných virtuálních počítačích, odesílání nevyžádané pošty nebo prohlížet portů a jiných zařízeních na Internetu. Pomocí výukové počítače v síti, může Centrum zabezpečení zjistit, kdy odchozí síťové komunikace překročit normu. V případě nevyžádané pošty, Centrum zabezpečení také koreluje přenosy neobvyklé e-mailu s intelligence z Office 365 a zjistit, jestli e-mailu je pravděpodobně nefarious nebo výsledek kampaně legitimní e-mailu.  

### <a name="anomaly-detection"></a>Zjišťování odchylky

Centrum zabezpečení Azure také odchylky rozpoznávání používá k identifikaci hrozeb. Na rozdíl chování technologie pro analýzu (který závisí na známé vzorků odvozeno z velkých sad dat), odchylky zjišťování je více "individuálně upravený" a se zaměřuje na standardní hodnoty, které jsou specifické pro nasazení vašeho. Výukové počítač se použije k určení normální aktivity svého nasazení a pak se vytvářejí pravidla definovat odlehlé podmínky, které by mohly představovat událost nastavit jako zabezpečení. Tady je příklad:

- **Příchozí RDP/SSH hrubou vynutit útoky**: nasazení vašeho pravděpodobně zaneprázdněné virtuálních počítačích s velkým množstvím přihlášení každý den a dalších virtuálních počítačích, které mají málo nebo libovolnou přihlášení. Centrum zabezpečení Azure můžete určit aktivitou přihlášení podle směrného plánu pro tyto virtuálních počítačích a používat počítač se naučíte definujte, co je mimo normální přihlášení aktivity. Pokud počet přihlášení nebo čas přihlášení nebo umístění, ze kterého jsou požadovány přihlášení nebo dalších vlastností souvisejících s přihlášení se výrazně liší od směrný plán, může být zpráva upozornění. Výukové počítač znovu Určuje, co je důležité.

## <a name="continuous-threat-intelligence-monitoring"></a>Nepřetržitý hrozbou, že monitorování analytických nástrojů

Centrum zabezpečení Azure funguje zabezpečení zdroje informací a dat pro výzkum družstev, která nepřetržitě sledovat změny hrozbou, že na šířku. Tato volba zahrnuje následující akce:

- **Monitorování analytických nástrojů hrozbou**: hrozbou, že intelligence obsahuje mechanismy, indikátorů KPI, důsledky a akce tipy existující nebo nové hrozeb. Tyto informace se sdílejí v komunitě zabezpečení a Microsoft neustále sleduje kanálů intelligence hrozbou, že z interních a externích zdrojů.
- **Sdílení signálu**: sdílený a analyzovat přehledy z zabezpečení týmy ve společnosti Microsoft obecných portfolia cloudu a místní služby, servery a klient koncový bod zařízení. 
- **Microsoft zabezpečení odborníků**: probíhajících zapojení s týmy ve společnosti Microsoft, které fungují v specializované zabezpečení polí, jako jsou forenzních a webových útok rozpoznávání.
- **Optimalizace zjišťování**: algoritmů se spustí hledání sady dat reálnou zákazníka a zabezpečení výzkumných práce se zákazníky ověřte výsledky. PRAVDA a NEPRAVDA pozitivní slouží k upřesnění algoritmů výukové počítače.

Tyto kombinované úsilí případech v nové a vylepšené zjišťování, které můžete využít výhod okamžitě – neexistuje žádná akce můžete provádět.

## <a name="see-also"></a>Viz taky
V tomto dokumentu dozvěděli jste se do centra zabezpečení Azure zjišťování funkce fungování. Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Centrum zabezpečení Azure plánování a příručka](security-center-planning-and-operations-guide.md)
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md)
- [Upozornění zabezpečení podle typu v Centru zabezpečení Azure](security-center-alerts-type.md)
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Zabezpečení Azure blog](http://blogs.msdn.com/b/azuresecurity/) – najít příspěvky Azure zabezpečení a dodržování předpisů.
