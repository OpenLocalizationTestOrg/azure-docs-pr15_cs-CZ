<properties
 pageTitle="Příručka vývojáře – IoT centrální SDK | Microsoft Azure"
 description="Azure IoT centrální vývojář průvodce – informace a odkazy na jednotlivé SDK zařízení a služby Azure IoT centrální."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>Rozbočovač IoT SDK

## <a name="iot-hub-device-sdks"></a>Rozbočovač IoT zařízení SDK

Zařízení Microsoft Azure IoT SDK obsahovat kód, který usnadňuje vytváření zařízení a aplikace, které připojení k a je spravováno službou Azure IoT centrální.

Následující zařízení IoT SDK jsou k dispozici ke stažení z GitHub:

- [Azure IoT zařízení SDK C] [ lnk-c-device-sdk] napsané v jazyce C ANSI (C99) přenosnost a obecných platformy kompatibilitu.
- [IoT zařízení Azure SDK pro .NET][lnk-dotnet-device-sdk]
- [IoT zařízení Azure SDK jazyka Java][lnk-java-device-sdk]
- [Azure IoT zařízení SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT zařízení SDK Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Zobrazit soubor Readme pro soubory v úložištích GitHub informace o používání jazyka a správci specifické pro platformu balíčku nainstalovat binární soubory a závislosti na vašem počítači vývoj.

## <a name="os-platforms-and-hardware-compatibility"></a>Funkce pro kompatibilitu s operačním systémem platformy a hardwaru

Podrobnosti o kompatibilitě SDK s konkrétní hardwarových zařízeních, najdete v článku [Azure certifikované pro katalog zařízení IoT][lnk-certified].

## <a name="iot-hub-service-sdks"></a>Rozbočovač IoT služby SDK

SDK služby Microsoft Azure IoT obsahovat kód, který usnadňuje vytváření aplikací, které vzájemně spolupracují přímo s IoT centrální správy zařízení a zabezpečení.

Následující služby IoT SDK jsou k dispozici ke stažení z GitHub:

- [IoT služby Azure SDK pro .NET][lnk-dotnet-service-sdk]
- [IoT služby Azure SDK pro Node.js][lnk-node-service-sdk]
- [IoT služby Azure SDK jazyka Java][lnk-java-service-sdk]

> [AZURE.NOTE] Zobrazit soubor Readme pro soubory v úložištích GitHub informace o používání jazyka a správci specifické pro platformu balíčku nainstalovat binární soubory a závislosti na vašem počítači vývoj.

## <a name="azure-iot-gateway-sdk"></a>Azure brány IoT SDK

Tento Azure IoT brány SDK obsahuje infrastrukturu a moduly pro vytvoření IoT brány řešení. Můžete rozšířit SDK pro vytvoření brány k žádný scénář začátku do konce.

Můžete si stáhnout [Azure IoT brány SDK] [ lnk-gateway-sdk] z GitHub.

## <a name="online-api-reference-documentation"></a>Online referenční dokumentaci rozhraní API

Následuje seznam odkazů na online rozhraní API odkaz si přečtěte následující dokumentaci pro zařízení, přihlašovacích údajů a knihoven brány Azure IoT:

- [Internet věcí (IoT) .NET][lnk-dotnet-ref]
- [Rozbočovač IoT REST][lnk-rest-ref]
- [Azure IoT zařízení SDK C][lnk-c-ref]
- [IoT zařízení Azure SDK jazyka Java][lnk-java-ref]
- [IoT služby Azure SDK jazyka Java][lnk-java-service-ref]
- [Azure IoT zařízení SDK Node.js][lnk-node-ref]
- [IoT služby Azure SDK pro Node.js][lnk-node-service-ref]
- [Azure IoT brány SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Další kroky

Další témata odkaz v této příručce vývojář IoT centrální patří:

- [Rozbočovač IoT koncové body][lnk-devguide-endpoints]
- [Je dotazovací jazyk pro twins, metody a úlohy][lnk-devguide-query]
- [Kvóty a omezení][lnk-devguide-quotas]
- [Podpora IoT centrální MQTT][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md