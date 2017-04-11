<properties
    pageTitle="Použití úložiště objektů blob pro služby IIS a tabulky úložiště pro události | Microsoft Azure"
    description="Protokol analýzy může číst protokoly Azure služeb, které psaní diagnostiky k úložišti tabulek nebo došlo k úložišti objektů blob zápisu protokolů IIS."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Použití úložiště objektů blob pro službu IIS úložiště tabulek události

Protokolu analýzy můžete přečíst v protokolech následující služby, které psaní diagnostiky k úložišti tabulek nebo došlo k úložišti objektů blob zápisu protokolů IIS:

+ Služba struktury clusterů (verze Preview)
+ Virtuálních počítačích
+ Web či pracovníka role

Před protokolu analýzy můžete shromáždit data pro tyto materiály, musí být povolený Azure diagnostiky.

Po povolení diagnostiky můžete používat portál Azure nebo prostředí PowerShell konfigurace protokolu analýzy na shromáždit protokoly.

Azure diagnostiky je Azure rozšíření, který umožňuje shromáždit data diagnostiky z kolegy role, web nebo virtuální počítač v Azure. Data se ukládá do účet Azure úložiště a lze shromažďovat podle protokolu analýzy.

Pro analýzu protokolu získat informace o těchto protokolování diagnostiky Azure musí být protokoly do těchto umístění:

| Typ protokolu | Pole Typ zdroje | Umístění |
| -------- | ------------- | -------- |
| Protokoly služby IIS | Virtuálních počítačích <br> Role web <br> Pracovní role | wad logfiles serveru iis (úložiště objektů Blob) |
| Syslog | Virtuálních počítačích | LinuxsyslogVer2v0 (tabulka úložiště) |
| Služba struktury funkční události | Služba struktury uzlů | WADServiceFabricSystemEventTable |
| Služba struktury spolehlivé Actor události | Služba struktury uzlů | WADServiceFabricReliableActorEventTable |
| Služba struktury spolehlivé služby události | Služba struktury uzlů | WADServiceFabricReliableServiceEventTable |
| Protokoly událostí systému Windows | Služba struktury uzlů <br> Virtuálních počítačích <br> Role web <br> Role kolegy | WADWindowsEventLogsTable (úložiště tabulek) |
| Protokoly systému Windows trasování událostí pro Windows | Služba struktury uzlů <br> Virtuálních počítačích <br> Role web <br> Pracovní role | WADETWEventTable (úložiště tabulek) |

>[AZURE.NOTE] Protokoly služby IIS z Azure webů aktuálně nepodporuje.

Pro virtuálních počítačích máte možnost instalace [protokolu analýzy agenta](log-analytics-azure-vm-extension.md) do virtuálního počítače povolit další přehledy. Kromě analyzovat protokoly událostí a protokolování služby IIS můžete provést další analýzu včetně sledování změn konfigurace, hodnocení SQL a aktualizovat hodnocení.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Povolení Azure Diagnostika v virtuálního počítače v protokolu událostí a IIS protokolu kolekce

Chcete-li povolit Azure Diagnostika v virtuálního počítače v protokolu událostí a IIS protokolu kolekce pomocí portálu Microsoft Azure pomocí následujícího postupu.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Chcete-li povolit Azure Diagnostika v virtuálního počítače pomocí portálu Azure

1. Nainstalujte agenta OM při vytváření virtuálních počítače. Pokud již existuje virtuální počítač, už instalaci agenta OM ověřte.
    - Na portálu Azure přejděte do virtuálního počítače, vyberte **Volitelná konfigurace** **diagnostických nástrojů** a nastavit **Stav** **na**.

    Po dokončení OM příponou Azure diagnostiky nainstalovanou a spuštěnou. Tuto linku je zodpovědný za shromažďování dat diagnostiky.

2. Povolení sledování a konfigurace protokolování událostí v existující OM. Můžete povolit diagnostiky na úrovni OM. Povolit diagnostických nástrojů a potom i konfiguraci protokolování událostí, proveďte následující kroky:
    1. Vyberte OM.
    2. Klikněte na tlačítko **Sledování**.
    3. Klikněte na **Diagnostika**.
    4. Nastavte **Stav** na **zapnuto**.
    5. Vyberte každý diagnostiky protokol, který chcete shromažďovat.
    7. Klikněte na **OK**.

