<properties
    pageTitle="Povolení vzdáleného ladění s nepřetržitý doručení | Microsoft Azure"
    description="Zjistěte, jak povolit vzdálené ladění při instalaci Azure pomocí nepřetržitý doručení"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Povolení vzdáleného ladění při použití průběžné doručení publikujete Azure

Můžete povolit vzdálené ladění v Azure, cloudovým službám nebo virtuálních počítačích, když použijete [Nepřetržitý doručení](cloud-services-dotnet-continuous-delivery.md) publikujete Azure pomocí následujících kroků.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Povolení vzdáleného ladění služby cloudu

1. Na agenta sestavení nastavte počáteční prostředí pro Azure podle pokynů uvedených v [Sestavit příkazového řádku pro Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. A proto runtime vzdáleného ladění (msvsmon.exe) je nutný pro balíček nainstalujte [Vzdálené nástroje pro Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (nebo [Vzdálené nástroje pro Microsoft Visual Studio 2013 aktualizaci 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) při používání aplikace Visual Studio 2013). Jako alternativu můžete zkopírovat binární vzdáleného ladění ze systému, který má Visual Studio nainstalovaný.
3. Vytvoření certifikátu, jak je uvedeno v [Přehled certifikátů pro službu Azure Cloud Services](cloud-services-certs-create.md). Zachovat .pfx a Miniatura certifikátu RDP a nahrajte certifikát cílové cloudové služby.
4. Pomocí následujících možností v příkazového řádku MSBuild sestavovat a balíčku s vzdáleného ladění povolené. (Nahraďte skutečné cest k souborům systému a project pro úhel závorkách položky).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`je cestu ke složce obsahující msvsmon.exe v nástroje vzdáleného for Visual Studio.
    `RemoteDebuggerConnectorVersion`je v cloudové služby Azure SDK verze. Měla by se taky shodovat nainstalovanou aplikaci Visual Studio verzi.

5. Publikujte cílové cloudové služby pomocí souboru balíčku a .cscfg generovaného v předchozím kroku.
6. Importujte certifikátu (soubor .pfx) k počítači, který má Visual Studio s Azure SDK pro .NET nainstalovaný. Zkontrolujte, že importovat do `CurrentUser\My` úložiště certifikátů, jinak programovém připojování k ladění ve Visual Studiu se nezdaří.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Povolení vzdáleného ladění pro virtuálních počítačích

1. Vytvoření Azure virtuálního počítače. V tématu [Vytvoření virtuálního počítače spuštěný Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [vytváření a správa Azure virtuálních počítačích ve Visual Studiu](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Na [stránku portálu Azure klasická](http://go.microsoft.com/fwlink/p/?LinkID=269851)zobrazení virtuálního počítače řídicího panelu zobrazíte virtuálního počítače **MINIATURU RDP certifikátu**. Tato hodnota se používá pro `ServerThumbprint` hodnota v konfiguraci rozšíření.
3. Vytvořit certifikát klienta, jak je uvedeno v [Přehled certifikátů pro službu Azure Cloud Services](cloud-services-certs-create.md) (ponechat .pfx a Miniatura certifikátu RDP).
4. Instalace prostředí Powershell Azure (verze 0.7.4 nebo novější) podle pokynů uvedených v [článku Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).
5. Spuštěním následujícího skriptu povolení RemoteDebug rozšíření. Nahraďte cesty a osobní údaje vlastní, například název předplatného, název služby a miniatura.

    >[AZURE.NOTE] Tento skript nakonfigurovaný pro Visual Studio 2015. Pokud používáte Visual Studio 2013, upravit `$referenceName` a `$extensionName` následující použít přiřazení `RemoteDebugVS2013` (ne `RemoteDebugVS2015`).

    <pre>
   Přidání AzureAccount

    Vyberte AzureSubscription "Předplatného Microsoft"

    $vm = get-AzureVM - ServiceName "mytestvm1"-název "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint v $endpoints) {přidat AzureEndpoint - OM $vm – název $endpoint. Name - protokolu tcp - PublicPort $endpoint. PublicPort - Místní_port $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > true < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Nastavení AzureVMExtension `
    -ReferenceName $referenceName ` 
    -vydavatel $publisher `
    -ExtensionName $extensionName ` 
    – verze $version "- PublicConfiguration $publicConfiguration

    foreach ($extension v $vm. OM. ResourceExtensionReferences) {Pokud (($extension. Název_odkazu - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - a ($extension. Jméno – eq $extensionName) "- a ($extension. Verze - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Klíč = konec "config.txt"}}

    $vm | Aktualizace AzureVM </pre>

6. Importujte certifikátu (.pfx) k počítači, který má Visual Studio s Azure SDK pro .NET nainstalovaný.
