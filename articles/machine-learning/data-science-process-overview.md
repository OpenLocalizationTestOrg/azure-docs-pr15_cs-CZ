<properties
    pageTitle="Co je proces pro výzkum týmu dat?  | Microsoft Azure"
    description="Proces týmu dat pro výzkum je metodu systematický vytvářet inteligentní aplikace, které využívají pokročilé technologie pro analýzu."
    keywords="data pro výzkum proces týmy vědy dat"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="what-is-the-team-data-science-process-tdsp"></a>Co je proces pro výzkum dat týmu (TDSP)?

Proces pro výzkum dat týmu (TDSP) poskytuje systematický přístup k vytváření inteligentní aplikací, který umožňuje týmy vědeckých dat do efektivně spolupracovat přes celou životního cyklu činností potřeby zapněte tyto aplikace do produktů.

Konkrétně TDSP aktuálně poskytuje týmy vědy dat pomocí:

- **Metodologie**: shrnuje několik kroků, které definují životního cyklu vývoje poskytování informací o tom, jak definovat problém, analyzovat relevantních dat, vytvořit a vyhodnocení prediktivní modely a nasazení tyto modely v podnikových aplikací.
- **Zdroje**: nástroje a technologiemi usnadnění, například OM vědy dat zjednodušit nastavení prostředí pro data pro výzkum aktivity a praktické pokyny pro nové technologie na místě.

Tady je životního cyklu vývoje procesu týmu dat pro výzkum:

