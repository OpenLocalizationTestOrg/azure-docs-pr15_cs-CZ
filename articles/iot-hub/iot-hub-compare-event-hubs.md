<properties
 pageTitle="Porovnání Azure centrální IoT na Azure události rozbočovače | Microsoft Azure"
 description="Porovnání služby Azure IoT centrální a Azure události rozbočovače zvýraznění rozdíly ve funkcích a případy použití."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Porovnání Azure centrální IoT a rozbočovače Azure události

Jednou z případy použití hlavního IoT rozbočovače je shromáždění telemetrie ze zařízení. Z tohoto důvodu IoT rozbočovači je často ve srovnání s [Azure události rozbočovače][]. Jako centrální IoT je událost rozbočovače události zpracování služba, která umožňuje událostí a telemetrie průniku do cloudu v rozsáhlé měřítku s nízkou latence a vysoký spolehlivost.

Ale služby mít mnohé rozdíly, které jsou uvedené v následující tabulce.

| Oblast | Rozbočovač IoT | Rozbočovače události |
| ---- | ------- | ---------- |
| Komunikace | Umožňuje zařízení do cloudu a cloudu zařízení zasílání zpráv. | Umožňuje pouze události průniku (obvykle za zařízení do cloudu scénáře). |
| Podpora protokolu zařízení | Podporuje MQTT, AMQP, AMQP přes WebSockets a HTTP. Navíc IoT centrální označené jako pracuje s [Azure IoT protokol brány][lnk-azure-protocol-gateway], provedení přizpůsobitelné protokolu brány na podporu vlastní protokoly. | Podporuje AMQP, AMQP přes WebSockets a HTTP. |
| Zabezpečení | Poskytuje identity na zařízení a řízení odvolatelný přístupu. Najdete v [části zabezpečení Průvodce IoT centrální vývojář]. | Poskytuje události nevyžádaného rozbočovače [zásady přístupu sdílené][Event Hubs - security], s omezenou odvolání podporu prostřednictvím [zásad vydavatele][Event Hubs publisher policies]. Řešení IoT často nutné implementovat vlastní řešení, které podporují přihlašovací údaje na zařízení a falšování anti míry. |
| Operace sledování | Umožňuje IoT řešení pro přihlášení k odběru celá řada událostí řízení a připojení identity zařízení například jednotlivé zařízení ověřování chyby, omezení a chybný formát výjimky. Tyto události umožňují rychle identifikovat problémy s připojením k úrovni jednotlivých zařízení. | Poskytuje jen agregovat metriky. |
| Stupnice | Je optimalizováno pro podporu miliony současně připojených zařízení. | Podporuje více omezený počet souběžné připojení – až 5 000 připojení AMQP podle [kvót Bus služby Azure][]. Na druhé straně události rozbočovače vám umožní určit oddíl každé zprávy odeslané. |
| Zařízení SDK | Poskytuje [zařízení SDK] [ Azure IoT Hub SDKs] pro velké různé platformy a jazyky. | Je podporován .NET ani C. Obsahuje také AMQP a HTTP odeslat rozhraní. |
| Odeslání souboru | Umožňuje IoT řešení nahrát soubory ze zařízení do cloudu. Obsahuje koncový bod soubor oznámení pro operace sledování kategorie pro podporu ladění a integrace pracovního postupu. | Používá vzorek zaškrtnutí deklarace ručně vyžádat soubory ze zařízení a poskytnout úložiště klíč pro transakce zařízení. |

Shrnuto, i když je pouze případu použití průniku telemetrie zařízení do cloudu, IoT rozbočovač poskytuje služba, která je speciálně IoT zařízení připojení. Budou dál rozbalte tvrzení hodnotu pro podobnému sledu s IoT funkcím. Událost rozbočovače je určený pro událost průniku v měřítku obrovské, obě v kontextu mimo datacentra a uvnitř datacentra scénáře.

Není běžné používání IoT centrální a událostí rozbočovače ve stejném řešení – centrální IoT zpracovává komunikaci zařízení do cloudu, kde události rozbočovače zpracovává průniku pozdější fázi událostí do modulů řeč v reálném čase zpracování.

## <a name="next-steps"></a>Další kroky

Další informace o plánování IoT centrální nasazení najdete v tématu [měřítko, HA a DR][lnk-scaling].

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[Rozbočovače Azure události]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Část zabezpečení příručky pro vývojáře IoT centrální]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure kvóty Bus služby]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
