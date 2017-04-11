<properties 
    pageTitle="Žádost o jednotek DocumentDB | Microsoft Azure" 
    description="Informace o tom, jak pochopit, zadejte a odhadu žádost o jednotku požadavky DocumentDB." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Žádost o jednotek DocumentDB
Teď k dispozici: DocumentDB [žádost o jednotku kalkulačky](https://www.documentdb.com/capacityplanner). Další informace najdete v [Estimating potřebuje vaše výkon](documentdb-request-units.md#estimating-throughput-needs).

![Kalkulačka výkon][5]

##<a name="introduction"></a>Úvod
Tento článek obsahuje přehled zboží žádost v [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Po přečtení v tomto článku si budete moct odpovězte na následující otázky:  

-   Co jsou požádat o jednotky a požádat o náklady?
-   Jak zadat požadavek jednotku kapacita pro kolekci?
-   Jak odhadu potřeb Moje aplikace žádost o jednotku?
-   Co se stane, když překročit žádost o jednotková kapacitu kolekce?


##<a name="request-units-and-request-charges"></a>Žádost o jednotky a požádejte Rozpis poplatků
DocumentDB nabízí rychlé a předvídatelná výkon *rezervaci* prostředky ke splnění potřeb vaší aplikace výkon.  Protože aplikace načíst a přístup k vzorků změn v průběhu času, DocumentDB umožňuje snadno zvýšení nebo snížení počtu vyhrazené výkon k dispozici pro aplikace.

S DocumentDB není zadán rezervovaná výkon z hlediska jednotky žádost zpracuje za sekundu.  Žádost o jednotek můžete představit jako výkon měny, kterým *Rezervovat* an, chyby množství zaručené žádost o jednotky zpřístupňuje aplikacím na základě za druhé.  Každou operaci v DocumentDB - psaní dokumentu při provádění dotazu, upravujete dokument – spotřebovává procesorů, procesoru a paměti.  To znamená každé operace poněkud na *žádost o poplatků*, vyjádřené v *žádosti o jednotky*.  Principy faktory, které ovlivňuje poplatky za jednotku žádost, spolu s požadavky na výkon aplikace, umožňuje spuštění aplikace jako náklady efektivně co nejjednodušší. 

##<a name="specifying-request-unit-capacity"></a>Určení žádost o jednotku doby
Při vytváření kolekce DocumentDB, zadat počet jednotek žádost o sekundu (RUs) se má rezervovaný kolekce.  Po vytvoření kolekci úplné rozdělení RUs zadané vyhrazená pro použití v kolekci.  Jednotlivé kolekce je zaručit, máte vyhrazená a samostatný charakteristik výkon.  

Je důležité mít na paměti, že DocumentDB pracuje na modelu rezervace; To znamená jsou fakturované množství výkon *vyhrazená* pro odběr, bez ohledu na to, kolik tento výkon je aktivně *používá*.  Mějte na paměti, ale, který jako vaše aplikace zatížení, data a použití vzorků změny, které můžete snadno změnit velikost nahoru nebo dolů množství vyhrazená RUs prostřednictvím DocumentDB SDK nebo pomocí [Portálu Azure](https://portal.azure.com).  Další informace k měřítko výkon nahoru a dolů najdete v článku [DocumentDB výkonu úrovně](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Žádost o aspektech jednotky
Stahují zboží žádost o rezervujte pro kolekci DocumentDB, je třeba vzít v úvahu následující proměnné:

- **Velikost dokumentu**. Jak velikost dokumentu rozšiřte jednotky spotřebované množství načtení nebo zápisu dat, že data také zvětšíte.
- **Počet vlastností dokumentu**. V případě výchozí indexování všech vlastností, jednotky spotřebované množství napsat dokument zvětšíte jako zvyšuje počet vlastnost.
- **Konzistence dat**. Pokud chcete použít data konzistence úrovně silné nebo ohraničených Staleness, další jednotky bude potřeba ke čtení dokumentů.
- **Vlastnosti indexovat**. Zásady indexu u každé kolekce určuje vlastnosti, které jsou ve výchozím nastavení indexováno. Žádost o Jednotková spotřeba můžete zmenšit omezení počtu indexované vlastnosti nebo povolení pustí indexování.
- **Indexování dokumentu**. Ve výchozím nastavení automaticky indexováno každý dokument bude využívat méně jednotky žádost o, pokud se rozhodnete indexovat některé dokumenty.
- **Vzorky dotazu**. Kolik požádat o jednotky jsou pro operaci spotřebované množství má vliv složitost dotazu. Počet predikáty, druh predikáty, průzkumy, počet funkce definované uživatelem a velikost zdroj uvedenou množinu dat všechny ovlivnit nákladů dotazu.
- **Použití skriptu**.  S dotazy, uložené procedury a aktivace zpracovávat žádosti o jednotky podle složitost provádí operace. Při přípravě aplikace zkontrolujte hlavičky poplatků a snadněji pochopit, jak každé operace spotřebovává žádost o jednotku doby.

##<a name="estimating-throughput-needs"></a>Odhad potřeb výkon
Jednotka žádost o vyjadřuje normalizovanou vyžádání náklady. Jednotka jeden požadavek představuje kapacitu zpracování potřebné ke čtení (přes sebe odkaz nebo id) jeden dokument JSON 1KB tvořené 10 jedinečné nemovitostí s hodnotou (s výjimkou vlastnosti systému). Žádost o vytvoření (vložení), nahrazení nebo odstranění do stejného dokumentu bude používat více procesorů ze služby a tím další žádosti.   

> [AZURE.NOTE] Směrný plán 1 žádost jednotky pro dokument 1KB odpovídá jednoduché získat vlastní odkaz nebo id dokumentu.

###<a name="use-the-request-unit-calculator"></a>Pomocí kalkulačky jednotku žádost
Pomůže zákazníci pořádku ladění jejich odhady výkon, není webovou aplikaci [žádost o jednotku kalkulačky](https://www.documentdb.com/capacityplanner) k odhadu požadavky na žádost o jednotku pro typické operace, včetně:

- Dokument vytvoří (zápisy)
- Čtení dokumentu
- Odstranění dokumentů
- Aktualizace dokumentu

Nástroj taky podporuje odhad úložiště potřeby v oblasti dat podle ukázkové dokumenty, které zadáte.

Pomocí nástroje je jednoduchá:

1. Nahrání jednoho nebo více zástupce JSON dokumentů.

    ![Ukládání dokumentů do kalkulačky jednotku žádost][2]

2. K odhadu požadavky na ukládání dat, zadejte počet dokumentů, které se bude ukládat.

3. Zadejte počet dokumentů vytvořit, číst, aktualizovat a odstraňovat operace požadujete (na základě rychlostí). K odhadu v žádosti o jednotku poplatcích operace aktualizace dokumentu, uložte kopii dokumentu ukázka z kroku 1, obsahuje aktualizace typické pole.  Například pokud aktualizace dokumentu obvykle změnit dvěma vlastnosti s názvem lastLogin a userVisits, klikněte jednoduše zkopírovat ukázkový dokument, aktualizovat hodnoty pro tyto dvě vlastnosti a nahrajte zkopírovaný dokument.

    ![Zadejte požadavky na výkon kalkulačky Jednotková žádost][3]

4. Klikněte na výpočty a zkoumat výsledky.

    ![Žádost o jednotku kalkulačky výsledků][4]

>[AZURE.NOTE]Pokud máte typů dokumentů, které se liší výrazně z hlediska velikost a počet indexované vlastnosti, pak nahrajte výběru každého *typu* typické dokumentů do nástroje a potom vypočítat výsledky.

###<a name="use-the-documentdb-request-charge-response-header"></a>Použití záhlaví DocumentDB žádost o částka odpovědí
Každá odpověď z služby DocumentDB obsahuje vlastní záhlaví (x-ms-žádost o poplatek), která obsahuje jednotek žádost o použití žádosti. Toto záhlaví je také k dispozici až SDK DocumentDB. V .NET SDK je RequestCharge vlastnosti objektu ResourceResponse.  Pro dotazy Průzkumníka DocumentDB dotazu na portálu Azure informace žádosti o nákladů provedených dotazů.

![Zkoumání RU náklady v podokně dotaz][1]

S tímto v paměti, jednu metodu odhad, že množství rezervovaná výkon vyžadované aplikace slouží k zaznamenání poplatku za jednotku žádost o přidružené ke spuštěným typické operace proti zástupce dokumentu používané aplikací a pak odhad počet operací předpokládáte provádění sekundu.  Nezapomeňte změřit a obsahovat typické dotazů a DocumentDB skript i použití.

>[AZURE.NOTE]Pokud máte typů dokumentů, které se liší výrazně z hlediska velikost a počet indexované vlastnosti záznam příslušných operace žádost o poplatku za jednotku přidružené každého *typu* typické dokumentu.

Příklad:

1. Záznam poplatku za jednotku žádost o vytváření (vložení) typické dokumentem. 
2. Záznam poplatku za jednotku žádost o čtení typické dokumentem.
3. Záznam poplatku za jednotku žádost o aktualizaci typické dokumentem.
3. Záznam poplatku za jednotku žádost o typické, všeobecné dokumentu dotazů.
4. Záznam žádost o poplatku za jednotku vlastních skriptů (uložené procedury, aktivačních událostí, funkcí definovaných uživatelem) využít aplikací
5. Výpočet jednotky požadované žádost dané odhadovaná počet operací, který očekáváte, spusťte sekundu.

##<a name="a-request-unit-estimation-example"></a>Příklad odhadu Jednotková požadavek
Zvažte následující dokument ~ 1 znalostní bázi Knowledge Base:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumenty jsou zmenšenou v DocumentDB, takže systému počítaný velikost dokumentu nahoře je mírně menší než 1 KB.


Následující tabulka zobrazuje přibližnou žádost jednotku poplatky za typické operace u tohoto dokumentu (poplatku za jednotku přibližnou žádost předpokládá úroveň konzistence účtu nastavena na "Relace" a, že všechny dokumenty jsou automaticky indexovat):

Operace|Žádost o poplatku za jednotku 
---|---
Vytvoření dokumentu|~ 15 RU 
Čtení dokumentu|~ 1 RU
Dokument dotazu podle id|~2.5 RU

Kromě toho této tabulce jsou uvedeny přibližnou žádost jednotku poplatky za typické dotazů používaných v aplikaci:

Dotaz|Žádost o poplatku za jednotku|# Vrácené dokumentů
---|---|--- 
Vyberte jídla podle id|~2.5 RU|1 
Vyberte jídla výrobce|~ 7 RU|7
Vyberte skupinu jídla a pořadí weight (váha)|~ 70 RU|100
Vyberte horní 10 jídla ve skupině jídla|~ 10 RU|10

>[AZURE.NOTE]Náklady RU lišit v závislosti na počet vrácených dokumentů.

Tyto informace jsme odhadnout RU požadavky pro tuto aplikaci získá počet operací a dotazy, že očekáváme za druhé:

Operace nebo dotaz|Odhadovaná čísla za sekundu|Povinný RUs 
---|---|--- 
Vytvoření dokumentu|10|150 
Čtení dokumentu|100|100 
Vyberte jídla výrobce|25|175 
Vyberte skupinou potravin|10|700 
Vyberte prvních 10|15|150 součet|155|1275

V tomto případě očekáváme požadavek průměr výkon 1,275 RU/s.  Zaokrouhlení nahoru na nejbližší 100 jsme by zřízení 1,300 RU/s kolekce této aplikace.

##<a id="RequestRateTooLarge"></a>Vyšší než omezení rezervovaný výkon
Odvolání žádost o Jednotková spotřeba je vyhodnoceno jako sazbu vztaženou na druhé. U aplikací, které překročit četnost jednotku zřizování požadavků pro kolekci žádosti o ní se sníží, dokud sazbu vynechává pod rezervovaná úroveň. Když dojde k omezení, serveru preventivně ukončit žádost o RequestRateTooLargeException (HTTP stavový kód 429) a vrátit se záhlaví x-ms opakování – po ms označující dobu v milisekundách, které uživatel musí čekat, než neúspěšných žádost.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Pokud používáte .NET klienta SDK a LINQ dotazy a potom ve většině případů, které nikdy musíte zacházet s Tato výjimka jako v předchozí verzi klienta SDK .NET implicitně zachytí tato odpověď, s ohledem na server zadán opakovat po záhlaví a opakování žádosti. Pokud váš účet přistupuje současně více klientů, proběhne úspěšně další opakovat.

Pokud máte víc než jednoho klienta kumulativním způsobem fungující nad sazba žádosti o způsobu opakování nemusí postačovat a klient vyvolají DocumentClientException s stavový kód 429 do aplikace. V těchto případech zvažte zpracování chování opakování a logiku aplikace omylem zpracování rutin nebo zvýšit výkon vyhrazená pro kolekci.

##<a name="next-steps"></a>Další kroky

Další informace o rezervovaná výkon s databázemi Azure DocumentDB, prostudujte si tyto materiály:
 
- [DocumentDB ceny](https://azure.microsoft.com/pricing/details/documentdb/)
- [Správa DocumentDB kapacity](documentdb-manage.md) 
- [Modelování dat v DocumentDB](documentdb-modeling-data.md)
- [Úrovně DocumentDB výkonu](documentdb-partition-data.md)

Další informace o DocumentDB, najdete v Azure DocumentDB [si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/documentdb/). 

Začít s měřítko a testování s DocumentDB výkonu, najdete v článku [Výkon a měřítko testováním pomocí Azure DocumentDB](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
