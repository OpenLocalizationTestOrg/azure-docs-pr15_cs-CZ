<properties
   pageTitle="Obnovení havárie a dostupnost pro Azure aplikace | Microsoft Azure"
   description="Technický přehled a hloubková informace o vytváření aplikací pro vysokou obnovení dostupnost a havárie aplikací založený na Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Obnovení havárie a dostupnost pro aplikace založený na Microsoft Azure

##<a name="introduction"></a>Úvod

Tento článek se zaměřuje na dostupnost pro aplikace spuštěné v Azure. Celková strategie pro dostupnost obsahuje taky poměr havárie obnovení. Plánování selhání a havárie v cloudu vyžaduje, abyste rychle rozpoznat k chybám. Potom provádět strategii, která odpovídá vaší odchylky výpadku aplikace. Kromě toho je nutné zvážit rozsah ztrátou dat, které aplikace nevadí vám snadnějšímu nežádoucí firmy důsledky jako se obnoví.

Většina společností Řekněme, že jsou připravené pro dočasné a rozsáhlých k chybám. Však před přijmout tuto otázku pro sebe, má vaše společnost vyzkoušet tyto chyby? Testování obnovení databáze ověřit, jestli že máte správná procesy na místě? Pravděpodobně nejde. Je proto, že úspěšné havárie obnovení začíná plánování a architecting k provedení těchto procesů. Stejně jako mnoho dalších nefunkční požadavky, například zabezpečení dostane havárie obnovení jen zřídka předem analýza a přidělení času, který potřebuje. Většina společností také nemají rozpočtu serverová oblastí s nadbytečné kapacity. V důsledku toho i velice důležité aplikace se často nevynechávají plánování obnovení správné havárie.

Cloud platformy, třeba Azure, a poskytují geograficky rozdělený oblastí ve světě. Tyto platformy taky poskytují možnosti, které podporují dostupnost a s rozmanitými havárie obnovení scénáře. Teď, může být zadán každá velice důležité cloudu aplikace ohled na havárie kontrola pravopisu a mluvnice systému. Azure má odolnost proti chybám a obnovení integrované ku mnoha svých služeb. Musíte zkoumá tyto funkce platformy pečlivě a doplnit strategie aplikace.

