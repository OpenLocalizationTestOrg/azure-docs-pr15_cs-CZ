
<properties
   pageTitle="Azure pokyny | Příklady a postupy | Microsoft Azure"
   description="Azure odkaz architektury"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure odkaz architektury

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Tento obsah je aktivní vývoji. Je vhodné dnes, proto jsme zpřístupnili ho (verze Preview). Hodnota nám svůj názor._

Náš odkaz architektury jsou uspořádány podle scénáře, s několika souvisejících architektury seskupené.
Každou jednotlivé architekturu nabízí doporučené postupy a obvyklé kroky, stejně jako součást spustitelný ztělesňuje doporučení.
V mnoha architektury jsou postupné; vytváření nad předchozí architektury, kteří mají méně požadavky.

## <a name="designing-your-infrastructure-for-resiliency"></a>Navrhování infrastrukturu odolnosti

Tato řada začíná doporučené postupy pro optimální OM konfiguraci a culminates ve více oblastech nasazení s překlopení.

- [Systém Windows OM na Azure](guidance-compute-single-vm.md)
- [Spuštění Linux OM na Azure](guidance-compute-single-vm-linux.md)
- [Spouštění více virtuálních škálovatelnost a dostupnosti](guidance-compute-multi-vm.md)
- [Spouštění virtuálních Windows N-vrstvy architektury](guidance-compute-n-tier-vm.md)
- [Spouštění Linux virtuálních N-vrstvy architektury](guidance-compute-n-tier-vm-linux.md)
- [Spouštění Windows virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters.md)
- [Spouštění Linux virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Připojení vaší místní síti k Azure

Tato řada spustí prokázat způsoby, jak připojit existující síť Azure. Potom šipka se rozbalí pokrýval požadavky na dostupnost a zabezpečení.

- [Provádění hybridní architektura sítě s Azure a místních VPN](guidance-hybrid-network-vpn.md)
- [Provádění hybridní architektura sítě s Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Provádění vysoce dostupné hybridní architektura sítě](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Zabezpečení hybridní sítě

Tato řada obsahuje osvědčené postupy týkající se vytváření DMZ v Azure zabezpečené připojení pocházejících z místního datacentra a na Internetu.

- [Provádění DMZ mezi Azure a místních datacentru](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Provádění DMZ mezi Azure a na Internetu](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Poskytování služeb Identity

Tato řada se spustí ukázkou ověřování uživatelů v Azure pomocí služby Azure Active Directory. Potom šipka se rozbalí pokrýval složité scénáře rozšíření infrastrukturu přidá Azure a pomocí služby AD FS pro delegování.

- [Provádění Azure Active Directory](./guidance-identity-aad.md)
- [Rozšíření adresářové služby Active Directory (přidá) na Azure](./guidance-identity-adds-extend-domain.md)
- [Vytvoření struktury zdrojů Active Directory adresáře služeb (přidá) v Azure](./guidance-identity-adds-resource-forest.md)
- [Implementace v Azure Active Directory Federation Services (služby AD FS)](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Architecting scalable webové aplikace pomocí Azure PaaS

Tato řada obsahuje doporučení pro vytváření scalable a snadno dostupné webových aplikacích. 

- [Základní webovou aplikaci](guidance-web-apps-basic.md)
- [Vylepšení škálovatelnost ve webové aplikaci](guidance-web-apps-scalability.md)
- [Webová aplikace s vysokou dostupnost](guidance-web-apps-multi-region.md)
