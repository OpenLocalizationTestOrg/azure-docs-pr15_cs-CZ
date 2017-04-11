## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Jak vytvořit VNet na portálu Azure

Pokud chcete vytvořit VNet podle výše uvedených scénáře, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://manage.windowsazure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **Nový** > **Síťové služby** > **Virtuální sítě** > **Vytvořit vlastní** jak je znázorněno na následujícím obrázku.

    ![Vytvoření VNet portálu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Na stránce **Podrobnosti virtuální sítě** zadejte **název** VNet, vyberte **umístění**a potom klikněte na šipku v pravém dolním rohu na stránce přejděte ke kroku 2. Následující obrázek ukazuje nastavení náš scénář.

    ![Stránka Podrobnosti virtuální sítě](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Na stránce **servery DNS a připojení k síti VPN** zadejte název a IP adresu až 9 serverů DNS. Pokud nezadáte serveru DNS, použije vaše VNet interní naming rozlišení rozlišení poskytovanou Azure. Pro naše scénář jsme nebude konfigurovat servery DNS.
5. Pokud chcete zajistit čárky webu VPN přístup k vaší VNet, zaškrtávací políčko **Konfigurovat VPN čárky webu** povolte. Pokud nenastavíte VPN čárky webu, můžete ho přidat do svého VNet kdykoli po vytvoření. Pro naše scénář jsme nebude konfigurovat VPN čárky webu.
6. Pokud chcete zadat na webu VPN propojení vaší VNet a jiné VNet nebo místní síti, **Konfigurovat VPN k webu** zaškrtávací políčko Povolit a určete, jestli má být připojení k pomocí **ExpressRoute** nebo poznámky a názvu sítě. Pokud není konfiguraci sítě VPN na webu, můžete ho přidat do svého VNet kdykoli po vytvoření. Pro naše scénář jsme nebude konfigurovat VPN k webu.
7. Klikněte na šipku v pravém dolním rohu stránky přejdete na krok 3, které nastavení naše scénář ukazuje následující obrázek.

    ![Servery DNS a VPN připojení stránky](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Na stránce **Virtuální sítě adresu mezery** v části **Spuštění IP**klikněte na *10.0.0.0* změnit adresní prostor VNet a zadejte počáteční adresní prostor, který chcete použít. Náš scénář zadejte *192.168.0.0*. 
9. V části **CIDR (počet adres)** vyberte je číslo posunuto masky podsítě. Náš scénář vyberte *16 (65536)*.
10. V části **PODSÍTÍ**klikněte na *podsítě-1* a v případě potřeby přejmenujte podsítě. Pro naše scénář přejmenujte ho na *FrontEnd*.

    >[AZURE.NOTE] Pokud kliknete mimo textové pole název podsítě, nebudete moct upravit název, pokud podsítě znovu. Můžete vyřešit tak, že budete muset uživateli odebrat podsítě kliknutím na tlačítko X napravo, potom přidáte nové podsítě podle pokynů v kroku 13 níže.

11. V části **Spuštění IP** první podsítě zadejte počáteční adresou podsítě. Náš scénář zadejte *192.168.1.0*.
12. V části **CIDR (počet adres)** vyberte je číslo posunuto masky podsítě pro první podsítě. Náš scénář vyberte *24 (256)*.
13. V případě potřeby, klikněte na **Přidat podsítě** přidáte nové podsítě. Pro naše scénář přidejte podsítě a opakujte kroky 10 až 12 ke konfiguraci VNet, jak je znázorněno na následujícím obrázku.

    ![Virtuální sítě adresu mezery stránky](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klikněte na tlačítko značka zaškrtnutí v pravém dolním rohu na stránce vytvořit VNet. Po několik sekund, než vaše VNet zobrazí v seznamu dostupné VNets, jak je znázorněno na následujícím obrázku.

    ![Nové virtuální sítě](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)