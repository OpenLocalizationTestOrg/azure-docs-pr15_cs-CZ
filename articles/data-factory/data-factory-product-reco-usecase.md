<properties 
    pageTitle="Případ použití dat Factory - doporučení produktu" 
    description="Informace o případu použití implementovaná pomocí Azure Data Factory spolu s dalšími službami." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Použití případu – doporučení produktu 

Azure Factory dat je jedním z mnoha služeb provádět sadu Intelligence Cortana akcelerátory řešení.  Viz [Cortana Intelligence sadu](http://www.microsoft.com/cortanaanalytics) stránka Podrobnosti o této sadě. V tomto dokumentu jsme popisují běžné případu použití, který už Azure uživatelé vyřešit a implementovaná pomocí Azure Data Factory a další služby součástí řady Cortana.

## <a name="scenario"></a>Scénář

Prodejců online se běžně chcete přesvědčit svým zákazníkům zakoupit produkty prezentace s produkty, které jsou nejčastěji pravděpodobně zajímavé a proto pravděpodobně koupit. K tomu prodejců online muset přizpůsobit své uživatele online pomocí přizpůsobených produktu doporučení pro konkrétní uživatele. Tato přizpůsobených doporučení se má na základě jejich aktuální a historických nákupní chování dat, informace o produktu, nově zavedená značky a produktům a segmentace data.  Kromě toho můžete poskytují doporučení produktu uživatele na základě analýzy celkového chování použití všem uživatelům jejich kombinovat.

Cílem tyto prodejců je optimalizovat pro uživatele klikněte na prodej převodů a získat vyšší výnosů z prodeje.  Budou dosáhnout tento převod předvádění doporučení kontextové, na základě chování produktu na základě zájmy zákazníka a akce. Pro tento případ použití používáme prodejců online jako příklad firem, které chcete optimalizovat svým zákazníkům. Tyto zásady však platí pro firmy, který chce zapojit svým zákazníkům kolem jeho zboží a služeb a nákupu zážitek svým zákazníkům s doporučení přizpůsobených produktu.

## <a name="challenges"></a>Úkoly

Existuje mnoho potenciálních této prodejců online nominální při pokusu o provedení tohoto typu případu použití. 

Nejdřív musíte požití data různě a obrazců z různých zdrojů dat, obou místních i cloudových. Tato data obsahuje údaje o produktu, historických chování o zákaznících a uživatelská data jako uživatel prohlíží online maloobchod webu. 

Druhá přizpůsobených product doporučení musí být přiměřeně a přesně výpočtu a předpovídanými. Kromě produktu značku a data o zákaznících chování a prohlížeč prodejců online potřeba zahrnout zákazníků na předchozí nákup faktor při určování doporučených produktu pro uživatele. 

Třetí musí být doporučení okamžitě dodávku propojit uživatele, aby zadal bezproblémové procházení a nákup prostředí a poskytují doporučení poslední a relevantními značkami. 

Nakonec prodejců potřebujete změřit účinnosti jejich přístup sledováním celkový prodej nahoru křížově zákazník prodejní úspěchů klikněte na převod a přizpůsobí budoucí doporučených.

## <a name="solution-overview"></a>Přehled řešení

Tento příklad případu použití byl vyřešit a implementovaná uživateli skutečné Azure pomocí Azure Data Factory a další služby součástí Cortana měřítka, včetně [HDInsight](https://azure.microsoft.com/services/hdinsight/) a [Power BI](https://powerbi.microsoft.com/).

Online prodejce, od používá úložiště objektů Blob Azure, místní SQL server, databáze SQL Azure a tržiště relačních dat jako své možnosti ukládání dat v rámci pracovního postupu.  Úložiště objektů blob obsahuje údaje o zákazníkovi, chování data o zákaznících a údaje o produktu informace. Data informace o produktu obsahuje informace o produktu značku a produktu katalogu uložená místně na datový sklad SQL. 

Všechna data kombinovat a uloženy v produktu doporučení systému předvádění přizpůsobených doporučení na základě zájmy zákazníka a akce, když uživatel přejde produkty v katalogu na webu. Zákazníci se taky podívat, produktů, které se týkají dělený součinem jejich, které jsou prohlížíte podle vzorce použití celkové webu, které nesouvisí jednoho uživatele.

![diagram případu použití](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabajty nezpracovanými web protokoly vygenerují denně z webu online obchodě jako částečně strukturovaná soubory. Do úložišti objektů Blob Azure pomocí přesun Data Factory globálně nasazeném dat jako služba je pravidelně požití protokoly nezpracovanými web a informací katalogu vztahů se zákazníky a product. Jako nezpracovaná protokoly pro den jsou oddíly (podle roku a měsíce) v úložišti objektů blob služby dlouhodobě úložiště.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) slouží k oddílu nezpracovanými souborů v úložišti objektů blob a zpracování požití protokolů ve velkém měřítku použití podregistru a Prasátko skriptů. Rozdělený webu zaznamenává data pak zpracovává extrahovat potřebné vstupů pro počítač výukové doporučení systému do generovat doporučení přizpůsobených produktu.

Doporučení systém používaný v počítači výukové v tomto příkladu je otevřít zdroj počítač výukové platformy doporučení od [Apache Mahout](http://mahout.apache.org/).  [Výukové počítače Azure](https://azure.microsoft.com/services/machine-learning/) nebo vlastní modelu můžete použije scénáře.  Mahout model slouží k předpovídání podobnosti mezi položkami na webu založené na celkové využití vyhledávání a generovat přizpůsobených doporučení na základě jednotlivé uživatele.

Nakonec sadu výsledků přizpůsobených produktu doporučení se přesune do relačních dat tržiště pro spotřebu webem prodejce.  Sada výsledků může taky přímý přístup z úložiště objektů blob jinou aplikací, nebo přesunutí k další úložiště u ostatních spotřebitele a případy použití.

## <a name="benefits"></a>Výhody

Optimalizace jejich doporučení strategie produktu a zarovnání s obchodními cíli, řešení došlo ke splnění online obchodě prodeje a marketingových cílů. Kromě toho jim podařilo umožňují a správa pracovního postupu doporučení produktu efektivně spolehlivý a nákladů efektivní způsobem. Přístup volání snadno pro ně aktualizaci jejich model a doladit účinnost založené na míru úspěchů klikněte na převod prodeje. Pomocí Azure Data Factory byly možnost opustit řízení zdrojů jejich časově náročný a drahé ruční cloudu a přesunutí do cloudu na vyžádání řízení zdrojů. Proto jim podařilo ušetřit čas, peníze a jejich zkrátit nasazení řešení. Zobrazení souvisejících zdrojích dat a stav provozní služby stal snadno vizualizace a odstraňování případných problémů s intuitivní Data Factory monitorování a správu uživatelského rozhraní dostupný z portálu Microsoft Azure. Jejich řešení můžete nyní naplánované a spravovat tak, aby data dokončení problémy se spolehlivým vyrobeno a doručeny uživatelům a dat a zpracování závislosti jsou automaticky Správa přístupových práv bez zásahu člověka.

Zadáním individuálního nákupní online prodejce, od vytvořili zákazníkem více konkurenční a poutavější příjemnější a tedy zvýšit spokojenost zákazníků a celkové prodeje.



  