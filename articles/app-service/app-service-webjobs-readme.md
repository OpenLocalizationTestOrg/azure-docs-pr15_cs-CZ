<properties
    pageTitle="WebJobs služby Azure aplikace"
    description="Naučte se vytvářet WebJobs spustit testy pozadí, pracovat s služby, jako třeba úložiště a služby Bus a vytvářet naplánované úkoly."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Pomocí WebJobs Azure aplikace služby

Tento článek obsahuje odkazy na prostředky si přečtěte následující dokumentaci o tom, jak používat Azure WebJobs a Azure WebJobs SDK. Azure WebJobs poskytují snadný způsob, jak spustit skripty a programy jako procesy na pozadí v [Aplikaci služby Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Nahrávání a spustit spustitelný soubor, jako je třeba jako cmd bat, exe (.NET) ps1, sh, php, kopírovat, js a sklenice. Tyto aplikace spustili WebJobs při plánování (cron) nebo nepřetržitě.

WebJobs SDK usnadňuje pomocí Azure úložiště. WebJobs SDK má vazby a aktivační událost systému, který spolupracuje objektů BLOB Microsoft Azure úložiště, dotazů a tabulek stejně jako fronty Bus služby.

Vytváření, nasazení a správu WebJobs je bezproblémové pomocí integrovaného nástrojů ve Visual Studiu. Můžete vytvářet WebJobs ze šablon, publikování a správa (spustit a zastavit/monitor/ladění) je.

Řídicí panel WebJobs na portálu Azure poskytuje možnosti výkonné správy poskytující úplnou kontrolu nad spuštění WebJobs, včetně možnost vyvolat jednotlivých funkcích v rámci WebJobs. Řídicí panel taky zobrazí runtimes (funkce) a výstup protokolování.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
