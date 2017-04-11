<properties
    pageTitle="Začínáme s předem řešení | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak pro nasazení řešení pro sadu IoT Azure předem."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Kurz: Začínáme s předem řešení

## <a name="introduction"></a>Úvod

Azure IoT sadu [předkonfigurované řešení] [ lnk-preconfigured-solutions] sloučení více služby Azure IoT dodávat začátku do konce řešení, které provádění běžných obchodních scénářů IoT. Řešení *vzdálené sledování* automaticky předem nakonfigurovaná připojí se k a sleduje svých zařízeních. Řešení můžete použít k analýze toku dat z vašeho zařízení a zlepšit výsledky firmy tím, že procesy automaticky odpovídat této toku dat.

Tento kurz se dozvíte, jak zřídit vzdálené monitorovací předkonfigurované řešení. Je taky vás provede základních funkcí vzdálené monitorovací řešení. Mnoho funkcí můžete přistupovat prostřednictvím řešení řídicí panel, který nasadí spolu s předem řešení:

![Řídicí panel předkonfigurované řešení Remote][img-dashboard]

Tento kurz musíte aktivní Azure předplatné.

> [AZURE.NOTE]  Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Zobrazení řídicího panelu řešení

Řídicí panel řešení umožňuje spravovat nasazení řešení. Můžete třeba zobrazit telemetrie, přidat zařízení a nakonfigurovat pravidla.

1.  Po dokončení zřizování a dlaždici předkonfigurované řešení pro označuje **připravená**, klikněte na **spuštění** otevřete portálu vzdálené řešení sledování na nové kartě.

    ![Spuštění předkonfigurované řešení][img-launch-solution]

2.  Ve výchozím nastavení zobrazuje portálu řešení *řešení řídicího panelu*. Můžete vybrat jiné zobrazení v nabídce vlevo.

    ![Řídicí panel předkonfigurované řešení Remote][img-dashboard]

Řídicí panel zobrazí následující informace:

- Mapy zobrazuje umístění jednotlivých zařízení připojené k řešení. Při prvním spuštění řešení, existují čtyři simulovaný zařízení. Simulovaný zařízení jsou definovány jako Azure WebJobs a řešení vykreslete informace na mapě pomocí rozhraní API aplikace mapy Bing.
- V panelu **Telemetrie historie** znázorněním vlhkosti a teploty telemetrie z vybraných zařízení v v reálném čase a agregace dat zobrazí třeba vlhkost maximální minimální a průměr.
- Panel **Upozornění historie** ukazuje nedávné upozornění události překročení prahovou hodnotu hodnotu telemetrie. Můžete definovat vlastní upozornění kromě příklady vytvořil předkonfigurované řešení.

## <a name="view-the-device-list"></a>Zobrazení seznamu zařízení

V seznamu zařízení se registrovaných zařízení v řešení. Zobrazení a úprava metadat zařízení, přidat nebo odebrat zařízení a odešle příkazy pro zařízení.

1.  Klikněte na **zařízení** v levé nabídce zobrazte *seznam zařízení* pro toto řešení.

    ![Seznam zařízení na řídicím panelu][img-devicelist]

2.  V seznamu zařízení se, že jsou čtyři simulovaný zařízení vytvořená na základě procesu zřizovací.

3.  Klikněte na zařízení v seznamu zařízení a zobrazit podrobnosti o zařízení.

    ![Podrobnosti o zařízení v řídicím panelu][img-devicedetails]

**Podrobnosti o zařízení** panel obsahuje tři části:

- V části **Akce** obsahuje akce, které mohou vykonávat na zařízení. Pokud vypnete zařízení, jsou už není povolené telemetrie odesílat a přijímat příkazy. Pokud vypnete zařízení, můžete pak ji povolit znovu. Můžete přidat pravidlo přidružené zařízení, která aktivuje varování při telemetrie hodnotu větší než prahovou hodnotu. Můžete taky poslat příkazu zařízení. Po prvním připojení zařízení uvedeno řešení příkazy, které můžete odpovědět.
- V části **Vlastnosti zařízení** jsou uvedeny metadata zařízení. Některé z těchto metadat pochází z samotném zařízení (třeba výrobce) a některé je generováno aplikací řešení (například čas vytvoření). Můžete upravit metadata zařízení tady.
- V části **Ověřování klíče** zobrazí klávesy, které zařízení můžete použít k ověření se na řešení.

## <a name="send-a-command-to-a-device"></a>Příkaz Odeslat zařízení

