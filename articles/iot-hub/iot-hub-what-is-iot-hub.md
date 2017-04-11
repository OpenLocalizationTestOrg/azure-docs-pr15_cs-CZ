<properties
 pageTitle="Azure přehled centrální IoT | Microsoft Azure"
 description="Přehled služby Azure IoT centrální: co je Centrum iot, připojení zařízení, internet věci komunikace vzorků a pomáhat služby komunikace"
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
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Co je Centrum IoT Azure?

Vítá vás Centrum Azure IoT. Tento článek obsahuje přehled centrální IoT Azure a popisuje, proč byste měli používat tuto službu implementovat řešení pro Internet věcí (IoT). Azure centrální IoT je plně spravovaných služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a back-end řešení. Azure IoT centrální:

- Poskytuje spolehlivé zařízení do cloudu a cloudu – na zařízení zpráv ve velkém měřítku.
- Umožňuje zabezpečená komunikace pomocí na zařízení zabezpečovací přihlašovací údaje a řízení přístupu.
- Poskytuje rozsáhlé sledování zařízení připojení a události, Správa identit zařízení.
- Zahrnuje knihovny zařízení pro Nejoblíbenější jazyky a platformách.

Tento článek [porovnání IoT centrální a událostí rozbočovače] [ lnk-compare] popisuje důležité rozdíly mezi těmito dvěma službami a zvýrazní výhody používání IoT centrální v IoT řešení.

![Azure centrální IoT jako brány cloudu v Internetu věci řešení][img-architecture]

