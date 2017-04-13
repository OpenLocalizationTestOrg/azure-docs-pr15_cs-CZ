<properties
 pageTitle="Limity Plánovač a výchozích hodnot"
 description="Plánovač limity a výchozích hodnot"
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

# <a name="scheduler-limits-and-defaults"></a>Plánovač limity a výchozích hodnot

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Plánovač kvóty, omezení, výchozí hodnoty a omezením

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Záhlaví x-ms žádost id

Každý žádost proti službu Plánovač vrátí odpověď záhlaví**x-ms žádost –**název – ID. Toto záhlaví obsahuje neprůhledné hodnoty, které jednoznačně identifikuje žádost.

Pokud žádost o konzistentní selhává a jste ověřili, že je správně úpravě žádost, mohli byste použít tuto hodnotu k Informujte ji o chybě společnosti Microsoft. Do sestavy zahrnout hodnota x-ms-žádost o – id, přibližnou okamžiku zadání požadavku, identifikátor předplatné, kolekce úlohy a/nebo úlohy a typ operaci, která pokus o žádosti.

## <a name="see-also"></a>Viz taky


 [Co je Plánovač?](scheduler-intro.md)

 [Azure Plánovač koncepty, terminologie a entity hierarchie](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)
