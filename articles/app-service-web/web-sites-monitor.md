<properties
    pageTitle="Sledování aplikací služby Azure aplikace"
    description="Naučte se monitorování aplikací v aplikaci služby Azure pomocí portálu Azure."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Postup: monitorování aplikací v aplikaci služby Azure

[Aplikace služby](http://go.microsoft.com/fwlink/?LinkId=529714) funguje předdefinovaný monitorování [Azure portálu](https://portal.azure.com).
Platí to i pro možnost kontrolujte potvrzení **kvót** a **metriky** pro aplikace, jakož i plán služeb aplikací, nastavte si **upozornění** a dokonce i **škálování** automaticky na základě těchto metriky.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Principy kvót a parametrů

### <a name="quotas"></a>Kvóty

Aplikace umístěné v aplikaci služby se vztahují určitých *limity* prostředky, které můžete použít. Limity jsou definovány **plán služeb aplikací** přidružený k aplikaci.

Pokud aplikace je umístěn v plánu **Free** nebo **sdílené** , jsou definované omezení prostředky, které můžete použít aplikaci **kvóty**.

Pokud aplikace je hostitelem v **základní**, plán **Standardní** nebo **Premium** a pak limity prostředky, které můžete použít nastavení **velikosti** (malá, střední, velká) a **počet instancí** (1, 2, 3,...) **plán služeb aplikací**.

Jsou **kvóty** aplikací **Free** nebo **sdílený** :

* **CPU(Short)**
   * Velikost procesoru povolených pro tuto aplikaci v intervalu 3 minuty. Tato kvóta znovu nastaví každé 3 minuty.
* **CPU(Day)**
   * Celková částka procesoru povolených pro tuto aplikaci v den. Tato kvóta znovu nastaví každých 24 hodin půlnoci UTC.
* **Paměti**
   * Celková částka paměti Povolené k této aplikaci.
* **Šířka pásma**
   * Celková částka odchozí šířky pásma pro tuto aplikaci v den povolené.
   Tato kvóta znovu nastaví každých 24 hodin půlnoci UTC.
* **Systém souborů**
   * Celková velikost úložiště povolené.

Pouze kvóty aplikace hostitelem **základní** **Standardní** a plány **Premium** je **systému souborů**.

Další informace o konkrétních kvóty, omezení a funkcí dostupných na jinou SKU aplikace služby najdete tady: [Omezení služby Azure předplatného](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Vynucení kvót

Pokud aplikace v jeho použití překročí **sloupcem procesor (krátké)**, **sloupcem procesor (den)**nebo **šířku pásma** kvóty pak aplikace se zastaví dokud znovu nenastaví kvóty. Během této doby všechny příchozí žádosti bude mít za následek **HTTP 403**.
![][http403]

Pokud překročení kvóty **paměti** aplikace aplikace bude opakuje.

Pokud je Překročená kvóta **systému souborů** , některé napište se nezdaří, a to včetně zápisu protokolů.

Kvóty můžete zvýšit nebo odstranit z aplikace pro upgrade vašeho plánu aplikace služeb.

### <a name="metrics"></a>Metriky

**Metriky** obsahují informace o aplikaci nebo chování plán služeb aplikací.

U **aplikace**jsou dostupné metriky:

* **Průměrná doba odezvy**
   * Průměrná doba aplikace do žádosti o ms.
* **Průměrná pracovní sada paměti**
   * Průměrná částka paměti v MIB používané aplikace.
* **Procesor času**
   * Velikost procesoru v sekundách využívané aplikace. Další informace o tomto metrických najdete v tématu: [procesoru procentuální hodnota procesoru a času](#cpu-time-vs-cpu-percentage)
* **Data v**
   * Velikost příchozí šířky pásma využívané v aplikaci MIB.
* **Data se**
   * Velikost odchozí šířky pásma využívané v aplikaci MIB.
* **Nastavit informace HTTP 2xx**
   * Počet požadavků výsledkem stavový kód http > = 200 ale < 300.
* **Nastavit informace HTTP 3xx**
   * Počet požadavků výsledkem stavový kód http > = 300 ale < 400.
* **Nastavit informace HTTP 401**
   * Počet požadavků výsledkem HTTP 401 stavový kód.
* **Nastavit informace HTTP 403**
   * Počet požadavků výsledkem HTTP 403 stavový kód.
* **Nastavit informace HTTP 404**
   * Počet požadavků výsledkem HTTP 404 stavový kód.
* **Nastavit informace HTTP 406**
   * Počet požadavků výsledkem HTTP 406 stavový kód.
* **Nastavit informace HTTP 4xx**
   * Počet požadavků výsledkem stavový kód http > = 400 ale < 500.
* **HTTP Server chyby**
   * Počet požadavků vzniknou stavový kód http > = 500 ale < 600.
* **Pracovní sada paměti**
   * Aktuální velikost paměti používaný v aplikaci MIB.
* **Požadavky**
   * Celkový počet požadavků bez ohledu na jejich výsledné stavový kód HTTP.

Pro **aplikaci služby plán**jsou k dispozici metriky:

>[AZURE.NOTE] Metriky plán služeb aplikace jsou dostupné jenom pro plány v **základní** **Standardní** a **Premium** SKU.

* **Procento využití procesoru**
   * Průměrná procesoru ve všech instancích plánu.
* **Procento paměti**
   * Průměrná paměti sloužícího všechny výskyty plánu.
* **Data v**
   * Průměrná příchozí šířku pásma sloužícího všechny výskyty plánu.
* **Data se**
   * Průměr odchozí pásma ve všech instancích plánu.
* **Délka fronty disku**
   * Průměrný počet čtení a zápis požadavky, které byly ve frontě úložný prostor. Délka fronty vysoké disku údaj aplikace, která může zpomalovat kvůli nadbytečné disku vstupu a výstupu.
* **Délka fronty http**
   * Průměrný počet požadavků HTTP, které bylo sednout ve frontě před jsou splněny. Nízké nebo rostoucí délka HTTP fronty je příznakem plán při velkém zatížení.

### <a name="cpu-time-vs-cpu-percentage"></a>Procesor Čas a procesoru procento
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Existuje 2 metriky, které odpovídají využití procesoru. **Čas procesoru** a **Procento využití procesoru**

**Čas procesoru** je užitečné pro aplikace hostované v plánech **Free** nebo **sdílený** od jejich kvót je definován v minutách procesoru používané aplikace.

**Procento využití procesoru** na druhou stranu slouží k aplikacím hostované v plánech **základní** **Standardní** a **premium** vzhledem k tomu může být diagramů s měřítky a tento míru je dobré ukazatelem celkové využití ve všech instancích.

##<a name="metrics-granularity-and-retention-policy"></a>Metriky Granularity a zásady uchovávání informací

Metriky pro aplikace a plán služeb aplikací jsou přihlášení k lyncu a agregovat službou s následující granularity a zásady uchovávání informací:

 * **Minuta** granularity metriky uchovávají pro **48 hodin**
 * Metriky granularity **hodinu** uchovávají pro **30 dní.**
 * Metriky granularity **den** se zachovají **90** dní

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Sledování kvót a metriky Azure portálu.

Můžete zkontrolovat stav různých **kvót** a **metriky** došlo k ovlivnění aplikace [Portál Azure](https://portal.azure.com).

![][quotas]
**Kvóty** najdete v části Nastavení >**kvóty**. Činnosti koncového uživatele umožňuje zkontrolovat: (1) kvóty název, (2) jeho obnovení interval, (3) jeho aktuální limit a (4) aktuální hodnotu.

![][metrics]
**Metriky** může být access přímo z zásuvné zdroje. Je možné upravit taky grafu tak, že: (1) **klikněte** na a vyberte (2) **Upravit graf**.
Tady můžete změnit (3) **časový rozsah**, (4) **typ grafu**a (5) **metriky** zobrazíte.  

Další informace o metriky tady: [metriky služby Monitor](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Upozornění a automatické měřítko
Metriky pro některý z plánů aplikace nebo služby aplikace můžete až zavěšení upozornění, na další informace najdete v článku [dostávat oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Použitý ve základní, standardní nebo premium aplikace služby plány jednotného zasílání zpráv aplikace aplikaci služby podpory **Automatické měřítko**. Pokud je aplikace navýšení poskytování díky nastavit pravidla, která sledovat metriky plánu aplikace služby a můžete zvětšit nebo zmenšit počet instancí poskytnout podpoře dalších zdrojů v případě potřeby nebo uložení peníze. Další informace o automatické měřítko tady: [jak měřítko](../monitoring-and-diagnostics/insights-how-to-scale.md) a tady [Doporučené postupy pro sledování Azure neobsahovaly text](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