Pomocí prostředí PowerShell Azure přesněji určením události, které jsou aby došlo k zápisu Azure úložiště. Podívejte se do [shromažďování dat pomocí napsali úložiště tabulek nebo protokoly služby IIS zapisovat objektů blob Azure diagnostiky](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Povolení Azure Diagnostika v roli webové služby IIS kolekce protokolu a události

Obecné pokyny o povolení Azure Diagnostika v nápovědě k [Jak chcete povolit Diagnostika v do cloudové služby](../cloud-services/cloud-services-dotnet-diagnostics.md) . Tyto kroky udělat tyto informace použít a upravit ho pro použití s protokolu analýzy.

Pomocí Azure diagnostiky povolené:

- Protokoly služby IIS jsou uložené ve výchozím nastavení se přenesených intervalech scheduledTransferPeriod přenos dat protokolu.
- Ve výchozím nastavení se nepřenesou protokoly událostí systému Windows.

### <a name="to-enable-diagnostics"></a>Chcete-li povolit diagnostických nástrojů

Povolit protokoly událostí systému Windows nebo změnit scheduledTransferPeriod konfigurace diagnostiky Azure pomocí konfiguračního souboru XML (diagnostics.wadcfg), jak je vidět v [Krok 4: vytvoření konfiguračního souboru diagnostických nástrojů a nainstalujte rozšíření](../cloud-services/cloud-services-dotnet-diagnostics.md)

Následující příklad konfiguračního souboru shromažďuje všech událostí a protokolování služby IIS z protokoly aplikací a systému:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Ujistěte se, že vaše ConfigurationSettings Určuje účet úložiště, jako v následujícím příkladu:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Hodnoty **název účtu** a **AccountKey** se nacházejí v portálu Azure na řídicím panelu úložiště účtu, klikněte v části Správa přístupových kláves. Protokol pro connection_string musí být **https**.

Po konfiguraci aktualizované diagnostiky se použije vaše cloudové služby, a zapisuje diagnostiky k základnímu úložišti Azure, pak budete chtít konfigurace protokolu analýzy.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Shromáždění protokolů z úložiště Azure pomocí portálu Azure

Používat portál Azure pro nastavení protokolu analýzy na shromáždit protokoly pro službu Azure následující:

+ Služba struktury clusterů
+ Virtuálních počítačích
+ Web či pracovníka role

Na portálu Azure přejděte do pracovního prostoru protokolu technologie pro analýzu a dělat tyto věci:

1. Klepněte na položku *protokoly účty úložiště*
2. Klikněte na *Přidat* úkol
3. Vyberte účet úložiště, který obsahuje protokolování diagnostiky
  - Tento účet může být klasické úložiště účet nebo účet správce prostředků Azure úložiště
4. Vyberte datový typ chcete shromáždit protokoly
  - Možnosti jsou služby IIS protokoly. Události. Syslog (Linux); Protokoly trasování událostí pro Windows. Služba struktury události
5. Hodnota pro zdroj naplní založené na datový typ a nelze změnit
6. Kliknutím na OK uložte konfigurace

Opakujte kroky 2 až 6 pro další úložiště účtech a datové typy podle potřeby protokolu analýzy shromažďovat.

Přibližně 30 minut se zobrazíte data z účtu úložiště v protokolu analýzy. Zobrazí se pouze data, která je došlo k zápisu úložiště po konfiguraci. Protokol analýzy nečte stávajících dat z účtu úložiště.

>[AZURE.NOTE] Na portálu ověřuje, zda zdroj existuje účtu úložiště nebo pokud nová data zapisuje.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Povolení Azure Diagnostika v virtuálního počítače v protokolu událostí a IIS protokolu kolekci pomocí prostředí PowerShell

Použití Powershellu ke čtení z Azure diagnostiky vzniku k úložišti tabulek pomocí postupu v [Konfiguraci protokolu technologie pro analýzu indexovat Azure diagnostiky](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) .

Pomocí prostředí PowerShell Azure přesněji určením události, které jsou aby došlo k zápisu Azure úložiště.
Další informace najdete v tématu [Povolení Diagnostika v Azure virtuálních počítačích](../virtual-machines-dotnet-diagnostics.md).

Můžete povolit a aktualizovat Azure diagnostiky pomocí tento skript Powershellu.
Můžete taky tento skript s vlastní konfiguraci protokolování.
Úprava skript, který chcete nastavit účet úložiště, služby název a název virtuálního počítače.
Skript používá rutiny pro klasické virtuálních počítačích.

Zkontrolujte následující ukázka skriptu, zkopírujte ho, ji podle potřeby upravit, uložit vzorku jako soubor skriptu prostředí PowerShell a spusťte skript.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Další kroky

- [Použití JSON souborů v úložišti objektů blob](log-analytics-azure-storage-json.md) číst protokoly z Azure služeb, které zápisu diagnostiky k úložišti objektů blob ve formátu JSON.
- [Povolení řešení](log-analytics-add-solutions.md) poskytují přehled o data.
- [Použití vyhledávací dotazy](log-analytics-log-searches.md) k analýze dat.
