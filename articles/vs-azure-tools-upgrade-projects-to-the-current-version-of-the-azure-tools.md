<properties
   pageTitle="Jak upgradovat projekty na aktuální verzi Azure nástroje | Microsoft Azure"
   description="Naučte se aktualizovat Azure projekt ve Visual Studiu aktuální verzi Azure nástroje"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Jak upgradovat projekty na aktuální verzi Azure nástroje for Visual Studio

## <a name="overview"></a>Základní informace

Po instalaci ální nástroje Azure (nebo předchozí verze, která je novější než 1,6) všechny projekty, které byly vytvořené pomocí nástroje Azure uvolněte před 1,6 (Listopad 2011) automaticky upgradují se hned po otevření. Pokud jste vytvořili projektů pomocí 1,6 (Listopad 2011) verzi nástrojů, a přesto se vám osvobození nainstalovaný, můžete tyto projekty otevřít ve starší verzi a rozhodnout později zda je upgradovat.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Jak projekt změní, když ho upgradu

Pokud automaticky upgradu projektu nebo určíte, že chcete upgradovat ho projektu je upraven pro práci s aktuálními verzemi určité sestavení a také změněny některé vlastnosti jako tato část popisuje. Pokud váš projekt vyžaduje další změny je kompatibilní s novější verzi nástroje, třeba tyto změny provést ručně.

- Nastavení(Web.config)) pro web role a konfiguračního souboru pro pracovní role se automaticky aktualizují neodkazuje na novější verzi Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.

- Sestavení Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll a Microsoft.WindowsAzure.ServiceRuntime.dll se upgradovat na nové verze.

- Profily publikování uložené v souboru Azure projektu (.ccproj) se přesunou do samostatného souboru s příponou .azurePubXml podadresáře **Publikovat** .

- Některé vlastnosti profilu publikování se automaticky aktualizují na podporu nové a změněné funkce. **AllowUpgrade** nahrazuje **DeploymentReplacementMethod** , protože nasazeném Cloudová služba můžete aktualizovat najednou nebo postupně.

- Vlastnost **UseIISExpressByDefault** a je nastavena na hodnotu NEPRAVDA, aby webového serveru, který se používá pro ladění se nemění automaticky ze služby (internetové) služby IIS Express. Služby IIS Express je výchozí webový server pro projekty, které jsou vytvořené pomocí novějších verzích nástrojů.

- Ukládání do mezipaměti Azure hostuje kterýmkoliv z vašeho projektu role, některé vlastnosti v konfiguraci služby (.cscfg soubor) a definice služby (.csdef soubor) změní se při upgradu projektu. Pokud projektu používá balíček NuGet Azure ukládání do mezipaměti, projektu upgradovat na nejnovější verzi balíček. Je vhodné otevírat nastavení(Web.config)) a ověřte, že byla během upgradu správně zachována konfigurace klienta. Pokud jste přidali odkazy na ukládání do mezipaměti Azure sestavení klienta bez použití NuGet balíčku, se nebudou aktualizovat tyto sestavení; je nutné ručně aktualizovat tyto odkazy na nové verze.

>[AZURE.IMPORTANT] U F # projektů je nutné ručně aktualizovat odkazy na Azure sestavení, tak, aby odkazovat novější verzí těchto sestavení.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Jak upgradovat Azure projektu na aktuální verzi

1. Nainstalujte aktuální verzi Azure nástroje do instalace aplikace Visual Studio, která chcete použít pro upgradovaném projektu a potom otevřete projekt, který chcete upgradovat. Pokud projekt byl vytvořený pomocí nástroje Azure uvolněte před 1,6 (Listopad 2011) klikněte projekt automaticky upgradovaný na verzi aktuální verzi. Vytvoření projektu s Listopad 2011 uvolněte a tuto verzi je nainstalovaná, projektu se otevře v této verzi.

1. V Průzkumníku otevření místní nabídky pro uzel projektu, vyberte **Vlastnosti**a pak zvolte kartu **aplikace** dialogovém okně, které se zobrazí.

    Karta **aplikace** zobrazuje verzi nástroje, který máte přidružený k projektu. Pokud se zobrazí v předchozí verzi Azure nástroje, projektu již byly aktualizoval. Pokud jste si nainstalovali novější verzi nástroje než co se zobrazí na kartu, zobrazí se tlačítko **upgradovat** .

1. Klikněte na tlačítko Aktualizovat projekt aktuální verzi nástroje **upgradovat** .

1. Vytvoření projektu a adresa všechny chyby, které vyplynou ze změnám rozhraní API. Informace o tom, jak upravit kód pro novou verzi najdete v dokumentaci pro konkrétní rozhraní API.
