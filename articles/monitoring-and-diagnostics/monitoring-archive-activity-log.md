<properties
    pageTitle="Archivace protokolu Azure činnosti | Microsoft Azure"
    description="Zjistěte, jak chcete archivovat přihlašovací aktivity Azure pro dlouhodobá uchovávání informací v účtu úložiště."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Archivace protokolu Azure činnosti
V tomto článku ukážeme, jak pomocí portálu Azure rutiny prostředí PowerShell nebo různé platformy rozhraní příkazového řádku [**Protokolu činnosti Azure**](monitoring-overview-activity-logs.md) v účtu úložiště archivovat. Tato možnost je užitečná, pokud chcete zachovat aktivity přihlašovací delší než 90 dnů (úplnou kontrolu nad zásady uchovávání informací) pro auditování, statické analýzy nebo zálohování. Pokud potřebujete zachovat vaší události 90 dní nebo méně nepotřebujete nastavit archivace k účtu úložiště, protože protokolu aktivity události zachovány v Azure platformu 90 dní bez povolení archivace.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Než začnete, je potřeba [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) do kterého můžete archivovat protokolu činnosti. Důrazně doporučujeme používat existující úložiště účet, který obsahuje jiné, než monitorování dat uložených v něm tak, že můžete lépe řídit přístup k monitorování dat. Ale pokud jsou také archivace protokoly pro diagnostiku a metriky k účtu úložiště, může být smysl používat úložiště pro vaše aktivity Log zachovat všechny monitorování dat na jednom centrálním místě. Úložiště účet, který používáte musí být univerzální úložiště s účtem, ne účtu úložiště objektů blob.

