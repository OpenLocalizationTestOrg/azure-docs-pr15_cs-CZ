<properties
 pageTitle="Azure IoT předem řešení | Microsoft Azure"
 description="Popis Azure IoT předem řešení a jejich architektura s odkazy na další zdroje informací."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Co je Azure IoT sadu automaticky předem nakonfigurovaná řešení?

Řešení Azure IoT sadu automaticky předem nakonfigurovaná jsou implementace vzorů běžné IoT řešení, které můžete nasadit Azure prostřednictvím svého předplatného. Je možné používat předkonfigurované řešení:

- Jako výchozí bod pro IoT řešení.
- Další informace o běžné vzorce v návrhu IoT řešení a vývoje.

Každý předkonfigurované řešením je dokončeno, začátku do konce implementace, která používá simulovaný zařízení generovat telemetrie.

Kromě zavádění a spouštění řešení v Azure, můžete stáhnout úplný zdrojový kód a potom přizpůsobení a rozšíření řešení podle specifických požadavků IoT.

> [AZURE.NOTE] Abyste mohli nasadit jednu z předem řešení, navštěvujte blog o [Microsoft Azure IoT Suite][lnk-azureiotsuite]. V článku [Začínáme s řešení IoT automaticky předem nakonfigurovaná] [ lnk-getstarted-preconfigured] poskytuje další informace o tom, jak nasazení a spustit jednu řešení.

Následující tabulka ukazuje, jak řešení namapovat specifických funkcí IoT:

| Řešení | Požití dat | Zařízení Identity | Příkaz a řízení | Pravidla a akce | Prognostické analýzy |
|------------------------|-----|-----|-----|-----|-----|
| [Vzdálené sledování][lnk-getstarted-preconfigured] | Ano | Ano | Ano | Ano | -   |
| [Prognostické údržby][lnk-predictive-maintenance] | Ano | Ano | Ano | Ano | Ano |

- *Požití dat*: průniku dat ve velkém měřítku v cloudu.
- *Zařízení identit*: Správa jedinečné identit každé spojení zařízení.
- *Příkaz a řízení*: odesílání zpráv na zařízení z cloudu způsobilo zařízení určitou akci.
- *Pravidla a akce*: řešení back-end používá pravidla chovalo na určitá data zařízení do cloudu.
- *Prognostické analýzy*: řešení back-end slouží k použití analýzy dat zařízení do cloudu odhadnout, kdy by měla proběhnout nějaké konkrétní akce. Například analýza letadla engine telemetrie k určení po engine údržbu.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Přehled řešení vzdálené sledování automaticky předem nakonfigurovaná

Zvolili jsme diskutovat o vzdálené monitorovací předkonfigurované řešení v tomto článku, protože ukazuje mnoho běžných prvků návrhu, které sdílejí ostatní řešení.

Následující obrázek znázorňuje klíčové prvky vzdálené monitorovací řešení. Následující části obsahují další informace o těchto prvků.

![Vzdálené sledování předem architektury řešení][img-remote-monitoring-arch]

## <a name="devices"></a>Zařízení

Při nasazení vzdálené monitorovací předkonfigurované řešení čtyři simulovaný zařízení jsou předem zřizování řešení, které simulovat chlazení zařízení. Tato simulovaný zařízení máte předdefinované teploty a vlhkosti modelu posílá telemetrie. Tato simulovaný zařízení jsou však započítávány lze znázornit tok začátku do konce dat pomocí řešení a poskytnout pohodlný zdroj telemetrie a cíl příkazů, pokud jste vývojáři back-end pomocí řešení jako výchozí bod pro vlastní implementaci.

Pokud zařízení nejdřív připojí k centrální IoT ve vzdáleném sledování předkonfigurované řešení, zařízení informace Komu centru IoT výčet seznamu dostupných příkazů, které můžete odpovědět zařízení. Ve vzdáleném sledování předkonfigurované řešení příkazy jsou: 

- *Příkaz ping zařízení*: zařízení odpoví na tento příkaz s odpověď na odeslanou zprávu. To je užitečné pro ověření, že zařízení je stále aktivní a listening.
- *Zahájení Telemetrie*: Nastaví zařízení spustit odesílání telemetrie.
- *Ukončení Telemetrie*: Nastaví zařízení zastavit odesílání telemetrie.
- *Změna nastavení bodu teploty*: řídí hodnot telemetrie simulovaný teploty odešle zařízení. To je užitečné pro účely testování back-end logiky.
- *Diagnostické Telemetrie*: ovládacích zařízení má poslat externí teplotní jako telemetrie.
- *Stav zařízení změny*.: Nastaví vlastnost metadat stav zařízení, která zařízení sestavy. To je užitečné pro účely testování back-end logiky.

