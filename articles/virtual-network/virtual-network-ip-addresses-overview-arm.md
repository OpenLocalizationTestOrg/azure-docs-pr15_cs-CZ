<properties
   pageTitle="Veřejných a privátních IP adres v správce prostředků Azure | Microsoft Azure"
   description="Další informace o veřejných a privátních IP adres v správce prostředků Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP adresy v Azure
IP adresy můžete přiřadit Azure materiály ke komunikaci s další Azure zdroje, v místní síti a k Internetu. Existují dva typy IP adresy, které můžete použít v Azure:

- **Veřejné IP adresy**: používá pro komunikaci s Internetu, ať už Azure veřejné služby
- **Soukromé IP adresy**: používá pro komunikaci v Azure virtuální sítě (VNet) a vaší místní sítě VPN brána nebo ExpressRoute okruh rozšířit sítě Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](virtual-network-ip-addresses-overview-classic.md).

Pokud máte zkušenosti s modelu klasické nasazení, obraťte [rozdíly ve IP adres mezi klasické a správce prostředků](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Veřejné IP adres
IP adresy povolit Azure materiály ke komunikaci s Internet a Azure veřejných webových služeb, jako je [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/), [Rozbočovače události Azure](https://azure.microsoft.com/services/event-hubs/), [SQL databáze](../sql-database/sql-database-technical-overview.md)a [Azure úložiště](../storage/storage-introduction.md).

V okně Správce prostředků Azure na [veřejnou IP](resource-groups-networking.md#public-ip-address) adresu je zdroj, který má své vlastní vlastnosti. Veřejné zdroje IP adresy můžete propojit s některým z následujících materiálů:

- Virtuálních počítačích (OM)
- Vyrovnávání zatížení internetového
- Virtuální privátní sítě bran
- Aplikace brány

### <a name="allocation-method"></a>Metoda přidělení
Kdy je k IP adrese přidělit *veřejné* zdroje IP - *dynamické* nebo *statické*dvěma způsoby. Výchozí metoda rozdělení je *dynamické*, kde k IP adrese **přiřazen v době jeho vytvoření** . Místo toho na veřejnou IP adresu je přidělený při spuštění (nebo vytvořte) přidružené zdroje (třeba OM nebo k načtení vyrovnávání). IP adresu vydání po ukončení (nebo po odstranění) zdroje. To způsobí, že na IP adresu změnit po ukončení a začněte zdroje.

Abyste měli jistotu, že na IP adresu přidruženou zdroje se nezmění, můžete nastavit metodu přidělení explicitně na *statické*. V tomto případě se okamžitě přiřadí k IP adrese. Uvolnění pouze v případě, že odstraníte zdroj nebo změnit její způsob rozdělení na *dynamické*.

>[AZURE.NOTE] I v případě, že má argument metodu přidělení *statické*, nelze zadat skutečné IP adresu přiřazené *veřejnou IP zdroje*. Místo toho získá přidělené z fondu dostupné IP adres v Azure umístění vytvořené v zdroje.

Veřejné statické IP adresy se obvykle používají v následujících situacích:

- Koncoví uživatelé potřebovali aktualizovat pravidla brány firewall komunikovat s Azure zdroje.
- DNS překlad, pokud změny ve IP adresu vyžadují aktualizaci záznam.
- Azure zdroje komunikovat s jiných aplikací ani služby, které používají model zabezpečení na základě adresy IP.
- Používáte certifikáty SSL propojené k IP adrese.

>[AZURE.NOTE] Seznam rozsahů IP ze kterých veřejnou IP adresy (dynamické/statického) přiřazených k Azure zdroje je publikován na [rozsahy IP Azure Datacentra](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Hostname služeb překládá názvy DNS
Můžete zadat popisek název domény DNS u veřejné zdroje IP vytvoří mapování *domainnamelabel*. *umístění*. cloudapp.azure.com na veřejnou IP adresu v Azure Správa přístupových práv DNS servery. Například pokud budete vytvářet veřejné zdroje IP s **contoso** jako *domainnamelabel* **Západní USA** Azure *umístění*úplný název (FQDN) **contoso.westus.cloudapp.azure.com** se překládal na veřejnou IP adresu zdroje. Tento plně kvalifikovaný název domény můžete použít k vytvoření záznamu CNAME vlastní doménu přejdete na veřejnou IP adresu v Azure.

>[AZURE.IMPORTANT] Vytvoření název název domény musí být jedinečný v rámci jeho Azure umístění.  

### <a name="virtual-machines"></a>Virtuálních počítačích
Veřejnou IP adresu můžete přidružit s [Windows](../virtual-machines/virtual-machines-windows-about.md) nebo [Linux](../virtual-machines/virtual-machines-linux-about.md) OM přiřazením **rozhraní sítě**. V případě více síťová karta OM můžete přiřadit ho jenom *primární* rozhraní sítě. Můžete přiřadit dynamické nebo statické veřejnou IP adresu virtuálního počítače.

### <a name="internet-facing-load-balancers"></a>Vyrovnávání zatížení internetového
Veřejnou IP adresu můžete přidružit [Služby Vyrovnávání zatížení Azure](../load-balancer/load-balancer-overview.md)přiřazením konfiguraci **frontend** Vyrovnávání zatížení. Tento veřejnou IP adresu slouží jako Vyrovnávání zatížení virtuální IP adresu (VIP). Můžete přiřadit dynamické nebo statické veřejnou IP adresu Vyrovnávání zatížení front-end. Můžete taky přiřadíte několik veřejnou IP adres při vyrovnávání zatížení front-end, což umožňuje scénáře [s víc VIP](../load-balancer/load-balancer-multivip.md) jako více klienta prostředí se systémem SSL weby.

### <a name="vpn-gateways"></a>Virtuální privátní sítě bran
[Brána VPN Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md) se používá pro připojení Azure virtuální sítě (VNet) další VNets Azure nebo místní síti. Potřebujete přiřadit veřejnou IP adresu **IP konfigurace** umožňující komunikovat s vzdálené síti. V současné době můžete jenom přiřadíte *dynamické* veřejnou IP adresu bránu VPN.

### <a name="application-gateways"></a>Aplikace brány
Veřejnou IP adresu můžete přidružit Azure [Aplikace brány](../application-gateway/application-gateway-introduction.md), přiřazením konfiguraci **frontend** brány. Tento veřejnou IP adresu slouží jako VIP Vyrovnávání zatížení. V současné době můžete jenom přiřadíte *dynamické* veřejnou IP adresu ke konfiguraci aplikace brány frontend.

### <a name="at-a-glance"></a>U – přehled
Následující tabulka ukazuje specifické vlastnosti, přes který může být veřejnou IP adresu přidruženou k prostředku nejvyšší úrovně a metody možného přidělení (dynamické nebo statické), které lze použít.

|Nejvyšší úrovně zdroje|Přidružení IP adresa|Dynamické|Statická|
|---|---|---|---|
|Virtuální počítač|Rozhraní sítě|Ano|Ano|
|Vyrovnávání zatížení|Konfigurace front-end|Ano|Ano|
|Brána VPN|Konfigurace IP brány|Ano|Ne|
|Aplikace brány|Konfigurace front-end|Ano|Ne|

## <a name="private-ip-addresses"></a>Soukromé IP adres
Soukromé IP adresy povolit Azure zdrojům komunikovat s dalších zdrojů v [virtuální síť](virtual-networks-overview.md) nebo místní síti prostřednictvím sítě VPN brány nebo ExpressRoute okruh bez používání Internet dostupný IP adresu.

Správce prostředků Azure nasazení modelu je přidružená k těmto typům Azure zdrojů soukromé IP adresy:

- VMs
- Vyrovnávání zatížení interní (ILBs)
- Aplikace brány

### <a name="allocation-method"></a>Metoda přidělení
Privátní IP adresu přidělené z adresy rozsahu podsítě, ke kterému je připojen zdroje. Rozsah adres podsítě samotné je součástí rozsah VNet adres.

Kdy soukromé IP adresu přidělit dvěma způsoby: *dynamické* nebo *statické*. Výchozí metoda rozdělení je *dynamické*, kde na IP adresu automaticky přidělit ze zdroje podsítě (pomocí DHCP). Tuto IP adresu můžete změnit po ukončení a začněte zdroje.

Metoda přidělování můžete nastavit na *statický* zajistit že IP adresu se nezmění. V takovém případě budete potřebovat k poskytování platnou IP adresu, která je součástí podsítě daného zdroje.

Soukromé statické IP adresy se běžně používají pro:

- VMs, které fungují jako řadiče domény nebo serverů DNS.
- Prostředky, které vyžadují pravidla brány firewall pomocí IP adres.
- Materiály k nim získat přístup z jiných aplikací/prostředků až k IP adrese.

### <a name="virtual-machines"></a>Virtuálních počítačích
Privátní IP adresu přiřazen **síťové rozhraní** systému [Windows](../virtual-machines/virtual-machines-windows-about.md) nebo [Linux](../virtual-machines/virtual-machines-linux-about.md) OM. V případě více síťová karta OM dostane každý rozhraní soukromé IP adresy přiřazené. Metoda přidělování můžete zadat jako dynamické nebo statické rozhraní sítě.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Vnitřní hostname služeb překládá názvy DNS (pro VMs)
Všechny VMs Azure budou nakonfigurována [servery DNS Azure Správa přístupových práv](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) ve výchozím nastavení, pokud explicitně konfigurovat vlastní serverů DNS. Tyto servery DNS překladu název interního VMs, které se nacházejí v rámci stejné VNet.

Při vytváření virtuálního počítače mapování hostname na soukromé IP adresu se přidá na servery DNS Azure Správa přístupových práv. V případě rozhraní více sítě OM název hostitele namapované na soukromé IP adresu rozhraní primární sítě.

VMs nakonfigurována servery DNS Azure Správa přístupových práv budou moct překládal názvy hostitelů všechny VMs v rámci jejich VNet soukromých IP adres.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Vyrovnávání zatížení interní (ILB) a aplikace brány
Soukromé IP adresy můžete přiřadit **front-end** konfiguraci [Azure interní řešení pro vyrovnávání zatížení](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo [Azure aplikace brány](../application-gateway/application-gateway-introduction.md). Tento soukromé IP adresu slouží jako interní koncový bod, přístupných osobám s postižením pouze k prostředkům ve virtuálních síti (VNet) a vzdálených sítí připojený k VNet. Konfigurace front-end můžete přiřadit buď dynamické nebo statické IP adresu.

### <a name="at-a-glance"></a>U – přehled
Následující tabulka ukazuje určitou vlastnost, přes který může být privátní IP adresu přidružený k prostředku nejvyšší úrovně a metody možného přidělení (dynamické nebo statické), které lze použít.

|Nejvyšší úrovně zdroje|IP adresy přidružení|Dynamické|Statická|
|---|---|---|---|
|Virtuální počítač|Rozhraní sítě|Ano|Ano|
|Vyrovnávání zatížení|Konfigurace front-end|Ano|Ano|
|Aplikace brány|Konfigurace front-end|Ano|Ano|

## <a name="limits"></a>Omezení

Omezení, která na IP adres jsou uvedeny v úplné sady [omezuje pro připojení k síti](azure-subscription-service-limits.md#networking-limits) v Azure. Tyto limity jsou jednotlivých oblastech a jedno předplatné. Můžete [kontaktovat podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) zvětšíte výchozí omezení až maximální limity podle potřeb uživatele.

## <a name="pricing"></a>Ceny

Veřejné adresy IP pravděpodobně nominal nákladů. Další informace o IP adresu ceny v Azure, zkontrolujte stránku [ceny IP adres](https://azure.microsoft.com/pricing/details/ip-addresses) .

## <a name="next-steps"></a>Další kroky
- [Nasazení OM statické veřejnou IP pomocí portálu Azure](virtual-network-deploy-static-pip-arm-portal.md)
- [Nasazení OM pomocí statické IP veřejné použít šablonu?](virtual-network-deploy-static-pip-arm-template.md)
- [Nasazení virtuálního počítače s statické soukromé adresy IP pomocí portálu Azure](virtual-networks-static-private-ip-arm-pportal.md)
