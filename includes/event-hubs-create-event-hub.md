## <a name="create-an-event-hub"></a>Vytvoření rozbočovači události

1. Přihlaste se k [portálu Azure][]a klikněte na tlačítko **Nový** v horní levé části obrazovky.

2. Klikněte na **Data + technologie pro analýzu**a potom klikněte na **Událost rozbočovače**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. V zásuvné **vytvořit názvů** zadejte název názvů. Systém okamžitě zkontroluje, zda název je k dispozici.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Po provedení, že oboru název je k dispozici, vyberte ceny osy (Basic nebo standardní). Zvolte také Azure předplatného, skupina zdroje a umístění, do kterého chcete vytvořit zdroj. 

2. Kliknutím na **vytvořit** vytvoříte obor názvů.

6. V seznamu obor názvů rozbočovače události klikněte na oboru nově vytvořený.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. V zásuvné názvů klikněte na **Událost rozbočovače**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. V horní části zásuvné klikněte na **Přidat rozbočovače události**.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Zadejte název rozbočovače události a potom klikněte na **vytvořit**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. V seznamu události rozbočovače klikněte na název nově vytvořený rozbočovače události. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Zpátky ve názvů zásuvné (není zvláštní událost centrální zásuvné) klikněte na **sdílené zásady přístupu**a klikněte na **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Klikněte na tlačítko Kopírovat a zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky. Uložte tento připojovací řetězec můžete dál v tomto kurzu.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Vaše Centrum událostí je vytvořena a máte řetězce připojení, které potřebujete k odesílání a přijímání události.

[Azure portálu]: https://portal.azure.com/