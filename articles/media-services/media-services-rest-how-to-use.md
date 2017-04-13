<properties 
    pageTitle="Přehled Media Services REST API | Microsoft Azure" 
    description="Přehled Media Services REST API" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Přehled Media Services REST API 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services je služba, která přijímá požadavků na základě OData HTTP a můžete odpovědět zpět v podrobného JSON nebo atom + pub. Protože Media Services odpovídá Azure návrh pravidla, není sadu požadované HTTP záhlaví, které každého klienta musí používat při připojování k Media Services, jakož i sadu volitelné záhlaví, které lze použít. V následujících částech jsou popsány záhlaví a HTTP slovesa lze použít při vytváření žádosti a doručování odpovědi z Media Services.

##<a name="considerations"></a>Co byste měli zvážit 

Tyto informace platí při použití ZBÝVAJÍCÍ.

- Při dotazování entity, je omezení na 1000 entity vrátit najednou, protože veřejné v2 ZBÝVAJÍCÍ omezení výsledků dotazu 1000 výsledky. Je potřeba použít **Přeskočit** a **Prohlédněte** (.NET) / **horní** (REST) jako popsaných v [tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu rozhraní REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Při použití JSON a zadáním použití klíčového **__metadata** v žádosti o (například odkazuje na propojený objekt) je nutné nastavit záhlaví **přijmout** ve formátu [JSON podrobného](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (viz následující příklad). OData nerozumí vlastnost **__metadata** v pozvánce na schůzku, pokud je nastavena na podrobný.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardní záhlaví HTTP žádost o nepodporuje Media Services

Pro každé volání provedené do Media Services je sada požadované záhlaví, které musí obsahovat v žádosti o a také sadu volitelné záhlaví je vhodné zahrnout. V následující tabulce jsou uvedeny požadované záhlaví:


Záhlaví|Typ|Hodnota
---|---|---
Povolení|Nosný|Nosný je mechanismus pouze přípustném ověření. Hodnota musí obsahovat také přístupový token poskytovanou ACS.
verze x ms|Decimal|2.11
DataServiceVersion|Decimal|3.0
MaxDataServiceVersion|Decimal|3.0



>[AZURE.NOTE] Protože Media Services využívá k zobrazování jeho základní úložiště metadat materiálů prostřednictvím rozhraní REST API OData, záhlaví DataServiceVersion a MaxDataServiceVersion měl by být součástí žádosti; ale pokud nejsou, pak aktuálně Media Services předpokládá, že DataServiceVersion použít hodnotu 3.0.

Následující obrázek je sada volitelné záhlaví:

Záhlaví|Typ|Hodnota
---|---|---
Datum|Datum RFC 1123|Časové razítko žádosti o
Přijmout|Typ obsahu|Požadovaný typ obsahu pro odpověď například následující:<p> -aplikace/json; odata = komentářem<p> -aplikace/atom + xml<p> Odpovědi může být různé typu obsahu, třeba načítání objektů blob kde úspěšné odpověď bude obsahovat toku objektů blob jako datové.
Přijetí kódování|GZIP, Uprostřed zúžené|GZIP a DEFLATE kódování, případně. Poznámka: Pro velké zdroje Media Services mohou ignorovat toto záhlaví a vrátit nekomprimované data.
Přijetí jazyka|"en" a "es".|Určuje preferovaný jazyk pro odpověď.
Přijetí znaková sada|Znaková sada typ jako "UTF-8"|Výchozí hodnota je UTF-8.
Metoda HTTP X|Metoda HTTP|Umožňuje klientů nebo brány firewall, které nepodporují metod protokolu HTTP jako vložit nebo odstranit pomocí těchto metod vytvořena prostřednictvím GET volání.
Typ obsahu|Typ obsahu|Žádosti o typu obsahu, na žádost o text v umístění nebo příspěvek.
id klienta požadavek|Řetězec|Volající definované hodnotu, který identifikuje daný požadavek. Pokud je zadaná, tato hodnota bude obsahovat zprávu s odpovědí jako způsob, jak mapování požadavku. <p><p>**Důležité**<p>Hodnoty by měl být uzavřeny v 2096b (2 kB).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Záhlaví standardních odpovědí HTTP nepodporuje Media Services

Následující obrázek je sada záhlaví, které mohou být vráceny pro vás v závislosti na zdroje, které byly vyžádání a akce, které jste chtěli provést.


Záhlaví|Typ|Hodnota
---|---|---
žádost o id|Řetězec|Jedinečný identifikátor pro aktuální operaci služby generovaného.
id klienta požadavek|Řetězec|Identifikátor nastavil volající v původní žádost, pokud je k dispozici.
Datum|Datum RFC 1123|Datum zpracovány žádosti.
Typ obsahu|Liší|Typ obsahu těla odpovědi.
Kódování obsahu|Liší|Gzip nebo uprostřed zúžené podle potřeby.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardní HTTP sloves podporovaných Media Services

Následující obrázek je na celý seznam HTTP akcí, které můžete použít k vytváření HTTP požadavky:


Akce|Popis
---|---
ZÍSKÁNÍ|Vrací aktuální hodnotu objektu.
PŘÍSPĚVEK|Vytvoří objekt na základě dat k dispozici nebo odešle příkazu.
UMÍSTĚNÍ|Nahradí objektu nebo vytvoří pojmenované objektu (je-li k dispozici).
ODSTRANĚNÍ|Odstranění objektu.
SLOUČENÍ|Aktualizuje existující objekt změn pojmenované vlastnosti.
HLAVY|Vrátí metadat objektu získat odpovědi.

##<a name="limitation"></a>Omezení

Při dotazování entity, je omezení na 1000 entity vrátit najednou, protože veřejné v2 ZBÝVAJÍCÍ omezení výsledků dotazu 1000 výsledky. Je potřeba použít **Přeskočit** a **Prohlédněte** (.NET) / **horní** (REST) jako popsaných v [tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu rozhraní REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Zjištění Media Services modelu

Zpřístupnění více konfigurací entity Media Services, lze použít operaci $metadata. Umožňuje načítat všechny typy entit platné, vlastnosti entity, přidružení, funkce, akce a tak dále. Následující příklad ukazuje, jak vytvářet identifikátor URI: https://media.windows.net/API/$ metadata.

Má připojit "? rozhraní api version=2.x" za účelem URI, pokud chcete zobrazit metadata v prohlížeči nebo do žádosti o nezahrnujte záhlaví x-ms verze.



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
