<properties
   pageTitle="Provádění architektura sítě vysoce dostupné hybridní | Microsoft Azure"
   description="Jak implementovat Architektura zabezpečení webu webu sítě, který přesahuje Azure virtuální sítě a v místní síti připojení pomocí ExpressRoute převzetí brány VPN."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Provádění vysoce dostupné hybridní architektura sítě

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro připojení v místní síti k virtuální sítím na Azure pomocí ExpressRoute, na webu webu virtuální privátní sítě (VPN) jako převzetí připojení. Přenos přetékat mezi místní síti a Azure virtuální síť (VNet) prostřednictvím ExpressRoute připojení.  Pokud dojde ke ztrátě konektivitu okruh ExpressRoute, budou návštěvníci směrovaná přes IPSec VPN tunelem.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento plán používá správce zdroje, který Microsoft doporučuje nové nasazení.

Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace, kde rozdělení pracovního vytížení mezi místní síti a Azure.

- Aplikace spuštěné ve velkém měřítku a kritické pracovního vytížení vyžadujících vysoké škálovatelnost.

- Ve velkých zálohování a obnovení zařízení pro data, která musí být uloženy mimo.

- Zpracování velké dat úloh.

- Použití Azure jako havárie obnovení webu.

Všimněte si, že pokud okruhem ExpressRoute není k dispozici, směrování VPN pouze zpracovat soukromé prozkoumávání připojení. Veřejné prozkoumávání a Microsoft prozkoumávání připojení projde přes Internet.

## <a name="architecture-diagram"></a>Diagram architektury

>[AZURE.NOTE] [Služba Azure VPN brána] [ azure-vpn-gateway] implementuje dva typy virtuální sítě bran; ExpressRoute virtuální sítě brány a bran virtuální sítě VPN. V tomto dokumentu termín, který odkazuje *Brána VPN* Azure služby při fráze *Brána virtuální sítě VPN* a *ExpressRoute virtuální sítě brány* slouží k odkazování na implementace VPN a ExpressRoute brány.

V následujícím diagramu jsou uvedeny důležitou součástí tato architektura:

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento graf je ve "hybridní síti - ER VPN" stránky.

![[0]][0]

- **Azure virtuálních sítí (VNets).** Každý VNet umístěná v jedné oblasti Azure a můžete hostovat více úrovní aplikace. Aplikace úrovní můžete segmentována pomocí podsítí v jednotlivých VNet a/nebo sítě skupin zabezpečení (NSGs).

- **Místní podnikové sítě.** Toto je síť počítačích a zařízeních připojení přes privátní sítě místní systém v rámci organizace.

- **Virtuální privátní sítě zařízení.** Toto je zařízení nebo služba, která poskytuje externí připojení k místní síti. Zařízení VPN mohou být zařízení nebo může být softwarové řešení například směrování a vzdáleného přístupu služby () v systému Windows Server 2012.

> [AZURE.NOTE] Seznam podporovaných spotřebiče VPN a informace o konfiguraci vybrané VPN zařízení pro připojení k bráně VPN Azure, naleznete v pokynech k příslušné zařízení v [seznamu zařízení VPN podporovaných Azure][vpn-appliance].

- **Brána virtuální sítě VPN.** Brána virtuální sítě VPN umožňuje VNet připojení VPN spotřebiče v místní síti. Brána virtuální sítě VPN nakonfigurovaný k přijímaní žádostí z místní síti jen přes VPN zařízení. Další informace najdete v článku [připojení v místní síti k síti virtuální Microsoft Azure][connect-to-an-Azure-vnet].

- **ExpressRoute virtuální sítě brány.** ExpressRoute virtuální sítě brány umožňuje VNet připojení k okruh ExpressRoute pro připojení k síti místní.

- **Podsítě brány.** Virtuální sítě brány se uskutečňuje ve stejném podsítě.

