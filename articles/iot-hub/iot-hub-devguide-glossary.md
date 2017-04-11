<properties
 pageTitle="Příručka pro vývojáře – Glosář | Microsoft Azure"
 description="Glosář běžné podmínky vztahující se k centrální IoT"
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

# <a name="glossary-of-iot-hub-terms"></a>Glosář termínů IoT centrální

V tomto článku jsou uvedeny některé běžné termínů přidružené IoT rozbočovači.

## <a name="advanced-message-queueing-protocol-amqp"></a>Rozšířené zprávy přidávání do fronty Protocol (AMQP)

[AMQP](https://www.amqp.org/) je taková zpráv protokolů, které podporuje IoT Centrum pro komunikaci s zařízení. Další informace o protokolech zpráv, které podporuje IoT centrální najdete v článku [odesílání a přijímání zpráv s IoT centrální](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Shluk zařízení (C2D)

Obvykle lze odkazovat na zprávám odesílaným z centrální IoT připojené zařízení. Často tyto zprávy jsou příkazy, které pokyn zařízení určitou akci. Další informace najdete v tématu [odesílání a přijímání zpráv s IoT centrální](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Podmínky

Odkazuje na informace o stavu zařízení, jako je způsob připojení k aktuálně používán vykázání zařízení aplikaci. Zařízení můžete nahlásit taky jejich funkce. Můžete dotaz podmínku a možnost používat dvojitých zařízení.

## <a name="data-point-message"></a>Datový bod zprávy

Cloud zařízení zprávu, která obsahuje telemetrickými daty rychlost větru ATP teplotu je zpráva datový bod.

## <a name="desired-properties"></a>Požadované vlastnosti

V rámci zařízení twins požadované vlastnosti umožňují ve spojení s *vykázaného vlastnosti* synchronizace konfigurace zařízení nebo podmínka. Požadované vlastnosti lze nastavit pouze aplikací zpět ukončení a dodržovat aplikaci zařízení. 

## <a name="device-to-cloud-d2c"></a>Zařízení do cloudu (D2C)

Obvykle lze odkazovat na zprávám odesílaným z zařízení připojené k rozbočovači IoT. Tyto zprávy může být [datový bod](#data-point-message) nebo [interaktivní](#interactive-message) zprávy. Další informace najdete v tématu [odesílání a přijímání zpráv s IoT centrální](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Zařízení identity registru

[Zařízení identity registru](iot-hub-devguide-identity-registry.md) je předdefinované součástí rozbočovači IoT, který obsahuje informace o jednotlivých zařízení lze připojit k rozbočovači.

## <a name="device"></a>Zařízení

V rámci IoT zařízení je obvykle malých, výpočetní zařízení samostatně, které může shromažďovat data o či ovládacích prvků jiných zařízeních. Například může být zařízení, klimatizace sledování zařízení nebo řadiči systému a napájení ventilace v skleníkových.

## <a name="device-twin"></a>Dvojité zařízení

[Zařízení dvojitých](iot-hub-devguide-device-twins.md) je kopií v centrální IoT podmínku a konfigurace nastavení fyzické zařízení. Zařízení dvojitých slouží ke správě konfigurace fyzické zařízení.

## <a name="direct-method"></a>Přímá metoda

[Přímá metoda](iot-hub-devguide-direct-methods.md) je způsob, jak můžete aktivovat metodu spustit na zařízení vyvoláním rozhraní API na vaše Centrum IoT.

## <a name="event-hub-compatible-endpoint"></a>Koncový bod kompatibilní s prohlížečem centrální události

Číst zařízení do cloudu zprávy poslané na vaše Centrum IoT, můžete připojení k koncový bod na vaše centrum a přečtených zprávách: tyto pomocí způsobem kompatibilní s prohlížečem události centrální. Událost kompatibilní s prohlížečem centrální metody patří pomocí SDK rozbočovače událostí a Azure toku analýzy.

## <a name="field-gateway"></a>Pole brány

Pole brány umožňuje připojení zařízení, která se nemůže připojit přímo k centrální IoT a je obvykle nasazený místně s svých zařízeních. Další informace najdete v tématu [Co je Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Úlohy

Vaše řešení back-end slouží úlohy plánovat a sledovat aktivity na několika zařízeních registrovaný u rozbočovače IoT. Aktivity zahrnují aktualizace dvojitých požadované vlastnosti zařízení, aktualizace zařízení dvojitých značky a vyvolání přímé metody.

## <a name="protocol-gateway"></a>Protokol brány

Protokol brána je obvykle nasazený v cloudu a poskytuje protokol překladatelské služby pro zařízení s připojením k centrální IoT. Další informace najdete v tématu [Co je Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktivní zprávy

Interaktivní zprávy je cloudu zařízení zprávy, která aktivuje okamžité akce v aplikaci back-end. Zařízení můžou například odesílat upozornění o chybě, která by měla být přihlášení k lyncu automaticky do CRM systému.

## <a name="iot-hub"></a>Rozbočovač IoT

Centrální IoT je plně spravovaných Azure služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a back-end řešení. Další informace najdete v tématu [Co je Centrum IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure sadu IoT balíčky společně více služby Azure pomocí předkonfigurované řešení. Tyto předem řešení umožňují začít pracovat s začátku do konce implementace obvyklé scénáře IoT. Další informace najdete v tématu [Co je sada IoT Azure?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Úlohy

[Úlohy](iot-hub-devguide-jobs.md) v centrální IoT umožňuje provádět operace, například firmwaru upgradu na několika zařízeních připojené k vaší rozbočovači.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) je taková zpráv protokolů, které podporuje IoT Centrum pro komunikaci s zařízení. Další informace o protokolech zpráv, které podporuje IoT rozbočovači najdete v článku [odesílání a přijímání zpráv s IoT rozbočovači](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Nahlášeného vlastnosti

V kontextu zařízení twins nahlášeného vlastnosti umožňují ve spojení s *požadované vlastnosti* synchronizace konfigurace zařízení nebo podmínka. Nahlášeného vlastnosti můžete nastavit jenom pomocí zařízení můžou být číst a zpracovávat pomocí aplikace back-end.

## <a name="tags"></a>Značky

V rámci devcie twins značky jsou zařízení metadata ukládají a načítané aplikaci back-end ve formátu JSON dokumentu. Značky nejsou viditelné pro aplikací na zařízení.

## <a name="system-properties"></a>Vlastnosti systému

V rámci zařízení twins vlastnosti systému jsou jen pro čtení a informace o použití zařízení například poslední aktivity čas a připojení stavu.