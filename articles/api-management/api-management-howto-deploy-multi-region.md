<properties
    pageTitle="Nasazení instanci služby Azure rozhraní API správy do několika Azure oblastí"
    description="Naučte se nasadit instanci služby Azure rozhraní API správy do několika Azure oblastí." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Nasazení instanci služby Azure rozhraní API správy do několika Azure oblastí

Správa rozhraní API podporuje více oblastí nasazení, což umožňuje rozhraní API vydavatelé k distribuci jediná služba Správa rozhraní API přes libovolný počet požadované Azure oblastí. Díky snížit žádost latence osoba geograficky distributed spotřebitele rozhraní API a také zlepšuje dostupnost služby pokud jednom regionu přejde do režimu offline. 

Po vytvoření rozhraní API správy služby počáteční obsahuje pouze jednu [jednotku][] a umístěná v jedné oblasti Azure, který je označen jako primární oblast. Další oblastí můžete snadno přidat s použitím klasické portál Azure. Nasazení serveru brány pro správu rozhraní API pro každou oblast a k nejbližšímu brány bude směrovat přenosy volání. V případě oblasti přenos je automaticky znovu řízené další nejbližší brány. 

> [AZURE.IMPORTANT] Více oblastí nasazení je dostupná jenom ve vrstvě **[Premium][]** .

## <a name="add-region"> </a>Nasazení instanci služby správy rozhraní API pro novou oblast

Začněte tím, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

Přejděte na kartu **Měřítko** Azure klasické portálu pro instanci služby správy rozhraní API. 

![Kartu Měřítko][api-management-scale-service]

Abyste mohli nasadit na novou oblast, klikněte v rozevíracím seznamu pod primární oblast a vyberte oblast ze seznamu.

![Přidání oblasti][api-management-add-region]

Po výběru oblasti vyberte počet jednotek pro novou oblast z rozevíracího seznamu jednotku.

![Určení jednotky][api-management-select-units]

Jakmile se požadované oblasti a jednotky jsou nakonfigurovány, klikněte na **Uložit**.

## <a name="remove-region"> </a>Instanci služby správy rozhraní API odstranit z oblasti

Z oblasti odebrat instanci služby správy rozhraní API, přejděte na kartu **Měřítko** Azure klasické portálu pro instanci služby správy rozhraní API. 

![Kartu Měřítko][api-management-scale-service]

Klikněte na **X** napravo od požadovanou oblast odebrat.  

![Odebrání oblasti][api-management-remove-region]

Jakmile odeberete požadované oblasti, klikněte na **Uložit**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Začít používat správu rozhraní API Azure]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[jednotky]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

