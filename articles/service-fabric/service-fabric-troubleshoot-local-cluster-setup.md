<properties
   pageTitle="Poradce při potížích s místní nastavení clusteru služby struktury | Microsoft Azure"
   description="Tento článek se věnuje sada návrhů pro řešení potíží s místní rozvoj obrázku"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Poradce při potížích s místní rozvoj nastavení obrázku

Pokud narazíte na chyby při interakci s místní cluster vývoj struktury služby Azure, zkontrolujte následující návrhy možná řešení.

## <a name="cluster-setup-failures"></a>Selhání vzhled obrázku

### <a name="cannot-clean-up-service-fabric-logs"></a>Nelze vyčistit služby struktury protokoly

#### <a name="problem"></a>Problém

Při spuštění DevClusterSetup skript, zobrazí se chyba takto:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Řešení

Zavřete okno aktuální prostředí PowerShell a otevření nového okna prostředí PowerShell jako správce. Teď byste měli být schopní úspěšně následujícím způsobem.

## <a name="cluster-connection-failures"></a>Chyby připojení obrázku

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Rutiny prostředí PowerShell struktury služby nerozpoznává v prostředí PowerShell Azure

#### <a name="problem"></a>Problém

Pokud se při pokusu o spuštění všech rutinách Powershellu struktury služby, jako `Connect-ServiceFabricCluster` v okně Azure PowerShell se nezdaří, oznamující, že se bohužel nerozpoznal rutiny. Důvodem je, že Azure PowerShell používá 32bitovou verzi Windows PowerShell (i v 64bitové operační systém verze), že rutiny pro správu služby struktury použít pouze v prostředí 64bitová verze.

#### <a name="solution"></a>Řešení

Vždy spustit službu struktury rutiny přímo z prostředí Windows PowerShell.

>[AZURE.NOTE] Nejnovější verzi Azure PowerShell nevytvoří zvláštní zástupce, takže už dojde.

### <a name="type-initialization-exception"></a>Zadání výjimek inicializace

#### <a name="problem"></a>Problém

Když se připojujete k obrázku v prostředí PowerShell, se zobrazí chyba TypeInitializationException pro System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Řešení

Proměnná path nebyla nastavena správně během instalace. Odhlaste se z Windows a přihlaste se zpátky. Tato cesta plně aktualizuje.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Připojení obrázku se nezdaří s "Objekt je zavřený"

#### <a name="problem"></a>Problém

Volání připojit ServiceFabricCluster selhává, zobrazí se chybová zpráva takto:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Řešení

Zavřete okno aktuální prostředí PowerShell a otevření nového okna prostředí PowerShell jako správce. Teď byste měli být schopní úspěšně se připojit.

### <a name="fabric-connection-denied-exception"></a>Výjimky připojení odmítnuto struktury

#### <a name="problem"></a>Problém

Když ladění z aplikace Visual Studio, zobrazí se chybová FabricConnectionDeniedException.

#### <a name="solution"></a>Řešení

K této chybě dochází, pokud zkuste k pokusu o spuštění hostitele služby zpracováváte ručně, není vhodné umožnit modul runtime služby struktury zahájíte za vás.

Ujistěte se, že nemáte službu projekty, nastavit jako při spuštění projekty v řešení. Pouze projekty aplikací služby struktury je třeba nastavit jako při spuštění projekty.

>[AZURE.TIP] Pokud po instalaci, místní clusteru, nebude zahájen chovat abnormálně, můžete je obnovit panelu používání místní clusteru správce systému. Odebere existující obrázku a nastavení novou. Dejte pozor, aby všechny nainstalovali aplikace a Spojenými daty, se odeberou.

## <a name="next-steps"></a>Další kroky

- [Princip a řešení potíží s svůj cluster pomocí sestav stavu systému](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Vizualizace svůj cluster v Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md)
