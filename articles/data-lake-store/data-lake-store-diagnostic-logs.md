<properties 
   pageTitle="Zobrazení diagnostických protokolů úložiště jezera dat Azure | Microsoft Azure" 
   description="Pochopit, jak nastavit a přístup k protokoly pro diagnostiku pro úložiště jezera dat Azure " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Přístup k protokoly pro diagnostiku pro úložiště jezera dat Azure

Informace o tom, jak povolit protokolování diagnostiky účtu úložiště jezera dat a jak zobrazit protokoly shromažďované pro váš účet.

Organizace můžete povolit diagnostické protokolování pro svůj účet úložiště jezera dat Azure shromažďování dat aplikace access auditování záznamy, které obsahuje informace, například seznam uživatelům přístup k datům, jak často se přistupuje data, jaká data se ukládají do klienta, atd.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Úložiště jezera dat azure účtu**. Postupujte podle pokynů na [Začínáme s úložiště jezera Azure dat na portálu Azure](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Povolení protokolování diagnostiky účtu úložiště jezera dat

1. Přihlaste se na novém [Portálu Azure](https://portal.azure.com).

2. Otevřete účtu úložiště jezera dat a od vašeho zásuvné účtu úložiště jezera dat klikněte na **Nastavení**a klikněte **Diagnostiky nastavení**.

3. V **diagnostiky** zásuvné proveďte následující změny konfigurace protokolování diagnostiky.

    ![Povolení protokolování diagnostiky] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Povolení protokoly pro diagnostiku")

    * Nastavit **Stav** **na** protokolování diagnostiky povolte.
    * Je možné/proces úložiště data dvěma různými způsoby.
        * Vyberte možnost Exportovat **do události centrální** toku dat protokolu událostí rozbočovači Azure. Největší pravděpodobností použijeme tuto možnost, pokud máte kanálu podřízené zpracování a analyzujte data příchozí protokoly v reálném čase. Pokud vyberete tuto možnost, je nutné zadat podrobnosti rozbočovače události Azure, který chcete použít.
        * Vyberte požadovanou možnost Exportovat **do účtu úložiště** pro ukládání protokolů účet Azure úložiště. Tuto možnost použijte, pokud chcete archivovat data, která bude dávkové zpracování k pozdějšímu datu. Pokud vyberete tuto možnost je nutné zadat účet Azure úložiště, který chcete uložit protokoly, které mají.
    * Určete, jestli chcete získat protokoly auditování nebo žádost o protokoly nebo obojí.
    * Zadejte počet dní, u kterých musí být zachovány data.
    * Klikněte na **Uložit**.

Jakmile jste povolili diagnostiky nastavení, můžete se podívat na protokoly na kartě **Protokoly pro diagnostiku** .

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Zobrazit diagnostické protokoly pro váš účet jezera úložiště dat

Existují dva způsoby zobrazení dat protokolu pro váš účet jezera úložiště.

* Z tohoto účtu úložiště jezera dat nastavení zobrazení
* Z tohoto účtu Azure úložiště, kde je uložena data

### <a name="using-the-data-lake-store-settings-view"></a>Zobrazení nastavení jezera úložiště dat

1. Vaše zásuvné **Nastavení** účtu úložiště jezera dat klikněte na **Protokoly pro diagnostiku**.

    ![Zobrazit diagnostické protokolování] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Zobrazení diagnostických protokolů") 

2. V zásuvné **Protokoly pro diagnostiku** byste měli vidět protokoly rozdělené do kategorií podle **Protokoly auditování** a **Požádat o**.
    * Žádost o protokoly zachytit každé rozhraní API žádost na účtu úložiště jezera dat.
    * Protokolů auditování se podobají požádat o protokoly, ale obsahují mnohem podrobnější rozdělení provedených v úložišti jezera účtu. Například hovoru jednoho uložení rozhraní API v žádosti o protokolech může mít za následek více operací "Připojit" v protokolech auditování.

3. Klikněte na odkaz **Stáhnout** před každý záznam pokud si chcete stáhnout protokoly.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Z Azure úložiště účtu, který obsahuje data protokolu

1. Otevřete zásuvné účet Azure úložiště přidružený k úložišti jezera pro protokolování a klikněte na objekty BLOB. **Služba objektů Blob** zásuvné seznamy dvěma kontejnery.

    ![Zobrazit diagnostické protokolování] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Zobrazení diagnostických protokolů")

    * Kontejner **přehledy protokolů auditování** obsahuje protokolů auditování.
    * Kontejner **žádosti o přehledy protokoly** obsahuje protokoly žádost.

2. V těchto kontejnery jsou uložené protokoly ve skupinovém rámečku následující strukturu.

    ![Zobrazit diagnostické protokolování] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Zobrazení diagnostických protokolů")

    Jako příklad může být úplnou cestu k protokolu auditování`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Obdobně, může být úplnou cestu k žádosti o protokolu`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Pochopení struktury dat protokolu

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| Název operace   | Řetězec | Název operaci, která je přihlášení k lyncu. Například getfilestatus.              |
| resultType      | Řetězec | Stav operace, například 200.                                 |
| callerIpAddress | Řetězec | IP adresa klienta, díky žádosti                                |
| correlationId   | Řetězec | Id protokol, který můžete použít k seskupení tak sady položek souvisejících protokolu |
| identity        | Objekt | Identita, které vygenerovalo protokolu                                            |
| Vlastnosti      | JSON   | Podrobnosti najdete v části pod                                                          |

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
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| Název operace   | Řetězec | Název operaci, která je přihlášení k lyncu. Například getfilestatus.              |
| resultType      | Řetězec | Stav operace, například 200.                                 |
| correlationId   | Řetězec | Id protokol, který můžete použít k seskupení tak sady položek souvisejících protokolu |
| identity        | Objekt | Identita, které vygenerovalo protokolu                                            |
| Vlastnosti      | JSON   | Podrobnosti najdete v části pod                                                          |

#### <a name="audit-log-properties-schema"></a>Schéma vlastnosti protokolu auditování

| Jméno       | Typ   | Popis                              |
|------------|--------|------------------------------------------|
| StreamName | Řetězec | Cesta operace proběhlo na  |


## <a name="samples-to-process-the-log-data"></a>Ukázky zpracuje dat protokolu

Úložiště jezera dat Azure poskytuje výběru o tom, jak procesu a analýza dat protokolu. Vyhledání vzorku na [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Viz taky

- [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)
- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)

