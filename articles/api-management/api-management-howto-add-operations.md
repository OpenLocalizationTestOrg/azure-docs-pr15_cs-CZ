<properties 
    pageTitle="Jak přidat operace API správy API Azure | Microsoft Azure" 
    description="Naučte se přidávat operace rozhraní API správy rozhraní API Azure." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Jak přidat operace rozhraní API správy rozhraní API Azure

Před použitím rozhraní API správy rozhraní API, musí být přidán operace. Tato příručka ukazuje, jak přidat a nakonfigurovat různé typy operace rozhraní API správy rozhraní API.

## <a name="add-operation"> </a>Přidáte operaci

Operace se přidá a nakonfigurované tak, aby rozhraní API v portálu aplikace publisher. Přístup k portálu Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

Vyberte požadované rozhraní API na portálu publisher a klikněte na kartu **operace** . 

![Operace][api-management-operations]

Klikněte na **Přidat operace** přidáte novou operaci. Zobrazí se **Nová operace** a na kartu **podpis** budou vybrané ve výchozím nastavení.

![Přidání operace][api-management-add-operation]

Zadejte **příkaz HTTP** výběru z rozevíracího seznamu.

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

Definování adresy URL šablony zadejte část adresy URL obsahující segmentu cesta URL a žádný nebo více parametrů řetězce dotazu. Adresa URL šablony přidaným k základní adresu URL rozhraní API identifikuje jedné operace HTTP. Může obsahovat jednu nebo více pojmenovaných proměnné části, které jsou označeny složené závorky. Následující proměnné části nazývají parametry šablony a dynamicky přiřazené hodnoty extrahovaných z adresy URL žádost o při žádosti zpracovávání správy rozhraní API platformy.

![Adresa URL šablony][api-management-url-template]

<a name="rewrite-url-template"></a>

Pokud budete chtít, určete **přepisu URL šablony**. Umožňuje použít standardní URL šablony pro zpracování příchozí žádosti o front-end, při volání back-end prostřednictvím převedené adresy URL podle šablony revize. Parametry šablony v šabloně adresa URL bude použito v šabloně revize. Následující příklad ukazuje, jak obsah kódovaný jako segmentu cesty webové služby z předchozího příkladu lze zadat jako parametr dotazu v rozhraní API publikovaných pomocí správy rozhraní API platformy pomocí adresy URL šablony.

![Adresa URL šablony revize][api-management-url-template-rewrite]

Volající operace použije formát `/customers?customerid=ALFKI` a to bude připojen k `/Customers('ALFKI')` při volání back-end služby.


**Zobrazovaný** název a **Popis** zadejte popis operace a slouží k poskytnutí si přečtěte následující dokumentaci pro vývojáře pomocí tohoto rozhraní API v portálu pro vývojáře.

![Popis][api-management-description]

Popis operace může být zadán jako prostého textu nebo HTML v textovém poli **Popis** .

## <a name="operation-caching"> </a>Operace ukládání do mezipaměti

Ukládání do mezipaměti odpověď snižuje zpoždění vnímat rozhraní API spotřebitele, snižuje šířku pásma a snížení zatížení HTTP webové služby provádění rozhraní API. 

Umožňuje snadno a rychle ukládání do mezipaměti pro operaci, vyberte kartu **ukládání do mezipaměti** a zaškrtněte políčko **Povolit** .

![Ukládání do mezipaměti][api-management-caching-tab]

**Doba trvání** Určuje časové období, po kterou odpověď operace zůstane v mezipaměti. Výchozí hodnota je 3600 sekund nebo 1 hodinu.

Mezipaměť klíče slouží k odlišit odpovědi tak, aby odpověď odpovídající každý klíč různých mezipaměti dostane vlastní samostatný režim cached hodnotu. V případě potřeby zadejte řetězec parametry specifické dotazu a/nebo záhlaví HTTP se nemusí používat v výpočetních mezipaměti klíčových hodnot v textových polích **měnit podle parametrů řetězce dotazu** a **měnit podle záhlaví** v tomto pořadí. Pokud nejsou žádné zadaný, úplné požadavku na adresu URL a následující hodnoty záhlaví HTTP se používají v mezipaměti generování klíče: **přijmout** a **Znaková sada přijmout**.

>Další informace o ukládání do mezipaměti a ukládání do mezipaměti zásad najdete v článku [jak výsledky do mezipaměti operace správy rozhraní API Azure][].


## <a name="request-parameters"> </a>Požádat o parametry

Parametry operace je spravováno na kartu parametry. Parametry zadanou v **Adrese URL šablony** na kartě **podpis** se automaticky přidají a mohou měnit pouze úpravy adresy URL šablony. Další parametry můžete zadat ručně.

Přidání nového dotazu parametr, klikněte na **Přidat parametr dotazu** a zadejte následující informace:

-   **Název** - název parametru.
-   **Popis** : stručný popis parametru (volitelné).
-   **Typ** – typ parametru vybraná v seznamu dolů.
-   **Hodnoty** – hodnoty, které lze přidělit tento parametr. Jedna z hodnot můžete označit jako výchozí (volitelné).
-   **Povinné** : Zkontrolujte parametr povinné zaškrtnutím políčka. 

![Žádost o parametry][api-management-request-parameters]

## <a name="request-body"> </a>Požádat o textu

Pokud to umožňuje operace (například umístění, příspěvek) a vyžaduje textu poskytnete příklad všechny podporované číselné formáty (například json, XML). 

>Žádost o textu se používá si přečtěte následující dokumentaci pouze pro účely a není ověřit.

Zadat text žádosti, přejděte na kartu **text** .

Klikněte na **Přidat znázornění**, začněte psát jméno požadovaný typ obsahu (například aplikace/json), vyberte v rozevíracím seznamu a vložte příklad textu požadované žádost ve vybraném formátu do textového pole. 

![Žádost o textu][api-management-request-body]

V dalších formátovaná jako, můžete taky určit popis Nepovinný text v textovém poli **Popis** .

## <a name="responses"> </a>Odpovědi

Je vhodné pro všechny stavů, které mohou způsobit operace jsou uvedeny příklady odpovědi. Každý stavový kód může mít víc odpovědí textu například jeden pro každou z podporovaných typů obsahu. 

Pokud chcete přidat odpověď, klikněte na tlačítko **Přidat** a začněte psát požadovaný stavový kód. V tomto příkladu je stavový kód **200 OK**. Po zobrazení Kód v rozevíracím seznamu vyberte ho a kód odpovědi vytvořené a přidané do operaci.

![Kód odpovědi][api-management-response-code]

Klikněte na **Přidat znázornění**, začněte psát jméno požadovaný typ obsahu (například aplikace/json) a potom vyberte v rozevíracím seznamu.

![Typ obsahu textu][api-management-response-body-content-type]

Vložení textu příklad odpověď bezprostředně do textového pole. 

![Odpověď textu][api-management-response-body]

Pokud budete chtít, přidáte do textového pole **Popis** nepovinný popis.

Po konfiguraci operace, klikněte na **Uložit**.


## <a name="next-steps"> </a>Další kroky

Jakmile se operace se přidají do rozhraní API, dalším krokem je rozhraní API přidružit k produktu a potom ji publikujte tak, aby vývojáři upoutat operací.

-   [Jak vytvořit a publikovat k produktu][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Začít používat správu API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Jak vytvořit a publikovat k produktu]: api-management-howto-add-products.md
[Jak výsledky do mezipaměti operace správy API Azure]: api-management-howto-cache.md