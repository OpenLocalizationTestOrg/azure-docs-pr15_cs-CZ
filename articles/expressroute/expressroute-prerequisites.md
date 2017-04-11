<properties
   pageTitle="Požadavky pro přijetí ExpressRoute | Microsoft Azure"
   description="Tato stránka obsahuje seznam požadavky, než objednejte Azure ExpressRoute okruh."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>Požadavky na ExpressRoute & kontrolního seznamu  

Pokud chcete připojit ke cloudovým službám společnosti Microsoft pomocí ExpressRoute, bude nutné ověřit, zda jsou splněny následující požadavky uvedené níže.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Účet Azure

- Účet Microsoft Azure platných a aktivní. To je nutné nastavit okruh ExpressRoute. Obvody ExpressRoute jsou zdroje informací v rámci Azure předplatná. Předplatné Azure je povinné i v případě připojení se omezí na Azure Microsoft cloudové služby, například služby Office 365 a CRM online.
- Aktivní předplatné Office 365 (Pokud pomocí služby Office 365). V části [specifických požadavků Office 365](#office-365-specific-requirements) tohoto článku Další informace.

## <a name="connectivity-provider"></a>Zprostředkovatel připojení
- Můžete spolupracovat s [partnerem připojení ExpressRoute](expressroute-locations.md#partners) připojení ke cloudu společnosti Microsoft. Můžete nastavit spojení mezi místní síti a Microsoft [třemi](expressroute-introduction.md#howtoconnect)způsoby. 
- Pokud váš poskytovatel není partnera ExpressRoute připojení, pořád připojením ke cloudu společnosti Microsoft prostřednictvím [poskytovatele exchange cloudu](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Požadavky na sítě
- **Nadbytečné připojení**: žádný redundance požadavek na fyzické připojení mezi vámi a poskytovatele. Microsoft nevyžaduje nadbytečné relací BGP nastavit mezi směrovači společnosti Microsoft a peering směrovači, i když máte jenom [jednu fyzické připojení k exchange cloudu](expressroute-faqs.md#onep2plink). 
- **Směrování**: v závislosti na způsobu připojení ke cloudu společnosti Microsoft, musí se dají vytvořit a spravovat relace BGP pro [směrování domény](expressroute-circuit-peerings.md)nebo poskytovatele. Některé Ethernet připojení nebo cloudu exchange zprostředkovatele můžou nabízet BGP Správa jako na hodnotu přidat službu.
- **Překladu síťových adres**: Microsoft přijímá jenom veřejné adresy IP pomocí Microsoft prozkoumávání. Pokud používáte soukromé IP adresy ve svojí místní síti, vy nebo váš poskytovatel potřeba přeložit soukromých IP adres na veřejnou IP adresu [Převaděč](expressroute-nat.md).
- **QoS**: Skype pro firmy má různé služby (například hlasových zpráv, video, text), které vyžadují liší QoS pozornost. A poskytovatele postupujte [QoS požadavky](expressroute-qos.md).
- **Síť zabezpečení**: [zabezpečení sítě](../best-practices-network-security.md) byste měli zvážit při připojení ke cloudu společnosti Microsoft prostřednictvím ExpressRoute.
 
## <a name="office-365"></a>Office 365

Pokud chcete povolit Office 365 na ExpressRoute, zkontrolujte tyto dokumenty Další informace o požadavcích na Office 365.


- [Základní informace o ExpressRoute pro Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Směrování s ExpressRoute pro Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Plánování sítě a ladění výkonu pro Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Kalkulačky šířky pásma a nástroje](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Integrace Office 365 s místním prostředím](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Pokud chcete povolit CRM Online na ExpressRoute, zkontrolujte tyto dokumenty Další informace o CRM Online

- [CRM Online adresy URL](https://support.microsoft.com/kb/2655102) a [rozsahy IP adres](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Další kroky

- Další informace o ExpressRoute najdete v tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md).
- Nalezení poskytovatele ExpressRoute připojení. V tématu [ExpressRoute partnery a prozkoumávání umístění](expressroute-locations.md).
- Podívejte se do požadavky pro [směrování](expressroute-routing.md), [překladu síťových adres](expressroute-nat.md) a [QoS](expressroute-qos.md).
- Konfigurace připojení k ExpressRoute.
    - [Vytvoření ExpressRoute okruh](expressroute-howto-circuit-classic.md)
    - [Konfigurace směrování](expressroute-howto-routing-classic.md)
    - [Odkaz VNet ExpressRoute obvodu](expressroute-howto-linkvnet-classic.md)

