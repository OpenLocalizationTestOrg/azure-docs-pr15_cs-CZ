<properties
    pageTitle="Azure sítě analýzy řešení v protokolu analýzy | Microsoft Azure"
    description="Řešení Azure sítě analýzy v protokolu analýzy můžete použít k prohlížení protokoly skupiny zabezpečení Azure sítě a Azure aplikace brány."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure řešení sítě analýzy (verze Preview) v protokolu analýzy

>[AZURE.NOTE] Jedná se o [Náhled řešení](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Řešení Azure sítě analýzy v protokolu analýzy můžete použít k prohlížení protokoly Azure aplikace brány a Azure sítě zabezpečení skupiny.

Můžete povolit protokolování pro protokoly Azure aplikace brány a skupiny zabezpečení Azure sítě. Tyto protokoly jsou napsali na místo, kam se můžete pak indexovat protokolu analýzy pro hledání a analýzu úložiště objektů Blob.

Pro aplikace bran jsou podporovány následující protokolů:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

Pro skupiny zabezpečení sítě jsou podporovány následující protokolů:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Instalace a konfigurace řešení

Instalace a konfigurace řešení Azure sítě analýzy postupujte podle následujících pokynů:

1.  Povolení protokolování diagnostiky pro prostředky, které chcete sledovat:
  + [Aplikace brány](../application-gateway/application-gateway-diagnostics.md)
  + [Skupina zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Konfigurace protokolu analýzy číst protokoly z úložiště objektů Blob pomocí procesu podle [JSON souborů v úložišti objektů blob](../log-analytics/log-analytics-azure-storage-json.md).
3.  Povolte řešení Azure sítě analýzy pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  

Pokud nepovolíte diagnostické protokolování pro určitého zdroje typ, listy řídicí panel pro daný zdroj bude prázdná hodnota.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Zkontrolujte podrobnosti kolekce Azure sítě analýzy dat

Řešení Azure sítě analýzy shromažďuje protokolování diagnostiky z úložiště objektů Blob Azure pro Azure aplikace brány a skupiny zabezpečení, sítě.
Žádný agent je potřebný pro shromažďování dat.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro službu Azure sítě Analytics.

| Platformy | Přímé agent | Agent systémy Centrum Operations Manager (SCOM) | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | Kolekce četnosti |
|---|---|---|---|---|---|---|
|Azure|![Ne](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ne](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ano](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Ne](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ne](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minut|

## <a name="use-azure-networking-analytics"></a>Použití Azure analýzy sítě

Po nainstalování řešení, můžete zobrazit přehled klienta a chyby serveru pro vaše sledované aplikace brány pomocí **Analýzy sítě Azure** dlaždice na stránce **Přehled** v protokolu analýzy.

![Obrázek rohu dlaždice analýzy sítě Azure](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Po kliknutí na dlaždici **Přehled** můžete zobrazit souhrny protokolů a potom přejít na podrobnosti o těchto kategorií:

+ Přístup k aplikaci brány protokoly
  - Chyby klienta a serveru pro protokoly brány aplikace access
  - Žádosti o / hod pro jednotlivé aplikace brány
  - Nepodařilo žádosti o / hod pro jednotlivé aplikace brány
  - Chyby uživatelského agenta aplikace bran
+ Výkon brány aplikace
  - Host (hostitel) stav brány aplikace
  - Maximální a 95. percentil pro žádosti o aplikaci brány se nezdařila.
+ Skupina zabezpečení sítě blokované toků
  - Pravidla skupiny zabezpečení sítě s blokovaných toků
  - MAC adres pomocí blokovaných toků
+ Skupina zabezpečení sítě povolené toků
  - Pravidla skupiny zabezpečení sítě s povolené toků
  - MAC adresy se povolené toků


![Obrázek s řídicím panelem analýzy sítě Azure](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Obrázek s řídicím panelem analýzy sítě Azure](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Obrázek s řídicím panelem analýzy sítě Azure](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Obrázek s řídicím panelem analýzy sítě Azure](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Chcete-li zobrazit podrobnosti o protokolu shrnutí

1. Na stránce **Přehled** klikněte na dlaždici **Azure sítě analýzy** .
2. Na řídicím panelu **Azure sítě analýzy** zkontrolujte souhrnné informace v jednom listy a potom klikněte na jednu zobrazíte podrobné informace o ho na stránce log vyhledávání.

    Na jakékoli stránce protokolu hledání se výsledky zobrazují čas, podrobné výsledky a historie hledání protokolu. Můžete taky filtrovat podle fasetami k upřesnění výsledků.

## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data Azure sítě analýzy.