Tento článek osnovy kroky potřebné architektura je třeba provést havárie doklad Azure nasazení. Pak můžete používat větší obchodnímu nepřetržitého procesu. Obchodní plán kontinuitu je plán vývoje pokračující provoz nežádoucí podmínek. Může to být selhání technologií, například služby vypnutých nebo přírodní katastrofě, například výpadku bouře nebo power. Odolnost proti chybám aplikace pro havárie je jen podmnožinu procesu obnovení větší havárie popsanou v tomto dokumentu NIST: [Příručka k plánování řešení nepředvídaných událostí pro systémy informačních technologií](https://www.fismacenter.com/sp800-34.pdf).

V následujících částech definovat různé úrovně chyby, metod, jak zacházet s nimi a architektury, které podporují těchto postupů. Tyto informace poskytuje předávat na vstupu havárie obnovení procesů a postupy, který zajistí, že strategie obnovení havárie funguje správně a efektivně.

##<a name="characteristics-of-resilient-cloud-applications"></a>Vlastnosti pružné cloudové aplikace

Aplikace a architekturu můžete nebyly funkce selhání taktické úrovni a ho taky nevadí vám strategic systémové selhání na úrovni oblast. V následujících částech definovat terminologie odkazuje AUTONUM popisuje různé aspekty pružné cloudovým službám.

###<a name="high-availability"></a>Dostupnost

Aplikace vysoce dostupné cloudu používá strategie k zachycení výpadku závislostí, jako jsou spravovaná služeb, které platformu cloudu. Bez ohledu na možné selhání platformu cloudové možnosti umožňuje tento přístup aplikace nadále vykazovat očekávané funkční a nejsou funkční systémové vlastnosti. To je součástí hloubkovou v řada videí Channel 9 [bezporuchový: pokyny pro pružné architektury cloudu](https://channel9.msdn.com/Series/FailSafe).

Při provádění aplikace je třeba uvážit pravděpodobnost výpadku funkce. Zvažte navíc, jaký vliv mají výpadku bude mít v aplikaci z hlediska obchodních před tmavě začte strategie implementaci. Bez termínu úvahu vliv obchodní a pravděpodobnost zasažení podmínka rizika implementace může být drahé a potenciálně nepotřebné.

Zvažte automobilový přiměřeně vysoké dostupnosti. I kvalitu částí a nadřazené inženýrské nezabrání občas k chybám. Až se vaše Auto dostane ploché plášť, například Auto pořád spustí, ale funguje s jejich funkčnost bude omezena funkčnost. Pokud, který jste naplánovali pro tento potenciální výskyt, můžete použít jednu z těchto tenké typ náhradní tires, dokud nebude aktivován opravit pracoviště. Sice náhradní plášť neumožňuje vysoké rychlosti, můžete dál pracovat vozidla dokud nahradit můžete zadat. Podobně do cloudové služby, které plány jednotného zasílání zpráv pro možnou ztrátu funkcí můžete zabránit relativně menší problém uvedení dolů celou aplikaci. To platí to i v případě cloudové služby spuštěním s jejich funkčnost bude omezena funkčnost.

Existuje několik klíčových vlastnosti vysoce dostupné cloudovým službám: dostupnost, škálovatelnost a odolnost proti chybám. I když tyto vlastnosti souvisejí, je důležité pochopit každý, a jak přispívají ke celkové dostupnosti řešení.

###<a name="availability"></a>Dostupnost

Dostupné aplikace považuje dostupnost jeho základní infrastruktura a závislé služby. Dostupné aplikace odebrat jeden body selhání prostřednictvím redundance a pružné návrhu. Když rozšířit rozsah vzít v úvahu dostupnost v Azure je důležité pochopit koncepci efektivní dostupnost platformu. Efektivní dostupnost byly použity smlouvami úroveň služeb (SLA) každý závislá služeb a jejich kumulativní vliv na celkové systémové dostupnosti.

Dostupnost systému je míra procenta z časového intervalu, který budou moct pracovat systém. Dostupnost SLA nejméně dvě instance webu nebo pracovního role v Azure je například 99.95 procent (mimo ze 100 %). Není změřte velikost, se výkonu a funkce služeb spuštěných na těmto rolím. Však efektivní dostupnost cloudové služby je rovněž ovlivněna různých rozsahu závislé služby. Další proměnné části sady více péče, je nutné provést k zajištění použití můžete pružným způsobem požadavkům dostupnost svým koncovým uživatelům.

Zvažte následující rozsahu pro službu Azure, která používá služby Azure: výpočetním databáze SQL Azure a úložišti Azure.

|Služby Azure|SLA   |Potenciální minut prostoje nebo měsících (30 dní)|
|:------------|:-----|:----------------------------------------:|
|Výpočet      |99.95 %|21,6 minut                              |
|Databáze SQL |99,99 %|4.3 minut                              |
|Úložiště      |99.90 %|43,2 minut                              |

Je nutné naplánovat pro všechny služby přejdete potenciálně ve stejnou dobu. V tomto příkladu zjednodušené je celkový počet minut za měsíc, který by mohl být aplikace dolů 108 minut. 30 dnech měsíc má celkem 43,200 minut. 108 minut je.25 procento celkového počtu minut za měsíc 30 dnech (43,200 minut). To vám dá efektivní dostupnost 99.75 procent ke cloudové službě.

Však pomocí dostupnost technik popsaných v tomto dokumentu můžete vylepšit to. Třeba při návrhu aplikace pokračujte, když není dostupná služba SQL databáze, můžete odebrat, který z rovnice. Může to znamenat, že spuštění aplikace s omezenou funkce, tak, aby byly taky firmy požadavky k zamyšlení. Úplný seznam Azure rozsahu najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Škálovatelnost:

Škálovatelnost ovlivní přímo k dispozici. Aplikace, která se nezdaří lepší zatížení už není dostupná. Scalable aplikace budou moct zahájit lepší služba s konzistentní výsledky ve windows přijatelné čas. Po scalable systém zmenší vodorovně nebo svisle ke správě zvýšení načítání a zachovat konzistentní výkonu. Základní řečeno změna měřítka vodorovné přidá další počítačích stejné velikosti (procesor paměti a šířky pásma), při svislé měřítka zvýšení velikost existující počítačích. Pro Azure musíte svislé možnosti měřítka pro výběr různých velikostech počítače pro výpočetním. Však změně velikosti počítač vyžaduje opětovné nasazení. Flexibilní řešení proto jsou sice pro vodorovné měřítko. Totiž především pro výpočetním, můžete snadno zvýšení počtu spuštěné instance webu nebo pracovního role. Tyto další instance zpracovat lepší přenosů Azure webového portálu, skriptů Powershellu nebo kód. Základní toto rozhodnutí pro zvýšení konkrétní sledované metriky. V tomto scénáři výkon uživatelského nebo metriky není dojít k výrazná rozevírací zatížení. Obvykle role pro web a pracovní obsahují libovolné stavu externě. Díky pro vyrovnávání zatížení flexibilní a bezproblémové zpracování hodnot změn počty instance. Změna měřítka vodorovné také dobře spolupracuje služby, jako je Azure úložiště, které nenabízejí vrstvené možnosti pro změnu velikosti svisle.

Cloudu nasazení by měly považovat za kolekce jednotky měřítka. Díky aplikaci být pružná v údržbu výkon potřeb koncoví uživatelé. Jednotky měřítka budou snadněji vizualizace na úrovni serveru pro web a aplikace. Je to proto Azure již obsahuje příslušnosti výpočetním uzly prostřednictvím webu a pracovní rolí. Přidání že více výpočetního jednotkách nasazení nezpůsobí všechny aplikace stavu správy straně efekty, protože jsou příslušnosti výpočetním jednotkách. Jednotkami měřítko úložiště je zodpovědný za správu oddíl dat (strukturovaná a nestrukturovaná). Příklady úložiště jednotkách: tabulky Azure oddílu, kontejneru objektů Blob Azure a databáze SQL Azure. I použití více účtů úložišti Azure má přímé vliv na škálovatelnost aplikace. Je třeba navrhnout vysoce scalable Cloudová služba začlenit víc úložiště měřítko jednotkami. Pokud aplikace používá relačních dat, oddílů napříč několika SQL databáze data. To umožňuje úložiště zachovávat modelu jednotku měřítko pružná výpočetním. Podobně úložišti Azure umožňuje dat schématy, které vyžadují záměrné návrhů potřebám výkon pro využití vrstvě oddílů. Seznam Doporučené postupy pro navrhování scalable cloudovým službám najdete v tématu [Doporučené postupy pro návrh ve velkém měřítku služby na Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Odolnost proti chybám

Aplikace by se předpokládá, že každé funkce závislá cloudu můžete a budou přesměrovány nastane okamžik v čase. Aplikace odolnost proti chybám rozpozná a manévry okolo selhalo prvků pokračovat a vrátit správné výsledky v rámci konkrétní časový rámec. Pro přechodná chybové podmínky bude odolnost proti chybám systém využívat zásadu opakovat. Pro více vážně chyb aplikace zjistit problémy a převzít alternativní hardware nebo plán řešení nepředvídaných událostí, dokud opravit chybu. Spolehlivé aplikace správně můžete spravovat selhání jednu nebo více částí a pokračovat správně funguje. Odolnost proti chybám aplikace můžete použít jednu nebo více Návrh strategie, například redundance, replikace nebo jejich funkčnost bude omezena funkčnost.

##<a name="disaster-recovery"></a>Obnovení havárie

Nasazení cloudu můžou přestat fungovat kvůli systémové výpadku závislá služeb nebo podkladového infrastruktury. Za těchto podmínek je podnikatelský plán kontinuitu spustí proces obnovení havárie. Tenhle proces zahrnuje obvykle operace zaměstnanců a automatické postupy pro opětovnou aktivaci aplikace v oblasti k dispozici. Při této akci musí přepojit uživatelé aplikace, dat a služeb na novou oblast. Zahrnuje také použití zálohování médií nebo probíhající replikace.

Zvažte předchozí přiměřeně, která porovnávat dostupnost na možnost obnovit z ploché plášť použitím náhradní. Obnovení havárie naopak zahrnuje kroky po zhroucení auta, kde je auto už provozní. V takovém případě nejlepším řešením je najít efektivně změnit auta, tak, že zavoláte cestovní služby nebo přítelem. V tomto scénáři tam pravděpodobně bude delší zpoždění při otevírání zpátky na cestách. Je také složitější oprava a návratu do původního vozidla. Obnovení havárie do jiné oblasti stejným způsobem, je složitou úlohu, která obvykle zahrnuje některé výpadek služeb a možnou ztrátu dat. Půjde vám ještě snadněji chápat a vyhodnocení strategie obnovení havárie, je třeba definovat dvěma podmínkami: obnovení čas cíle (RTO) a obnovení odkazovat cíle (operace RPO).

###<a name="recovery-time-objective"></a>Obnovení čas cíle

RTO je maximální počet minut přiřazených obnovení funkcí aplikace. To je založena na obchodní požadavky a související s důležitost aplikace. Klíčové podnikové aplikace vyžadují zhoršeným RTO.

###<a name="recovery-point-objective"></a>Obnovení čárky cíle

Operace RPO je přijatelné časového intervalu ke ztrátě dat z důvodu procesu obnovení. Například-li operace RPO hodinu, musíte úplně obecnějším údajům nebo replikovat data aspoň každou hodinu. Po vyvoláte aplikace v alternativní oblasti záložních dat může chybět až jednu hodinu data. Jako RTO zacílit důležitých aplikací množství menší operace RPO.

##<a name="checklist"></a>Kontrolní seznam

Pojďme sumarizovat klíčové body, které byly uvedené v tomto článku (a jeho související články na [dostupnost](./resiliency-high-availability-azure-applications.md) a [obnovení havárie](./resiliency-disaster-recovery-azure-applications.md) Azure aplikací). Tento souhrn bude sloužit jako kontrolní seznam položek, které byste měli zvážit pro vlastní dostupnost havárie plánování a obnovení. Toto jsou doporučené postupy, které byly užitečný pro zákazníky, kteří potřebují vážně o implementaci úspěšné řešení.

1. Vedení rizik pro každou aplikaci, protože každý může mít odlišné požadavky. Některé aplikace je považován za kritický více než ostatní a by zarovnání zvyšovat náklady na architektonické havárie obnovení.
1. Tyto informace použijte k určení RTO a operace RPO pro každou aplikaci.
1. Návrh nepovede, počínaje architektura aplikací.
1. Implementace doporučené postupy pro dostupnost, při vyrovnávání náklady, složitostí i rizika.
1. Implementace havárie obnovení plány a procesů.
  * Zvažte chyby, které zahrnují úroveň modulu až výpadku dokončení cloudu.
  * Vytvoření záložní strategie pro všechny odkazy a transakční data.
  * Zvolte architektura obnovení havárie více webu.
1. Definování určitého vlastníka pro havárie obnovení procesů automatizace a testování. Vlastník by měl Správa a vlastní celého procesu.
1. Procesy tak, aby byly snadno opakující dokumentu Sice jednoho vlastníka víc lidem by měla pochopit a sledovat procesy v případě nebezpečí.
1. Školení zaměstnanců implementovat procesu.
1. Běžná havárie simulace určenou školení a ověření procesu.

##<a name="summary"></a>Souhrn

Když hardwaru nebo aplikací v Azure, postupů a strategie pro správu je se liší od kdy dojde k chybě v místní sítě. Hlavní důvodem je, že cloudové řešení obvykle další závislostí na infrastrukturu, která je rozvržena Azure oblast a spravovat jako jednotlivé služby. Nutné zpracovat částečně neúspěšné technik dostupnost. Ke správě přísnější chyby, případně kvůli událost nastavit jako havárie, použijte strategie obnovení havárie.

Azure rozpozná a úchyty pro mnoho chyb, ale mnoho typů chyb, které vyžadují strategie specifické pro aplikaci. Musíte aktivně připravit a správa selhání aplikací, službami a data.

Při vytváření aplikace dostupnost a havárie obnovení plán, zvažte důsledky firmy selhání aplikace. Definování procesy, zásady a postupy pro po kritické události trvá obnovit kritické systémů plánování a potvrzení. A po vytvoření plány nelze zabránit tam. Musí pravidelně analyzovat, otestujte a neustále zlepšit plány podle portfolia aplikací, obchodních potřeb a technologie k dispozici. Azure poskytuje nové funkce a vyvolá nové úkoly k vytváření robustních aplikací, které nebyly k chybám.

##<a name="additional-resources"></a>Další zdroje informací

[Dostupnost pro aplikace založený na Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Obnovení pro aplikace založený na Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Azure odolnost proti chybám příručku](resiliency-technical-guidance.md)

[Přehled: V cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md)

[Vysoký dostupnost a havárie obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Bezporuchový: Pokyny pro architektury pružné cloudu](https://channel9.msdn.com/Series/FailSafe)

[Doporučené postupy pro návrh rozsáhlé služby Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady článků věnovaných havárie využití a dostupnost pro Azure aplikace. Další článek v této řadě je [dostupnost pro aplikace založený na Microsoft Azure](resiliency-high-availability-azure-applications.md).
