<properties
   pageTitle="Nastavení prostředí vývoj | Microsoft Azure"
   description="Nainstalujte modul runtime, SDK a nástroje a vytvoření místní vývoj clusteru. Po dokončení tohoto nastavení, pak budete připraveni k vytvoření aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Příprava vývojové prostředí

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 K vytvoření a spuštění [aplikací struktury služby Azure] [ 1] v počítači vývoj nainstalujte modul runtime, SDK a nástroje. Bude potřeba povolit provádění skriptů Windows Powershellu zahrnutý v sadě SDK.

## <a name="prerequisites"></a>Zjistit předpoklady pro
### <a name="supported-operating-system-versions"></a>Podporované operační systém verze
Pro vývoj jsou podporovány následující operační systém verze:

- Windows 7
- Windows 8 nebo Windows 8.1
- Windows serveru 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 obsahuje pouze prostředí Windows PowerShell 2.0 ve výchozím nastavení. Rutiny prostředí PowerShell struktury služby vyžaduje prostředí PowerShell 3.0 nebo vyšší. Je možné [Stáhnout Windows PowerShell 5.0] [ powershell5-download] z Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Nainstalujte modul runtime, SDK a nástroje

Webové platformy nabízí dvě konfigurace rozvoje struktury služby:

- [Nainstalujte modul runtime služby struktury, SDK a nástroje pro Visual Studio 2015 (vyžaduje Visual Studio 2015 aktualizace 2 nebo novější)][full-bundle-vs2015]
- [Nainstalujte modul runtime služby struktury a SDK pouze (bez nástroje Visual Studio)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Povolení spouštění skriptů Powershellu

Služba struktury používá skripty Windows Powershellu pro vytvoření místní rozvoj clusteru a nasazení aplikace od Visual Studio. Ve výchozím nastavení Windows zablokuje tyto skripty spuštění. Aby, je třeba změnit zásad spouštění PowerShell. Otevřete PowerShell jako správce a zadejte tento příkaz:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Další kroky
Teď jste dokončili jste nastavení vývojové prostředí, pustit do vytváření a spouštění aplikací.

- [Vytvoření první aplikace služby struktury ve Visual Studiu](service-fabric-create-your-first-application-in-visual-studio.md)
- [Naučte se nasadit a Správa aplikací na místní obrázku](service-fabric-get-started-with-a-local-cluster.md)
- [Další informace o modely programování: spolehlivé služby a spolehlivé účastníky](service-fabric-choose-framework.md)
- [Podívejte se na ukázky struktury služby na GitHub](https://aka.ms/servicefabricsamples)
- [Vizualizace svůj cluster v programu Průzkumník struktury služby](service-fabric-visualizing-your-cluster.md)
- [Postupujte podle struktury služby naučné stezky, abyste obecných Úvod k platformu](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka služby struktury kampaň"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "A RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Propojení a WebPI 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Základní SDK WebPI odkaz"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
