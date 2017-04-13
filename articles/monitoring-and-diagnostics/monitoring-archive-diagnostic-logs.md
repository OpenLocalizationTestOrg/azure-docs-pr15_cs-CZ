<properties
    pageTitle="Archivace Azure protokoly pro diagnostiku | Microsoft Azure"
    description="Zjistěte, jak archivace vaší Azure protokoly pro diagnostiku pro dlouhodobá uchovávání informací v účtu úložiště."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Archivace Azure protokoly pro diagnostiku
V tomto článku ukážeme, jak můžete pomocí portálu Azure rutiny prostředí PowerShell rozhraní příkazového řádku a rozhraní REST API archivovat [Protokoly pro diagnostiku Azure](monitoring-overview-of-diagnostic-logs.md) v účtu úložiště. Tato možnost je užitečná, pokud chcete zachovat diagnostických protokolů s zásad volitelné uchovávání informací pro auditování, statické analýzy nebo zálohování.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Než začnete, je potřeba [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) , do kterého můžete archivovat diagnostických protokolů. Důrazně doporučujeme používat existující úložiště účet, který obsahuje jiné, než monitorování dat uložených v něm tak, že můžete lépe řídit přístup k monitorování dat. Ale pokud jsou také archivace protokolu aktivity a diagnostiky metriky k účtu úložiště, může být vhodné použití tohoto účtu úložiště pro vaše protokoly pro diagnostiku i k synchronizaci všech monitorování dat na jednom centrálním místě. Úložiště účet, který používáte musí být univerzální úložiště s účtem, ne účtu úložiště objektů blob.

