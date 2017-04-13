<properties
   pageTitle="Plánování struktury služeb aplikací kapacity | Microsoft Azure"
   description="Popisuje, jak určit počet výpočetním uzly požadovaný pro služby struktury aplikace"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Plánování struktury služeb aplikací kapacity


V tomto dokumentu se naučíte k odhadu množství prostředků (procesory RAM úložištích) potřebujete k spuštění aplikace struktury služby Azure. Je běžné zdrojů požadavky na v průběhu času mění. Obvykle nastavíte, že několik zdroje jako vyvíjet nebo zkoušení služby a potom vyžadovat další materiály a přejděte do provozu aplikace roste v oblíbenosti. Při návrhu aplikace představit prostřednictvím dlouhodobé požadavky a zvolit, které umožňují služby zobrazit podle vysoké zákaznická služba.

 Při vytváření služeb struktury obrázku se rozhodnete, jaké druhy virtuálních počítačích (VMs) tvoří clusteru. Každý OM získáváte omezené množství zdrojů v podobě procesorech (jádra a rychlosti), šířka pásma, RAM a místa na disku. Narůstající velikostí služby v čase, můžete upgradovat na VMs, které nabízejí větší zdroje a/nebo přidejte další VMs do clusteru. Provedete tyto musí architektonické služby počáteční tak, aby ji mohli využívat nové VMs, které se dynamicky přidat clusteru.

Některé služby Správa malé o žádná data na VMs sami. Proto kapacita plánování těchto služeb by měly zaměřit především na výkonu, což znamená, vyberte příslušnou procesorů (jádra a rychlosti) VMs. Kromě toho měli byste zvážit šířka pásma, včetně frekvence sítě přenosy dochází přenášena množství zpracovávaných dat. Pokud potřebujete-li provést i zvýšení využití služeb služby, můžete přidat další VMs k obrázku a načíst zůstatek síťové požadavky přes všechny VMs.

Služby, které spravovat velké objemy dat na VMs plánování kapacity by měly zaměřit se především na velikost. Tím zvažte kapacita OM RAM a místa na disku. Virtuální paměť systému správy v systému Windows zajišťuje místa na disku vypadat RAM kódu aplikace. Kromě toho modul runtime služby struktury poskytuje inteligentní stránkování uchovávání jenom aktivní dat v paměti a přesunutí studenou dat na disku. Aplikace tedy používat více paměti, než je fyzicky k dispozici v OM. Máte další RAM jednoduše dojde ke zvýšení výkonu, protože OM můžete mít disku úložiště v RAM. OM, které vyberete by měla být dostatečně velký k tomu pro ukládání data, která se má v OM disk. Podobně OM měli dost RAM k poskytování s výkonem, které si přejete. Pokud vaše služba dat roste určitou dobu, můžete přidat další VMs obrázku a oddíl data přes všechny VMs.

## <a name="determine-how-many-nodes-you-need"></a>Zjistit, kolik potřebujete uzlů

Rozdělení služby umožňuje rozšiřování data vaše služby. Další informace o oddílech najdete v tématu [Rozdělení struktury služby](service-fabric-concepts-partitioning.md). Každý oddíl musí vešel do jednoho OM, ale více oddílů (malá), mohou být umístěny na jedné OM. Ano s více malých oddílů umožňuje větší flexibilitu než s několika větší oddíly. Nejvhodnější poměr je spousta oddíly zvyšuje režijních služby struktury a není možné provádět transakční mezi oddíly. Je taky další potenciální v síti Pokud kód služby často potřebuje přístup k použitelné části data, která live do různých oddílů. Při návrhu služby, pečlivě zvažte tyto výhody a nevýhody zahltit efektivní strategie rozdělení.

