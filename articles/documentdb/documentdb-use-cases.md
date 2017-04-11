<properties 
    pageTitle="Běžné DocumentDB případy použití | Microsoft Azure" 
    description="Informace o horní pět případy použití pro DocumentDB: uživatel generovaného obsah protokolování událostí, katalogu dat, uživatelských předvoleb dat a Internet věcí (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Běžné případy použití DocumentDB
Tento článek obsahuje přehled o několik běžné případy použití pro DocumentDB.  Doporučení v tomto článku sloužit jako výchozí bod jako vyvíjet aplikace s DocumentDB.   

Po přečtení v tomto článku si budete moct odpovězte na následující otázky: 
 
- Jaké jsou běžné případy použití pro DocumentDB?
- Jaké jsou výhody používání DocumentDB jako uživatel generovaného úložiště obsahu?
- Jaké jsou výhody používání DocumentDB jako katalog dat uložit?
- Jaké jsou výhody používání DocumentDB jako protokolu událostí uložit?
- Jaké jsou výhody používání DocumentDB jako uživatelská data předvolby uložit?
- Jaké jsou výhody používání DocumentDB jako datový úložiště pro systémy Internet věcí (IoT)?

## <a name="common-use-cases-for-documentdb"></a>Běžné případy použití pro DocumentDB
Azure DocumentDB je univerzální NoSQL databáze, která se používá v širokou škálu aplikací a případy použití. Je vhodné použít pro všechny aplikace, které potřebuje doby odezvy zhoršeným pořadí milisekund a musí rychle zobrazit. Zde jsou některé atributy DocumentDB, díky kterým ji vhodné pro výkonné aplikace.

- DocumentDB nativně oddíly dat pro vysokou dostupnost a škálovatelnost.
- Na DocumentDB má SSD záložní úložiště s doby odezvy nízkou latencí pořadí milisekund.
- Na DocumentDB podpora konzistence úrovně jako případný, relace a ohraničených staleness umožňuje nízké náklady na výkon poměr. 
- DocumentDB má flexibilní popisný dat ceny modelu metry úložiště a výkon nezávisle na sobě.
- DocumentDB je rezervovaná výkon modelu umožňuje představit z hlediska počet čtení a zápis místo procesoru a paměti/procesorů podkladového hardwaru.
- Na DocumentDB návrh vám umožňuje přizpůsobit rozsáhlé žádost svazky v pořadí miliardy žádosti na jeden den.

Když přijde na web, mobilní, herní a IoT aplikace, které bylo nutné zhoršeným odezvy a zpracovat obrovské množství čtení a zápis jsou zvláště tyto atributy. 

## <a name="user-generated-content"></a>Uživatel generovaného obsahu
Běžné případ použití pro DocumentDB je k ukládání a uživatel dotazu generovaného obsahu (UGC) pro web a mobilní aplikace, zejména sociálních médií aplikace.  Pár příkladů UGC jsou konverzaci tweety, příspěvků na blogu, hodnocení a komentáře.  Často UGC v aplikacích sociálních médií, je prolnutí křivka text, vlastnosti, značky a relací, které nejsou ohraničená pevné strukturu.   

Obsah například konverzace, komentáře a příspěvky mohou být uloženy v DocumentDB bez nutnosti transformace nebo složité objekt relační mapování vrstvy.  Vlastnosti dat můžete přidat nebo upravit snadno podle požadavků jako vývojáři iteraci kód aplikace tedy podporu rychlý vývoj.  

Aplikace, které integrace s různými sociálních sítí musí odpovídat změňte schémata z nich.  Jak dat automaticky indexováno ve výchozím nastavení DocumentDB, je připravená k zpracovávat kdykoli data.  Proto tyto aplikace mít možnost načíst průzkumy podle svých potřeb.       