## <a name="log-profile"></a>Protokol profilu
Archivace protokolu činnosti některým z následujících metod, nastavení **Protokolu profilu** pro předplatné. Profil protokolu definuje typ události, které jsou uložené nebo streamují a výstupy – centrální úložiště účtu a/nebo události. Definuje taky zásady uchovávání informací (počet dnů uchovávání) pro události uložené na účtu úložiště. Pokud zásady uchovávání informací je nastavený na nulu, ukládají donekonečna Udržovat události. V ostatních případech to můžete nastavit na libovolnou hodnotu 1 až 2147483647. [Můžete získat další informace o protokolu profily tady](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Archivace protokol aktivitu na portálu
1. Na portálu klikněte na odkaz **Protokol činnosti** na levém navigačním panelu. Pokud nevidíte odkaz pro protokol činnosti, nejdřív klepněte na odkaz **Další služby** .

    ![Přejděte na zásuvné protokolu činnosti](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. V horní části zásuvné klikněte na **Exportovat**.

    ![Klikněte na tlačítko Exportovat](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. V zásuvné, která se zobrazí zaškrtněte políčko pro **Export do úložiště účtu** a vyberte účet úložiště.

    ![Nastavení účtu úložiště](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Pomocí posuvníku nebo textové pole, definujte počet dní, ke kterým by měly být neustále protokolu aktivity události ve vašem účtu úložiště. Pokud chcete, aby vaše data zachován účtu úložiště donekonečna udržovat, nastavte toto číslo na nulovou hodnotu.
5. Klikněte na **Uložit**.

## <a name="archive-the-activity-log-via-powershell"></a>Archivace protokolu činnosti pomocí prostředí PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Vlastnost         | Povinné | Popis                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Ne       | Pole číslo ID zdroje účtu úložiště, na který mají ukládání protokolů aktivity.                                                                                                                                                                                                                        |
| Umístění        | Ano      | Seznam oddělený čárkami oblastí, pro která chcete shromažďovat protokolu aktivity události. Můžete zobrazit seznam všech oblastí [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [REST API Azure správy](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays  | Ano      | Počet dnů pro události, které by měl být zachován, 1 až 2147483647. Nulovou hodnotou jsou uloženy protokoly donekonečna udržovat (stále).                                                                                                                                                                                             |
| Kategorie       | Ano      | Seznam oddělený čárkami kategorií událostí, které by měl být shromáždit. Možné hodnoty jsou zapsat, odstraňovacích nebo akce.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Archivace protokolu činnosti prostřednictvím rozhraní příkazového řádku
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Vlastnost        | Povinné | Popis                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jméno            | Ano      | Název profilu protokolu.                                                                                                                                                                                                                                                                         |
| storageId       | Ne       | Pole číslo ID zdroje účtu úložiště, na který mají ukládání protokolů aktivity.                                                                                                                                                                                                                        |
| umístění       | Ano      | Seznam oddělený čárkami oblastí, pro která chcete shromažďovat protokolu aktivity události. Můžete zobrazit seznam všech oblastí [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [REST API Azure správy](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays | Ano      | Počet dnů pro události, které by měl být zachován, 1 až 2147483647. Nulové hodnoty donekonečna udržovat ukládat protokoly (stále).                                                                                                                                                                                             |
| kategorie      | Ano      | Seznam oddělený čárkami kategorií událostí, které by měl být shromáždit. Možné hodnoty jsou zapsat, odstraňovacích nebo akce.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Schéma úložiště protokolu činnosti
Jakmile jste nastavili archivace, kontejneru úložiště se vytvoří v účtu úložiště hned dojde k protokolu aktivity události. Objekty BLOB v kontejneru podle stejný formát přes protokol aktivity a protokoly pro diagnostiku. Struktura těchto objektů blob je:

> přehledy provozní protokoly/názvů = = výchozí/resourceId/PŘEDPLATNÁ / {ID předplatného} / y = {číselné čtyřmístný} / m = {dvojmístné číslo měsíce} / d = {Dvojmístné číselné den} / h = {hour}/m=00/PT1H.json Dvojmístné 24hodinový formát času

Například objektů blob název může být:

> insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Každý objektů blob PT1H.json obsahuje JSON objektů blob událostí, ke kterým došlo do jedné hodiny zadaný objektů blob (například h = 12). Během prezentace hodiny události připojeny k souboru PT1H.json při jejich vzniku. Minuta hodnotu (m = 00) je vždy 00, protože protokolu aktivity události jsou rozdělené jednotlivé objekty BLOB za hodinu.

V rámci souboru PT1H.json každou událost uložených v poli "záznamy" sledovat tento formát:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Název elementu    | Popis                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| čas            | Časové razítko při generování události službou Azure zpracování požadavku odpovídající požadovanou událost.    |
| resourceId      | Pole číslo ID zdroje dotčeném zdroje.                                                                          |
| Název operace   | Název operace.                                                                                         |
| kategorie        | Kategorie Akce, např. Zápis, čtení, akce.                                                               |
| resultType      | Typ atributu výsledku, např. Zahájení úspěch, nepovede,                                                            |
| resultSignature | Závisí na typu zdroje.                                                                                  |
| durationMs      | Doba trvání operace v milisekundách                                                                      |
| callerIpAddress | IP adresu uživatele, který má provádí operace, deklarace nebo deklaraci identity hlavního názvu služby podle dostupnosti.         |
| correlationId   | Obvykle GUID ve formátu řetězce. Události, které sdílejí correlationId patří do stejné akce uber.         |
| identity        | Kulatý JSON popisující autorizace a deklarované.                                                             |
| povolení   | Kulatý RBAC vlastnosti události. Obvykle obsahuje vlastnosti "akce", "role" a "rozsah".            |
| úroveň           | Úroveň události. Jednu z následujících hodnot: "Kritické", "Chyba", "Upozornění", "Informační" a "Podrobné" |
| umístění        | Oblast, ve kterém došlo k umístění (nebo globální).                                                             |
| Vlastnosti      | Sada `<Key, Value>` dvojice (tedy slovníku) popisující podrobnosti o události.                             |

> [AZURE.NOTE] Vlastnosti a použití te000126961 tyto vlastnosti se může lišit v závislosti zdroje.

## <a name="next-steps"></a>Další kroky
- [Stáhněte si objektů blob pro analýzu](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Toku protokolu aktivity události rozbočovače](monitoring-stream-activity-logs-event-hubs.md)
- [Přečtěte si něco víc o protokolu činnosti](monitoring-overview-activity-logs.md)
