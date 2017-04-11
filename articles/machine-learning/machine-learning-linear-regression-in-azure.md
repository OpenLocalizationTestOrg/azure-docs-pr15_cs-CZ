<properties 
    pageTitle="Použití lineární regresní ve počítače výukové | Microsoft Azure" 
    description="Porovnání lineární regrese model v aplikaci Excel a v Azure počítače výukové Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Použití lineární regrese Azure počítač přečíst

> *Kate Baroni* a *Robert Boatman* jsou tvůrci řešení enterprise ve společnosti Microsoft dat přehledy Centrum dokonalosti souvisí. V tomto článku popisují migrace regresní analýzy stávajícího cloudové řešení pomocí Azure počítače výukové možnosti uživatelů.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Cíl

Náš projekt začít s dvěma cíle myslet:  

1. Použití prediktivní technologie pro analýzu ke zlepšení přesnosti naši organizaci měsíční příjmy průzkumy  
2. Použití Azure ML k potvrzení, optimalizace, zvětšit rychlosti a měřítka naše výsledky.  

Jako mnoha firmách naši organizaci prochází měsíční příjmy Prognózování obrázku. Tým malá obchodní analytici byl úkol při použití počítače výukové k podpoře procesu a zlepšit přesnost prognózy.  Týmu stráví několik měsíců shromažďování dat z různých zdrojů a dat atributy projdete statistické analýzy identifikační klíčové odpovídající atributy pro prodejní Prognózování služby.  Další kroky byl zahájíte vytváření prototypů statistické regresní modely na kartě data v Excelu.  Do několika týdnů bylo modelu regresní Excelu, který byl outperforming na aktuální pole a finance Prognózování procesů. To stalo výsledek předpovídání pomocí podle směrného plánu.  


Společnost Microsoft pak trvaly, dalším krokem přesouvat naše prediktivní analýzy Azure ml chcete zjistit, jak by mohl zlepšit Azure ML na výkon prognostické.


## <a name="achieving-predictive-performance-parity"></a>Dosažení výkon prognostické dostupná

Náš prioritu byl dosáhnout dostupná mezi modely regresní Azure ML a aplikace Excel.  Dostali přesné stejná data a stejné rozdělení školení a testování dat chtěli jsme dosažení výkon prognostické dostupná mezi aplikací Excel a Azure ML.   Nejprve se nezdařilo. Modelu Excelu outperformed Azure ML modelu.   Bylo kvůli chybí Principy nastavení základní nástroje v Azure ML. Po synchronizace s Azure ML produktového týmu jsme získaných lepší znalost základu nastavení potřebné pro naše sady dat a dosáhnout dostupná mezi dvěma modely.  

### <a name="create-regression-model-in-excel"></a>Vytvoření regresní modelu v Excelu
Náš regresní Excel použít standardní lineární regrese modelu v Excelu analytické. 

Vypočítá *nechtěli absolutní % chyby* jsme použité jako měření výkonu pro model.  Trvala 3 měsíce k dosažení pracovní modelu pomocí Excelu.  Jsme přenášet moc výuky do experiment Azure ML, který nakonec skutečným v vysvětlení požadavků.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vytvoření srovnatelná experiment Azure počítač přečíst  
Společnost Microsoft a za ním postupem vytvořte naše experiment v Azure ML:  

1.  Nahrané datové do souboru csv na Azure ML (příliš malý soubor)
2.  Vytvoření nového testu a použít [Vybrat sloupce v sadě dat] [ select-columns] modul vyberte stejných dat funkcí použité v aplikaci Excel   
3.  Použít [Rozdělenými daty] [ split] modul ( *Relativní výraz* v režimu s) se data rozdělit přesně stejné sady vlaku jako bylo v Excelu  
4.  Experimented s [Lineární regrese] [ linear-regression] modul (výchozí možnosti pouze), popsány a porovnání výsledky regresní model naše aplikace Excel

### <a name="review-initial-results"></a>Počáteční zobrazit výsledky
Nejdřív modelu Excelu jasně outperformed Azure ML modelu:  

