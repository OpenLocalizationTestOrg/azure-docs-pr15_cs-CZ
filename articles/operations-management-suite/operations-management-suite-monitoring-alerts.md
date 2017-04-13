<properties 
   pageTitle="Upozornění správy v aplikaci Microsoft sledování produkty | Microsoft Azure"
   description="Upozornění označuje nějaký problém, který vyžaduje pozornost od správce.  Tento článek popisuje rozdíly ve způsobu vytváří a spravuje v systému Centrum Operations Manager (SCOM) a technologie pro analýzu protokolu upozornění a poskytuje doporučené postupy ve využívání dva prostředky pro strategie upozornění hybridní." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Správa upozornění se sledováním společnosti Microsoft 

Upozornění označuje nějaký problém, který vyžaduje pozornost od správce.  Existuje výrazných rozdílů mezi System Center operace správce (SCOM) a technologie pro analýzu protokolu v sadě správy operace (OMS) z hlediska vytvoření upozornění, jak jsou spravovat a analyzovat, a jak budete upozorněni, že byl zjištěn problém s kritické.

## <a name="alerts-in-operations-manager"></a>Upozornění v Operations Manager
Upozornění v SCOM vznikají jednotlivá pravidla nebo monitorů označíte určitý problém.  Monitor generovat upozornění, pokud zadá chybovém stavu, při pravidla může způsobit upozornění označíte některé důležité problém, který nesouvisí přímo na stav spravovanému objektu.  Správa sad obsahují celou řadu pracovních postupů, které vytvoření upozornění pro aplikaci nebo službu, které spravují.  Součást procesu konfigurace nové management pack je optimalizace ho zajistit, že nechcete dostávat nadbytečné upozornění pro problémy, které nechcete zvažte kritické.

![Zobrazit upozornění SCOM](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM nabízí kompletní upozornění správy upozornění s stav, který bude možné měnit správci jak fungují tento problém vyřešit.  Po vyřešení problému správcem upozornění na uzavřený, kdy se už nezobrazuje v zobrazení zobrazení aktivní upozornění.  Upozornění na vytvořené z monitory automaticky vyřešením při monitor vrátí správný stavu.

![Stav výstrahy](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Upozornění v protokolu analýzy
Upozornění v protokolu analýzy vytvořené z protokolu dotazu, který se automaticky spustí v pravidelných intervalech.  Vytvoření pravidla výstrahy z libovolné protokolu dotazu.  Pokud dotaz vrací výsledky, které splňují daná kritéria, které zadáte, se vytvoří upozornění.  Může to být konkrétní dotaz, který vytváří upozornění, pokud je zjištěno zvláštní událost nebo můžete použít obecnější dotaz, který bude hledat všechny chyby událost související pro konkrétní aplikaci.

Přihlaste se analýzy upozornění jsou aby došlo k zápisu úložišti OMS jako zvláštní událost a lze získat pomocí protokolu dotazu.  Tak, že je možné nastavit při odstranění problému nemají stav, třeba SCOM události.

![OMS upozornění](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Při použití SCOM jako zdroje dat pro analýzu protokolu SCOM upozornění jsou došlo k zápisu úložišti OMS jsou vytvořené a změny.  

![SCOM upozornění](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Řešení upozornění správy](http://technet.microsoft.com/library/mt484092.aspx) obsahuje souhrn upozornění na aktivní a několik běžné dotazy k načtení různé sady upozornění.  To vám poskytne efektivnější analýzy upozornění než sestavy SCOM.  Můžete k podrobnostem pro z souhrny podrobných dat a vytváření ad hoc dotazů k načtení různé sady upozornění.

![Řešení pro správu upozornění](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Oznámení
Oznámení v SCOM odeslat pošta nebo text v odpovědi na upozornění, které splňují určitá kritéria.  Můžete vytvořit předplatná různých oznámení, které mají různých osob oznámení podle kritérií, jako na objekt sledován, závažnosti upozornění, identifikovat problém, na který nerozpoznal nebo čas.

Několik předplatná mohou sloužit k provedení úplného oznámení strategie pro velké množství management Pack.

![SCOM upozornění](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Protokol analýzy můžete vás upozorní poštou, aby byl vytvořen upozornění nastavení e-mailové oznámení akce u každého [pravidla výstrahy](http://technet.microsoft.com/library/mt614775.aspx).  Nemuselo mít možnost SCOM na přihlásit k odběru několik upozornění s jedno pravidlo.  Potřebujete vytvořit upozornění pravidel, protože OMS Microsoft neposkytuje žádnou některý předem.

![Protokol analýzy upozornění](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Spravovat nedají úplně SCOM oznámení v protokolu analýzy přes vzhledem k tomu, že je možné upravit pouze v konzole operace.  Technologie pro analýzu protokolu je užitečný jako součást správy výstrah zpracovat přes pro poskytování analytických nástrojů, které SCOM samotný nemá.

## <a name="alert-remediation"></a>Upozornění Remediation
[Remediation](http://technet.microsoft.com/library/mt614775.aspx) odkazuje na pokus o automaticky problém označenými upozornění.
  
SCOM umožňuje spuštění nástroje pro diagnostiku a obnovení v odpovědi na monitor zadávání nefunkčního stavu.  K tomu dojde souběžné monitor vytváření upozornění.  Diagnostika a obnovení jsou obvykle implementovaná jako skript, který běží na zástupce.  Diagnostiku pokusí se získat další informace o zjištěné problém obnovení pokusů k odstranění problému.

Protokol analýzy umožňuje zahájení [postupu runbook automatizaci Azure](https://azure.microsoft.com/documentation/services/automation/) nebo hovoru webhook v odpovědi na upozornění na protokolu analýzy.  Runbooks může obsahovat komplexní logiky implementovaná v prostředí PowerShell.  Skript běží v Azure a přístup Azure zdrojů nebo externí zdroje k dispozici v cloudu.  Azure automatizaci dokážou provedení runbooks na serveru ve vaší místní datacentru, ale tato funkce není momentálně neexistuje při spuštění v protokolu analýzy výstraze postupu runbook.

Obnovení v SCOM i runbooks v OMS může obsahovat skriptů Powershellu, ale obnovení jsou obtížnější vytvářet a spravovat, protože musí být obsažen v management pack.  Runbooks jsou uložené v Azure automatizaci, který obsahuje funkcí k vytváření, testování a správa runbooks.

Pokud používáte SCOM jako zdroje dat pro analýzu protokolu, můžete vytvořit upozornění k protokolu technologie pro analýzu použití protokolu dotazu k načtení SCOM upozornění uložené v úložišti OMS.  To umožní spuštění postupu runbook Azure automatické odpovědi na upozornění na SCOM.  Samozřejmě protože postupu runbook bude spouštět v Azure, by pak nebyl životaschopné strategie pro obnovení místní problémů.

## <a name="next-steps"></a>Další kroky

- Další podrobnosti o [upozornění v systému Centrum Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).