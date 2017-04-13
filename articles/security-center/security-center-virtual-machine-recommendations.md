<properties
   pageTitle="Ochrana virtuálních počítačích v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument adresy doporučení v Centru zabezpečení Azure, které pomáhají chránit virtuálních počítačích a ponechání souladu s zásady zabezpečení."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Ochrana virtuálních počítačích v Centru zabezpečení Azure

Centrum zabezpečení Azure analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje chyby potenciální zabezpečení, vytvoří doporučení, které by vás provede procesem konfigurace potřebných ovládacích prvků.  Doporučení použít u typů Azure zdroje: virtuálních počítačích (VMs), připojení k síti SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují k VMs.  Centrum doporučení OM kolem shromažďování dat, instalace aktualizací systému zřizování antimalware, šifrování disků OM a další.  Použijte v následující tabulce jako odkaz na vám pomůže pochopit dostupné doporučení OM a co každý z nich bude dělat, když ho použijete.

## <a name="available-vm-recommendations"></a>K dispozici OM doporučení

|Doporučení|Popis|
|-----|-----|
|[Povolení shromažďování dat u předplatného](security-center-enable-data-collection.md)|Doporučuje zapnutí shromažďování dat v zásady zabezpečení pro platnost předplatného a všechny virtuálních počítačích (VMs) v předplatné.|
|[Nápravě chyby OS](security-center-remediate-os-vulnerabilities.md)|Doporučuje zarovnání konfiguraci OS s pravidly doporučená konfigurace například nepovolují ukládání hesel.|
|[Použití aktualizací systému](security-center-apply-system-updates.md)|Doporučuje nasazení chybějící systém zabezpečení a důležité aktualizace pro VMs.|
|[Restartujte po aktualizace systému](security-center-apply-system-updates.md#reboot-after-system-updates)|Doporučuje restartovat OM dokončete proces instalace aktualizací systému.|
|[Instalace Endpoint Protection](security-center-install-endpoint-protection.md)|Doporučuje zřízení antimalware programů VMs (pouze Windows VMs).|
|[Řešení Endpoint Protection stavu upozornění](security-center-resolve-endpoint-protection-health-alerts.md)|Doporučuje vyřešit chyby ochrany koncového bodu.|
|[Povolení OM agenta](security-center-enable-vm-agent.md)|Slouží k zobrazení, které vyžadují VMs agenta OM. V VMs musí být nainstalované agenta OM, aby bylo možné vytvořit oprava prohledávání vyhledávání podle směrného plánu a antimalware programy. Agent OM nainstalovaný ve výchozím nastavení pro VMs, které jsou nasazeny z Azure Marketplace. V článku [rozšíření – část 2 a OM Agent](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) obsahuje informace o tom, jak nainstalovat agenta OM.|
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje šifrování OM discích šifrovanou Azure disku (Windows a Linux VMs). Šifrování doporučujeme OS a data svazky na vaše OM.|
| [Aktualizovat operační systém verze](security-center-update-os-version.md) | Doporučuje aktualizace verze operačního systému (OS) pro vaše cloudové služby na nejnovější verzi k dispozici pro rodinu s operačním systémem.  Další informace o cloudové služby, najdete v článku [Přehled Cloudovým službám](../cloud-services/cloud-services-choose-me.md). |
| [Chyba hodnocení Nenainstalováno](security-center-vulnerability-assessment-recommendations.md) | Doporučuje nainstalovat řešení hodnocení chybu na vaše OM. |
| [Nápravě chyby](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Umožňuje zobrazit chyby systému a aplikací zjistit řešení hodnocení chybu na vaše OM nainstalovaný. |

## <a name="see-also"></a>Viz taky

Další informace o doporučení, které se vztahují na jiné typy Azure zdroje, najdete v těchto článcích:

- [Ochrana vašich aplikací v Centru zabezpečení Azure](security-center-application-recommendations.md)
- [Ochrana sítě v Centru zabezpečení Azure](security-center-network-recommendations.md)
- [Ochrana služby Azure SQL v Centru zabezpečení Azure](security-center-sql-service-recommendations.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
