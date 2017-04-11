<properties
   pageTitle="Azure brány protokol IoT | Microsoft Azure"
   description="Popisuje, jak používat bránu protokol Azure IoT rozšířit funkce a podpora protokolu rozbočovače IoT Azure."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Podpora další protokoly pro centrální IoT

Azure centrální IoT nativně podporuje komunikace přes protokoly MQTT, AMQP a HTTP. V některých případech zařízení nebo pole bran nebudete moct používat jedním z těchto standardní protokolů a budou vyžadovat protokol přizpůsobení. V takovém případě můžete použít vlastní brány. Vlastní brány můžete povolit přizpůsobení protokol pro koncové body IoT centrální přemostění příchozích a odchozích IoT centrální. [Azure IoT protokol brány](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) jako vlastní brány umožňuje povolit přizpůsobení protokol pro centrální IoT.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protokol brány

Brána protokol Azure IoT je rámec pro přizpůsobení Protocol (protokol), které je určený pro vysokou – měřítko, obousměrnou komunikaci zařízení s IoT centrální. Protokol brána je předávací komponentu, která přijímá zařízení připojení přes určitý protokol. Přemosťuje umožnění datových přenosů do IoT rozbočovači průběhu AMQP 1.0. Protokol brány Azure IoT je k dispozici jako projekt otevřít zdroj software poskytuje flexibilitu pro přidání podpory pro řadu protokoly a protokol verze.

Pomocí služby Azure Cloud Services pracovníka role nástroje můžete nasazovat protokol brány v Azure vysoce scalable způsobem. Protokol brány kromě toho můžete nasazenou v místním prostředím, například pole brány.

Protokol brány Azure IoT obsahuje adaptér MQTT protokol, který umožňuje přizpůsobit chování protokol MQTT případné potíže. Protože IoT centrální podporuje předdefinované MQTT v3.1.1 Protocol (protokol), pouze zvažte použití adaptér protokolu MQTT, pokud máte třeba pro přizpůsobení protokol nebo konkrétní požadavky pro další funkce.

Adaptér MQTT také ukazuje programovacího modelu pro vytváření adaptéry protokol pro jiné protokoly. Kromě toho programovací model Azure IoT protokol brány umožňuje provádět zapojte vlastní součásti pro specializované zpracování například vlastní ověřování, transformace zprávy, komprese/dekomprese nebo šifrování/dešifrování komunikace mezi zařízeními a IoT centrální.

Dosažení větší flexibility protokol brány a implementaci MQTT jsou součástí projekt otevřít zdroj software. Umožňuje provádět přizpůsobení provedení podle potřeby.

## <a name="next-steps"></a>Další kroky

Další informace o Azure IoT protokol brány a jak pomocí a ho jako součást vašeho řešení IoT nasadit, najdete tady:

* [Azure úložiště brány na GitHub IoT Protocol (protokol)](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT protokol brány vývojář Průvodce](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Další informace o plánování IoT centrální nasazení, najdete tady:

- [Porovnání s rozbočovače události][lnk-compare]
- [Změna velikosti, HA a DR][lnk-scaling]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
