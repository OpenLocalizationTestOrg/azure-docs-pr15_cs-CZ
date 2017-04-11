## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Jak vytvořit VNet na portálu Azure

Pokud chcete vytvořit VNet podle scénáře nad pomocí portálu Azure náhledu, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Nový** > **sítě** > **virtuální sítě**, klikněte na **Správce prostředků** ze seznamu **Vyberte nasazení modelu** a klikněte na **vytvořit**, jak je vidět na následujícím obrázku.

    ![Vytvoření VNet Azure portálu](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Na zásuvné **vytvořit virtuální síť** nastavení VNet jak je znázorněno na následujícím obrázku.

    ![Vytvoření zásuvné virtuální sítě](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Klikněte na **pole Skupina zdroje** a vyberte skupinu zdrojů k přidání VNet k nebo klikněte na **vytvořit nový** přidání VNet do nové skupiny prostředků. Následující obrázek ukazuje zdroje nastavení skupiny pro nové skupiny prostředků s názvem **TestRG**. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](../articles/resource-group-overview.md#resource-groups).

    ![Pole Skupina zdroje](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. V případě potřeby změňte nastavení **předplatného** a **umístění** pro váš VNet. 

6. Pokud nechcete zobrazit VNet dlaždici v **Startboard**, zakážete **Připnout k Startboard**. 

7. Klikněte na **vytvořit** a Všimněte si dlaždice s názvem **vytvoření virtuální sítě** , jak je znázorněno na následujícím obrázku.

    ![Vytváření virtuálních sítí dlaždice](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Počkejte VNet vytvořit a potom na zásuvné **virtuální sítě** , klikněte na **všechna nastavení** > **podsítí** > **Přidat** jak je vidět níže.

    ![Přidání podsítě na portálu Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Nastavení podsítě *back-end* podsítě, jak je ukázáno v následujícím příkladu a potom klikněte na **OK**. 

    ![Nastavení podsítě](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Všimněte si seznam podsítí, jak je znázorněno na následujícím obrázku.

    ![Seznam podsítí v VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
