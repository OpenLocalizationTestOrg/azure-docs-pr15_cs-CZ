<properties
   pageTitle="Plánování kapacita obrázku služby | Microsoft Azure"
   description="Služba clusteru kapacita plánování aspektech. Nodetypes, životnost a spolehlivost úrovní"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Služba clusteru kapacita plánování co byste měli zvážit

Plánování kapacity provozní nasazení, je důležitým krokem. Tady je několik položek, které je nutné zvážit jako součást procesu.

- Počet uzel typy vašim potřebám clusteru začít s
- Vlastnosti každého typu uzel (velikost, primární, internetové, počet VMs atd.)
- Vlastnosti spolehlivost a životnosti clusteru

Stručně dejte nám prohlédněte si všechny tyto položky.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Počet uzel typy vašim potřebám clusteru začít s

Nejdřív budete muset zjistit, co obrázku, který vytváříte se bude používat pro a jaké typy aplikací chcete naplánovat nasadit na tomto obrázku. Pokud si nejste vymazat označenou clusteru, se pravděpodobně dosud Pokud chcete zadat kapacitu procesu plánování.

Vytvoření počtu typy uzlů, které svůj cluster potřebuje začít s.  Každý typ uzlu namapovala nastavovaná měřítko virtuálního počítače. Každý typ uzlu lze nastavit nebo dolů nezávisle na sobě, mají různé sady otevřených portů a může obsahovat jinou kapacitu metriky. Takže rozhodnutí počet typy uzlů v podstatě Doručená následující skutečnosti:

- Má mít vaše aplikace více služeb, a libovolné z nich musí být veřejná nebo internetové? Typické aplikace obsahují front-end brány služba, která přijímá vstupní z klienta a jeden nebo více back-end služby, které komunikovat s front-end službami. To je v tomto případě skončily s nejméně dvěma typy uzlů.

- Mají služeb (tvořící aplikace) různých infrastruktury potřeb například větší RAM nebo vyšší cykly CPU? Předpokládejme například, dejte nám, aby obsahoval aplikace, kterou chcete nasadit služby front-end a back-end služby. Služba front-end je možné spouštět na menší VMs (OM velikosti jako D2), která může obsahovat otevřete k Internetu.  Služba back-end však značnou výpočtu a musí běžet větší VMs (s velikostí OM jako D4 D6, D15), které nejsou internet směrem.

 V tomto příkladu sice můžete se rozhodnout, umístit všechny služby na jeden uzel typ, doporučujeme, aby se, že je umístíte v clusteru s dvěma typy uzlů.  Díky tomu u jednotlivých typů uzel mít odlišné vlastnosti připojení k Internetu nebo OM velikost. Počet VMs lze nastavit nezávisle na sobě, stejně.  

- Protože nemůžete předpovídat budoucnost, přejděte s údaje, které znáte z a rozhodnete počtu typy uzlů, které aplikace musí začínat. Vždy můžete přidat nebo odebrat typy uzlů později. Služba struktury obrázku musí být alespoň jeden uzel typu.

## <a name="the-properties-of-each-node-type"></a>Vlastnosti každého typu uzel

**Typ uzlu** můžete považovat za srovnatelná s rolí ve službě Cloud Services. Typy uzlů definovat velikosti OM, počet VMs a jejich vlastnosti. Všechny typy uzel, která je definovaná v clusteru služby struktury nastavenou jako samostatný nastavte měřítko virtuálního počítače. OM měřítko, jsou Azure výpočet zdroje, které vám pomohou nasazovat a spravovat kolekce virtuálních počítačích jako sady. Sestava definována jako nastaví různých OM měřítko, každý typ uzlu lze nastavit nebo dolů nezávisle obsahovat různé sady porty otevřít a může obsahovat jinou kapacitu metriky.

Svůj cluster můžete mít víc než jeden typ uzel, ale typ primární uzlu (prvního úkolu, které určíte v portálu) musí obsahovat minimálně pět VMs clusterů pro výrobní úloh (nebo nejméně tři VMs test clusterů). Pokud vytváříte clusteru pomocí šablony pro správce prostředků, bude atribut **je primární** ve skupinovém rámečku Typ definice uzel hledat. Typ primární uzel je typ uzel umístění služby struktury systémové služby.  

### <a name="primary-node-type"></a>Typ primární uzel
Pro cluster s více typů uzel musíte se zvolte jednu z nich je primární. Tady jsou vlastnosti typu primární uzel:

