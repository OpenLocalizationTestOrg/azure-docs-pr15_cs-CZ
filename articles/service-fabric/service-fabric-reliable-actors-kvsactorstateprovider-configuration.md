<properties
   pageTitle="Základní informace o konfiguraci služby Azure struktury spolehlivé objekty actor KVSActorStateProvider | Microsoft Azure"
   description="Zjistěte, jak konfigurovat struktury služby Azure stavové účastníky typu KVSActorStateProvider."
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
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurace spolehlivé účastníky – KVSActorStateProvider
Výchozí konfigurace KVSActorStateProvider můžete upravit změnou settings.xml soubor, který je vytvořený v Microsoft Visual Studio balíčku kořenové složce Konfigurace pro zadané actor.

Modul runtime struktury služby Azure vyhledá názvy předdefinované oddílů v souboru settings.xml a zpracuje hodnoty konfigurace při vytváření základní runtime součásti.

>[AZURE.NOTE] Proveďte **nelze** odstranit nebo upravit názvy oddílů následující konfigurace v souboru settings.xml vytvořený ve Visual Studiu řešení.

## <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikace
Konfigurace zabezpečení replikace se používá k zabezpečení komunikace kanál, který se používá během replikace. To znamená, že služby nevidí vzájemně replikace přenosy, zajistit, aby data, která je nastavená jako vysoce dostupné taky zabezpečené.
Ve výchozím nastavení zabrání sekce konfigurace prázdné zabezpečení replikace zabezpečení.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfigurace replikace
Konfigurace replikace konfigurace replikace, která je odpovědná týkající se uplatňování stavu poskytovatele stavu Actor vysoce spolehlivé.
Výchozí konfigurace je generováno aplikací Šablona Visual Studia a měli stačit. Tato část pojednává o další konfigurace, které jsou k dispozici pro optimalizaci replikace.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurace názvů

|Jméno|Jednotky|Výchozí hodnota|Poznámky:|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekund|0.015|Časové období, pro kterou replikace na vedlejší čeká po přijetí operaci před odesláním zpět na primární odpověď na odeslanou zprávu. Další potvrzení nechat zasílat pro operace zpracování uvnitř tohoto intervalu se odesílají jako jedna odpověď.|
|ReplicatorEndpoint|NENÍ K DISPOZICI|Výchozí – potřeba parametr|Nastavení IP adresa a portu, které replikace primární a sekundární bude používat ke komunikaci s jiných replikátorů v otevřené. To by měl odkaz koncového bodu TCP zdroje v manifest služby. Podívejte se do [seznamu zdrojů služby](service-fabric-service-manifest-resources.md) pro další informace o definování zdroje koncového bodu v manifest služby. |
|RetryInterval|Sekund|5|Časové období, po jejímž uplynutí replikace znovu přenáší zprávy neobdrží potvrzení operace.|
|MaxReplicationMessageSize|Bajtů|50 MB|Maximální velikosti doručovaných replikace dat, které lze přenášet v jedné zprávě.|
|MaxPrimaryReplicationQueueSize|Počet operací|1 024|Maximální počet operací ve frontě primární. Operace se uvolnit nahoru po primární replikace přijímání sekundární replikátorů odpověď na odeslanou zprávu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|
|MaxSecondaryReplicationQueueSize|Počet operací|2048|Maximální počet operací ve frontě sekundární. Operace se uvolnit nahoru po provedení vysoce dostupné prostřednictvím trvalé jeho stavu. Tato hodnota musí být větší než 64 a zadanou mocninu čísla 2.|

## <a name="store-configuration"></a>Konfigurace úložiště
Konfigurace úložiště přihlašovacích údajů se používá ke konfiguraci místní úložiště, který slouží k zachování stavu, který je replikace.
Výchozí konfigurace je generováno aplikací Šablona Visual Studia a měli stačit. Tato část pojednává o další konfigurace, které jsou k dispozici pro optimalizaci v místním úložišti.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurace názvů

|Jméno|Jednotky|Výchozí hodnota|Poznámky:|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Milisekund|200|Nastaví maximální dávky interval pro potvrzení trvalé místním úložišti.|
|MaxVerPages|Počet stránek|16384|Maximální počet verze stránek v místní uložení databáze. Určuje maximální počet zbývající transakce.|

## <a name="sample-configuration-file"></a>Ukázkový konfigurační soubor

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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
