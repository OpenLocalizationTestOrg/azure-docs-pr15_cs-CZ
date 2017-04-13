<properties
   pageTitle="Sestavení příkazového řádku pro Azure | Microsoft Azure"
   description="Sestavení příkazového řádku pro Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Sestavení příkazového řádku pro Azure

## <a name="overview"></a>Základní informace

Vytvořit balíček pro nasazení Azure spuštěním MSBuild příkazového řádku. Můžete nakonfigurovat a definovat sestavení pro ladění pracovní a výrobní kromě některých proces vytváření automatizaci.


## <a name="microsoft-build-engine-msbuild"></a>Sestavení Microsoft Engine (MSBuild)

Pomocí Microsoft Build Engine (MSBuild) je možné vytvářet produkty v buildu laboratorní prostředí, kde není nainstalovaný Visual Studio. MSBuild používá formát XML pro soubory projektu, které extensible a plně podporované společností Microsoft. V tomto formátu souboru můžete popsat co musí být položky vytvořené pro jeden nebo více platformy a konfigurace.

Můžete taky spustit MSBuild příkazového řádku a toto téma popisuje tento přístup. Nastavením vlastnosti na příkazovém řádku, je možné vytvářet určité konfigurace projektu. Podobně lze také určit, cílů, které bude vytvářet příkaz MSBuild. Další informace o parametry příkazového řádku a MSBuild najdete v článku Principy [MSBuild příkazového řádku](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Instalace

Jak popisuje následující postup, musíte nainstalovat software a nástroje na serveru vytvořit před vytvořením balíčku Azure pomocí MSBuild:

1. Instalace .NET Framework 4 nebo novější, který obsahuje MSBuild.

1. Instalace [Nástroje pro vytváření Azure](http://go.microsoft.com/fwlink/?LinkId=394615) (hledejte MicrosoftAzureAuthoringTools x64.msi nebo MicrosoftAzureAuthoringTools x86.msi.

1. Instalace [Azure knihovny pro .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (hledejte MicrosoftAzureLibsForNet x64.msi nebo MicrosoftAzureLibs x86.msi.

1. Zkopírujte soubor Microsoft.WebApplication.targets z instalaci Visual Studio na jiném počítači.

    Soubor je umístěn v adresáři C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 for Visual Studio 2012) a měli jej zkopírujete do stejného adresáře na serveru vytvořit.

1. Nainstalujte [Azure Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Vyhledejte WindowsAzureTools.vs120.exe vytvářet projekty Visual Studio 2013.

## <a name="msbuild-parameters"></a>MSBuild parametry

Nejjednodušší způsob, jak vytvořit balíček je spuštění MSBuild s `/t:Publish` možnost. Ve výchozím nastavení vytvoří tento příkaz adresář vzhledem k kořenové složce pro projekt, například ProjectDir\bin\Configuration\app.publish\. při vytváření Azure projektu vytvoříte dva soubory, samotný soubor balíčku a doprovodnou konfiguračního souboru:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Ve výchozím nastavení jednotlivých Azure project obsahuje jiný, který se vytvoří soubor konfigurace služby pro místní (ladění) a jiné pro buildy cloud (pracovní nebo produkční), ale můžete přidat nebo odebrat konfiguraci služby soubory podle potřeby. Po vytvoření balíčku aplikace Visual Studio se objeví výzva konfiguraci služby souborů, které chcete zahrnout vedle balíček. Po vytvoření balíčku pomocí MSBuild místní konfiguraci služby soubor nachází ve výchozím nastavení. Chcete-li zahrnout jiný soubor konfigurace služby, nastavte `TargetProfile` vlastnost příkazu MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Pokud chcete použít alternativní adresář uložené balíčku a konfigurace souborů, cestu můžete nastavit pomocí `/p:PublishDir=Directory\` možnost, včetně koncových zpětné lomítko oddělovač.

## <a name="deployment"></a>Nasazení

Po balíček, můžete ho nasadit do Azure. Kurz, který ukazuje tento proces najdete v článku Azure webu. Další informace o tom, jak tento proces zautomatizovat, najdete v článku [Nepřetržitý doručení služby cloudu v Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
