<properties
    pageTitle="Pomocí Azure CDN CORS | Microsoft Azure"
    description="Naučte se používat Azure obsahu doručení Network (CDN) do s sdílení zdrojů mezi Origin (CORS)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Pomocí CORS Azure CDN     

## <a name="what-is-cors"></a>Co je CORS?

CORS (křížové Origin sdílení zdrojů) je funkce HTTP, který umožňuje webové aplikace spuštěné v části jednu doménu a získat informace v jiné doméně. Chcete omezit možnost útoky skriptování webů, všechny moderní webových prohlížečích implementovat zabezpečení omezení jmenoval [zásady stejné origin](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Tím na webovou stránku, volající API v jiné doméně.  CORS poskytuje zabezpečené tak a nechte jednu doménu (původní doméně) na volat rozhraní API v jiné doméně.
 
## <a name="how-it-works"></a>Jak to funguje
1.  V prohlížeči pošle žádost možnosti pod hlavičkou **Origin** HTTP. Toto záhlaví hodnotu domény, podávané množství nadřazené stránky. Když na stránku z https://www.contoso.com pokusí o přístup k datům uživatelů v domény fabrikam.com, by fabrikam.com posílat následující hlavičky: 
    
    `Origin: https://www.contoso.com`
 
2.  Server může odpoví následující:
    - **Access – ovládací prvek-povolit-Origin** záhlaví v odpovědi označující origin servery, které jsou povolené. Příklad:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Chybová stránka není-li serveru křížově origin žádosti
    - **Access – ovládací prvek-povolit-Origin** záhlaví s zástupný znak, který umožňuje všech domén:
        
        `Access-Control-Allow-Origin: *`
 
Složité požadavků HTTP je "výstupem" žádost o Hotovo první a zjistit, jestli má oprávnění před odesláním celá žádost.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Zástupný znak nebo jeden origin scénáře

Při **Přístupu – ovládací prvek-povolit-Origin** záhlaví nastavení zástupný znak (*) nebo jeden origin CORS na Azure CDN automaticky pracovat s žádnou další konfiguraci.  CDN bude mezipaměti první odpovědi a následné požadavky budou používat stejný záhlaví.
 
Pokud již byly provedeny žádosti o CDN před CORS nastavena na origin, budete muset vymazání obsahu na koncový bod obsah tak, aby nové načtení obsahu v záhlaví **Access – ovládací prvek-povolit-Origin** .
 
## <a name="multiple-origin-scenarios"></a>Více origin scénářů

Pokud budete muset povolit seznam určitých typů povolit CORS, co potřebujete trochu složitější. K problému dochází při CDN uloží **Access – ovládací prvek-povolit-Origin** záhlaví první CORS origin.  Při různých origin CORS požadavek na pozdější, bude CDN podávané množství režim cached **Access – ovládací prvek-povolit-Origin** záhlaví, které neodpovídají.  Tento problém můžete vyřešit několika způsoby.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium z Verizon

Nejlepší způsob, jak povolit to je použití **Azure CDN Premium z Verizon**, která poskytuje některé rozšířené funkce. 
 
Musíte k [Vytvoření pravidla](cdn-rules-engine.md) ke kontrole **Origin** záhlaví na žádost.  Pokud je platný origin, pravidla sada záhlaví **Access – ovládací prvek-povolit-Origin** se origin uvedené v pozvánce.  Původ podle záhlaví **Origin** není povolená, pravidla by měl vynecháte **Access – ovládací prvek-povolit-Origin** hlavičky, která bude mít za následek prohlížeče odmítnout žádost. 
 
Existují dva způsoby, jak to provést pomocí modul pravidla.  V obou případech úplně ignoruje záhlaví **Access – ovládací prvek-povolit-Origin** ze serveru původ souboru, stroj pravidel CDN úplně spravuje povolené CORS typů.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Jeden regulárních výrazů s všechny platné typů
 
V tomto případě vytvoříte regulární výraz, který obsahuje všechny typů, kterou chcete povolit: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure CDN z Verizon** používá [Perl kompatibilní regulární výrazy](http://pcre.org/) jako jeho modul regulární výrazy.  Nástroj stejně jako [běžný 101 výrazů](https://regex101.com/) slouží k ověření vašich regulárních výrazů.  Všimněte si, že znaků "/" platí v regulární výrazy a není nutné však úniku znak je považován za doporučený postup a očekává tak, že některé regex validátory.

Shoduje s regulárních výrazů, pravidla nahradí **Access – ovládací prvek-povolit-Origin** záhlaví (pokud existuje) ze zdroje s origin, který odeslal požadavek.  Můžete taky přidat další CORS záhlaví, například **Aplikace Access – ovládací prvek-povolit-metody**.

![Příklad pravidla s regulárních výrazů](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Požádat o záhlaví pravidlo pro každý origin.

Místo regulárních výrazů můžete místo toho vytvoříte samostatný pravidlo pro každý původ, jestli že chcete povolit používání **Zástupných požádat o záhlaví** [splňují podmínku](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Stejně jako metodu regulárních výrazů modul pravidel samotný nastaví CORS záhlaví. 
  
![Příklad pravidel bez regulárních výrazů](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] V příkladu výše, použijte zástupný znak * říká modul pravidla podle HTTP a HTTPS.
 
### <a name="azure-cdn-standard"></a>Standardní Azure CDN

Na Azure CDN standardní profily pouze mechanismus povolit více typů bez použití zástupných origin je použití [mezipaměti řetězce dotazu](cdn-query-string.md).  Budete muset povolit nastavení řetězce dotazu pro koncový bod CDN a potom použijte řetězec jedinečný dotazu na žádosti o každé povolené domény. Tím bude mít za následek CDN ukládání do mezipaměti samostatný objekt pro každý řetězec jedinečný dotazu. Tento postup však není ideální, jak je výsledkem je více kopií stejného souboru do zprávy CDN mezipaměti.  

