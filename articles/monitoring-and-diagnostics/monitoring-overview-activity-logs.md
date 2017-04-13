<properties
    pageTitle="Základní informace o protokolu Azure činnosti | Microsoft Azure"
    description="Zjistěte, co je Azure protokolu činnosti a jak můžete použít k pochopení událostí v rámci předplatného Azure."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Základní informace o protokolu Azure činnosti
Protokol, který obsahuje přehled o operace, které byly provedeny zdrojů ve vašem předplatném je **Protokolu činnosti Azure** . Protokol činnosti se dřív jmenoval "Protokolů auditování" nebo "Provozní protokoly," od hlásí řídicí událostí pro vaše předplatné. Pomocí protokolu činnosti, můžete určit "co, kdo a kdy" pro některé operace (umístění, příspěvku DELETE) provedené zdrojů ve vašem předplatném zápisu. Můžete taky pochopit stav operace a další odpovídající vlastnosti. Protokol činnosti nezahrnuje operace čtení (GET).

Protokol činnosti se liší od [Diagnostických protokolů](./monitoring-overview-of-diagnostic-logs.md), které jsou všechny protokolů, které zdroje. Tyto protokoly zdrojem dat o fungování daný zdroj, nikoli operace na daný zdroj.

Načtete události z přihlašovací aktivitu na portálu Azure, rozhraní příkazového řádku, rutiny prostředí PowerShell a rozhraní REST API Azure Monitor.

