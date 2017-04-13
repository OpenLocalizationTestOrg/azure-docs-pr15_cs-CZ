<properties
   pageTitle="Základní informace o konfiguraci Azure struktury spolehlivé služby | Microsoft Azure"
   description="Zjistěte, jak konfigurovat stavové spolehlivé služby v struktury služby Azure."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Konfigurace stavové spolehlivé služby

Existují dvě sady konfigurace nastavení spolehlivé služeb. Jednu sadu je globální pro všechny spolehlivé služby v clusteru, zatímco ostatní nastavení je specifické pro určitý spolehlivé služby.

## <a name="global-configuration"></a>Globální konfigurace

Konfigurace globálního spolehlivé služby není zadán v clusteru instalované obrázku v části KtlLogger. Umožňuje konfiguraci umístění sdíleného protokolu a velikost plus limity globální paměti používaný protokolování. Manifest clusteru je soubor XML, který obsahuje nastavení a konfigurace, které platí pro všechny uzly a služeb v clusteru. Soubor se obvykle nazývá ClusterManifest.xml. Zobrazí se clusteru seznamu pomocí příkazu powershellu Get-ServiceFabricClusterManifest clusteru.

### <a name="configuration-names"></a>Konfigurace názvů

|Jméno|Jednotky|Výchozí hodnota|Poznámky:|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilobajty.|8388608|Minimální počet KB přidělit v režimu jádra protokolování zapsat vyrovnávací paměť fondu. Tento fond paměti se používá k ukládání do mezipaměti informace o stavu před psaní na disk.|
|WriteBufferMemoryPoolMaximumInKB|Kilobajty.|Neomezeno|Maximální velikost, ke kterému protokolování psaní vyrovnávací paměť fondu můžete zvětšit.|
|SharedLogId|IDENTIFIKÁTOR GUID|""|Určuje jedinečný identifikátor GUID používat k identifikaci výchozí sdílený soubor protokolu používají všechny spolehlivé služby ve všech uzlech clusteru určující, nikoli SharedLogId v jejich konkrétní konfiguraci služby. Pokud není zadán SharedLogId, pak SharedLogPath musí být také určen.|
|SharedLogPath|Plně kvalifikovaný název cesty|""|Určuje použití protokolu sdílené všech spolehlivé služeb ve všech uzlech clusteru určující, nikoli SharedLogPath v jejich konfigurace specifické úplnou cestu. Ale pokud je zadána SharedLogPath, pak SharedLogId také je povinný.|
|SharedLogSizeInMB|Megabajtů|8192|Určuje počet MB místa na disku staticky přidělit sdílené protokolu. Hodnota musí být 2048 nebo větší.|

### <a name="sample-cluster-manifest-section"></a>Ukázka obrázku seznamu oddílu
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Poznámky:
Protokolování má fond globální přidělené z paměti nestránkovaný jádra, která je dostupná ke všem službám spolehlivé na uzel pro ukládání do mezipaměti dat Stav před zápis vyhrazené protokol přidružený k otevřené spolehlivé služby paměti. Nastavením WriteBufferMemoryPoolMinimumInKB a WriteBufferMemoryPoolMaximumInKB řídí velikost fondu. WriteBufferMemoryPoolMinimumInKB Určuje počáteční velikost tohoto paměti fondu a velikost nejnižší, do kterého lze zmenšit fondu paměti. WriteBufferMemoryPoolMaximumInKB je nejvyšší velikost, které mohou dosáhnout fondu paměti. Každý otevřené spolehlivé služby, který je otevřen může zvětšit fondu paměti systém určený částku až WriteBufferMemoryPoolMaximumInKB. Pokud existuje více služba paměti z fondu paměti, než je k dispozici, lze žádosti o paměti zpozdit, dokud nebude k dispozici paměti. Proto pokud fondu zápisu vyrovnávací paměť příliš malý pro konkrétní konfigurace potom výkonu může dojít k.

Nastavení SharedLogId a SharedLogPath vždy slouží společně k definování GUID a umístění sdílené protokolu výchozí pro všechny uzly clusteru. Výchozí sdílené protokol se používá pro všechny spolehlivé služby, které nejsou nastavení v settings.xml pro konkrétní službu. Nejlepších výsledků dosáhnete sdílené soubory protokolu se zařadí disků používaných výhradně k souboru sdíleného protokolu zmenšit konflikty.

SharedLogSizeInMB Určuje velikost místa na disku pro předběžné přidělení výchozí sdílené přihlášení všech uzlů.  SharedLogId a SharedLogPath nemusí být zadán v pořadí SharedLogSizeInMB být zadán.


