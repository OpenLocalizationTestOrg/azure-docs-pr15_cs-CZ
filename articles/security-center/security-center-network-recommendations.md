<properties
   pageTitle="Ochrana sítě v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument adresy doporučení v Centru zabezpečení Azure, které vám pomůžou chránit Azure síť a ponechání souladu s zásady zabezpečení."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>Ochrana sítě v Centru zabezpečení Azure

Centrum zabezpečení Azure analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení, které by vás provede procesem konfigurace potřebných ovládacích prvků.  Doporučení použít u typů Azure zdroje: virtuálních počítačích (VMs), připojení k síti SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují k síti.  Centrum doporučení sítě kolem další bránách firewall generování, skupiny zabezpečení sítě, konfigurace příchozích pravidla a další.  Umožňuje v následující tabulce jako odkaz na vám pomůže pochopit, doporučení dostupné síťové a co každý z nich bude dělat, když ho použijete.

## <a name="available-network-recommendations"></a>Dostupné síťové doporučení

|Doporučení|Popis|
|-----|-----|
|[Přidání dalšího brány Firewall generování](security-center-add-next-generation-firewall.md)|Doporučuje zvýšit zabezpečení ochrany přidat brány Firewall pro další generování (NGFW) od partnera společnosti Microsoft.|
|[Směrování přenosů NGFW pouze](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Doporučuje konfigurovat sítě zabezpečení skupiny (NSG) pravidla, která vynutit příchozích OM prostřednictvím svého NGFW.|
|[Povolení skupiny zabezpečení sítě podsítí nebo virtuálních počítačích](security-center-enable-network-security-groups.md)|Doporučuje povolit NSGs na podsítí nebo VMs.|
|[Omezit přístup prostřednictvím Internetu koncový bod](security-center-restrict-access-through-internet-facing-endpoints.md)|Konfigurace příchozích pravidla pro NSGs doporučuje.|

## <a name="see-also"></a>Viz taky

Další informace o doporučení, které se vztahují na jiné typy Azure zdroje, najdete v těchto článcích:

- [Ochrana virtuálních počítačích v Centru zabezpečení Azure](security-center-virtual-machine-recommendations.md)
- [Ochrana vašich aplikací v Centru zabezpečení Azure](security-center-application-recommendations.md)
- [Ochrana služby Azure SQL v Centru zabezpečení Azure](security-center-sql-service-recommendations.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
