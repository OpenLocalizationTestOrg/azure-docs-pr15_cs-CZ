## <a name="view-device-telemetry-in-the-dashboard"></a>Telemetrické zařízení zobrazit v řídicím panelu

V řešení vzdáleného monitorování řídicího panelu můžete telemetrické, který odeslat zařízení IoT rozbočovač.

1. V prohlížeči vzdálené monitorování řídicího panelu řešení vrátit, klepněte na **zařízení** v levém panelu přejděte do **seznamu zařízení**.

2. V **seznamu zařízení**měli byste vidět, že stav zařízení je nyní **spuštěna**.

    ![][18]

3. Klepněte na položku **řídicího panelu** , vraťte se do řídicího panelu, vyberte **zařízení pro zobrazení** rozevíracího seznamu zobrazíte jeho telemetrické zařízení. Telemetrické z ukázkové aplikace je 50 jednotek pro vnitřní teploty 55 jednotek pro vnější teplotu a 50 jednotek pro vlhkost. Poznámka: ve výchozím nastavení zobrazí řídicí panel pouze hodnoty teploty a vlhkosti.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Odeslání příkazu do zařízení

Řídicí panel v vzdálené monitorovací řešení umožňuje odeslat příkazy pomocí rozbočovače IoT zařízení. Například v vzdálené monitorovací řešení můžete odeslat příkaz nastavit vnitřní teplotu zařízení.

1. V vzdálené monitorování řešení řídicího panelu klepněte na **zařízení** v levém panelu přejděte do **seznamu zařízení**.

2. Klepněte na zařízení v **seznamu zařízení** **ID zařízení** .

3. V panelu **Podrobnosti o zařízení** klepněte na položku **Příkazy**.

    ![][13]

4. V rozevírací nabídce **příkaz** vyberte **SetTemperature**a potom zadejte novou hodnotu teploty **teploty** . Klepněte na tlačítko **Odeslat příkaz** k odeslání příkazu do zařízení.

    ![][14]

    > [AZURE.NOTE] Historie příkazů zpočátku zobrazuje stav příkazu jako **čekající na vyřízení**. Pokud zařízení uznává příkazu, stav se změní na **Úspěch**.

5. Na řídicím panelu ověřte, že zařízení je nyní odesílání 75 jako novou hodnotu teploty.

## <a name="next-steps"></a>Další kroky

V článku [Vlastní nastavení předem řešení] [ lnk-customize] popisuje některé způsoby, jak můžete rozšířit tento vzorek. Možné rozšíření zahrnovat použití skutečného snímače a provádění dalších příkazů.

Další informace o [oprávnění na webu azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
