<properties
 pageTitle="Co je Azure Plánovač? | Microsoft Azure"
 description="Azure plánovač umožňuje deklarativně popište akce spustit v cloudu. Ho pak naplánuje a tyto akce se spustí automaticky."
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
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Co je Azure Plánovač?

Azure plánovač umožňuje deklarativně popisují akce, které chcete spustit v cloudu. Ho pak naplánuje a tyto akce se spustí automaticky.  Plánovač to dělá pomocí [portálu Azure](scheduler-get-started-portal.md), kód, [Rozhraní REST API](https://msdn.microsoft.com/library/mt629143.aspx)nebo Azure Powershellu.

Plánovač vytvoří udržuje a vyvolá plánované práce.  Plánovač nevytvářejte hostovat libovolnou pracovního vytížení a spusťte jakýkoli kód. IT pouze _vyvolá_ kód hostovaný někde jinde – v Azure, místní, nebo u jiného poskytovatele. Spustí prostřednictvím protokolu HTTP, HTTPS, fronty úložiště, bus frontě nebo tématu bus služby.

Plánovač plánují [úlohy](scheduler-concepts-terms.md), uchovávat historii výsledky spuštění úlohy jeden můžete zkontrolovat a deterministically a problémy se spolehlivým plánuje pracovního vytížení proběhnout. Azure WebJobs (součást funkci Web Apps v aplikaci služby Azure) a další možnosti plánování Azure pomocí Plánovač na pozadí. [Plánovač REST API](https://msdn.microsoft.com/library/mt629143.aspx) pomáhá spravovat komunikace pro tyto akce. Jako takové Plánovač podporuje [složité plány a Upřesnit opakování](scheduler-advanced-complexity.md) snadno.

Existuje několik příčin, které sami půjčovat použití plánovač. Příklad:

+ _Opakovaný aplikace akce:_ Pravidelně shromažďování dat z Twitter do informačního kanálu.
+ _Každodenní údržbu:_ Denní vyřazování protokoly, zálohování a jiných údržbu. Například může správce vyberte zálohovat databázi na 1:00 každý den dalšího devět měsíců.

Plánovač umožňuje vytvářet, aktualizovat, odstranit, zobrazit a spravovat úlohy a [úlohy kolekcí](scheduler-concepts-terms.md) programově pomocí skriptů a na portálu.

## <a name="see-also"></a>Viz taky

 [Azure Plánovač koncepty, terminologie a hierarchie entit](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Jak vytvářet složité plány a Upřesnit opakování s Azure Plánovač](scheduler-advanced-complexity.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)
