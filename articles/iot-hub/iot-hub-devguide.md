<properties
 pageTitle="Příručka témata IoT rozbočovače | Microsoft Azure"
 description="Azure Průvodce IoT centrální vývojář, který obsahuje koncové body IoT Centrum zabezpečení, registru identity zařízení, Správa zařízení a zasílání zpráv"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT centrální vývojář Průvodce

Azure centrální IoT je plně spravovaných služba, která pomáhá povolit spolehlivý a zabezpečené obousměrné komunikaci mezi miliony IoT zařízení a ukončení aplikace zpět.

Azure centrální IoT umožňuje:

* Zabezpečená komunikace pomocí na zařízení zabezpečovací přihlašovací údaje a řízení přístupu.
* Spolehlivé zařízení do cloudu a cloudu zařízení hyper měřítko zasílání zpráv.
* Snadno zařízení připojení s knihovnami zařízení pro Nejoblíbenější jazyky a platformách.

Tato příručka vývojář IoT centrální obsahuje v následujících článcích:

- [Odesílání a přijímání zpráv s IoT centrální] [ devguide-messaging] popisuje funkce zasílání zpráv (zařízení do cloudu a cloudu zařízení), které poskytuje IoT centrální. Článek obsahuje také informace o tématech, jako jsou jednotlivými formáty zpráv a protokoly podporované komunikaci a číslo portu, které používají.
- [Nahrání souborů ze zařízení] [ devguide-upload] popisuje, jak nahrát soubory ze zařízení. Článek obsahuje taky informace o tématech třeba oznámení, že proces uplaod můžete poslat.
- [Správa identity zařízení v centrální IoT] [ devguide-identities] popisuje, co každý IoT Centrum zařízení identity registru informace jsou uloženy a jak můžete zpřístupnit a upravovat.
- [Řízení přístupu k centrální IoT] [ devguide-security] popisuje model zabezpečení slouží k udělit přístup k funkcím IoT centrální obě zařízení a ke cloudové součásti. Článek obsahuje informace o používání tokeny a certifikáty X.509 a podrobnosti, které můžete udělit oprávnění.
- [Použití twins zařízení pro synchronizaci stavu a konfigurace] [ devguide-device-twins] popisuje koncept *dvojitých zařízení* a k dokumentům funkci poskytuje například synchronizace zařízení s jeho dvojitých. Článek obsahuje informace o data uložená v dvojitých zařízení.
- [Vyvolání přímý metody na zařízení] [ devguide-directmethods] popisuje životním cyklu přímý metody, informace o tom, jak volat metody na zařízení z aplikace back-end a obsloužení metodu přímý na vašem zařízení.
- [Naplánovat úlohy na několika zařízeních] [ devguide-jobs] popisuje, jak můžou plánovat úlohy na několika zařízeních. Tento článek popisuje, jak posílat úkoly, které provádět úkoly, jako je spuštění přímé metody aktualizace devcie pomocí dvojitých devcie. Také popisuje, jak k vytvoření dotazu stavu projektu.
- [Odkaz - koncové body IoT centrální] [ devguide-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy. V článku také popisuje, jak můžete pomocí pole brány povolit některých zařízeních se připojit k koncové body IoT centrální.
- [Odkaz - je dotazovací jazyk pro twins, metody a úlohy] [ devguide-query] popisuje tento dotazovací jazyk, který umožňuje načítat informace z rozbočovače o úlohy a twins zařízení.
- [Odkaz - kvót a omezení] [ devguide-quotas] shrnuje kvóty nastavené v centrální IoT služby a omezení chování můžete očekávat zobrazíte, když překročení kvóty.
- [Odkaz - zařízení a přihlašovacích údajů SDK] [ devguide-sdks] seznamy SDK můžete vyvíjet aplikace zařízení a služeb, které vzájemně spolupracují s rozbočovače IoT. Článek obsahuje odkazy na online dokumentaci k rozhraní API.
- [Odkaz – Podpora IoT centrální MQTT] [ devguide-mqtt] obsahuje podrobné informace o tom, jak IoT centrální podporuje protokol MQTT. Tento článek popisuje podporu protokolu MQTT předdefinované SDK a obsahuje informace o použití protokolu MQTT přímo.
- [Glosář] [ devguide-glossary] seznam běžných IoT centrální souvisí podmínky.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

