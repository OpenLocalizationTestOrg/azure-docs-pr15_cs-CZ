<properties
   pageTitle="Základní informace o konfiguraci služby Azure struktury spolehlivé objekty actor ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Zjistěte, jak konfigurovat struktury služby Azure stavové účastníky typu ReliableDictionaryActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Konfigurace spolehlivé účastníky – ReliableDictionaryActorStateProvider
Výchozí konfigurace ReliableDictionaryActorStateProvider můžete upravit změnou settings.xml soubor vytvořený v kořenovém balíček aplikace Visual Studio ve složce Konfigurace pro zadané actor.

Modul runtime struktury služby Azure vyhledá názvy předdefinované oddílů v souboru settings.xml a zpracuje hodnoty konfigurace při vytváření základní runtime součásti.

>[AZURE.NOTE] Proveďte **nelze** odstranit nebo upravit názvy oddílů následující konfigurace v souboru settings.xml vytvořený ve Visual Studiu řešení.

Existují také globálních nastavení, které ovlivňují konfiguraci ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globální konfigurace

Globální konfigurace není zadán v clusteru instalované obrázku v části KtlLogger. Umožňuje konfiguraci umístění sdíleného protokolu a velikost plus limity globální paměti používaný protokolování. Všimněte si, že v manifest clusteru ovlivní všechny, které používají ReliableDictionaryActorStateProvider a spolehlivé stavové služby.

Manifest clusteru je soubor XML, který obsahuje nastavení a konfigurace, které platí pro všechny uzly a služeb v clusteru. Soubor se obvykle nazývá ClusterManifest.xml. Zobrazí se clusteru seznamu pomocí příkazu powershellu Get-ServiceFabricClusterManifest clusteru.

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
Protokolování má fond globální přidělené z paměti nestránkovaný jádra, která je dostupná ke všem službám spolehlivé na uzel pro ukládání do mezipaměti dat Stav před zápis vyhrazené protokol přidružený k otevřené spolehlivé služby paměti. Nastavením WriteBufferMemoryPoolMinimumInKB a WriteBufferMemoryPoolMaximumInKB řídí velikost fondu. WriteBufferMemoryPoolMinimumInKB Určuje počáteční velikost tohoto paměti fondu a velikost nejnižší, do kterého lze zmenšit fondu paměti. WriteBufferMemoryPoolMaximumInKB je nejvyšší velikost, které mohou dosáhnout fondu paměti. Každý otevřené spolehlivé služby, který je otevřen může zvětšit fondu paměti systém určený částku až WriteBufferMemoryPoolMaximumInKB. Pokud existuje více služba paměti z fondu paměti, než je k dispozici, lze žádosti o paměti zpozdit, dokud nebude k dispozici paměti. Proto je-li fondu zápisu rezervy paměti příliš malá pro konkrétní konfigurace potom výkonu může dojít k.

Nastavení SharedLogId a SharedLogPath vždy slouží společně k definování GUID a umístění sdílené protokolu výchozí pro všechny uzly clusteru. Výchozí sdílené protokol se používá pro všechny spolehlivé služby, které nejsou nastavení v settings.xml pro konkrétní službu. Nejlepších výsledků dosáhnete sdílené soubory protokolu se zařadí disků používaných výhradně k souboru sdíleného protokolu zmenšit konflikty.

SharedLogSizeInMB Určuje velikost místa na disku pro předběžné přidělení výchozí sdílené přihlášení všech uzlů.  SharedLogId a SharedLogPath nemusí být zadán v pořadí SharedLogSizeInMB být zadán.

## <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikace
Konfigurace zabezpečení replikace se používá k zabezpečení komunikace kanál, který se používá během replikace. To znamená, že služby nevidí vzájemně replikace přenosy, zajistit, že data, která je nastavená jako vysoce dostupný je taky zabezpečené.
Ve výchozím nastavení zabrání sekce konfigurace prázdné zabezpečení replikace zabezpečení.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfigurace replikace
Konfigurace replikace se používá ke konfiguraci replikace, která odpovídá týkající se uplatňování stavu poskytovatele stavu Actor vysoce spolehlivé replikace a uchování stavu místně.
Výchozí konfigurace je generováno aplikací Šablona Visual Studia a měli stačit. Tato část pojednává o další konfigurace, které jsou k dispozici pro optimalizaci replikace.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurace názvů