|   |Aplikace Excel|Azure ML|
|---|:---:|:---:|
|Výkon|   |  |
|<ul style="list-style-type: none;"><li>Upravit hranatých R</li></ul>| 0.96 |NENÍ K DISPOZICI|
|<ul style="list-style-type: none;"><li>Koeficient <br />Určení</li></ul>|NENÍ K DISPOZICI|   0.78<br />(zhoršeným přesnost)|
|Nechtěli absolutní chyby |  $9. 5M|  $ 19.4 M|
|Nechtěli absolutní chyby (%)|   6.03 %|  12.2 %

Při jsme naše proces a výsledky vývojáři a vědeckých dat Azure ML týmu, poskytly rychle některé užitečné tipy.  

* Pokud používáte [Lineární regrese] [ linear-regression] modul Azure ml, jsou k dispozici dvěma způsoby:
    *  Online sestup přechodu: Může být vhodnější větší problémů
    *  Běžné nejmenších čtverců: Toto je metody, kterou většina lidí si můžete představit při dozvíme lineární regrese. Pro malé datové sady běžnému nejmenších čtverců může být vhodnější volba.
*  Zvažte doladění parametr L2 účtovat tloušťky aby se zvýšil výkon. Nastavení je to 0,001 ve výchozím nastavení a pro naše kratších seznamů dat, můžeme nastavte na 0,005, aby se zvýšil výkon.    

### <a name="mystery-solved"></a>Mystery vyřešit!
Když použijete jsme doporučení, jsme dosáhli stejné podle směrného plánu výkonu v Azure ML jako v Excelu:   

|| Aplikace Excel|Azure ML (počáteční)|Azure ML s nejmenších čtverců|
|---|:---:|:---:|:---:|
|Hodnotu s popisky  |Skutečné hodnoty (číselné)|stejné|stejné|
|Student  |Excel -> Data analýzy -> regresní|Lineární regrese.|Lineární regrese|
|Možnosti student|NENÍ K DISPOZICI|Ve výchozím nastavení|běžné nejmenších čtverců<br />L2 = 0,005|
|Uvedenou množinu dat|26 řádky, funkce 3, 1 popisek.   Všechny číselné.|stejné|stejné|
|Rozdělení: vlaku|Excel školení na první 18 řádky na poslední 8 řádků.|stejné|stejné|
|Rozdělení: Test|Vzorce v Excelu regresní poslední 8 řádků|stejné|stejné|
|**Výkon**||||
|Upravit hranatých R|0.96|NENÍ K DISPOZICI||
|Koeficient určení|NENÍ K DISPOZICI|0.78|0.952049|
|Nechtěli absolutní chyby |$9. 5M|$ 19.4 M|$9. 5M|
|Nechtěli absolutní chyby (%)|<span style="background-color: 00FF00;">6.03 %</span>|12.2 %|<span style="background-color: 00FF00;">6.03 %</span>|

Koeficienty Excel navíc dobře srovnání gramáží funkce v Azure školení modelu:

||Koeficienty aplikace Excel|Azure funkce gramáží|
|---|:---:|:---:|
|INTERCEPT/posun|19470209.88|19328500|
|Funkce A|0.832653063|0.834156|
|Funkce B|11071967.08|11007300|
|Funkce C|25383318.09|25140800|

## <a name="next-steps"></a>Další kroky

Chtěli jsme používat webové služby Azure ML aplikace Excel.  Náš obchodní analytici přes v aplikaci Excel a potřebujeme způsob, jak volání webové služby Azure ML pomocí řádku dat aplikace Excel a s její pomocí vrátit hodnotu předpovídané do aplikace Excel.   

Také chtěli jsme optimalizace naše modelu pomocí možnosti a algoritmy k dispozici v Azure ML.

### <a name="integration-with-excel"></a>Integrace s Excelem
Náš řešení bylo umožňují naše Azure ML regresní model vytvořením webové služby z školení modelu.  Objevit během pár minut byl vytvořen webové služby a jsme můžou volat přímo z aplikace Excel vrátí hodnotu předpovídané příjmy.    

