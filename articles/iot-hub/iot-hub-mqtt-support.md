<properties
 pageTitle="Podpora IoT centrální MQTT | Microsoft Azure"
 description="Popis MQTT podpora v centrální úrovně IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>Podpora IoT centrální MQTT

Rozbočovač IoT umožňuje zařízení komunikovat s koncové body zařízení IoT rozbočovači pomocí [MQTT v3.1.1] [ lnk-mqtt-org] protokolu na port 8883 nebo MQTT v3.1.1 přes protokol WebSocket port 443. Rozbočovač IoT vyžaduje všechny komunikace zařízení zabezpečená protokolem TLS/SSL (tedy IoT centrální, které nejsou podporovány-zabezpečené připojení přes port 1883).

## <a name="connecting-to-iot-hub"></a>Připojení k centrální IoT

Zařízení můžete protokol MQTT připojit k rozbočovači IoT pomocí knihoven v [Microsoft Azure IoT SDK] [ lnk-device-sdks] nebo pomocí panelů MQTT protokol přímo.

## <a name="using-the-device-client-sdks"></a>Pomocí klienta zařízení SDK

[Zařízení klienta SDK] [ lnk-device-sdks] podporující protokol MQTT jsou k dispozici pro Java Node.js, C, C# a Python. Klient zařízení SDK používat standardní připojovací řetězec IoT centrální připojení k rozbočovači IoT. Použití protokolu MQTT, musí být klienta protokol parametr nastaven na **MQTT**. Ve výchozím nastavení klienta zařízení, které SDK připojit k rozbočovači IoT s **CleanSession** označte nastavena na hodnotu **0** a použití **QoS 1** pro výměnu zprávu s centrální IoT.

Pokud je zařízení připojené k rozbočovači IoT, klientského zařízení SDK poskytují metody, které umožňují zařízení zprávy odesílat a přijímat zprávy z rozbočovači IoT.

Následující tabulka obsahuje odkazy na ukázky podporovaných jazyků a určuje parametru lze použít k vytvoření připojení k centrální IoT pomocí protokolu MQTT.

| Jazyk                   | Parametr protokolu        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure iot zařízení mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migrace z aplikace zařízení z AMQP MQTT
Pokud používáte [zařízení klienta SDK][lnk-device-sdks], přechod z pomocí AMQP MQTT vyžaduje změna parametr protokolu inicializace klienta výše uvedenou.

Přitom, zkontrolujte, zkontrolujte následující položky:

* AMQP vrátí chyby pro mnoho podmínky, zatímco MQTT přeruší připojení. Výsledkem vaší výjimka zpracování logiky může vyžadovat některé změny.
* MQTT nepodporuje operace *Odmítnout* při příjmu [zprávy C2D][lnk-messaging]. Pokud vaše back-end potřebuje dostanete odpověď od aplikaci zařízení, zvažte použití [přímý metody][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Pomocí protokolu MQTT přímo

Pokud zařízení pomocí klienta zařízení SDK, můžete pořád připojení k veřejné zařízení koncové body pomocí protokolu MQTT. V paketu **Připojit** zařízení používejte tyto hodnoty:

- Pole **ClientId** dosáhnete **deviceId**. 
- Pole **Username** použít `{iothubhostname}/{device_id}`, kde {iothubhostname} je celé záznamy CName centru IoT.

    Například, pokud je v poli název rozbočovače IoT **contoso.azure devices.net** a název zařízení, musí být **MyDevice01**, úplné **uživatelské jméno** pole obsahovalo `contoso.azure-devices.net/MyDevice01`.

- Pole pro **heslo** použijte token přidružení zabezpečení. Formát token přidružení zabezpečení je stejná jako HTTP a AMQP protokolů:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Další informace o tom, jak generovat tokeny přidružení zabezpečení, naleznete v části zařízení tokenů [zabezpečení pomocí centrální IoT][lnk-sas-tokens].
    
    Při testování, můžete také [Zařízení Explorer] [ lnk-device-explorer] nástroj rychle vygenerovat token přidružení zabezpečení, který můžete zkopírovat a vložit do vlastního kódu:
    
    1. Přejděte na kartu **Správa** v Průzkumníku zařízení.
    2. Klikněte na tlačítko **Přidružení zabezpečení tokenu** (vpravo nahoře).
    3. Na **SASTokenForm**, vyberte svoje zařízení najdete v **DeviceID** rozevírací seznam. Nastavte **hodnotu TTL**.
    4. Klikněte na **Generovat** vytvoření tokenu.
    
    Přidružení zabezpečení token vygenerovaný má tuto strukturu:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Součást tohoto tokenu sloužit jako pole pro **heslo** se připojit pomocí MQTT:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT připojení a odpojení paketů, IoT centrální problémy události na kanál **Sledování operace** s dalšími informacemi, které vám můžou pomoct řešit problémy s připojením.

### <a name="sending-messages-to-iot-hub"></a>Odesílání zpráv IoT centrální

Po provedení úspěšné připojení zařízení můžete posílat zprávy IoT rozbočovači pomocí `devices/{device_id}/messages/events/` nebo `devices/{device_id}/messages/events/{property_bag}` jako **Název tématu**. `{property_bag}` Element umožňuje odeslání zprávy s další vlastnosti ve formátu zakódovaný url zařízení. Příklad:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] To `{property_bag}` prvek používá stejnou kódování jako řetězce dotazu v protokolu HTTP.

