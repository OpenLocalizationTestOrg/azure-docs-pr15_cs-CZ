## <a name="create-an-iot-hub"></a>Vytvoření rozbočovači IoT

Vytvořte Centrum IoT simulovaný zařízení pro připojení k. Podle těchto kroků ukazují, jak to udělat pomocí portálu Azure.

1. Přihlaste se k [portálu Azure][lnk-portal].

2. V Jumpbar, klikněte na **Nový** > **Internet věci** > **Centrální IoT Azure**.

    ![Azure portál Jumpbar][1]

3. V **Centrální IoT** zásuvné zvolte konfiguraci rozbočovače IoT.

    ![IoT centrální zásuvné][2]

    * Do pole **název** zadejte název pro vaše Centrum IoT. Pokud **název** je platných a dostupných, zobrazí se v poli **název** zelená značka zaškrtnutí.
    * Vyberte [vrstvy ceny a měřítko][lnk-pricing]. Tento kurz nevyžaduje konkrétní osy. Pro účely tohoto návodu pomocí bezplatné osy F1.
    * Do **pole Skupina zdroje**vytvoření nové skupiny prostředků nebo vyberte stávající. Další informace najdete v tématu [pomocí zdroje skupin pro správu Azure prostředků][lnk-resource-groups].
    * **Umístění**vyberte umístění pro hostovat rozbočovače IoT. Pro účely tohoto návodu zvolte nejbližší umístění.

4. Po výběru rozbočovače IoT možnosti konfigurace, klikněte na **vytvořit**.  Může trvat několik minut Azure k vytvoření rozbočovače IoT. Pokud chcete zkontrolovat stav, můžete sledovat průběh Startboard nebo na panelu oznámení.

    ![Nový stav centrální IoT][3]

5. Po úspěšném vytvoření centrální IoT, klikněte na nový dlaždici pro vaše Centrum IoT Azure portálu otevřete zásuvné nové rozbočovače IoT. Si poznamenejte **název hostitele**a potom klikněte na **sdílené zásady přístupu**.

    ![Nové centrum zásuvné IoT][4]

6. V zásuvné **zásady přístupu sdílené** vyberte zásadu **iothubowner** a zkopírujte poznamenejte připojovací řetězec zásuvné **iothubowner** . Další informace najdete v tématu [řízení přístupu] [ lnk-access-control] "Azure centrální IoT vývojář příručka."

    ![Zásady zásuvné sdílený přístup][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
