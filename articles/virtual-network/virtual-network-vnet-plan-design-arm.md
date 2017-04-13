<properties
   pageTitle="Plán Azure virtuální sítě (VNet) a návrh průvodce | Microsoft Azure"
   description="Informace o plánování a návrh virtuální sítě Azure svým požadavkům izolace, připojení a umístění."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Plánování a návrh virtuálních sítí Azure

Vytváření VNet experiment s je dostatečně jednoduché, ale je pravděpodobné, budete nasazovat více VNets postupně na podporu výrobní potřeb své organizace. S některými plánování a návrh budou moct nasazení VNets a připojte prostředky, které budete potřebovat efektivněji. Pokud znáte není VNets, doporučujeme, která se [informace o VNets](virtual-networks-overview.md) a [nasazení](virtual-networks-create-vnet-arm-pportal.md) jednu pokračovat. 

## <a name="plan"></a>Plánování

Důkladně Principy Azure předplatná, oblasti a síťových prostředků, je naprosto zásadní praktické. Seznam co byste měli zvážit pod můžete použít jako výchozí bod. Po těchto Principy můžete definovat požadavky pro návrh sítě.

### <a name="considerations"></a>Co byste měli zvážit

Před odpovídání plánování otázky pod, zvažte následující skutečnosti:

