<properties
   pageTitle="Poradce při potížích s trasování událostí | Microsoft Azure"
   description="Nejčastější problémy při zavedení služby na Microsoft Azure služby struktury došlo."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Řešení běžných problémů s při nasazení služby Azure služby struktury

Pokud používáte služby v počítači vývojář, je snadno se použije [aplikace Visual Studio ladění nástroje](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Vzdálené clusterů [sestav stavu](service-fabric-view-entities-aggregated-health.md) jsou vždy vhodným místem pro zahájení. Nejjednodušší možnosti přístupu k tyto sestavy jsou pomocí prostředí PowerShell nebo [SFX](service-fabric-visualizing-your-cluster.md). Tento článek předpokládá ladění vzdáleného clusteru a máte základní principy použijte některý z těchto nástrojích.

##<a name="application-crash"></a>Zhroucení aplikace
"Oddíl je pod obrázku otevřené nebo instance počet" sestava je dobré údajem, že dochází k selhání služby. Pokud chcete zjistit, kde dochází k selhání službě trvá trochu další vyšetřování. Spuštěná služba ve velkém měřítku přítele nejlepší bude sadu dobře promyšlený trasování.  Měli byste, zkuste [Azure Diagnostika](service-fabric-diagnostics-how-to-setup-wad.md) pro tyto trasování shromažďování a používání řešení například [Pružná hledání](service-fabric-diagnostic-how-to-use-elasticsearch.md) pro prohlížení a hledání trasování.

![SFX oddíl stavu](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Během inicializace služby nebo actor
Požadované výjimky před inicializací typem služby způsobí procesu pád. Pro tyto typy chyb protokolu událostí se zobrazí chyba z služby.
Jedná se o nejčastěji používané výjimky zobrazíte před inicializaci služby.

***System.IO.FileNotFoundException***

Tato chyba je často z důvodu chybějící sestavení závislosti. Zkontrolujte vlastnost CopyLocal ve Visual Studiu nebo globální mezipaměti sestavení pro uzel.

***System.Runtime.InteropServices.COMException***
 *na System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Tento údaj označuje, že název typu registrovaných služby neodpovídá manifest služby.

[Diagnostika Azure](service-fabric-diagnostics-how-to-setup-wad.md) je možné konfigurovat nahrát protokolu událostí pro všechny uzly automaticky.

###<a name="runasync-or-onactivateasync"></a>RunAsync() nebo OnActivateAsync()
Pokud k chybě probíhá během inicializace nebo spuštěné typ registrovaných služby nebo actor, se budou výjimce podle struktury služby Azure. Můžete zobrazit tyto od poskytovatelů ZDROJ_UDÁLOSTI uvedené v části "Další kroky".

## <a name="next-steps"></a>Další kroky

Další informace o existující diagnostiky poskytovanou struktury služby:

* [Spolehlivé diagnostiky účastníky](service-fabric-reliable-actors-diagnostics.md)
* [Spolehlivé diagnostiky služby](service-fabric-reliable-services-diagnostics.md)
