<properties 
   pageTitle="Jak se připojit k správce prostředků virtuální sítě pomocí prostředí PowerShell klasické virtuálních sítí | Microsoft Azure"
   description="Naučte se vytvořit připojení VPN mezi klasické VNets a správce prostředků VNets pomocí VPN brány a prostředí PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Připojení virtuálních sítí z různých nasazení modelů pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
- [Prostředí PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure má nyní dvě modely správy: klasické a zdroje správce (SV). Pokud používáte Azure delší dobu, pravděpodobně máte Azure VMs a instance role spuštěné v klasickém VNet. Novější VMs a instance role může běžet v VNet vytvořené ve Správci zdrojů. Tento článek vás provede připojení klasické VNets správce prostředků VNets umožňuje prostředky umístěné v samostatných nasazení modely vzájemně komunikovat připojení brány.

Můžete vytvořit spojení mezi VNets, které jsou v různých předplatných a v různých oblastech. Můžete taky se připojit VNets, které už máte připojení k místní sítích, dokud brány, která byla nakonfigurována dynamické nebo na základě směrování. Další informace o připojení VNet VNet najdete v tématu [VNet VNet – nejčastější dotazy](#faq) na konci tohoto článku.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Modely nasazení a způsoby připojení VNets v různých nasazení modely

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

V následující tabulce aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Při článek neexistuje, jsme propojit přímo ji z tabulky.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Prozkoumávání VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Před zahájením

Podle těchto kroků vás provede jednotlivými nastavení potřebných pro konfiguraci brány dynamické směrování systémem nebo pro každý VNet a vytvořit připojení VPN mezi brány. Konfigurace nepodporuje statické nebo zásadami brány.

### <a name="prerequisites"></a>Zjistit předpoklady pro

 - Obě VNets jste již vytvořili.
 - Rozsahy adres VNets překrývat navzájem, ani překrývat pomocí kterékoli z oblastí pro ostatní připojení, která brány může být připojen k.
 - Máte nainstalované nejnovější rutiny prostředí PowerShell (1.0.2 nebo novější). Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) . Ujistěte se, že musíte nainstalovat Management Service (ko) a rutinách zdroje správce (SV). 

### <a name="exampleref"></a>Příklad nastavení

Příklad nastavení můžete použít jako odkaz.

**Klasický VNet nastavení**

Název VNet = ClassicVNet <br>
Umístění = západ USA <br>
Virtuální sítě adresu mezery = 10.0.0.0/24 <br>
Podsítě-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Název místní síti = RMVNetLocal <br>
GatewayType = DynamicRouting

**Správce prostředků VNet nastavení**

Název VNet = RMVNet <br>
Pole Skupina zdroje = RG1 <br>
Virtuální sítě IP adresu mezery = 192.168.0.0/16 <br>
Podsítě-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Umístění = východoasijských USA <br>
Název brány veřejnou IP = gwpip <br>
Místní síti brány = ClassicVNetLocal <br>
Virtuální název brány sítě = RMGateway <br>
Konfigurace brány IP adresách = gwipconfig


## <a name="createsmgw"></a>Část 1 – konfigurace klasické VNet

### <a name="part-1---download-your-network-configuration-file"></a>Část 1 – stažení souboru konfigurace sítě

1. Přihlaste se k účtu Azure v konzole prostředí PowerShell s zvýšenými oprávněními. Následující rutinu výzvu pro přihlašovací údaje pro váš účet Azure. Po přihlášení se stáhne nastavení účtu tak, aby byly k dispozici na Azure Powershellu. Použijete rutiny prostředí PowerShell ko dokončete tuto část konfiguraci.

        Add-AzureAccount

2. Spuštěním následujícího příkazu exportujte konfigurační soubor Azure sítě. Umístění pole soubor k exportu do jiného umístění v případě potřeby můžete změnit. Bude upravit tento soubor a pak je naimportujte do Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Otevřete soubor XML, který jste stáhli pro úpravy. Příklad konfiguračního souboru sítě najdete v článku [Schéma konfigurace sítě](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Část 2 – Zkontrolujte podsítě brány

V elementu **VirtualNetworkSites** dodejte brány podsítě vaší VNet Pokud nebyla byl vytvořen. Když pracujete s konfiguračního souboru sítě, podsítě brány musí být s názvem "GatewaySubnet" nebo Azure nelze rozpoznat a použít je jako podsítě brány.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Příklad:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Část 3 – Přidání webu místní síti

Místní síti webů, které přidáte představuje SV VNet, ke kterému chcete připojit. Bude pravděpodobně nutné přidat prvek **LocalNetworkSites** k souboru, pokud ještě neexistuje. V tomto okamžiku v konfiguraci, VPNGatewayAddress může být libovolný platný veřejnou IP adresu protože jsme ještě nevytvořili bránu pro VNet správce prostředků. Po vytvoření brány, jsme nahraďte tento zástupný symbol IP adresa správná veřejnou IP adresu přiřazené k bráně SV.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Část 4: spojte VNet s webem místní síti

V této části jsme zadejte, který chcete připojit VNet na web místní síti. V tomto případě je VNet správce prostředků, na které odkazuje dříve. Ujistěte se, že názvy odpovídat. Tento krok nevytvoří brány. Určuje místní síti, které brány se připojí k.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Část 5 – soubor uložit a odeslat

Uložte soubor pak importovat do Azure spuštěním následujícího příkazu. Zkontrolujte, že změníte cesta k souboru podle potřeby ve vašem prostředí.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Měli byste vidět podobnou tento výsledek zobrazující úspěšnou importu.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Část 6 – Vytvoření brány 

Brána VNet můžete vytvořit pomocí portálu klasické nebo pomocí Powershellu.

Umožňuje vytvořit dynamické směrování bránu v následujícím příkladu:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Zkontrolujte stav brány pomocí `Get-AzureVNetGateway` rutiny.


## <a name="creatermgw"></a>Část 2: Nakonfigurujte bránu VNet SV

Pokud chcete vytvořit brány VPN pro VNet SV, postupujte podle následujících pokynů. Nespouštět krocích, dokud po načtení veřejnou IP adresu pro klasické VNet brány. 

1. **Přihlaste se k účtu Azure** v konzole Powershellu. Následující rutinu výzvu pro přihlašovací údaje pro váš účet Azure. Po přihlášení se nastavení účtu stahování tak, aby byly k dispozici na Azure Powershellu.

        Login-AzureRmAccount 

    Pokud máte víc předplatných Dostaňte seznam předplatné Azure.

        Get-AzureRmSubscription

    Určení předplatné, ke které chcete použít. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Vytvoření brány místní síti**. V síti virtuální brány místní síti obvykle odkazuje místního pracoviště. V tomto případě brány místní síti odkazuje na klasické VNet. Pojmenujte ho prodloužením doby Azure můžete odkazují na ni a také zadat předponu místo adresy. Azure používá předponu IP adresy, kterou zadáte určit které přenos odešlete místního pracoviště. V případě potřeby upravte informace tady později, před vytvořením brány, můžete změnit hodnoty a znova spusťte vzorku.<br><br>
    - `-Name`je název, které chcete přiřadit odkázat na brány místní síti.<br>
    - `-AddressPrefix`je adresní prostor pro klasické VNet.<br>
    - `-GatewayIpAddress`je veřejnou IP adresu klasické VNet brány. Ujistěte se, pokud chcete změnit v následujícím příkladu, aby odrážely správná IP adresa.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Žádost o veřejnou IP adresu** přidělená ho bráně virtuální sítě pro VNet správce prostředků. Nelze zadat IP adresy, kterou chcete použít. IP adresy se dynamicky přidělit brány virtuální sítě. Však neznamená, že se změní na IP adresu. Pouze virtuální sítě brány IP adresu změny při odstranění a k opětovnému vytvoření brány. Nezmění přes Změna velikosti, obnovení nebo jiné interní údržbu/inovace brány.<br>V tomto kroku jsme také nastavit proměnnou, která je použita později.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Ověření, že síť virtuální má podsítě brány**. Pokud existuje žádné brány podsítě ho vytvořit. Zkontrolujte, že podsítě brány jmenuje *GatewaySubnet*.

5. **Načtení podsítě** používá brána spuštěním následujícího příkazu. V tomto kroku jsme také nastavit proměnnou se nemusí používat v dalším kroku.
    - `-Name`stejný název VNet správce prostředků.
    - `-ResourceGroupName`je skupina zdroje, kterému je přidružen VNet. Podsítě brány, musí existovat pro tento VNet a musí mít název *GatewaySubnet* správně fungovat.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Vytvoření konfiguraci brány IP adres**. Konfigurace brány definuje podsítě a veřejnou IP adresu používat. Umožňuje vytvořit konfiguraci brány v následujícím příkladu.<br>V tomto kroku `-SubnetId` a `-PublicIpAddressId` musí být parametrů vlastnost id z podsítě a IP adresy objektů, respektive. Nelze použít jednoduchý řetězec. V kroku požádat o veřejnou IP a krok načíst podsítě jsou nastaveny tyto proměnné.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Vytvoření brány virtuální sítě Správce prostředků** spuštěním následujícího příkazu. `-VpnType` Musí být *RouteBased*. Může trvat a 45 minut pro dokončení.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Zkopírujte veřejnou IP adresu** jednou brány VPN byl vytvořen. Můžete ho použít při konfiguraci nastavení místní sítě pro klasické VNet. Následující rutinu slouží k načtení veřejnou IP adresu. Na veřejnou IP adresu je uvedený v vrácení jako *adresa IP*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Část 3: Úprava místní síti pro klasické VNet

Máte tento krok, který buď Export souboru konfigurace sítě, úpravy, ukládání a import souboru zpět na Azure. Nebo můžete změnit toto nastavení klasické portálu. 

### <a name="to-modify-in-the-portal"></a>Chcete-li změnit na portálu

1. Otevřete [portál klasické](https://manage.windowsazure.com).

2. Na portálu klasické přejděte dolů na levé straně a klikněte na **sítě**. Na stránce **sítě** klikněte na **Místní sítě** v horní části stránky. 

3. Na stránce **Místní sítě** kliknutím vyberte místní síti, že můžete konfigurovat část 1 přejděte do dolní části stránky a klikněte na **Upravit**.

4. Na stránce **Zadejte podrobnosti místní síti** nahraďte zástupný symbol IP adresu veřejnou IP adresu pro správce prostředků brány, který jste vytvořili v předchozí části. Klikněte na šipku přejdete k následující části. Ověřte správnost **Adresní prostor** a klikněte na zaškrtnutí změny potvrďte.

### <a name="to-modify-using-the-network-configuration-file"></a>Chcete-li změnit pomocí konfiguračního souboru sítě

1. Exportujte do souboru konfigurace sítě.
2. V textovém editoru upravte VPN brány IP adresu.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Uložte provedené změny a importujte upravený soubor zpátky na Azure.


## <a name="connect"></a>Část 4: Vytvoření připojení mezi brány

Vytvoření připojení mezi brány vyžaduje Powershellu. Budete muset přidat svůj účet Azure pomocí klasické rutin prostředí PowerShell. K tomu můžete provádět následující rutinu: 
        
    Add-AzureAccount

1. Spuštěním následujícího příkazu **Nastavení sdílené klíče** . V tomto příkladu `-VNetName` je název klasické VNet a `-LocalNetworkSiteName` je název, který jste zadali v místní síti při konfiguraci klasické portálu. `-SharedKey` Je hodnota, která můžete generovat a zadat. Tady zadanou hodnotu musí být stejnou hodnotu, kterou určíte v dalším kroku při vytvoření připojení k. Výnosové v tomto příkladu by měl zobrazit **Stav: úspěšné**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Vytvoření připojení k síti VPN spuštěním následující příkazy.

    **Nastavení proměnných**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Vytvoření připojení**<br> Všimněte si, že `-ConnectionType` je IPsec, ne Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Připojení k můžete zobrazit v některém z portálu nebo pomocí `Get-AzureRmVirtualNetworkGatewayConnection` rutiny.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>Nejčastější dotazy týkající se VNet VNet

Zobrazení podrobností o nejčastější dotazy týkající se další informace o připojení VNet VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


