<properties
   pageTitle="Implementace zabezpečeného hybridní architektura sítě v Azure | Microsoft Azure"
   description="Jak implementovat architektura sítě zabezpečené hybridní v Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
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

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Provádění DMZ mezi Azure a místních datacentru

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro implementace zabezpečeného hybridní sítě, která se v místní síti Azure. Tento odkaz architektura používá DMZ mezi místní síti a Azure virtuální sítě pomocí směruje definované uživatelem (UDRs). DMZ obsahuje vysoce dostupné síťové virtuální spotřebiče (NVAs) implementující funkcích zabezpečení například bránách firewall a inspekci paketů. Všechny odchozí přenosy dat z VNet je vytvořena platnost k Internetu prostřednictvím místní síti, ke které je jde auditovat. 

Tato architektura vyžaduje připojení k vaší místní datacentru implementovaná pomocí [Brána VPN][ra-vpn], nebo [ExpressRoute] [ ra-expressroute] připojení.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení.

Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace kde úloh spustit částečně místní a částečně v Azure.

- Azure infrastruktury, který směruje příchozích z místním prostředím a na Internetu.

- Povinné mají být auditovány odchozí přenosy dat aplikace. To je často zákonné požadavky komerční systémů a může pomoci zabránit zveřejnění důvěrné informace.

## <a name="architecture-diagram"></a>Diagram architektury

V následujícím diagramu jsou uvedeny důležitou součástí tato architektura:

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je na stránce "DMZ – soukromé".

[! [0]][0]

- **V místní síti.** Toto je síť počítačů a zařízení připojení přes osobní místní síti implementovaná v organizaci.

- **Azure virtuální sítě (VNet).** VNet hostuje aplikace a dalších zdrojů spuštěné v cloudu.

- **Brány.** Brány umožňuje připojení mezi směrovači v místní síti a VNet.

- **Síť virtuální zařízení (NVA).** NVA je obecný termín, který popisuje OM provádění úloh, jako je povolení nebo odepření přístupu jako bránu firewall, optimalizace sítě WAN operace (včetně komprese sítě), vlastní směrování nebo jiných funkcí sítě. 

- **Web osy firmy osy a podsítí osy dat.** Toto jsou podsítí hostingu VMs a službách, které implementace příkladu 3 osy aplikace spuštěné v cloudu. V tématu [Spuštění Windows VMs N-vrstvy architektury na Azure] [ ra-n-tier] Další informace.

- **Uživatelsky definované směruje (UDR).** [Uživatelem definované směruje] [ udr-overview] určení toku přenosu IP v rámci Azure VNets. 

> [AZURE.NOTE] Podle toho, že splníte požadavky připojení VPN můžete nakonfigurovat směruje ohraničení brány (BGP Protocol) jako alternativa k použití UDRs implementace pravidel přesměrování směrování adres zpět v rámci místní síti.

- **Správa podsítě.** Tento podsítě obsahuje VMs implementující řízení a možnosti sledování pro součásti spuštěné v VNet. 

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura Pokud požadujete konkrétní příliš velký doporučení by měl těmito doporučeními.

### <a name="rbac-recommendations"></a>Doporučení RBAC

Vytvoření několik RBAC role pro přidávání a používání zdrojů v aplikaci. Zvažte vytvoření DevOps [vlastní roli] [ rbac-custom-roles] oprávnění ke správě infrastrukturu pro aplikace. Zvažte vytvoření centrální správce IT [vlastní roli] [ rbac-custom-roles] ke správě zdrojů sítě a samostatné zabezpečení správce IT [vlastní roli] [ rbac-custom-roles] ke správě zdrojů zabezpečené sítě, například NVAs. 

DevOps role by měl obsahovat oprávnění nasadit součástí aplikace, stejně jako sledování a restartujte VMs. Centralizované role správce IT by měl obsahovat oprávnění ke sledování síťové prostředky. Žádné z těchto role má mít přístup k NVA zdrojů, jako třeba s omezeným přístupem k zabezpečení role správce IT.

### <a name="resource-group-recommendations"></a>Doporučení skupina zdroje

Azure zdroje, jako jsou VMs, VNets a vyrovnávání zatížení může být snadno spravovat seskupením společně do skupiny zdrojů. Můžete potom můžete přiřadit role nad každou skupinu zdroje a omezit přístup.

Doporučujeme vytváření z následujících akcí:

