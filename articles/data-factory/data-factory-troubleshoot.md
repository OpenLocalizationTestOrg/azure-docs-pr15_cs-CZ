<properties 
    pageTitle="Poradce při potížích Azure Data Factory" 
    description="Zjistěte, jak řešit problémy s použitím Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Poradce při potížích Data Factory
Tento článek obsahuje tipy pro jejich odstranění problémů při použití Azure Data Factory. Tento článek neobsahuje seznam všech možných problémech při používání služby, ale popisuje některé problémy a Obecné tipy pro jejich odstranění.   

## <a name="troubleshooting-tips"></a>Tipy pro odstraňování potíží

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Chyba: Předplatné není registrovaný používat názvů "Microsoft.DataFactory"
Pokud se zobrazí tato chyba, nebyla zprostředkovatele prostředků Azure Data Factory registrována na vašem počítači. Postupujte takto: 

1. Spusťte Azure Powershellu. 
2. Přihlaste se k účtu Azure pomocí následujícího příkazu.
        Přihlášení AzureRmAccount 
3. Spusťte tento příkaz k registraci poskytovatele Azure Data Factory.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problém: Neoprávněným při spuštění rutina Data Factory
Pravděpodobně používáte účet vpravo Azure nebo předplatného pomocí prostředí PowerShell Azure. Pomocí těchto rutin vyberte předplatného pomocí služby Azure PowerShell a vpravo Azure účtu. 

1. Přihlášení AzureRmAccount - použití správné uživatelské ID a heslo
2. Get-AzureRmSubscription - zobrazit všechna předplatná pro účet. 
3. Vyberte AzureRmSubscription &lt;název předplatného&gt; – vyberte správné předplatné. Použijte stejný jako ten, který použijete k vytváření továrny dat na portálu Azure.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Problém: Nepodařilo spustit brány Expresní nastavení pro správu dat z Azure portálu
Express nastavení brány pro správu dat vyžaduje aplikaci Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče. Pokud expresní instalaci nespustí, proveďte jednu z následujících akcí: 

- Pomocí aplikace Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.

    Pokud používáte Chrome, přejděte do [Chrome webového úložiště](https://chrome.google.com/webstore/)vyhledávání pomocí klíčových slov "ClickOnce", zvolte jednu z rozšíření ClickOnce a nainstalujte ji. 
    
    Stejně postupujte i Firefox (nainstalovat doplněk). Klikněte na tlačítko Otevřít nabídku na panelu nástrojů (tři vodorovné čáry v pravém horním rohu), klikněte na doplňky, vyhledávání pomocí klíčových slov "ClickOnce", zvolte jednu z rozšíření ClickOnce a nainstalujte ji. 

- Pomocí odkazu na **Ruční nastavení** zobrazují na stejné zásuvné na portálu. Tento postup použijte stáhnout instalační soubor a spustit ručně. Po úspěšném dokončení instalace, se zobrazí dialogové okno Konfigurace brány pro správu dat. Zkopírujte **klíč** z portálu obrazovku, můžete ho ve Správci konfigurace ručně bránu zaregistrujte se službou.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Problém: Nepodaří připojit k místní SQL Server 
Spuštění **Správce konfigurace brány pro správu dat** na počítači brány a použijte kartu **Poradce při potížích** na test připojení k serveru SQL Server z počítače brány. Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problém: Řezy vstupní jsou v někdy čekání stavu

Výseče může být ve **čekání** stavu kvůli různých důvodů. Jednou z běžných příčin je, že **externí** vlastnost není nastavená na **hodnotu true**. Všechny sady dat, která je vyrobeno mimo rozsah Azure Data Factory by měla být označena **externí** vlastnost. Tato vlastnost označuje, že data externích a ne zálohované tak, že všechny potrubí v rámci výroby data. Výseče dat jsou označeny jako **připravení** po data k dispozici v úložišti odpovídajících. 

Viz následující příklad pro používání **externích** vlastnosti. Volitelně můžete zadat **externalData*** při nastavování externí true (pravda).

Viz článek [datové sady](data-factory-create-datasets.md) podrobné informace o této vlastnosti.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Řešení chyby, přidat **externí** vlastnosti a oddílu volitelné **externalData** JSON definici vstupní tabulka a znovu vytvořit tabulku. 

### <a name="problem-hybrid-copy-operation-fails"></a>Problém: Hybridní kopírovat nezdaří
V tématu služby řízení [Poradce při potížích brány](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) zpráv kroky k řešení problémů s kopírování z úložiště místní data pomocí Brána pro správu dat. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problém: Na vyžádání HDInsight zřizujete dojde k chybě
Pokud chcete použít propojené služba typu HDInsightOnDemand, budete muset zadat linkedServiceName odkazující na úložiště objektů Blob Azure. Datové služby Factory používá tento úložiště uložit protokoly a podpůrné soubory pro svůj cluster na vyžádání HDInsight.  Někdy zřizujete clusteru HDInsight na vyžádání nezdaří a zobrazí se tato chyba:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Tato chyba obvykle označuje umístění účtu úložiště zadaného v linkedServiceName není ve stejném umístění Centrum dat místo, kam se děje zřizování HDInsight. Příklad: Pokud factory dat je západní USA a Azure úložiště je východoasijských USA, na vyžádání zřizování selže západní USA.

Kromě toho je druhá additionalLinkedServiceNames vlastnost JSON místo, kam lze zadat další úložiště účty v na vyžádání HDInsight. Tyto další úložiště propojené účty by měl být ve stejném umístění jako obrázku HDInsight nebo dojde k stejné chyby.

### <a name="problem-custom-net-activity-fails"></a>Problém: Vlastní aktivity .NET se nezdaří
Podrobný postup najdete v článku [ladění profilace s vlastní aktivitou](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Poradce při potížích s pomocí Azure portálu 

### <a name="using-portal-blades"></a>Pomocí portálu listy
Postup najdete v článku [Monitor kanálem k odesílání zpráv](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) . 

### <a name="using-monitor-and-manage-app"></a>Sledování a Správa aplikací
V tématu [sledování a správa potrubí factory dat pomocí sledování a Správa aplikací](data-factory-monitor-manage-app.md) podrobnosti. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Poradce při potížích s pomocí prostředí PowerShell Azure

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Poradce při potížích s chybou pomocí prostředí PowerShell Azure  
Podrobnosti najdete v části [Sledování Data Factory potrubí pomocí prostředí PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 