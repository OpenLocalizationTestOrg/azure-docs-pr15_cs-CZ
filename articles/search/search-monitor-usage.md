<properties 
   pageTitle="Sledovat použití a statistiky služby Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu" 
   description="Sledování zdroje spotřebu a index velikost mají být vyhledávány Azure, hostovanou cloudu vyhledávací služby na Microsoft Azure." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Sledovat použití a statistiky v Azure vyhledávací služby

Sledování růst indexy a velikost dokumentu vám můžou pomoct včasným upravit kapacita před zasažení horní mez vytvořené službě. 

Sledování využití prostředků, počty a statistiky službě jsou snadno zobrazit na [Portál Azure](https://portal.azure.com), ale také můžete získat informace o programově Pokud vytváříte nástroj pro správu vlastní služby. Tento článek popisuje, jak pro obě metody.

Můžete taky pomocí nové funkce analýzy přenosy hledání podstatu aktivitu na úrovni index. Navštivte [Analýzy přenosy hledání mají být vyhledávány Azure](search-traffic-analytics.md) začít pracovat.

##<a name="view-counts-and-metrics-in-the-portal"></a>Zobrazení počtu a metriky na portálu 

1. Přihlaste se k [portálu Azure](https://portal.azure.com). 

2. Otevřete řídicí panel služeb Azure vyhledávací služby. Dlaždice pro službu se nachází na domovské stránce nebo ve službě můžete přecházet z Procházet v JumpBar. Podrobné pokyny najdete v článku [vytvoření služby](search-create-service-portal.md) .

V části použití obsahuje metr, která sděluje, jaká část dostupné zdroje jsou v současnosti se používá.

  ![][1]

Odvolání sdílených služeb má maximálně jedno otevřené a každý oddíl. Navíc ji podporuje pouze 10 000 dokumenty v úplném nebo 50 MB dat, podle toho, co je v ní dřív.

##<a name="get-index-statistics-using-the-rest-api"></a>Získání indexy statistiky pomocí rozhraní REST API

Rozhraní REST API pro hledání Azure a .NET SDK zpřístupnit programové metriky služby.  Pokud používáte [indexování](https://msdn.microsoft.com/library/azure/dn946891.aspx) načíst rejstřík z databáze SQL Azure nebo DocumentDB, další rozhraní API je možné získat čísla, která nastavíte, že. 

  + [Získání statistických údajů indexu](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Počet dokumentů](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Stav indexování](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Další kroky

Prohlédněte si [limity a kapacitu](search-limits-quotas-capacity.md) k určení kombinací oddíly a repliky, které budete potřebovat pokud nestačí stávající kapacity. 

[Spravovat vyhledávací služby na Microsoft Azure](search-manage.md) naleznete další informace o správě služby.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
