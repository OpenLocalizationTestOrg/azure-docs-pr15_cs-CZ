<properties
 pageTitle="Karta Vývojář v příručce - zasílání zpráv | Microsoft Azure"
 description="Azure Průvodce vývojář IoT centrální - zařízení do cloudu a cloudu zařízení zasílání zpráv"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Odesílání a přijímání zpráv s IoT centrální

## <a name="overview"></a>Základní informace

Rozbočovač IoT poskytuje následující zpráv základní komunikovat s zařízení:

- [Zařízení do cloudu] [ lnk-d2c] ze zařízení aplikaci serverová.
- [Shluk zařízení] [ lnk-c2d] z aplikace serverová (*služby* nebo *cloudového*).

Základní vlastnosti IoT Centrum zpráv funkce jsou spolehlivost a životnosti zprávy. Tyto vlastnosti povolit odolnost k chybovému připojení na straně zařízení a načíst vrcholy pole v případě zpracování na straně cloudu. Rozbočovač IoT používá *aspoň jednou* doručení záruky pro zařízení do cloudu a cloudu zařízení zasílání zpráv.

Rozbočovač IoT podporuje více [zařízení webových protokoly] [ lnk-protocols] (například MQTT AMQP a HTTP). K podpoře Bezproblémová spolupráce přes protokoly, IoT centrální definuje [obvyklý formát zprávy] [ lnk-message-format] podporující všechny protokoly webových zařízení.

Centrální IoT zpřístupňuje [koncový bod kompatibilní s prohlížečem události centrální] [ lnk-compatible-endpoint] umožníte back-end aplikace čtení dostali Centru zpráv zařízení do cloudu.

### <a name="when-to-use"></a>Kdy použít

Zasílání zpráv je základní funkce IoT rozbočovače. Použijte potřeby můžete do svého back-end odesílat zprávy z vašeho zařízení nebo odesílat zprávy z vašeho back-end na zařízení.

Porovnání IoT centrální a událostí rozbočovače služby, najdete v článku [porovnání IoT centrální a událostí rozbočovače][lnk-compare].

## <a name="device-to-cloud-messages"></a>Zprávy zařízení do cloudu

Odesílání zpráv zařízení do cloudu pomocí zařízení webových koncového bodu (**/devices/ {deviceId} / zprávy nebo události**). Služby back-end obdrží zpráv zařízení do cloudu pomocí koncový bod webových služeb (**/messages/events**), který je kompatibilní se službou [Události rozbočovače][lnk-event-hubs]. Proto, můžete použít standardní [Integrace rozbočovače událostí a SDK] [ lnk-compatible-endpoint] přijímat zprávy zařízení do cloudu.

Centrální IoT implementuje zařízení do cloudu zpráv způsobem, který je podobný [Události rozbočovače][lnk-event-hubs]. Zpráva zařízení do cloudu IoT centrální budou jako události rozbočovače *události* víc než [Služby Bus] [ lnk-servicebus] *zprávy*.

Tato implementace obsahuje následující důsledky:

* Podobně na událost rozbočovače události zařízení do cloudu zprávy jsou trvalé a dosažených v centrální IoT až 7 dnů (najdete v článku [Možnosti konfigurace zařízení do cloudu][lnk-d2c-configuration]).
* Zařízení do cloudu zprávy jsou rozdělený napříč pevné sadu oddílů, která je nastavená na údaje o času vytvoření (najdete v článku [Možnosti konfigurace zařízení do cloudu][lnk-d2c-configuration]).
* K události rozbočovače musí analogicky klientů čtení zpráv zařízení do cloudu zpracovat, oddíly a kontrola. V tématu [Rozbočovače události - jinými události][lnk-event-hubs-consuming-events].
* Stejně jako události rozbočovače události zařízení do cloudu zprávy může být nejvýše 256 KB a můžete v dávkách optimalizovat odešle seskupené. Listy může být maximálně 256 KB a maximálně 500 zpráv.

Existuje ale několik důležité rozdíly mezi IoT Centrum zpráv zařízení do cloudu a rozbočovače událostí:

