<properties
    pageTitle="Kódy chyb Azure Media Services | Microsoft Azure"
    description="Téma obsahuje přehled kódy chyb Azure Media Services."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Kódy chyb Azure Media Services

Pokud používáte Microsoft Azure Media Services, obdržet kódy chyb HTTP ze služby v závislosti na problémy, jako je ověřování tokeny před vypršením platnosti akce, které nejsou podporovány v Media Services. Následuje seznam **kódy chyb HTTP** , který může být vrácen Media Services a možných příčin pro ně.  
  
## <a name="400-bad-request"></a>400 Chybný požadavek

Žádost obsahuje neplatné informace a odmítnutý kvůli jednu z těchto důvodů:

- Není zadán nepodporovanou verzi rozhraní API. Na nejnovější verzi najdete v článku [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).
- Rozhraní API verzi Media Services není zadán. Informace o tom, jak určit verze rozhraní API najdete v tématu [připojení k Media Services s Media Services REST API](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Pokud používáte .NET nebo Java SDK se připojit k Media Services, verze rozhraní API není určen můžete kdykoli zkuste a provedení určité akce proti Media Services.
- Byla specifikována nedefinovanou vlastnost. Název vlastnosti se chybová zpráva. Je možné zadat jenom vlastnosti, které jsou součástí dané entity. Najdete v článku Principy [Azure Media Services ZBÝVAJÍCÍ rozhraní API](http://msdn.microsoft.com/library/azure/hh973617.aspx) seznam entity a jejich vlastnosti.
- Byl zadán neplatný vlastnosti hodnota. Název vlastnosti se chybová zpráva. Odkaz předchozí platná vlastnost typů a jejich hodnoty.
- Hodnota vlastnosti chybí a je nutný.
- Část adresy URL zadán obsahuje chybná hodnota.
- Pokus o byl aktualizovat vlastnosti WriteOnce.
- Pokus o byl vytvořit úlohu vstupní majetku s primárního AssetFile nebyla zadána nebo nelze určit.
- Pokus o byl aktualizovat Locator přidružení zabezpečení. Přidružení zabezpečení Locator pouze lze vytvořené nebo odstranit. Přenos Locator je možné aktualizovat. Další informace najdete v tématu [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Nepodporované operace nebo dotaz byla odeslána. 

## <a name="401-unauthorized"></a>Neoprávněné 401

Žádost nelze ověřit (dříve, než může být oprávněn) kvůli jednu z těchto důvodů:

- Chybějící ověřování záhlaví.
- Hodnota chybná ověřování záhlaví.
    - Tokenu vypršela. Používáte-li rozhraní REST API přímo, najdete v tématu [připojení k Media Services s Media Services REST API](media-services-rest-connect_programmatically.md) se dozvíte, jak pro vytvoření nového tokenu ověřování. Pokud používáte .NET nebo Java SDK, vytvořte objekt CloudMediaContext nebo MediaContract generovat tokenu. Další informace o tom, jak to udělat najdete v tématu [připojení ke službám médium s Media Services SDK pro .NET](media-services-dotnet-connect-programmatically.md).
    - Token obsahuje neplatným podpisem.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 zakázán

Zadání není povolená kvůli jednu z těchto důvodů:

- Účet Media Services nebyla nalezena nebo byl odstraněn.
- Zakázání účtu Media Services a není typ žádost HTTP GET. Operace služeb vrátí 403 odpověď.
- Ověřovací token neobsahuje přihlašovacími údaji uživatele: název účtu a/nebo SubscriptionId. Tyto informace najdete v koncovku médií služby UI účtu Media Services na portálu Správa Azure.
- Nelze použít zdroje.
    - Pokus o byl používat MediaProcessor, která není k dispozici pro váš účet Media Services.
    - Pokus o byl aktualizovat JobTemplate definované Media Services.
    - Pokus o byl uvedeného postupu přepsat Locator některé další Media Services účtu.
    - Pokus o byl uvedeného postupu přepsat ContentKey některé další Media Services účtu.

- Zdroje nelze vytvořit z důvodu kvóty služby, které dosáhl u účtu Media Services. Další informace o kvóty služby najdete v článku [kvóty](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 Nenalezeno

Zadání není povolená zdroje kvůli jednu z těchto důvodů:

- Pokus o byl aktualizovat entita, který neexistuje.
- Pokus o byl odstranit entita, který neexistuje.
- Pokus o byl vytvořit entita propojující entita, který neexistuje.
- Pokus o byl získat entita, který neexistuje.
- Pokus o byl určit úložiště účtu, který není přidružený k účtu Media Services.  

## <a name="409-conflict"></a>409 konflikt

Zadání není povolená kvůli jednu z těchto důvodů:

- Víc AssetFile má zadaný název v rámci majetku.
- Pokus o byl k vytvoření druhého primární AssetFile v rámci majetku.
- Pokus o byl vytvořit ContentKey se zadaným Id už používá.
- Pokus o byl vytvořit Locator se zadaným Id už používá.
- Víc IngestManifestFile má zadaný název v IngestManifest.
- Pokus o byl propojíte druhý šifrování úložiště ContentKey materiálů zašifrovaných úložiště.
- Pokus o byl propojíte stejné ContentKey majetku.
- Pokus o byl vytvořit locator aktivum jehož kontejner úložiště chybí nebo už není přidružený k majetku.
- Pokus o byl vytvořit locator materiálů, které již obsahují 5 Locator použití. (Azure úložiště vynucuje maximálně pěti zásad sdílený přístup na jeden úložiště kontejner.)
- Propojení účtu úložiště aktiva IngestManifestAsset je stejná jako účet úložiště používaný nadřazené IngestManifest.  

## <a name="500-internal-server-error"></a>500 Vnitřní chyba serveru

Během zpracování požadavku narazí Media Services některé chyby, které brání zpracování pokračovat. Může to být kvůli jednu z těchto důvodů:

- Vytváření materiálů nebo úlohy selhala, protože Media Services účet služby kvóty je dočasně nedostupný.
- Vytvoření kontejneru úložiště objektů blob materiálů nebo IngestManifest selhala, protože informace o účtu úložiště na účet je dočasně nedostupný.
- Neočekávané chybě. 

## <a name="503-service-unavailable"></a>503 Služba není k dispozici

Příjem žádosti o aktuálně nedokáže serveru. Tuto chybu může způsobovat nadbytečné žádosti o služby. Media Services omezení mechanismus omezuje využití prostředků pro aplikace, díky kterým nadbytečné žádosti o služby.

>[AZURE.NOTE]Zkontrolujte chybová zpráva a řetězec kódu chyby získejte podrobnější informace o důvodu, že jste získali 503 chyby. Tato chyba vždy neznamená omezení.

Možné stav popisy jsou:

- "Server je zaneprázdněná. Předchozích spuštění tohoto typu žádost přijal více než {0} sekund."
- "Server je zaneprázdněná. Více než {0} požadavků sekundu můžete sníží."
- "Server je zaneprázdněná. Více než {0} požadavků vyvolané {1} můžete sníží."

Zpracovat tuto chybu, doporučujeme vám použít logika exponenciální zpět vypnout opakování. To znamená, že pomocí postupně delší čeká mezi opakování pro odpovědi po sobě jdoucí chyby.  Další informace najdete v tématu [Přechodná poruch zpracování blok aplikace](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Pokud používáte [Azure Media Services SDK pro .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), prováděn Logika opakování 503 chyby SDK.  
  
## <a name="see-also"></a>Viz taky  

[Mediální služby Řízení kódy chyb](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