Do řešení, které posílat stejné telemetrie zpráv a odpovídání na stejné příkazy můžete přidat další simulovaný zařízení. 

## <a name="iot-hub"></a>Rozbočovač IoT

V tomto předkonfigurované řešení instance IoT centrální odpovídá *Brány cloudu* v typické [architektury řešení IoT][lnk-what-is-azure-iot].

Rozbočovači IoT obdrží telemetrie ze zařízení na jeden koncový bod. Rozbočovači IoT také udržuje zařízení konkrétní koncové body, kde každý zařízení můžete obnovit příkazy, které jsou odeslány do ní.

Rozbočovač IoT zpřístupní přijaté telemetrie prostřednictvím služby straně telemetrie číst koncového bodu.

## <a name="azure-stream-analytics"></a>Azure toku analýzy

Předkonfigurované řešení používá třemi [Azure toku analýzy] [ lnk-asa] úloh (ASA) k filtrování tok telemetrie z zařízení:


- *Úlohy DeviceInfo* - výstupní data k rozbočovači události, který směruje zařízení registrace určité zprávy, při prvním připojení zařízení nebo odpověď na příkaz **Změnit stav zařízení** registru zařízení řešení (DocumentDB databáze). 
- *Úlohy telemetrie* - odešle všechny nezpracovanými telemetrie k úložišti objektů blob Azure pro chladírenské a vypočítá telemetrie agregace, které se zobrazí na řídicím panelu řešení.
- *Pravidla úlohy* – filtruje proudu telemetrie pro hodnoty, které překročení žádné pravidlo prahových hodnot a uloží data k rozbočovači události. Když pravidlo se aktivuje, zobrazení portálu řídicího panelu řešení zobrazí tato událost jako nový řádek v tabulce historie upozornění a spustí akci v závislosti na nastavení definovaný v zobrazeních pravidla a akce na portálu řešení.

V tomto předkonfigurované řešení úlohy ASA součástí **IoT řešení serverová** ve typické [architektury řešení IoT][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Procesor událostí

V tomto předkonfigurované řešení procesor událostí je součástí **IoT řešení serverová** ve typické [architektury řešení IoT][lnk-what-is-azure-iot].

Úlohy **DeviceInfo** a **pravidla** ASA poslat rozbočovače událostí pro doručování do jiných služeb back-end jeho výstup. Řešení používá [EventPocessorHost] [ lnk-event-processor] instanci spuštěné v [WebJob][lnk-web-job], číst zprávy od těchto rozbočovače události. **EventProcessorHost** používá **DeviceInfo** data k aktualizaci dat zařízení v databázi DocumentDB a používá data **pravidel** vyvolat logiky aplikaci a v portálu řešení zobrazit upozornění na aktualizace.

## <a name="device-identity-registry-and-documentdb"></a>Zařízení identity registru a DocumentDB

Každý IoT centrální obsahuje [zařízení identity registru] [ lnk-identity-registry] zařízení zkratky, které obsahuje. Rozbočovač IoT použije tyto informace ověření zařízení - zařízení musí být registrovány a mít platné klíč, abyste mohli připojit k rozbočovači.

Toto řešení ukládá dodatečné informace o zařízení například stavu, příkazy, které podporují a ostatních metadatech. Řešení používá DocumentDB databázi k ukládání dat zařízení specifické pro řešení a portálu řešení načte data z této databáze DocumentDB pro zobrazení a úpravy.

Řešení musí obsahovat informace registru identity zařízení synchronizovat s obsahem DocumentDB databáze. **EventProcessorHost** použije data z **DeviceInfo** toku analýzy úloha spravovat synchronizace.

## <a name="solution-portal"></a>Portál řešení

![Řídicí panel řešení][img-dashboard]

Portál řešení je uživatelské rozhraní založené na webu, které je nasazený do cloudu v rámci předkonfigurované řešení. Umožňuje:

- Zobrazení historie telemetrie a upozornění na řídicím panelu.
- Zřízení nového zařízení.
- Správa a sledování zařízení.
- Odeslání příkazů pro určitá zařízení.
- Spravovat pravidla a akce.

V tomto předkonfigurované řešení portálu řešení formulářů část **IoT řešení serverová** a část **zpracování a služby Připojení obchodních** v typické [architektury řešení IoT][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Další kroky

Další informace o IoT architektury řešení najdete v tématu [služby Microsoft Azure IoT: Přehled architektura][lnk-refarch].

Teď víte, jaká předkonfigurované řešení se mohli rovnou začít nasazením řešení *vzdálené sledování* automaticky předem nakonfigurovaná: [Začínáme s předem řešení][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md