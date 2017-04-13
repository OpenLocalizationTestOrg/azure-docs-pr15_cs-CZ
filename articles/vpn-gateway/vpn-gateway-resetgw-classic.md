<properties
   pageTitle="Obnovení brány Azure VPN | Microsoft Azure"
   description="Tento článek vás provede obnovením bránu VPN Azure. Tento článek platí pro VPN bran v klasickou i modelů nasazení Správce prostředků."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Obnovení brány VPN Azure pomocí prostředí PowerShell


Tento článek vás provede obnovením bránu VPN Azure pomocí rutin prostředí PowerShell. Tyto pokyny zahrnují modelu klasické nasazení a nasazení modelu správce prostředků.

Obnovení brány Azure VPN je užitečné při ztrátě připojení k síti VPN křížově místní na jeden nebo více tunelů S2S VPN. V takovém případě zařízení VPN místní všechny fungují správně, ale nejsou prokázat IPsec tunelů s Azure VPN brány. 

Každý Azure VPN brány se skládá z dvě instance OM spuštěné v úsporném režimu aktivní konfiguraci. Při použití rutiny prostředí PowerShell resetovat brány restartuje brány a pak znovu konfigurace křížové místní k němu. Brány bude veřejnou IP adresu, kterou už má. To znamená, že nebudete potřebovat aktualizaci VPN směrovači konfigurace s veřejnou IP adresu nového Azure VPN brány pro.  

Po vydání příkazu aktuální aktivní instance brána Azure VPN po restartování okamžitě. Při selhání z active instance (právě restartování) úsporném instanci budou stručný mezeru. Mezeru by mělo být menší než jednu minutu.

Pokud není po prvním restartování obnovení připojení, zadejte stejné příkazu znovu restartujte druhou instanci OM (Nová brána aktivní). Pokud se dvěma restartování výzva za sebou, bude něco delší dobu kde jsou právě restartování obě OM instance (aktivní a úsporném režimu). To způsobí delší na připojení VPN k až 2 až 4 minut VMs dokončete nové.

Po dvou restartování počítače Pokud jste zaznamenali stále potíže s připojením mezi místní, otevřete žádost o podporu z portálu Microsoft Azure.

## <a name="before-you-begin"></a>Než začnete

Před obnovit bránu ověřte níže uvedené pro každý tunelem IPsec webu-server (S2S) VPN klíčové položky. Všechny neshodu v položkách povede odpojit tunelů S2S VPN. Ověření a oprava konfigurace místním prostředím a bran Azure VPN vyžaduje menší počet nepotřebných restartování a přerušení práce připojení na brány.

Před nastavením brány, zkontrolujte následující položky:

- Internetové adresy IP (adres VIP) brána Azure VPN a místní brána VPN jsou správně nakonfigurované v Azure a místních zásad VPN.
- Předem sdílený klíč se musí shodovat s na Azure a místních VPN brány.
- Pokud použijete zvláštní konfigurace IPsec/IKE, například šifrování transformační algoritmů a PFS (správný vpřed tajemství), zajistěte, aby Azure a místních bran VPN mají stejné konfigurace.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Obnovení brány VPN pomocí modelu nasazení řízení zdrojů

Rutina prostředí PowerShell správce prostředků pro resetování brány je `Reset-AzureRmVirtualNetworkGateway`. Následující příklad obnoví Azure VPN brány, "VNet1GW" v "TestRG1" pole Skupina zdroje.

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Obnovení VPN brány pomocí klasické nasazení modelu

Rutiny prostředí PowerShell pro resetování Azure VPN brány je `Reset-AzureVNetGateway`. Následující příklad obnoví brána Azure VPN pro virtuální sítě tomuto postupu říkalo "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Výsledek:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Další kroky
    
V tématu [reference pro rutiny prostředí PowerShell služba Správa](https://msdn.microsoft.com/library/azure/mt617104.aspx) a [reference pro rutiny prostředí PowerShell správce prostředků](http://go.microsoft.com/fwlink/?LinkId=828732) pro další informace.