Mnoho sociálních aplikací spustit ve velkém měřítku globální a můžou objevit neočekávané využití vyhledávání.  Pružnost při proměnné velikosti úložiště dat je důležité jako vrstvě aplikací upraví podle použití služba.  Velikost můžete změnit tím, že přidáte další data oddílů pod účtem DocumentDB.  Další účty DocumentDB můžete taky vytvořit mezi více oblastí. Dostupnost oblast služby DocumentDB najdete v článku [Azure oblastí](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Katalog dat
Příklady použití katalogu dat zahrnují ukládání a dotazování sadu atributy entit například osoby, místa a produkty.  Pár příkladů katalogu dat jsou uživatelských účtů, katalogy produktů, registry zařízení pro IoT a faktury systémů materiály.  Atributy pro data může být trochu jiný a že můžou změnit v čase podle požadavků na aplikace.  

Zvažte příklad katalogu produktů pro dodavatele automobilový částí. Všechny části může mít vlastní atributy kromě běžné atributy, které všechny části sdílejí.  Kromě toho můžete změnit atributy pro určitou část v následujícím roce při uvolnění nový model.  Jako k úložišti dokumentů JSON DocumentDB podporuje flexibilní schémata a umožňuje představující data s vnořené vlastnosti a proto je vhodné pro ukládání data katalogu produktů.       

## <a name="logging-and-time-series-data"></a>Protokolování a časových řad dat
Protokolování aplikace je často vyzařovaného do velkých objemů a může mít odlišné atributy na základě verze nasazené aplikace nebo součást protokolování událostí.  Dat protokolu není omezena vzájemně složité vztahy nebo pevná struktury. Stále protokolu data uložena ve formátu JSON protože JSON je lehké a snadno lidi ke čtení.
   
Obvykle existují dva hlavní použití případy souvisí s daty v protokolu událostí.  První případu použití, je provádět ad-hoc dotazů podmnožinu dat pro řešení potíží.  Při řešení problémů podmnožinu dat je nejdřív načtená z kontingenčního seznamu protokoly, obvykle tak, že časovou řadu.  Potom podrobnostmi provádí filtrování datovou sadu s úrovně chyby nebo chybové zprávy. Toto je ukládat protokoly událostí v DocumentDB je výhodou. Ve výchozím nastavení automaticky indexováno protokolu data uložená v DocumentDB a tedy bude připravená k zpracovávat kdykoli. Kromě toho můžete dat protokolu zachován mezi oddíly dat jako časovou řadu. Starší protokoly můžete být vrácena chladírenský za zásady uchovávání informací.          

Druhá případu použití spouštějí dlouho úlohy technologie pro analýzu dat provést v režimu offline přes velké množství dat protokolu.  Tento případ použití příkladem analýzy dostupnost serveru, aplikace chyby analýzy a analýza dat clickstream.  Obvykle Hadoop slouží k provedení těchto typů analýzy.  Pomocí konektoru Hadoop DocumentDB fungovat DocumentDB databáze jako zdroje dat a propadů pro Prasátko, podregistru a mapy a zmenšení práce. Podrobnosti u spojnice Hadoop DocumentDB najdete v tématu [spuštění úlohy Hadoop s DocumentDB a HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Herní
Vrstvy databáze je důležité součásti herní aplikací. Moderní hry provést grafické zpracování na mobilní telefon/konzoly klienty, ale přes v cloudu, aby doručování obsahu vlastní a přizpůsobených například v hry stat integrace sociálních médií a maximum skóre tabulky. Her vyžadovat velmi nízký čekacích dob pro čtení zápisy poskytnout atraktivních v hry prostředí a osy databáze je potřeba zpracování nejvyšších a nejnižších hodnot v žádosti o sazby během nové herní spustí a aktualizovat funkce.

DocumentDB používá her rozsáhlé měřítko jako [procházení nefunkční: Nevyžádaná pošta je přesunuta bez člověka](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) [Další her](http://www.nextgames.com/)a [bylo 5: zákonných zástupcích](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). V obou případy použití, hlavních výhod použití DocumentDB byly takto:

- DocumentDB umožňuje výkonu měnit jejich velikost nahoru nebo dolů elastically. Díky hry zpracovat aktualizace profilu a Statistika z desítek miliony souběžné hráče voláním jednoho rozhraní API.
- DocumentDB podporuje milisekund čtení a zapíše do se tím vyhnout všechny pomalou průběhu hry.
- Automatické indexování na DocumentDB umožňuje filtrování proti několika různých vlastností v reálném čase, například najděte přehrávače podle jejich vnitřní přehrávače ID, nebo jejich GameCenter Facebook, Google ID nebo dotaz založený na přehrávače členství v sdružení. Je to možné bez vytvářet složité indexování nebo sharding infrastruktury.
- Sociálních funkcí včetně zprávy v konverzaci v hra přehrávače sdružení členství, úkoly dokončeny, maximum skóre tabulky a sociální grafy jsou snadněji implementovat s flexibilní schéma.
- DocumentDB jako spravovanou platformy jako službu (PaaS) povinné minimální nastavení správy práce umožňující rychlé iterace a snížit čas na trh.


## <a name="user-preferences-data"></a>Uživatelské předvolby dat
V současné době většina moderní webu a mobilní aplikace jsou součástí složité zobrazení a prostředí. Tyto zobrazení a prostředí jsou obvykle dynamické, kuchyňský uživatelské předvolby nebo atmosféru a branding potřeb.  Proto aplikace musejí být schopné k načtení individuální nastavení efektivně k vykreslování prvků uživatelského rozhraní a prostředí rychle. 

JSON je efektivní formát představující data rozložení uživatelského rozhraní, nejenže se lightweight, ale taky můžete snadno interpretovat JavaScript.  DocumentDB nabízí optimalizovat konzistence úrovně, které umožňují rychle čtení s nízkou latence zápisy. Ukládání dat rozložení uživatelského rozhraní včetně individuální nastavení jako JSON dokumenty v DocumentDB proto je efektivní prostředky k získání dat přes lince.

## <a name="iot-and-device-sensor-data"></a>Data snímače IoT a zařízení
Případy použití IoT běžně sdílet některé vzorce v jejich jedí, zpracovat a uložit data.  Nejprve tyto systémy umožnit příjmu dat, který můžete jedí roztržení dat ze zařízení senzorů různá národní prostředí.  Potom tyto systémy procesu a analýze streamování dat pro přehledy reálném čase. A poslední ale ne nejméně většina v opačném případě všechna data budou se má v úložišti dat pro analýzy ad hoc dotazů a offline.    

Microsoft Azure nabízí případy použití bohaté služby, které můžete využít pro IoT.  Jsou služby Azure IoT sadu služby včetně rozbočovače události Azure, Azure DocumentDB, Azure toku analýzy, centrální oznámení Azure, výukové počítače Azure, Azure HDInsight a PowerBI. 

Roztržení dat můžete požití tak, že Azure události rozbočovače jako nabízí vysoký výkon dat požití s zhoršeným latence. Požití požadovaná data zpracovat přehled reálném čase můžete funneled na Azure toku Analytics pro analýzy reálném čase. Data lze načíst do DocumentDB pro dotazování na ad hoc. Po načtení dat do DocumentDB data je připravená k zpracovávat.  Data v DocumentDB mohou sloužit jako referenčních dat jako součást analýzy reálném čase. Kromě toho dat můžete dál kontrast se zpracovány službou připojení DocumentDB dat k Hdinsightu pro Prasátko, podregistru nebo mapy a zmenšení projektů.  Kontrast potom načtení dat do DocumentDB pro vytváření sestav.   

Řešení IoT vzorku pomocí DocumentDB EventHubs a bouře, najdete v tématu [hdinsight bouře příklady úložiště na GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Další informace o Azure nabídky pro IoT najdete v článku [Vytvoření Internet Your věci](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Další kroky
 
Pokud chcete začít pracovat s DocumentDB, můžete vytvořit [účet](https://azure.microsoft.com/pricing/free-trial/) a postupujte podle našeho [cesta výuky](https://azure.microsoft.com/documentation/learning-paths/documentdb/) informace o DocumentDB a informace, které potřebujete. 

Nebo pokud chcete další informace o zákazníkům, kteří používají DocumentDB, jsou k dispozici následující příběhy zákazníků:

- [Další her](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Vycházkové nepoužívaných: bez člověka řidiče Land herní soars # 1 nepodporuje Azure DocumentDB.
- [Bylo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Jak bylo 5 implementovaná social hraní v režimu pomocí Azure DocumentDB.
- [Galerie analýzy Cortana](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana analýzy Galerie – web scalable komunity založená na Azure DocumentDB.
- [Uloženy](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Vést integrátor poskytuje mezinárodní podniky globální přehled v minutách s technologiemi flexibilní cloudu.
- [Novinky republika](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Přidání měřítka příspěvky pro informování s účelem obsazené občanů. 
- [Mezinárodní SGS](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Pro jednotné barvy po celém světě hlavní značky hodit SGS. A SGS změní Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globální vedoucí Telenor pohyboval spolu s rychlost spouštěcí pomocí cloudu. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Úložiště budoucnost běží rychlé hledání a snadno toku dat.
