<properties
   pageTitle="Použití Microsoft Azure a povolit rozhraní API RateCard Cloudyn ITFM pro zákazníky | Microsoft Azure"
   description="Poskytuje jedinečného pohledu z Microsoft Azure fakturace partnera Cloudyn, jejich zkušenosti integrace rozhraní API fakturace Azure do svých produktů.  To je užitečné pro Azure a Cloudyn zákazníky, kteří mají zájem pomocí/vyzkoušení Cloudyn služby Azure."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Použití Microsoft Azure a rozhraní API RateCard povolit Cloudyn stanovit ITFM zákazníci

Cloudyn, vývoj partnera společnosti Microsoft a úvodní poskytovatele cloudové možnosti správy, byla zvolit pro soukromé náhled nové RateCard API a používání zdrojů Microsoft Azure.  Použití rozhraní API poskytuje přístup k datům odhadovaná Azure spotřeba pro předplatné. Rozhraní API RateCard poskytuje kompletní informace cenách všech Azure služeb - Enterprise Agreement EA zákazníci. Integrované dohromady, tato rozhraní API jako základ podrobné informace o zadání kritérií do nástroje IT finanční Management (ITFM) třeba můžou být poskytovanou Cloudyn.

## <a name="introduction"></a>Úvod

Takzvané "násobení" dat z rozhraní API použití daty z rozhraní API RateCard (cena použití [jednotky] [$unit] = podrobné použití a nákladové) vytvoří největší podrobného, přesné a spolehlivé fakturačních údajů k dispozici pro Azure dnes.

![Přehled ITFM][1]