- **Připojení VPN.** Připojení má vlastnosti, které určují připojení typ (IPSec) a klávesu nasdílel zařízení VPN místní šifrovat komunikaci.

- **ExpressRoute obvod.** Toto je vrstvy 2 nebo layer 3 okruh poskytnutých poskytovatel připojení, které spojení místní síti s Azure pomocí směrovači okraje. Obvod využívá infrastrukturu hardwaru spravuje poskytovatel připojení.

- **N-osy cloudu aplikace.** Toto je aplikace umístěná v Azure. Může obsahovat více úrovní s více podsítí připojení přes Vyrovnávání zatížení Azure. Přenosy v každé podsítě vyměřit poplatky pravidla definované pomocí [Skupin zabezpečení sítě Azure][azure-network-security-group](NSGs). Další informace najdete v tématu [Začínáme s Microsoft Azure zabezpečením][getting-started-with-azure-security].

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními.

### <a name="vnet-and-gatewaysubnet"></a>VNet a GatewaySubnet

Vytvoření brány ExpressRoute virtuální sítě a Brána virtuální sítě VPN ve stejném VNet. To znamená, že by měl sdílet stejnou podsítě s názvem **GatewaySubnet**

Pokud VNet už podsítě s názvem **GatewaySubnet**, aby byly /27 nebo větší adresní prostor. Pokud existující podsítě je příliš malý, odeberte ho následujícím způsobem a vytvořte nový účet, jak je vidět na další odrážku:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Pokud VNet neobsahuje podsítě s názvem **GatewaySubnet**, vytvořte novou následujícím způsobem:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>Virtuální privátní sítě a ExpressRoute brány

Ověřte, že vaše organizace splňuje [požadavky na předpoklady ExpressRoute] [ expressroute-prereq] připojit se k Azure.

Pokud už Brána virtuální sítě VPN v Azure VNet, odeberte tak, jak je ukázáno v následujícím příkladu.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Postupujte podle pokynů při [provádění hybridní architektura sítě s Azure ExpressRoute] [ implementing-expressroute] ExpressRoute připojení.

Postupujte podle pokynů při [provádění hybridní architektura sítě s Azure a místních VPN] [ implementing-vpn] k vytvoření připojení k síti VPN virtuální sítě brány.

Po vytvoření připojení brány virtuální sítě testovacím prostředí jako následující:

1. Zkontrolujte, že se můžete připojit z místní síti k Azure VNet.

2. Požádejte poskytovatele ukončení připojení k ExpressRoute pro účely testování.

3. Ověřte, zda je můžete pořád připojit z místní síti k vaší VNet Azure pomocí připojení brány virtuální sítě VPN.

4. Obraťte se na svého poskytovatele reestabilish ExpressRoute připojení.

## <a name="considerations"></a>Co byste měli zvážit

ExpressRoute aspektech, najdete v článku [provádění hybridní architektura sítě s Azure ExpressRoute] [ guidance-expressroute] pokyny.

Aspekty VPN k webu, najdete v článku [provádění hybridní architektura sítě s Azure a místních VPN] [ guidance-vpn] pokyny.

Otázky bezpečnosti obecné Azure, najdete v článku [cloudovým službám společnosti Microsoft a zabezpečení sítě][best-practices-security].

## <a name="solution-deployment"></a>Nasazení řešení

Pokud máte existující místní infrastruktury nakonfigurováno pomocí zařízení vhodné sítě, můžete nasadit architektura odkaz pomocí následujících kroků:

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Čekat na odkaz v portálu Azure otevřít a pak postupujte podle těchto kroků: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-hybrid-vpn-er-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Čekat na odkaz otevřít na portálu Azure zadejte pak postupujte podle těchto kroků: 
  - Vyberte možnost **použít existující** v části **Skupina zdroje** a zadejte `ra-hybrid-vpn-er-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architektura sítě architektury vysoce dostupné hybridní pomocí ExpressRoute a VPN brány"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
