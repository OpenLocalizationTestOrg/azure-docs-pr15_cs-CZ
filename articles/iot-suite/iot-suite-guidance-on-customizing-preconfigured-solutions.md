<properties
    pageTitle="Přizpůsobení automaticky předem nakonfigurovaná řešení | Microsoft Azure"
    description="Obsahuje pokyny, jak přizpůsobit Azure IoT sadu automaticky předem nakonfigurovaná řešení."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Přizpůsobení předkonfigurované řešení

Předkonfigurované řešení součástí sady IoT Azure ukazují služby v rámci sady spolupráce při řešení začátku do konce. Od tohoto počátečního bodu jsou různých míst, ve kterých můžete rozšířit a přizpůsobit řešení konkrétních situacích. Následující oddíly popisují tyto běžné přizpůsobení body.

## <a name="finding-the-source-code"></a>Hledání zdrojového kódu

Zdrojový kód předkonfigurované řešení je k dispozici na GitHub v následujících úložištích:

- Vzdálené sledování: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Prognostické údržba: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Zdrojový kód předkonfigurované řešení je k dispozici prokázat vzorky a postupy slouží k implementaci začátku do konce funkcí IoT řešení pomocí Azure IoT sadu. Můžete najít další informace o tom, jak sestavovat a nasazovat řešení v úložištích GitHub.

## <a name="changing-the-preconfigured-rules"></a>Změna předkonfigurované pravidel

Vzdálené sledování řešení zahrnuje tři úlohy [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/) implementace zařízení informace, telemetrie a pravidla použití logických operátorů zobrazí řešení.

Tři toku analýzy úloh a jejich syntaxi je popsaná v hloubku [vzdálené sledování návodu předkonfigurované řešení](iot-suite-remote-monitoring-sample-walkthrough.md). 

Můžete upravit tyto úlohy přímo ke změně logickou nebo přidání logiky konkrétní nefunguje. Úlohy toku analýzy najdete takto:
 