Můžete taky použít klientské aplikaci zařízení `devices/{device_id}/messages/events/{property_bag}` jako **bude název tématu** definovat *budou zprávy* předávané jako telemetrie zprávu.

Rozbočovač IoT nepodporuje QoS 2 zprávy. Pokud klienta zařízení publikování zprávu s **QoS 2**, IoT rozbočovači slouží k zavření připojení k síti.
Rozbočovač IoT nebude zachován zachovat zprávy. Pokud zařízení odešle zprávu příznakem **zachovat** nastavena na hodnotu 1, přidá IoT centrální **x-určovat, jestli zachovat** vlastnosti aplikace na zprávu. V tomto případě místo uchování zachovat zprávy, IoT rozbočovači předá ho back-end aplikaci.

### <a name="receiving-messages"></a>Přijímání zpráv

Pro příjem zpráv z centrální IoT, zařízení odběr pomocí `devices/{device_id}/messages/devicebound/#` jako **Téma filtr**. Víceúrovňové zástupných **#** ve filtru téma se používá pouze k zařízení chcete zobrazit další vlastnosti do pole název tématu povolit. Centrální IoT nedovoluje použití **#** nebo **?** zástupné znaky pro filtrování dílčích témat. Protože IoT centrální není univerzální zpráv zprostředkovatele pub sub, podporuje pouze dokumentovaného téma jména a tématu filtry.

Zadejte osobní zprávu, že zařízení nebudou všechny zprávy z centrální IoT předtím, než se úspěšně připojila své zařízení konkrétní koncový bod, který je ve `devices/{device_id}/messages/devicebound/#` téma filtr. Po úspěšném předplatné, začne zařízení přijímat jen cloudu zařízení zprávy odeslané do ní po dobu předplatné. Pokud připojení zařízení s příznakem **CleanSession** nastavena na **hodnotu 0**, bude v jiných relacích zachován předplatné. V tomto případě při příštím připojení s **CleanSession 0** zařízení dostanou zbývající zprávy, které byly odeslány ho během bylo zrušeno. Pokud zařízení používá **CleanSession** nastavený příznak **1** to, že neobdrží všechny zprávy od IoT rozbočovače dokud přihlásí k jeho zařízení koncového bodu.

IoT Centrum poskytuje zprávy s **Název tématu** `devices/{device_id}/messages/devicebound/`, nebo `devices/{device_id}/messages/devicebound/{property_bag}` Pokud jsou všechny zprávy vlastnosti. `{property_bag}`obsahuje zakódovaný url klíč/hodnota dvojice vlastností zprávy. Pouze aplikaci a nastavit pro uživatele systému vlastností (například **messageId** nebo **correlationId**) jsou součástí vlastností. Názvy vlastností systém mít předponu **$**, vlastnosti aplikace pomocí bez předpony původní název vlastnosti.

Pokud klienta zařízení přihlásí k tématu **QoS**2, IoT centrální uděluje maximální QoS úrovně 1 v paketu **SUBACK** . Až to bude IoT centrální doručování zpráv do zařízení pomocí QoS 1.

### <a name="additional-considerations"></a>Další informace

Jako konečný pozornost v případě potřeby můžete přizpůsobit chování protokol MQTT na straně cloudu byste měli zkontrolovat [Azure IoT protokol brány] [ lnk-azure-protocol-gateway] , díky kterému můžete nasadit vlastní protokol výkonné brány, která rozhraní přímo s IoT centrální. Protokol brány Azure IoT umožňuje přizpůsobit protokol zařízení tak, aby zahrnoval brownfield MQTT nasazení nebo jiné vlastní protokoly. Tento postup vyžaduje, však spustit a provoz brány vlastní protokol.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [poznámky MQTT podporují] [ lnk-mqtt-devguide] v Průvodci Azure IoT centrální vývojář.

Další informace o protokolu MQTT, najdete v článku [si přečtěte následující dokumentaci MQTT][lnk-mqtt-docs].

Další informace o plánování IoT rozbočovači nasazení, najdete tady:

- [Azure certifikované pro IoT zařízení katalogu][lnk-devices]
- [Podpora další protokoly][lnk-protocols]
- [Porovnání s rozbočovače události][lnk-compare]
- [Změna velikosti, HA a DR][lnk-scaling]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
