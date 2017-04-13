<properties
   pageTitle="Správce prostředků služby struktury clusteru – skupiny aplikací | Microsoft Azure"
   description="Základní informace o funkci skupiny aplikace v struktury clusteru zdroje Správce služeb"
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

# <a name="introduction-to-application-groups"></a>Úvod ke skupinám aplikace
Správce prostředků clusteru služby struktury obvykle spravuje prostředky obrázku tak, že načíst (představující prostřednictvím metriky) rovnoměrně v celém clusteru. Služba struktury také spravuje kapacita uzlů v clusteru a clusteru jako celek prostřednictvím pojmu kapacity. To funguje výborně pro mnoho různých typů úloh, ale vzorce, které usnadňují použití různých instancí aplikace služby struktury někdy zkrácení požadavky na další. Některé další požadavky jsou obvykle:

- Možnost rezervovat kapacitu služby Instance aplikace na některé počtu uzlů
- Omezit celkový počet uzlů povolených spustit danou sadu služby v aplikaci
- Definování kapacity na samotné instanci aplikace za účelem omezení počtu nebo využití prostředků celkové služeb uvnitř

Abyste mohli tyto požadavky splnit, budeme vyvinutý podpora označujeme skupiny aplikací.

## <a name="managing-application-capacity"></a>Správa aplikací kapacity
Kapacita aplikace mohou sloužit k omezení počtu uzlů rozložený aplikací, stejně jako celkový metrických zatížení, instance aplikací v jednotlivých uzlech. Je taky slouží k rezervaci zdrojů v obrázku pro aplikaci.

Můžete nastavit kapacity aplikace pro nové aplikace při vytvoření; Můžete se taky aktualizovaly pro existující aplikace, které byly vytvořené bez zadání kapacity aplikace.

### <a name="limiting-the-maximum-number-of-nodes"></a>Omezení maximální počet uzlů
Nejjednodušší případu použití aplikace kapacity při instanci aplikace musí být omezená na určitý maximální počet uzlů. Pokud je zadána žádná kapacita aplikace, vytvoříte Správce služby struktury clusteru instanci repliky podle normální pravidel (vyrovnávání nebo defragmentace), které obvykle to znamená, že jeho služeb rozšíří ve všech dostupných uzlů v clusteru, nebo pokud defragmentace je zapnutá některé libovolné ale menší počtu uzlů.

Následující obrázek znázorňuje potenciální umístění instance aplikace bez maximální počet uzlů definované a nastavte potom stejné aplikaci maximální počet uzlů. Všimněte si, že je žádné záruky, o které repliky nebo pro případy, kdy bude společně umístěny služby.

![Definování maximální počet uzlů Instance aplikace][Image1]

V levém příkladu aplikace nemá aplikace kapacity nastavení a má tři služby. CRM připíše logické rozhodnutí o roztažení ostatními přes šest uzly k dispozici pro dosažení nejlepších zůstatek clusteru. V pravém příkladu vidíme stejné aplikaci, která je omezen na tři uzly a kde Service struktury CRM dosáhla nejlepší zůstatek kopie aplikace služeb.

Parametr určuje toto chování se nazývá MaximumNodes. Tento parametr lze nastavit při tvorbě aplikace nebo aktualizovat pro instance aplikace která byla spuštěná, ve kterém případ Service struktury CRM omezit kopie aplikace služby definovaný maximální počet uzlů.

Prostředí PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Metriky aplikace, načítání a kapacity
Skupiny aplikací umožňují definovat metriky přidružených k dané aplikace instance i kapacita uplatnění tyto metriky. Tak například můžete definovat, jako mnoho služeb požadovaného může být vytvořeny v

Každý metriky jsou 2 hodnoty, které můžete nastavit, aby popisují kapacitu pro tuto instanci aplikace:

-   Celková aplikace kapacita – představuje celkovou kapacitu žádosti o příslušné metrice. Služba struktury CRM se bude snažit k omezení součet metrických zatížení této aplikace služeb zadanou hodnotu; Kromě toho pokud aplikace služby už zabírají zatížení až toto omezení, správce prostředků clusteru struktury služby zakázat vytváření nové služby nebo oddílů, které způsobí celkové zatížení přejdete myší toto omezení.
-   Nejvyšší uzel kapacita – určuje maximální celkové zatížení kopie aplikací služby načítání. Pokud celkové zatížení uzel přejde myší kapacita, pokusí se Service struktury CRM posunout, repliky do jiných uzlů nerespektuje omezení kapacity.

