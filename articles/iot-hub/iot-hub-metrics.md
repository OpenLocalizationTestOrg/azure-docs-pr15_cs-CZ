<properties
 pageTitle="Diagnostické metriky IoT centrální"
 description="Přehled centrální IoT Azure metriky uživatelům umožňuje posuďte celkové stavu jejich zdroje"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Úvod k diagnostiky metriky

Diagnostické metriky umožňují lepší údaje o stavu Azure zdrojů v Azure předplatné. Metriky umožňují posuďte celkový stav služby a zařízení připojených k němu. Statistiky uživatelů webových jsou důležité, protože pomáhají najdete v článku co je rozchodit problémy IoT nápovědy a centrální příčin aniž by musel kontaktovat podporu Azure.

Můžete povolit diagnostické metriky z portálu Microsoft Azure.

## <a name="how-to-enable-diagnostic-metrics"></a>Jak povolit diagnostické metriky

1. Vytvořte Centrum IoT. Najde pokyny k vytváření rozbočovači IoT v [Začínáme] [ lnk-get-started] průvodce.

2. Otevřete zásuvné rozbočovače IoT. Z tohoto umístění klikněte na **Diagnostika**.

    ![][1]

3. Konfigurace vaší diagnostiky nastavení stavu na **zapnuto** a výběrem účet Azure úložiště pro ukládání diagnostických nástrojů data. Kontrola **metriky**a stiskněte klávesu **Uložit**. Všimněte si, že je třeba vytvořit účet Azure úložiště předem a že se účtovány samostatně úložiště. Můžete taky odeslat datům diagnostiky koncového rozbočovače události.

    ![][2]

4. Když jste nastavili Diagnostika, vraťte se do zásuvné centrální IoT **Přehled** . Metriky informace vyplněné v části **Sledování** zásuvné. Kliknutím na tlačítko graf otevřete podokno metriky, kde můžete zobrazit souhrn informací metriky IoT rozbočovače a upravit výběr metriky zobrazené v grafu. Můžete taky nakonfigurovat upozornění na základě metrických hodnot.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Metriky a jejich použití

Rozbočovač IoT poskytuje několika metrik vám přehled o stavu rozbočovače a celkového počtu zařízení připojených k němu. Můžete zkombinovat informace z několika metrik Malování větší vzhledu stav centru IoT. Následující tabulka popisuje metriky, který každého IoT rozbočovače sleduje a skladovými každý míru celkový stav centru IoT.

| Metriky | Metriky popis | Metriky k čemu slouží |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Počet zprávy poslané na všech zařízeních | Přehled dat na odeslání zprávy |
| d2c.telemetry.ingress.Success | Počet všech úspěšné zpráv v Centru | Základní informace o průniku úspěšné zprávu do rozbočovači |
| c2d.Commands.egress.COMPLETE.Success | Počet všech zpráv příkazový doplněné přijímání zařízení na všech zařízeních | Spolu s metriky na opuštění a odmítnout dává přehled o celkové úspěšnosti C2D příkaz |
| c2d.Commands.egress.Abandon.Success | Počet všech zpráv úspěšně opuštěné přijímání zařízením na všech zařízeních | Pokud se zobrazuje zprávy opuštěné dochvilnější spoje, než byste čekali se zvýrazní potenciálních problémů |
| c2d.Commands.egress.Reject.Success | Počet všech zpráv úspěšně odmítnuto přijímání zařízením na všech zařízeních | Pokud zprávy začíná odmítnuty dochvilnější spoje, než byste čekali se zvýrazní potenciálních problémů |
| devices.totalDevices | Průměr, min a max číslo zařízení registrované k rozbočovači IoT | Počet zařízení registrovaných k rozbočovači |
| devices.connectedDevices.allProtocol | Průměr, min a max číslo souběžné připojených zařízení | Základní informace o počtu zařízení připojené k rozbočovači |

## <a name="next-steps"></a>Další kroky

Teď ukázali jsme si přehled o diagnostiky metriky, tento odkaz zobrazíte další informace o správě Azure IoT centrální:

- [Operace sledování][lnk-monitor]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
