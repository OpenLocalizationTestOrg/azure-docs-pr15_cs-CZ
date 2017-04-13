<properties
   pageTitle="Veřejných a privátních IP adres (klasický) v Azure | Microsoft Azure"
   description="Další informace o veřejných a privátních IP adres v Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP adresy (klasický) v Azure
IP adresy můžete přiřadit Azure materiály ke komunikaci s další Azure zdroje, v místní síti a k Internetu. Existují dva typy IP adresy můžete použít v Azure: veřejných a privátních.

IP adresy se používají pro komunikaci s Internetu, ať už Azure veřejné služby.

Soukromé IP adresy se používají pro komunikaci v rámci Azure virtuální sítě (VNet), Cloudová služba a v místní síti při použití Brána VPN nebo ExpressRoute okruh rozšířit sítě Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu nasazení Správce prostředků](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Veřejné IP adres
IP adresy povolit Azure materiály ke komunikaci s Internet a Azure veřejných webových služeb, jako je [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/), [Rozbočovače události Azure](https://azure.microsoft.com/services/event-hubs/), [SQL databáze](../sql-database/sql-database-technical-overview.md)a [Azure úložiště](../storage/storage-introduction.md).

Veřejnou IP adresu je spojena s následujícími typy zdrojů:

- Cloud services
- IaaS virtuálních počítačích (VMs)
- Role instance PaaS
- Virtuální privátní sítě brány
- Aplikace brány

### <a name="allocation-method"></a>Metoda přidělení
Potřebujete-li přiřadit Azure zdroje veřejnou IP adresu, je *dynamicky* přidělit z fondu dostupné veřejnou IP adresu do umístění, do kterého je vytvořena zdroje. Tuto IP adresu vydání při ukončení práce zdroje. V případě, že z cloudové služby, k tomu dojde při zastavení všechny instance role, které můžete zabránit pomocí *statické* (rezervovaná) IP adresy (viz [Cloudovým službám](#Cloud-services)).

>[AZURE.NOTE] Seznam rozsahů IP ze kterých jsou veřejné adresy IP přidělit Azure zdroje je publikován na [rozsahy IP Azure Datacentra](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Hostname služeb překládá názvy DNS
Když vytvoříte do cloudové služby nebo IaaS OM, budete muset zadat název DNS služby cloudu, který je v celé všech zdrojů v Azure jedinečná. Vytvoří mapování v serverů DNS Azure Správa přístupových práv pro *název_dns*. cloudapp.net na veřejnou IP adresu zdroje. Například při vytváření do cloudové služby s názvem DNS služby cloudu společnosti **contoso**úplný název (FQDN) **contoso.cloudapp.net** se překládal veřejnou IP adresu (VIP) služby cloudu. Tento plně kvalifikovaný název domény můžete použít k vytvoření záznamu CNAME vlastní doménu přejdete na veřejnou IP adresu v Azure.

### <a name="cloud-services"></a>Cloud services
Cloudová služba má vždy veřejnou IP adresu označovaný taky jako virtuální IP adresu (VIP). Vytvoření koncové body v cloudové službě přiřadit různé porty v VIP k interním porty VMs a instancí role v cloudovou službu. 

Cloudová služba může obsahovat více IaaS VMs nebo PaaS role případech všechny vystaveného prostřednictvím stejné VIP služby cloudu. Můžete také přiřadit [více virtuální do cloudové služby](../load-balancer/load-balancer-multivip.md), což umožňuje scénáře s víc VIP jako více klienta prostředí se systémem SSL weby.

Zajistíte tím že veřejnou IP adresu do cloudové služby zůstane stejná, i když jsou všechny instance role zastavit, pomocí *statické* veřejnou IP adresu, označovaný taky jako [User](virtual-networks-reserved-public-ip.md). Můžete vytvořit prostředek statické IP (rezervovaná) v určitém umístění a přiřadit ji někomu jiné cloudové služby v uvedeném umístění. Nelze zadat skutečné IP adresu pro rezervovaná IP, přidělit z fondu dostupných IP adres v umístění, do kterého je vytvořena. Tuto IP adresu neuvolní, dokud neodstraníte explicitně.

Statické IP adresy veřejného (rezervovaná) se obvykle používají v situacích, kde do cloudové služby:

- vyžaduje pravidla brány firewall je nastavení koncových uživatelů.
- závisí na externích název překládá názvy DNS, a dynamické IP vyžadují aktualizaci záznam.
- spotřebovává externí webové služby, které používají model zabezpečení IP založena.
- používá certifikáty SSL propojené k IP adrese.

>[AZURE.NOTE] Při vytváření klasické OM kontejneru *cloudové služby* vytvořil Azure, který má virtuální IP adresu (VIP). Po dokončení vytvoření portálu výchozí RDP nebo SSH *koncový bod* nakonfigurovaný tak, že na portálu tak, aby se můžete připojit k OM prostřednictvím virtuální služby cloudu. Tento virtuální služby cloudu můžete vyhrazeno poskytující efektivně rezervovaná IP adresu připojení bude v angličtině. Otevření další porty nakonfigurováním další koncové body.

### <a name="iaas-vms-and-paas-role-instances"></a>Role instance IaaS VMs a PaaS
Můžete přiřadit veřejnou IP adresu přímo IaaS [OM](../virtual-machines/virtual-machines-linux-about.md) instanci rolí PaaS do cloudové služby. Toto je uvedené jako úrovni instance veřejnou IP adresu ([ILPIP](virtual-networks-instance-level-public-ip.md)). Tento veřejnou IP adresu lze pouze dynamické.

>[AZURE.NOTE] Tím se liší od VIP cloudové služby, což je kontejner IaaS VMs nebo PaaS role instance, protože do cloudové služby může obsahovat více VMs IaaS nebo případech role PaaS všechny vystaven prostřednictvím stejné VIP služby cloudu.

### <a name="vpn-gateways"></a>Virtuální privátní sítě brány
[Brána VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) mohou sloužit k připojení Azure VNet k jiným Azure VNets nebo místní sítě. Brána VPN přiřazena veřejné IP adresu *dynamicky*, což umožňuje komunikace s vzdálené síti.

### <a name="application-gateways"></a>Aplikace brány
Azure [aplikace brány](../application-gateway/application-gateway-introduction.md) se dá použít pro Layer7 Vyrovnávání zatížení chcete směrovat přenosy v síti na HTTP. Aplikace brány se přiřadí veřejnou IP adresu *dynamicky*sloužící jako VIP Vyrovnávání zatížení.

### <a name="at-a-glance"></a>Rychlý přehled
Následující tabulka ukazuje každý typ zdroje s možné metod (dynamické/statického) a přiřadit několik veřejnou IP adres.

|Zdroje|Dynamické|Statická|Více IP adres|
|---|---|---|---|
|Cloudová služba|Ano|Ano|Ano|
|IaaS OM nebo PaaS instanci rolí|Ano|Ne|Ne|
|Brána VPN|Ano|Ne|Ne|
|Aplikace brány|Ano|Ne|Ne|

## <a name="private-ip-addresses"></a>Soukromé IP adres
Soukromé IP adresy povolit Azure zdrojům komunikovat s dalších zdrojů v do cloudové služby nebo [virtuální sítě](virtual-networks-overview.md)(VNet) a v místní síti (přes VPN brány nebo ExpressRoute okruh), bez používání Internet dostupný IP adresu.

V Azure klasické nasazení modelu soukromé IP adresy můžete přidělovat následující Azure zdroje:

- Role instancí IaaS VMs a PaaS
- Vyrovnávání zatížení interní
- Aplikace brány

### <a name="iaas-vms-and-paas-role-instances"></a>Role instance IaaS VMs a PaaS
Virtuálních počítačích (VMs) vytvořená pomocí klasické nasazení modelu jsou vždy umístěny do cloudové služby, podobně jako PaaS role instance. Chování soukromé IP adresy se tedy podobají pro tyto materiály.

Je důležité mít na paměti, že do cloudové služby může být nasazené dvěma způsoby:

- Jako *samostatný* cloudové služby, kde se v rámci virtuální sítě.
- V rámci virtuální sítě.

#### <a name="allocation-method"></a>Metoda přidělení
V případě do *samostatného* cloudové služby zdroje soukromé IP adresu přidělit *dynamicky* dostat z rozsah Azure datacentra soukromé IP adres. Lze použít pouze pro komunikaci s jiných VMs v rámci stejné cloudové služby. Tuto IP adresu lze změnit zdroj je zastaveno a začít.

V případě cloudové služby používaný v rámci virtuální sítě potřebujete zdroje soukromé IP adresy přidělit v oblasti adresy přidružené subnet(s) (jak je uvedeno v konfiguraci sítě). Tento soukromé IP adresy se dá použít pro komunikaci mezi všechny VMs v VNet.

Navíc pro nečekané cloudovým službám v rámci VNet soukromé IP adresu přiřazen *dynamicky* (pomocí DHCP) ve výchozím nastavení. Můžete změnit při zastaven a začít zdroje. Abyste měli jistotu, že IP adresa zůstane stejná, musíte nastavit metodu přidělení *statické*a zadejte platnou IP adresu odpovídající rozsahu adres.

Soukromé statické IP adresy se běžně používají pro:

 - VMs, které fungují jako řadiče domény nebo serverů DNS.
 - VMs, které vyžadují pravidla brány firewall pomocí IP adres.
 - VMs službami používána v jiných aplikací až k IP adrese.

#### <a name="internal-dns-hostname-resolution"></a>Vnitřní hostname překládá názvy DNS
Všechny instance role PaaS a Azure VMs budou nakonfigurována [servery DNS Azure Správa přístupových práv](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) ve výchozím nastavení, pokud explicitně konfigurovat vlastní serverů DNS. Tyto servery DNS překladu název interního VMs a instance role, které se nacházejí v rámci stejné VNet nebo cloudové služby.

Při vytváření virtuálního počítače mapování hostname na soukromé IP adresu se přidá na servery DNS Azure Správa přístupových práv. V případě více NIC OM název hostitele namapovala soukromé IP adresu primární NIC. Tyto informace o mapování je ale omezený k prostředkům ve stejném cloudové služby, nebo VNet.

V případě do *samostatného* cloudové služby budou moct vyřešit názvy hostitelů instancí všechny VMs/role ve stejném cloudovou službu pouze. Když v rámci VNet do cloudové služby budou moct vyřešit názvy hostitelů všech instancí VMs/role v VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Vyrovnávání zatížení interní (ILB) a aplikace brány
Soukromé IP adresy můžete přiřadit **front-end** konfiguraci [Azure interní řešení pro vyrovnávání zatížení](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo [Azure aplikace brány](../application-gateway/application-gateway-introduction.md). Tento soukromé IP adresu slouží jako interní koncový bod, přístupných osobám s postižením pouze k prostředkům ve virtuálních síti (VNet) a vzdálených sítí připojený k VNet. Konfigurace front-end můžete přiřadit buď dynamické nebo statické IP adresu. Můžete také přiřadit více soukromé IP adresy povolit více vip scénáře.

### <a name="at-a-glance"></a>Rychlý přehled
Následující tabulka ukazuje každý typ zdroje s možné metod (dynamické/statického) a přiřadit více soukromých IP adres.

|Zdroje|Dynamické|Statická|Více IP adres|
|---|---|---|---|
|OM (v *samostatné* Cloudová služba)|Ano|Ano|Ano|
|Instanci rolí PaaS (v *samostatné* Cloudová služba)|Ano|Ne|Ano|
|OM nebo PaaS role instanci (VNet)|Ano|Ano|Ano|
|Vyrovnávání zatížení vnitřní front-end|Ano|Ano|Ano|
|Aplikace brány front-end|Ano|Ano|Ano|

## <a name="limits"></a>Omezení

Následující tabulka ukazuje omezení, která na IP adres v Azure jedno předplatné. Můžete [kontaktovat podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) zvětšíte výchozí omezení až maximální limity podle potřeb uživatele.

||Výchozí limit|Maximální limit|
|---|---|---|
|Veřejné IP adresy (dynamické)|5|kontaktování podpory|
|Rezervované veřejné adresy IP|20|kontaktování podpory|
|Veřejné VIP za nasazení (cloudové služby)|5|kontaktování podpory|
|Soukromé VIP (ILB) na nasazení (cloudové služby)|1|1|

Zkontrolujte, že si přečíst celou sadu [omezuje pro připojení k síti](azure-subscription-service-limits.md#networking-limits) v Azure.

## <a name="pricing"></a>Ceny

Ve většině případů jsou zadarmo veřejnou IP adres. Existuje nominal poplatku za použití veřejných další a/nebo statické IP adres. Zkontrolujte, jestli že víte, [ceny strukturu pro veřejnou IP adresy](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Rozdíly mezi správce zdrojů a klasické nasazení
Níže je srovnání IP adresách funkcí Správce zdrojů a modelu klasické nasazení.

||Zdroje|Klasický|Správce prostředků|
|---|---|---|---|
|**Veřejnou IP adresu**|OM|Jen ILPIP (pouze dynamické)|Jen veřejnou IP (dynamického nebo statického)|
|||Přiřazená OM IaaS nebo instanci PaaS rolí|Související s NIC OM|
||Internet vystaveného Vyrovnávání zatížení|Jen VIP (dynamické) nebo User (statická)|Jen veřejnou IP (dynamického nebo statického)|
|||Přiřazené do cloudové služby|Související s front-end konfigurace služby Vyrovnávání zatížení|
||||
|**Soukromé IP adresa**|OM|Jen DIP|Označovaný taky jako soukromé IP adresa|
|||Přiřazená OM IaaS nebo instanci PaaS rolí|Přiřazená NIC OM|
||Vyrovnávání zatížení interní (ILB)|Přiřazené k ILB (dynamického nebo statického)|Přiřazená ILB front-end konfigurace (dynamické nebo statické)|

## <a name="next-steps"></a>Další kroky
- [Nasazení OM pomocí statické IP adresu soukromé](virtual-networks-static-private-ip-classic-pportal.md) na klasické portálu.
