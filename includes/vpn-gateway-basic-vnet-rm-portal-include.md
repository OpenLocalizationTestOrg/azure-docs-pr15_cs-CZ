Vytvoření VNet v modelu nasazení Správce prostředků pomocí portálu Azure, postupujte následujícím způsobem. Snímky obrazovek jsou uvedeny příklady. Ujistěte se, že nahradit hodnoty vlastní. Další informace o práci s virtuálních sítí najdete v článku [Přehled virtuální sítě](../articles/virtual-network/virtual-networks-overview.md).

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby Přihlaste se pomocí účtu Azure.

2. Klikněte na **Nový**. Do pole **Hledat Tržiště** text "Virtuální síť". Vyhledejte **Virtuální sítě** ze seznamu vrácené a klepnutím otevřete zásuvné **Virtuální sítě** .

    ![Vyhledejte virtuální sítě zdroje zásuvné] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Vyhledejte virtuální sítě zdroje zásuvné")

3. V dolní části zásuvné virtuální sítě ze seznamu **Vyberte model nasazení** vyberte **Správce**a pak klikněte na **vytvořit**.


    ![Vyberte správce prostředků] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Vyberte správce prostředků")

4. Na zásuvné **vytvořit virtuální síť** nakonfigurujte nastavení VNet. Při vyplňování polí červený vykřičník po jsou platné znaky v příslušném poli stane zelená značka zaškrtnutí.

    ![Ověření pole] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Ověření pole")

5. **Vytvořit virtuální síť** zásuvné vypadá podobně jako v následujícím příkladu. Doména může obsahovat hodnoty, které se vyplní automaticky. Pokud ano, nahraďte hodnoty vlastní.

    ![Vytvořit virtuální sítě zásuvné] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Vytvořit virtuální sítě zásuvné")

6. **Název**: Zadejte název pro virtuální sítě.

7. **Adresní prostor**: Zadejte vzdálenost adresu. Pokud ještě několika mezer adresu přidat přidejte první adresní prostor. Můžete přidat další adresa mezery později, po vytvoření VNet.
 
8. **Podsítě název**: Přidání podsítě název a rozsah adres podsítí. Přidáním dalších podsítí později, po vytvoření VNet.

10. **Předplatné**: Ověřte, že předplatné uvedené je správná. Předplatné můžete změnit pomocí rozevíracího seznamu.

11. **Pole Skupina zdroje**: Vyberte existující skupiny zdrojů nebo vytvořte novou tak, že zadáte název nové skupiny prostředků. Pokud vytváříte novou skupinu, zadejte název skupiny zdroje podle hodnot plánované konfigurace. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](resource-group-overview.md#resource-groups).

12. **Umístění**: Vyberte umístění pro váš VNet. Umístění Určuje, kde se bude nacházet prostředky, které můžete nasadit do této VNet.

13. Vyberte **Připnout na řídicí panel** , pokud chcete, aby mohli snáz našli váš VNet na řídicím panelu a potom klikněte na **vytvořit**.
    
    ![Připnout do řídicího panelu] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "Připnout do řídicího panelu")

14. Po kliknutí na příkaz **vytvořit**, zobrazí se na dlaždici na řídicím panelu, které budou obsahovat průběh vašeho VNet. Na dlaždici se mění VNet se vytváří.

    ![Vytváření virtuálních sítí dlaždice] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Vytváření virtuálních sítí dlaždice")