<properties
 pageTitle="Příručka pro vývojáře – kvót a omezení | Microsoft Azure"
 description="Azure IoT centrální vývojář Průvodce - Popis kvóty, které platí pro centrální IoT a očekávaná omezení"
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

# <a name="reference---quotas-and-throttling"></a>Odkaz - kvót a omezení

## <a name="quotas-and-throttling"></a>Kvóty a omezení

Každý Azure předplatné může obsahovat maximálně 10 IoT rozbočovače.

Každý IoT centrální zřízení s počtem jednotek v konkrétní SKU (Další informace najdete v tématu [Azure IoT centrální ceny][lnk-pricing]). SKU a počet jednotek určit maximální kvóta denních zpráv, které můžete poslat.

Skladovou také určuje omezení limity, které IoT centrální vynucuje všechny operace.

## <a name="operation-throttles"></a>Operace omezením

Operace omezením používáte sazba, omezení, použijí se v oblasti minute a mají blokovat zneužívání. Rozbočovač IoT pokusí zamezit vrácení chyby kdykoli je to možné, ale začínala vrácení výjimek, pokud omezení porušení moc dlouho.

Toto je seznam vynucenou omezením. Hodnoty v nápovědě k rozbočovači jednotlivé.

| Omezení | Za centrální hodnota |
| -------- | ------------- |
| Identita registru operace (vytvoření, načíst, seznam, aktualizace, odstranění) | 5000/min/jednotky (S3) <br/> 100/min/jednotky (S1 a S2). |
| Připojení zařízení | 6000/sec/jednotky (S3) 120/sec/jednotky (S2), 12/sec/jednotky (S1). <br/>Minimální 100/sec. <br/> Dvě S1 jednotky jsou například 2\*12 = 24/sec, ale můžete mít minimálně 100/sec na vašich jednotkách. Pomocí devět S1 jednotky, máte 108/sec (9\*12) na vašich jednotkách. |
| Odešle zařízení do cloudu | 6000/sec/jednotky (S3) 120/sec/jednotky (S2), 12/sec/jednotky (S1). <br/>Minimální 100/sec. <br/> Dvě S1 jednotky jsou například 2\*12 = 24/sec, ale můžete mít minimálně 100/sec na vašich jednotkách. Pomocí devět S1 jednotky, máte 108/sec (9\*12) na vašich jednotkách. |
| Odešle cloudu zařízení | 5000/min/jednotky (S3), 100/min/jednotky (S1 a S2). |
| Shluk zařízení obdrží | 50000/min/jednotky (S3), 1 000/min/jednotky (S1 a S2). |
| Nahrávání souborů | 5000 soubor nahrát oznámení/min/jednotky (S3), 100 soubor odeslat oznámení/min/jednotky (S1 a S2). <br/> Identifikátory URI 10000 přidružení zabezpečení může být se pro účet Azure úložiště najednou.<br/> 10 přidružení zabezpečení URI/je možné zařízení se najednou. | 

Je důležité objasnit omezení *připojení zařízení* řídí sazbu pro niž lze vytvořit nové připojení zařízení s rozbočovači IoT a ne maximální počet současně připojená zařízení. Omezení závisí na počet jednotek, které jsou k dispozici centru.

Pokud se rozhodnete jednu jednotku S1, například dostanete omezení 100 připojení sekundu. To znamená, že se pokud chcete připojit 100 000 zařízení, trvá minimálně 1 000 sekund (přibližně 16 minut). Však může mít tolik současně připojená zařízení máte zařízení registraci v registru identity zařízení.

Podrobné informace o IoT centrální omezení chování, najdete v blogovém příspěvku [IoT centrální omezení a][lnk-throttle-blog].

>[AZURE.NOTE] Kdykoli je možné zvýšit zvýšení počtu zřizování jednotek rozbočovači IoT kvót nebo omezení omezení.

>[AZURE.IMPORTANT] Operace registru identity jsou určeny pro použití v správy zařízení a zřízení scénáře. Čtení nebo aktualizaci velkého počtu zařízení identity je podporována prostřednictvím [import a export úlohy][lnk-importexport].

## <a name="next-steps"></a>Další kroky

Další témata odkaz v této příručce vývojář IoT centrální patří:

- [Rozbočovač IoT koncové body][lnk-devguide-endpoints]
- [Je dotazovací jazyk pro twins, metody a úlohy][lnk-devguide-query]
- [Podpora IoT centrální MQTT][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md