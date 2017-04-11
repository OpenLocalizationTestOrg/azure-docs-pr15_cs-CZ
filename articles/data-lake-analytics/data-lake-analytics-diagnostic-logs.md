<properties 
   pageTitle="Zobrazení diagnostických protokolů jezera analýzy dat Azure | Microsoft Azure" 
   description="Pochopit, jak nastavit a přístup k protokoly pro diagnostiku pro analýzu dat jezera Azure " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Přístup k protokoly pro diagnostiku pro analýzu dat jezera Azure

Informace o tom, jak povolit diagnostické protokolování pro váš účet jezera analýzy dat a jak zobrazit protokoly shromažďované pro váš účet.

Organizace můžete povolit diagnostické protokolování pro svůj účet Azure dat jezera analýzy shromažďování dat aplikace access auditování záznamy. Tyto protokoly obsahují informace, jako:

* Seznam uživatelů, kterým přistupovat data.
* Jak často se přistupuje data.
* Jaká data se ukládají do účtu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Povolení předplatného Azure** dat jezera analýzy veřejné (verze Preview). V tématu [pokyny](data-lake-analytics-get-started-portal.md#signup).
- **Účet azure dat jezera analýzy**. Postupujte podle pokynů na [Začínáme s Azure dat jezera technologie pro analýzu pomocí portálu Azure](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Povolení protokolování

1. Přihlaste se do nového [Azure portálu](https://portal.azure.com).

2. Otevřete účtu jezera analýzy dat a zásuvné účtu vaší jezera analýzy dat, klikněte na **Nastavení**a klikněte na **Nastavení diagnostických**.

3. V **diagnostiky** zásuvné proveďte následující změny konfigurace protokolování diagnostiky.

    ![Povolení protokolování diagnostiky] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Povolení protokoly pro diagnostiku")

    * Nastavit **Stav** **na** protokolování diagnostiky povolte.
    * Je možné/proces úložiště data dvěma různými způsoby.
        * Vyberte **Export do události rozbočovači** toku dat protokolu událostí rozbočovači Azure. Tuto možnost použijte, pokud máte kanálu podřízené zpracování a analyzujte data příchozí protokoly můžete v reálném čase. Pokud vyberete tuto možnost, je nutné zadat podrobnosti rozbočovače události Azure, který chcete použít.
        * Vyberte **Export do účtu úložiště** pro ukládání protokolů účet Azure úložiště. Tuto možnost použijte, pokud chcete data archivovat. Pokud vyberete tuto možnost, je nutné zadat účet Azure úložiště, který chcete uložit protokoly, které mají.
    * Určete, jestli chcete získat protokoly auditování nebo žádost o protokoly popřípadě obojí.
    * Zadejte počet dní, u kterých musí být zachovány data.
    * Klikněte na **Uložit**.

Jakmile jste povolili diagnostiky nastavení, můžete se podívat na protokoly na kartě **Protokoly pro diagnostiku** .

## <a name="view-logs"></a>Zobrazení protokolů

Existují dva způsoby zobrazení dat protokolu pro váš účet dat jezera analýzy.

* Z okna Nastavení účtu jezera analýzy dat
* Z tohoto účtu Azure úložiště, kde je uložena data

### <a name="using-the-data-lake-analytics-settings-view"></a>Zobrazení nastavení technologie pro analýzu dat jezera

1. Vaše zásuvné **Nastavení** účtu jezera analýzy dat klikněte na **Protokoly pro diagnostiku**.

    ![Zobrazit diagnostické protokolování] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Zobrazení diagnostických protokolů") 

2. V zásuvné **Protokoly pro diagnostiku** byste měli vidět protokoly rozdělené do kategorií podle **Protokoly auditování** a **Požádat o**.
    * Žádost o protokoly zachytit každé rozhraní API žádost analýzy dat jezera účtu.
    * Protokolů auditování se podobají požádat o protokoly, ale obsahují mnohem podrobnější rozdělení operací analýzy dat jezera účtu. Například hovoru jednoho uložení rozhraní API v žádosti o protokolech může mít za následek více operací "Přidávacího" v protokolech auditování.

3. Klikněte na odkaz **Stáhnout** protokolu položky ke stažení protokoly.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Z Azure úložiště účtu, který obsahuje data protokolu

1. Otevřete zásuvné účet Azure úložiště přidružené jezera analýzy dat pro protokolování a klikněte na objekty BLOB. **Služba objektů Blob** zásuvné seznam dvěma kontejnery.

    ![Zobrazit diagnostické protokolování] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Zobrazení diagnostických protokolů")

    * Kontejner **přehledy. protokolů auditování** obsahuje protokolů auditování.
    * Kontejner **žádosti o přehledy protokoly** obsahuje protokoly žádost.

2. V těchto kontejnery jsou uložené protokoly ve skupinovém rámečku následující strukturu.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] `##` Položky v cestě obsahují rok, měsíc, den a hodinu, ve kterém byl vytvořen protokol. Jezera analýzy dat vytvoří jeden soubor každou hodinu, takže `m=` vždy obsahovat hodnotu `00`.

    Jako příklad může být úplnou cestu k protokolu auditování:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Podobně může být úplnou cestu k žádosti o protokolu:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Struktura protokolu

Protokoly auditování a žádost o jsou ve formátu JSON. V této části podívejte se na strukturu JSON požadavku na jsme protokolů auditování.

### <a name="request-logs"></a>Žádost o protokoly

Tady je ukázková položka v protokolu ve formátu JSON žádost. Každý objektů blob obsahuje jeden objekt kořenové s názvem **záznamy** , které obsahuje maticových objekty protokolu.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Žádost o protokolu schématu

| Jméno            | Typ   | Popis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| čas            | Řetězec | Časovým razítkem (UTC) protokolu                                              |
| resourceId      | Řetězec | Včasné ID prostředek, který operaci                            |
| kategorie        | Řetězec | Kategorie protokolu. Například, **požadavky**.                                   |
| Název operace   | Řetězec | Název operaci, která je přihlášení k lyncu. Například GetAggregatedJobHistory.              |
| resultType      | Řetězec | Stav operace, například 200.                                 |
| callerIpAddress | Řetězec | IP adresa klienta, díky žádosti                                |
| correlationId   | Řetězec | Id protokol. Tato hodnota mohou sloužit k seskupení souvisejících protokolu položek |
| identity        | Objekt | Identita, které vygenerovalo protokolu                                            |
| Vlastnosti      | JSON   | Přejděte k následující části (žádost o protokolu vlastnosti schéma) podrobnosti |

#### <a name="request-log-properties-schema"></a>Žádost o protokolu vlastnosti schématu

| Jméno                 | Typ   | Popis                                               |
|----------------------|--------|-----------------------------------------------------------|
| Vlastnost HttpMethod           | Řetězec | Metoda protokolu HTTP se použije pro operaci. Například potřebujete. |
| Cesta                 | Řetězec | Cesta operace proběhlo na                   |
| RequestContentLength | Funkce INT    | Délka obsahu žádost HTTP                    |
| ClientRequestId      | Řetězec | Id, které jednoznačně identifikuje požadavek              |
| Čas spuštění            | Řetězec | Čas přijetí žádosti serveru         |
| Čas_ukončení              | Řetězec | Čas odeslání odpověď na server              |

### <a name="audit-logs"></a>Protokolů auditování

Tady je ukázková položka v protokolu auditování ve formátu JSON. Každý objektů blob obsahuje jeden objekt kořenové s názvem **záznamy** , které obsahuje maticových objekty protokolu

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schéma protokolu auditování

| Jméno            | Typ   | Popis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| čas            | Řetězec | Časovým razítkem (UTC) protokolu                                              |
| resourceId      | Řetězec | Včasné ID prostředek, který operaci                            |
| kategorie        | Řetězec | Kategorie protokolu. Například **auditování**.                                      |
| Název operace   | Řetězec | Název operaci, která je přihlášení k lyncu. Například JobSubmitted.              |
| resultType | Řetězec | Dílčí stav pro stav úlohy (název operace). |
| resultSignature | Řetězec | Další informace o stavu úloh (název operace). |
| identity      | Řetězec | Uživatel, který požaduje operaci. Například susan@contoso.com.                                 |
| Vlastnosti      | JSON   | Přejděte k následující části (schéma vlastnosti protokolu auditování) podrobnosti |

> [AZURE.NOTE] __resultType__ __resultSignature__ na výsledek operaci zadejte informace a obsahovat pouze hodnotu, pokud má k dokončení. Například obsahují hodnoty __název operace__ obsahuje hodnotu __JobStarted__ nebo __JobEnded__.

#### <a name="audit-log-properties-schema"></a>Schéma vlastnosti protokolu auditování

| Jméno       | Typ   | Popis                              |
|------------|--------|------------------------------------------|
| JobId | Řetězec | ID přiřazené úkoly  |
| Název_úlohy | Řetězec | Název, který byl k dispozici pro danou úlohu |
| JobRunTime | Řetězec | Modul runtime umožňuje procesu úlohy |
| SubmitTime | Řetězec | Čas (UTC), která byla odeslána projektu |
| Čas spuštění | Řetězec | Při spuštění úlohy spuštění po odeslání (v UTC). |
| Čas_ukončení | Řetězec | Čas ukončení úkolu. |
| Paralelismus | Řetězec | Počet jednotek analýzy dat jezera požadované pro tuto úlohu při odeslání. |

> [AZURE.NOTE] __SubmitTime__, __čas spuštění__, __Čas_ukončení__ __paralelismus__ poskytovat informace o operaci a pouze obsahovat hodnotu operaci zahájit nebo dokončit. Například __SubmitTime__ obsahuje hodnotu po __název operace__ označuje __JobSubmitted__.

## <a name="process-the-log-data"></a>Proces dat protokolu

Azure jezera analýzy dat obsahuje výběru návod k obrázku a analýza dat protokolu. Vyhledání vzorku na [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Další kroky

- [Přehled analýzy jezera Azure dat](data-lake-analytics-overview.md)


