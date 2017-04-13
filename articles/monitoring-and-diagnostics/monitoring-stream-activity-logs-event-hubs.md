<properties
    pageTitle="Můžete vysílat datovými proudy protokolu Azure aktivity události rozbočovače | Microsoft Azure"
    description="Zjistěte, jak můžete vysílat datovými proudy protokolu Azure aktivity události rozbočovače."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Toku protokolu Azure aktivity události rozbočovače
[**Protokol činnosti Azure**](./monitoring-overview-activity-logs.md) můžete streamují v reálném čase nejblíže k libovolné aplikaci možnost předdefinované "Export" na portálu nebo povolením Id služeb Bus pravidla v protokolu profilu pomocí rutin prostředí PowerShell Azure nebo Azure rozhraní příkazového řádku.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Co můžete dělat s protokolu aktivity a události rozbočovače
Můžete použít funkci streamování pro protokol činnosti jen pomocí několika způsoby:

- **Toku systémy protokolování a telemetrie třetích stran** – s časem události rozbočovače streamování stane mechanismus kanálu protokolu činnosti do SIEMs třetích stran a přihlásit se analýzy řešení.
- **Vytvořit vlastní telemetrie a protokolování platformu** – Pokud jste už uživatelském telemetrie platformu nebo jsou jenom přemýšlíte o vytváření jednu, vysoce scalable publikovat přihlášením druh události rozbočovače umožňuje pružně jedí protokolu činnosti. [V příručce Dan Rosanova pomocí události rozbočovače telemetrie platformě globální měřítko tady.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Povolit streamování protokolu činnosti
Můžete povolit streamování protokolu činnosti programově nebo prostřednictvím portálu. Oběma způsoby, který jste si vybrali Namespace Bus služby a sdílené přístupu pro tento obor názvů a rozbočovači událostí v této názvů při vznikne první nového aktivitu protokolu událostí. Pokud nemáte Namespace Bus služby, musíte nejdřív a vytvořte si ho. Pokud jste dříve streamují protokolu aktivity události Namespace Bus této služby, budou znovu použít centru události, která byla dříve vytvořena. Sdílené přístupu definuje oprávnění, která obsahuje streamování mechanismus. Dnes datových proudů události rozbočovače vyžaduje oprávnění **Spravovat**, **Číst**a **odesílat** . Můžete vytvořit nebo upravit zásady přístupu sdílené Namespace Bus služby na portálu klasické klikněte v části "Konfigurovat" pro vaše Namespace Bus služby. Aktualizovat profil protokolu protokolu činnosti zahrnout streamování, musíte mít uživatele provádějícího změnu oprávnění ListKey na tuto službu Bus ověřovací pravidlo.

### <a name="via-azure-portal"></a>Prostřednictvím Azure portálu 
1. Přejděte na zásuvné **Protokolu činnosti** přes nabídku na levé straně na portálu.

    ![Přejděte do protokolu činnosti portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknutím na tlačítko **Exportovat** v horní části zásuvné.

    ![Tlačítko Exportovat portálu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. V zásuvné, která se zobrazí můžete vybrat oblasti, u kterých chcete toku událostí a Namespace Bus službu, ve kterém se mají rozbočovači události vytvořit streamování tyto události.

    ![Export zásuvné protokolu činnosti](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klepněte na tlačítko **Uložit** uložte toto nastavení. Nastavení okamžitě zaevidují do vašeho předplatného.


### <a name="via-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Pokud již existuje protokolu profilu, musíte nejdřív odebrat profilu.

1. Použití `Get-AzureRmLogProfile` zjistit, zda profil protokolu existuje
2. Pokud ano, pomocí `Remove-AzureRmLogProfile` ji odstraňte.
3. Použití `Set-AzureRmLogProfile` vytvořit profil:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

ID služeb Bus pravidla je řetězec s v tomto formátu: {služby bus pole číslo ID zdroje} /authorizationrules/ {název klíče}, třeba 

### <a name="via-azure-cli"></a>Přes Azure rozhraní příkazového řádku
Pokud již existuje protokolu profilu, musíte nejdřív odebrat profilu.

1. Použití `azure insights logprofile list` zjistit, zda profil protokolu existuje
2. Pokud ano, pomocí `azure insights logprofile delete` ji odstraňte.
3. Použití `azure insights logprofile add` vytvořit profil:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

ID služeb Bus pravidla je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Jak používat protokolu data z rozbočovače událostí?
[Tady je k dispozici schéma protokolu činnosti](./monitoring-overview-activity-logs.md). Každou událost je v argumentu pole objekty BLOB JSON s názvem "záznamy,."

## <a name="next-steps"></a>Další kroky
- [Archivace protokolu činnosti k účtu úložiště](./monitoring-archive-activity-log.md)
- [Přečtěte si základní informace o protokolu činnosti Azure](./monitoring-overview-activity-logs.md)
- [Nastavení upozornění na základě protokolu aktivity události](./insights-auditlog-to-webhook-email.md)