> [AZURE.NOTE] Podrobný popis architektury IoT, najdete v článku [Microsoft Azure IoT odkaz architektura][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Problémy při připojení zařízení IoT

Rozbočovač IoT a knihoven zařízení umožňují zahájit výzvy problémy se spolehlivým a zabezpečené připojení zařízení k řešení back-end. IoT zařízení:

- Vložený systémy bez operátor oblasti lidských zdrojů, jsou často.
- Může být ve vzdáleném umístění, kde je drahé fyzický přístup.
- Může být pouze dostupné prostřednictvím back-end řešení.
- Omezená výkon a zpracování zdroje.
- Možná bude potřeba připojení k síti chybovému pomalé nebo drahé.
- Může být nutné používat aplikaci speciální, vlastní nebo oborové protokoly.
- Lze vytvářet pomocí velké sady Oblíbené hardware a software platformách.

Kromě výše uvedenými požadavky zaujme také všechny IoT řešení měřítko, zabezpečení a spolehlivost. Výsledný sada připojení požadavky je pevné a časově náročnější provádět při použití tradiční technologií pro server, jako jsou webové kontejnery a zpráv zprostředkovatelů.

## <a name="why-use-azure-iot-hub"></a>K čemu se hodí centrální IoT Azure

Azure centrální IoT adresy výzvy zařízení připojení následujícími způsoby:

-   **Na zařízení ověřování a zabezpečené připojení**. Zřízení každého zařízení s vlastním [klíč zabezpečení] [ lnk-devguide-security] umožňuje připojení k centrální IoT. [Rozbočovač IoT identity registru] [ lnk-devguide-identityregistry] ukládá identit zařízení a klíče řešení. Řešení back-end můžete přidat jednotlivé zařízení a můžete povolit nebo odepřít seznamy povolit úplnou kontrolu nad přístup zařízení.

-   **Sledování operace připojení zařízení**. Můžete zobrazit podrobné operace protokoly o operace správy identit zařízení a zařízení připojení k události. Tuto funkci sledování umožňuje řešení IoT k identifikaci problémy s připojením, jako je zařízení, která se zkoušíte připojit přes nepovedlo přihlašovací údaje, posílat zprávy příliš často nebo odmítnout všechny zprávy cloudu zařízení.

-   **Velké množství knihoven zařízení**. [Azure IoT zařízení SDK] [ lnk-device-sdks] jsou k dispozici a podporované pro různé jazyky a platformách - C pro mnoho Linux rozdělení, Windows a v reálném čase operační systémy. Azure SDK zařízení IoT taky podporoval spravovaných, například C# Java a JavaScript.

-   **Protokoly IoT a rozšíření**. Pokud řešení pomocí knihoven zařízení, poskytuje IoT centrální veřejné protokol, který umožňuje zařízení nativně použít MQTT v3.1.1, HTTP 1.1 nebo AMQP 1.0 protokoly. Můžete také rozšířit IoT Centrum poskytuje podporu pro vlastní protokolů:

    - Vytvoření brány pole s [Azure IoT brány SDK] [ lnk-gateway-sdk] , převede vlastní protokol do jedné ze tří protokoly srozumitelné IoT centrální. 
    - Přizpůsobení [Azure IoT protokol brány][protocol-gateway], otevřít zdrojovou komponentu, které se spouští v cloudu.

-   **Měřítko**. Azure centrální IoT měřítko miliony současně připojených zařízení a miliony události sekundu.

Tyto výhody jsou obecné mnoho vzorů komunikace. Rozbočovač IoT aktuálně umožňuje provádět následující vzorků konkrétní komunikace:

-   **Na základě události požití zařízení do cloudu.** Rozbočovač IoT problémy se spolehlivým dostávat miliony události sekundu ze svého zařízení. Potom je může zpracovat pomocí engine procesor události na žhavé cestě. To je také ukládat na studenou cesty pro analýzu. Rozbočovač IoT zachová data události až 7 dnů zaručit spolehlivé zpracování a přijetí píků v načíst.

-   * *Spolehlivé zpráv cloudu zařízení (nebo *příkazy*). ** řešení back-end slouží IoT centrální posílat zprávy s garance doručení u aspoň jednou do jednotlivých zařízení. Každá zpráva má individuální nastavení time-to-live a back-end můžete požádat o potvrzení o doručení a vypršení platnosti. Tyto příjmy zajištění úplné viditelnosti do životního cyklu zprávu cloudu zařízení. Můžete pak implementovat obchodní logiky obsahující operace spuštěné v operačním systému zařízení.

-   **Nahrajte soubory a data uložená v mezipaměti snímače do cloudu.** Vaše zařízení můžete nahrajte soubory do úložiště Azure pomocí přidružení zabezpečení URI spravuje IoT centrální za vás. Centrální IoT můžete generovat oznámení při příchodu soubory v cloudu, aby povolit back-end pro jejich zpracování.

## <a name="gateways"></a>Brány

Brána v řešení IoT je obvykle [protokol brány] [ lnk-gateway] , která je nasazenou v cloudu nebo [pole brány] [ lnk-field-gateway] je místně nasazených pomocí svých zařízeních. Protokol brány provede protokol překladatelské, například MQTT AMQP. Brána pole můžete spustit analýzy na jeho okraj, rozhodovat dobou snížit latence poskytování služeb správy zařízení, vynucení zabezpečení a ochrany osobních údajů omezení a provést také protokol překladatelské. Obě brány typy představovat zprostředkovatelů mezi zařízeních a rozbočovače IoT.

Pole brány se liší od jednoduchého přenosy směrování zařízení (třeba síťové adresu překladu zařízení nebo bránu firewall), protože obvykle provádí aktivní role ve správě toku přístup a informace ve vašem řešení.

Řešení může obsahovat bran Protocol (protokol) a pole.

## <a name="how-does-iot-hub-work"></a>Jak funguje IoT centrální?

Azure centrální IoT implementuje [pomoci služby komunikace] [ lnk-service-assisted-pattern] vzorek roli prostředníka interakce mezi zařízeních a vašeho řešení back-end. Cílem pomoci služby komunikace je vytvořit důvěryhodné, cesty obousměrný komunikace mezi ovládací prvek systému, například IoT centrální a speciální zařízení, které jsou nasazené v nedůvěryhodné fyzické místo. Vzorek vytvoří následující zásady:

- Zabezpečení přednost před další možnosti.
- Zařízení není přijmout informace nevyžádaného sítě. Zařízení s zavádí všechna připojení a trasy odchozího způsobem. Pro zařízení s přijme příkazu od back-end zařízení pravidelně zahájit připojení k vyhledat některého z příkazů na zjištění stavu úkolů pro zpracování.
- Zařízení by měl pouze připojení k nebo vytvořit směruje známý služeb, které jsou peered, například IoT centrální.
- Cesta komunikace mezi zařízení a přihlašovacích údajů nebo zařízení a brána je zabezpečená ve vrstvě protokol aplikací.
- Úroveň systému autorizace a ověřování vycházejí identit na zařízení. Provádění pověření přístup a oprávnění téměř okamžitě odvolatelný.
- Obousměrnou komunikaci pro zařízení připojujících se občas kvůli power nebo připojení se týká usnadnit tak, že podržíte příkazy a zařízení oznámení, dokud zařízení připojuje k jejich odběru. Rozbočovač IoT udržuje fronty zařízení specifické pro příkazy, které je přesměruje.
- Data datové části aplikace je pro chráněné dopravní prostřednictvím bran pro konkrétní službu zabezpečená samostatně.

Mobilní odvětví použil vzorek pomoci služby komunikace ve velkém měřítku obrovské implementace nabízená oznámení služeb, jako je [Windows nabízená oznámení služby][lnk-wns], [Zasílání zpráv Cloud Google][lnk-google-messaging]a [Služba nabízených oznámení společnosti Apple][lnk-apple-push].

Rozbočovač IoT je podporována nad ExpressRoute na veřejné prozkoumávání cestu.

## <a name="next-steps"></a>Další kroky

Jak Azure IoT centrální umožňuje standardy správy zařízení IoT vzdáleně spravovat, konfigurace a aktualizovat zařízení najdete v tématu [Přehled Azure IoT centrální správy zařízení][lnk-device-management].

Implementace klientské aplikace na širokou škálu platformy hardwaru zařízení a operačních systémů, můžete pomocí zařízení IoT SDK. Zařízení IoT SDK zahrnout knihoven, které usnadňují odesílání telemetrie rozbočovači IoT a přijímat příkazy cloudu zařízení. Při použití SDK můžete z různých síťové protokoly komunikovat s IoT centrální. Další informace najdete v tématu [informace o zařízení SDK][lnk-device-sdks].

Začněte psát některé kód a spuštění pár ukázek najdete v článku [Začínáme s IoT centrální] [ lnk-get-started] kurz.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Služba pomoci komunikace, příspěvek na blogu od Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
