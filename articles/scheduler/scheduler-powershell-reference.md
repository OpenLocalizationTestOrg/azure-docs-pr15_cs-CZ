<properties
 pageTitle="Odkaz rutiny prostředí PowerShell Plánovač"
 description="Odkaz rutiny prostředí PowerShell Plánovač"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Odkaz rutiny prostředí PowerShell Plánovač

Následující tabulka popisuje a odkazy na stránce přehled všech hlavní rutinách v Azure plánovač.

Instalace prostředí PowerShell Azure a přidružit Azure předplatné, najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). 

Další informace o [rutinách správce prostředků Azure](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).

|Rutina|Rutina popis|
|---|---|
[Jak zakázat AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Zakáže kolekce projektu. 
[Povolit AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Umožňuje kolekce projektu.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Získá Plánovač úloh.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Získá úlohy kolekcí.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Získá historie úlohy.
[Nové AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Vytvoří HTTP projektu.
[Nové AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Vytvoří kolekci projektu.
[Nové AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Vytvoří úlohy ve frontě bus služby.
[Nové AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Vytvoří úloha téma bus služby.
[Nové AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Vytvoří úlohy ve frontě úložiště. 
[Odebrat AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Odebere Plánovač úloh.  
[Odebrat AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Odebere kolekce projektu. 
[Nastavení AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Změní HTTP Plánovač úloh.
[Nastavení AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Změní kolekce projektu. 
[Nastavení AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Změní úlohy ve frontě bus služby.  
[Nastavení AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Změní úloha téma bus služby. 
[Nastavení AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Změní úlohy ve frontě úložiště.   

Podrobnější informace můžete použít některou z těchto rutin: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Viz taky


 [Co je Plánovač?](scheduler-intro.md)

 [Azure Plánovač koncepty, terminologie a entity hierarchie](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)
