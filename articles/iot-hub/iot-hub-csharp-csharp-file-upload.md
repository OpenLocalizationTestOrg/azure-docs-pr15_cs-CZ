<properties
    pageTitle="Nahrání souborů na zařízeních pomocí IoT centrální | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak nahrát soubory ze zařízení používáte rozbočovač IoT Azure s C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Kurz: Jak nahrát soubory ze zařízení do cloudu pomocí IoT centrální

## <a name="introduction"></a>Úvod

Azure rozbočovači IoT je plně spravovaných služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a aplikace serverová. Předchozí výukové programy pro ([začít pracovat s IoT centrální] a [posílání zpráv cloudu zařízení s IoT centrální]) ilustrují základní zařízení na cloudu a cloudu zařízení zpráv funkce centrální IoT a kurz [proces zařízení cloudu zprávy] popisuje způsob, jak problémy se spolehlivým ukládání zpráv zařízení do cloudu v úložišti objektů blob Azure. Však v některých případech můžete nejde mapovat snadno data, která zařízení odeslat do relativně malým akceptované IoT Centrum zpráv zařízení do cloudu. Jako příklad lze uvést velkých souborů, které obsahují obrázky, videa, vibrační dat odběr vysokou frekvencí nebo, které obsahují některé formulář předem zpracovaný data. Tyto soubory jsou obvykle dávkové zpracování v cloudu pomocí nástrojů, jako jsou [Azure Data Factory] nebo [Hadoop] vrstvě. Při ukládání souborů ze zařízení jsou přednost před odesláním události, můžete dál používat funkce IoT Centrum zabezpečení a spolehlivosti.

Tento kurz je založena na kód v kurzu [odesílání zpráv cloudu zařízení s IoT Centrum] pro předvedení použití funkcí odeslat soubor IoT rozbočovače. Ukazuje, jak bezpečně poskytnout zařízení Azure objektů blob URI pro odeslání souboru a jak používat oznámení o odeslání souboru IoT centrální aktivovat zpracování soubor v aplikaci zadní.

Na konci kurzu, spusťte dvě aplikace konzoly Windows:

* **SimulatedDevice**, upravenou verzi aplikace vytvořené v kurzu [odesílání zpráv cloudu zařízení s IoT rozbočovači] , která odešle soubor k základnímu úložišti pomocí přidružení zabezpečení URI poskytovanou rozbočovače IoT.
* **ReadFileUploadNotification**, která přijímá oznámení o odeslání souboru z rozbočovače IoT.

> [AZURE.NOTE] Centrální IoT podporuje přes Azure IoT zařízení SDK mnoho platformy zařízení a jazyky (včetně C, Java a Javascript). Podívejte se do [Středisko pro vývojáře IoT Azure] pokyny krok za krokem připojení zařízení kód zobrazený v tomto kurzu a obecně k rozbočovači IoT Azure.

Chcete-li tento kurz potřebujete:

+ Microsoft Visual Studio 2015,

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Přidružení účet Azure úložiště k rozbočovači IoT

Protože simulovaný zařízení, odešlou souboru objektů blob, musíte mít účet [Azure úložiště] přidružené k centrální IoT. Pokud účet Azure úložiště přidružit k rozbočovači IoT, centru generovat přidružení zabezpečení URI využívající zařízení bezpečně nahrání souboru do kontejneru objektů blob. Služba IoT centrální a zařízení SDK koordinaci proces, který vytváří URI přidružení zabezpečení a díky kterému je dostupný na zařízení, můžete k nahrání souboru.

Postupujte podle pokynů uvedených v článku [nahrání souboru konfigurovat pomocí portálu Azure] [ lnk-configure-upload] přidružíte účet Azure úložiště pro vaše Centrum IoT.

## <a name="upload-a-file-from-a-simulated-device"></a>Nahrání souboru z simulovaný zařízení

V této části můžete změnit simulovaný zařízení aplikaci vytvořené v části [odesílání zpráv cloudu zařízení s IoT centrální] přijímat cloudu zařízení zprávy z centra IoT.

