## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Jak vytvořit VNet pomocí souboru konfigurace sítě z prostředí PowerShell

Azure pomocí souboru xml k dispozici všechny VNets předplatné. Stáhněte si tento soubor a upravit nebo odstranit existující VNets a vytvářet nové. V tomto dokumentu zjistěte, jak stáhnout tento soubor, uvedené jako síť konfigurace (nebo netcgf) a upravte ji chcete vytvořit nové VNet. Zaškrtněte políčko [schéma konfigurace Azure virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx) zobrazíte další informace o konfiguračního souboru sítě.

Pokud chcete vytvořit VNet pomocí souboru netcfg pomocí Powershellu, postupujte následujícím způsobem.

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../articles/powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.
2. Z konzoly Azure PowerShell použijte rutinu **Get-AzureVnetConfig** ke stažení konfiguračního souboru sítě spuštěním příkazu dole. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Očekávaný výstup:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Otevřete soubor jste si uložili v kroku 2 nahoře pomocí libovolné aplikaci editor XML nebo text a hledejte **<VirtualNetworkSites>** prvek. Pokud máte žádné sítě vytvořen, všechny sítě se zobrazí jako vlastní **<VirtualNetworkSite>** prvek.
4. Pokud chcete vytvořit virtuální sítě popsané v tomto scénáři, přidejte následující XML těsně pod **<VirtualNetworkSites>** prvek:

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

9.  Uložte soubor konfigurace sítě.
10. Z konzoly Azure PowerShell umožňuje rutinu **Set-AzureVnetConfig** nahrát soubor konfigurace sítě spuštěním příkazu dole. Všimněte si výstupu v části příkazu, byste měli vidět ve skupinovém rámečku **OperationStatus** **byl úspěšný** . Pokud tedy v opačném případě zkontrolujte soubor xml chyby.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Tady je očekávané výstup pro výše uvedené příkaz:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Z konzoly Azure PowerShell získáte pomocí rutiny **Get-AzureVnetSite** ověřit, zda bude novou síť přidány spuštěním příkazu dole. 

        Get-AzureVNetSite -VNetName TestVNet

    Tady je očekávané výstup pro výše uvedené příkaz:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded