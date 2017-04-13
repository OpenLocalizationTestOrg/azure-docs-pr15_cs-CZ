<properties
   pageTitle="Úvod do integrace protokolu Microsoft Azure | Microsoft Azure"
   description="Informace o integration Azure protokolu klíčové funkce a jak to funguje."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Úvod do integrace protokolu Microsoft Azure (verze Preview)

Informace o integration Azure protokolu klíčové funkce a jak to funguje.

## <a name="overview"></a>Základní informace

Platformu pro služby (PaaS) a infrastruktury služby (IaaS) použitý ve Azure generovat velké množství dat v protokoly zabezpečení. Tyto protokoly obsahují důležitých informací, které lze zadat měřítka a výkonných podstatu porušení zásad interním a externím hrozeb, dodržování předpisů a problémů odchylky v síti, Host (hostitel) a činnosti uživatelů.

Integrace Azure log umožňuje integraci neformátovaných protokolů z Azure zdrojů do místního informací o zabezpečení a události Management (SIEM) systémy. Integrace Azure protokolu shromažďuje Azure diagnostiky z Windows *(WAD)* virtuálních počítačích, jakož i diagnostiky z partnerských řešení například webové aplikace brány Firewall (WAF). Tato integrace poskytuje jednotné řídicí panel pro všechny vaše majetek místní nebo v cloudu, tak, aby se daly agregační, sladit, analyzovat a výstrahy zabezpečení události.

![Integrace Azure protokolu][1]

## <a name="what-logs-can-i-integrate"></a>K čemu protokoly můžete integrovat?

Azure vytvoří rozsáhlé protokolování pro každou službu Azure. Tyto protokoly jsou rozdělené do kategorií podle dva hlavní typy:

- **Ovládací prvek a správa protokoly**, který umožňuje zobrazit na operace vytvoření správce prostředků Azure, aktualizovat a odstranit. Azure protokolů auditování je znázorněn příklad tohoto typu protokolu.
- **Protokoly rovině dat**, která umožňuje zobrazit do události aktivované jako součást použití Azure zdroje. Příklady tohoto typu protokolu událostí systému Windows systému, zabezpečení a aplikace protokoly do virtuálního počítače.

Integrace Azure protokolu v současné době podporuje integrace protokolů auditování Azure, protokoly virtuálního počítače a upozornění na Centrum zabezpečení Azure.

Pokud máte nějaké otázky o Integration protokolu Azure, pošlete nám prosím e-mailu pro [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Další kroky

V tomto dokumentu byly vydané na integraci Azure protokolu. Další informace o integration Azure protokolu a typů protokolů podporovaná, najdete v těchto článcích:

- [Microsoft Azure protokolu integrace Azure protokolů (verze Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, systémové požadavky a pokyny k instalaci integraci Azure protokolu.
- [Začínáme s Azure protokolu integrace](security-azure-log-integration-get-started.md) - tento kurz vás provede instalaci Azure protokolu integrace a integrace protokoly z Azure úložiště, protokolů auditování Azure a upozornění na Centrum zabezpečení.
- [Kroky konfigurace partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu se dozvíte, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar.
- [Azure protokolu integrace často kladené otázky (Nejčastější dotazy týkající se)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento najdete odpovědi na otázky týkající se integrace Azure protokolu.
- [Upozornění na Centrum zabezpečení integrace s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tohoto dokumentu uvidíte, jak synchronizovat upozornění na Centrum zabezpečení, spolu s virtuální počítač událostí zabezpečení shromážděná diagnostiky Azure a protokolů auditování Azure se protokolu analýzy nebo SIEM řešení.
- [Nové funkce služby Azure diagnostiky a protokolů auditování Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tento příspěvek blogu vás seznámí s protokolů auditování Azure a další funkce, které vám pomůžou získat podstatu operace Azure prostředků.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
