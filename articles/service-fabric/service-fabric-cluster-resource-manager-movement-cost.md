<properties
   pageTitle="Správce služeb struktury clusteru zdroje: pohybu nákladů | Microsoft Azure"
   description="Přehled nákladů pohybu pro službu struktury služby"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Služba pohyb náklady ovlivňující správce prostředků clusteru voleb
Důležité faktor zvážit, pokud chcete zjistit, co se změní tak, aby clusteru a skóre řešení je celkové náklady dosažení řešení.

Přesunutí služby instancí nebo repliky náklady procesoru čas a síťové šířky pásma minimálně. Stavová služby ho taky nákladů, množství místa na disku, který je potřeba vytvořit kopii stav před ukončením staré repliky. Jasně by chcete minimalizovat náklady žádné řešení, které jsou správce prostředků clusteru struktury služby Azure součástí nahoru. Ale taky nechcete ignorovat řešení, která by mohl zlepšit výrazně přidělení zdrojů v clusteru.

Správce prostředků clusteru obsahuje dva způsoby výpočet nákladů a omezení, i když se pokusí ke správě clusteru podle svých cílů. První z nich je, že správce prostředků clusteru je plánování nové rozložení pro clusteru, počítá každý přesunout, které byste měli přesně. Jednoduchý případu najděte dva řešení s o stejné celkový zůstatek (výsledek) na konci potom pořiďte s nejnižší náklady (celkový počet přesune).

To funguje poměrně dobře. Pořád stejně jako výchozí nebo statické zatížení, pravděpodobně v libovolné složité systému rovnají všechny přesune. Některé by mohly být mnohem víc drahé.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Změna přesunout náklady otevřené a faktory, které je třeba zvážit
S sestav zatížení (Další funkce Správce prostředků obrázku), můžete dát službu způsob automatické vytváření sestav jak nákladné má služba přechod na určitý čas.

Kód:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost má čtyři úrovně: nula, minimum, střední a maximum. Toto jsou rozmístěné vůči sobě, s výjimkou nula. Nula znamená, že přesunutí otevřené je zadarmo a nesmí se započítávají do skóre řešení. Nastavení přesunout náklady na hodnotu Vysoká je *není* záruky, že otevřené nepřesune, právě, že ho nebudete přesouvat nejedná pádný důvod pro.

![Přesunutí náklady jako faktor při výběru repliky pro přesun][Image1]

MoveCost vám pomůže najít řešení, které způsobují nejnižších narušení celkové a jsou nejjednodušší dosáhnout při pořád doručovat odpovídající rozložení. Do služby pojmu náklady mohou být relativní hodně věcí. Nejběžnější faktory při výpočtu přesunout náklady jsou:

- Velikost státu nebo data, která službu má přesunout.
- Náklady na odpojení klientů. Náklady přesouvat primární otevřené jsou obvykle vyšší než náklady přesouvat sekundární otevřené.
- Náklady na přerušení letu operaci. Některé operace dat uloženy úroveň nebo jsou nákladné operace provést v odpovědi na volání klienta. Po určitém bodě nechcete je zastavit, pokud nemusíte. Takže po celou dobu operace, můžete dojde k nahoru náklady snížit pravděpodobnost, že služba otevřené nebo instance přesune. Po dokončení operace umístěte do normálního zobrazení.

## <a name="next-steps"></a>Další kroky
- Správce prostředků clusteru struktury služby používá metriky ke správě spotřebu a kapacita clusteru. Další informace o metriky a jak je nastavit, podívejte se na [Správa využití prostředků a načíst do služby struktury s metriky](service-fabric-cluster-resource-manager-metrics.md).
- Informace o tom, jak správce prostředků obrázku spravuje a zůstatky zatížení clusteru, podívejte se na [svůj cluster struktury služby Vyrovnávání](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
