<properties
    pageTitle="Technologie pro analýzu protokol přihlášení hledání rozhraní REST API | Microsoft Azure"
    description="Tato příručka kurz základní popisující, jak můžete hledat protokolu analýzy rozhraní REST API v sadě operace Management (OMS) a obsahuje příklady, které ukazují, jak používat příkazy."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Protokol analýzy protokolu hledání rozhraní REST API

Tato příručka kurz základní popisující, jak můžete použít REST API protokolu analýzy hledání v sadě operace Management (OMS) a obsahuje příklady, které ukazují, jak používat příkazy. Některé příklady v tomto článku odkazovat provozní přehledy, což je název předchozí verzi protokolu analýzy.

## <a name="overview-of-the-log-search-rest-api"></a>Základní informace o hledání protokolu rozhraní REST API

Rozhraní REST API protokolu analýzy hledání je RESTful a můžete k nim získat přístup prostřednictvím rozhraní API Azure správce prostředků. V tomto dokumentu najdete příklady kde rozhraní API prostřednictvím [ARMClient](https://github.com/projectkudu/ARMClient), nástroj příkazového řádku otevřít zdroj, který usnadňuje vyvolání rozhraní API Azure správce prostředků. Použití ARMClient a prostředí PowerShell je jednu z mnoha možností pro přístup k rozhraní API pro hledání protokolu analýzy. Další možností je používat modul Azure Powershellu pro OperationalInsights, který obsahuje rutiny pro přístup k hledání. Pomocí těchto nástrojů můžete využít Azure správce prostředků rozhraní API RESTful volat do pracovních prostorů OMS a spusťte hledání příkazy v nich. Rozhraní API bude výstupní výsledky hledání můžete ve formátu JSON umožňuje používat výsledky hledání v mnoha různými způsoby programově.

Správce prostředků Azure se dají použít přes [knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) , jakož i [Rozhraní REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Prohlédněte si propojené webové stránky Další informace.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Základní kurz protokolu analýzy hledání REST API

### <a name="to-use-the-arm-client"></a>Pokud chcete používat ARM klienta

1. Instalace [Chocolatey](https://chocolatey.org/), což je otevřít správce balíčku zdrojů pro Windows. Otevřete okno příkazového řádku jako správce a potom spusťte tento příkaz:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Nainstalujte ARMClient spuštěním následujícího příkazu:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Provádět jednoduché hledání pomocí ARMClient

1. Přihlášení k účtu Microsoft nebo investovat:

    ```
    armclient login
    ```

    Úspěšné přihlášení obsahuje seznam všech předplatných se stejným účtem dané:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Zobrazit pracovní prostory sadu správy operace:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Hovor úspěšně Get by výstup všech pracovních prostorů stejným k předplatnému:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Vytvoření proměnné hledání:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Hledat pomocí novou proměnnou hledání:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Přihlaste se příklady odkaz analýzy hledání REST API
Následující příklady ukazují, jak můžete používat rozhraní API pro hledání.

### <a name="search---actionread"></a>Vyhledávání – akce/přečtené

**Ukázková adresa Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Žádost o:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
V následující tabulce jsou popsané vlastnosti, které jsou k dispozici.

|**Vlastnost**|**Popis**|
|---|---|
|horní|Maximální počet výsledků vrátit.|
|zvýraznění|Obsahuje před a po parametry, obvykle pro zvýraznění odpovídající pole|
|Před|Zadání předpony souladu dané řetězci s odpovídajícími poli.|
|příspěvek|Přidá odpovídající polím zadaného řetězce.|
|dotaz|Vyhledávacího dotazu mohli shromažďovat a vrací výsledky.|
|zahájení|Na začátek časového intervalu chcete výsledky nalezen z.|
|ukončení|Na konec časového intervalu chcete výsledky nalezen z.|


**Odpověď:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Hledání / {ID} - akce/přečtené

**Žádost o obsah uložený hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Pokud hledání vrátí "Na zjištění stavu úkolů" stavem, dotazování aktualizované výsledky lze provést pomocí tohoto rozhraní API. Po 6 min výsledků hledání dojde ke ztrátě z mezipaměti a HTTP pryč, bude vrácena. Vrátí-li počátečním požadavku hledání "Úspěšné" stav okamžitě, nebude přidána do mezipaměti příčinou toto rozhraní API vrátí HTTP pryč, pokud dotazovaném. Obsah HTTP 200 výsledek bude ve formátu stejný jako počátečním požadavku hledání jenom s aktualizovanými hodnotami.

### <a name="saved-searches---rest-only"></a>Uložená hledání – pouze REST

**Žádost o seznam uložená hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Podporované metody: GET umístění odstranit

Podporované metody kolekce: GET

V následující tabulce jsou popsané vlastnosti, které jsou k dispozici.

|Vlastnost|Popis|
|---|---|
|ID|Jedinečný identifikátor.|
|Etag|Je **nutné pro opravu**. Server aktualizaci na každém zápisu. Hodnota musí být rovna hodnotě aktuální uložené nebo "*" aktualizovat. pro staré/neplatný hodnoty vrací 409.|
|Properties.Query|**Povinné**. Vyhledávacího dotazu.|
|properties.displayName|**Povinné**. Uživatelské jméno definovaný zobrazení dotazu. Pokud modelovat jako zdroj Azure, by to bylo značku.|
|Properties.category|**Povinné**. Vlastní kategorie dotazu. Pokud modelovat jako zdroj Azure to bude značku.|

>[AZURE.NOTE] Rozhraní API pro hledání protokolu analýzy nyní vrátí uživatel vytvořil uložená hledání při dotazování uložené vyhledávání v pracovním prostoru. Rozhraní API nevrátí uložená hledání poskytovanou řešení v současné době. Tato funkce se přidá k pozdějšímu datu.

### <a name="create-saved-searches"></a>Vytvoření uložená hledání

**Žádost o:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Odstranit uložené hledání

**Žádost o:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualizace uložené hledání

 **Žádost o:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadata - JSON pouze

Tady je způsob, jak je uvedeno v poli pro všechny typy protokolu dat shromážděných v pracovním prostoru. Například pokud chcete vědět, pokud typ události obsahuje pole s názvem počítače, pak toto je jedním ze způsobů vyhledat a potvrďte.

**Žádost o pole:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odpověď:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

V následující tabulce jsou popsané vlastnosti, které jsou k dispozici.

|**Vlastnost**|**Popis**|
|---|---|
|Jméno|Název pole.|
|displayName|Zobrazovaný název pole.|
|Typ|Typ hodnoty pole.|
|facetable|Kombinace aktuální "indexované", "uložené" a "podmínka" vlastnosti.|
|zobrazení|Aktuální "Zobrazit vlastnosti. True, pokud bude vidět do vyhledávacího pole.|
|ownerType|Menší typům pouze patřící do onboarded IP.|


## <a name="optional-parameters"></a>Volitelné parametry
Tyto informace popisuje volitelné parametry dostupné.

### <a name="highlighting"></a>Zvýraznění

Parametr "Zvýraznění" je volitelný parametr, které můžete použít požádat o podsystém hledání zahrnout sadu značky v odpovědi.

Tyto značky označují začátek a konec zvýrazněný text, který odpovídá za podmínek uvedených v vyhledávacího dotazu.
Můžete zadat počáteční a koncové značky, které bude při hledání využívány zalomení zvýrazněný termínů.

**Příklad vyhledávacího dotazu**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Ukázka výsledků:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Všimněte si, že nahoře výsledek obsahuje chybovou zprávu, která byla předponou a přidaným.

## <a name="computer-groups"></a>Skupiny počítačů

Počítač skupiny jsou jinak uložená hledání, jejichž výsledkem je více počítačů.  Skupiny můžete použít v jiné dotazy omezení výsledků na počítačích ve skupině.  Skupiny počítačů implementovaná jako uložený výsledek hledání se značkou skupiny s hodnotou počítače.

Následuje ukázka odpověď pro skupinu počítačů.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Načítání skupiny počítačů

Použijte metodu Get s ID skupiny k načtení skupinu počítačů.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Vytvoření nebo aktualizace počítače skupiny

Umožňuje metodu umístění s jedinečným uložené ID hledání vytvořit novou skupinu počítač. Pokud chcete použít existující ID skupiny počítače, budou změněny že jeden. Při vytváření skupiny v konzole OMS ID vytvořené ze skupiny a jméno.

Dotaz používaný definice skupiny musí vrátit nastavení počítače pro skupinu budou fungovat správně.  Je vhodné ukončit dotaz *| Jednoznačné počítače* zajistit, je vrácena správná data.

Definice uloženého hledání, musí obsahovat značku s názvem skupiny s hodnotou počítače hledání zařadit jako skupinu počítačů.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Odstranění skupiny počítačů

Použijte metodu odstranit s ID skupiny odstranit skupinu počítačů.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) k vytvoření dotazů pomocí vlastní pole kritérií.
