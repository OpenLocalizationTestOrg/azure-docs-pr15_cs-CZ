<properties
 pageTitle="Plánovač vysoké dostupnosti a spolehlivosti"
 description="Plánovač vysoké dostupnosti a spolehlivosti"
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
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Plánovač vysoké dostupnosti a spolehlivosti

## <a name="azure-scheduler-high-availability"></a>Vysoké dostupnosti Azure Plánovač

Jako základní služby Azure platformu Azure Plánovač vysoce neexistuje funkcí a nasazení geo nadbytečné služby a replikace geo místní projektu.

### <a name="geo-redundant-service-deployment"></a>Nasazení GEO nadbytečné služby

Azure Plánovač je k dispozici prostřednictvím uživatelského rozhraní v téměř všechny geo oblast, která v Azure dnes. Seznam oblastí, které je k dispozici v Azure Plánovač je [uvedený tady](https://azure.microsoft.com/regions/#services). Pokud datacentrem hostovanou oblast vykreslena není k dispozici, je funkce selhání Plánovač Azure tak, aby služba není k dispozici z jiného datového centra.

### <a name="geo-regional-job-replication"></a>Replikace GEO místní projektu

Nejen je plánovači Azure front-end je k dispozici pro žádosti o řízení, ale vlastní pracovní i replikovat geo. Po výpadku v jednom regionu Azure Plánovač selhání a zajišťuje bezchybnou projektu z jiného datového centra v párových zeměpisnou oblast.

Například pokud jste si vytvořili úlohy v Jižní centrální nám, Azure Plánovač automaticky zreplikuje této úlohy v Severní centrální nám. Po selhání v Jižní centrální nám Azure Plánovač zaručuje, spuštění úlohy z severní centrální US. 

![][1]

V důsledku toho Azure Plánovač zaručuje, datům nepřekročil stejné širší zeměpisnou oblast když Azure nepovede. Výsledkem je nemusí duplikovat práce jenom přidáte dostupnost – Azure Plánovač automaticky poskytuje možnosti Vysoká dostupnost pro vaše práce.

## <a name="azure-scheduler-reliability"></a>Azure Plánovač spolehlivosti

Azure Plánovač zaručuje vlastní vysoké dostupnosti a přistupují uživatel vytvořil projektům. Například práce může vyvolat HTTP koncový bod, který není k dispozici. Azure Plánovač se snaží přesto k provedení práce povede, tím, že alternativní možnosti řešení s chybou. Azure Plánovač to dělá dvěma způsoby:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurovat opakovat zásad prostřednictvím "retryPolicy"

Azure plánovač umožňuje konfigurace zásad opakovat. Ve výchozím nastavení Pokud úlohy nepovede, Plánovač pokusí úlohy znovu další čtyřikrát, každých 30 sekund. Můžete znovu nakonfigurovat tuto zásadu opakovat být více agresivní (například deseti časy každých 30 sekund) nebo čím větší (například dvakrát denní intervalech.)

Jako příklad ze kdy to vám mohou pomoci můžete vytvořit projekt, který spustí jednou týdně a vyvolá koncový bod HTTP. Pokud koncový bod HTTP určen několik hodin při spuštění práce, nemusí chcete počkat jeden další týden pro daný úkol znovu spustit, protože se nepovede i výchozí zásady opakovat. V takovém případě může překonfigurovat zásadu standardní opakovat (například) opakovat každé tři hodiny místo každých 30 sekund.

Další informace o konfiguraci zásady opakovat najdete v nápovědě k [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternativní konfigurovatelnost koncový bod prostřednictvím "errorAction"

Pokud cílový koncový bod pro danou úlohu Azure Plánovač zůstane není dostupný, Azure Plánovač přejde na alternativní koncový bod zpracování chyb provedli své zásady opakovat. Pokud máte nakonfigurované alternativní koncový bod zpracování chyb, Azure Plánovač jej spustí. Alternativní koncový bod vlastní úlohy se vysoce dostupné při selhání.

Jako příklad v diagramu pod Azure Plánovač následuje jeho opakovat zásady narazí New York webové služby. Po pokusy nezdaří, zjistí, pokud je náhradní. Poté přejde rovnou a spustí tvoří požadavky alternativní s stejné zásady opakovat.

![][2]

Všimněte si, že stejné zásady opakovat platí pro původní akce a akce alternativní chyby. Je také možné mít typ akce alternativní chyby akce se liší od typ akce hlavní akce. Třeba při hlavní akce může vyvolání koncový bod HTTP, akce chyba místo toho pravděpodobně fronty úložiště, bus frontě nebo služby bus téma akci, která znamená protokolování chyb.

Další informace o konfiguraci alternativní koncového bodu najdete v nápovědě k [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Viz taky

 [Co je Plánovač?](scheduler-intro.md)

 [Azure Plánovač koncepty, terminologie a entity hierarchie](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Jak vytvářet složité plány a Upřesnit opakování s Azure Plánovač](scheduler-advanced-complexity.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
