<properties 
    pageTitle="Postup nastavení počítače pro služby vývoj médium s .NET" 
    description="Informace o požadavcích na používání Media Services SDK pro .NET Media Services. Naučte se vytvářet aplikace Visual Studio." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Vývoj Media Services s .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Toto téma popisuje, jak začít vývoj aplikací Media Services pomocí .NET.

Knihovna **Azure Media Services.NET SDK** umožňuje program proti Media Services pomocí .NET. Můžete usnadnit její i se dají s .NET, je k dispozici knihovnu **Azure Media Services .NET SDK rozšíření** . Tuto knihovnu obsahuje sadu rozšíření metod a podporu funkcí usnadňujících kódu .NET. Obě knihovny jsou k dispozici prostřednictvím **NuGet** a **GitHub**.


##<a name="prerequisites"></a>Zjistit předpoklady pro

-   Účet Media Services v nové nebo existující Azure předplatného. Přečtěte si téma [jak vytvořit účet služby média](media-services-portal-create-account.md).
-   Operační systémy: Windows 10, Windows 7, Windows 2008 R2 nebo Windows 8.
-   .NET framework 4.5.
-    Visual Studio 2015 Visual Studio 2013, Visual Studio 2012 a Visual Studio 2010 SP1 (Professional, Premium, Ultimate nebo Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Vytváření a konfigurace projekt aplikace Visual Studio

V této části se dozvíte, jak vytvořit projekt ve Visual Studiu a nastavit pro vývoj Media Services.  V tomto případě projektu je aplikace konzoly C# Windows, ale stejný postup nastavení ukazuje tato část platí pro jiné typy projektů, můžete vytvořit pro aplikace Media Services (například Windows formulářů aplikace nebo webové aplikace ASP.NET).

Tato část popisuje přidání Media Services .NET SDK a jiných závislá knihoven pomocí **NuGet** .

Můžete taky můžete získat nejnovější bitů Media Services .NET SDK z GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) a [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), sestavte řešení a přidat odkazy na projektu klienta. Poznámka: všechny potřebné závislosti získat stáhli extrahovat automaticky.

1. Vytvoření nového C# konzoly aplikace Visual Studio 2010 SP1 nebo novější verze a. Zadejte **název**, **umístění**a **název řešení**a klikněte na OK.

2. Vytvoření řešení.

2. Pomocí **NuGet** nainstalovat a přidejte **Azure Media Services .NET SDK rozšíření**. Při instalaci tohoto balíčku, také nainstaluje **Media Services.NET SDK** a přidá všechny ostatní požadované závislosti.

    Ujistěte se, jestli máte nejnovější verzi NuGet nainstalovaný. Další informace a pokyny k instalaci najdete v článku [NuGet](http://nuget.codeplex.com/).

2. V Průzkumníku klikněte pravým tlačítkem myši na název projektu a zvolte spravovat NuGet balíčků...

    Zobrazí se dialogové okno Spravovat balíčků NuGet.

3. V galerii Online hledání Azure MediaServices rozšíření, zvolte Azure Media Services .NET SDK rozšíření a potom klikněte na tlačítko nainstalovat.

    Změny projekt a odkazy na Media Services .NET SDK rozšíření, Media Services .NET SDK a jiných závislá sestavení přidají.

4. Označit opět dostanete čistší vývojové prostředí, zvažte povolení obnovení NuGet balíčku. Další informace najdete v tématu [NuGet balíčku obnovit "](http://docs.nuget.org/consume/package-restore).

3. Přidání odkazu do **System.Configuration** sestavení. Sestavení obsahuje System.Configuration. **ConfigurationManager** předmětu, který se používá pro přístup k souborům konfigurace (například App).

    Chcete-li přidat odkazy, pomocí dialogu Správa odkazů, klikněte pravým tlačítkem myši na název projektu v okně Průzkumník. Vyberte Přidat a odkazy.

    Zobrazí se dialogové okno Spravovat odkazy.

4. V části sestavení .NET framework najděte a vyberte sestavení System.Configuration a klikněte na tlačítko OK.
5. Otevřete konfiguračního souboru (přidání souboru do projektu Pokud nepřidalo ve výchozím nastavení) a přidejte oddíl *appSettings* k souboru.     
Nastavení hodnot pro svůj Azure Media Services název a účet klíč účtu, jak je vidět v následujícím příkladu.

    Najít hodnoty název a klíč, přejděte na portál Azure a vyberte svůj účet. Nastavení okno na pravé straně. V okně Nastavení vyberte klíče. Klepnutím na ikonu vedle každé textové pole slouží ke kopírování hodnoty do schránky systému.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Přepsat existující **pomocí** příslušným na začátku souboru Program.cs následující kód.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

V tomto okamžiku jste připraveni začít vývoj aplikace Media Services.    


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
