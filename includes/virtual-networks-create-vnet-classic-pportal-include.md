## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Jak vytvořit klasické VNet na portálu Azure

Pokud chcete vytvořit klasické VNet podle výše uvedených scénáře, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Nový** > **sítě** > **virtuální sítě**, oznámení, že již zobrazuje **klasické**seznamu **Vyberte model nasazení** a klikněte na **vytvořit**, jak je vidět na následujícím obrázku.

    ![Vytvoření VNet Azure portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Na zásuvné **virtuální sítě** zadejte **název** VNet a klikněte na **adresní prostor**. Nastavení vašeho adresu místa VNet a jeho první podsítě a potom klikněte na **OK**. Následující obrázek ukazuje nastavení rozšířeného blokování CIDR naše scénáře.

    ![Adresa místa zásuvné](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Klikněte na **Pole Skupina zdroje** a vyberte skupinu zdrojů k přidání VNet k nebo klikněte na **vytvořit nové skupiny prostředků** , které chcete přidat VNet do nové skupiny prostředků. Následující obrázek ukazuje zdroje nastavení skupiny pro nové skupiny prostředků s názvem **TestRG**. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Vytvoření skupiny zásuvné zdroje](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. V případě potřeby změňte nastavení **předplatného** a **umístění** pro váš VNet. 

6. Pokud nechcete zobrazit VNet dlaždici v **Startboard**, zakážete **Připnout k Startboard**. 

7. Klikněte na **vytvořit** a Všimněte si dlaždice s názvem **vytvoření virtuální sítě** , jak je znázorněno na následujícím obrázku.

    ![Vytvoření VNet portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Počkejte VNet vytvořit a pokud se zobrazí pod dlaždice, kliknutím na Přidat další podsítí.

    ![Vytvoření VNet portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Měli byste vidět **Konfigurace** pro vaše VNet jak je ukázáno v následujícím příkladu. 

    ![Vytvoření VNet portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Klikněte na **podsítí** > **Přidat**, zadejte **název** a zadejte **rozsah adres (CIDR blok)** pro vaší podsítě a klikněte na **OK**. Následující obrázek ukazuje nastavení pro naše aktuální scénář.

    ![Vytvoření VNet Azure portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)