## <a name="service-specific-configuration"></a>Konfigurace specifické
Stavová spolehlivé služeb výchozí konfigurace můžete změnit pomocí balíček configuration (konfigurace) nebo implementaci služby (kód).

+ **Konfigurace** – konfigurace prostřednictvím balíček konfigurace dosáhnete změnou Settings.xml soubor vytvořený v kořenovém balíček aplikace Microsoft Visual Studio ve složce Konfigurace pro každou službu v aplikaci.
+ **Kód** – konfigurace pomocí kódu dosáhnete vytvořením ReliableStateManager pomocí objektu ReliableStateManagerConfiguration sadou příslušné možnosti.

Ve výchozím nastavení runtime modul Azure služby struktury vyhledá názvy předdefinované oddílů v souboru Settings.xml a zpracuje hodnoty konfigurace při vytváření základní runtime součásti.

>[AZURE.NOTE] **Not** odstranit názvy oddílů následující konfigurace v Settings.xml souboru, který generovaného ve Visual Studiu řešení, pokud chcete nakonfigurovat služby prostřednictvím kódu.
Přejmenování názvy balíčku blok nebo oddíl config budou vyžadovat změnu kódu, při konfiguraci ReliableStateManager.


### <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikace
Konfigurace zabezpečení replikace se používá k zabezpečení komunikace kanál, který se používá během replikace. To znamená, že služby nebude moct v tématu vzájemně replikace přenos, zajistit, aby data, která je nastavená jako vysoce dostupné taky zabezpečené. Ve výchozím nastavení zabrání sekce konfigurace prázdné zabezpečení replikace zabezpečení.

### <a name="default-section-name"></a>Výchozí název oddílu
ReplicatorSecurityConfig

>[AZURE.NOTE] Pokud chcete změnit tento název oddílu, přepište replicatorSecuritySectionName parametr konstruktoru ReliableStateManagerConfiguration při vytváření ReliableStateManager pro tuto službu.


### <a name="replicator-configuration"></a>Konfigurace replikace
Konfigurace replikace konfigurace replikace, která odpovídá týkající se uplatňování stavu stavové spolehlivé služby vysoce spolehlivé replikace a uchování stavu místně.
Výchozí konfigurace je generováno aplikací Šablona Visual Studia a měli stačit. Tato část pojednává o další konfigurace, které jsou k dispozici pro optimalizaci replikace.

### <a name="default-section-name"></a>Výchozí název oddílu
ReplicatorConfig

>[AZURE.NOTE] Pokud chcete změnit tento název oddílu, přepište replicatorSettingsSectionName parametr konstruktoru ReliableStateManagerConfiguration při vytváření ReliableStateManager pro tuto službu.