* Způsobem popsaným v tématu [řízení přístupu k centrální IoT] [ lnk-devguide-security] části IoT rozbočovač umožňuje na zařízení ověřování a řízení přístupu.
* Rozbočovač IoT umožňuje miliony současně připojených zařízení (viz [kvót a omezení][lnk-quotas]), zatímco rozbočovače události se omezí na 5000 připojení AMQP obor.
* Rozbočovač IoT neumožňuje libovolného dělení pomocí **PartitionKey**. Zařízení do cloudu zprávy jsou rozděleny podle jejich původní **deviceId**.
* Změna velikosti IoT centrální je mírně liší od měřítka rozbočovače události. Další informace najdete v tématu [Měřítka centrální IoT][lnk-guidance-scale].

> [AZURE.NOTE] Ve všech případech nemůže nahradit IoT základem rozbočovače události. Například v některých výpočty zpracování události, je nutné znovu vytvořit oddíly události týkající různých vlastnost nebo pole před analýza datových proudů. V tomto scénáři byste mohli oddělit dvě části proudu zpracování kanálem k odesílání zpráv přes Centrum události. Další informace najdete v tématu *oddíly* v [Azure události rozbočovače přehled][lnk-eventhub-partitions].

Podrobnosti o tom, jak používat zařízení do cloudu zprávy, najdete v článku [IoT centrální API a SDK][lnk-sdks].

> [AZURE.NOTE] Pokud chcete použít HTTP k odesílání zpráv zařízení do cloudu, názvy vlastností a hodnoty může obsahovat pouze alfanumerických znaků ASCII, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Přenosy non telemetrie

Často, kromě telemetrie datových bodů, zařízení taky zprávy a pošlete požadavky, které vyžadují spuštění a zpracování z vrstva aplikace obchodní logiky. Například kritické upozornění, které musíte aktivovat určité akce v back-end nebo zařízení odpovědi na příkazy odesílaným z back-end.

Další informace o nejlepší způsob, jak zpracovat takovou zprávy najdete v tématu [kurz: způsobu zpracování zpráv zařízení do cloudu IoT centrální] [ lnk-d2c-tutorial] kurz.

### <a name="device-to-cloud-configuration-options"></a>Možnosti konfigurace zařízení do cloudu

IoT Centrum poskytuje následující vlastnosti pro umožňují řídit zpráv zařízení do cloudu.

* **Počet oddílů**. Vlastnost při vytvoření určující počet oddílů pro požití události zařízení do cloudu.
* **Doba uchovávání informací**. Tato vlastnost zadává dobu zpráv zařízení do cloudu. Výchozí hodnota je jeden den, ale může být zvětšené tak, aby sedmi dnů.

Navíc analogicky k události rozbočovače IoT centrální umožňuje spravovat spotř skupiny na zařízení cloudu dostanou koncového bodu.