- Skupina zdroje obsahující podsítí (s výjimkou VMs), NSGs a zdroje brány pro připojení k místní síti. Přiřadíte roli správce centralizované IT do této skupiny zdrojů.

- Skupina prostředků obsahující VMs NVAs (včetně Vyrovnávání zatížení), pole odkazů a jiných VMs správy a UDR podsítě brány, který vynucuje všech přenosů NVAs. Přiřaďte zabezpečení role správce IT této skupiny zdrojů.

- Samostatné skupiny prostředků pro jednotlivé aplikace osy, které obsahují Vyrovnávání zatížení a VMs. Všimněte si, že tato skupina zdroje nezahrnujte podsítí pro každou úroveň. Přiřadíte roli DevOps do této skupiny zdrojů.

### <a name="virtual-network-gateway-recommendations"></a>Virtuální sítě brány doporučení

Místní přenosy VNet předává prostřednictvím virtuální sítě brány. Doporučujeme [Brána Azure VPN] [ guidance-vpn-gateway] nebo [Azure ExpressRoute brány][guidance-expressroute]. 

### <a name="nva-recommendations"></a>Doporučení NVA

NVAs poskytují různé služby pro správu a sledování zatížení sítě. Azure Marketplace nabízí několik dodavatelů NVAs, včetně:

- [Barracuda webové aplikace Firewall] [ barracuda-waf] a [Barracuda NextGen Firewall][barracuda-nf]

- [Kompaktní sítě VNS3 brány Firewall/směrovači/VPN][vns3]

- [Fortinet FortiGate OM][fortinet]

- [Brána Firewall SecureSphere webové aplikace][securesphere]

- [Brána Firewall DenyAll webové aplikace][denyall]

- [Kontrola vSEC bodu][checkpoint]

Pokud žádný z těchto třetích stran NVAs odpovídají vašim požadavkům, můžete vytvořit vlastní NVA pomocí VMs. Příklad vytvoření vlastní NVAs najdete v článku DMZ tento odkaz architektury implementující následující funkce:

- Směrovat přenosy pomocí [IP přesměrování] [ ip-forwarding] na nic NVA.

- Přenosy se projít NVA jenom v případě, že je to potřeba. Každý NVA OM architektury odkaz je jednoduchý směrovači Linux s příchozích odeslané na síť rozhraní *eth0*a odchozí přenosy odpovídající pravidla definované vlastních skriptů odesláno prostřednictvím sítě rozhraní *eth1*. 

- Přenosy přesměrované do podsítě správy není projít NVAs a NVAs můžete konfigurovat pouze z podsítě správy. Umožnění datových přenosů do podsítě správy potřeby na směrovány NVAs, je bez směrování podsítě správy opravit NVAs, pokud mají být.  

- VMs NVA jsou součástí dostupné nastavit za vyrovnávání zatížení. NVA požadavky na Vyrovnávání zatížení směrovány UDR v podsítě brány.

Další doporučení k zamyšlení připojuje více NVAs v řadě s každý NVA provedení úkolu specializované zabezpečení. Díky jednotlivou funkci zabezpečení spravovat na základě za NVA. NVA provádění bránu firewall může proběhnout například v řadě s NVA službami identity. Kompromis pro snadnější správu je přidání směrování navíc sítě, který může zvětšit latence, tak zajistit, že to nemá vliv výkon aplikace.

### <a name="nsg-recommendations"></a>Doporučení NSG

Brána VPN zpřístupňuje veřejnou IP adresu pro připojení k místní síti. Doporučujeme vytvořit skupinu zabezpečení síti (NSG) pro vstupní NVA podsítě provádění blokovat všechny přenosy nepochází z místní síti.

Doporučujeme také implementace NSGs pro každou podsítě poskytnout druhé úrovně ochrany proti příchozích vynechání NVA chybně nakonfigurované nebo zakázané. Například podsítě web vrstvy architektury odkaz používá NSG pomocí pravidel ignorovat všechny žádosti o nejsou dostali z místní síti (192.168.0.0/16) nebo VNet a jiné pravidlo, které ignoruje všechny žádosti není na portu 80. 

### <a name="internet-access-recommendations"></a>Doporučení přístup k Internetu

