> [AZURE.SELECTOR]
- [C v systému Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C na platformě Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C na mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Přehled scénáře

V tomto scénáři vytvořit zařízení, které odesílá následující telemetrické vzdálené monitorování [řešení předem][lnk-what-are-preconfig-solutions]:

- Vnější teplota
- Vnitřní teplota
- Vlhkost

Pro zjednodušení kódu na zařízení generuje ukázkové hodnoty, ale doporučujeme rozšířit připojením zařízení skutečného snímače a skutečné telemetrické odesílání vzorku.

Chcete-li dokončit tento návod, potřebujete aktivní účet Azure. Pokud nemáte účet, můžete vytvořit Bezplatný zkušební účet jen několika málo minut. Podrobnosti naleznete v tématu [Bezplatnou zkušební verzi Azure][lnk-free-trial].

## <a name="before-you-start"></a>Před zahájením

Před psát jakýkoli kód pro vaše zařízení, musíte vytvořit řešení vzdáleného monitorování předem a potom vytvořit nové vlastní zařízení v tomto řešení.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Poskytnutí vzdálené monitorování předem řešení

Zařízení, které vytvoříte v tomto výukovém kurzu odesílá data do instance [vzdálené sledování] [ lnk-remote-monitoring] předem řešení. Pokud již nebyla vytvořena vzdálené sledování předem řešení v Azure účtu, postupujte následujícím způsobem:

1. Na stránce <https://www.azureiotsuite.com/> klepněte na **+** Chcete-li vytvořit nové řešení.

2. Klepněte na tlačítko **Vybrat** v panelu **vzdálené sledování** Chcete-li vytvořit nové řešení.

3. Na stránce **vytvořit vzdálený monitorovací řešení** zadejte **název řešení** dle vašeho výběru, vyberte **oblast** kterou chcete nasadit a vyberte Azure předplatné, které chcete použít. Klepněte na **vytvořit řešení**.

4. Počkejte na dokončení procesu zřizování.

> [AZURE.WARNING] Předem nakonfigurované řešení používat fakturaci služeb Azure. Je nutné odebrat z odběru předem řešení, po dokončení s ním žádné zbytečné náklady. Předem nakonfigurované řešení můžete zcela odebrat z předplatného na stránce <https://www.azureiotsuite.com/> .

Po dokončení procesu zřizování vzdálené monitorovací řešení, klepněte na tlačítko **Spustit** Chcete-li otevřít řídicí panel řešení v prohlížeči.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Zřizování zařízení v řešení vzdáleného monitorování

> [AZURE.NOTE] Zřízením zařízení již ve vašem řešení, můžete tento krok přeskočit. Musíte znát zařízení pověření při vytváření klientské aplikace.

Zařízení pro připojení k předem řešení ji musí se identifikovat IoT rozbočovači pomocí platných pověření. Můžete načíst pověření zařízení z řídicího panelu řešení. Aplikace klient dále v tomto výukovém programu zahrnout zařízení pověření. 

Chcete-li přidat nové zařízení k řešení vzdáleného monitorování, proveďte následující kroky v řešení řídicího panelu:

1.  V levém dolním rohu řídicího panelu klepněte na tlačítko **Přidat zařízení**.

    ![][1]

2.  V panelu **Vlastní zařízení** klepněte na **Přidat nový**.

    ![][2]

3.  Zvolte **Manuální definovat ID zařízení**, zadejte ID zařízení, jako jsou **mydevice**, klepněte na tlačítko **Zkontrolovat ID** ověřte, zda že tento název není již použit, a klepněte na tlačítko **vytvořit** ke zřízení zařízení.

    ![][3]

5. Poznamenejte si zařízení pověření (ID zařízení IoT rozbočovač Hostname a klíč zařízení), klientské aplikace potřebují připojit k vzdálené monitorovací řešení. Klepněte na tlačítko **Hotovo**.

    ![][4]

6. Ujistěte se, že se že zařízení zobrazí v části zařízení. Stav zařízení čeká **na vyřízení** , dokud zařízení naváže připojení k vzdálené monitorovací řešení.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/