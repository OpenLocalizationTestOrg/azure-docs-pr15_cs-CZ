<properties
 pageTitle="Příručka vývojáře – registru identity zařízení | Microsoft Azure"
 description="Azure IoT centrální vývojář Průvodce - Popis registru identity zařízení a jak ji používat ke správě svých zařízeních"
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

# <a name="manage-device-identities-in-iot-hub"></a>Správa identity zařízení v centrální IoT

## <a name="overview"></a>Základní informace

Každý IoT centrální má registru identity zařízení, která obsahuje informace o zařízení, která jsou povoleny pro připojení k centru. Abyste zařízení mohli připojit k rozbočovači, musí být položky pro toto zařízení v Centrum zařízení identity registru. Zařízení musí taky ověření s rozbočovači podle přihlašovací údaje uložené v registru identity zařízení.

Na vysoké úrovni registru identity zařízení najdete podporou ZBÝVAJÍCÍ zařízení identity zdrojů. Po přidání položky do registru IoT centrální vytvoří sadu zdroje na zařízení v služby, třeba fronty, která obsahuje letu cloudu zařízení zprávy.

### <a name="when-to-use"></a>Kdy použít

Pomocí registru identity zařízení, když potřebujete zřízení zařízení, připojte k IoT centrální a pokud potřebujete řídit přístup na zařízení k zařízení webových koncové body v rozbočovače.

> [AZURE.NOTE] Registr identity zařízení neobsahuje metadatům specifické pro aplikaci.

## <a name="device-identity-registry-operations"></a>Operace registru identity zařízení

Registru identity zařízení IoT Centrum poskytuje tyto operace:

* Vytvoření zařízení identity
* Aktualizace zařízení identity
* Načtení zařízení identit podle ID
* Odstranění identity zařízení
* Seznam až 1000 identitami
* Export všechny identitami k úložišti objektů blob Azure
* Import identit z úložišti objektů blob Azure