![Diagram: Data pro výzkum proces týmy ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


Proces je **iterativních**: pochopení s novým i dosavadním a upřesnění v modelu vývoj vyžaduje přebudování dříve dokončené posloupnosti kroky. Existující organizační vývoj a procesy plánování projektu se **snadno upravit** pracovat s definované TDSP posloupnost kroků.

Postup v procesu jsou znázorněné propojeny [TDSP naučná stezka](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) a píše níže.  


## <a name="planning-and-preparation-steps"></a>Plánování a příprava kroků

## <a name="p1-business-and-technology-planning"></a>P1. Obchodní poznámek a plánování technologie

Spuštění analýzy projektu tím, že definujete obchodní cíle a problémy. Jsou jazyku z hlediska **obchodních požadavků**. Ústřední cíl tento krok je identifikace proměnné klíčovými podnikovými (prognóza nebo pravděpodobnost pořadí podvodné, například), které analýzu předpovídání splňovat tyto požadavky. Další informace o plánování je pak obvykle základní abyste pochopili **zdroje dat** potřebné k adresy cíle projektu od analytical perspektivy. Není běžné, například najít, že existující systémy muset shromažďovat a znovu přihlásit, další typy dat za účelem řešení tohoto problému a k dosažení cílů projektu. Najdete v článku [plánování prostředí pro proces týmu dat pro výzkum](machine-learning-data-science-plan-your-environment.md) a [scénáře pro pokročilé technologie pro analýzu Azure počítač přečíst](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Plánování a příprava infrastruktury

Prostředí analytics pro proces týmu dat pro výzkum zahrnuje některé komponenty:

- **pracovní prostory dat** místo, kam přípravu dat pro analýzu a modelování,
- **zpracování infrastruktury** předzpracování, zkoumání a modelování dat
- **Infrastruktura runtime** umožňují analytical modely a spustit inteligentní klientské aplikace používat modely.  

Infrastruktura analýzy, kterou je potřeba nastavit je často část prostředí, které je nezávislý základní operační systémy. Ale obvykle využívá data z více systémů v rámci organizace a od společnosti externích zdrojů. Infrastruktura analýzy může být čistě cloudové, nebo místní nastavení, nebo hybridním uvedených přístupů. Možnosti najdete v tématu [Nastavení dat pro výzkum prostředí pro použití v procesu týmu dat pro výzkum](machine-learning-data-science-environment-setup.md).


## <a name="analytics-steps"></a>Technologie pro analýzu pomocí následujících kroků:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1. jedí data do platformu dat

Cílem prvního kroku je k přenesení relevantních dat z různých zdrojů, buď z v rámci nebo z mimo úrovni podniku, do analýzy prostředí Pokud můžete zpracování data. **Formát** data na straně zdroje se může lišit od formátu vyžadované cíle. Některé transformace můžou tak mít taky zbývá dokončit požití nástrojů. Možnosti najdete v tématu [načtení dat do úložiště prostředí pro analýzy](machine-learning-data-science-ingest-data.md)

Kromě toho počáteční požití dat jsou potřeba pravidelně aktualizovat data jako součást procesu probíhající výukové mnoho inteligentní aplikace. Tím se teď dá nastavením **datového kanálu** nebo pracovního postupu. To je součástí iterativních součást procesu, který obsahuje vytvářet podobné powerpointové prezentace a znovu vyhodnocení analytical modely používá inteligentní aplikace nasazením řešení. Zobrazíte například [přesunutí dat ze serveru SQL místní na SQL Azure pomocí Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. prozkoumávání a vizualizaci dat

Dalším krokem je dosáhnout hlubší porozumění data pomocí své **Přehled**relací a pomocí technik tyto **vizualizace**. Toto je také kde jsou zpracovány problémy **kvality dat** a integritu, například chybějících hodnot, odlišného typu dat a nekonzistentní data relace. Předem zpracování transformace slouží vyčištění původní data před další analýzy a modelování může proběhnout. Popis najdete v článku [výukové úkoly Příprava dat pro lepší počítač](machine-learning-data-science-prepare-data.md).


## <a name="3-generate-and-select-features"></a>3. generovat a vyberte příkaz funkce

Vědeckých dat ve spolupráci s odborníky domény, musíte identifikovat funkce, které zachytit důležité vlastnosti uvedenou množinu dat a který nejlépe lze předpovídání klíčovými podnikovými proměnnými zjištěné při plánování. Tyto nové funkce můžou pocházet z existujících dat nebo můžou vyžadovat další data shromažďované. Tento proces se nazývá **inženýrské funkce** a je jednou z základní kroky při vytváření systému efektivní prognostické analýzy. Tento krok vyžaduje kreativní kombinací všechno pod kontrolou domény a další informace získáte od kroku průzkum dat. Najdete v článku [funkce inženýrské v procesu týmu dat pro výzkum](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4. vytvoření a proškolit počítače výukové modely

Data vědeckých vytvářet analytical modely odhadnout klíčové proměnné označenými obchodní požadavky podle plánování pomocí data, která má čištění krok a featurized. Systémy učení počítače domovské stránce podpory víc **modelování algoritmů** , které jsou k dispozici širokou škálu případy. Najdete v článku [Výběr algoritmů výukové počítače Azure](machine-learning-algorithm-choice.md).

Data vědeckých musí zvolte nejvhodnější modelu doplňku pro jejich předpovědí úkolu a je běžné, výsledky z více modelů potřebujete zkombinovat k dosažení nejlepších výsledků. Zadávání dat pro modelování obvykle náhodně rozdělit na tři části:

- sady dat školení
- k sadám ověření dat
- testování k sadám dat

Modely jsou vytvořené pomocí **uvedenou množinu dat školení**. Spuštěním modely a měření chyby předpovědí pro **ověření uvedenou množinu dat**je vybrána optimální kombinací modely (s parametry správně zapnuli). Nakonec **testování uvedenou množinu dat** slouží k vyhodnocení výkonu zvolené modelu na nezávisle na datech, pomocí kterého nebyl školení nebo ověření modelu.  Postupy naleznete v tématu [vyhodnocení modelu výkonu Azure počítač přečíst](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. nasazení a používání modelů produktu

Jakmile máme sadu modely, které dobře provést, mohou být **operationalized** jiných aplikací používat. V závislosti na obchodní požadavky předpovědí jsou určené v **reálném čase** nebo na základě **dávku** . Operationalized, modely potřeba vystavit **otevřete rozhraní API** , která je snadno spotřebované množství z různých aplikací tyto online webu, tabulek, řídicí panely nebo čára business a back-end aplikací. V tématu [Deploy webové služby Azure počítače učení](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Souhrnné informace a další kroky

[Týmu dat pro výzkum proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) modelovat jako několik vstupní kroků, které **obsahují pokyny** pro úkoly potřebné k použití pokročilé technologie pro analýzu a vytváření inteligentní aplikace. Každý krok také najdete informace o používání různých technologií Microsoft dokončete úkoly popsané.

Během TDSP neuloží určité typy **si přečtěte následující dokumentaci** artefakty, je nejvhodnější dokumentu výsledky průzkum, modelování a hodnocení a uložte podstatné kód tak, aby analýzu můžete vstupní, když vyžaduje. To umožňuje, aby opakované použití technologie pro analýzu práce při práci v jiných aplikacích zahrnující podobná data a prediktivní vkládání úkoly.

Slouží také úplného začátku do konce návody, které ukazují všechny potřebné kroky postupu pro **konkrétní scénáře** . Jsou uvedeny se miniatury popisy v tématu [návody týmu dat pro výzkum obrázku](data-science-process-walkthroughs.md) .