Můžete upravit všechny tyto vlastnosti, buď programově prostřednictvím [IoT Centrum zdrojů poskytovatele rozhraní REST API][lnk-resource-provider-apis], nebo pomocí [Azure portál][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Falšování Anti vlastnosti

Chcete-li předejít zařízení falšování zpráv zařízení do cloudu, IoT centrální razítka všech zpráv s následujícími vlastnostmi:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

První dva obsahují **deviceId** a **generationId** původní zařízení podle [vlastností identity zařízení][lnk-device-properties].

Vlastnost **ConnectionAuthMethod** obsahuje objekt JSON serializován s následujícími vlastnostmi:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Shluk zařízení zprávy

Odesílání zpráv cloudu zařízení prostřednictvím služby webových koncového bodu (**/messages/devicebound**). Zařízení s obdrží prostřednictvím specifické pro zařízení koncového bodu (**/devices/ {deviceId} / zprávy/devicebound**).

Každá zpráva cloudu zařízení je určený jenom pro jedno zařízení nastavením vlastnosti **k** **/devices/ {deviceId} / zprávy/devicebound**.

>[AZURE.IMPORTANT] Každý fronta zařízení uchovává maximálně 50 cloudu zařízení zprávy. Vyzkoušení odešlete více zpráv stejné zařízení způsobí chybu.

> [AZURE.NOTE] Při odesílání zpráv cloudu zařízení názvy vlastností a hodnoty může obsahovat pouze alfanumerických znaků ASCII, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Zpráva cyklus

Zajistit aspoň jednou doručení zprávy, trvá IoT Centrum zpráv cloudu zařízení ve frontách na zařízení. Zařízení musí potvrdit explicitně *dokončování* IoT centrální odebrat z fronty. Zajistíte odolnost proti připojení a selhání zařízení.

Na následujícím obrázku vidíte grafu stavu životního cyklu zprávu cloudu zařízení.

![Zpráva cloudu zařízení cyklus][img-lifecycle]

Pokud službu odešle zprávu, nebude považována za *byla zařazena do fronty*. Když zařízení chce *přijímat* zprávy, IoT centrální *zámků webů* zprávy (nastaví stav **neviditelné**) umožňuje jiných vláknech na stejné zařízení zahájíte příjem ostatní zprávy. Zpracování zprávy dokončení vlákna zařízení upozorněním IoT centrální *dokončení* zprávu.

Zařízení můžete taky:

- *Odmítnout* zprávu, která způsobí, že IoT centrální nastavit na stav **Deadlettered** . Poznámka: zařízení připojením pomocí MQTT přijmout nebo zamítnout C2D zprávy.
- *Opustit* zprávu, která IoT centrální vložte zprávu do fronty s stavu, bude se nastavit, aby **byla zařazena do fronty**.

Posloupnost selhat zpracuje zprávy bez oznámení IoT centrální. V tomto případě zprávy automaticky přechodu z **neviditelné** stavu zpět do stavu **byla zařazena do fronty** po *vypršení časového limitu viditelnost (nebo lock)*. Výchozí hodnota této časový limit je jednu minutu.

Zprávu můžete přechod mezi **byla zařazena do fronty** a **neviditelné** státy pro maximálně počet určený ve vlastnosti **počet maximální doručení** IoT rozbočovače. Po uplynutí tohoto počtu přechody nastaví IoT centrální **Deadlettered**stavu zprávy. Podobně IoT centrální nastaví stav zprávy **Deadlettered** po jeho platnosti (viz [Hodnota Time to live][lnk-ttl]).

Kurz cloudu zařízení zpráv najdete v článku [kurz: odeslání zprávy cloudu zařízení s IoT centrální][lnk-c2d-tutorial]. Odkazy na témata o různých rozhraní API a SDK vystavit funkci cloudu zařízení, naleznete v tématu [IoT centrální API a SDK][lnk-sdks].

> [AZURE.NOTE] Obvykle cloudu zařízení zpráv proveďte pokaždé, když ke ztrátě zprávy neovlivní logiky aplikace. Například obsah zprávy byly úspěšně zachován v místním úložišti nebo operace byla úspěšně provedena. Zprávy můžou taky provádění přechodná informace, jehož ztráta nebudou mít vliv funkce aplikace. V některých případech dlouho probíhajících úkolů provedením zpráva cloudu zařízení po uchování popis úkolu v místním úložišti. Pak upozorní aplikaci back-end s jedna nebo více zpráv zařízení do cloudu v jednotlivých fázích průběh úkolu.

### <a name="message-expiration-time-to-live"></a>Vypršení platnosti zprávy (hodnota time to live)

Každá zpráva cloudu zařízení má čas vypršení platnosti. Toto je nastavený čas službu (ve vlastnosti **ExpiryTimeUtc** ), nebo IoT centrální pomocí zadaný jako centrální IoT vlastnost výchozí *Hodnota time to live* . V tématu [Možnosti konfigurace cloudu zařízení][lnk-c2d-configuration].

> [AZURE.NOTE] Běžným způsobem využít vypršení platnosti zprávy a vyhněte se odesílání zpráv odpojeno zařízení, je nastavit čas (krátký) s dynamickými hodnoty. Tento přístup dosahuje stejný výsledek jako uchování stavu připojení zařízení a při tom efektivnější. Pokud požadovat potvrzení zprávy IoT centrální s upozorněním zařízení, která budou moct přijímat zprávy a která zařízení jsou online nebo se nezdařil.

### <a name="message-feedback"></a>Zpráva názory

Když pošlete zprávu cloudu zařízení, službu můžete požádat o doručení jednotlivých zpráv názor týkající se stavu konečný této zprávy.

- Pokud má argument vlastnost **Ack** **kladné**, IoT centrální vygeneruje zprávu názory Pokud a jenom v případě, zpráva cloudu zařízení dorazila stavu **Dokončeno** .
- Pokud má argument vlastnost **Ack** **záporné**, IoT centrální vygeneruje zprávu zpětné vazby pouze v případě, zpráva cloudu zařízení dosáhne **Deadlettered** stavu.
- Je-li nastavit vlastnost **Ack** na **úplné**IoT centrální vygeneruje zprávu zpětné vazby v obou případech.

> [AZURE.NOTE] Pokud je už **plná** **Ack** a není zpráva zpětnou vazbu, znamená to, že zpráva zpětnou vazbu, kterým vypršela platnost. Služba nelze vědět, co se stalo s původní zprávy. Ve skutečnosti službu zajistí, že ji zpracovat zpětnou vazbu před vypršením jeho platnosti. Čas maximální vypršení dva dny, které umožňuje dostatek času na získání službu běží znovu dojde k chybě.

Způsobem popsaným v tématu [koncové body][lnk-endpoints], IoT Centrum poskytuje svůj názor pomocí webových služeb koncového bodu (**/messages/servicebound/feedback**) jako zprávy. Sémantiku pro příjem názory jsou stejné jako u zprávy cloudu zařízení a mít stejné [zprávy cyklus][lnk-lifecycle]. Kdykoli je to možné, zpráva názory dávce v jedné zprávě, v tomto formátu:

| Vlastnost | Popis |
| -------- | ----------- |
| EnqueuedTime | Časové razítko označující vytvoření zprávy. |
| ID uživatele | `{iot hub name}` |
| Typ obsahu | `application/vnd.microsoft.iothub.feedback.json` |

Text je serializován JSON polem záznamů, oba objekty mají následující vlastnosti:

| Vlastnost | Popis |
| -------- | ----------- |
| EnqueuedTimeUtc | Časové razítko označující, kdy se stalo výsledek zprávy. Například zařízení dokončit nebo zprávu vypršení platnosti. |
| OriginalMessageId | **MessageId** cloudu zařízení zprávy, ke které se vztahují tyto informace svůj názor. |
| StatusCode | Povinný celé číslo. Použít v názory zpráv generovaných IoT centrální. <br/> 0 = Úspěch <br/> 1 = konec platnosti zprávy <br/> 2 = maximální doručení počtu směrování. <br/> 3 = zpráva byla odmítnuta |
| Popis | Řetězec hodnoty pro **StatusCode**. |
| DeviceId | **DeviceId** cílové zařízení cloudu zařízení zprávy, ke které se vztahuje tato část názory. |
| DeviceGenerationId | **DeviceGenerationId** cílové zařízení cloudu zařízení zprávy, ke které se vztahuje tato část názory. |


>[AZURE.IMPORTANT] Služba zadejte **MessageId** cloudu zařízení zprávy mají být k sladit jeho zpětnou vazbu s původní zprávy.

Následující příklad ukazuje textu zprávy svůj názor.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Možnosti konfigurace cloudu zařízení

Každý IoT Centrum poskytuje následující možnosti konfigurace pro zasílání zpráv cloudu zařízení.

| Vlastnost | Popis | Oblast a výchozí |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Výchozí hodnota TTL cloudu zařízení zpráv. | Interval ISO_8601 až 2D (minimální minutu). Výchozí hodnota: 1 hodinu. |
| maxDeliveryCount | Doručení maximální počet různých hodnot fronty cloudu zařízení na zařízení. | 1 až 100. Výchozí: 10. |
| feedback.ttlAsIso8601 | Uchovávání informací pro službu vazbou názory zprávy. | Interval ISO_8601 až 2D (minimální minutu). Výchozí hodnota: 1 hodinu. |
| feedback.maxDeliveryCount | Doručení maximální počet různých hodnot fronty svůj názor. | 1 až 100. Výchozí: 100. |

Další informace najdete v tématu [Vytvoření IoT rozbočovače][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Číst zprávy zařízení do cloudu

Rozbočovač IoT zpřístupňuje koncový bod služby back-end čtení dostali vaše Centrum zpráv zařízení do cloudu. Koncový bod is Event centrální kompatibilní s prohlížečem, které vám umožní použít některou z mechanismy službu události rozbočovače podporuje pro čtení zpráv.

Když použijete [SDK Bus služby Azure pro .NET] [ lnk-servicebus-sdk] nebo [Události rozbočovače - události procesor hostitele][lnk-eventprocessorhost], můžete použít libovolný IoT centrální připojovací řetězec s správná oprávnění. Pak použijte jako název události centrální **zprávy nebo události** .

Pokud používáte SDK (nebo integrace produkt), které neví IoT rozbočovače, musíte kompatibilní s prohlížečem události centrální koncového bodu a název kompatibilní s prohlížečem události centrální z nastavení IoT centrální [Azure portál][lnk-management-portal]:

1. V centrální zásuvné IoT klikněte na **zasílání zpráv**.
2. V části **Nastavení zařízení do cloudu** nenajdete následující hodnoty: **koncový bod kompatibilní s prohlížečem události centrální**, **kompatibilní s prohlížečem události centrální**a **oddíly**.

    ![Nastavení zařízení do cloudu][img-eventhubcompatible]

> [AZURE.NOTE] Vyžaduje-li v SDK hodnotu **název hostitele** nebo **Namespace** , odebráním schématu **kompatibilní s prohlížečem události centrální koncového bodu**. Pokud například koncový bod kompatibilní s prohlížečem události centrální je **sb://iothub-ns-myiothub-1234.servicebus.windows.net/** **Hostname** bude **iothub ns myiothub 1234.servicebus.windows.net**a **Namespace** bude **iothub ns myiothub 1234**.

Pak můžete všechny sdílené zabezpečení přístupu, který má oprávnění **ServiceConnect** připojení k centru zadaný události.

Pokud potřebujete k vytvoření připojovacího řetězce centrální události pomocí předchozí informace, pomocí následujícího vzorce:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Následující obrázek je seznam SDK a integrace, které můžete použít s kompatibilní s prohlížečem události centrální koncové body, které poskytuje IoT centrální:

* [Java události rozbočovače klienta](https://github.com/hdinsight/eventhubs-client)
* [Apache bouře spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Zobrazení [spout zdroje](https://github.com/apache/storm/tree/master/external/storm-eventhubs) na GitHub.
* [Integrace Apache Spark](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o výměně zpráv s IoT centrální.

## <a name="message-format"></a>Formát zprávy

Rozbočovač IoT zprávy zahrnují:

* Nastavení *Vlastnosti systému*. Vlastnosti, které IoT centrální interpretovat nebo sady. Toto nastavení je předem.
* Nastavení *vlastností aplikace*. Slovník řetězec vlastnosti, které můžete definovat aplikace a přístup, aniž by musel rekonstruovat textu zprávy. Rozbočovač IoT nikdy neupraví tyto vlastnosti.
* Neprůhledné binární textu.

Další informace o jak zakódovaný zprávu v různých protokoly najdete v článku [IoT centrální API a SDK][lnk-sdks].

V následující tabulce jsou uvedeny sadu vlastností systém IoT Centrum zpráv.

| Vlastnost | Popis |
| -------- | ----------- |
| MessageId | Nastavit uživatele identifikátor pro tuto zprávu pro požadavek odpověď vzorků. Formát: Velká a malá písmena řetězec (až 128 znaků) alfanumerických znaků ASCII 7-bit + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Pořadové číslo | Číslo (jedinečné za zařízení fronty) přiřazené tak, že IoT centrální každá zpráva cloudu zařízení. |
| K | Určení v [Cloudu zařízení] [ lnk-c2d] zprávy. |
| ExpiryTimeUtc | Datum a čas vypršení platnosti zprávy. |
| EnqueuedTime | Datum a čas přijetí zprávy službou tak, že IoT centrální. |
| CorrelationId | Vlastnost řetězce ve zprávě odpověď, která obvykle obsahuje MessageId pozvánce na schůzku, ve vzorcích požadavek odpověď. |
| ID uživatele | ID slouží k určení origin zprávy. Jestliže IoT centrální vznikají zprávy, je nastavena na `{iot hub name}`. |
| Potvrzení | Generátor zprávy svůj názor. Tato vlastnost se používá v cloudu zařízení zprávy s žádostí o IoT centrální generování názory zpráv důsledku spotřebu zprávy tak, že zařízení. Možné hodnoty: **žádné** (výchozí): vygeneruje se žádná zpráva zpětnou vazbu, **kladné**: zpráva zpětné vazby v případě zprávy byl dokončen, **záporné**: zpráva názory Pokud platnost zprávy (nebo doručení maximální počet dosáhl) bez vystavení zařízení, nebo **celou**: kladné a záporné. Další informace najdete v tématu [zprávy názory][lnk-feedback]. |
| ConnectionDeviceId | ID nastavil IoT Centrum zpráv zařízení do cloudu. Obsahuje **deviceId** zařízení, které zprávu odeslal. |
| ConnectionDeviceGenerationId | ID nastavil IoT Centrum zpráv zařízení do cloudu. Obsahuje **generationId** (podle [vlastností identity zařízení][lnk-device-properties]) zařízení, které zprávu odeslal. |
| ConnectionAuthMethod | Metody ověřování nastavil IoT Centrum zpráv zařízení do cloudu. Tato vlastnost obsahuje informace o metodě ověřování používá k ověření zařízení odeslání zprávy. Další informace najdete v tématu [zařízení do cloudu falšování anti][lnk-antispoofing].|

## <a name="communication-protocols"></a>Komunikační protokoly

Rozbočovač IoT umožňuje zařízení na používání [MQTT][lnk-mqtt], MQTT přes WebSockets [AMQP][lnk-amqp], AMQP přes WebSockets a HTTP protokoly pro komunikaci straně zařízení. Následující tabulka uvádí nejvyšší úrovně doporučení pro výběr Protocol (protokol):

| Protocol (protokol) | Kdy by měla zvolte tento protokol |
| -------- | ------------------------------------ |
| MQTT <br> MQTT přes WebSocket     | Použít na všech zařízeních, které nevyžadují připojit přes stejné připojení TLS několika zařízeních (oba objekty mají vlastní přihlašovací údaje na zařízení). |
| AMQP <br> AMQP přes WebSocket    | Použití na pole a cloudu bran umožní využít výhod připojení multiplexování na zařízeních. |
| NASTAVIT INFORMACE HTTP    | Slouží k zařízení, která nepodporuje jiné protokoly. |

Když zvolíte straně zařízení komunikace Protocol (protokol), zvažte následující možnosti:

* **Vzorek cloudu zařízení**. Nastavit informace HTTP nemá efektivně implementovat nabízených serveru. Jako takové při použití protokolu HTTP zařízení hlasování IoT Centrum zpráv cloudu zařízení. Tento přístup je zařízení a IoT centrální. V části aktuální HTTP pokyny by měl každý zařízení hlasování zpráv každých 25 minut nebo více. Na druhé straně MQTT AMQP podpora a server nabízených při příjmu zprávy cloudu zařízení. Umožňují okamžité posune zpráv z centrální IoT zařízení. Pokud doručení latence je důležité, MQTT nebo AMQP protokoly nejlepší používat. Jen zřídka připojeného v případě zařízení funguje taky HTTP.
* **Pole brány**. Pokud chcete použít MQTT a HTTP, nemůže připojit víc zařízeních (oba objekty mají vlastní přihlašovací údaje na zařízení) použití stejné připojení TLS. Proto pro [pole brány scénáře][lnk-azure-gateway-guidance], tyto protokoly jsou nepřesnými, protože vyžadují jednu TLS propojení pole brány a IoT centrální u každého zařízení připojené k bráně pro pole.
* **Nízké zdroje zařízení**. Knihovny MQTT a HTTP mít stopu menší než AMQP knihoven. Jako například pokud zařízení má omezenou zdroje (pro například méně než 1 MB RAM), tyto protokoly může být k dispozici pouze protokol implementaci.
* **Přecházení sítě**. Standardní protokol AMQP používá port 5671, zatímco MQTT sleduje port 8883, což může způsobit problémy s sítí, které jsou uzavřeny do jiného HTTP protokoly. MQTT přes WebSockets AMQP přes WebSockets a HTTP jsou k dispozici pro použití v tomto scénáři.
* **Velikost datové části**. MQTT a AMQP protokoly binární, které vedou kompaktnější datové části než HTTP.

> [AZURE.NOTE] Při použití protokolu HTTP, každého zařízení by měl hlasování cloudu zařízení zpráv zabere nejméně každých 25 minut. Během vývoje, je přijatelné dotazování častěji každých 25 minut.

## <a name="port-numbers"></a>Čísla portů

Zařízení mohli komunikovat s IoT centrální v Azure pomocí různých protokoly. Volba protokol je obvykle řízeno specifickým požadavkům řešení. V následující tabulce jsou uvedeny Odchozí porty, které musí být otevřený pro zařízení, aby se nebudou moct používat určitý protokol:

| Protocol (protokol) | Porty |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT přes WebSockets | 443    |
| AMQP     | 5671    |
| AMQP přes WebSockets | 443    |
| NASTAVIT INFORMACE HTTP     | 443     |
| LWM2M (Správa zařízení) | 5684 |

Po vytvoření rozbočovači IoT v Azure oblasti centru pořád stejnou IP adresu dobu trvání této centrální. Však udržovat kvality služeb, pokud Microsoft přesune centru IoT jednotku jiné měřítko pak je přiřazeno nové IP adresu.

## <a name="notes-on-mqtt-support"></a>Poznámky: MQTT podpory

Rozbočovač IoT implementuje protokol v3.1.1 MQTT se tato omezení a konkrétní chování:

  * **QoS 2 nepodporuje**. Pokud klienta zařízení publikování zprávu s **QoS 2**, IoT centrální slouží k zavření připojení k síti. Pokud klienta zařízení přihlásí k tématu **QoS**2, IoT centrální uděluje maximální QoS úrovně 1 v paketu **SUBACK** .
  * **Zachovat zpráv se neuloží**. Pokud klienta zařízení publikuje zprávy s příznakem zachovat nastavena na hodnotu 1, přidá IoT centrální **x-určovat, jestli zachovat** vlastnosti aplikace na zprávu. V tomto případě IoT centrální nebude zachován zachovat zprávy, ale místo toho předá back-end aplikaci.

Další informace najdete v tématu [MQTT centrální IoT podporují][lnk-devguide-mqtt].

Jako konečný pozornost, přečtěte si [Azure IoT protokol brány] [ lnk-azure-protocol-gateway] , díky kterému můžete nasadit vlastní protokol výkonné brány, která rozhraní přímo s IoT centrální. Protokol brány Azure IoT umožňuje přizpůsobit protokol zařízení tak, aby zahrnoval brownfield MQTT nasazení nebo jiné vlastní protokoly. Tento postup vyžaduje, však spustit a provoz brány vlastní protokol.

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT rozbočovači a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT centrální MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT centrální protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď jste se naučili způsob odesílání a přijímání zpráv s IoT centrální, vás mohla zajímat v následujících tématech vývojář příručka:

- [Nahrání souborů ze zařízení][lnk-devguide-upload]
- [Správa identity zařízení v centrální IoT][lnk-devguide-identities]
- [Řízení přístupu k centrální IoT][lnk-devguide-security]
- [Pomocí zařízení twins synchronizovat stav a konfigurace][lnk-devguide-device-twins]
- [Přímé metodu na zařízení][lnk-devguide-directmethods]
- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, může být zajímavé v těchto kurzech IoT centrální:

- [Začínáme s centrální IoT Azure][lnk-getstarted-tutorial]
- [Odeslání zprávy cloudu zařízení s IoT centrální][lnk-c2d-tutorial]
- [Způsob zpracování zprávy IoT Centrum zařízení do cloudu][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