*Řídicí panel služeb Web* oddíl obsahuje ke stažení sešitu aplikace Excel.  Sešit získáváte předformátované webové služby rozhraní API a schématu informace vložené.   Po kliknutí na *Stažení sešitu aplikace Excel*Pozvánka se otevře a můžete ho uložit do místního počítače.    

![][1]
 
S sešit otevřený zkopírujte předdefinované parametry do části modré parametr ukázáno v následujícím příkladu.  Po zadání parametrů, Excel volání webová služba AzureML a předpovídané dosáhne štítky se zobrazí v části zelené předpovězené hodnoty.  Sešit zůstanou vytvořit předpovědí parametrů založené na modelu školení pro všechny položky řádku zadané ve skupinovém rámečku Parametry.   Další informace o používání této funkce najdete v článku [jinými webové služby Azure počítače výukové z aplikace Excel](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimalizace a další pokusů
Teď bylo uložení směrného plánu s našeho modelu Excel, jsme přesunuli rovnou optimalizovat naše Azure ML lineární regresní Model.  Jsme použili modulu [systémem filtru výběr funkcí] [ filter-based-feature-selection] zlepšit na naše výběru počátečního data prvky a pomohl nám dosáhnout zvýšení výkonu 4.6 % nechtěli absolutní chyby.   Pro budoucí projekty použijeme tuto funkci, kterou můžou uložit nám týdnů v iterace atributy dat zobrazíte správné sadu funkcí pro účely modelování.  

Plánujeme další zahrnovat další algoritmů jako [Hodnota bayesovského] [ bayesian-linear-regression] nebo [Zesílen stromové struktury rozhodování] [ boosted-decision-tree-regression] v naší pokus porovnat.    

Pokud chcete vyzkoušet regresní, je dobré datovou sadu zkusit datovou sadu ukázkové energie efektivity regresní nabízí spousty číselném atributy. Datové slouží jako součást ukázkové datové sady v ML Studio.  Různé výukové moduly umožňuje předpovídání vytápění načtení nebo chlazení načíst.  Graf níže je že výkon srovnání různých regresní učí proti předpovídání energetické účinnosti datovou sadu proměnné cílové chlazení zatížení: 

|Model|Nechtěli absolutní chyby|Průměr kořenové spolehlivosti chyby|Relativní absolutní chyba|Vztažné spolehlivosti chyby|Koeficient určení
|---|---|---|---|---|---
|Zesílen rozhodovacího stromu|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineární regrese (přechodu sestup)|2.035693|2.98006|0.233414|0.094881|0.905119
|Analytický nástroj Regrese neural sítě|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineární regrese (běžnému nejmenších čtverců)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Klíčové Takeaways 

Dozvěděli mnohem tak, že z pracovního regresní aplikace Excel a Azure počítače výukové pokusy souběžně. Vytvoření základního modelu v aplikaci Excel a porovnávání k modelům pomocí Azure ML [Lineární regrese] [ linear-regression] pomohl nám informace Azure ML a jsme zjistil příležitosti ke zlepšení výkonu výběru a model data.         

Také najít, je vhodné použít [filtr založený výběr funkcí] [ filter-based-feature-selection] urychlit předpovědí budoucí projekty.  Pomocí funkce Výběr dat, můžete vytvořit vylepšený model v Azure ML s zlepšit celkový výkon. 

Možnost převést prediktivní analytický Prognózování z Azure ML do aplikace Excel systemically umožňuje významné zvýšení možnost úspěšně poskytnout takové výsledky cílové skupině uživatelů obecných business.     


## <a name="resources"></a>Zdroje informací
Nápovědu, se kterými pracujete s regresní jsou uvedeny některé zdroje informací:  

* Analytický nástroj Regrese v aplikaci Excel.  Pokud jste doposud vyzkoušeli nikdy regresní v aplikaci Excel, tento kurz usnadňuje: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Analytický nástroj Regrese a Prognózování.  Tyler Chessman napsali článek blogu vysvětlíme, jak časové řady Prognózování v aplikaci Excel, která obsahuje popis dobré začátečníky lineární regrese. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Běžné nejmenších čtverců lineární regrese: Chyby, problémy a nástrahy.  Úvod a diskuse regresní: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