1. Ve Visual Studiu klikněte pravým tlačítkem **SimulatedDevice** projektu, klikněte na tlačítko **Přidat**a potom klikněte na **Existující položku**. Přejděte na soubor s obrázkem a vložit ho do projektu. Tento kurz se předpokládá s názvem obrázek `image.jpg`.

2. Klikněte pravým tlačítkem myši na obrázek a potom klikněte na **Vlastnosti**. Ujistěte se, že **zkopírovat do výstupu** je nastavena na **Kopírovat vždy**.

    ![][1]

3. V souboru **Program.cs** přidejte následující příkazy v horní části souboru:

        using System.IO;

4. Přidejte následující metodu třídy **aplikace** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    `UploadToBlobAsync` Metoda přenese ve zdrojovém souboru název a toku souboru na nahrát a zpracovává nahrávání k základnímu úložišti. Aplikaci konzoly zobrazuje dobu potřebnou k nahrání souboru.

5. Přidejte následující metodu v metodu **hlavní** přímo před `Console.ReadLine()` řádek:

        SendToBlobAsync();

> [AZURE.NOTE] V jednoduchosti saké tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení), jako navrhované v článku MSDN [Přechodná zpracování chyby].

## <a name="receive-a-file-upload-notification"></a>Příjem upozornění na odeslání souboru

V této části napíšete konzoly aplikace pro Windows obdrží upozornění nahrát soubor z centrální IoT.

1. V aktuálním Visual Studio řešení vytvoření nového projektu Visual Basic Windows pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **ReadFileUploadNotification**.

    ![Nový projekt ve Visual Studiu][2]

2. V Průzkumníku klikněte pravým tlačítkem **ReadFileUploadNotification** projektu a potom klikněte na **Spravovat balíčků NuGet**.

    Zobrazí se okno Správa balíčků NuGet.

2. Vyhledejte `Microsoft.Azure.Devices`, klikněte na tlačítko **instalovat**a přijměte podmínky použití. 

    To ke stažení, instalace a přidá odkaz na [Azure IoT - balíčku služby SDK NuGet] v aplikaci project **ReadFileUploadNotification** .

3. V souboru **Program.cs** přidejte následující příkazy v horní části souboru:

        using Microsoft.Azure.Devices;

4. Následující pole přidáte do **programu** předmětu. Nahraďte zástupný symbol hodnotou s IoT centrální připojovacího řetězce z [začít pracovat s IoT centrální]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Přidejte následující metodu třídy **aplikace** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Všimněte si, že vzorek přijímání stejné použitým přijímat cloudu zařízení zprávy z aplikace zařízení.

6. Nakonec přidejte následující řádky metody **hlavní** :

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Ve Visual Studiu klikněte pravým tlačítkem myši řešení a vyberte **Nastavení spuštění projekty**. Výběr **více projektů po spuštění**a pak vyberte **Spustit** akci **ReadFileUploadNotification** a **SimulatedDevice**.

2. Stiskněte klávesu **F5**. Obě aplikace by měly začít. Měli byste vidět nahrávání dokončení v jedné aplikaci konzoly a odeslání oznámení přijímané konzoly aplikace. [Azure portál] nebo Visual Studio Server Explorer umožňuje zkontrolovat stav uloženého souboru ve vašem úložišti Azure účtu.

  ![][50]


## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte využít možnosti nahrát soubor IoT centrální zjednodušit ukládání souborů ze zařízení. Můžete dál Objevte funkce centrální IoT a scénáře s v následujících článcích:

- [Vytvoření rozbočovači IoT programově][lnk-create-hub]
- [Úvod k C SDK][lnk-c-sdk]
- [Rozbočovač IoT SDK][lnk-sdks]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure portálu]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Odesílání zpráv cloudu zařízení s IoT centrální]: iot-hub-csharp-csharp-c2d.md
[Zpracování zpráv zařízení do cloudu]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s IoT rozbočovači]: iot-hub-csharp-csharp-getstarted.md
[Středisko pro vývojáře IoT Azure]: http://www.azure.com/develop/iot

[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - služby SDK NuGet balíčku]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