|Jméno|Jednotky|Výchozí hodnota|Poznámky:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekund|0.015|Časové období, pro kterou replikace na vedlejší čeká po přijetí operaci před odesláním zpět na primární odpověď na odeslanou zprávu. Další potvrzení nechat zasílat pro operace zpracování uvnitř tohoto intervalu se odesílají jako jedna odpověď.||
|ReplicatorEndpoint|NENÍ K DISPOZICI|Výchozí – potřeba parametr|Nastavení IP adresa a portu, které replikace primární a sekundární bude používat ke komunikaci s jiných replikátorů v otevřené. To by měl odkaz koncového bodu TCP zdroje v manifest služby. Podívejte se do [seznamu zdrojů služby](service-fabric-service-manifest-resources.md) pro další informace o definování zdroje koncového bodu v manifest služby. |
|MaxReplicationMessageSize|Bajtů|50 MB|Maximální velikosti doručovaných replikace dat, které lze přenášet v jedné zprávě.|
|MaxPrimaryReplicationQueueSize|Počet operací|8192|Maximální počet operací ve frontě primární. Operace se uvolnit nahoru po primární replikace přijímání sekundární replikátorů odpověď na odeslanou zprávu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|
|MaxSecondaryReplicationQueueSize|Počet operací|16384|Maximální počet operací ve frontě sekundární. Operace se uvolnit nahoru po provedení vysoce dostupné prostřednictvím trvalé jeho stavu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|
|CheckpointThresholdInMB|MB|200|Velikost po jejímž uplynutí stav je kontrolní místa v souboru protokolu.|
|MaxRecordSizeInKB|ZNALOSTNÍ BÁZI KNOWLEDGE BASE|1 024|Největší záznam velikosti replikace může zapisovat do protokolu. Tato hodnota musí být násobkem 4 a větší než 16.|
|OptimizeLogForLowerDiskUsage|Logická hodnota|PRAVDA|True, protokol nakonfigurovaný tak, aby otevřené vyhrazené soubor protokolu vytvořený s využitím sparse souboru NTFS. To je posunut skutečný místo na disku souboru. V případě hodnoty false soubor se vytvoří pomocí pevné přiřazení, které poskytují že ty nejcennější psaní výkonu.|
|SharedLogId|identifikátor GUID|""|Určuje jedinečný identifikátor guid používat k identifikaci sdílené protokolu používá se tento otevřené. Obvykle služby nepoužívejte toto nastavení. Ale pokud je zadána SharedLogId, pak SharedLogPath také je povinný.|
|SharedLogPath|Plně kvalifikovaný název cesty|""|Určuje úplnou cestu, kde bude vytvářeny sdílené protokolu pro tento otevřené. Obvykle služby nepoužívejte toto nastavení. Ale pokud je zadána SharedLogPath, pak SharedLogId také je povinný.|


## <a name="sample-configuration-file"></a>Ukázkový konfigurační soubor

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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

## <a name="remarks"></a>Poznámky:
Latence replikace BatchAcknowledgementInterval parametr ovládací prvky. Hodnotu "0" výsledkem nejnižší možné latence druhou stranu výkon (jako další potvrzovací zprávy musí být odeslány a zpracování každý obsahuje méně potvrzení).
Větší hodnotu BatchAcknowledgementInterval, tím vyšší celkové replikace výkon, druhou stranu vyšší latence operace. Převádí to přímo na latence transakce potvrzení.

Parametr CheckpointThresholdInMB řídí velikost místa na disku, replikace můžete použít k uložení informací o stavu v souboru protokolu vyhrazené otevřené. Zvětšení to na hodnotu vyšší než výchozí může vést k konfigurace bylo rychlejší po přidání nového otevřené na sadu. Toto je kvůli přenosu částečné stavu, která se provede kvůli dostupnost další historie operací v protokolu. Po zhroucení může potenciálně zvětšit obnovení čas otevřené.

Pokud jste nastavili OptimizeForLowerDiskUsage true (pravda), budou místo v souboru protokolu povolená zřizování tak, aby aktivní repliky mohou být uloženy další informace o stavu v jejich protokoly, zatímco neaktivní repliky použije méně místa na disku. Díky tomu hostovat další repliky na uzel. Pokud jste nastavili OptimizeForLowerDiskUsage NEPRAVDA, informace o stavu je došlo k zápisu souborů protokolu rychleji.

Nastavení MaxRecordSizeInKB definuje maximální velikosti doručovaných záznamy, které můžou být replikace napsal do souboru protokolu. Ve většině případů je optimální výchozí velikosti 1 024 KB záznamu. Ale pokud služby způsobuje větší datové položky jako součást informací o stavu, pak tuto hodnotu potřebovat prodloužit. Výhoda je malá při MaxRecordSizeInKB menší než 1 024, protože menší záznamů používat jenom prostor potřebný pro menší záznamu. Očekáváme, že tuto hodnotu by potřeba změnit pouze v některých případech.

Nastavení SharedLogId a SharedLogPath se vždy používají společně aby služba samostatné sdílené hovoru ze sdílené protokol výchozí určenou uzel. Pro nejlepších výsledků zadají tolik služby největšímu stejné sdílené protokolu. Sdílené soubory protokolu se zařadí disků používaných výhradně k souboru sdíleného protokolu zmenšit hlavy pohyb konflikty. Očekáváme, že by tyto hodnoty musí změnit pouze v některých případech.