Zobrazte toto [video úvodní informace o protokolu činnosti](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Co můžete dělat s protokolu činnosti
Tady jsou některé věci, které můžete dělat s protokolem aktivity:

- Dotaz a zobrazte **Azure portálu**.
- Dotaz se prostřednictvím rozhraní REST API, rutiny prostředí PowerShell nebo rozhraní příkazového řádku.
- [Vytvoření e-mailu nebo webhook upozornění, která aktivuje vypnout protokolu aktivity události.](./insights-auditlog-to-webhook-email.md)
- [Uložit, abyste ji k **Účtu úložiště** pro archivaci nebo ruční kontroly](./monitoring-archive-activity-log.md). Můžete zadat dobu uchovávání informací (ve dnech), používání **Profilů protokolu**.
- Analyzujete PowerBI pomocí [**PowerBI obsahu pack**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [Můžete vysílat datovými proudy ho k **Centrální událostí** ](./monitoring-stream-activity-logs-event-hubs.md) pro požití služby třetích stran nebo vlastní analytics řešení například PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Export protokol aktivit s profily protokolu
**Profil protokolu** určuje způsob exportu protokolu činnosti. Pomocí protokolu profilu, můžete nakonfigurovat:

- Kde protokolu činnosti mají zasílat (úložiště klienta nebo rozbočovače událostí)
- Mají zasílat kategorií, které události (Zapsat, odstranit, akce).
- By měly být exportovány oblasti, které (umístění)
- Jak dlouho by měl být protokolu činnosti jsou zachovány v úložiště účet – uchování nula dní znamená, že protokoly zachovány trvale. Hodnota jinak, může být libovolný počet dní mezi 1 a 2147483647. Pokud jsou nastavené zásady uchovávání informací, ale ukládání protokolů v účtu úložiště je vypnutá, (například pokud jen jsou vybrané možnosti rozbočovače událost nebo OMS), zásady uchovávání informací mít žádný vliv.

Toto nastavení můžete konfigurovat prostřednictvím možnost "Export" v protokolu činnosti zásuvné na portálu. Lze také nakonfigurované programově [pomocí rozhraní Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), rutiny prostředí PowerShell nebo rozhraní příkazového řádku. Přihlášení k odběru může obsahovat pouze jeden profil protokolu.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurace protokolu profilů pomocí portálu Azure
Můžete vysílat datovými proudy protokolu aktivity události centrální nebo ukládány v účtu úložiště pomocí možnosti "Export" Azure portálu.

1. Přejděte na zásuvné **Protokolu činnosti** přes nabídku na levé straně na portálu.

    ![Přejděte do protokolu činnosti portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknutím na tlačítko **Exportovat** v horní části zásuvné.

    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. V zásuvné, která se zobrazí můžete vybrat:  
    - oblasti, u kterých chcete exportovat události
    - Úložiště účet, ke kterému chcete ukládat události
    - počet dnů, které chcete zachovat tyto události v úložišti. Nastavení nulová zachová protokoly trvale.
    - Namespace Bus službu, ve kterém se mají rozbočovači události vytvořit streamování tyto události.

    ![Export zásuvné protokolu činnosti](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klepněte na tlačítko **Uložit** uložte toto nastavení. Nastavení okamžitě zaevidují do vašeho předplatného.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurace protokolu profilů pomocí rutin prostředí PowerShell Azure
#### <a name="get-existing-log-profile"></a>Získání existujícího profilu protokolu
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Přidání protokolu profilu
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Vlastnost         | Povinné | Popis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jméno             | Ano      | Název profilu protokolu.                                 |
| StorageAccountId | Ne       | Pole číslo ID zdroje účtu úložiště, do které má být protokolu činnosti uložen.                         |
| serviceBusRuleId | Ne       | ID pravidla Bus služby pro obor Bus služby rádi byste mít události rozbočovače vytvořené v. Je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`. |
| Umístění        | Ano      | Seznam oddělený čárkami oblastí, pro která chcete shromažďovat protokolu aktivity události.              |
| RetentionInDays  | Ano      | Počet dnů pro události, které by měl být zachován, 1 až 2147483647. Nulové hodnoty donekonečna udržovat ukládá protokoly (stále). |
| Kategorie       | Ne       | Seznam oddělený čárkami kategorií událostí, které by měl být shromáždit. Možné hodnoty jsou zapsat, odstraňovacích nebo akce.                                 |

#### <a name="remove-a-log-profile"></a>Odebrání protokolu profilu
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfigurace protokolu profilů pomocí rozhraní příkazového řádku platformě Azure
#### <a name="get-existing-log-profile"></a>Získání existujícího profilu protokolu
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
`name` Vlastnost by měl být název profilu protokolu.

#### <a name="add-a-log-profile"></a>Přidání protokolu profilu
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Vlastnost         | Povinné | Popis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jméno             | Ano      | Název profilu protokolu.                                 |
| storageId        | Ne       | Pole číslo ID zdroje účtu úložiště, do které má být protokolu činnosti uložen.                         |
| serviceBusRuleId | Ne       | ID pravidla Bus služby pro obor Bus služby rádi byste mít události rozbočovače vytvořené v. Je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`. |
| umístění        | Ano      | Seznam oddělený čárkami oblastí, pro která chcete shromažďovat protokolu aktivity události.              |
| retentionInDays  | Ano      | Počet dnů pro události, které by měl být zachován, 1 až 2147483647. Nulovou hodnotou jsou uloženy protokoly donekonečna udržovat (stále).     |
| kategorie       | Ne       | Hodnoty oddělené seznam kategorií událostí, které by měl být shromáždit. Možné hodnoty jsou zapsat, odstraňovacích nebo akce.                                 |

#### <a name="remove-a-log-profile"></a>Odebrání protokolu profilu
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Schématu události
Každou událost v protokolu činnosti obsahuje JSON blob podobně jako tento příklad:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
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
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Název elementu         | Popis             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| povolení        | Kulatý RBAC vlastnosti události. Obvykle obsahuje vlastnosti "akce", "role" a "rozsah".|
| volající               | E-mailovou adresu uživatele, který má provádět operace, deklarace nebo deklaraci identity hlavního názvu služby podle dostupnosti.|
| kanály             | Jednu z následujících hodnot: "Správce", "Operace"|
| correlationId        | Obvykle GUID ve formátu řetězce. Události, které sdílejí correlationId patří do stejné akce uber.   |
| Popis          | Statický text popis události.                              |
| eventDataId          | Jedinečný identifikátor události.    |
| ZDROJ_UDÁLOSTI          | Název služby Azure nebo infrastrukturu, která obsahuje generované Tato událost.    |
| požadavek protokolu HTTP          | Kulatý popisující žádost Http. Obvykle obsahuje "clientRequestId", "clientIpAddress" a "metoda" (HTTP. For example, umístění).                            |
| úroveň                | Úroveň události. Jednu z následujících hodnot: "Kritické", "Chyba", "Upozornění", "Informační" a "Podrobné"  |
| resourceGroupName    | Název skupiny zdrojů dotčeném zdroje.               |
| resourceProviderName | Název zdroje poskytovatele dotčeném zdroje             |
| resourceUri          | Pole číslo id zdroje dotčeném zdroje.                               |
| operationId          | Identifikátor GUID sdíleny události, které odpovídají jedné operace.         |
| Název operace        | Název operace.  |
| Vlastnosti           | Sada `<Key, Value>` dvojice (to znamená slovníku) popisující podrobnosti o události.                                |
| Stav               | Řetězec popisující stav operace. Některé běžné hodnoty jsou: V průběhu byl úspěšný, neúspěšný, aktivní, spuštěná vyřešený.                                |
| dílčí stav            | Obvykle stavový kód HTTP odpovídající zbývající volání však mohou také obsahovat ostatní řetězce s popisem podřízeného stavu, například tyto běžné hodnoty: OK (HTTP stavový kód: 200), které byly vytvořeny (HTTP stavový kód: 201), přípustném (HTTP stavový kód: 202), nevidí žádný obsah (HTTP stavový kód: 204), chybná požádat o (HTTP stavový kód: 400), nebyla nalezena (HTTP stavový kód: 404), konflikt (HTTP stavový kód : 409), vnitřní chyba serveru (HTTP stavový kód: 500), služba není k dispozici (HTTP stavový kód: 503), vypršel časový limit brány (HTTP stavový kód: 504). |
| eventTimestamp       | Časové razítko při generování události službou Azure zpracování požadavku odpovídající požadovanou událost.     |
| submissionTimestamp  | Časové razítko při události dostupná pro dotaz.             |
| subscriptionId       | ID Azure předplatného.  |
| nextLink             | Pokračování token vzdáleně další sady výsledků, když je rozdělen do víc odpovědí. Obvykle nutné, pokud existuje více než 200 záznamy.     |

## <a name="next-steps"></a>Další kroky
- [Další informace o protokolu činnosti (dřív to bylo protokolů auditování)](../resource-group-audit.md)
- [Toku protokolu Azure aktivity události rozbočovače](./monitoring-stream-activity-logs-event-hubs.md)
