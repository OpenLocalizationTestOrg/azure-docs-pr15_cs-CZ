<properties
   pageTitle="Ochrana vašich aplikací v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument adresy doporučení v Centru zabezpečení Azure, které pomáhají chránit Azure aplikace a ponechání souladu s zásady zabezpečení."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Ochrana vašich aplikací v Centru zabezpečení Azure

Centrum zabezpečení Azure analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení, které by vás provede procesem konfigurace potřebných ovládacích prvků.  Doporučení použít u typů Azure zdroje: virtuálních počítačích (VMs), připojení k síti SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují na aplikace.  Aplikace Centrum doporučení kolem nasazení bránu firewall webové aplikace.  Umožňuje v následující tabulce jako odkaz na vám pomůže pochopit, doporučení k dispozici aplikace a co každý z nich bude dělat, když ho použijete.

## <a name="available-application-recommendations"></a>Doporučení k dispozici aplikací

|Doporučení|Popis|
|-----|-----|
|[Přidání bránu firewall webové aplikace](security-center-add-web-application-firewall.md)|Doporučuje nasadit web aplikace brány firewall (WAF) pro koncové body web. Přidáním tyto aplikace do stávajících nasazení WAF můžete chránit více webových aplikací v Centru zabezpečení. WAF spotřebiče (vytvořené pomocí Správce prostředků nasazení modelu) musí být nasazené na samostatném virtuální sítě. WAF spotřebiče (vytvořená pomocí klasické nasazení modelu) jsou používat skupiny zabezpečení sítě. Toto odborné pomoci bude prodlužuje na plně přizpůsobené nasazení (klasické) WAF zařízení v budoucnu.|
|[Dokončení Ochrana aplikace](security-center-add-web-application-firewall.md#finalize-application-protection)|Dokončete konfiguraci WAF musí přenosy přesměrovány WAF zařízení. Sledovat tento doporučení dokončí potřeby vzhled.|

## <a name="see-also"></a>Viz taky

Další informace o doporučení, které se vztahují na jiné typy Azure zdroje, najdete v těchto článcích:

- [Ochrana virtuálních počítačích v Centru zabezpečení Azure](security-center-virtual-machine-recommendations.md)
- [Ochrana sítě v Centru zabezpečení Azure](security-center-network-recommendations.md)
- [Ochrana služby Azure SQL v Centru zabezpečení Azure](security-center-sql-service-recommendations.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
