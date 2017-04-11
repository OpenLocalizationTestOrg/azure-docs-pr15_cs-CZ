<properties 
pageTitle="Zpracování událostí životního cyklu cloudové služby | Microsoft Azure" 
description="Zjistěte, jak lze v .NET metody životního cyklu role cloudové služby" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Přizpůsobení životním cyklu webu nebo pracovního role v .NET

Při vytváření pracovního role rozšíření [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) předmětu, který zajišťuje, že metody potlačit, které vám umožní reagovat na události životního cyklu. Pro web role této třídy vynechán, je nutné použít reagovat na události životního cyklu.

## <a name="extend-the-roleentrypoint-class"></a>Rozšíření RoleEntryPoint třídy

Třídy [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) zahrnuje metody, které se označují jako tak, že Azure při ho **spustí**, **spustí**nebo **zastaví** webu nebo pracovního role. Volitelně můžete potlačit těchto postupů můžete spravovat role inicializace, sekvence vypnutí rolí nebo odstraňuje role. 

Po rozšíření **RoleEntryPoint**, byste měli znát následující chování metod:

-   [Při spuštění](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) a [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metody vrátí hodnotu typu boolean tak, aby bylo možné vrátit **hodnotu false** z těchto postupů.

     Pokud váš kód vrátí **hodnotu false**, proces role je ukončí, bez spuštění vypnutí posloupnost, který může mít na místě. Obecně neměli byste vrací **hodnotu false** z metody **při spuštění** .
     
-   Nezachycená výjimka v rámci přetížení metodu **RoleEntryPoint** zpracován jako neošetřené výjimce.

     V případě výjimky v rámci metod životního cyklu Azure vyvolá [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) události a potom ukončení. Po vaše role offline, je nutné restartovat tak, že Azure. Pokud dojde k neošetřené výjimce, není aktivována událost [krátkou zprávu](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) a metodu **OnStop** není volat.

Pokud vaše role nespustí nebo recyklace mezi inicializace, zaneprázdněn a zastavení států, může kódu vyvolání neošetřené výjimce v rámci jedné události životního cyklu pokaždé, když se restartuje roli. V tomto případě umožňuje události [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) zjistit příčinu výjimky a obsloužení řádně podporovat. Vaše role může taky vrací z metody [Spustit](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , který způsobuje roli chcete znovu spustit. Podrobnosti o stavech nasazení najdete v článku [Běžné problémy s které způsobují role na Koš](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Pokud používáte **Azure nástroje pro Microsoft Visual Studio** se dají aplikace, šablony projektů role automaticky rozšířit třídu **RoleEntryPoint** , *WebRole.cs* a *WorkerRole.cs* souborů se změnami.

## <a name="onstart-method"></a>Metoda při spuštění

Metoda **při spuštění** se nazývá při instanci rolí do online režimu tak, že Azure. Během provádění kód při spuštění instanci rolí označen jako **zaneprázdněn(a)** a externích přenosů budete přesměrováni do ní Vyrovnávání zatížení. Mohou přepsat tuto metodu provádět inicializace prací, třeba provádění obslužné rutiny události a od [Azure diagnostiky](cloud-services-how-to-monitor.md).

Pokud **při spuštění** vrátí **hodnotu PRAVDA**, úspěšném obnovení výchozího instanci a Azure volání **RoleEntryPoint.Run** metody. Pokud **při spuštění** vrátí **hodnotu false**, ukončí roli okamžitě, bez spuštění žádnou sekvenci plánované vypnutí.

Následující příklad ukazuje, jak přepsat metodu **při spuštění** . Tento způsob nakonfiguruje a začíná diagnostiky monitor instanci rolí spustí a nastaví předávání protokolování údajů do účtu úložiště:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>Metoda OnStop

Metoda **OnStop** se nazývá po instanci rolí je v režimu offline tak, že Azure a před ukončení procesu. Tento způsob kontaktování kódu potřebného pro instanci rolí čistě vypnout můžete přepsat.

> [AZURE.IMPORTANT] Kód spuštěný v metodu **OnStop** má po omezenou dobu dokončete při volání důvodů než vypnutí inicializovaný uživatelem. Po uplynutí tentokrát procesu ukončení, takže musíte tento kód do pole může metoda **OnStop** spustit rychle nebo toleruje neběží k dokončení. Metoda **OnStop** se nazývá po **ukončení** události.


## <a name="run-method"></a>Spustit metodu

Můžete přepsat metodu **Spustit** implementovat dlouhé spuštěný podproces role instance.

Přepsání metody **Spustit** nepožaduje; provedení výchozí spustí podproces, který v režimu spánku trvale. Je-li změnit způsob **spuštění** , byste měli donekonečna udržovat zablokovat kódu. Pokud vrátí metodu **Spustit** roli je automaticky řádně z koše; jinými slovy Azure vyvolá událost **krátkou zprávu** a volá metodu **OnStop** tak, aby vypnutí sekvence může provádět před roli do režimu offline.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Provádění metody životního cyklu ASP.NET roli web

Metody životního cyklu ASP.NET kromě těch podle předmětu **RoleEntryPoint** slouží ke správě inicializace a vypnutí sekvence pro web roli. To může být užitečné pro účely kompatibility, pokud jsou přenos existujících aplikace ASP.NET Azure. Metody životního cyklu ASP.NET nazývají z v rámci metod **RoleEntryPoint** . **Aplikace\_Start** metoda nazývá po dokončení **RoleEntryPoint.OnStart** metody. **Aplikace\_ukončit** metoda nazývá před metodu **RoleEntryPoint.OnStop** volat.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak [vytvořit balíček služby cloudu](cloud-services-model-and-package.md).