1. Přejděte na [portál Azure](https://portal.azure.com).
2. Přejděte na skupina zdroje se stejným názvem jako IoT řešení. 
3. Vyberte Azure toku analýzy úlohu, kterou chcete změnit. 
4. Ukončete úlohu výběrem **Ukončit**sady příkazů. 
5. Upravte vstupů, dotaz a výstupů.

    Jednoduchou úpravou je změnit dotaz pro danou úlohu **pravidla** pro použití **"<"** místo **">"**. Na portálu řešení, bude mít **">"** -li upravit pravidlo, ale zjistíte chování překlopené kvůli změně základní úlohy.

6. Zahájení projektu

> [AZURE.NOTE] Vzdálené sledování řídicího panelu závisí na konkrétní data, takže změnila úlohy mohou způsobit řídicího panelu selhání.

## <a name="adding-your-own-rules"></a>Přidání vlastního pravidla

Kromě změny předkonfigurované úlohy Azure toku analýzy, můžete portálu Azure přidat nové dotazy do existující projekty nebo přidat nové úkoly.

## <a name="customizing-devices"></a>Přizpůsobení zařízení

Jednu nejběžnější činností rozšíření pracuje s zařízení specifické pro nefunguje. Existuje několik způsobů pro práci s zařízení. Těchto postupů patří změny simulovaný zařízení podle nefunguje nebo využití [SDK IoT zařízení][] k připojení zařízení fyzicky řešení.

Podrobný postup přidání zařízení vzdálené sledování předkonfigurované řešení najdete v článku [Iot sadu připojení zařízení](iot-suite-connecting-devices.md) a [Vzdáleného sledování ukázkové C SDK](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , která je navržený pro práci s vzdálené monitorovací předkonfigurované řešení.

### <a name="creating-your-own-simulated-device"></a>Vytvoření vlastního simulovaného zařízení

Součástí vzdálené sledování řešení zdrojový kód (zmíněné), je .NET simulator. Tento simulator je zřízení jako součást řešení a můžete změnit na jiné metadata telemetrie odeslání nebo odpověď na různé příkazy.

Předkonfigurované simulator ve vzdáleném sledování předkonfigurované řešení je chladič zařízení, které posílá teploty a vlhkosti telemetrie simulator v aplikaci project [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) můžete upravit, když jste forked GitHub úložiště.

### <a name="available-locations-for-simulated-devices"></a>Dostupná umístění pro simulovaný zařízení

Nastavení výchozí umístění je v Seattlu/Redmond, WA, Spojené státy americké. Můžete změnit těmto místům [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Vytvoření a použití (fyzické) zařízení

[Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) stanovit knihoven připojení mnoho typů zařízení (jazyky a operačních systémech) do IoT řešení.

## <a name="modifying-dashboard-limits"></a>Změna limitů řídicího panelu

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Počet zařízení v rozevíracím seznamu řídicího panelu

Výchozí hodnota je 200. Můžete změnit toto číslo v [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Počet spojky zobrazení v ovládacím prvku mapy Bing

Výchozí hodnota je 200. Můžete změnit toto číslo v [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Časové období telemetrie grafu

Výchozí hodnota je 10 minut. Můžete změnit v [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Ruční nastavení aplikace rolí

Následující postup popisuje, jak přidat aplikaci role **správců** a **jen pro čtení** předkonfigurované řešení. Všimněte si, že předkonfigurované řešení pro zřízení z webu azureiotsuite.com už obsahovat role **Správce** a **jen pro čtení** .

Členy této role **jen pro čtení** můžete zobrazit řídicí panel a seznam zařízení, ale nejsou povolené přidat zařízení, můžete změnit atributy zařízení nebo odeslat příkazy.  Členové role **Správce** mají úplný přístup ke všem funkcím v řešení.

1. Přejděte na [portál Azure klasické][lnk-classic-portal].

2. Vyberte **služby Active Directory**.

3. Klikněte na název AAD klienta, který jste použili při zřízení řešení.

4. Klikněte na **aplikace**.

5. Klikněte na název aplikace, která odpovídá názvu předkonfigurované řešení. Pokud aplikace v seznamu nevidíte, vyberte **aplikací vlastní naší společnosti** v **zobrazení** rozevírací seznam a klikněte na značku zaškrtnutí.

6.  V dolní části stránky klikněte na **Spravovat Manifest** a potom **Stáhnout Manifest**.

7. To je možné stáhnout soubor .json do místního počítače.  Tento soubor otevřen pro úpravy v textovém editoru podle svého výběru.

8. Na řádku třetí soubor .json najdete:

  ```
  "appRoles" : [],
  ```
  Nahraďte to takto:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Uložte soubor aktualizované .json (mohou přepsat existující soubor).

10.  Na portálu Správa Azure v dolní části stránky vyberte **Spravovat Manifest** **Nahrát Manifest** nahrát .json soubor, který jste si uložili v předchozím kroku.

11. Role **Správce** a **jen pro čtení** jste přidali do aplikace.

12. Přiřadit jeden z těchto rolích pro uživatele v adresáři, najdete v článku [oprávnění na webu azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Zpětné vazby

Máte vlastní nastavení chcete zobrazit uvedené v tomto dokumentu? Přidejte návrhy funkce [Hlasové uživatele](https://feedback.azure.com/forums/321918-azure-iot)nebo komentář na tento článek dole. 

## <a name="next-steps"></a>Další kroky

Další informace o možnostech přizpůsobení předkonfigurované řešení najdete v tématu:

- [Připojení aplikace použití logických operátorů k řešení Azure IoT sadu vzdálené sledování automaticky předem nakonfigurovaná][lnk-logicapp]
- [Použití dynamického telemetrie s vzdálené monitorovací předkonfigurované řešení][lnk-dynamic]
- [Metadata zařízení informace ve vzdáleném sledování předkonfigurované řešení][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Zařízení IoT SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
