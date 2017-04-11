<properties
 pageTitle="Povolit spravovanou zařízení za brány IoT | Microsoft Azure"
 description="Pokyny pro téma pomocí brány IoT vytvořené pomocí SDK brány IoT spolu s zařízení spravuje IoT centrální."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Povolit spravovanou zařízení za IoT brány

## <a name="basic-device-isolation"></a>Základní zařízení izolace

Organizace často IoT bran zvýšit pomocí celkové zabezpečení jejich IoT řešení. Některá zařízení potřebujete odeslání dat do cloudu, ale nejsou sami chrání před hrozeb na Internetu. Tato zařízení z vnější můžete chránit tím, že je komunikovat s vnější prezentace pomocí brány.

Brána je umístěn na hranici mezi bezpečné prostředí a otevřete internet. Zařízení mluvit brány a brány předá zprávy podél do cíle správné cloudu. Brány je zesílený proti externí vláknech blokuje neoprávněným požadavky, umožňuje oprávněnými v vazbou přenos a předá tento přenos v vazbou na správné zařízení.

![][1]

Můžete také vložit zařízení, která můžete chránili za brány přidané vrstvy cenného papíru. V tomto scénáři stačí ponechání brány opravené před nejnovější chybami namísto aktualizace OS na všech zařízeních s operačním systémem.

## <a name="isolation-plus-intelligence"></a>Izolace plus měřítka

Zesílený směrovači je dostatečně brány jednoduše vyčlenění zařízení. Řešení IoT však často vyžadují brány poskytuje další intelligence než jednoduše izolace zařízení. Chcete například zařízení můžete spravovat z cloudu. Jste v aplikaci [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), Správa protokol standardní zařízení, pro součást správy cloudu řešení. Však zařízení odesláno telemetrie pomocí není TCP/IP podporou protokolu. Kromě toho zařízení plodiny velké množství dat a chcete nahrát filtrované podmnožinu telemetrie. Je možné vytvářet řešení, které zahrnuje brány IoT může nakládání s dvěma odlišné datové proudy. Brány by měl:

-   Princip **Telemetrie**, filtrovat a pak ho nahrajte do cloudu pomocí brány. Brána je už směrovačem jednoduché předává dat mezi zařízení a cloudu.

-   Jednoduše exchange **LWM2M zařízení správy dat** mezi zařízení a cloudu. Brána není nutné rychlé porozumění datům chodit do ní a jenom je potřeba zkontrolujte, jestli že data prochází přepínat mezi zařízení a cloudem.

Následující obrázek znázorňuje tento scénář:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Řešení: Správa zařízení Azure IoT a SDK IoT brány 

Veřejnosti náhled verzi [Azure IoT správy zařízení] [ lnk-device-management] a tento scénář povolení beta verzi [Azure IoT brány SDK] . Brány zpracuje každou toku dat následujícím způsobem:

-   **Telemetrie**: SDK brány IoT můžete použít k vytváření kanálů, což rozumí, filtrů a odešle telemetrickými daty do cloudu. SDK brány IoT obsahuje kód implementující části tohoto kanálu jménem autora. Další informace o architektura SDK můžete najít v [SDK brány IoT – Začínáme] [ lnk-gateway-get-started] kurz.

-   **Správa zařízení**: Správa Azure zařízení poskytuje LWM2M klienta, který běží na zařízení, jakož i v cloudu rozhraní pro vydávání příkazy pro správu zařízení.
    
    Zvláštní logiku na brány nevyžadují, protože nemusí zpracuje LWM2M data mezi zařízení a rozbočovače IoT. Můžete povolit sdílení připojení k Internetu, funkce mnoho moderní operační systémy, na brány výměně LWM2M data. Vhodné operační systém v tomto scénáři můžete zvolit, protože SDK brány IoT podporuje různých operačních systémech. Tady jsou pokyny pro povolení sdílení na [Windows 10] a [systémem Ubuntu], dvě z mnoha podporované operační systémy připojení k Internetu.

Následující obrázek ukazuje nejvyšší úrovně architektura umožňuje tento scénář správou [zařízení Azure IoT] [ lnk-device-management] a [Azure IoT brány SDK].

![][3]

## <a name="next-steps"></a>Další kroky

Informace o tom, jak používat bránu SDK IoT najdete v tématu následující kurzy:

- [IoT brány SDK: Začínáme s používáním Linux][lnk-gateway-get-started]
- [IoT brány SDK – odesílání zpráv zařízení do cloudu pomocí simulovaný zařízení, které používá Linux][lnk-gateway-simulated]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure brány IoT SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Se systémem Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md