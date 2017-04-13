<properties 
    pageTitle="Správa účtů Azure Media Services pomocí prostředí PowerShell" 
    description="Naučte se spravovat účty Azure Media Services pomocí rutin prostředí PowerShell." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Správa účtů Azure Media Services pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Portál](media-services-portal-create-account.md)
- [Prostředí PowerShell](media-services-manage-with-powershell.md)
- [ZBÝVAJÍCÍ](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Abyste mohli vytvořit účet Azure Media Services, musíte mít účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Bezplatnou zkušební verzi Azure</a>.

##<a name="overview"></a>Základní informace 

Tento článek obsahuje seznam rutiny prostředí PowerShell Azure pro Azure Media Services (AMS) v rámci správce prostředků Azure. Rutiny existovat v oboru **Microsoft.Azure.Commands.Media** .

## <a name="versions"></a>Verze

**ApiVersion**: "2015 10 01"
               

## <a name="new-azurermmediaservice"></a>Nové AzureRmMediaService

Vytvoří službu média.

### <a name="syntax"></a>Syntaxe

Nastavení parametrů: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Nastavení parametrů: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy | žádná
---|---
Povinné?   |  PRAVDA
Umístění?   |  0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky?  |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |Jméno
---|---
Povinné? |PRAVDA
Umístění? |1
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |NEPRAVDA
Použít zástupné znaky? |NEPRAVDA

**– Umístění &lt;řetězec&gt;**

Určuje umístění zdroje služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |2
Výchozí hodnota  |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**-StorageAccountId &lt;řetězec&gt;**

Určuje primární účet, který přidružený ke službě média.

- Nový účet úložiště jsou (vytvořená pomocí rozhraní API Správce prostředků) podporované jenom.

- Účet úložiště musí existovat a má na stejném místě se službou média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |3
Výchozí hodnota  |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Parametr nastaven název |StorageAccountIdParamSet
Použít zástupné znaky?|NEPRAVDA

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje úložiště účty, které přidružený ke službě média.

- Nový účet úložiště jsou (vytvořená pomocí rozhraní API Správce prostředků) podporované jenom.

- Účet úložiště musí existovat a má na stejném místě se službou média.

- Jen s jedním klientem úložiště může být zadán jako primární.

Aliasy |žádná
---|---
Povinné?  |PRAVDA
Umístění?  |3
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Parametr nastaven název |StorageAccountsParamSet
Použít zástupné znaky? |NEPRAVDA

**-Značky &lt;Hashtable&gt;**

Určuje hash tabulku značky, které jsou přidružené ke službě média.

- Příklad:@{"tag1"="value1";"tag2"=:value2"}

Aliasy |žádná
---|---
Povinné?  |NEPRAVDA
Umístění?  |s názvem
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |NEPRAVDA
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="set-azurermmediaservice"></a>Nastavení AzureRmMediaService

Aktualizace služby média.

### <a name="syntax"></a>Syntaxe

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné?  |PRAVDA
Umístění?  |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |Jméno
---|---
Povinné? |PRAVDA
Umístění? |1
Výchozí hodnota |Žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje úložiště účty, které přidružený ke službě média.

- Nový účet úložiště jsou (vytvořená pomocí rozhraní API Správce prostředků) podporované jenom.

- Účet úložiště musí existovat a má na stejném místě se službou média.

- Jen s jedním klientem úložiště může být zadán jako primární.

Aliasy |žádná
---|---
Povinné? |NEPRAVDA
Umístění? |S názvem
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Parametr nastaven název |StorageAccountsParamSet
Použít zástupné znaky? |NEPRAVDA

**-Značky &lt;Hashtable&gt;**

Určuje hash tabulku značky, které jsou přidružené k této službě média.

- Značky, které jsou přidružené ke službě médií nahrazeny hodnotami nastavil zákazníka.

Aliasy |žádná
---|---
Povinné? |NEPRAVDA
Umístění?  |S názvem
Výchozí hodnota |Žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="remove-azurermmediaservice"></a>Odebrat AzureRmMediaService

Odebere službu média.

### <a name="syntax"></a>Syntaxe

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |2
Výchozí hodnota |Žádná
Zadávají kanálem k odesílání zpráv?  |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Získá všechny služby médií ve skupině prostředků nebo službu médií s křestní jméno.

### <a name="syntax"></a>Syntaxe

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění?  |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Parametr nastaven název |ResourceGroupParameterSet AccountNameParameterSet
Použít zástupné znaky?   NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění?  |1
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Parametr nastaven název  |AccountNameParameterSet
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Získá zkratky médií služby.

### <a name="syntax"></a>Syntaxe

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění?  |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |1
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="set-azurermmediaservicekey"></a>Nastavení AzureRmMediaServiceKey

Znovu vytvoří primární a sekundární klíč služby média.

### <a name="syntax"></a>Syntaxe

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné?  |PRAVDA
Umístění?  |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv?  |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění?  |1
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv?   |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Typ klíče - &lt;typ klíče&gt;**

Určuje typ klíče služby média.

- Primární a sekundární

Aliasy |žádná
---|---
Povinné?  |PRAVDA
Umístění?  |2
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |NEPRAVDA
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="sync-azurermmediaservicestoragekeys"></a>Synchronizace AzureRmMediaServiceStorageKeys

Synchronizuje klávesy účtu úložiště pro účet úložiště přidružený ke službě média.

### <a name="syntax"></a>Syntaxe

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, ke kterému patří tuto službu média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |0
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**Název účtu - &lt;řetězec&gt;**

Určuje název služby média.

Aliasy |žádná
---|---
Povinné? |PRAVDA
Umístění? |1
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**-StorageAccountId &lt;řetězec&gt;**

Určuje úložiště účet spojený se službou média.

Aliasy |ID
---|---
Povinné? |PRAVDA
Umístění?  |2
Výchozí hodnota |žádná
Zadávají kanálem k odesílání zpráv? |      true(ByPropertyName)
Použít zástupné znaky? |NEPRAVDA

**&lt;Parametry_příkazu&gt;**

Tato rutina podporuje běžné parametry:-ladění - ErrorAction - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer - PipelineVariable-podrobného, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Zadávání

Zadávání typ je typ objekty, které můžete kanálu rutině.

### <a name="outputs"></a>Výstupy

Typ výstupu je typ objekty, které posílá rutiny.

## <a name="next-step"></a>Další krok 

Podívejte se na Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
