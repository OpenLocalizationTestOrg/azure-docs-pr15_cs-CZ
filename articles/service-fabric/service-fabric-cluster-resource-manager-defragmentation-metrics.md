<properties
   pageTitle="Defragmentaci metriky v Azure služby struktury | Microsoft Azure"
   description="Základní informace o používání defragmentace nebo balicích jako strategie pro metriky v struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentaci metriky a načíst do struktury služby
Správce služby struktury clusteru hlavně se týká vyrovnávání z hlediska distribuce načíst – jak zajistit, že všechny uzly clusteru rovnoměrně z vnější využít. Je to obvykle nejbezpečnější a dosud nejchytřejším rozložení z hlediska přežití chyby, protože zajišťuje, že dané selhání nemá uzavře některé velké procento danou úlohu. Správce služby struktury clusteru podporuje různé strategie i, tedy defragmentace. Defragmentace obecně znamená, že místo potíže s distribuování využití metriky v clusteru, má ve skutečnosti pokusíme chcete sloučit. Toto je fortunate inverzi naší normální strategie – místo optimalizace clusteru podle minimalizace průměr směrodatnou odchylku metrických zatížení dané metriky, začneme optimalizace pro zvýšení odchylku. Ale proč by se má tuto strategii?

Dobře Pokud jste jste roztažení načíst rovnoměrně mezi uzly clusteru pak můžete jste požívaných si některé zdroje, které mají uzly nabízet. Obvykle to není problém, ale někdy některých úloh vytváření služeb, které jsou výjimečně velké a používání většina uzel – například 75 95 % zdrojů na uzel by skončily vyhrazené instance jediný služby nebo otevřené. To není problém, správce prostředků clusteru zjistí času vytvoření služby, které potřebuje k uspořádání obrázku při uvolnění místa pro velké pracovního vytížení a nastavit o tom, jak se stát, ale mezitím tento pracovní zátěž má čekat na clusteru je možné naplánovat.

Vzhledem je plánování nového pracovního vytížení obvykle aspoň nevelká latence malá písmena, pokud jsme není nijak jinak můžeme někdy vypálení doprava tak, že tyto rozsahu při spoustu služby a stav přemístíte, zejména pokud pracovního vytížení clusteru obecně velké (a tedy trvá déle pohyb v clusteru). Nakonec vypočtena času vytvoření v simulace založených na datech skutečné clusteru jsme jsme viděli, pokud služby byly dostatečně velký k tomu a clusteru je poměrně využívána, vytvoření těchto velké služeb by se zpomalovat a že jsme zlepšit to zavedením zásad defragmentace metriky.

Stejně jako soubor vytváření nebo access může dostat ke zpomalení že pokud pevného disku byl fragmentován a může být urychlí defragementací jednotku tak, aby došlo k větší souvislé bloky dostupné, můžete nakonfigurovat defragmentace metriky mít správce prostředků clusteru zkusit včasným zúžit načíst služeb do méně uzlů tak, aby je (téměř) vždy místnosti i velké služby , mohli rychle vytvořit. Většina lidí nebudete potřebovat, protože služby by měly být obvykle stručné a proto není snadné vyhledání místnosti, je, ale pokud máte velký služby a je vytvořen rychle potřebují (a jste ochotni přijmout nevýhody například lepší impactfulness selhání a některé zdroje, které se ponechává zemědělsky nevyužitá čekají na zatížení, je možné naplánovat) a pak strategie defragmentace je článek pro vás.

Následující obrázek zobrazí vizuální znázornění dva různé clusterů, které je defragmentovaný a jednu, který není. V případě rovnováha zvažte pohybu, které by potřeba místo jednu největší objektů služby Pokud novou byly vytvořené, ve srovnání s defragmentovanou clusteru, kde by mohly okamžitě uložená uzlech 4 a 5.

![Porovnání Rovnováha a defragmentovány clusterů][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentace výhody a nevýhody
Jaké tak ty koncepční poměry? Doporučujeme důkladné měření svého pracovního vytížení před zapnutí defragmentace metriky. Tady je rychlý tabulku co je potřeba vzít v úvahu:

| Defragmentace profesionály  | Nevýhody Defragmentace |
|----------------------|----------------------|
|Umožňuje rychlejší vytváření velké služeb | Koncentráty nahrát do méně uzly zvětšení konflikty
|Umožňuje Ztlumením přesun dat při vytváření    | Selhání může mít vliv na další služby a způsobit další konve
|Umožňuje multimédii popis požadavků a obnovit místa | Složitější celkové konfigurace řízení zdrojů

Můžete kombinovat defragmentovanou a normální metriky ve stejném clusteru a správce prostředků udělá je nejlepší zajistit, abyste měli rozložení, které slučuje co nejvíc metriky defragmentace jako můžete při pokusu o roztažení ostatních. Přesné výsledky, které dostanete závisí na počet vyrovnávání metriky ve srovnání s počet defragmentace metriky a jejich gramáží aktuální zatížení, atd.

## <a name="configuring-defragmentation-metrics"></a>Konfigurace defragmentace metriky
Konfigurace defragmentace metriky je globální rozhodnutí clusteru a jednotlivé metriky mohou být vybrány defragmentace:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Další kroky
- Správce prostředků clusteru má spoustu možností pro popis clusteru. Zjistit další informace o jejich najdete v tématech tohoto článku na [popisující služby struktury obrázku](service-fabric-cluster-resource-manager-cluster-description.md)
- Metriky jsou, jak správce služby struktury clusteru prostředků spravuje spotřeby a kapacita clusteru. Přečtěte si další informace o jejich a jak je nastavit najdete v tématech [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