V podokně podrobností zařízení zobrazí všechny příkazy, které podporuje určitému zařízení a umožňují můžete odeslat příkazy pro zařízení. Při prvním spuštění zařízení, odešle informace o příkazech, které podporuje řešení.

1.  Klikněte na položku **Příkazy** v podokně podrobností zařízení pro vybrané zařízení.

    ![Příkazy zařízení na řídicím panelu][img-devicecommands]

2.  Příkaz seznamu vyberte **PingDevice** .

3.  Klikněte na **příkaz Odeslat**.

4.  Můžete zobrazit stav příkaz do seznamu historie příkaz.

    ![Příkaz stav na řídicím panelu][img-pingcommand]

Řešení sleduje stav jednotlivé příkazy, které odesílá. Výsledek je zpočátku **na zjištění stavu úkolů**. Když zařízení hlásí, že provedl příkazu, výsledek je nastavena na **úspěšné**.

## <a name="add-a-new-simulated-device"></a>Přidání nového simulovaný zařízení

Při nasazení předkonfigurované řešení zřizujete automaticky čtyři ukázkové zařízení, která se zobrazí v seznamu zařízení. *Simulovaný zařízení* s v Azure WebJob jsou těchto zařízení. Simulovaný zařízení usnadňují experimentovat s předem řešení bez nutnosti nasazení real, fyzické zařízení. Pokud chcete připojit k řešení skutečné zařízení, přečtěte si článek [Připojte zařízení pro vzdálené sledování předkonfigurované řešení] [ lnk-connect-rm] kurz.

Podle těchto kroků předvedení přidejte simulovaný zařízení řešení:

1.  Přejděte zpět do seznamu zařízení.

2.  Klikněte na tlačítko **+ Přidat zařízení A** v levém dolním přidání zařízení.

    ![Přidání zařízení předkonfigurované řešení][img-adddevice]

3.  Na dlaždici **Simulovaného zařízení** klepněte na tlačítko **Přidat nové** .

    ![Nastavení nové podrobnosti o zařízení v řídicím panelu][img-addnew]
    
    Kromě vytváření nových simulovaný zařízení, můžete také přidat fyzické zařízení, pokud budete chtít vytvořit **Vlastní zařízení**. Další informace o připojování k řešení fyzických zařízení, přečtěte si článek [připojení zařízení vzdálené sledování předkonfigurované řešení IoT sadě][lnk-connect-rm].

4.  Vyberte **Manuální definovat ID zařízení**a zadejte název ID jedinečné zařízení například **mydevice_01**.

5.  Klikněte na **vytvořit**.

    ![Uložení nového zařízení][img-definedevice]

6. V kroku 3 **Přidat simulovaný zařízení**klepněte na **Hotovo** a vraťte se do seznamu zařízení.

7. Zařízení s **systém** můžete zobrazit v seznamu zařízení.

    ![Zobrazení nové zařízení v seznamu zařízení][img-runningnew]