Předpokládejme, že má aplikace jednoho stavová služba, která má velikost úložiště, který očekáváte neomezenou DB_Size GB v roce. Chcete-li přidat další aplikace (a oddíly) jako zaznamenáte LOGLINTREND za tento rok.  Replikace faktorem (RF), která určuje počet repliky službě ovlivňuje celkový DB_Size. Celková DB_Size mezi ostatními je faktor replikace vynásobené DB_Size.  Node_Size představuje místo/RAM disk na uzel, který chcete použít pro službu. Pro dosažení nejlepších výsledků DB_Size by měl vešla do paměti clusteru a Node_Size, která je kolem RAM OM by měl zvolili. Pomocí přidělování Node_Size, která je větší než kapacitu paměti RAM, se spoléháte na stránkování poskytovanou modul runtime služby struktury. Tím, nemusí být výkon optimální Pokud celý dat se považuje za aktivní (od data je stránkování/se). Mnoho služby, kde je aktivní jenom část dat, je efektivnější.

Počtu uzlů potřebných pro maximální výkon můžete vypočítat následujícím způsobem:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Účet pro růst

Je vhodné výpočet počtu uzlů podle DB_Size, který očekáváte služby rozšířit, kromě DB_Size zadaných s. Potom postupně se zvětšují počtu uzlů narůstající velikostí služby tak, že není povolená zřizování počtu uzlů. Ale počet oddílů vycházet z počtu uzlů, které jsou potřebné při provozu služby při maximálního růstu.

Je dobré mít některé další počítače dostupné kdykoli tak, aby bylo možné zpracovat neočekávané vrcholy pole nebo selhání (například pokud několik VMs přejděte).  Během kapacitu navíc stanovit pomocí očekávané vrcholy pole, je výchozí bod rezervujte několik dalších VMs (5 až 10 % navíc).

Předchozí předpokládá jediná stavová služba. Pokud máte víc než jeden stavová služba, budete muset přidat DB_Size spojené s jinými službami na rovnici. Můžete taky můžete výpočet počtu uzlů zvlášť pro každou stavové službu.  Služby pravděpodobně repliky nebo oddílů, které nejsou Rovnováha. Mějte na paměti, že oddíly můžou mít taky další data než ostatní. Další informace o oddílech najdete v tématu [rozdělení článek doporučené postupy](service-fabric-concepts-partitioning.md). Však je definována vztahem předchozí oddíl a otevřené která, protože služby struktury zajistí, že replik jsou rozložené mezi uzly optimalizované způsobem.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Použití tabulky pro výpočet nákladů

Teď Pojďme umístění některých reálných čísel ve vzorci. [Příklad tabulky](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) ukazuje, jak plánovat kapacitu aplikace, který obsahuje tři typy dat objektů. Pro každý objekt jsme aproximaci počtu jeho velikost a kolik objekty že očekáváme mít. Jsme také vybrat kolik repliky chceme každého typu objektu. Tabulky vypočítá celkovou kapacitu paměti uložený v clusteru.

Zadejte jsme OM velikost a náklady na měsíční. Na základě velikosti OM tabulky se dovíte, minimální počet oddílů, které je nutné použít k rozdělení data fyzicky přizpůsobovat buňce na uzlů. Vyžadujete velký počet oddílů tak, aby zahrnoval konkrétní výpočtu aplikace a musí v síti. Tabulka zobrazuje počet oddílů, které spravujete objektů profilu uživatele zvýšil 1 až 6.

Teď založená na tyto informace, tabulka zobrazuje může fyzicky vám všechna data s požadované oddíly a repliky clusteru 26 uzel. Však tento cluster by hustě zabalit, tak je vhodné některé další uzly tak, aby zahrnoval selhání uzlů a inovací. Tabulky se zobrazí také, že máte víc než 57 uzly poskytuje žádné další hodnoty, protože byste měli prázdné uzlů. Chcete znovu přejít nad 57 uzly přesto tak, aby zahrnoval selhání uzlů a inovací. Si můžete doladit tabulky podle potřeby vaše aplikace.   

![Tabulka pro výpočet nákladů][Image1]



## <a name="next-steps"></a>Další kroky

Podívejte se na [rozdělení struktury služby služby] [ 10] zobrazíte další informace o vytváření oddílů služby.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
