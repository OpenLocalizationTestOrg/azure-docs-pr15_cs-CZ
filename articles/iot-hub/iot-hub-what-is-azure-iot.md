<properties
 pageTitle="Azure řešení pro Internet činnosti | Microsoft Azure"
 description="Základní informace o IoT na Azure včetně architektury řešení vzorku a vztah k centrální IoT Azure, SDK zařízení a předkonfigurované řešení"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Další kroky

Azure centrální IoT je Azure služba, která umožňuje zabezpečené a spolehlivé obousměrnou komunikaci mezi aplikace zpět ukončení a miliony zařízení. Aplikaci back-end pro umožňuje:

- Příjem telemetrie ve velkém měřítku ze svého zařízení.
- Směrování data z vašich zařízení procesor událostí proudu.
- Příjem ukládání souborů ze zařízení.
- Odeslání cloudu zařízení příkazů pro určitá zařízení.

Použití centrální IoT implementovat vlastní řešení back-end. Kromě toho IoT centrální zahrnuje registru identity zařízení používá ke zřízení zařízeních a jejich zabezpečovací přihlašovací údaje a jejich oprávnění pro připojení k centru. Další informace o IoT centrální najdete v tématu [Co je Centrum IoT?] [lnk-iot-hub].

Jak Azure IoT centrální umožňuje standardy správy zařízení IoT vzdáleně spravovat, konfigurace a aktualizovat zařízení najdete v tématu [Přehled Azure IoT centrální správy zařízení][lnk-device-management].

Implementace klientské aplikace na širokou škálu platformy hardwaru zařízení a operačních systémů, můžete pomocí zařízení IoT SDK. Zařízení IoT SDK zahrnout knihoven, které usnadňují odesílání telemetrie rozbočovači IoT a přijímat příkazy cloudu zařízení. Při použití SDK můžete z několika síťové protokoly komunikovat s IoT centrální. Další informace najdete v tématu [informace o zařízení SDK][lnk-device-sdks].

Začněte psát některé kód a spuštění pár ukázek najdete v článku [Začínáme s IoT centrální] [ lnk-getstarted] kurz.

Možná by vás zajímaly i [Azure IoT sady][lnk-iot-suite], které je soubor předkonfigurované řešení. Sadu IoT umožňuje rychlý start a měřítko IoT projekty při řešení obvyklé scénáře IoT – například vzdálené sledování majetku správy a prediktivní údržbu.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md