## <a name="reserving-capacity"></a>Rezervace kapacity
Jiné běžné skupin aplikace slouží zajistit, že zdroje v rámci clusteru rezervováno pro instanci dané aplikace, přestože nemá instance aplikace služby v ní ještě nebo i v případě, že nejsou zatím jinými zdroje. Podívejme se na která by fungování.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Určení minimální počet uzly a rezervaci prostředku
Rezervace zdrojů pro instance aplikace vyžaduje zadání pár další parametry: *MinimumNodes* a *NodeReservationCapacity*

- MinimumNodes - stejně jako určující cílové maximálního počtu uzlů, které se, mohlo by umožnit spuštění služby v aplikaci můžete taky určit minimální počet uzlů, které by měla běžet aplikace. Toto nastavení efektivně definuje počtu uzlů, které zdroje se budou rezervovat na přinejmenším zajištění kapacity v rámci obrázku jako součást vytváření instance aplikace.
- NodeReservationCapacity - NodeReservationCapacity je to možné definovat každý metriky aplikace. Tato možnost definuje množství metrických zatížení vyhrazená pro aplikaci uzlu umístění žádné replice nebo instancí služby v něm obsažené.

Podívejme se na příklad kapacita rezervace:

![Definování rezervované kapacity instancí aplikace][Image2]

V levém příkladu aplikace nemají kteréhokoliv aplikace definované. Správce prostředků clusteru struktury služby bude zůstatek instance spolu s aplikací z jiných služeb (mimo aplikaci) a podřízené služby repliky aplikace zajistit zůstatek clusteru.

V příkladu na pravé straně Dejme tomu, aby byl vytvořen aplikace MinimumNodes nastavit hodnotu 2, 3 a aplikace míru tato pole definovány rezervace 20, maximální kapacitou na uzel 50 a celkové aplikace objemu 100, MaximumNodes služby struktury rezervuje kapacitu na dvou uzlů modré aplikace a nepovolí replikami v clusteru používání této funkci. Kapacita rezervovaná aplikace se považuje spotřebované a spočítané týkající se zbývající kapacita obrázku.

Po vytvoření aplikace s rezervace správce prostředků clusteru rezervuje kapacitu rovno MinimumNodes * NodeReservationCapacity v clusteru, ale nebude rezervovat kapacitu na konkrétní uzly tak, aby vytvořili a umístí kopie aplikace služeb. Tato funkce umožňuje flexibilitu, protože uzly jsou zvolené pro nové repliky pouze kdy se vytvářejí. Kapacita je rezervován určitého uzlu při alespoň jeden otevřené ho.

## <a name="obtaining-the-application-load-information"></a>Získat informace o aplikaci zatížení
Pro každou aplikaci můžete, že má aplikace kapacita definované můžete získat informace o agregační načíst vykázaného kopie svých služeb. Služba struktury poskytuje prostředí PowerShell a spravovaným rozhraním API dotazy k tomuto účelu.

Například zatížení lze získat prostřednictvím následující rutinu Powershellu:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Výstup dotazu bude obsahovat základní informace o aplikaci kapacita určený pro aplikaci, třeba uzly minimální a maximální počet uzlů. Také budou informace o počtu uzlů, které aktuálně pomocí aplikace. Proto metriky každý zatížení bude informace o:
- : Metriky název metriky.
-   Rezervace kapacita: Clusteru rezervace kapacity clusteru k této aplikaci.
-   Načtení aplikace: Celkové zatížení replik podřízené této aplikace.
-   Kapacita aplikaci: Maximální povolenou hodnotu zatížení aplikace.

## <a name="removing-application-capacity"></a>Odebrání aplikace kapacity
Po parametry kapacita aplikace se nastaví pro aplikaci, můžete být odeberou používání rutin rozhraní API aplikace aktualizace nebo Powershellu. Příklad:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Tento příkaz odeberete všechny parametry kapacita aplikace z aplikace a služby struktury clusteru zdroje správce spustí replikace této aplikace jako jakékoli jiné aplikace v obrázku, který nemá tyto parametry definované. Příkazu dosáhnete okamžité a správce prostředků clusteru budou odstraněny všechny parametry aplikace kapacitu k této aplikaci; určení znovu vyžadují rozhraní API aplikace aktualizace pro volání s příslušnými parametry.