Jinými těchto rozhraní API obsahuje klíčové informace o použití a náklady, povolení Cloudyn analyzovat účet zákazníka jednoduché, programové způsobem a dělat nejrůznější věci ITFM svým zákazníkům zákazníků.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integrace Cloudyn s RateCard a rozhraní API použití
Rozhraní API RateCard vyžaduje několik vstupních parametrů – jako je oblast informace, měny a národní prostředí – ale nejdůležitějším OfferDurableID, která určuje, že typ Azure nabízející zákazníka používá (přislíbený, starší verze 6 a 12měsíční potvrzení plány, MSDN nabídek, MPN nabídky, propagační nabídky a další). OfferDurableID najdete v [Azure použití a fakturace portál](https://account.windowsazure.com/Subscriptions), klikněte v části "Nabízejí ID" pro dané předplatné.

Při registraci služby [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) můžete přidat zákazníci jejich OfferDurableID kód, který umožňuje Cloudyn zobrazíte jejich relevantní informace prostřednictvím rozhraní API RateCard cenách.  Informace o různých typech nabídek najdete jednu stránku [Microsoft Azure nabízejí podrobnosti](https://azure.microsoft.com/support/legal/offer-details/) .

![Přehled Engine Cloudyn ITFM][2]

Použití Cloudyn použití i rozhraní API RateCard kromě rozhraní API výkonu Azure vytvořit další vrstvy vizualizace, analýzy, výstrahy, sestav, náklady řízení a akce doporučení poskytování Azure zákazníci nástroje ITFM cloudu spolehlivé organizace.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Případy použití Cloudyn ITFM povolené použití a rozhraní API RateCard integrace
Běžné ITFM Cloudyn pomocí případech povolené použití a rozhraní API RateCard patří:

+ Umožňuje **Analýzy náklady** - náklady, které mají být nefunkční dolů dimenzí nativní identifikační (poskytovatele služby, účtu, oblasti apod.) v cloudu. Použití Azure a rozhraní API RateCard nastavte u této úkol snadno poskytnutím nejčastěji podrobný rozpis použití a data na účet, který je potom seskupené filtrované podle Cloudyn a prezentovat uživateli ve formě obrázku nebo tabulkový nákladů.

![Náklady analýzy výsečového grafu][3]

+ **360 přidělení náklady** – umožňuje finance a správci IT vám pomůžou odhalit rozdělení skutečných nákladů, ovladače a trendy nasazení cloudu. Další umožňuje správcům nasazení výdaje snadno přidružit organizačních jednotek, oddělení, oblasti a další, poskytnutí špičkovým jádrem přehledy do cloudu náklady a usnadnění chargebacks organizace a showbacks. Použití Azure a rozhraní API RateCard sloužit jako vstup společnosti Cloudyn náklady přidělení stroje, které doplňuje rozhraní API tím, že definujete metody a obchodní logiky pro přidělování netagované nebo untaggable zdrojů.

![Přidělení nákladů 360 grafu][4]

+ **Pro změnu velikosti cost-Effective** - doporučí vpravo pro změnu velikosti další není virtuálních počítačích, čímž se zmenší zákazníka výdaje na počítačích příliš velký nebo povolená zřizování. Stejně tak porovnáním virtuální počítač procesoru a paměti RAM metriky (přes výkonu rozhraní API) hodin běhu (přes použití rozhraní API) a náklady (přes RateCard rozhraní API). Cloudyn potom doporučí vpravo pro změnu velikosti podle není zdroje procesoru a paměti RAM (výkon) a vypočítá odhad úspor vynásobením delta cena (RateCard) mezi VMs skutečný čas-využívání (použití) není počítač.

![Efektivní nákladů pro změnu velikosti][5]

+ **Doporučení přenos cloudu** - poskytuje finanční Rady v cloudu přenos. Zkontroluje uživatele aktuální náklady na zdroje cloudu, které jsou nasazeny na hlavní cloudu dodavatele a porovnává náklady na nasazení odpovídající na Azure. Potom poskytuje podrobného, za časově, finanční na základě přenos doporučení Azure. Po hodnocení odpovídající nasazení požadované na Azure (založenou na výkonu metriky a uživatelské předvolby), Cloudyn pomocí rozhraní API RateCard vyhodnocení náklady na odpovídající nasazení na Azure.

+ **Sestav výkonu** – povolit tak, že je Azure výkonu rozhraní API, tyto sestavy poskytují maticových funkcí z využití procesoru a paměti RAM optimalizace doporučení. Tady je příklad instance využití sestav, prezentaci instance rozdělení podle průměru využití procesoru.

![Sestavy výkonu][6]

+ **Správce kategorie** – výkonné funkce v Cloudyn, která přiblíží objednávky k neuspořádaný cloudové zdroje. Poskytuje uživatelům volného vytvořit vlastní jedinečný kategorie (značky) pro efektivní měření a vytváření sestav, který je v souladu s obchodních postupů. Dále uživatelů můžete snadno upravit zařadit do kategorií nekonzistentní označování (tedy překlepů a dalších rozdílů) a automaticky zjišťovat netagované zdroje pro přesné náklady přiřazení.

![Správce kategorie][7]

## <a name="video"></a>Video

Tady je krátké video, které ukazuje, jak můžete zákazníkem Azure pomocí Cloudyn Azure a rozhraní API fakturace Azure získat přehledy z dat Azure spotřebu.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Další kroky

+ Spusťte bezplatnou zkušební verzi [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) zobrazíte, jak lze získat náklady průhlednost používat přizpůsobené sestavy a analýzy pro Microsoft Azure cloudu nasazení.
+ Základní informace o používání zdrojů Azure a rozhraní API RateCard naleznete v tématu [získání podstatu využití prostředků Microsoft Azure](billing-usage-rate-card-overview.md) .
+ Podívejte se na [Azure fakturace REST API odkaz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) Další informace o obou rozhraní API, které jsou součástí sady rozhraní API správcem zdrojů Azure k dispozici.
+ Pokud chcete pusťte do ukázkový kód, podívejte se na našeho Microsoft Azure fakturace rozhraní API ukázky na [Azure ukázky](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Víc se uč
+ Další informace o nabídkách k Microsoft Azure Enterprise Agreement (EA), navštivte [Azure licence pro podniku] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Naleznete v článku [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) Další informace o správce prostředků Azure.
+ Další informace o sadu nástrojů nutné Nápověda v tím, že cloud výdajů, naleznete v článku Gartner [Market Příručka pro nástroje IT finanční Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
