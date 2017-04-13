<properties 
    pageTitle="Poznámky k verzi technologie pro analýzu toku | Microsoft Azure" 
    description="Poznámky k verzi technologie pro analýzu toku" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Poznámky k verzi toku analýzy

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Poznámky k verzi 04/15/2016 toku analýzy ##

Tato verze obsahuje následující aktualizaci.

Název | Popis
---|---
Výstup všeobecně dostupná pro Power BI  | [Výstup Power BI](stream-analytics-power-bi-dashboard.md) jsou teď přístupné. Odebrali jsme vypršení platnosti 90 dnů se tak mohli ověřovat k Power BI. Další informace o scénáře, kde se tak mohli ověřovat musí být obnoven naleznete v části [prodloužit se tak mohli ověřovat](stream-analytics-power-bi-dashboard.md#Renew-authorization) vytváření řídicího panelu Power BI.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Poznámky k 03/03/2016 verzi toku analýzy ##

Tato verze obsahuje následující aktualizaci.

Název | Popis
---|---
Nové položky toku analýzy dotazovací jazyk  | SAQL se [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN stránky"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN stránky") a [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN stránky").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Poznámky k 12/10/2015 verzi toku analýzy ##

Tato verze obsahuje následující aktualizaci.

Název | Popis
---|---
Aktualizace verze rozhraní REST API | Verze rozhraní REST API aktualizovaný 2015 10 01. Podrobnosti najdete na webu MSDN v [ [Toku analýzy správy REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) počítače výukové integraci](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)a v toku analýzy.
Integrace výukové Azure počítače | V této verzi je podpory Azure počítače výukové uživatelsky definované funkce. V tématu [kurz](stream-analytics-machine-learning-integration-tutorial.md) pro další informace, jakož i [Obecné blogu oznámení](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Poznámky k 11/12/2015 verzi toku analýzy ##

Tato verze obsahuje následující aktualizaci.

Název | Popis
---|---
Nové chování vyberte | Vyberte v toku analýzy rozšířil umožňuje * jako vlastnost přístupové vnořené záznamu. Další informace naleznete v [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Komplexních datových typů").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Poznámky k verzi 10/22/2015 toku analýzy ##

Tato verze obsahuje tyto aktualizace.

Název | Popis
---|---
Funkce jazyků další dotazu | Technologie pro analýzu toku má rozbalená dotazovací jazyk s využitím následujících funkcí: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx) [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [prostorového uspořádání](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [ODHLÁSIT](https://msdn.microsoft.com/library/azure/mt605290.aspx), [ČTVEREC](https://msdn.microsoft.com/library/azure/mt605288.aspx)a [odmocnina](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Odebrání agregační omezení  | Tato verze odebere omezení 15 agregace v dotazu. Je teď bez omezení počtu agregace na dotaz.
Seskupit podle System.Timestamp funkci | Funkce [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) nyní umožňuje window_type nebo [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Přidané ODSAZENÍ Tumbling a vše systému windows | Ve výchozím nastavení jsou [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) a [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows zarovnat proti nula času (1/1/0001 UTC 12:00:00 dop.). Nové (volitelné) parametr "offsetsize" umožňuje určit vlastní posun (nebo zarovnání).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Poznámky k 09/29/2015 verzi toku analýzy ##

Tato verze obsahuje tyto aktualizace.

Název | Popis
---|---
Azure IoT sadu Public Preview | Technologie pro analýzu toku je součástí Public Preview sadu IoT Azure.
Azure integrace portálu | Kromě Další informace o stavu v portálu pro správu Azure toku analýzy teď integrované [Azure portálu](https://azure.microsoft.com/overview/preview-portal/). Poznámku, analýzy toku funkce na portálu náhled právě podmnožinu funkce, které na portálu Správa Azure bez podpory pro účely testování dotazu v prohlížeči, konfigurace a prohlížení nebo vytváření nových vstupní a výstupní zdroje v předplatné, ke kterým máte přístup k výstupní Power BI.
Podpora pro DocumentDB výstup | Úlohy analýzy toku můžete nyní výstup [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Podpora k zadání vstupních hodnot IoT centrální | Úlohy analýzy toku můžete nyní jedí data z IoT rozbočovače.
Časové razítko tak, že pro nesourodými zvláštní události | Když jednoho datového proudu obsahuje více typů událostí v různých polích s časová razítka, teď můžete [Časové razítko tak, že](http://msdn.microsoft.com/library/mt573293.aspx) pomocí výrazů zadat jinou časové razítko pole pro ke každému případu.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Poznámky k verzi 09/10/2015 toku analýzy ##

Tato verze obsahuje tyto aktualizace.

Název|Popis
---|---
Podpora pro PowerBI skupiny|Povolte sdílení dat s ostatními uživateli Power BI, analýzy toku úlohy můžete teď k zápisu [PowerBI skupiny](stream-analytics-define-outputs.md#power-bi) do svého účtu Power BI.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Poznámky k 08/20/2015 verzi toku analýzy ##

Tato verze obsahuje tyto aktualizace.

Název|Popis
---|---
Přidání poslední (funkce) |Funkce [poslední](http://msdn.microsoft.com/library/mt421186.aspx) je teď dostupná v toku analýzy úlohy umožňuje načítat posledních události v toku událostí v rámci dané časový rámec.
Nové funkce Array|Funkce Array [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) a [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) jsou teď k dispozici.
Nové funkce záznamu|Funkce záznam [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) a [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) jsou teď k dispozici.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Poznámky k 07/30/2015 verzi toku analýzy ##

Tato verze obsahuje tyto aktualizace.

Název|Popis
---|---
Power BI organizace Id oddělené z Azure Id|Tato funkce umožňuje [výstup Power BI](stream-analytics-power-bi-dashboard.md) pro ASA úlohy v části libovolný typ účet Azure (Live Id nebo Org Id). Kromě toho můžete používat Id organizace pro váš účet Azure a použijte jiný název pro povolení výstup Power BI.
Podpora pro službu Bus fronty výstup|[Služba Bus fronty](stream-analytics-define-outputs.md#service-bus-queues) výstupy jsou teď dostupné v toku analýzy úlohy.
Podpora pro službu Bus témata výstup|[Služba Bus témata](stream-analytics-define-outputs.md#service-bus-topics) výstupy jsou teď dostupné v toku analýzy úlohy.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Poznámky k 07/09/2015 verzi toku analýzy ##

Tato verze obsahuje tyto aktualizace.


Název|Popis
---|---
Vlastní objektů Blob výstup rozdělení|Výstupy úložiště objektů BLOB teď povolit možnosti určete frekvenci výstupu psané objektů BLOB a struktury a formát struktura složek cestu dat výstupu. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Poznámky k 05/03/2015 verzi toku analýzy ##

Tato verze obsahuje tyto aktualizace.


Název|Popis
---|---
Okno odchylky nepřítomnosti v pořádku zvětšeným maximální hodnota|Maximální velikost okna odchylky mimo pořádku odteď umožňuje zadávání 59:59 (mm: ss)
JSON výstupní formát: Řádek oddělené nebo matici|Teď je k dispozici možnost při výstupu do úložiště objektů Blob nebo události centrální výstup buď matici JSON objektů nebo oddělení objektů JSON s nový řádek. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Poznámky k verzi toku analýzy 04/16/2015 ##


Název|Popis
---|---
Zpoždění v úložišti Azure konfigurace účtu|Při vytváření toku analýzy úlohy v oblasti poprvé, zobrazí se pokyny k vytvoření nového účtu úložiště nebo zadejte existující účet pro sledování toku analýzy úloh v dané oblasti. Z důvodu latence v konfiguraci sledování vytvoření jiného toku analýzy úlohy ve stejné oblasti než 30 minut vás vyzve, určete druhý účet úložiště místo zobrazující toho naposledy nakonfigurováno v rozevíracím seznamu sledování úložiště účtu. Aby nedocházelo k vytvoření nepotřebných účtu úložiště, počkejte, než 30 minut po vytvoření projektu v oblasti poprvé před další úkoly v této oblasti.
Úlohy inovace|V současné době toku analýzy nepodporuje živou úpravy definice nebo konfigurace spuštěná úloha. Chcete-li změnit vstup, výstup, dotazu, měřítko nebo konfigurace spuštěná úloha, musíte nejdřív zastavit úlohu.
Datové typy odvozeny od vstupní zdroj|Pokud není příkaz CREATE TABLE, zadávání typ je odvozeno z vstupní formát, například všechna pole ze souboru CSV řetězec. Pole musí explicitně převedeny na správný typ pomocí funkce CAST a zabránit tak typu chyby.
Chybějící pole jsou výstupu jako hodnoty null|Odkazování na pole, které není k dispozici ve zdroji vstupní bude mít za následek hodnoty null v akci výstup.
S příkazy musí předcházet příkazy SELECT|V dotazu příkazy SELECT musí splňovat poddotazy definované pomocí příkazů.
Problém nedostatku paměti|Restartujte streamování analýzy úlohy s velké odchylku pro mimo pořadí událostí a/nebo složitých dotazů Správa velkého množství stavu může způsobit, že úlohu docházet paměti výsledkem projektu. Spuštění a ukončení operace budou zobrazeny v protokolech operace projektu. Chcete-li předejít toto chování, měřítko dotazu se přes více oddílů. V budoucí verzi bude toto omezení řešit zhoršuje výkon v dotčeném úlohách místo jejich restartováním počítače.
Velké objektů blob vstupů bez časové razítko datové může způsobit problém nedostatku paměti|Jinými velkých souborů z úložiště objektů Blob může způsobit toku analýzy úlohy pád, pokud pole časového razítka není zadán prostřednictvím časové razítko tak, že. K tomuhle problému předejít, mějte na každý objektů blob do 10MB velikost.
Omezení objemu události databáze SQL|Při použití SQL databáze jako cílový výstup, velmi vysoké objemy dat výstupu může způsobit toku analýzy časový limit. Tento problém vyřešíte snížení hlasitosti výstup pomocí agregační funkce a operátory filtr nebo klikněte na úložiště objektů Blob Azure nebo rozbočovače událost jako cílový výstup místo.
Datové sady PowerBI může obsahovat pouze jednu tabulku|PowerBI podporuje víc než jednu tabulku v dané datové sady.

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
