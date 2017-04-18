## <a name="create-a-device-management-enabled-iot-hub"></a>Vytvoření zařízení povolena IoT Centrální správa

Protože IoT centrální správy zařízení v náhledu, je potřeba vytvořit centrum IoT povolit správu zařízení. Všeobecně dostupná dosáhne IoT centrální správy zařízení tento kurz se aktualizují. Podle těchto kroků ukazují, jak to udělat pomocí portálu Azure.

1.  Přihlaste se k [portálu Azure].
2.  V Jumpbar klikněte na **Nový**a pak klikněte na **Internetu věcí**a klikněte na **Centrum IoT Azure**.

    ![][img-new-hub]

3.  V **Centrální IoT** zásuvné zvolte konfiguraci vaše Centrum IoT.

    ![][img-configure-hub]

  -   Do pole **název** zadejte název pro vaše Centrum IoT. Pokud **název** je platných a dostupných, se zobrazí v poli **název** zelená značka zaškrtnutí.
  -   Vyberte **úroveň ceny a měřítko**. Tento kurz nevyžaduje konkrétní vrstvě.
  -   Do **pole Skupina zdroje**vytvoření nové skupiny prostředků nebo vyberte stávající. Další informace najdete v tématu [pomocí zdroje skupin pro správu Azure prostředků].
  -   Zaškrtněte políčko **Povolit správu zařízení – verze PREVIEW**.
  -   **Umístění**vyberte umístění pro hostovat rozbočovače IoT. IoT centrální správy zařízení je dostupná jenom ve východoasijských USA, severní Evropy a jihovýchodní Asie během veřejné náhled.

    > [AZURE.NOTE]Pokud nemáte zaškrtněte políčko **Povolit správu zařízení**, vzorků nefungují.<br/>Zaškrtnutím **Povolit správu zařízení**vytvoříte náhled IoT centrální podporuje pouze východoasijských USA, severní Evropy a jihovýchodní Asie a nejsou určeny výrobní scénářích. Zařízení nelze migrovat do a od rozbočovače povolit správu zařízení.

4.  Po výběru vaše Centrum IoT možností konfigurace, klikněte na **vytvořit**. Může trvat několik minut Azure k vytvoření vaše Centrum IoT. Pokud chcete zkontrolovat stav, můžete sledovat průběh **Startboard** nebo na panelu **oznámení** .

    ![][img-monitor]

5.  Po úspěšném vytvoření centra IoT, zásuvné pro vaše Centrum automaticky otevře. Si poznamenejte **název hostitele**a potom klikněte na **sdílené zásady přístupu**.

    ![][img-keys]

6.  Vyberte zásadu **iothubowner** a pak zkopírujte a poznamenejte si připojovací řetězec v zásuvné **iothubowner** . Zkopírujte do umístění, které dostanete později vzhledem k tomu potřebujete tento kurz.

    > [AZURE.NOTE] Ve výrobním scénáře zkontrolujte, že zdržet pomocí přihlašovacích údajů **iothubowner** .

    ![][img-connection]

Teď jste vytvořili zařízení povolena IoT Centrální správa. Potřebujete připojovací řetězec pro dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření zařízení identitu

V této části používáte nástroj uzel s názvem [IoT centrální Explorer] [ iot-hub-explorer] vytvořit identitu zařízení pro účely tohoto návodu.

1. Spusťte následující v prostředí příkazového řádku:

    npm instalace – giothub-explorer@latest

2. Spusťte tento příkaz přihlášení k rozbočovače pamatovat nahrazovat `{service connection string}` pomocí připojovacího řetězce IoT centrální jste dříve zkopírovali:

    přihlášení iothub explorer "{služby připojovací řetězec}"

3. Nakonec vytvořte novou identitu zařízení s názvem `myDeviceId` pomocí příkazu:

    Průzkumník iothub vytvořit myDeviceId – připojovacího řetězce

Poznamenejte si zařízení připojovacího řetězce z výsledek. Tento připojovací řetězec používá aplikaci zařízení pro připojení k centrální IoT jako zařízení.

![][img-identity]

V nápovědě k [Začínáme s IoT centrální] [ lnk-getstarted] pro způsob, jak vytvořit zařízení identit programově.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure portálu]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Používání skupiny prostředků pro správu Azure prostředků.]: ../articles/azure-portal/resource-group-portal.md
