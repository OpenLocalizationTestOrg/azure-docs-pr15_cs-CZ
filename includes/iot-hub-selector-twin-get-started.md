> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Úvod

Twins zařízení jsou JSON dokumenty, které obsahují informace o stavu zařízení (metadata konfigurace a podmínky). Rozbočovač IoT zůstává v dvojitých zařízení pro každé zařízení, které jste připojení k centrální IoT.

Použijte twins zařízení na:

* Obsahují zařízení metadata ze svého back-end.
* Aktuální informace o stavu, jako jsou dostupné možnosti a podmínky (například připojení metoda používaná) zprávy z aplikace pro zařízení.
* Synchronizujte stav dlouhé spuštěné pracovní postupy (například aktualizace firmwaru a konfigurace) mezi aplikací zařízení a back-end.
* Dotaz zařízení metadata, konfigurace nebo stavu.

> [AZURE.NOTE] Zařízení twins slouží k synchronizaci a dotazování konfigurace zařízení a podmínky. Pomocí [zařízení do cloudu zpráv] [ lnk-d2c] řady událostí označen časovým razítkem (například založená na čase senzor datové proudy telemetrie) a [metod cloudu zařízení] [ lnk-methods] pro interaktivní ovládací prvek zařízení, třeba na ventilace pomocí funkčně uživatelem řízené aplikace.

Twins zařízení uložené v centrální IoT a obsahují:

* *značky*zařízení metadata dostupné jen back-end;
* *požadované vlastnosti*JSON objekty upravitelné back-end a lze zobrazit pomocí zařízení; a
* ukončení *nahlášeného vlastnosti*JSON objekty měnit podle aplikace zařízení a čitelné na pozadí. Značky a vlastnosti nesmí obsahovat pole, ale může být vnořeno objekty.

![][img-twin]

Aplikace back-end navíc můžete dotaz zařízení twins podle výše uvedených dat.
V nápovědě k [Vysvětlení informací na zařízení twins] [ lnk-twins] Další informace o zařízení twins a [IoT centrální dotazovací jazyk] [ lnk-query] odkaz pro dotaz.

> [AZURE.NOTE] V tomto okamžiku jsou dostupné pouze z zařízení připojujících se k rozbočovači IoT pomocí protokolu MQTT twins zařízení. Získáte [podporují MQTT] [ lnk-devguide-mqtt] článku najdete pokyny, jak chcete převést existující zařízení aplikaci používat MQTT.

Tento kurz se dozvíte, jak chcete:

- Vytvoření back-end aplikace, která přidá *značek* do dvojitých zařízení a simulovaný zařízení, které hlásí jejího připojení kanálu *vykázaného vlastnost* v dvojitých zařízení.
- Dotaz zařízení z aplikace back-end pomocí filtrů na značky a vlastnosti dřív vytvořili.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md