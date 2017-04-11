<properties
     pageTitle="Konfigurace nahrávání souboru pomocí portálu Azure | Microsoft Azure"
     description="Základní informace o konfiguraci nahrávání souboru pomocí portálu Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Konfigurace ukládání souborů pomocí portálu Azure

## <a name="file-upload"></a>Odeslání souboru

Použití [funkce v centrální IoT nahrát soubor][lnk-upload], musíte nejdřív přidružit účet Azure úložiště vaše centrum. Vyberte **Nahrát soubor** nastavení zobrazte seznam vlastností nahrát soubor rozbočovače IoT upravované.

![][13]

**Kontejner úložiště**: pomocí portálu Azure vyberte kontejneru objektů blob Azure úložiště účtu v Azure na aktuálního předplatného přidružit rozbočovače IoT. V případě potřeby můžete vytvořit účet Azure úložiště na **úložiště účty** zásuvné a objektů blob kontejneru na zásuvné **kontejnerů** . Rozbočovač IoT automaticky vygeneruje přidružení zabezpečení URI s oprávnění k zápisu do tento kontejner objektů blob pro zařízení při nahrávání souborů.

![][14]

**Přijímání upozornění pro nahrané soubory**: povolení nebo zakázání oznámení o odeslání souboru pomocí přepínacího tlačítka.

**Přidružení zabezpečení TTL**: Toto nastavení je čas to-live z přidružení zabezpečení URI vrácené IoT Centrum zařízení. Ve výchozím nastavení hodinu, ale můžete přizpůsobit a dalších hodnot pomocí posuvníku.

**Soubor oznámení, že nastavení výchozí TTL**: time-to-live souboru oznámení webu upload před jeho vypršela. Ve výchozím nastavení jeden den, ale můžete přizpůsobit a dalších hodnot pomocí posuvníku.

**Soubor oznámení o doručení maximální počet**: oznámení webu upload toho, kolikrát centru IoT snaží poskytovat souboru. Ve výchozím nastavení 10, ale můžete přizpůsobit a dalších hodnot pomocí posuvníku.

![][15]

## <a name="next-steps"></a>Další kroky

Další informace o možnostech nahrát soubor IoT centrální najdete v tématu [nahrání souborů ze zařízení] [ lnk-upload] v příručce pro vývojáře.

Tyto odkazy vedou na další informace o správě Azure IoT centrální:

- [Hromadné Správa IoT zařízení][lnk-bulk]
- [Použití metriky][lnk-metrics]
- [Operace sledování][lnk-monitor]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]
- [Zabezpečené IoT řešení od základu nahoru][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md