[Platnost tunelem] [ azure-forced-tunneling] všechny odchozí internetových přenosů na místní sítí tunelem VPN k webu a směrování k Internetu pomocí sítě tranlation adresu (překladu síťových adres). To jak zabrání náhodné únik žádné důvěrné informace uložené v osy dat a taky povolit kontrolu a auditování všechny odchozí přenosy dat.

> [AZURE.NOTE] Nechcete blokovat úplně internetový provoz z úrovní webový server, business a aplikace. Pokud tyto vrstvy pomocí služby Azure PaaS, které používají veřejnou IP adres pro protokolování diagnostiky OM, stáhněte si OM rozšíření a další funkce. Azure diagnostiky také vyžaduje, aby součásti můžete čtení a zápis účet Azure úložiště závislé na Internetu.

Další doporučujeme ověřit, že odchozí internetový provoz je vytvořena platnost správně. Pokud používáte připojení VPN se [Služba Směrování a vzdálený přístup] [ routing-and-remote-access-service] na místního serveru použijte nástroj například [WireShark] [ wireshark] nebo [Microsoft zprávy Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Správa podsítě doporučení

Správa podsítě obsahuje pole odkaz, který se spustí, Správa a sledování funkce. Implementace následující doporučení pro pole odkazů:
- Nevytvářejte veřejnou IP adresu v poli odkaz. 
- Vytvoření jednoho z nich chcete přejít do pole odkaz prostřednictvím příchozí brány a implementaci NSG v podsítě Správa žádostí o odpovědět pouze z povolených postupu.
- Omezení provádění všechny úlohy správy zabezpečení do pole odkaz.

### <a name="nva-recommendations"></a>Doporučení NVA

Zahrnout vrstvy 7 NVA chcete ukončit připojení aplikací na úrovni NVA a udržovat spřažen úrovní back-end. Zaručuje, ve kterém odpověď adres úrovní back-end vrátí prostřednictvím NVA symetrickou připojení.

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Architektura odkaz používá při vyrovnávání zatížení přesměrování místní síti do fondu NVA zařízení. Jak bylo popsáno dříve, NVA VMs spuštění pravidla směrování přenosu sítě a zařízení jsou nasazeny do [Nastavte dostupnost][availability-set]. Tento návrh umožňuje sledovat výkon NVAs v čase a přidejte NVA zařízení v odpovědi na zvýšení načíst.

Standardní brána SKU VPN podporuje trvalý výkon až 100 MB /. Vysoký výkon SKU poskytuje až 200 MB /. Většími šířkami pásma zvažte možnost upgradu na ExpressRoute brány. ExpressRoute poskytuje až 10 GB/s šířky pásma s nižší latence než připojení VPN.

> [AZURE.NOTE] Články [provádění hybridní architektura sítě s Azure a místních VPN] [ guidance-vpn-gateway] a [provádění hybridní architektura sítě s Azure ExpressRoute] [ guidance-expressroute] popisují problémy kolem škálovatelnost Azure brány.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Architektura odkaz používá při vyrovnávání zatížení distribuce žádosti z místního do fondu NVA zařízení v Azure. NVA VMs spuštění pravidla směrování přenosu sítě a zařízení jsou nasazeny do [Nastavte dostupnost][availability-set]. Vyrovnávání zatížení pravidelně vyhledá stavu zkušební implementovaná v jednotlivých NVA a odeberete odpovídat NVAs z fondu. 

Pokud používáte Azure ExpressRoute zajišťuje propojení mezi VNet a místní síti, [Nakonfigurujte bránu VPN k poskytování převzetí] [ guidance-vpn-failover] Pokud připojení ExpressRoute nebude k dispozici.

Konkrétní informace o zachování dostupnost pro připojení VPN a ExpressRoute najdete v článcích [provádění hybridní architektura sítě s Azure a místních VPN] [ guidance-vpn-gateway] a [provádění hybridní architektura sítě s Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Všechny aplikace a sledování prostředků mají vykonat pole odkazů v podsítě správy. V závislosti na požadavcích aplikace budete muset přidat další sledování zdroje v podsítě správy, ale znovu některou z těchto další zdroje informací by měl k nim získat přístup prostřednictvím do pole odkaz.

Pokud je připojení brány z místní síti k Azure dolů, pořád zobrazíte do pole odkaz nasazením PIP ho přidáte do pole odkaz a Vzdálená v z Internetu.

Podsítě jednotlivé vrstvy architektury odkaz se po zamknutí pravidly NSG a může být nutné vytvořit pravidlo, které otevřete port 3389 RDP přístupu na Windows VMs nebo 22 SSH přístup na Linux VMs. Pravidla se otevře další porty může vyžadovat další nástroje pro správu a sledování.

Pokud používáte k poskytování propojení mezi místním datacentra a Azure ExpressRoute, použijte [Azure připojení nástrojů (AzureCT)] [ azurect] ke sledování a potíží s připojením.

> [AZURE.NOTE] Další informace konkrétně zaměřené na sledování a Správa připojení VPN a ExpressRoute v článcích [provádění hybridní architektura sítě s Azure a místních VPN] můžete najít[ guidance-vpn-gateway] a [provádění hybridní architektura sítě s Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Tento odkaz architektura implementuje několik úrovní zabezpečení: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Směrování všechny žádosti o místní uživatele prostřednictvím NVA

UDR v podsítě brány blokuje všechny žádosti o uživatele nejsou dostali z místního. Průchodů UDR povolené požadavky NVAs v soukromé podsítě DMZ a tyto požadavky předávají se aplikaci pokud jsou povoleny pravidly NVA. Jiné způsoby můžete přidat UDR, ale zkontrolujte, zda že není NVAs nebo bloku pro správu přenosy určené pro správu podsítě omylem nepoužili.

Vyrovnávání zatížení před NVAs taky slouží jako zabezpečení zařízení ignorování přenosy na porty, které nejsou otevřené v pravidla pro vyrovnávání zatížení. Vyrovnávání zatížení architektury odkaz sledovat HTTP požadavky na port 80 a HTTPS požadavky na port 443. Žádná další pravidla do Vyrovnávání zatížení musí popsány a provoz by měly být sledovány zajistit že neexistují problémy zabezpečení.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Použití NSGs do bloku/průchodu přenosy mezi aplikací vrstev

Všech úrovní webový server, business a data omezit přenosy mezi nimi pomocí NSGs. To znamená osy firmy používá NSG Pokud chcete blokovat všechny přenosy, které nemá pocházejí ve vrstvě web a osy dat pomocí NSG blokovat všechny přenosy, že není pocházejí z firmy osy. Pokud požadujete rozbalte pravidla NSG širší přístupu k tyto vrstvy, zvažte tyto požadavky hlediska zabezpečení rizikové. Každý nový příchozí cesty představuje příležitost náhodné nebo purposeful data únik nebo aplikace škod.

### <a name="devops-access"></a>DevOps přístup

Omezení operací provádějící DevOps u každé osy pomocí [RBAC] [ rbac] ke správě oprávnění. Při udělování oprávnění používat [Princip nejnižších možných oprávnění][security-principle-of-least-privilege]. Odhlaste se všechny operace pro správu a provést běžná audity zajistit, aby že byly plánované změny konfigurace.

> [AZURE.NOTE] Rozsáhlejší informace, příklady a scénáře Správa sítě cenného papíru s Azure, najdete v článku [cloudovým službám společnosti Microsoft a zabezpečení sítě][cloud-services-network-security]. Podrobné informace o ochraně zdrojů v cloudu, najdete v článku [Začínáme s Microsoft Azure zabezpečení][getting-started-with-azure-security]. Další podrobnosti o adresování zabezpečení se přes připojení Azure brány [provádění hybridní architektura sítě s Azure a místních VPN] najdete v článku[ guidance-vpn-gateway] a [provádění hybridní architektura sítě s Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Nasazení řešení

Nasazení pro implementující těmito doporučeními architekturu odkaz je dostupný na Github. Tento odkaz architektura obsahuje virtuální sítě (VNet), skupiny zabezpečení síti (NSG), Vyrovnávání zatížení a dvě virtuálních počítačích (VMs).

Architektura odkaz můžete nasadit buď s Windows nebo Linux VMs podle pokynů níže: 

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-private-dmz-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Parametr soubory obsahují pevně uživatelské jméno a heslo správce pro všechny VMs a důrazně doporučujeme okamžitě změnit obojí. Pro každou OM nasazení vyberte Azure portálu a klikněte na **Resetovat heslo** v zásuvné **podpory + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** uchovávat.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak implementovat [DMZ mezi Azure a na Internetu](./guidance-iaas-ra-secure-vnet-dmz.md).
- Zjistěte, jak implementovat [vysoce dostupné hybridní architektura sítě](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Zabezpečené architektura sítě hybridní"
