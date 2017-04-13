<properties
   pageTitle="Vyrovnávání svůj Cluster s Azure struktury clusteru zdroje Správce služeb | Microsoft Azure"
   description="Úvod do služby Vyrovnávání svůj cluster s struktury clusteru zdroje Správce služeb."
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

# <a name="balancing-your-service-fabric-cluster"></a>Vyrovnávání svůj cluster struktury služby
Správce služby struktury clusteru umožňuje vykazování dynamické zatížení reagovat na změny v clusteru, kontrole porušení omezení a nové posouzení cluster v případě potřeby. Ale intervalu dělá tyto věci, jaký vyvolá? Existuje několik ovládacích prvků související s tím.

První ovládací prvky kolem vyrovnávání jsou sadu časovače. Tyto časovače řídit, jak často správce prostředků clusteru kontroluje stav clusteru pro činnosti, které je nutné zvážit. Existují tři různé kategorie práce, oba objekty mají svoje vlastní odpovídající časovače. Jsou:

1.  Umístění – této fázi se zabývá uvedení příslušnosti instance, které nebyly nalezeny ani stavové repliky. Toto se vztahuje nové služby a zpracování stavové repliky nebo příslušnosti instance, které se nezdařil a nutné znovu vytvořit. Odstranění a přetažením repliky nebo instance taky zpracován tady.
2.  Omezení zkontroluje – této fázi kontroluje a opravuje porušení omezení jiné umístění (pravidla) v systému. Příklady pravidel jsou například zajištění uzly nejsou přes kapacitu a že jsou splněny omezení umístění do služby (Další informace o těchto novější).
3.  Vyrovnávání – této fázi zkontroluje aktivní nové posouzení stačí podle nakonfigurovaného požadovanou úroveň rozložení pro různé metriky, a že v takovém případě pokusí se najít uspořádání v clusteru, který je další Rovnováha.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurace kroky správce prostředků obrázku a časovače
Každý z těchto různých typů oprav, kterou můžete udělat správce prostředků clusteru je řízené pomocí různých časovače, který se řídí jejich četnost. Tak například pokud chcete zacházet s uvedení nového pracovního vytížení služby clusteru každé hodinu (dávkové je nahoru), ale chcete běžná vyrovnávání zkontroluje každých několik sekund, můžete nakonfigurovat toto chování. Při každé časovače se úkol naplánuje. Ve výchozím nastavení správce prostředků prohledá stavu a nainstalovat aktualizace (dávky všechny změny, které se uskutečnily od posledního prohledávání, jako je sledováním, která je uzel dolů) každé 1/10 sekund, sady umístění a omezení najdete příznaků pro každý druhý a spojenou příznaku každých 5 sekund.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Dnes jsme provádět pouze jednu z následujících akcí najednou, sebou (proto označovány tyto konfigurace jako "minimální intervalů")). Toto je tak, aby například jsme jste již odpověděli libovolné pole Čekání na žádosti o vytvoření nové repliky před jsme přejděte vyrovnávání clusteru. Zobrazíte tak, že časových intervalech výchozí zadali, doporučujeme zkontrolovat a kontrola něco potřebujeme se často, což znamená, že změny Provedeme na konci každého kroku je obvykle menší: můžeme nejsou skenování pomocí hodinách změn v clusteru a pokusíte opravte v celém dokumentu, jsme pokoušíte se zpracování věci více nebo méně jak probíhají, ale s některé dávky při řadu možností v stejnou dobu. Díky zdroje služby struktury správce velmi citlivé akcí, které se provedou clusteru.

Při většina z nich úkoly jsou jednoduchých (pokud jsou tu uvedené omezení porušení opravný nástroj je, pokud jsou služby vytvořit, byla vytvořená), správce prostředků clusteru, je nutné některé další informace o lze zjistit, zda imbalanced obrázku. Pro které máme dvěma datovými konfigurace: *Vyrovnávání limity* a *Prahové hodnoty aktivity*.