## <a name="diagnostic-settings"></a>Diagnostické nastavení
Archivace diagnostických protokolů některým z následujících způsobů zadejte příslušné **Diagnostiky nastavení** určitého zdroje. Diagnostické nastavení pro zdroj definuje kategorie zaznamenává jsou, která jsou uložená nebo streamují a výstupy – centrální úložiště účtu a/nebo události. Definuje taky zásady uchovávání informací (počet dnů uchovávání) pro události kategoriemi protokolu uložené na účtu úložiště. Pokud zásad uchovávání informací je nastavený na nulu, událostí pro danou kategorii protokolu jsou uloženy donekonečna udržovat (které je tedy stále). Zásady uchovávání informací v opačném případě lze libovolný počet dní mezi 1 a 2147483647. [Můžete získat další informace o diagnostiky nastavení](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Protokoly pro diagnostiku archiv na portálu

1. Na portálu klikněte na zásuvné zdroje pro zdroj, na které chcete povolit archivace protokoly pro diagnostiku.
2. V části **Sledování** v nabídce Nastavení zdroje vyberte **diagnostických nástrojů**.

    ![Sledování část nabídky zdroje](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Zaškrtněte políčko pro **Export do úložiště účtu**a potom vyberte účet úložiště. Pokud chcete, nastavte počet dnů uchovávání tyto protokoly pomocí **uchovávání informací (dní)** posuvníky. Uchování nula dní ukládá protokoly donekonečna udržovat.

    ![Diagnostické protokolování zásuvné](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Klikněte na **Uložit**.

Protokoly pro diagnostiku archivovány k tomuto účtu úložiště hned, jak je generováno nová data události.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Protokoly pro diagnostiku archivu pomocí rutin prostředí PowerShell

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Vlastnost         | Povinné | Popis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Ano      | Pole číslo ID zdroje zdroje, podle kterého chcete nastavit diagnostiky.                            |
| StorageAccountId | Ne       | Pole číslo ID zdroje účtu úložiště, na který mají ukládání diagnostických protokolů.                          |
| Kategorie       | Ne       | Hodnoty oddělené seznam kategorií protokolu povolit.                                                     |
| Povoleno          | Ano      | Boolean označující, zda jsou diagnostiky povolit nebo zakázat na tento zdroj.                  |
| RetentionEnabled | Ne       | Boolean označující, pokud jsou povoleny zásad uchovávání informací na tohoto zdroje.                            |
| RetentionInDays  | Ne       | Počet dní, ke kterým by měl být zachován události od 1 do 2147483647. Nulové hodnoty donekonečna udržovat ukládá protokoly. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Archivace protokolu činnosti prostřednictvím rozhraní příkazového řádku různé platformy

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Vlastnost         | Povinné | Popis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Ano      | Pole číslo ID zdroje zdroje, podle kterého chcete nastavit diagnostiky.                            |
| storageId        | Ne       | Pole číslo ID zdroje účtu úložiště, na který mají ukládání diagnostických protokolů.                          |
| kategorie       | Ne       | Hodnoty oddělené seznam kategorií protokolu povolit.                                                     |
| povoleno          | Ano      | Boolean označující, zda jsou diagnostiky povolit nebo zakázat na tento zdroj.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Protokoly pro diagnostiku archivu prostřednictvím rozhraní REST API
Návod, jak můžete nastavit diagnostiky nastavení pomocí rozhraní API Azure monitoru ZBÝVAJÍCÍ [naleznete v tomto dokumentu](https://msdn.microsoft.com/library/azure/dn931931.aspx) .

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schéma protokoly pro diagnostiku účtu úložiště
Jakmile jste nastavili archivace, vytvoří se kontejneru úložiště ve účtu úložiště hned, jak v jednom z protokolu kategorií, které jste zapnuli automatický přístup dojde k události. Objekty BLOB v kontejneru podle stejný formát přes protokoly pro diagnostiku a protokolu činnosti. Struktura těchto objektů blob je:

> přehledy - protokoly-{název kategorie protokolu} / = resourceId/PŘEDPLATNÁ / {ID předplatného} /RESOURCEGROUPS/ {název skupiny prostředků} /PROVIDERS/ {název poskytovatele zdroje} / {zdroje zadejte} / {název zdroje} / y = {číselné čtyřmístný} / m = {dvojmístné číslo měsíce} / d = {Dvojmístné číselné den} / h = {hour}/m=00/PT1H.json Dvojmístné 24hodinový formát času

Nebo více jednoduše

> přehledy - protokoly-{název kategorie protokolu} / resourceId = / {zdroj Id} / y = {číselné čtyřmístný} / m = {dvojmístné číslo měsíce} / d = {Dvojmístné číselné den} / h = {hour}/m=00/PT1H.json Dvojmístné 24hodinový formát času

Například objektů blob název může být:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Každý objektů blob PT1H.json obsahuje JSON objektů blob událostí, ke kterým došlo do jedné hodiny zadaný objektů blob (například h = 12). Během prezentace hodiny události připojeny k souboru PT1H.json při jejich vzniku. Minuta hodnotu (m = 00) je vždy 00, protože diagnostickém protokolu událostí jsou rozdělené jednotlivé objekty BLOB za hodinu.

V rámci souboru PT1H.json každou událost uložených v poli "záznamy" sledovat tento formát:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Název elementu  | Popis                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| čas          | Časové razítko při generování události službou Azure zpracování požadavku odpovídající požadovanou událost. |
| resourceId    | Pole číslo ID zdroje dotčeném zdroje.                                                                       |
| Název operace | Název operace.                                                                                      |
| kategorie      | Protokol kategorie události.                                                                                  |
| Vlastnosti    | Sada `<Key, Value>` dvojice (tedy slovníku) popisující podrobnosti o události.                            |

> [AZURE.NOTE] Vlastnosti a použití te000126961 tyto vlastnosti se může lišit v závislosti zdroje.

## <a name="next-steps"></a>Další kroky
- [Stáhněte si objektů blob pro analýzu](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Protokoly pro diagnostiku toku k rozbočovače události](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Přečtěte si něco víc o protokoly pro diagnostiku](monitoring-overview-of-diagnostic-logs.md)