Tyto operace můžete použít optimistické souběžné jako v souladu s [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Jediný způsob, jak získat všechny identity v registru identity k rozbočovači, je použít [Exportovat] [ lnk-export] funkce.

Rozbočovač IoT zařízení identity registru:

- Neobsahuje žádné metadat aplikace.
- Je přístupný podobně jako slovník, pomocí **deviceId** klíče.
- Nepodporuje výrazové dotazů.

Řešení pro IoT má obvykle samostatné úložiště specifické řešení, které obsahuje specifické pro aplikaci metadata. Řešení specifické úložiště v řešení inteligentní stavební by například záznam místnosti, ve kterém je nasazený senzoru teploty.

> [AZURE.IMPORTANT] Pouze pomocí registru identity zařízení pro správu zařízení a zřízení operace. Vysoký výkon operace za běhu neměli závisí na provádění operací v registru identity zařízení. Kontrola stavu připojení zařízení před odesláním příkazu je například není podporované vzorek. Projděte [omezení sazby] [ lnk-quotas] registru identity zařízení a [zařízení prezenční] [ lnk-guidance-heartbeat] vzorku.

## <a name="disable-devices"></a>Zakázání zařízení

Aktualizací **stavu** vlastnost identity v registru můžete zakázat zařízení. Tato vlastnost se obvykle používají ve dvou situacích:

- Při vytváření průběhu procesu. Další informace najdete v tématu [Příprava zařízení][lnk-guidance-provisioning].
- Pokud z nějakého důvodu, zvažte stal neoprávněným nebo je ohroženo zařízení.

## <a name="import-and-export-device-identities"></a>Import a export zařízení identity

Zařízení identit hromadně můžete exportovat z registru identity IoT centrální pomocí asynchronní operací na [koncový bod poskytovatele IoT Centrum zdrojů][lnk-endpoints]. Export jsou časově náročné úlohy, které používají kontejneru předávaných zákazníka objektů blob uložte zařízení identity dat přečíst z registru systému identity.

Zařízení identit hromadně rozbočovači IoT identity registru můžete importovat pomocí asynchronní operace [IoT Centrum zdrojů poskytovatele koncový bod][lnk-endpoints]. Importy je dlouhodobě spuštěných úloh, které využívají data v kontejneru dodané zákazníka objektů blob psát zařízení identity data do registru identity zařízení.

- Podrobné informace o importu a exportu rozhraní API, najdete v článku [IoT Centrum zdrojů poskytovatele rozhraní REST API][lnk-resource-provider-apis].
- Další informace o tom, jak import a export úlohy, najdete v tématu [Hromadná správy identit zařízení IoT centrální][lnk-bulk-identity].

## <a name="device-provisioning"></a>Příprava zařízení

Zařízení data, která ukládá dané řešení IoT závisí na specifickým požadavkům řešení. Ale minimálně, třeba uložit řešení identit zařízení a ověřování klíče. Azure centrální IoT obsahuje identity registru, která mohou být uloženy hodnoty pro každý zařízení, třeba ID, klíče ověřování a stav kódy. Řešení můžete použít jiné Azure služeb, jako je úložiště tabulek Azure, úložišti objektů blob Azure nebo Azure DocumentDB k uložení dat další zařízení.

*Příprava zařízení* je proces přidávání dat počáteční zařízení do úložiště v řešení. Povolit nové zařízení pro připojení k rozbočovače, musíte přidat nové ID zařízení a klíče registru IoT centrální identity. Jako součást procesu zřizovací můžete zkusit inicializace specifické pro zařízení dat v jiné řešení ukládá.

## <a name="device-heartbeat"></a>Zařízení prezenční signál

Registr identity IoT centrální obsahuje pole s názvem **connectionState**. Používejte pouze pole **connectionState** průběhu vývoje a ladění IoT řešení neměli dotazu pole za běhu (například zaškrtněte, pokud je zařízení připojené k rozhodnout, jestli chcete poslat zprávu cloudu zařízení nebo e).

Pokud řešení IoT potřeby lze zjistit, zda zařízení připojení (za běhu, nebo s větší přesnost, než poskytuje vlastnost **connectionState** ), řešení plnit *prezenční signál vzorku*.

Ve vzorku prezenční signál zařízení odešle zařízení do cloudu zprávy aspoň jednou každých pevné množství času (například aspoň každou hodinu). To znamená, že i v případě zařízení neobsahuje žádná data k odeslání, pořád pošle prázdná zpráva zařízení do cloudu (obvykle s vlastností, které označuje jako prezenční signál). Na straně služby řešení udržuje mapy s poslední prezenční signál dostali u každého zařízení a předpokládá, že došlo k potížím se zařízením neobdrží prezenční signál v rámci předpokládanou dobu.

Složitější implementace může obsahovat informace ze [Sledování operace] [ lnk-devguide-opmon] k identifikaci zařízení, které jsou pokusíte připojit nebo komunikovat, ale neúspěšně. Při provádění vzorek prezenční signál projděte [IoT centrální kvót a omezením][lnk-quotas].

> [AZURE.NOTE] Řešení pro IoT potřebuje stav připojení zařízení výhradně se rozhodnout, jestli k odesílání zpráv cloudu zařízení a zpráv nejsou vysílat velkých sad zařízení, mnohem jednodušší vzorek vzít v úvahu při krátké vypršení platnosti automaticky používat. To dosahuje stejný výsledek jako zachování registru stavu připojení zařízení pomocí prezenční signál vzorku a při tom mnohem efektivnější. Je také možné, tak, že žádosti o potvrzení zprávy, aby vás upozornil IoT Centrum zařízení, která budou moct přijímat zprávy a které jsou online nebo se nepodařilo.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o devcie identity registru.

## <a name="device-identity-properties"></a>Vlastnosti identity zařízení

Identit zařízení jsou zobrazeny jako JSON dokumenty s následujícími vlastnostmi.

| Vlastnost | Možnosti | Popis |
| -------- | ------- | ----------- |
| deviceId | požadované informace o aktualizacích jen pro čtení | Velká a malá písmena řetězec (až 128 znaků) alfanumerických znaků ASCII 7-bit + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | potřeby jen pro čtení | Řetězec generované centrální, velká a malá písmena až 128 znaků. Slouží k rozlišení zařízení s stejné **deviceId**po odstranění a znovu vytvořit. |
| etag | potřeby jen pro čtení | Řetězec vyjadřující slabými etag pro identitě zařízení podle [RFC7232][lnk-rfc7232].|
| auth | volitelné | Složené objekt obsahující ověřování informace a zabezpečení materiály. |
| auth.symkey | volitelné | Složené objekt obsahující primární a sekundární klíč uložené ve formátu ve formátu Base 64. |
| Stav | povinné | Indikátor přístup. Může být **povoleno** nebo **Zakázáno**. Pokud **povoleno**, zařízení se může připojit. Pokud **Zakázané**toto zařízení nemůžete získat přístup ke všechny koncový bod směřující zařízení. |
| statusReason | volitelné | 128 znaků dlouhé řetězec, který ukládá důvod stavu identity zařízení. Jsou povoleny všechny znaky UTF-8. |
| statusUpdateTime | jen pro čtení | Časový indikátor ukazující, datum a čas poslední aktualizace stavu. |
| connectionState | jen pro čtení | Pole označující stav připojení: **Připojit** nebo **Odpojeno**. Toto pole představuje centrální IoT zobrazení stavu připojení zařízení. **Důležité**: Toto pole bude použito pouze pro účely vývoj/ladění. Stav připojení se aktualizuje pouze pro zařízení pomocí MQTT nebo AMQP. Také je založena na příkazy úroveň protokolu ping (příkazy MQTT ping nebo příkazy AMQP ping) a může mít zpoždění pouze 5 minut. Z těchto důvodů může být falešně pozitivní, například zařízení hlášené jako připojení, ale ve skutečnosti odpojena. |
| connectionStateUpdatedTime | jen pro čtení | Byl aktualizován časový indikátor ukazující data a času poslední stav připojení. |
| lastActivityTime  | jen pro čtení | Časový indikátor zobrazující data a času poslední zařízení připojené, přijatá nebo odeslaná zpráva. |

> [AZURE.NOTE] Stav připojení můžete jenom představuje centrální IoT zobrazení stavu připojení. V závislosti na stav sítě a konfigurace může zpoždění aktualizací tento stav.

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT rozbočovači a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT centrální MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT centrální protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď jste se naučili používání registru IoT Centrum zařízení identity, vás mohla zajímat v následujících tématech vývojář příručka:

- [Řízení přístupu k centrální IoT][lnk-devguide-security]
- [Pomocí zařízení twins synchronizovat stav a konfigurace][lnk-devguide-device-twins]
- [Přímé metodu na zařízení][lnk-devguide-directmethods]
- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, vás mohla zajímat následující kurz IoT centrální:

- [Začínáme s centrální IoT Azure][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md