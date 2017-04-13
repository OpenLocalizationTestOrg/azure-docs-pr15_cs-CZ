<properties
    pageTitle="Vytvoření služby Azure vyhledávání pomocí portálu Azure | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Zjistěte, jak zřídit služby Azure vyhledávání pomocí portálu Azure."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Vytvoření služby Azure vyhledávání pomocí portálu Azure

Tato příručka vás provede jednotlivými proces vytvoření (nebo zřizovací) služby Azure vyhledávání pomocí [Portálu Azure](https://portal.azure.com/).

Tato příručka předpokládá, že už máte předplatné Azure a můžete se přihlásit k portálu Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Najít Azure hledání na portálu Azure
1. Přejděte na [Portál Azure](https://portal.azure.com/) a přihlaste se.
1. Klikněte na znaménko plus ("a") v levém horním rohu.
2. Vyberte **Data + úložiště**.
3. Vyberte **Hledat Azure**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Vyberte název služby a koncový bod adresy URL pro službu
1. Vaše jméno služby bude část adresy URL koncového bodu služby Azure hledání proti němuž změníte vzhled rozhraní API volání na spravovat a používat službu hledání.
2. Do pole **Adresa URL** zadejte své jméno služby. Název služby:
  * musí obsahovat pouze malá písmena, číslice pomlček nebo prázdných buněk ("-")
  * nelze použít pomlčku ("-") jako první 2 znaky nebo poslední znak
  * nesmí obsahovat po sobě jdoucí pomlčky ("-")
  * je omezené mezi 2 až 60 znaků


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Vyberte předplatné, které bude držet služby
Pokud máte víc předplatných, můžete vybrat, který z nich bude obsahovat tuto službu Azure vyhledávání.

## <a name="select-a-resource-group-for-your-service"></a>Vyberte skupinu zdrojů službě
Vytvoření nové skupiny prostředků nebo vyberte stávající. Skupina zdroje je kolekce Azure služeb a zdroje informací, které se používají společně. Například pokud používáte indexovat databázi SQL Azure vyhledávání, pak oba z těchto služeb musí být součástí stejné skupiny prostředků.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Vyberte umístění hostitelem služby
Jako služby Azure Azure vyhledávání je k dispozici hostitelem datacentrech ve světě. Dejte pozor, které [se můžou lišit ceny](https://azure.microsoft.com/pricing/details/search/) zeměpisná oblast.

## <a name="select-your-pricing-tier"></a>Vyberte ceny osy
[Hledání Azure je aktuálně k dispozici v více ceny úrovní](https://azure.microsoft.com/pricing/details/search/): bezplatnou základní nebo standardní. Jednotlivé vrstvy má vlastní [funkce a omezení](search-limits-quotas-capacity.md). Pokyny najdete v článku [Volba ceny osy nebo SKU](search-sku-tier.md) .

V tomto případě jsme vybrali jste standardní osy naší služby.

## <a name="select-the-create-button-to-provision-your-service"></a>Vyberte tlačítko "Vytvořit" zřízení služby

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Změna velikosti služby

Po zřízení služby je možné přizpůsobit tak, aby vyhovovala vašim potřebám. Pokud jste zvolili standardní vrstvy pro službu Azure hledání, můžete změnit měřítko služby v dvojrozměrné: repliky a oddíly. Pokud jste zvolili základní osy, můžete přidat jenom repliky.

*__Oddíly__* povolení služby pro ukládání a hledání pomocí dalších dokumentů.

*__Repliky__* povolení služby pro zpracování vyšší zatížení vyhledávacích dotazů - [službu vyžaduje 2 repliky dosáhnout SLA jen pro čtení a vyžaduje 3 repliky dosáhnout SLA pro čtení i zápis](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Přejděte na zásuvné správy Azure vyhledávací služby na portálu Azure.
2. V **Nastavení** zásuvné vyberte **Měřítko**.
3. Služby je možné přizpůsobit přidáním repliky nebo oddílů.
  * Nemůžete změnit velikost služby dřívější 36 hledání jednotky. Celkový počet jednotek hledání je produkt repliky a oddíly (repliky * oddíly = celkový počet jednotek hledání).
  * Pokud jste zvolili základní osy, je možné přizpůsobit pouze pro 3 repliky. Základní služby je vázaný na jeden oddíl.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Další
Po zajištění služby Azure vyhledávací službu, budete moci k [definování indexu vyhledávání Azure](search-what-is-an-index.md) , můžete odeslat a vyhledávat data.

Stručný návod najdete v článku [Začínáme s Azure hledání na portálu](search-get-started-portal.md) .
