<properties
   pageTitle="Vytvořit pravidlo založené na cesty pro brány aplikační pomocí portálu | Microsoft Azure"
   description="Naučte se vytvořit pravidlo založené na cesty pro brány aplikační pomocí portálu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Vytvořit pravidlo založené na cesty pro brány aplikační pomocí portálu

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-url-route-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-url-route-arm-ps.md)

Na základě cestu směrování adres URL umožňuje spojit postupy založené na adresu URL žádost Http. Zkontroluje, zda je směrování do back-end fondu nakonfigurován pro seznamy adresa URL aplikace brány a odešlete provozu v síti do fondu definovaný back-end. Společné použití aplikace založené na adrese URL směrování je načtení zůstatek žádosti o různých typů obsahu k jiné serveru back-end fondů.

Nový typ pravidla ho bráně aplikace založené na adrese URL směrování představuje. Aplikace brány má dva typy pravidel: základní a na základě cestu pravidla. Základní pravidla typ poskytuje kruhového služby pro fondy back-end při pravidel cestu kromě kruhového rozdělení, taky, která bude cesta vzor požadavku na adresu URL v úvahu při výběru fondu back-end.

## <a name="scenario"></a>Scénář

Následující scénář prochází vytváření pravidlo založené na cesty v existující brány aplikace.
Scénář předpokládá, že už jste postupovali podle kroků k [Vytvoření brány aplikační](application-gateway-create-gateway-portal.md).

![směrování adres URL][scenario]

## <a name="createrule"></a>Vytvořit pravidlo založené na cestu

Vytvořit pravidlo založené na cestu vyžaduje vlastní posluchače, než vytvoření pravidla je potřeba ověřit máte k dispozici posluchače používat.

### <a name="step-1"></a>Krok 1

Přejděte na http://portal.azure.com a vyberte existující brány aplikace. Klikněte na položku **pravidla**

![Přehled aplikací brány][1]

### <a name="step-2"></a>Krok 2

Klikněte na **základě cestu** s cílem přidat nové pravidlo založené na cesty.

### <a name="step-3"></a>Krok 3

**Přidat pravidlo založené na cestu** zásuvné má dvě části. První oddíl je místo, kam jste definovali posluchače, název pravidla a výchozí nastavení cestu. Výchozí nastavení cesta je pro postupy, které se nevztahuje vlastní postupu založených na cesty. Druhý oddíl zásuvné **pravidlo založené na cestu přidat** je, kde můžete definovat tato cesta pravidla sami.

**Základní nastavení**

- **Název** - popisný název pravidla, které je k dispozici na portálu.
- **Posluchače** – to je posluchače, který se používá pro dané pravidlo.
- **Výchozí back-end fond** - toto nastavení je nastavení, která definuje back-end pro výchozí pravidlo
- **Nastavení výchozí HTTP** – toto nastavení je nastavení, která definuje HTTP nastavení, aby se dá použít pro výchozí pravidlo.

**Pravidla založená na cestu**

- **Název** - popisný název pravidla na základě cestu.
- **Cesty** - toto nastavení určuje cestu, kterou pravidlo pro při vypadat předávání vysílání
- **Fond back** - toto nastavení je nastavení, která definuje back-end použijí pravidla
- **Nastavení protokolu HTTP** - toto nastavení je, který definuje nastavení HTTP se nemusí používat pravidla.

>[AZURE.IMPORTANT] Cesty: Seznam cestu vzorků podle. Každý musí začínat / a pouze blokování "\*" smí je na konci. Platné příklady /xyz /xyz* nebo /xyz/*.  

![Přidat pravidlo založené na cestu zásuvné s informacemi o vyplněnými][2]

Přidání pravidla na základě cestu k existující bráně aplikace je jednoduchý proces prostřednictvím portálu. Po vytvoření pravidla na základě cesta lze upravovat snadno přidat pravidla nehledá. 

![Přidání pravidla nehledá založené na cestu][3]

## <a name="next-steps"></a>Další kroky

Naučíte se konfigurovat SSL odstranění Azure aplikace brána najdete v článku [Konfigurace převzít SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png