- Minimální velikost VMs typu primární uzel je určený podle životnosti osy, které zvolíte. Výchozí vrstvu životnosti je Bronzová. Podrobnosti o novinky životnosti osy a hodnoty, které může trvat pomocí posuvníku dolů.  

- Minimální počet VMs typu primární uzel je určený podle spolehlivost osy, které zvolíte. Výchozí hodnota spolehlivosti osy je stříbrný. Podrobnosti o novinky osy spolehlivost a hodnoty, které může trvat pomocí posuvníku dolů.

- Systémové služby služby struktury (například služba Správce obrázku nebo obrázek úložiště přihlašovacích údajů) jsou umístěná na typu primární uzlu a tak spolehlivost a životnosti clusteru je určen hodnota osy spolehlivost a životnosti osy hodnoty pro typ primární uzel.

![Snímek obrazovky s obrázku, který má dva typy uzel ][SystemServices]


### <a name="non-primary-node-type"></a>Typ non primární uzel
Clusteru s více typů uzel existuje jeden typ primární uzel a zbytek z nich jsou mimo primární. Tady jsou vlastnosti typu není primární uzel:

- Minimální velikost VMs pro tento typ uzlu je určený podle životnost osy, které zvolíte. Výchozí vrstvu životnosti je Bronzová. Podrobnosti o novinky životnosti osy a hodnoty, které může trvat pomocí posuvníku dolů.  

- Minimální počet VMs pro tento typ uzlu může být jeden. Měli byste však zvolit toto číslo na základě počtu kopie aplikaci nebo službu, které se mají spustit tento typ uzlu. Počet VMs v typu uzel můžete zvýšit po nasazení clusteru.


## <a name="the-durability-characteristics-of-the-cluster"></a>Vlastnosti životnosti clusteru

Životnost osy slouží k označení systému oprávnění, která se základní Azure infrastruktury máte vaší VMs. V seznamu Typ primární uzel toto oprávnění umožňuje služby struktury pozastavit jakékoliv OM úrovně infrastruktury žádosti o (například restartování OM, OM reimage nebo OM migrace) které ovlivnit kvora požadavky na systém služby a služby stavové. V seznamu není primární uzel typy toto oprávnění umožňuje služby struktury pozastavit jakékoliv žádosti o úrovni infrastruktury OM jako restartování OM, reimage OM, OM migrace atd., které ovlivnit požadavky kvora stavové služeb spuštěné v nich.

Toto oprávnění jsou vyjádřeny v následujících hodnot:

- Gold - úlohy infrastruktury může být pozastavena po dobu 2 hodin za UD

- Slovo stříbro - úlohy infrastruktury může být pozastavena po dobu 30 minut na UD (to není aktuálně povoleno pro použití. Jednou povolené to bude k dispozici pro všechny standardní VMs jednoho core a výše).

- Bronzová - žádná oprávnění. Toto je výchozí hodnota.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Spolehlivost clusteru

Vrstvy spolehlivost: slouží k nastavení počet kopie služeb systému, které chcete spustit v tomto clusteru na typu primární uzlu. Další počet repliky, vyšší spolehlivost služby systému jsou v clusteru.  

Vrstvy spolehlivosti může trvat následující hodnoty.

- Platinum – spuštění služby systému s cílovou otevřené nastavit počet z 9

- Gold – spuštění služby systému s cílovou otevřené nastavte počet 7

- Slovo stříbro – spuštění služby systému s cílovou otevřené nastavte počet 5

- Bronzová – spuštění služby systému s cílovou otevřené nastavit počet / 3

>[AZURE.NOTE] Spolehlivost osy, které zvolíte Určuje minimální počet uzly primární uzel typ musí mít. Vrstvy spolehlivost má žádný vliv na maximální velikosti clusteru. Abyste mohli mít 20 uzel clusteru, se systémem na bronzová spolehlivost.

 Je možné aktualizovat spolehlivost jedna úroveň cluster. Tím spustí upgrady clusteru potřeby změnit otevřené služby systému nastavte počet. Čekat na upgrade probíhá dokončete než budete dělat jakékoliv jiné změny obrázku, například přidat uzly atd.  Můžete sledovat průběh upgradu služeb struktury Průzkumníka nebo spuštěním [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Po dokončení plánování kapacity a nastavení obrázku, přečtěte si následující:
- [Služba struktury clusteru zabezpečení](service-fabric-cluster-security.md)
- [Úvod modelu stavu služby struktury](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
