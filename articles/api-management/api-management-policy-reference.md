<properties 
    pageTitle="Odkaz na Azure rozhraní API správy zásad" 
    description="Informace o dostupných konfigurace rozhraní API správy zásad." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Odkaz na Azure rozhraní API správy zásad

Tato část obsahuje rejstříku pro zásady [odkaz zásad správy rozhraní API][]. Informace o přidání a konfigurace zásad najdete v tématu [zásady správy rozhraní API][].

Zásady výrazů lze použít jako hodnoty atributu nebo textové hodnoty v některém z zásady správy rozhraní API, dokud zásadu určuje jinak. Několik zásad například zásady [řízení toku][] a [nastavit proměnnou][] vycházejí zásad výrazů. Další informace najdete v tématu [Upřesnit zásady][] a [zásady výrazy][]

## <a name="policy-reference-index"></a>Index odkaz zásad

-   [Zásady omezení přístupu][]
    -   [Kontrola HTTP záhlaví][] – vynucuje existenci a/nebo přínosu HTTP záhlaví.
    -   [Limit volání sazba tak, že předplatné][] – použití zabrání rozhraní API špičky omezením volání sazbu na základě předplatného za.
    -   [Omezení volání sazba klíčem](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - použití zabrání rozhraní API špičky omezením volání úroková_sazba, na základě jednoho klíče.
    -   [Omezit volající IP adresy][] - filtry (umožňuje/zakazuje) hovory z konkrétní IP adresy a/nebo rozsahy adres.
    -   [Nastavení kvóta využití prostředků tak, že předplatné][] – umožňuje vynutit obnovitelné nebo životnost volání hlasitost nebo šířku pásma kvóty, na základě předplatného za.
    -   [Nastavení kvóta využití prostředků klíčem](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) – umožňuje vynutit obnovitelné nebo životnost volání hlasitost nebo šířku pásma kvóty, na základě jednoho klíče.
    -   [Ověření JWT][] - vynucuje existenci a platnost JWT extrahovaných z určité záhlaví HTTP nebo parametru zadaném dotazu.
-   [Rozšířené zásady][]
    -   [Řízení toku][] – podmíněně platí zásad výkazy založené na výsledky vyhodnocení logické [výrazy][].
    -   [Přeposílání žádost][] - přepošle žádost o službu back-end.
    -   [Protokol událostí centrální][] – odešle zprávy ve formátu zadaný do cíle zprávy definovaného [protokolování](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.
    -   [Opakovat](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - opakování provádění uzavřené zásad výkazech, pokud až splnění podmínky. Spuštění opakujte v zadaných časových intervalech a nahoru na zadaný počet opakování.
    -   [Vrátit odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) – spuštění kanálem k odesílání zpráv přerušení nebo vrátí zadaný odpovědi přímo k volajícímu.
    -   [Odeslání žádosti o jedním ze způsobů](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - odešle žádost o zadané adrese URL bez nutnosti čekání na odpověď.
    -   [Odeslání žádosti o](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - odešle žádost o zadanou adresu URL.
    -   [Metoda žádost o set](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - umožňuje změnit metodu HTTP pro žádost.
    -   [Nastavit stav](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) – změní stavový kód HTTP na určité hodnoty.
    -   [Nastavit proměnnou][] - přetrvávají hodnotu do proměnné pojmenované [kontext][] pro pozdější přístup.
    -   [Sledování](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - přidá řetězec do výstupu [Rozhraní API Kontrola metadat](../api-management/api-management-howto-api-inspector.md) .
    -   [Počkat](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) – čeká uzavřené odeslat žádost získat hodnotu z mezipaměti nebo zásady řízení toku dokončit před pokračováním.
-   [Zásady ověřování][]
    -   [Ověřit s Basic][] - ověřit s back-end služby základní ověřování.
    -   [Ověřit certifikátem klient][] - ověřit se službou back-end prostřednictvím certifikátů.
-   [Ukládání do mezipaměti zásady][] 
    -   [Získat z mezipaměti][] - provést mezipaměti vyhledat a vraťte se platná režim cached odpověď, pokud jsou dostupné.
    -   [Úložiště přihlašovacích údajů do mezipaměti][] - mezipaměti odpověď podle přepínacím zadaný mezipaměti.
    -   [Hodnota z mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - obnovit položky v mezipaměti klíčem.
    -   [Hodnota úložiště přihlašovacích údajů do mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) – úložiště položky v mezipaměti klíčem.
    -   [Odebrání hodnotu z mezipaměti](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - odebrat položky v mezipaměti klíčem.
-   [Křížové zásady domény][] 
    -   [Povolení doménami hovorů][] – umožňuje rozhraní API přístup klientů Adobe Flash a Microsoft Silverlight pomocí prohlížeče.
    -   [CORS][] - přidá (CORS) podpory operaci nebo rozhraní API umožňuje hovory mezi doménami od založených na prohlížeči klientů sdílení zdrojů mezi origin.
    -   [JSONP][] - přidá JSON podporující odsazení (JSONP) operace nebo rozhraní API umožňuje hovory mezi doménami od založených na prohlížeči klientů JavaScript.
-   [Transformace zásady][] 
    -   [Převedení JSON XML][] - převede požadavku nebo odpovědi text z JSON na XML.
    -   [Převedení XML JSON][] - převede požadavku nebo odpovědi text ze souboru XML do formátu JSON.
    -   [Najít a nahradit řetězec textu][] – vyhledá podřetězec požadavku nebo odpovědi a nahradí různých podřetězec.
    -   [Adresy URL masky v obsahu][] - znovu data zapisuje (masky) odkazy v odpovědi text tak, aby ukazovaly na odpovídající odkaz přes bránu.
    -   [Nastavení služby back][] - změní pro příchozí žádosti o službu back-end.
    -   [Nastavení textu][] – nastaví zprávy u příchozích a odchozích požadavků.
    -   [Nastavení HTTP záhlaví][] - přiřadí hodnotu existující odpověď nebo žádost o záhlaví nebo přidá nové odpověď a/nebo žádost o záhlaví.
    -   [Nastavení parametru řetězce dotazu][] - přidá, hodnota slouží k nahrazení nebo odstranění žádosti o parametru řetězce dotazu.
    -   [Revize URL][] - převede požadavek URL z veřejné formuláře na formuláři očekávání pomocí webové služby.

## <a name="next-steps"></a>Další kroky

Další informace o výrazech zásad naleznete v následujícím videu.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Zásady omezení přístupu]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Kontrola záhlaví HTTP]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit volání sazba podle předplatného]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Omezit volající IP adresy]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Kvóta využití nastavit tak, že předplatné]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Ověření JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Rozšířené zásady]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Řízení toku]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Nastavit proměnnou]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[výrazy]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[kontext]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Přeposílání požadavek]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Protokol událostí rozbočovači]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Zásady ověřování]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Ověření pomocí Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Ověřování pomocí certifikátu klienta]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Ukládání do mezipaměti zásady]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Získat z mezipaměti]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Ukládání do mezipaměti]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Křížové zásady domény]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Povolit volání doménami]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformace zásady]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Převedení JSON XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Převést do formátu JSON XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Najít a nahradit řetězec v textu]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Adresy URL masky v obsahu]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Nastavení služby back-end]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Nastavení textu]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Nastavení protokolu HTTP záhlaví]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Nastavení parametru řetězce dotazu]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Adresa URL revize]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Zásady správy rozhraní API]: api-management-howto-policies.md
[Odkaz na zásad správy rozhraní API]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Zásady výrazů]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
