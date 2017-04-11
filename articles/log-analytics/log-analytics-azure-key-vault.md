<properties
    pageTitle="Azure klíč trezoru řešení v protokolu analýzy | Microsoft Azure"
    description="Řešení Azure klíč trezoru v protokolu analýzy můžete použít k prohlížení protokoly Azure klíč trezoru."
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
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure řešení trezoru klíč (verze Preview) v protokolu analýzy

>[AZURE.NOTE] Jedná se o [Náhled řešení](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Řešení Azure klíč trezoru v protokolu analýzy můžete použít k prohlížení Azure klíč trezoru AuditEvent protokoly.

Můžete povolit protokolování auditu událostí pro Azure klíč trezoru. Tyto protokoly jsou napsali k úložišti objektů Blob Azure místo, kam se lze potom indexovat protokolu analýzy pro hledání a analýzy.

## <a name="install-and-configure-the-solution"></a>Instalace a konfigurace řešení

Instalace a konfigurace řešení Azure klíč trezoru postupujte podle následujících pokynů:

1.  Povolení [protokolování diagnostiky pro klíč trezoru](../key-vault/key-vault-logging.md) prostředky, které chcete sledovat
2.  Konfigurace protokolu analýzy číst protokoly z úložiště objektů blob pomocí procesu podle [JSON souborů v úložišti objektů blob](../log-analytics/log-analytics-azure-storage-json.md).
3.  Povolte řešení trezoru klíč Azure pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Zkontrolujte podrobnosti kolekce dat Azure klíč trezoru

Azure řešení klíč trezoru shromažďuje protokolování diagnostiky z úložiště objektů blob Azure pro Azure klíč trezoru.
Žádný agent je potřebný pro shromažďování dat.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro Azure klíč trezoru.

| Platformy | Přímé agent | Agent systémy Centrum Operations Manager (SCOM) | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | Kolekce četnosti |
|---|---|---|---|---|---|---|
|Azure|![Ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ano](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minut|

## <a name="use-azure-key-vault"></a>Použití Azure klíčové trezoru

Po nainstalování řešení, můžete zobrazit přehled žádost, které stavy v čase pro svých sledovaných trezorů klíč pomocí **Azure klíč trezoru** dlaždice na stránce **Přehled** analýzy protokolu.

![Obrázek rohu dlaždice trezoru klíč Azure](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Po kliknutí na dlaždici **Přehled** můžete zobrazit souhrny protokolů a potom přejít na podrobnosti o těchto kategorií:

- Objem všechny operace klíčové trezoru v čase
- Nepodařilo operace svazky v čase
- Průměrnou latenci provozní operací
- Zlepšení kvality služby pro operace s číslem operacích, které trvat víc než 1 000 ms a seznam operacích, které trvat víc než 1 000 ms

![Obrázek s řídicím panelem trezoru klíč Azure](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Obrázek s řídicím panelem trezoru klíč Azure](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Chcete-li zobrazit podrobnosti pro všechny operace

1. Na stránce **Přehled** klikněte na dlaždici **Azure klíč trezoru** .
2. Na řídicím panelu **Azure klíč trezoru** zkontrolujte souhrnné informace v jednom listy a potom klikněte na jednu zobrazíte podrobné informace o ho na stránce log vyhledávání.

    Na jakékoli stránce protokolu hledání se výsledky zobrazují čas, podrobné výsledky a historie hledání protokolu. Můžete taky filtrovat podle fasetami k upřesnění výsledků.

## <a name="log-analytics-records"></a>Technologie pro analýzu záznamů protokolu

Řešení Azure klíč trezoru analyzuje záznamy, které mají typu **KeyVaults** , které byly shromážděny z [AuditEvent protokolování](../key-vault/key-vault-logging.md) diagnostiky Azure.  Vlastnosti těchto záznamů jsou v této tabulce.  

| Vlastnost | Popis |
|:--|:--|
| Typ | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | IP adresa klienta, který odeslal požadavek |
| Kategorie      | Klíč trezoru protokolů AuditEvent hodnotu jedné, která je dostupná.|
| CorrelationId | Volitelné GUID, který klienta předat sladit protokoly na straně klienta se protokoly na straně služby (klíč trezoru). |
| DurationMs | Doba trvání pro zpracování požadavku rozhraní REST API v milisekundách. Aby údaj o čase, měřit na straně klienta může se stát odpovídaly tentokrát nezahrnuje latence sítě. |
| HttpStatusCode_d | Vrácené žádost HTTP stavový kód |
| Id_s       | Jedinečné ID žádosti o |
| Identity_o | Identita z tokenu, který byl prezentovat při vytváření žádosti o rozhraní REST API. Je to obvykle "uživatel", "objekt zabezpečení služeb" nebo "uživatele + ID aplikace" kombinace stejně jako v případě žádost o výsledkem rutiny prostředí PowerShell Azure. |
| Název operace      | Název operace, jak je uvedeno v [Azure klíč trezoru protokolování](../key-vault/key-vault-logging.md)|
| OperationVersion      | Požadovaná klientem verze rozhraní REST API|
| RemoteIPLatitude | Zeměpisná šířka klienta, který odeslal požadavek |
| RemoteIPLongitude | Délky klienta, který odeslal požadavek |
| RemoteIPCountry | Zemi klienta, který odeslal požadavek  |
| RequestUri_s | Identifikátor URI žádosti o |
| Zdroje   | Název klíčové trezoru |
| ResourceGroup | Skupina zdroje klíčové trezoru |
| ResourceId | Číslo ID zdroje Azure správce prostředků. Klíč trezoru protokoly to je vždy ID klíč trezoru zdroje. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | Stav protokolu HTTP|
| ResultType      | Výsledek žádost rozhraní REST API|
| SubscriptionId | ID předplatného Azure předplatného obsahující trezoru klíč |


## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data Azure klíč trezoru.
