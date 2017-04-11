<properties
 pageTitle="Příručka vývojáře – koncové body IoT centrální | Microsoft Azure"
 description="Azure IoT centrální vývojář Průvodce - referenční informace o IoT centrální koncové body"
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

# <a name="reference---iot-hub-endpoints"></a>Odkaz - IoT centrální koncové body

## <a name="list-of-iot-hub-endpoints"></a>Seznam IoT centrální koncové body

Azure centrální IoT je služba více klienta, která poskytuje jeho funkce různé účastníky. Na následujícím obrázku vidíte různé koncové body, zda IoT rozbočovači zpřístupňuje.

![Rozbočovač IoT koncové body][img-endpoints]

Následuje popis koncové body:

* **Zprostředkovatele prostředků**. Zprostředkovatel zdroje IoT Centrum poskytuje [Správce prostředků Azure] [ lnk-arm] rozhraní, které umožňuje Azure předplatné vlastníci k vytváření a odstraňování IoT rozbočovače a aktualizovat vlastnosti centrální IoT. Vlastnosti IoT centrální řídí [zásady zabezpečení na úrovni Centrální][lnk-accesscontrol], namísto řízení přístupu zařízení a funkční možnosti cloudu zařízení a zařízení do cloudu zasílání zpráv. Zprostředkovatele IoT Centrum zdrojů můžete také export [zařízení identit][lnk-importexport].
* **Správa identit zařízení**. Každý IoT Centrum poskytuje sadu koncových bodů HTTP ZBÝVAJÍCÍ správy identit zařízení (vytvoření načíst, aktualizovat a odstranit). [Zařízení identit] [ lnk-device-identities] se používají pro zařízení ověřování a řízení přístupu.
* **Správa dvojitých zařízení**. Každý IoT Centrum poskytuje sadu koncový bod HTTP ZBÝVAJÍCÍ webových služeb dotaz a aktualizovat [zařízení twins] [ lnk-twins] (aktualizovat značky a vlastnosti).
* **Úlohy správy**. Každý IoT Centrum poskytuje sadu webových služeb HTTP ZBÝVAJÍCÍ koncový bod k vytváření dotazů a správa [úloh][lnk-jobs].
* **Koncové body zařízení**. U každého zařízení v registru identity zařízení zřízení IoT Centrum poskytuje sadu koncové body, které zařízení můžete použít k odesílání a přijímání zpráv:
    - *Odeslání zprávy zařízení do cloudu*. Použít tento koncový bod k [odesílání zpráv zařízení do cloudu][lnk-d2c].
    - *Přijmout cloudu zařízení zprávy*. Zařízení používá tento koncový bod cílových [cloudu zařízení zprávy][lnk-c2d].
    - *Zahájení odesílání souborů*. Zařízení používá tento koncový bod pro příjem Azure úložiště přidružení zabezpečení URI z centrální IoT k [nahrání souboru][lnk-upload].
    - *Načtení a aktualizace dvojitých vlastnosti*. Zařízení používá tento koncové body pro přístup k jeho [zařízení dvojitých][lnk-twins]na vlastnosti.
    - *Přijímání přímé metody žádosti*. Zařízení používá tento koncové body poslouchat [přímé metody][lnk-methods]na žádosti o.

    Tyto koncové body jsou k dispozici pomocí [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 a [AMQP 1.0] [ lnk-amqp] protokoly. Všimněte si, že AMQP také k dispozici prostřednictvím [WebSockets] [ lnk-websockets] na port 443.
    
    Jsou dostupné jenom prostřednictvím [MQTT v3.1.1]twins' a metody endpoins[lnk-mqtt].

* **Koncové body služby**. Každý IoT Centrum poskytuje sadu koncové body, které vaše aplikace back-end slouží ke komunikaci s zařízení. Tyto koncové body jsou aktuálně jenom vystaveného pomocí [AMQP] [ lnk-amqp] Protocol (protokol), s výjimkou koncového bodu vyvolání metody, který se zobrazí prostřednictvím protokolu HTTP 1.1.
    - *Přijímání zpráv zařízení do cloudu*. Tento koncový bod je kompatibilní se službou [Azure události rozbočovače][lnk-event-hubs]. Služba back-end můžete používat ke čtení všech [zpráv zařízení do cloudu] [ lnk-d2c] odeslány pomocí svých zařízeních.
    - *Cloud zařízení zprávy odesílat a přijímat potvrzení doručení*. Tyto koncové body povolit vaše aplikace back-end posílat spolehlivé [cloudu zařízení zprávy][lnk-c2d]a získat odpovídající příjemce nebo potvrzení vypršení platnosti.
    - *Přijímání oznámení souborů*. Tento zpráv koncový bod umožňuje přijímat oznámení o při zařízení úspěšně nahrát soubor. 
    - *Vyvolání přímé metody*. Tento koncový bod umožňuje službě back-end vyvolat [Přímá metoda] [ lnk-methods] na zařízení.

V [Centrální IoT rozhraní API a SDK] [ lnk-sdks] článek obsahuje popis různých způsobů zobrazíte tyto koncové body.

Nakonec je důležité mít na paměti, že všechny koncové body IoT centrální používat [TLS] [ lnk-tls] protokol a bez koncového bodu někdy vystaven na bez šifrování/nezabezpečenou kanály.

## <a name="field-gateways"></a>Pole brány

V řešení IoT *pole brány* najdete mezi zařízeních a koncové body IoT centrální. Je obvykle umístěn zavřít svých zařízeních. Vaše zařízení komunikovat přímo pole brány pomocí protokolu nepodporuje zařízení. Pole brány připojuje k centrální IoT koncový bod pomocí protokol, který je podporován IoT centrální. Brána pole může být vysoce speciální hardware nebo Nízká power počítač se systémem software, který začátku do konce scénáře, u kterého je určená brány se musí splnit.

Můžete použít v [Azure IoT brány SDK] [ lnk-gateway-sdk] implementovat brány pole. Tento SDK nabízí určité funkce například možnost multiplex komunikace z víc zařízení na stejné připojení k rozbočovači IoT.

## <a name="next-steps"></a>Další kroky

Další témata odkaz v této příručce vývojář IoT centrální patří:

- [Je dotazovací jazyk pro twins, metody a úlohy][lnk-devguide-query]
- [Kvóty a omezení][lnk-devguide-quotas]
- [Podpora IoT centrální MQTT][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md