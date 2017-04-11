<properties
  pageTitle="Azure IoT sady a aplikace logiky | Microsoft Azure"
  description="Kurz o tom, jak připojit logiky aplikace Azure IoT sadě pro obchodní proces."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Kurz: Připojení k řešení Azure IoT sadu vzdálené sledování automaticky předem nakonfigurovaná logiky aplikace

[Sada Microsoft Azure IoT] [ lnk-internetofthings] vzdálené sledování předkonfigurované řešení je skvělý způsob, jak začít pracovat s sady začátku do konce funkce, které exemplifies IoT řešení. Tento kurz provede vás jak přidat logiku aplikace sady Microsoft Azure IoT vzdálené sledování předkonfigurované řešení. Tento postup ukazují, jak může trvat i dál IoT řešení připojením k obchodních procesů.

_Pokud hledáte informace o tom, jak by vzdálené sledování automaticky předem nakonfigurovaná řešení, přečtěte si článek [kurz: Začínáme s řešení IoT automaticky předem nakonfigurovaná][lnk-getstarted]._

Před zahájením tohoto kurzu, postupujte následujícím způsobem:

- Zřízení vzdálené monitorovací předkonfigurované řešení v Azure předplatné.

- Vytvoření účtu SendGrid můžete poslat e-mailu, který spustí obchodní proces. Můžete registraci bezplatnou zkušební verzi účtu na [SendGrid](https://sendgrid.com/) po kliknutí na **akci zdarma**. Po registraci mít účtu bezplatnou zkušební verzi, je potřeba vytvořit [klíč rozhraní API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) v SendGrid, která uděluje oprávnění k posílání pošty. Je třeba tento klíč rozhraní API dál v tomto kurzu.

Za předpokladu, že jste již zřízení vzdálené sledování automaticky předem nakonfigurovaná řešení, přejděte na skupina zdroje pro řešení [Azure portál][lnk-azureportal]. Skupina zdroje má stejný název jako název řešení jste zvolili při zřízení vzdálené sledování řešení. Ve skupině zdroje můžete zobrazit všechny zřizování Azure zdroje řešení s výjimkou aplikace služby Azure Active Directory, která můžete najít v portálu klasické Azure. Následující obrázek ukazuje zásuvné příklad **pole Skupina zdroje** pro vzdálené monitorovací předkonfigurované řešení:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Chcete-li začít nastavení pro práci s předem řešení aplikace použití logických operátorů.

## <a name="set-up-the-logic-app"></a>Nastavení aplikace logiky

1. Klikněte na __Přidat__ v horní části vaší zásuvné skupina zdroje na portálu Azure.

2. Hledání aplikace __Použití logických operátorů__, vyberte ho a klikněte na **vytvořit**.

3. Vyplňte __název__ a použít stejné **předplatné** a **pole Skupina zdroje** , které jste použili při zřízení vzdálené sledování řešení. Klikněte na __vytvořit__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Po dokončení nasazení, uvidíte, že aplikace použití logických operátorů uvedené jako zdroj ve skupině prostředků.

5. Klikněte na aplikaci logiky přejděte na zásuvné logiky aplikace, vyberte **Prázdné aplikace logiky** šablonu k otevření **Logiky aplikace Návrhář**.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Vyberte možnost __požádat o__. Tato akce určuje, že na příchozí žádost HTTP s konkrétní JSON ve formátu datové úkony aktivační událost.

7. Vložte následující do schématu požádat o JSON textu:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Po uložení aplikace pro použití logických operátorů, ale musíte nejdřív přidat akci, můžete zkopírujte adresu URL pro odeslání HTTP.

8. Klepněte na __+ nový krok__ ruční spuštění. Klikněte na **Přidat akci**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Vyhledejte **SendGrid - odeslání e-mailu** a klikněte na něj.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Zadejte název připojení, například **SendGridConnection**, zadejte **SendGrid rozhraní API klíč** , kterou jste vytvořili při nastavení účtu SendGrid a klikněte na **vytvořit**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Přidání e-mailové adresy, které vlastníte na **od** a **do** pole. Přidání **vzdálené sledování upozornění [DeviceId]** do pole **Předmět** . V poli **Text e-mailu** přidat **zařízení [DeviceId] vykazuje [measurementName] s hodnotou [measuredValue].** Kliknutím v části **vložíte data z předchozích kroků** můžete přidat **[DeviceId]**, **[measurementName]**a **[measuredValue]** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. V horní nabídce klikněte na __Uložit__ .

13. Klikněte na **požádat o** aktivační událost a zkopírujte hodnotu __Http příspěvek na tuto adresu URL__ . Tato adresa URL je třeba dál v tomto kurzu.

> [AZURE.NOTE] Použití logických operátorů Apps můžete spustit [mnoha různých typů akce] [ lnk-logic-apps-actions] včetně akcí v Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>Nastavení EventProcessor Web projektu

V této části připojení k aplikaci logiky jste vytvořili předkonfigurované řešení. K provedení tohoto úkolu, přidejte adresu URL aktivovat aplikaci použití logických operátorů k akci, která se aktivuje, když hodnota senzor zařízení přesahuje prahovou hodnotu.

1. Použití libovolná klienta klonovat nejnovější verzi [azure iot vzdáleného – sledování úložiště github][lnk-rmgithub]. Příklad:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Ve Visual Studiu otevřete __RemoteMonitoring.sln__ z místní kopii úložiště.

3. Otevřete soubor __ActionRepository.cs__ v **infrastruktury\\úložiště** složky.

4. Aktualizujte slovník **actionIds** __Http příspěvek na tuto adresu URL__ poznamenat z aplikace pro použití logických operátorů následujícím způsobem:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Uložte změny v řešení a zavřete Visual Studio.

## <a name="deploy-from-the-command-line"></a>Nasazení z příkazového řádku

V této části můžete nasadit aktualizovanou verzi vzdálené monitorovací řešení chcete nahradit aktuálně spuštěny v Azure verze.

1. Po [Nastavení vývojáře] [ lnk-devsetup] pokyny k nastavení prostředí pro nasazení.

2.  Abyste mohli nasadit místně, postupujte podle [místního nasazení] [ lnk-localdeploy] pokyny.

3.  Nasazení do cloudu a aktualizovat existující nasazení cloudu, postupujte podle [cloudu nasazení] [ lnk-clouddeploy] pokyny. Použijte název vaší původní nasazení jako název nasazení. Příklad původního nasazení označovalo jako **demologicapp**-li použít tento příkaz:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Při spuštění skriptu Tvůrce dotazů je potřeba používat stejný účet Azure předplatného, oblast a instancí služby Active Directory, který jste použili při zřízení řešení.

## <a name="see-your-logic-app-in-action"></a>V tématu Použití logických operátorů aplikace v akci

Vzdálené monitorovací předkonfigurované řešení má dvě pravidla ve výchozím nastavení když zřizujete řešení. Na zařízení **SampleDevice001** jsou obě pravidla:

* Teplotní > 38.00
* Vlhkost > 48.00

Pravidlo teploty spustí akci **Vysokoškolskou úroveň upozornění** a pravidlo vlhkost spustí akci **SendMessage** . Za předpokladu, že jste použili adresu URL pro obě akce **ActionRepository** předmětu, aplikace pro použití logických operátorů aktivační události pro buď pravidlo. Obě pravidla pomocí SendGrid odesílat e-mailu **na** adresu s podrobnostmi upozornění.

> [AZURE.NOTE] Aplikaci logiky zůstane spustí pokaždé, když došlo ke splnění je mezní hodnota. Chcete-li předejít nepotřebných e-mailů, můžete zakázat pravidla v portálu řešení nebo zakázání aplikace logiky [Azure portál][lnk-azureportal].

Kromě přijímat e-maily, můžete zobrazit při spuštění aplikace logiky na portálu:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Další kroky

Teď jste vypotřebovali logiky aplikace připojení předkonfigurované řešení k obchodních procesů, můžete se další informace o možnostech přizpůsobení předkonfigurované řešení:

- [Použití dynamického telemetrie s vzdálené monitorovací předkonfigurované řešení][lnk-dynamic]
- [Metadata zařízení informace ve vzdáleném sledování předkonfigurované řešení][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