## <a name="balancing-thresholds"></a>Vyrovnávání prahové hodnoty
Mezní vyrovnávání je hlavní ovládací prvek pro spuštění nové aktivní posouzení (Nezapomeňte, že časovač jenom pro intervalu správce prostředků obrázku zkontrolujte - neznamená, že se nic nestane). Mezní vyrovnávání definuje, jak imbalanced clusteru musí být konkrétní metriky v pořadí pro správce clusteru vzít v úvahu imbalanced a vyrovnávání aktivační událost.

Vyrovnávání mezní hodnoty jsou definované na základě za míru jako součást definici obrázku. Další informace o metriky rezervování souborů [v tomto článku](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Mezní hodnota vyrovnávání pro metriky je poměr. Pokud počtu zatížení na uzel nejčastěji načtené rozdělené podle počtu zatížení na uzel nejméně načtené překročí toto číslo a pak clusteru je považován za imbalanced a vyrovnávání bude aktivován při příštím zkontroluje, správce prostředků obrázku (ve výchozím nastavení někdy 5 sekundy jako upraveny tak, že MinLoadBalancingInterval nahoře).

![Příklad vyrovnávání mezní hodnota][Image1]

V tomto příkladu jednoduché spotřebovává každé služby jednu jednotku některé metriky. V horním příkladu maximální zatížení uzel je 5 a minimální hodnota je 2. Řekněme, že vyrovnávání mezní hodnota této metriky je 3. Proto v horním příkladu clusteru je považován za Rovnováha a bez vyrovnávání se spouští při zkontroluje Správce prostředků obrázku (protože poměr clusteru je 5/2 = 2,5, který je menší než zadaný vyrovnávání prahové hodnoty 3).

V příkladu dole maximální zatížení uzel je 10, zatímco minimální hodnota je 2, výsledkem poměr 5. To clusteru umístí nad navržené vyrovnávání mezní hodnota 3 tohoto metriky. Globální rebalancing spustit jako výsledek, budou plánované příště vyrovnávání časovače se aktivuje. Všimněte si, že jenom proto, že je vykazuje vyrovnávání hledání postavu neznamená nic přesouvá - clusteru je někdy imbalanced ale situace nelze lepší –, ale v případě, že tohoto typu (minimálně ve výchozím nastavení) některé načíst bude úměrně pravděpodobností Uzel3. Všimněte si, že od jsme nepoužíváte hladový přístup některé zatížení může taky rozdělí Uzel2 vzhledem k tomu, která by vedla v minimalizace celkové rozdílů mezi uzly, ale by Očekáváme, že by většinou načíst flow Uzel3.

![Vyrovnávání mezní příklad akce][Image2]

Poznámka: zobrazuje pod vyrovnávání prahové hodnoty není explicitní cíl – vyrovnávání prahové hodnoty jsou jenom *aktivační události* , která říká správce prostředků obrázku, který by měla vypadat do clusteru zjistit jaká vylepšení můžete nastavit.

## <a name="activity-thresholds"></a>Mezní hodnoty aktivity
V některých případech sice relativně imbalanced uzly *celkovou* kapacitu zatížení clusteru nedostatku. To může být jenom z důvodu čas nebo protože clusteru je nové a právě dosáhnout toho, aby zavedeny. V obou případech nechcete přemýšlet vyrovnávání clusteru, protože je skutečně jenom účelné – budete jenom čekat sítě a výpočetním prostředky, které přesunout objekty kolem, aniž by absolutní rozdíl. Protože chceme toto nedělejte, je další ovládací prvek uvnitř správce prostředků, jmenoval prahové hodnoty činnosti, které vám umožní určit některé absolutní dolní mez aktivity – Pokud žádný uzel obsahuje aspoň to velikost zatížení pak vyrovnávání nebudete spustil i v případě, že došlo ke splnění vyrovnávání prahové hodnoty.

Jako příklad Řekněme, že máme sestav s následující celkové součty pro spotřebu na tyto uzly. Řekněme taky jsme zachovat naše mezní hodnota vyrovnávání 3 tohoto metriky, ale teď máme také aktivity mezní 1536. V prvním případě při imbalanced za mezní vyrovnávání clusteru žádný uzel splňuje této mez aktivity, takže samotné jsme opustit věci. V příkladu dole Uzel1 překročení způsob, jak je mezní hodnota aktivity tak, aby vyrovnávání proběhne (protože překročení mezní vyrovnávání a aktivity mezní metriky)

![Příklad mezní aktivity][Image3]

Stejně jako vyrovnávání mezní hodnoty aktivity mezní hodnoty jsou definované za míru prostřednictvím definici obrázku:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Všimněte si, že vyrovnávání a aktivity obě se míru - vyrovnávání stejným bude pouze aktivován vyrovnávání a aktivity mezní hodnoty jsou stejné metriky překročení. Tedy pokud jsme větší než mezní vyrovnávání paměti a mezní aktivity pro procesor, vyrovnávání nespustí, dokud nejsou překročena zbývající mezní hodnoty (vyrovnávání mezní hodnoty pro procesor) a prahové hodnoty aktivity paměti.

## <a name="balancing-services-together"></a>Vyrovnávání služby společně
Je něco, co je potřeba pamatovat zajímavé zda clusteru imbalanced nebo ne je clusteru celé rozhodnutí, že tak, jak jsme přejít o řešení pohybu repliky jednotlivých služeb a instance kolem. Tohle dává smysl správné? Pokud paměti je skládaný na jednom z uzlů více replikách nebo instance může způsobovat k němu, tak ji může vyžadovat přesunutí stavové repliky nebo příslušnosti instance, které metrickými problémového, imbalanced.

Občas když zákazníka vyzvedne nás nahoru nebo soubor lístek oznamující, že služba, která nebyla imbalanced máte přesunout. Jak může stát, že službu získá přesunutí i v případě, že všechny tuto službu metriky byly Rovnováha, i dokonale Ano, v době jiných neodpovídá? Podívejme!

Přebírají například čtyři služby Service1, Service2, Service3 a Service4. Service1 sestavy proti metriky Metric1 a Metric2 Service2 proti Metric2 a Mmetric3 Service3 proti Metric3 a Metric4 a Service4 proti některé metrických Metric99. Surely uvidíte, kde ukážeme, tady. Máme řetěz! Z hlediska správce prostředků clusteru nemáme skutečně čtyř nezávislé služeb, máme spoustu služby, které jsou související (Service1 Service2 a Service3) a ten, který je mimo na vlastní.

![Vyrovnávání služby společně][Image4]

Tak, aby bylo možné, že byl zjištěn rozdíl v Metric1 může způsobit repliky nebo instance patřící do Service3 přemístíte. Obvykle se poměrně omezuje tyto pohyby, ale může být větší v závislosti na přesně jak imbalanced Metric1 máte a jaké změny byly nutné clusteru k opravit. Můžete taky říkáme, se ujistili se, že byl zjištěn rozdíl v metriky 1, 2 nebo 3 nikdy způsobí, že pohybu v Service4 – bude žádný bod od přesunutí replik nebo instance patřící do Service4 kolem může opravdu nic Nedělejte a ovlivnit zůstatek metriky 1, 2 nebo 3.

Správce prostředků clusteru automaticky, na jaké služby se vztahují, protože služeb byly přidány, odebrání nebo měli jejich změna metrických konfigurace – například, mezi dvěma výskytů vyrovnávání Service2 může mít byla překonfigurovat odebrat Metric2 hodnoty. Tím se přeruší řetězec mezi Service1 a Service2. Teď místo dvěma skupinami služeb, máte tři:

![Vyrovnávání služby společně][Image5]

## <a name="next-steps"></a>Další kroky
- Metriky jsou, jak správce služby struktury clusteru prostředků spravuje spotřeby a kapacita clusteru. Přečtěte si další informace o jejich a jak je nastavit najdete v tématech [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)
- Pohyb náklady je jedním ze způsobů signalizace správce prostředků clusteru některých služby se více drahé přesunout než ostatní. Další informace o nákladech pohybu, najdete pod odkazy [v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md)
- Správce prostředků clusteru má několik omezením, které je možné konfigurovat zpomalit konve clusteru. Ještě obvykle potřebné, ale v případě potřeby je můžete seznámit s je [zde](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