- Všechno, co, které vytvoříte v Azure se skládá z jednoho nebo více zdrojů. Virtuálního počítače (OM) je zdroj, v síti adaptér rozhraní (NIC) používané virtuálního počítače je zdrojem, veřejnou IP adresu používá NIC je zdrojem je VNet NIC je připojený k zdroje.
- Vytvoření zdrojů v rámci předplatného a [Azure oblast](https://azure.microsoft.com/regions/#services) . A zdroje můžete jenom připojený k VNet rozlohu ve stejném oblast a předplatné, které se nacházejí. 
- VNets můžete připojit k sobě pomocí Azure [VPN brány](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). Můžete taky připojit VNets různých oblastí a předplatná tímto způsobem.
- VNets můžete připojit k místní síti pomocí jedné z [možností připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) k dispozici v Azure. 
- Jiné zdroje můžete seskupené do [skupiny zdrojů](../azure-resource-manager/resource-group-overview.md#resource-groups)a snadněji spravovat zdroje jako celek. Skupina zdroje mohou obsahovat zdroje z více oblastí, jakož zdroje patří do stejného předplatného.

### <a name="define-requirements"></a>Definování požadavky

Otázky pod použijte jako výchozí bod pro návrh Azure sítě.  

1. Umístění, ve kterých Azure použijete na hostitele VNets?
2. Potřebujete pro komunikaci mezi těmto místům Azure?
3. Potřebujete pro komunikaci mezi Azure VNet(s) a místních datacenter(s)?
4. Kolik infrastruktury služby (IaaS) VMs cloud services rolí a webové aplikace, které potřebujete pro řešení?
5. Je třeba izolovat přenosy podle skupin VMs (webových serverů tedy front-end a back-end databáze serverem)?
6. Potřebujete řídit tok pomocí virtuální zařízení?
7. Potřebují uživatelé různé sady oprávnění k prostředkům Azure?

### <a name="understand-vnet-and-subnet-properties"></a>Princip VNet a podsítě vlastnosti

Definování hranici zabezpečení pro pracovní vytížení v Azure vám pomoct VNet a podsítí materiály. VNet rozdělení kolekcí adresu mezery, rozumí CIDR bloky. 

>[AZURE.NOTE] Správci sítě znají CIDR zápisu. Pokud znáte není CIDR, [Přečtěte si další informace o ho](http://whatismyipaddress.com/cidr).

VNets obsahují následující vlastnosti.

|Vlastnost|Popis|Omezení|
|---|---|---|
|**Jméno**|Název VNet|Řetězec až 80 znaků. Může obsahovat písmena, čísla, podtržení, období nebo spojovníky. Musí začínat písmenem nebo číslo. Musí končit písmeno, číslo nebo podtržítka. Můžete obsahuje velká a malá písmena.|  
|**umístění**|Azure umístění (označuje také jako oblast).|Musí být platná Azure umístění.|
|**addressSpace**|Kolekce předpony adres, které tvoří VNet v CIDR zápisu.|Musí být maticových platné blok adresy CIDR včetně veřejné rozsahy IP adres.|
|**podsítí**|Soubor podsítí tvořící VNet|viz následující tabulka vlastnosti podsítě.||
|**dhcpOptions**|Objekt, který obsahuje jednu vlastnost je nutno zadat název **dnsServers**.||
|**dnsServers**|Pole servery DNS používané VNet. Pokud není zadán žádný server, použije se Azure interní překlad.|Musí být matice až 10 serverů DNS IP adresy.| 

Podsítě je zdroj podřízené VNet a pomáhá definovat segmentů adresního prostoru v rámci bloku CIDR pomocí předpony IP adres. Nic můžete přidat podsítí a připojení k VMs poskytování připojení pro různé úloh.

Podsítí obsahují následující vlastnosti. 

|Vlastnost|Popis|Omezení|
|---|---|---|
|**Jméno**|Podsítě název|Řetězec až 80 znaků. Může obsahovat písmena, čísla, podtržení, období nebo spojovníky. Musí začínat písmenem nebo číslo. Musí končit písmeno, číslo nebo podtržítka. Můžete obsahuje velká a malá písmena.|
|**umístění**|Azure umístění (označuje také jako oblast).|Musí být platná Azure umístění.|
|**addressPrefix**|Předponu jedné adresy, které tvoří podsítě v CIDR zápisu|Musí být jeden blok CIDR, která je součástí jednu VNet adresního prostoru.|
|**networkSecurityGroup**|NSG použité u podsítě|v tématu [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Směrování tabulky použité u podsítě|v tématu [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Kolekce objektů konfigurace IP používaný nic připojené k podsítě|v tématu [Konfigurace IP](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Překlad

Ve výchozím nastavení používá vaše VNet [Azure-za předpokladu, že překlad.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) Kontrola jmen uvnitř VNet a veřejné sítě Internet. Ale pokud se vaše VNets připojit k datacentrech vaší místní, musíte zadat [serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) jmen mezi vašich sítí.  

### <a name="limits"></a>Omezení

Prohlédněte si sítě omezení v článku [omezení Azure](../azure-subscription-service-limits.md#networking-limits) zajistit, že návrhu není v konfliktu se změnami některou z výše. Určité datové limity, lze zvětšit tak, že otevřete požadavek podpory můžete.

### <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě rolí (RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) umožňuje určit úroveň přístupu, kterou různí uživatelé mohou mít k prostředkům ve Azure. Tímto způsobem můžete oddělit práci provedenou váš tým podle svých potřeb. 

Jde o virtuální sítě, mají uživatelé v role **Přispěvatele sítě** úplnou kontrolu nad správce prostředků Azure virtuální sítě zdroje. Podobně uživatelů v role **Přispěvatele klasické sítě** mít oprávnění k úplnému řízení zdrojů klasické virtuální sítě.

>[AZURE.NOTE] Můžete taky [vytvořit vlastní role](../active-directory/role-based-access-control-configure.md) k oddělení potřeb pro správu.

## <a name="design"></a>Návrh

Když víte, odpovědi na otázky v části [plánování](#Plan) , zkontrolujte následující před definování vaší VNets.

### <a name="number-of-subscriptions-and-vnets"></a>Počet předplatná a VNets

Zvažte vytvoření více VNets v následujících situacích:

- **VMs, které potřebujete umístěná na různých místech Azure**. VNets v Azure jsou místní. Nelze pokrýval umístění. Proto potřebujete alespoň jeden VNet pro každé Azure, který chcete VMs Host (hostitel) v umístění.
- **Úloh, které musí být úplně izolovaný od sebe**. Můžete vytvořit samostatné VNets, dokonce i využívající stejné mezery, izolovat různých úloh od sebe. 

Mějte na paměti, že jsou limity, které se zobrazí nad jednotlivých oblastech jedno předplatné. To znamená, že používáte víc předplatných zvýšení limitu prostředky, které můžete spravovat v Azure. Připojení VNets v různých předplatných, můžete pomocí sítě VPN na webu nebo ExpressRoute obvodu.

### <a name="subscription-and-vnet-design-patterns"></a>Předplatné a VNet provedeních

Následující tabulka ukazuje některé běžné provedeních pro používání předplatného a VNets.

|Scénář|Diagram|V oblasti IT|Nevýhody|
|---|---|---|---|
|Jeden předplatného dvěma VNets za aplikace|![Jeden předplatného](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Jenom jedno předplatné pro správu.|Maximální počet VNets za Azure oblast. Až to potřebujete další předplatná. Přečtěte si článek [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) podrobnosti.|
|Jedno předplatné na aplikaci, dvě VNets za aplikace|![Jeden předplatného](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Použije jen dvěma VNets jedno předplatné.|Složitější ke správě případě, že máte příliš mnoho aplikace.|
|Jedno předplatné na organizační jednotku, dvě VNets za aplikace.|![Jeden předplatného](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Zůstatek mezi počet předplatná a VNets.|Maximální počet VNets za organizační jednotce (předplatné). Přečtěte si článek [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) podrobnosti.|
|Jedno předplatné na organizační jednotku, dvě VNets skupinu aplikací.|![Jeden předplatného](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Zůstatek mezi počet předplatná a VNets.|Aplikace, musí být samostatný pomocí podsítí a NSGs.|


### <a name="number-of-subnets"></a>Počet podsítí

Měli byste zvážit více podsítí v VNet v následujících situacích:

- **Není dost soukromé IP adres pro všechny nic v podsítě**. Pokud vaší podsítě adresní prostor neobsahuje dostatečný počet IP adres pro číslem nic v podsítě, je potřeba vytvořit více podsítí. Nezapomeňte, že Azure rezervy 5 soukromé IP adres z každé podsítě, které nemohou být použity: první a poslední adres adresní prostor (pro adresu podsítě a multicast) a 3 adresy tak, aby se dá použít interně (pro účely DHCP a DNS). 
- **Zabezpečení**. Pomocí podsítí oddělit skupiny VMs od sebe pro úloh, které mají strukturu více vrstev a použití různých [skupin zabezpečení síti (NSGs)](virtual-networks-nsg.md#subnets) pro tyto podsítě.
- **Hybridní připojení**. Můžete VPN brány a obvody ExpressRoute pro [připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) vaší VNets vzájemně a center(s) vaší místní data. VPN brány a ExpressRoute obvody vyžadují podsítě vlastní nevytvoří.
- **Virtuální zařízení**. Virtuální spotřebiče, například bránu firewall, sítě WAN přístupové nebo brána VPN můžete použít v Azure VNet. Po výběru je to potřeba [směrovat přenosy](virtual-networks-udr-overview.md) na těchto zařízeních a izolovat ve své vlastní podsítě.

### <a name="subnet-and-nsg-design-patterns"></a>Podsítí a NSG provedeních

Následující tabulka ukazuje některé běžné provedeních pro používání podsítí.

|Scénář|Diagram|V oblasti IT|Nevýhody|
|---|---|---|---|
|Jeden podsítě NSGs na vrstvu aplikace za aplikace|![Jeden podsítě](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Jedinou podsítě ke správě.|Více NSGs nutné izolovat jednotlivých aplikací.|
|Jeden podsítě za aplikace, NSGs na vrstvu aplikace|![Podsítě za aplikace](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Méně NSGs ke správě.|Více podsítí ke správě.|
|Jeden podsítě za aplikace vrstvy, NSGs na aplikace.|![Podsítě za vrstvy](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Zůstatek mezi čísly podsítí a NSGs.|Maximální počet NSGs jedno předplatné. Přečtěte si článek [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) podrobnosti.|
|Jeden podsítě na vrstvu aplikace na aplikaci, NSGs za podsítě|![Podsítě na vrstvu za aplikace](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Případně menší počet NSGs.|Více podsítí ke správě.|

## <a name="sample-design"></a>Ukázka návrhů

Ke znázornění použití informace v tomto článku, zvažte následující scénář.

Pracujete společnost, která se má 2 datacentrech v Severní Americe a dvě datacentrech Europe. Jste určili 6 vystaveného aplikací jinému zákazníkovi vedené 2 různých organizačních jednotek, které chcete migrovat do Azure jako pilotního nasazení. Základní architektura aplikace je takto:

- Apl1, Apl2, App3 a App4 jsou hostované na Linux serverech musí běžet se systémem Ubuntu webových aplikací. Každá aplikace spojí se serverem samostatné aplikace, která hostuje RESTful služby na serverech Linux. RESTful služby připojení k databázi MySQL back-end.
- App5 a App6 jsou webové aplikace hostující na serverech Windows systémem Windows Server 2012 R2. Každá aplikace se připojí k back-end databáze SQL serveru.
- Všechny aplikace jsou právě hostovaný v jednom z firmy datacentrech v Severní Americe.
- Datacentrech místní pomocí 10.0.0.0/8 adresní prostor.

Potřebujete navržení řešení virtuální sítě, který splňuje následující požadavky:

- Každý organizační jednotka neměli mít vliv využití prostředků jiných organizačních jednotek.
- Jste měli minimalizovat VNets a podsítí snadnější správu.
- Každý organizační jednotka by měla být jediné test/vývojové VNet použít pro všechny aplikace.
- Každá aplikace je hostovaný ve 2 různých Azure datacentrech za kontinent (Severní Americe a Evropa).
- Každá aplikace je úplně izolovaný od sebe.
- Každá aplikace přístupné zákazníci přes Internet pomocí protokolu HTTP.
- Každá aplikace přístupné uživatelům připojení k místní datacentrech pomocí šifrované tunelem.
- Připojení k místní datacentrech používejte existující VPN zařízení.
- Firemní síti skupiny měli úplnou kontrolu nad nastavením VNet.
- Vývojáři v každé organizační jednotce pouze by měla nasadit VMs existující podsítí.
- Všechny aplikace se přesunou jsou Azure (výtah a shift).
- Databáze v každém umístění by měly replikovat do jiných umístění Azure jednou za den.
- Každá aplikace používejte 5 front-end webových serverů, 2 aplikace servery (v případě potřeby) a 2 servery databáze.

### <a name="plan"></a>Plánování

By měly začít návrhu plánování odpovědí na otázky v části [definovat požadavky](#Define-requirements) jak je ukázáno v následujícím příkladu.

1. Umístění, ve kterých Azure použijete na hostitele VNets?

    2 umístění v Severní Americe a 2 umístění v Evropě. Je vhodné vybírat operace založené na fyzické umístění vaší stávající datacentrech místní. Tímto způsobem připojení z vaší fyzické umístění Azure bude mít lepší latence.

2. Potřebujete pro komunikaci mezi těmto místům Azure?

    Ano. Vzhledem k tomu, že databáze je třeba replikovat pro všechna umístění.

3. Potřebujete pro komunikaci mezi Azure VNet(s) a vaší místní data center(s)?

    Ano. Od uživatelům připojení k místní datacentrech musíte mít přístup k aplikace šifrované tunelem.
 
4. Kolik VMs IaaS musíte řešení pro?

    200 IaaS VMs. Apl1, Apl2 a App3 vyžadují 5 webových serverů každý, 2 aplikací servery každý a 2 servery databáze. To je celkem 9 VMs IaaS jedné aplikace nebo 36 IaaS VMs. App5 a App6 vyžadují 5 webových serverů a 2 servery databáze. To je celkem 7 VMs IaaS jedné aplikace nebo 14 IaaS VMs. Proto musíte 50 IaaS VMs pro všechny aplikace v jednotlivých oblastech Azure. Když budeme potřebovat 4 oblastí, bude 200 IaaS VMs.

    Bude taky muset poskytují servery DNS v každé VNet nebo ve vaší místní datacentrech řešení název mezi vaší VMs IaaS Azure a v místní síti. 

5. Je třeba izolovat přenosy podle skupin VMs (webových serverů tedy front-end a back-end databáze serverem)?

    Ano. Každá aplikace by měl být úplně izolovaný od sebe a jednotlivé vrstvy aplikace by měly být také izolace. 

6. Potřebujete řídit tok pomocí virtuální zařízení?

    Ne. Virtuální zařízení mohou sloužit k poskytuje větší kontrolu nad přenos, včetně podrobnější data rovině protokolování. 

7. Potřebují uživatelé různé sady oprávnění k prostředkům Azure?

    Ano. Sociální sítě tým úplné řízení na virtuální nastavení sítě během vývojáři pouze měli nasadit jejich VMs do stávajících podsítí. 

### <a name="design"></a>Návrh

Postupujte podle designu určující předplatná, VNets, podsítí a NSGs. Bude probereme tato NSGs tady, ale měli další informace o [NSGs](virtual-networks-nsg.md) před dokončením návrhu.

**Počet předplatná a VNets**

Následující požadavky se vztahují předplatná a VNets:

- Každý organizační jednotka neměli mít vliv využití prostředků jiných organizačních jednotek.
- Jste měli minimalizovat VNets a podsítí.
- Každý organizační jednotka by měla být jediné test/vývojové VNet použít pro všechny aplikace.
- Každá aplikace je hostovaný ve 2 různých Azure datacentrech za kontinent (Severní Americe a Evropa).

Založené na tyto požadavky splnit, musíte předplatné pro každou organizační jednotku. Tímto způsobem spotřeba zdrojů z organizační jednotku nebude mezi limity nepočítá pro ostatní organizačních jednotek. A od chcete minimalizovat počet VNets, měli byste zvážit použití vzorku **jedno předplatné na organizační jednotku, dvě VNets skupinu aplikací** jak je vidět níže.

![Jeden předplatného](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Budete potřebovat k určení adresní prostor pro každého VNet. Vzhledem k tomu potřebujete propojení mezi místních dat centra oblasti Azure adresní prostor pro Azure VNets nelze kolidovat s místní síti a adresní prostor používaný každý VNet neměli kolidovat s jiné existující VNets. Splňovat tyto požadavky můžete použít adresu mezery v následující tabulce.  

|**Předplatné**|**VNet**|**Azure oblast**|**Adresní prostor**|
|---|---|---|---|
|BU1|ProdBU1US1|Západ USA|172.16.0.0/16|
|BU1|ProdBU1US2|Východní USA|172.17.0.0/16|
|BU1|ProdBU1EU1|Severní Evropě|172.18.0.0/16|
|BU1|ProdBU1EU2|Západní Evropě|172.19.0.0/16|
|BU1|TestDevBU1|Západ USA|172.20.0.0/16|
|BU2|TestDevBU2|Západ USA|172.21.0.0/16|
|BU2|ProdBU2US1|Západ USA|172.22.0.0/16|
|BU2|ProdBU2US2|Východní USA|172.23.0.0/16|
|BU2|ProdBU2EU1|Severní Evropě|172.24.0.0/16|
|BU2|ProdBU2EU2|Západní Evropě|172.25.0.0/16|

**Počet podsítí a NSGs**

Následující požadavky se vztahují podsítí a NSGs:

- Jste měli minimalizovat VNets a podsítí.
- Každá aplikace je úplně izolovaný od sebe.
- Každá aplikace přístupné zákazníci přes Internet pomocí protokolu HTTP.
- Každá aplikace přístupné uživatelům připojení k místní datacentrech pomocí šifrované tunelem.
- Připojení k místní datacentrech používejte existující VPN zařízení.
- Databáze v každém umístění by měly replikovat do jiných umístění Azure jednou za den.

Založená na tyto požadavky splnit, můžete použít jednu podsítě na vrstvu aplikace a pomocí NSGs přenosy filtr na aplikace. Tak, stačí 3 podsítí v jednotlivých VNet (front-end aplikace vrstvy a data layer) a jeden NSG jedné aplikace za podsítě. V tomto případě zvažte použití návrhového vzoru **jeden podsítě za aplikace vrstvy, NSGs za aplikace** . Následující obrázek znázorňuje použití vzorku návrh představující **ProdBU1US1** VNet.

![Jeden podsítě na vrstvu, jeden NSG na aplikaci na vrstvu](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Však je taky potřeba vytvořit navíc podsítě pro připojení k síti VPN mezi VNets a datacentrech vaší místní. A budete muset zadat adresní prostor pro každého podsítě. Následující obrázek znázorňuje ukázkové řešení pro **ProdBU1US1** VNet. Tento scénář by replikovat pro každou VNet. Každá barva představuje do jiné aplikace.

![Ukázka VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Řízení přístupu**

Následující požadavky se vztahují k řízení přístupu:

- Firemní síti skupiny měli úplnou kontrolu nad nastavením VNet.
- Vývojáři v každé organizační jednotce pouze by měla nasadit VMs existující podsítí.

Podle tyto požadavky splnit, můžete přidat uživatele od týmu služeb sítě předdefinované role **Přispěvatele sítě** do každého předplatného; a vytvořte vlastní role pro vývojáře aplikace do každého předplatného jim práva k přidání VMs do existující podsítí.

## <a name="next-steps"></a>Další kroky

- [Nasazení virtuální síť](virtual-networks-create-vnet-arm-template-click.md) podle situace.
- Princip [Vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) VMs IaaS a [správě směrování přes více Azure oblastí](../traffic-manager/traffic-manager-overview.md).
- Další informace o [NSGs a jak se plánování a návrh](virtual-networks-nsg.md) NSG řešení.
- Další informace o vaší [místní křížově a VNet konektivity](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
