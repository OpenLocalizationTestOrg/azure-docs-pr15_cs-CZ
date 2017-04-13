<properties
   pageTitle="Rozhraní API verze Azure vyhledávací | Microsoft Azure | Rozhraní API pro hledání"
   description="Zásady správy verzí pro Azure hledání REST API a knihovně .NET SDK na klienta."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Rozhraní API verzí v Azure hledání

Azure vyhledávání vrátí funkce aktualizací v pravidelných intervalech. Někdy, ale ne vždy vyžadují tyto aktualizace nám publikovat novou verzi naše rozhraní API pro zachování zpětná kompatibilita. Publikování nové verze umožňuje určit, kdy a jak integrovat aktualizací služeb hledání v kódu.

Zpravidla pokusíme publikovat nové verze pouze v případě potřeby, protože může zahrnovat některé plánování řízené úsilí upgrade kód použít novou verzi rozhraní API. Novou verzi jsme bude publikovat pouze pokud je třeba změnit některé aspekty rozhraní API tak, aby konce zpětná kompatibilita. Tomu může dojít kvůli opravy existujících funkcí nebo z důvodu nových funkcí, které změnit existující rozhraní API plocha.

Jsme podle stejné pravidlo SDK aktualizace. SDK vyhledávací Azure následující pravidla [sémantického Správa verzí](http://semver.org/) , což znamená, že svou verzi má tři části: hlavní, podverze a sestavíte čísla (například 1.1.0). Jsme dáme nové hlavní verze SDK pouze v případě změny, které konce zpětná kompatibilita. Funkce aktualizací pevné jsme zvýší podverze a pro opravy chyb jsme zvětšíte pouze verzí buildu.

##<a name="snapshot-of-current-versions"></a>Snímek s aktuálními verzemi 

Níže je snímek s aktuálními verzemi všechny rozhraní pro programování Azure hledání. Plánování a další podrobnosti najdete v následujících částech tohoto dokumentu.

Rozhraní|Posledních hlavní verze|Stav
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Obecně k dispozici, uvolnění únor 2016
[Náhled .NET SDK](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|Náhled 2.0|Náhled uvolnění srpen 2016
[Služba REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015 02 28|Obecně k dispozici
[Služba REST API náhledu](search-api-2015-02-28-preview.md)|2015 02 28 náhledu|Náhled
[Správa rozhraní REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015. 08 19|Obecně k dispozici

Pro rozhraní REST API, včetně `api-version` při každém volání požaduje. Usnadňuje jej směrovat konkrétní formátu, jako je třeba náhled rozhraní API. Následující příklad ukazuje, jak `api-version` zadaný parametr:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] I když má každý požadavek `api-version`, doporučujeme používat stejnou verzi pro všechny žádosti o rozhraní API. To platí zejména při nové verze rozhraní API Představte atributů nebo operacích, které nerozpoznala předchozí verze. Míchání rozhraní API verzí můžete mít před nechtěným důsledky a je nutno.
> 
Služba REST API a rozhraní REST API správy jsou verzí nezávisle na sobě. Všechny podobnosti čísla verze je spolu náhodných.

Obecně dostupných (nebo GA) rozhraní API lze použít v výrobní a se vztahují smlouvy o úrovni služeb Azure. Náhled verzím pokusné funkce, které se vždy nemigruje GA verzi. **Důrazně doporučujeme před použitím náhledu rozhraní API aplikace výroby.**

##<a name="sdk-version-roadmap"></a>Přehled verze SDK

Každou verzí .NET SDK zaměřuje konkrétní verzi rozhraní REST API služeb. Funkce jsou chránění v rozhraní REST API nejdřív a potom implementovaná v SDK.

.NET SDK je teď přístupné a práce probíhá už na příští verzi. Následující tabulka vypadá rovnou budoucích verzích SDK tak, aby měli zjistit, co se blíží Další.

Verze .NET SDK|Verze rozhraní REST API|Funkce|ÉTA
----------------|----------------|--------|---
1.1|2015 02 28|Syntaxe dotazu Lucene|Únor 2016
Náhled 2.0|2015 02 28 náhledu|Vlastní analyzátory, objektů Blob Azure a indexování tabulky, mapování polí ETags|Srpen 2016
2.x|Novou verzi rozhraní API GA|Stejný jako náhled 2.0|Nejdříve možné Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Informace o náhled a obecně dostupných verzí

Azure hledání vždy předem vydání pokusné funkce prostřednictvím rozhraní REST API nejdřív pak prostřednictvím zkušební verze .NET SDK.

Funkce nemusí být poštovním GA verzi. Zatímco funkce ve verzi GA jsou považovány za stabilní a pravděpodobně měnit s výjimkou malé zpětně kompatibilní opravy a vylepšení, funkce jsou dostupné pro testování a vyzkoušení cílem shromáždit názory na funkce návrh a implementace. 

Protože funkce se mohou změnit, doporučujeme však proti psaní výrobní kódu, který má závislost na verze preview. Pokud používáte starší verzi preview, doporučujeme migrace na obecně dostupných verzí (GA). 

Pro .NET SDK: pokyny pro migraci kód, najdete na [Upgrade .NET SDK](search-dotnet-sdk-migration.md).

Všeobecně dostupná znamená, že hledání Azure nyní ve skupinovém rámečku smlouva o úrovni služeb (SLA). SLA najdete v [Azure hledání úroveň služeb](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

