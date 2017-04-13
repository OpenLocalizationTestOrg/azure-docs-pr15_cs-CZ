<properties
   pageTitle="Operace správy sadu zabezpečení a zabezpečení dat auditování řešení | Microsoft Azure"
   description="Tento dokument vysvětluje způsob správy a chráněny v operace správy sadu zabezpečení a auditování řešení."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Operace správy sadu zabezpečení a auditování zabezpečení dat řešení

Zákazníkům zabránit, zjišťování a odpovědět na hrozeb usnadnit [operace správy sady (OMS) zabezpečení a auditování řešení](operations-management-suite-overview.md) shromažďuje a zpracovává data o zdrojích, která zahrnuje:

- Protokol událostí
- Událost trasování pro Windows (trasování událostí pro Windows) události
- Nástroje AppLocker auditování události
- Protokol brány Windows Firewall
- Pokročilé technologie pro analýzu hrozbou, že události
- Výsledky podle směrného plánu hodnocení
- Výsledky antimalware hodnocení
- Výsledky/oprava hodnocení
- Datové proudy Syslogs, které jsou na agent výslovně povolena

Provedeme silných závazky chránit osobní údaje a zabezpečuje data. Microsoft vyhovuje pro striktně zabezpečení a dodržování – z kódování provozní služby.
Tento článek popisuje způsob správy a chráněny v OMS zabezpečení a auditování řešení.

## <a name="data-sources"></a>Zdroje dat

OMS zabezpečení a auditování řešení analýza dat z virtuálních počítačích a fyzické počítače nainstalovanou agenta OMS. Konfigurace informace o událostech zabezpečení, například událostí systému Windows, protokolů auditování, protokoly služby IIS a syslog zprávy můžete shromažďovat OMS zabezpečení a auditování řešení. Příklady tyto údaje: typ operačního systému a verze systémy procesy, název počítače, IP adresy přihlášení uživatele a ID klienta.  

## <a name="data-protection"></a>Ochrana dat

**Oddělení dat**: dat bude k dispozici logicky samostatné na jednotlivé součásti v rámci služby. Všechna data označen za organizace. Tento označování zůstává v celém životním cyklu dat a nevynucují u každé vrstvy služby. 

**Přístup k datům**: poskytovat doporučení týkající se zabezpečení a prošetřit potenciální hrozeb zabezpečení, Microsoft zaměstnanci přístup shromažďované a analyzovat služby. Jsme dodržovat [Microsoft Online Services termínů](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [Zásady ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), které stát, že nebude Microsoft pomocí Data o zákaznících nebo dědí informace z něho pro obchodní účely reklamní nebo podobné. Poskytovat doporučení týkající se zabezpečení a prošetřit potenciální hrozeb zabezpečení, Microsoft pracovníci přístup shromažďované a analyzovat služby. Doporučujeme použít jenom Data o zákaznících v případě potřeby k poskytování služby Azure, včetně účely kompatibilní s poskytování těchto služeb. Zachovat všechny nároky na vlastních datech.

**Použití dat**: Společnost Microsoft použije vzorů a měřítka hrozbou, že vidět přes víc tenantů k vylepšení naše funkce Zabránění a zjišťování; jsme to udělat v souladu s závazky ochrany osobních údajů podle naše [Zásady ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Umístění dat nakonfigurovaný na úrovni OMS pracovního prostoru, při vytváření pracovního prostoru, který je součástí počáteční OMS zabezpečení a auditování konfigurace procesu.

## <a name="see-also"></a>Viz taky

V tomto dokumentu jste se naučili způsob správy a chráněny v OMS data. Další informace o řešení OMS zabezpečení a auditování, najdete tady:

- [Přehled správy sady (OMS) operace](operations-management-suite-overview.md)
- [Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení](oms-security-responding-alerts.md)
- [Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení](oms-security-monitoring-resources.md)

