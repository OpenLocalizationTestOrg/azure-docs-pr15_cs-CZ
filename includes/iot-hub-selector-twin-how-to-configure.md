> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Úvod

V [Seznámení s IoT rozbočovač twins][lnk-twin-tutorial], se dozvěděli, jak nastavit metadata zařízení z vašeho řešení back-end pomocí *značek*, podmínky sestavy zařízení od zařízení app pomocí *Vlastnosti oznámenou*a tyto informace pomocí jazyka SQL jako dotaz.

V tomto kurzu se dozvíte, jak používat twin *požadované vlastnosti* ve spojení s *oznámila vlastnosti*, vzdáleně konfigurovat zařízení apps. Přesněji řečeno tento návod ukazuje, jak oznámil na twin a požadované vlastnosti povolit více krok konfigurace aplikace nastavení zařízení a poskytují viditelnost řešení zadní konec stav této operace ve všech zařízeních.

Tento kurz na vysoké úrovni, následuje *vzor požadovaný stav* pro správu zařízení. Základní myšlenkou tohoto vzoru je řešení back-end zadejte požadovaný stav spravovaných zařízení namísto odeslání zvláštních příkazů. To umístí zařízení pro stanovení nejlepší způsob, jak dosáhnout požadovaného stavu (velmi důležité pro IoT scénáře, kde podmínky pro konkrétní zařízení ovlivnit schopnost okamžitě provádět určité příkazy) při aktuální stav a případné chybové stavy neustále hlášení na back-end. Požadovaný stav vzorek je instrumentální správy velkého zařízení, protože umožňuje, aby zadní mít úplný přehled o stavu procesu konfigurace ve všech zařízeních.
Další informace o roli vzorek požadovaný stav naleznete v Správa zařízení ve [správě zařízení přehled Azure IoT rozbočovač][lnk-dm-overview].

> [AZURE.NOTE] V případech, kdy zařízení jsou řízeny způsobem interaktivní (zapnout ventilátor z aplikace se řídí uživatel), zvažte použití [metody cloudu zařízení][lnk-methods].

V tomto výukovém programu změny back-end aplikace telemetrické konfigurace cílového zařízení a v důsledku toho app zařízení řídí několika krocích použít aktualizaci konfigurace (například vyžadují restartování modulu softwaru), který simuluje tento kurz jednoduché zpoždění).

Back-end ukládá konfiguraci do zařízení twin požadované vlastnosti následujícím způsobem:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Vzhledem k tomu, že konfigurace může být složité objekty, jsou obvykle přiřazeny jedinečné ID (hash nebo [identifikátory GUID][lnk-guid]) ke zjednodušení jejich porovnání.

Aplikace zařízení hlásí jeho aktuální konfiguraci požadovanou vlastnost **telemetryConfig** ve vlastnostech oznámenou zrcadlení:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Všimněte si, jak oznámenou **telemetryConfig** má další vlastnost **Stav**, používané k vykazování stavu konfigurace procesu aktualizace.

Při přijetí nové požadované konfiguraci zařízení app nevyřízené Konfigurace sestav změnou údajů:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Potom později, app zařízení ohlásí úspěch selhání této operace aktualizací výše uvedené vlastnosti.
Všimněte si, jak je možné, back-end kdykoli zjistit stav procesu konfigurace ve všech zařízeních.

Tento návod ukazuje, jak chcete:

- Vytvořte simulované zařízení, které přijímá aktualizace konfigurace z back-end a hlásí více aktualizací *Vlastnosti vykázané* v procesu aktualizace konfigurace.
- Vytvoření aplikace back-end, který aktualizuje konfiguraci požadované zařízení a potom se dotazuje procesu aktualizace konfigurace.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier