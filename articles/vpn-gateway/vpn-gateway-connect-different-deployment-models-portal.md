<properties 
   pageTitle="Jak se připojit k virtuální sítě správce na portálu klasické virtuálních sítí | Microsoft Azure"
   description="Naučte se vytvořit připojení VPN mezi klasické VNets a správce prostředků VNets pomocí Brána VPN a portálu"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Připojení virtuálních sítí z různých nasazení modely na portálu

> [AZURE.SELECTOR]
- [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
- [Prostředí PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure má nyní dvě modely správy: klasické a zdroje správce (SV). Pokud používáte Azure delší dobu, pravděpodobně máte Azure VMs a instance role spuštěné v klasickém VNet. Novější VMs a rolí instancí může běžet v VNet vytvořené ve Správci zdrojů. Tento článek vás provede připojení klasické VNets správce prostředků VNets umožňuje prostředky umístěné v samostatných nasazení modely vzájemně komunikovat připojení brány. 

Můžete vytvořit spojení mezi VNets, které jsou v různých předplatných a v různých oblastech. Můžete taky se připojit VNets, které už máte připojení k místní sítích, dokud brány, která byla nakonfigurována dynamické nebo na základě směrování. Další informace o připojení VNet VNet najdete v tématu [VNet VNet – nejčastější dotazy](#faq) na konci tohoto článku.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modely nasazení a metody pro VNet VNet připojení

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

V následující tabulce aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Při článek neexistuje, jsme propojit přímo ji z tabulky.<br><br>

**VNet VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>Prozkoumávání VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Před zahájením

Podle těchto kroků vás provede jednotlivými nastavení potřebných pro konfiguraci brány dynamické směrování systémem nebo pro každý VNet a vytvořit připojení VPN mezi brány. Konfigurace nepodporuje statické nebo zásadami bran. 

V tomto článku používáme portálu klasické, Azure portál a Powershellu. V současné době není možné vytvořit tuto konfiguraci pouze na portálu Azure.

### <a name="prerequisites"></a>Zjistit předpoklady pro

 - Obě VNets jste již vytvořili.
 - Rozsahy adres VNets překrývat navzájem, ani překrývat pomocí kterékoli z oblastí pro ostatní připojení, která brány může být připojen k.
 - Máte nainstalované nejnovější rutiny prostředí PowerShell (1.0.2 nebo novější). Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) . Ujistěte se, že musíte nainstalovat Management Service (ko) a rutinách zdroje správce (SV). 

### <a name="values"></a>Příklad nastavení

Příklad nastavení můžete použít jako odkaz.

**Klasický VNet nastavení**

Název VNet = ClassicVNet <br>
Umístění = západ USA <br>
Virtuální sítě adresu mezery = 10.0.0.0/24 <br>
Podsítě-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Název místní síti = RMVNetLocal <br>

**Správce prostředků VNet nastavení**

Název VNet = RMVNet <br>
Pole Skupina zdroje = RG1 <br>
Virtuální sítě IP adresu mezery = 192.168.0.0/16 <br>
Podsítě-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Umístění = východoasijských USA <br>
Název brány virtuální sítě = RMGateway <br>
Název brány veřejnou IP = gwpip <br>
Typ brány = VPN <br>
Typ VPN = na základě směrování <br>
Místní síti brány = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Část 1: Nastavení klasické VNet

V této části vytvoříme místní síti a bránu pro klasické VNet. Pokyny v tomto oddílu používat portál klasické. V současné době portálu Azure nenabízí všechna nastavení, které se týkají klasické VNet.

### <a name="part-1---create-a-new-local-network"></a>Část 1: vytvoření nové místní síti

Otevřete [portál klasické](https://manage.windowsazure.com) a přihlaste se pomocí účtu Azure.

1. V levém dolním rohu obrazovky, klikněte na **Nový** > **Síťové služby** > **Virtuální sítě** > **Přidat místní síti**.

2. V okně **Zadejte místní síti podrobnosti** zadejte název SV VNet, které se chcete připojit. V dialogovém okně **VPN zařízení IP adresy (nepovinné)** zadejte libovolný platný veřejnou IP adresu. Toto je právě dočasné zástupný symbol. Tuto IP adresu změnit později. V pravém dolním rohu okna klikněte na tlačítko se šipkou.
 
3. Na stránce **zadejte do adresního prostoru** v textovém poli **Spuštění IP** zadejte předponu sítě a CIDR blok pro správce prostředků VNet, které se chcete připojit. Toto nastavení slouží k určení adresní prostor pro směrování do VNet SV.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Část 2: spojte místní síti vaší VNet

1. Klikněte na **Virtuálních sítí** v horní části stránky přejděte na obrazovku virtuální sítě a potom kliknutím vyberte klasické VNet. Na stránce vašeho VNet klikněte na **Konfigurovat** navigaci na stránce konfigurace.

2. V části **připojení k webu** připojení zaškrtněte políčko **připojit k místní síti** . Vyberte místní síti, které jste vytvořili. Pokud máte víc místní sítě, které jste vytvořili, je nutné vybrat ten, který jste vytvořili představující správce prostředků VNet z rozevíracího seznamu.

3. Klepněte na tlačítko **Uložit** v dolní části stránky.

### <a name="part-3---create-the-gateway"></a>Část 3 – vytvoření brány

1. Po uložení nastavení, klikněte na **řídicí panel** v horní části stránky přejděte na stránku řídicího panelu. V dolní části stránky řídicího panelu klikněte na **Vytvořit brány**a potom na **Dynamické směrování**. Kliknutím na tlačítko **Ano** začít vytvářet brány. Dynamické směrování brána je nutný pro tuto konfiguraci.

2. Počkejte brány vytvořit. V některých případech to může trvat a 45 minut dokončete.

### <a name="ip"></a>Část 4: zobrazení veřejnou IP adresu brány

Po vytvoření brány můžete zobrazit na IP adresu brány na stránky **řídicího panelu** . Toto je veřejnou IP adresu brány. Zapište si nebo zkopírujte veřejnou IP adresu. Použili jste ho v dalších krocích při vytváření místní síti pro konfiguraci vašeho správce prostředků VNet.


## <a name="creatermgw"></a>Část 2: Nakonfigurujte nastavení VNet správce prostředků

V této části vytvoříme pro správce prostředků VNet Brána virtuální sítě a místní síti. Nespouštět podle těchto kroků až po načtení veřejnou IP adresu pro klasické VNet brány.

Snímky obrazovek jsou uvedeny příklady. Ujistěte se, že nahradit hodnoty vlastní. Pokud vytváříte tuto konfiguraci jako cvičení, podívejte se na tyto [hodnoty](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Část 1: Vytvoření podsítě brány

Před připojením virtuální sítě brány, musíte nejdřív vytvořit podsítě brány virtuální sítě, ke kterému chcete připojit. Vytvoření brány podsítě s počtem CIDR /28 nebo větší (/ 27, / 26, atd.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Část 2 – Vytvoření brány pro virtuální sítě


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Část 3 – vytvoření místní síti brány

Místní síti brány obvykle odkazuje místního pracoviště. Říká Azure IP adresu, která oblasti směrování umístění a veřejnou IP adresu zařízení pro dané umístění. Však v tomto případě záznam odkazuje rozsah adres a veřejnou IP adresu přidruženou k klasické VNet a branou virtuální sítě.

Název místní síti brány prodloužením doby Azure může označovat ho. Při vytváření brány virtuální sítě, můžete vytvořit brány místní síti. V této konfiguraci použijete veřejnou IP adresu, která byla přiřazená klasické brána VNet v [předchozí části](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Část 4: Kopírovat veřejnou IP adresu

Po dokončení brány virtuální sítě vytváření, zkopírujte veřejnou IP adresu, která souvisí s brány. Můžete ho použít při konfiguraci nastavení místní sítě pro klasické VNet. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Část 3: Úprava místní síti pro klasické VNet

Otevřete [portál klasické](https://manage.windowsazure.com).

2. Na portálu klasické přejděte dolů na levé straně a klikněte na **sítě**. Na stránce **sítě** klikněte na **Místní sítě** v horní části stránky. 

3. Kliknutím vyberte místní síti, které jste nakonfigurovali v části 1. V dolní části stránky klikněte na **Upravit**.

4. Na stránce **Zadejte podrobnosti místní síti** nahraďte zástupný symbol IP adresu veřejnou IP adresu pro správce prostředků brány, který jste vytvořili v předchozí části. Klikněte na šipku přejdete k následující části. Ověřte správnost **Adresní prostor** a klikněte na zaškrtnutí změny potvrďte.

## <a name="connect"></a>Část 4: Vytvoření připojení

V této části Vytvoření připojení mezi VNets. Postup pro tuto vyžaduje Powershellu. Toto připojení nelze vytvořit některým z portály. Ujistěte se, jste stáhli a nainstalovali klasické (ko) a rutiny prostředí PowerShell zdroje správce (SV).


1. Přihlaste se k účtu Azure v konzole Powershellu. Následující rutinu výzvu pro přihlašovací údaje pro váš účet Azure. Po přihlášení se nastavení účtu stahování tak, aby byly k dispozici na Azure Powershellu.

        Login-AzureRmAccount 

    Pokud máte víc předplatných Dostaňte seznam předplatné Azure.

        Get-AzureRmSubscription

    Určení předplatné, ke které chcete použít. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Přidejte svůj účet Azure pomocí klasické rutin prostředí PowerShell. Postup, můžete tento příkaz:

        Add-AzureAccount

3. Nastavte sdílenou klíč spuštěním v následujícím příkladu. V tomto příkladu `-VNetName` je název klasické VNet a `-LocalNetworkSiteName` je název, který jste zadali v místní síti při konfiguraci klasické portálu. `-SharedKey` Je hodnota, která můžete generovat a zadat. Tady zadanou hodnotu musí být stejnou hodnotu, kterou určíte v dalším kroku při vytvoření připojení k.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Vytvoření připojení k síti VPN spuštěním následující příkazy:
    
    **Nastavení proměnných**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Vytvoření připojení**<br> Všimněte si, že `-ConnectionType` je IPsec, ne Vnet2Vnet. V tomto příkladu `-Name` je název, který chcete zavolat připojení. V následujícím příkladu je vytvořeno připojení s názvem "*SV do klasického připojení*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Ověřte připojení

Ověřte připojení k pomocí portálu klasické, Azure portál nebo Powershellu. Podle těchto kroků můžete ověřit připojení. Nahraďte hodnoty vlastní.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>Nejčastější dotazy týkající se VNet VNet

Zobrazení podrobností o nejčastější dotazy týkající se další informace o připojení VNet VNet.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


