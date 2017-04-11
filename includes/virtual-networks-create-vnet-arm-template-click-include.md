## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Nasazení šablony ARM pomocí klikněte na nasazení

Můžete znovu použít předdefinované šablony nahrát ARM do úložiště github spravovaný společností Microsoft a otevřete do komunity. Tyto šablony můžete nasazeném přímo z github, nebo stáhnout a upravit tak, aby odpovídal vašim potřebám. Abyste mohli nasadit šablonu, která vytvoří VNet s dvěma podsítí, postupujte následujícím způsobem.

1. V prohlížeči přejděte na [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Přejděte dolů v seznamu šablony a klikněte na **101 vnet dvě podsítí**. Vrácení souboru **README.md** , jak je ukázáno v následujícím příkladu.

    ![Soubor READEME.md v github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klikněte na **nasazení Azure**. Pokud je to potřeba, zadejte svoje Azure přihlašovací údaje. 
4. V zásuvné **Parametry** zadejte hodnoty, které chcete použít k vytvoření nové VNet a potom klikněte na **OK**. Následující obrázek ukazuje hodnoty naše scénář.

    ![Parametry šablony ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Klikněte na **pole Skupina zdroje** a vyberte skupinu zdrojů k přidání VNet k nebo klikněte na **vytvořit nový** VNet přidáte nové skupiny prostředků. Následující obrázek ukazuje zdroje nastavení skupiny pro nové skupiny prostředků s názvem **TestRG**.

    ![Pole Skupina zdroje](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. V případě potřeby změňte nastavení **předplatného** a **umístění** pro váš VNet.
6. Pokud nechcete zobrazit VNet dlaždici v **Startboard**, zakážete **Připnout k Startboard**.
5. Klikněte na **Leagl podmínky**, přečtěte si podmínky a klepněte na položku **zakoupit** souhlas. 
6. Kliknutím na **vytvořit** vytvoříte VNet.

    ![Odeslání nasazení dlaždice náhledu portálu](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Po dokončení nasazení, klikněte na **TestVNet** > **všechna nastavení** > **podsítí** zobrazíte vlastnosti podsítě, jak je ukázáno v následujícím příkladu.

    ![Vytvoření VNet náhled portálu](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)