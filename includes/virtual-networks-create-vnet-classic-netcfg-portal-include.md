## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Jak vytvořit VNet pomocí souboru konfigurace sítě Azure portálu

Azure pomocí souboru xml k dispozici všechny VNets předplatné. Stáhněte si tento soubor a upravit nebo odstranit existující VNets a vytvářet nové. V tomto dokumentu zjistěte, jak stáhnout tento soubor, uvedené jako síť konfigurace (nebo netcgf) a upravte ji chcete vytvořit nové VNet. Zaškrtněte políčko [schéma konfigurace Azure virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx) zobrazíte další informace o konfiguračního souboru sítě.

Pokud chcete vytvořit VNet pomocí souboru netcfg portálu Azure, postupujte následujícím způsobem.

1. V prohlížeči přejděte na http://manage.windowsazure.com a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Přejděte dolů v seznamu služby a klikněte na **sítích** , jak je vidět níže.

    ![Azure virtuálních sítí](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. V dolní části stránky klikněte na tlačítko **EXPORTOVAT** , jak je ukázáno v následujícím příkladu.

    ![Tlačítko Exportovat](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Na stránce **Export konfigurace sítě** vyberte požadovaný export konfigurace virtuální sítě z předplatné a klikněte tlačítko se zatržítkem v dolním levém horním rohu stránky.
5. Podle pokynů vašeho prohlížeče pro uložení souboru **NetworkConfig.xml** . Nezapomeňte Poznámka: místo, kam chcete soubor uložit.
6. Otevřete soubor jste si uložili v kroku 5 nad pomocí libovolné aplikaci editor XML nebo text a hledejte **<VirtualNetworkSites>** prvek. Pokud máte žádné sítě vytvořen, všechny sítě se zobrazí jako vlastní **<VirtualNetworkSite>** prvek.
7. Pokud chcete vytvořit virtuální sítě popsané v tomto scénáři, přidejte následující XML těsně pod **<VirtualNetworkSites>** prvek:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Uložte soubor konfigurace sítě.
9.  Na portálu Azure v dolním levém horním rohu stránky klikněte na **Nový**a pak klikněte na **Síť služby**klikněte **Virtuální sítě**a potom klikněte na **IMPORT konfigurace** , jak je znázorněno na následujícím obrázku.

    ![Import konfigurace](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Na stránce **Importovat soubor konfigurace sítě** klikněte na **Procházet pro soubor...**, a pak přejděte do složky, které jste soubor uložili v předchozím kroku 8, vyberte soubor a klikněte na tlačítko **Otevřít**. Webové stránky by měl vypadat podobně jako na následujícím obrázku. V pravém dolním rohu stránky klikněte na tlačítko se šipkou přejděte k dalšímu kroku.

    ![Import sítě konfigurační soubor stránky](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Na stránce **vytváření sítě** Všimněte si položky pro váš nový VNet, jak je znázorněno na následujícím obrázku.

    ![Vytváření stránku sítě](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Klikněte na tlačítko značka zaškrtnutí v pravém dolním rohu na stránce vytvořit VNet. Po několik sekund, než vaše VNet zobrazí v seznamu dostupné VNets, jak je znázorněno na následujícím obrázku.

    ![Nové virtuální sítě](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)