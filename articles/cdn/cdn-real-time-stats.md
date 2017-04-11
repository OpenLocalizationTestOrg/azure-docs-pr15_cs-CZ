<properties
    pageTitle="Real-úvazek-stat v Azure CDN | Microsoft Azure"
    description="Data v reálném statistiky obsahuje data v reálném čase o výkonu Azure CDN při doručení obsahu pro klienty."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Data v reálném stat v Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Základní informace

Tento dokument vysvětluje v reálném čase stat v Microsoft Azure CDN.  Tato funkce obsahuje data v reálném čase, jako jsou šířka pásma, stavy mezipaměti a souběžné připojení do vlastního profilu CDN při doručení obsahu pro klienty. Díky nepřetržitého sledování stavu služby kdykoli, včetně live přejít událostí.

Následující grafy jsou k dispozici:

* [Šířka pásma](#bandwidth)
* [Stavů](#status-codes)
* [Zobrazit stavy mezipaměti](#cache-statuses)
* [Připojení](#connections)


## <a name="accessing-real-time-stats"></a>Přístup k v reálném čase stat

1. Na [Portálu Azure](https://portal.azure.com)přejděte do vlastního profilu CDN.

    ![Zásuvné CDN profilu](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-real-time-stats/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

3. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **Stat v reálném čase** .  Klepněte na **HTTP velkého objektu**.

    ![Portál Správa CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Data v reálném stat grafy zobrazují.
    
Každá grafů zobrazí v reálném čase statistiky vybraného časového rozsahu spuštění při načtení stránky.  Grafy automaticky aktualizován každých několik sekund.  Tlačítko **Aktualizovat graf** , pokud je k dispozici, bude zrušte v grafu, po jejímž uplynutí se zobrazí jenom vybraná data.

## <a name="bandwidth"></a>Šířka pásma

![Šířka pásma grafu](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Šířka pásma** graf zobrazuje šířku pásma používaných pro aktuální platformu přes vybraného časového období. Stínované část grafu označuje využití šířky pásma. Zobrazí se přesně šířku pásma aktuálně používaných přímo pod spojnicový graf.

## <a name="status-codes"></a>Stavů

![Stavový kód grafu](./media/cdn-real-time-stats/cdn-status-codes.png)

Graf **Stavů** označuje, jak často určité kódy odpověď HTTP dochází přes vybraného časového rozsahu.

> [AZURE.TIP]  Popis jednotlivých možností stavu kód HTTP najdete v tématu [Azure CDN HTTP stav kódy](https://msdn.microsoft.com/library/mt759238.aspx).

Zobrazí se seznam HTTP stavů přímo nad grafem. Tento seznam uvádí každý stavový kód, který může být součástí spojnicový graf a aktuální počet výskytů sekundu pro tuto stavový kód. Ve výchozím nastavení se zobrazí řádek pro každou z těchto stavů v grafu. Můžete však sledovat pouze kódy stavu, které zvláštním významem konfiguraci CDN. K tomuto účelu zaškrtněte požadované stavů a vymazat všechny další možnosti a klikněte na **Aktualizovat graf**. 

Můžete dočasně skrýt protokolované dat pro konkrétní stavový kód.  V legendě přímo pod grafem klikněte na stavový kód, který chcete skrýt. Stavový kód se okamžitě skryté z grafu. Kliknutím na tento kód stav znovu způsobí tuto možnost znovu zobrazovat.

## <a name="cache-statuses"></a>Zobrazit stavy mezipaměti

![Graf stavy mezipaměti](./media/cdn-real-time-stats/cdn-cache-status.png)

Grafu **Mezipaměti stavy** označuje, jak často určitých typů mezipaměti stavy dochází přes vybraného časového rozsahu. 

> [AZURE.TIP]  Popis jednotlivých možností stavu kód mezipaměti najdete v článku [Azure CDN mezipaměti stavů](https://msdn.microsoft.com/library/mt759237.aspx).

Zobrazí se seznam mezipaměti stavů přímo nad grafem. Tento seznam uvádí každý stavový kód, který může být součástí spojnicový graf a aktuální počet výskytů sekundu pro tuto stavový kód. Ve výchozím nastavení se zobrazí řádek pro každou z těchto stavů v grafu. Můžete však sledovat pouze kódy stavu, které zvláštním významem konfiguraci CDN. K tomuto účelu zaškrtněte požadované stavů a vymazat všechny další možnosti a klikněte na **Aktualizovat graf**. 

Můžete dočasně skrýt zaznamenán pro konkrétní stavový kód.  V legendě přímo pod grafem klikněte na stavový kód, který chcete skrýt. Stavový kód se okamžitě skryté z grafu. Kliknutím na tento kód stav znovu způsobí tuto možnost znovu zobrazovat.

## <a name="connections"></a>Připojení

![Připojení grafu](./media/cdn-real-time-stats/cdn-connections.png)

Tento graf označuje vytvořily kolik připojení k serverům okraje. Každý požadavek na materiálů, které prochází naše CDN za následek připojení.

## <a name="next-steps"></a>Další kroky

- Oznámení s [v reálném čase upozornění v Azure CDN](cdn-real-time-alerts.md)
- Prozkoumat důkladněji s [Rozšířené HTTP sestavy](cdn-advanced-http-reports.md)
- Analýza [využití vyhledávání](cdn-analyze-usage-patterns.md)

