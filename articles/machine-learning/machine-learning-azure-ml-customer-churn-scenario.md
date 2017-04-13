<properties
    pageTitle="Analýza zákazníka Churn pomocí počítače výukové | Microsoft Azure"
    description="Případová studie vývoje integrované modelu doplňku pro analýzu a bodování konve zákazníka"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analýza Churn zákazníka pomocí výukové počítače Azure

##<a name="overview"></a>Základní informace
Toto téma uvádí odkaz provádění projektu analýzy konve zákazníků, která je integrovaná pomocí Azure počítače výukové Studio. Tento článek popisuje přidružené obecný modely komplexně řešení tohoto problému konve průmyslové zákazníka. Jsme také změřit přesnost modely, vytvořené pomocí počítače učení a vyhodnocení pokyny pro další vývoj.  

### <a name="acknowledgements"></a>Potvrzení

Tento experiment byl vývoji a testování Serge Berger, vědecký pracovník dat jistiny v Microsoft a Roger Barga dřív produktu správce pro Microsoft Azure počítače Learning. Tým Azure si přečtěte následující dokumentaci Děkujeme za jejich potvrdí své znalosti a poděkováním pro sdílení tento dokument white paper.

>[AZURE.NOTE] Data použitá pro tento pokusy není veřejně dostupný. Příklad k vytváření modelu počítače výukové pro analýzu konve naleznete v tématu: [Telco churn modelu šablony](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) v [Galerii Intelligence Cortana](http://gallery.cortanaintelligence.com/)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Problém konve zákazníka
Firmy na trhu příjemce a ve všech oblastech organizace mají konve. Někdy konve je nadbytečné a ovlivňuje rozhodnutí zásad. Tradiční řešením je předpovídání maximum přestavující churners a adresa svým potřebám prostřednictvím služby concierge marketingových kampaní, nebo použít zvláštní výjimky. Těchto postupů se může lišit od odvětví odvětví a dokonce i z určitého spotř obrázku do druhého v rámci jedné odvětví (například telekomunikačních).

Běžné faktor je, že firmy minimalizovat tyto zvláštní zákazníkům uchovávání informací. Tedy natural metodologie bude skóre každé zákazníka s pravděpodobností konve a vyvraťte horních N z nich. Hlavní zákazníci může být nejvyšším ziskem z nich; například v složitější scénáře, je funkce zisk zaměstnán při výběru kandidáty pro zvláštní výjimku. Tyto další skutečnosti jsou ale jenom část jedné komplexní strategie pro práci s konve. Firmy taky muset udělat do rizika účtu (a odchylky přidružené rizika) úroveň a náklady na zásah a segmentace pravděpodobné zákazníka.  

##<a name="industry-outlook-and-approaches"></a>Outlook odvětví a přístupy
Sofistikované zpracování konve je znak zdokonaleným partnery. V klasickém příkladu je odvětví telekomunikačních kde předplatitele je známo, často přejít z jednoho poskytovatele na jiný. Tento nepovinného konve je přednostní významný. Navíc poskytovatelů mít sečtenou za platnou znalostí o *churn ovladače*, které jsou faktory, které zákazníci jednotka přejdete.

Například sluchátko nebo zařízení volba je známý ovladač konve podniku mobilního telefonu. Oblíbené zásada jako výsledek je subvencovat cena sluchátko pro nové účastníky a nabíjení plnou cenu do stávajících zákazníků pro upgrade. Z historických důvodů tuto zásadu vedla zákazníkům vše z jednoho poskytovatele do jiného získat novou slevu, která zároveň má výzva poskytovatelů upřesnit jejich strategie.

Vysoký pohyblivostí nabídky sluchátko je faktor velmi rychle zruší platnost modely konve, které jsou založeny na aktuální sluchátko modely. Kromě toho mobilních telefonů nejsou pouze telekomunikačních systémů v plánech zařízení. ty představují taky příkazy způsobem (zvažte iPhone) a tyto sociální prognostické jsou mimo rozsah běžná telekomunikačních datové sady.

Čistý výsledek pro modelování je, že nelze navrhnout zásadu zvuk jednoduše odstraněním známé důvody konve. Nepřetržitý modelování strategie, včetně klasické modelů, které podpořte kategorií proměnné (například stromy rozhodnutí), ve skutečnosti je **povinný**.

Používání velkých sad dat na zákazníkům, organizace jsou při provádění analýzy velký dat (zejména konve zjišťování založených na datech velký) jako efektivní přístup k problému. Můžete najít další informace o velký dat přístup k problému konve v doporučení ohledně toho ETL oddíl.  

##<a name="methodology-to-model-customer-churn"></a>Metodologie pro zákazníka konve modelu
Běžných potíží procesu řešení zákazníka konve je znázorněn v čísla 1 až 3:  

1.  Model rizika umožňuje trocha zamyšlení nad prezentacemi vliv akce pravděpodobnosti a rizika.
2.  Model zásah trocha zamyšlení nad prezentacemi úrovně zásah může vliv pravděpodobnost konve a množství zákazníků umožňuje životnost hodnotu (CLV).
3.  Analýza různě kvalitativní analýze, který je eskalována aktivní marketingovou kampaní, který se zaměřuje segmenty zákazníků předvádění optimální nabídky.  

![][1]

Tento vpřed vypadající přístup je nejlepší způsob, jak zacházet s konve, ale je součástí složitost: máme se dají více modelech archetype a sledování závislosti mezi modely. Interakce mezi modely můžete formát, jak je znázorněno na následujícím obrázku:  

![][2]

*Obrázek 4: Jednotné více modelech archetype*  

Interakce mezi modely je zkratka Pokud jsme předvádění jedné komplexní přístup odběratele uchovávání informací. Každý model nutně snižuje v čase; Architektura tedy implicitní smyčka (podobný archetype nastavil OSTRÉ DM standardní dolování dat, [***3***]).  

Celková cyklus rizika rozhodnutí marketing segmentace/rozložené je pořád generalized strukturu, která platí pro mnoho obchodních problémů. Analýza konve je jednoduše silných zástupce této skupiny problémy, protože vykazuje všechny znaky složité obchodní problém, který neumožňuje zjednodušené prediktivní řešení. Sociální aspekty moderní přístup k churn nejsou se zvýrazněnými zejména přístup, ale sociální aspekty jsou formát v archetype modelování, jako by byly v libovolné modelu.  

Tady zajímavé sčítání je velký dat analýzy. Maloobchodě a telekomunikačních systémů v plánech aktuálnímu shromažďovat vyčerpávající data o svých zákazníků a jsme můžete snadno stanovit, že je nutné více modelech připojení nepůjde běžné trendu zadaných vznikající trendů jako je Internet věcí a všudypřítomná zařízení, které umožňují firmy využívat inteligentní řešení na více vrstev.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Implementace archetype modelování ve počítače výukové Studio
Možnost potíže jenom popsané, co je nejlepší způsob, jak implementovat integrované modelování a bodování přístup? V této části jsme ukážeme, jak jsme provést to pomocí Azure počítače výukové Studio.  

Více modelech přístup je vyžadováno při návrhu globální archetype pro konve. I bodování (prediktivní) části přístupu by měl být více modelu.  

Na následujícím obrázku vidíte, že jsme vytvořili, který využívá čtyři bodování algoritmů ve počítače výukové studiu odhadnout konve prototypů. Důvody použití více modelech přístupu není pouze k vytvoření celek třídění ke zvýšení přesnosti, ale taky chránit před navýšení přizpůsobení a zlepšit výběr obvyklé funkcí.  

![][3]

*Obrázek 5: Prototypů konve modelování přístup*  

V následujících částech jsou podrobněji popsány prototypů bodování model, který jsme implementovaná pomocí Studio výukové počítače.  

###<a name="data-selection-and-preparation"></a>Výběr dat a příprava
Data slouží k vytvoření modelů a zákazníci skóre získaný z svislé řešení CRM, s daty zakódovány chránit osobní údaje zákazníka. Importovaná data obsahují informace o 8000 předplatných ve Spojených státech a sloučí tři zdroje: zřizování dat (předplatné metadata), údaje o aktivitě (použití systému) a Zákaznická podpora data. Data neobsahuje žádné firmy související informace o zákaznících; neobsahuje například věrnostní metadata nebo kreditní výsledků.  

Pro zjednodušení ETL a čištění procesy jsou mimo rozsah vzhledem k tomu, že už má Příprava dat uděláno jinde.   

Výběr funkcí pro modelování vychází z předběžné významnost bodování množiny prognostické součástí proces, který používá modulu náhodné doménové. K provedení ve počítače výukové studiu jsme vypočítá průměr, střední a oblasti zástupce funkcí. Například jsme přidali agregace kvalitativní dat, jako jsou minimální a maximální hodnoty pro činnosti uživatelů.    

Jsme také zachytit časový informace za posledních šest měsíců. Jsme analýzy dat pro jeden rok a jsme zřídit, i kdyby statistické významné trendů, neovlivní konve výrazně dojde ke snížení šest měsíců.  

Nejdůležitější bod je, aby byl celého procesu, včetně ETL, výběr funkcí a modelování implementovaná ve počítače výukové studiu použití zdrojů dat v Microsoft Azure.   

Následující obrázky znázorňují data, která byla použita.  

![][4]

*Obrázek 6: Úryvek zdroje dat (zakódovány)*  

![][5]


*Obrázek 7: Funkce extrahuje ze zdroje dat*
 
> Poznámka: Tato data jsou soukromé proto nejde sdílet modelu a data.
> Však podobné modelu pomocí veřejně dostupný dat, najdete v tomto příkladu experimentovat v [Galerii Intelligence Cortana](http://gallery.cortanaintelligence.com/): [Churn Telco zákazníka](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Další informace o jak implementovat modelu analýzy konve pomocí Cortana Intelligence sadu, doporučujeme taky [v tomto videu](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) po Tok Hyong západní vyšší Program manažer. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algoritmech použitých v prototypů

Následující čtyři algoritmy výukové počítače jsme použili k vytváření prototypů (bez přizpůsobení):  

1.  Analytický nástroj Regrese logistickou (LR)
2.  Zesílen rozhodovacího stromu (BT)
3.  Průměrné perceptron (Onenotových)
4.  Podpora vektorové počítače (SVM)  


Následující obrázek znázorňuje část návrhovou plochu testu, které určuje pořadí, ve které jsou vytvořené modely:  

![][6]  


*Obrázek 8: Vytváření modelů ve počítače výukové Studio*  

###<a name="scoring-methods"></a>Hodnocení metody
Čtyři modely jsme dosáhne pomocí s popisky školení datovou sadu.  

Jsme také odeslaný bodování datovou sadu k srovnatelného vytvořené pomocí desktopové verzi přidružení zabezpečení Enterprise analýzy 12. Jsme měří přesnost modelu přidružení zabezpečení a všechny čtyři Modely Studio výukové počítače.  

##<a name="results"></a>Výsledky
V této části jsme prezentovat naše zjištění o přesnosti modely podle hodnocení datovou sadu.  

###<a name="accuracy-and-precision-of-scoring"></a>Přesnost a s přesností bodování
Obecně vzato implementaci Azure počítač přečíst je za přidružení zabezpečení v přesnost o 10 až 15 % (oblasti v části křivky nebo AUC).  

Nejdůležitější míru v konve je však chybnou sazba: tedy z churners horních N jako předpovídané pomocí třídění, který z nich skutečně nebyla **** konve a ještě dostali zvláštní pozornost? Následující diagram ve srovnání tento kurz chybnou pro všechny modely:  

![][7]


*Obrázek 9: Passau prototypů oblasti v části křivky*

###<a name="using-auc-to-compare-results"></a>Použití AUC a umožňuje porovnávat výsledků
Oblast v části křivky (AUC) je metriky představující globální míra *separability* mezi distribuce skóre pro kladné a záporné populace. Je podobný tradiční grafu sluchátko operátor vlastnostmi (ROC), ale jeden důležité rozdíly je, že míru AUC nevyžaduje zvolit mezní hodnota. Místo toho vytvořil souhrn výsledků přes **všechny** možností. Naopak tradiční ROC graf zobrazuje kladné sazbu na svislé ose a falešně pozitivní sazba na vodorovné ose a mezní klasifikace se liší.   

AUC se obecně používá jako míra jmění pro různé algoritmy (nebo jiné systémy) protože umožňuje modely porovnat prostřednictvím jejich AUC hodnoty. Toto je Oblíbené přístup v odvětví například meteorologie a biosciences. Proto AUC představuje Oblíbené nástroj pro vyhodnocování třídění výkonu.  

###<a name="comparing-misclassification-rates"></a>Porovnání chybnou sazeb
Jsme porovnání chybnou sazby pro datovou sadu předmětné pomocí data CRM přibližně 8000 předplatných.  

-   Sazba chybnou přidružení zabezpečení byla 10 15 %.
-   Sazba chybnou počítače výukové Studio byla 15-20 % churners horní 200 300.  

V odvětví telekomunikačních je důležité, který bude řešit jenom zákazníky, kteří mají nejvyšší riziko churn tím, že je nabízí službu concierge nebo jiných zvláštní pozornost. V tomto směru dosahuje implementaci počítače výukové Studio výsledky obsahuje modelu přidružení zabezpečení.  

Ze stejného důvodu přesnost totiž důležitější než přesnost jsme zajímají převážně správně klasifikaci potenciální churners.  

Následující obrázek z Wikipedie znázorňuje relace v oživení a snadno vysvětlení obrázku:  

![][8]

*Obrázek 10: Vytvářenou přesnost*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Přesnost výsledky zesílen rozhodovací strom modelu  

V následující tabulce zobrazí jako nezpracovaná výsledky bodování pomocí počítače výukové prototypů pro zesílen rozhodovací strom modelu a se stane s být nejpřesnější mezi čtyři modely:  

![][9]

*Obrázek 11: Zesílen rozhodovací strom modelu vlastnosti*

##<a name="performance-comparison"></a>Porovnání výkonu
Porovnáním rychlost niž byl dat za použití modelů Studio výukové počítače a srovnatelného vytvořené v desktopové verzi 12.1 analýzy Enterprise přidružení zabezpečení.  

Následující tabulka shrnuje výkonu algoritmech:  

*Tabulka 1. Obecné výkon (přesnost) algoritmů*

| LR|BT|PŘÍSTUPOVÝ BOD|SVM|
|---|---|---|---|
|Průměrná modelu|Nejlepší modelu|Nevedou podle očekávání|Průměrná modelu|

Modely použitý ve počítače výukové Studio outperformed přidružení zabezpečení o 15 25 % rychlosti spuštění, ale přesnost byla na nom_hodnota.  

##<a name="discussion-and-recommendations"></a>Diskuse a doporučení
V odvětví telekomunikačních několika postupy vznikly a analyzujte data konve, včetně:  

-   Jsou odvozeny metriky pro čtyři základní kategorie:
    -   **Entita (třeba jste si předplatné)**. Zřízení základní informace o předplatné a/nebo zákazníka, které jsou předmětem konve.
    -   **Aktivity**. Získá všechny možné použití, který souvisí s entitu, například počet přihlášení.
    -   **Zákaznická podpora**. Sklizně informace z Zákaznická podpora protokoly označuje, zda bylo předplatné problémy nebo interakce s Zákaznická podpora.
    -   **Competitive a obchodní data**. Získání možné informací o zákazníkovi (například může být k dispozici nebo těžko sledovat).
-   Použití význam pro výběr funkcí jednotky. To znamená, model stromu zesílen rozhodnutí je vždy slíbíte něco přístup.  

Pomocí těchto čtyř kategorií vytvoří dojem, který jednoduché *deterministický* přístupu na základě indexů formátovaná na rozumné faktory podle kategorií, by měly postačovat k identifikaci zákazníků riziko pro konve. Bohužel i když tento pojem zdá pravděpodobné, je NEPRAVDA principy. Důvod, proč je konve je efekt časový a faktory, které přispívají k churn se většinou používá pro přechodná v USA. Co vede zákazníkovi zvážit ponechání dnes se můžou lišit zítra a určitě bude různých šest měsíců od tohoto okamžiku. Proto *pravděpodobnosti* modelu je nutné provádět.  

Tato důležitá pozorování často zapomíná v podniku, který obecně dává přednost business intelligence určený přístup analýzy, převážně, protože je jednodušší prodej a automatizaci jednoduchých přijímá.  

Příslibem samoobslužných funkcí analýzy pomocí počítače výukové Studio je však čtyři druhy informace stupněm dělení nebo oddělení, budou zdroj cenné pro počítače víc o konve.  

Další zajímavé funkce chodit Azure počítač přečíst není možnost přidat vlastní modul do úložiště předdefinované modulů kontroly, které jsou k dispozici. Tato možnost vytvoří v podstatě příležitost k výběru knihoven a vytvoření šablon pro vertikální skupiny. Je důležité výhodu oproti konkurenci učení Azure počítači na místě market.  

Jsme ať v budoucnu, pokračujte v tomto tématu, zejména související s velký dat analýzy.
  
##<a name="conclusion"></a>Uzavření
Tento dokument popisuje smysluplné přístup k řešení běžných problémů zákazníků konve pomocí obecný rámec. Považuje za vzor pro bodování modely jsme implementovaná pomocí výukové počítače Azure. Nakonec jsme posouzena přesnost a výkon řešení prototypů pokud jde o srovnatelná algoritmy přidružení zabezpečení.  

**Další informace:**  

Tento dokument můžete? Zkontrolujte sdělte svůj názor. Sdělte nám v měřítku 1 (špatné) až 5 (pracovníků), jak by se ohodnocení tohoto dokumentu a proč jste udělili ho toto hodnocení? Příklad:  

-   Jsou můžete hodnocení ho vysoké kvůli máte dobré příklady, pracovníků obrazovek, zrušte zaškrtnutí políčka zápisu nebo jiný důvod, proč?
-   Jsou můžete hodnocení se zhoršeným kvůli špatné příklady, rozmazaný obrazovek nebo nejasné psaní?  

Tuto zpětnou vazbu bude Pomozte nám zlepšit kvalitu dokumenty white paper, kterou jsme uvolněte.   

[Odeslání názoru](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Odkazy
Prognostické analýzy [1] : za hranice předpovědí západní McKnight správy informací, červenec/srpnu 2011 p.18 – 20.  

Článek z wikipedie obsahuje [2] : [Přesnost](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [OSTRÉ DM 1.0: Příručka pro dolování podrobných dat](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Marketing velký dat: zapojit zákazníky efektivnější a řízení hodnotu](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco churn modelu šablony](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) v [Galerii Intelligence Cortana](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Dodatek

![][10]

*Obrázek 12: Snímek v prezentaci na konve prototypů*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