## <a name="restrictions-on-application-capacity"></a>Omezení aplikace kapacity
Existuje několik omezení parametry kapacita aplikace, které musí být zachovány. V případě chybových zpráv funkce ověření vytvoření nebo aktualizace aplikace odmítnuta zobrazí se chybová zpráva.
Všechny parametry celé číslo musí být nezáporné čísla.
Kromě toho pro jednotlivé parametry omezení jsou následujícím způsobem:

-   MinimumNodes nikdy musí být větší než MaximumNodes.
-   Pokud jsou definovány kapacity metriky zatížení, je nutné dodržujte tato pravidla:
  - Rezervace kapacity uzel nesmí být větší než maximální uzel kapacity. Nelze například omezit kapacita metriky "Procesoru" na uzel 2 jednotky a zkuste rezervujte 3 jednotky v jednotlivých uzlech.
  - MaximumNodes není zadán, součin MaximumNodes a nejvyšší uzel kapacita nesmí být větší než celkovou kapacitu aplikace. Například pokud nastavíte nejvyšší uzel kapacita metriky zatížení, které "Procesoru" 8 a můžete nastavit maximální počet uzlů až 10, pak celkovou kapacitu aplikace musí být vyšší než 80 metriky tento zatížení.

Omezení vynucuje při tvorbě aplikace (na straně klienta) i při aktualizaci aplikace (na straně serveru). Při vytváření, jedná se o příklad vymazat porušení požadavky od MaximumNodes < MinimumNodes a příkaz selže v klientovi i odesílá žádost o službu struktury clusteru:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Příklad Neplatná aktualizace je následujícím způsobem. Pokud nám prohlédněte existující aplikace a aktualizovat maximální uzly s nějakou hodnotou, bude předávat aktualizace:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Až to jsme pokus o aktualizaci minimální uzly:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Klient nemá dost kontextu o aplikaci tak, aby vám umožní aktualizace předat clusteru struktury služby. V clusteru, ale struktury služby ověří nový parametr společně s existující parametry a operace aktualizace se nepovede, protože minimální uzly nepřítel hodnota je větší než hodnota pro maximální počet uzlů. V tomto případě parametry kapacita aplikace se nezmění.

Tato omezení se budou zobrazovat na místě v pořadí pro clusteru správce prostředků mohli dát nejlepší umístění pro repliky služeb aplikací.

## <a name="how-not-to-use-application-capacity"></a>Jak nepoužít aplikace kapacity

-   Nepoužívejte kapacita aplikace Pokud chcete omezit aplikace dílčí uzlů: Ačkoli struktury služby zajistí dodržování maximální počet uzlů pro jednotlivé aplikace, která má kapacitu aplikace uvedené, uživatelé nelze rozhodnout uzlů, které se bude vytvořena ve. Toho můžete dosáhnout pomocí umístění omezení služeb.
-   Nepoužívejte kapacita aplikace zajistit, že dvou službách z stejné aplikace se vždy umístěny vedle sebe. Můžete dosáhnout pomocí spřažení vztah mezi službami a spřažení můžete provádět pouze služby, které by měly být umístěny ve skutečnosti společně.

## <a name="next-steps"></a>Další kroky
- Pro další informace o dalších možností konfigurace služeb podívejte se na téma na ostatní clusteru Správce konfigurace k dispozici [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)
- Najdete informace o tom správce prostředků clusteru spravuje a zůstatky náklad v clusteru podívejte se na článek o [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Začít od začátku a [Úvod do služby struktury clusteru správce zdrojů](service-fabric-cluster-resource-manager-introduction.md)
- Další informace o fungování metriky obecně důkladně metrice [zatížení struktury služby](service-fabric-cluster-resource-manager-metrics.md)
- Správce prostředků clusteru má spoustu možností pro popis clusteru. Zjistit další informace o jejich najdete v tématech tohoto článku na [popisující služby struktury obrázku](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