8. Simulovaný telemetrie můžete taky zobrazit na nové zařízení na řídicím panelu:

    ![Zobrazení telemetrie na nové zařízení][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Úprava metadat zařízení

Pokud zařízení nejdřív připojí k řešení, odešle řešení jeho metadata. Při úpravě metadata zařízení prostřednictvím řídicím panelu řešení odešle nové hodnoty metadat zařízení a ukládá nové hodnot v databázi DocumentDB řešení. Další informace najdete v tématu [zařízení identity registru a DocumentDB][lnk-devicemetadata].

1.  Přejděte zpět do seznamu zařízení.

2.  V **Seznamu zařízení**vyberte nové zařízení a pak klikněte na **Upravit** a upravit **Vlastnosti zařízení**:

    ![Úprava metadat zařízení][img-editdevice]

3. Posuňte se dolů a proveďte změny šířky a délky vales. Klepněte na tlačítko **Uložit změny registru zařízení**.

    ![Úprava metadat zařízení][img-editdevice2]

4. Přejděte zpět do řídicího panelu, umístění zařízení změnil na mapě:

    ![Úprava metadat zařízení][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Přidání pravidla pro nové zařízení

Existuje žádné pravidlo pro nové zařízení, že jste právě přidali. V této části můžete přidat pravidlo, které se spustí zvukové upozornění, když teplota nové zařízení překročí 47 stupňů. Než začnete, oznámení, že historie na nové zařízení na řídicím panelu telemetrie je uvedený teploty zařízení nikdy překračuje 45 stupňů.

1.  Přejděte zpět do seznamu zařízení.

2.  V **Seznamu zařízení**vyberte nové zařízení a potom na položku **Přidat pravidlo** přidat pravidlo pro zařízení.

3. Vytvořte pravidlo, které používá **teplotní** jako pole data a **AlarmTemp** jako výstup při teplota překročí 47 stupňů:

    ![Přidání pravidla zařízení][img-adddevicerule]

4. Klikněte na **Uložit a zobrazit pravidla** uložte provedené změny.

5.  Klikněte na položku **Příkazy** v podokně podrobností zařízení pro nové zařízení.

    ![Přidání pravidla zařízení][img-adddevicerule2]

6.  Vyberte **ChangeSetPointTemp** ze seznamu příkazů a nastavte **SetPointTemp** 45. Klepnutím na příkaz **Odeslat**:

    ![Přidání pravidla zařízení][img-adddevicerule3]

7.  Přejděte zpět do řídicího panelu řešení. Po chvíli zobrazí se nová položka v podokně **Historie upozornění** při teplotní nové zařízení větší než mezní hodnota stupňů: 47:

    ![Přidání pravidla zařízení][img-adddevicerule4]

8. Možné prohlížet a upravovat všech pravidel řídicího panelu na stránce **pravidla** :

    ![Seznam pravidla zařízení][img-rules]

9. Možné prohlížet a upravovat všechny akce, které můžou být přijatých pravidlo na stránce **Akce** řídicího panelu:

    ![Akce seznamu zařízení][img-actions]

> [AZURE.NOTE] Je možné definovat akce, které můžete poslat-mailovou zprávu nebo služby SMS v odpovědi do pravidla nebo integraci s obchodními systémem prostřednictvím [Aplikace použití logických operátorů][lnk-logic-apps]. Další informace najdete v tématu [připojení aplikace použití logických operátorů k vaší Azure IoT sadu vzdálené sledování předem řešení][lnk-logicapptutorial].

## <a name="other-features"></a>Další funkce

Na portálu řešení, které se dají Hledat pro zařízení s specifické vlastnosti, například číslo modelu:

![Hledání pro zařízení][img-search]

Můžete zakázat zařízení a poté, co to je vypnutá, můžete ho odebrat:

![Zakázání a odebrání zařízení][img-disable]

## <a name="behind-the-scenes"></a>Na pozadí

Při nasazení předkonfigurované řešení procesu nasazení vytvoří více zdrojů v Azure předplatné, které jste vybrali. Můžete zobrazit tyto materiály Azure [portál][lnk-portal]. Procesu nasazení vytvoří **pole Skupina zdroje** s názvem podle názvu, jaká jste předkonfigurované řešení pro:

![Předkonfigurované řešení na portálu Azure][img-portal]

Nastavení jednotlivé zdroje můžete zobrazit výběrem v seznamu zdrojů ve skupině prostředků.

Můžete taky zobrazit zdrojového kódu pro předkonfigurované řešení. Vzdálené sledování zdrojový kód předkonfigurované řešení je v [azure iot vzdáleného – sledování] [ lnk-rmgithub] GitHub úložiště:

- Složka **DeviceAdministration** obsahuje zdrojového kódu řídicího panelu.
- Složka **Simulator** obsahuje zdrojového kódu pro simulovaný zařízení.
- Složka **EventProcessor** obsahuje zdrojového kódu back-end procesu, který pracuje s příchozí telemetrie.

Až budete hotovi, předkonfigurované řešení můžete odstranit ze svého předplatného Azure na [azureiotsuite.com] [ lnk-azureiotsuite] webu. Tento web umožňuje snadno odstranit všechny zdroje, které poskytnuté při vytvoření předkonfigurované řešení.

> [AZURE.NOTE] Abyste měli jistotu, že odstraníte všechno, co souvisí předkonfigurované řešení, odstraňte ji na [azureiotsuite.com] [ lnk-azureiotsuite] webu a jednoduše neodstraní skupina zdroje na portálu.

## <a name="next-steps"></a>Další kroky

Teď jste nainstalovali práci předem řešení, můžete dál začínáte pracovat s IoT sadu načtením v následujících článcích:

- [Vzdálené sledování návodu předkonfigurované řešení][lnk-rm-walkthrough]
- [Připojte zařízení vzdálené sledování předkonfigurované řešení][lnk-connect-rm]
- [Oprávnění na webu azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