### <a name="configuration-names"></a>Konfigurace názvů
|Jméno|Jednotky|Výchozí hodnota|Poznámky:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekund|0.015|Časové období, pro kterou replikace na vedlejší čeká po přijetí operaci před odesláním zpět na primární odpověď na odeslanou zprávu. Další potvrzení nechat zasílat pro operace zpracování uvnitř tohoto intervalu se odesílají jako jedna odpověď.|
|ReplicatorEndpoint|NENÍ K DISPOZICI|Výchozí – potřeba parametr|Nastavení IP adresa a portu, které replikace primární a sekundární bude používat ke komunikaci s jiných replikátorů v otevřené. To by měl odkaz koncového bodu TCP zdroje v manifest služby. Podívejte se do [seznamu zdrojů služby](service-fabric-service-manifest-resources.md) pro další informace o definování zdroje koncového bodu v manifest služby. |
|MaxPrimaryReplicationQueueSize|Počet operací|8192|Maximální počet operací ve frontě primární. Operace se uvolnit nahoru po primární replikace přijímání sekundární replikátorů odpověď na odeslanou zprávu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|
|MaxSecondaryReplicationQueueSize|Počet operací|16384|Maximální počet operací ve frontě sekundární. Operace se uvolnit nahoru po provedení vysoce dostupné prostřednictvím trvalé jeho stavu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|
|CheckpointThresholdInMB|MB|50|Velikost po jejímž uplynutí stav je kontrolní místa v souboru protokolu.|
|MaxRecordSizeInKB|ZNALOSTNÍ BÁZI KNOWLEDGE BASE|1 024|Největší záznam velikosti replikace může zapisovat do protokolu. Tato hodnota musí být násobkem 4 a větší než 16.|
|MinLogSizeInMB|MB|0 (systém určený)|Minimální velikost transakční protokol. Protokol nebude moct pořizovat zkrátit velikost pod toto nastavení. 0 označuje, že replikace bude určovat, minimální velikost protokolu. Tuto hodnotu zvyšuje pravděpodobnost způsobem částečné kopie a přírůstková zálohování od pravděpodobnost odpovídající záznamů, které zkrácení je snížit.|
|TruncationThresholdFactor|Faktor|2|Určuje, jakou velikost protokol zkrácení spouštěný. Zkrácení mezní je určený podle MinLogSizeInMB vynásobené TruncationThresholdFactor. TruncationThresholdFactor musí být větší než 1. MinLogSizeInMB * TruncationThresholdFactor musí být menší než MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Faktor|4|Určuje, jakou velikost protokol otevřené začne se omezena. Omezení mezní hodnota (v MB), je určený podle Max ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Omezení mezní hodnota (v MB) musí být větší než mezní hodnota zkrácení (v MB). Zkrácení mezní (v MB) musí být menší než MaxStreamSizeInMB.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max souhrnné velikost (MB) záložní protokolů v řetězci dané protokol zálohování. Přírůstková záložní požadavky selže, pokud zálohování vygeneruje záložní protokol, který způsobí Akumulované záložní protokoly od příslušné úplné zálohování větší než velikost. V takovém případě potřebujete udělat úplné zálohování.|
|SharedLogId|IDENTIFIKÁTOR GUID|""|Určuje jedinečný identifikátor GUID používat k identifikaci sdílené protokolu používá se tento otevřené. Obvykle služby nepoužívejte toto nastavení. Ale pokud je zadána SharedLogId, pak SharedLogPath také je povinný.|
|SharedLogPath|Plně kvalifikovaný název cesty|""|Určuje úplnou cestu místo, kam se bude vytvořena sdílené protokolu pro tento otevřené. Obvykle služby nepoužívejte toto nastavení. Ale pokud je zadána SharedLogPath, pak SharedLogId také je povinný.|
|SlowApiMonitoringDuration|Sekund|300|Nastaví sledování interval spravované rozhraní API volání. Příklad: uživatel zadal záložní zpětné (funkce). Po uplynutí interval sestavy stavu upozornění odešle správci stavu.|

### <a name="sample-configuration-via-code"></a>Ukázka konfiguraci pomocí kódu
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Ukázkový konfigurační soubor
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Poznámky:
Latence replikace BatchAcknowledgementInterval ovládací prvky. Hodnotu "0" výsledkem nejnižší možné latence druhou stranu výkon (jako další potvrzovací zprávy musí být odeslány a zpracování každý obsahuje méně potvrzení).
Větší hodnotu BatchAcknowledgementInterval, tím vyšší celkové replikace výkon, druhou stranu vyšší latence operace. Převádí to přímo na latence transakce potvrzení.

Hodnota pro CheckpointThresholdInMB řídí velikost místa na disku, replikace můžete použít k uložení informací o stavu v souboru protokolu vyhrazené otevřené. Zvětšení to na hodnotu vyšší než výchozí může vést k konfigurace bylo rychlejší po přidání nového otevřené na sadu. Toto je kvůli přenosu částečné stavu, která se provede kvůli dostupnost další historie operací v protokolu. Po zhroucení může potenciálně zvětšit obnovení čas otevřené.

Nastavení MaxRecordSizeInKB definuje maximální velikosti doručovaných záznamy, které můžou být replikace napsal do souboru protokolu. Ve většině případů je optimální výchozí velikosti 1 024 KB záznamu. Ale pokud služby způsobuje větší datové položky jako součást informací o stavu, pak tuto hodnotu potřebovat prodloužit. Výhoda je malá při MaxRecordSizeInKB menší než 1 024, protože menší záznamů používat jenom prostor potřebný pro menší záznamu. Očekáváme, že by tato hodnota musí změnit pouze výjimečně.

Nastavení SharedLogId a SharedLogPath se vždy používají společně aby služba samostatné sdílené hovoru ze sdílené protokol výchozí určenou uzel. Pro nejlepších výsledků zadají tolik služby největšímu stejné sdílené protokolu. Sdílených souborů protokolu by měla proběhnout na discích, které se používají pouze pro soubor protokolu sdílené zmenšit hlavy pohyb konflikty. Očekáváme, že by tato hodnota musí změnit pouze výjimečně.

## <a name="next-steps"></a>Další kroky
 - [Ladění aplikace služby struktury ve Visual Studiu](service-fabric-debugging-your-application.md)
 - [Referenční informace pro vývojáře pro spolehlivé služby](https://msdn.microsoft.com/library/azure/dn706529.aspx)
