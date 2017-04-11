<properties
 pageTitle="Příručka vývojáře – přímé metody | Microsoft Azure"
 description="Azure IoT centrální vývojář průvodce - použijte přímé metody uplatnit kód na vašich zařízeních"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Přímé metodu na zařízení (verze preview)

## <a name="overview"></a>Základní informace

Rozbočovač IoT umožňuje volat metody na zařízeních z cloudu. Metody představují požadavek odpověď interakce s podobným pozvání na HTTP zařízením úspěšné nebo nepodařilo okamžitě (po uživatelem zadaný časový), aby uživatel vědět stav hovor. To je užitečné pro scénáře místo, kam se liší podle toho, zda bylo možno reagovat, například odesláním služby SMS nakreslenými důsledku zařízení, pokud je zařízení offline (SMS právě drahé více než volání metody) zařízení kurzu okamžité akce.

Metoda si můžete představit jako vzdálené volání procedur přímo do zařízení. Pouze metody, které byly provedeny na zařízení můžou jen z cloudu. Pokud cloudu pokusí vyvolat metoda na zařízení, které nemá příslušný způsob definované, selže volání metody.

Obou metod zařízení zaměřuje jednoho zařízení. [Úlohy] [ lnk-devguide-jobs] umožňují volat metody na několika zařízeních a fronty vyvolání metody pro odpojeno zařízení.

Kdo má oprávnění **služby připojení** na IoT centrální může vyvolat metodu na zařízení.

### <a name="when-to-use"></a>Kdy použít

Metody zařízení jsou podobné [cloudu zařízení zprávám] [ lnk-devguide-messages] v tom, jak povolit back-end cloudu předávání informací do zařízení, ale se liší základní způsoby. Entity způsoby synchronní a ne trvalé, zatímco cloudu zařízení zprávy jsou asynchronní až 48 hodin životnost.

Metody podle odpovědi na žádosti o vzorek a ostatní vás trvalé. Chybějící životnosti výhody dvou okamžité když jsou ovládá zařízení:

- **Okamžité názory na způsob spuštění** znamená, že není potřeba pro správu srovnávací mezi žádostí a odpovědí.
- **Vyšší výkon** znamená, že operace mohou provést rychleji protože IoT centrální neposkytuje všechny životnost. Rozbočovač IoT umožňuje dalších hovorů metoda za jednotku než cloudu zařízení zprávy.

Cloud zařízení zprávy se nutně příkazy k zařízení, ale raději představují do cloudové služby předávání kus informací zařízení pro ni chcete pokračovat ve své volném čase a které zařízení může nebo nemusí odpověď. Shluk zařízení zprávy mají vypršení časového limitu déle (až 48 hodin) při metody platnost rychleji.

Pomocí metody zařízení pro vyvolání okamžité příkaz na zařízení a úloh pro plánované vyvolání příkazu na zařízení.

## <a name="method-lifecycle"></a>Metoda životního cyklu

Metody implementovaná v zařízení a může vyžadovat žádný nebo více vstupy v datové metoda správně vytvoření instance. Vyvolání přímý metody prostřednictvím služby webových URI (`{iot hub}/twins/{device id}/methods/`). Zaznamenání přímé metody přes tematickým MQTT specifické pro zařízení (`$iothub/methods/POST/{method name}/`). Může podporujeme metody na další síťové protokoly straně zařízení v budoucnu.

> [AZURE.NOTE] Pokud vyvoláte přímé metoda na zařízení s názvy vlastností a hodnoty může obsahovat pouze US-ASCII tisknutelného alfanumerický, s výjimkou těch následující nastavení: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Způsoby synchronní a buď úspěšné nebo po časový limit selhat (výchozí: 30 sekund, nastavit nahoru 3600 sekund). Metody popsané v interaktivní scénáře místo, kam chcete zařízení k následným akcím a pouze v případě zařízení je online a přijímání příkazy, například zapnutí light z telefonu. V těchto scénářích chcete najdete v článku okamžité úspěšném nebo neúspěšném řešení tak cloudovou službu s mohli pracovat výsledek co nejdříve. Zařízení může vrátit některé zprávy důsledku metodu, ale není potřeba pro metodu k tomu nevyzve. Není nijak zaručené na pořadí nebo libovolnou souběžné sémantiku na volání metody.

Volání metody zařízení jsou HTTP výhradně na straně cloudu a MQTT výhradně na straně zařízení.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o používání přímé metody.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Přímé metodu z app back-end

### <a name="method-invocation"></a>Vyvolání metody

Přímá metoda vyvolání na zařízení je HTTP hovory, které zahrnují:

- *Identifikátor URI* specifické pro zařízení (`{iot hub}/twins/{device id}/methods/`)
- PŘÍSPĚVEK *metodu*
- *Záhlaví* , které obsahují povolení požádat o ID typu obsahu a kódování obsahu
- Průhledný JSON *textu* v tomto formátu:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Časový limit je v sekundách. Pokud není nastavená časový limit, ve výchozím nastavení 30 sekund.
  
### <a name="response"></a>Odpověď

Back-end obdrží odpověď, která zahrnuje:

- *HTTP stavový kód*, který se používá k chybám pocházejících z centra IoT, včetně chyba 404 pro zařízení připojených nesynchronizujete
- *Záhlaví* , které obsahují etag, požádat o ID typu obsahu a kódování obsahu
- JSON *textu* v tomto formátu:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Obě `status` a `body` podle zařízení a použita k vytvoření odpovědi se zařízení vlastní stavový kód a popis.

## <a name="handle-a-direct-method-on-a-devcie"></a>Úchyt přímý metoda na devcie

### <a name="method-invocation"></a>Vyvolání metody

Zařízení dostávat přímá metoda žádosti o na téma MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

Text, který obdrží zařízení je v tomto formátu:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Požadavky na metodu jsou QoS 0.

### <a name="response"></a>Odpověď

Zařízení odešle odpovědi na `$iothub/methods/res/{status}/?$rid={request id}`, kde:

 - `status` Vlastnost je stav zařízení předávaných způsob spuštění.
 - `$rid` Vlastnost je ID žádosti z vyvolání metody dostali z centrální IoT.

Obsah je nastavený tak, že zařízení a může mít libovolnou stav.

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT centrální a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT rozbočovači MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT rozbočovači protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď se naučíte používat přímý metody, vás mohla zajímat následující téma vývojář příručka:

- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, vás mohla zajímat následující kurz IoT centrální:

- [Použití metod cloudu zařízení][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
