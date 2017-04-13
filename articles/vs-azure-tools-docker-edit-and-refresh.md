<properties
   pageTitle="Ladění aplikace v kontejneru místní Docker | Microsoft Azure"
   description="Informace o úpravách aplikace, která běží v kontejneru místní Docker, aktualizujte kontejner prostřednictvím úpravy a aktualizace a nastavení ladění zarážek"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Ladění aplikace v kontejneru místní Docker

## <a name="overview"></a>Základní informace
Visual Studio Tools for Docker poskytují jednotný způsob, jak se dají v a ověření aplikace místně v kontejneru Linux Docker.
Nemusíte restartujte kontejneru pokaždé, když uděláte kód změnit.
Tento článek vám ukazují, jak používat funkci "Upravovat a aktualizovat" v kontejneru místní Docker začít ASP.NET základní webovou aplikaci, proveďte potřebné změny a aktualizujte prohlížeč zobrazíte tyto změny.
To bude taky vidíte, jak nastavit zarážky pro ladění.

> [AZURE.NOTE] Podpora kontejneru Windows se už v budoucí verzi

## <a name="prerequisites"></a>Zjistit předpoklady pro
Následující nástroje je třeba nainstalovat.

- [Aktualizace 2015 Visual Studio 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Aktualizace [aplikace Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Spuštění Docker kontejnery místně, musíte mít místní docker klienta.
Můžete použít vydané [Docker nástrojů](https://www.docker.com/products/overview#/docker_toolbox) , který vyžaduje Hyper-V zakázat nebo můžete použít [Docker pro Windows Beta](https://beta.docker.com) , který používá Hyper-V a vyžaduje Windows 10.

Pomocí nástrojů Docker, musíte [nakonfigurovat Docker klienta](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. vytvořit web appu

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. podporu Docker přidat

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. upravit kód a aktualizace

Rychle iterace změny, můžete spustit aplikaci v kontejneru a i nadále provádět změny, stejně jako u služby IIS Express jejich zobrazení.

1. Nastavte konfigurace řešení na `Debug` a stiskněte klávesu ** &lt;kombinaci kláves CTRL + F5 >** sestavovat obrázek docker a spustit místně.

    Jakmile obrázek kontejneru byla vytvořena a běží v kontejneru Docker, spustí Visual Studio Web app v prohlížeči výchozí.
    Pokud používáte prohlížeče Microsoft Edge nebo jinak chybami, přečtěte si téma [Poradce při potížích s](vs-azure-tools-docker-troubleshooting-docker-errors.md) oddíl.

1. Přejděte na stránku o produktu, který je místo, kam chceme naše změny.

1. Vraťte se do aplikace Visual Studio a otevřete `Views\Home\About.cshtml`.

1. Přidejte následující obsah HTML na konec souboru a uložte požadované změny.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Po dokončení sestavení .NET a zobrazit tyto řádky zobrazení v okně výstupu, přepněte se zpátky do prohlížeče a aktualizujte stránku o produktu.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Byly použity provedené změny.

## <a name="4-debug-with-breakpoints"></a>4. ladění zarážek

Často změny, musíte dál kontroly využívání funkce ladění aplikace Visual Studio.

1.  Vraťte se do aplikace Visual Studio a otevřete`Controllers\HomeController.cs`

1.  Nahraďte obsah metodu About() takto:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Nastavení zarážky nalevo od `string message`... řádku.

1.  Přístupů ** &lt;F5 >** pro spuštění ladění.

1.  Přejděte na stránku o produktu strefíte vaše zarážka.

1.  Přepněte do aplikace Visual Studio zobrazíte zarážka a zkontrolovat, jestli hodnota zprávy.

    ![][2]

##<a name="summary"></a>Souhrn

Pomocí [Aplikace Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS)můžete získat produktivity místně, práci s realism výrobní vývoje uvnitř kontejneru Docker.

## <a name="troubleshooting"></a>Řešení potíží

[Poradce při potížích s vývoj Docker Visual Studio](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Další informace o Docker s Visual Studiu, Windows a Azure

- [Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - vývoj kódu .NET Core v kontejneru
- [Docker Tools for Visual Studio týmovou](http://aka.ms/dockertoolsforvsts) – vytvořte a nasaďte docker kontejnery
- [Docker Tools for Visual Studio kód](http://aka.ms/dockertoolsforvscode) - jazykové služby pro úpravy docker soubory s další příklady e2e chodit
- [Informace kontejneru Windows](http://aka.ms/containers)– Windows Server a informace o serveru Nano
- [Služba Azure kontejneru](https://azure.microsoft.com/services/container-service/) - [obsah služby Azure kontejneru](http://aka.ms/AzureContainerService)
-    Další příklady práce s Docker najdete v článku [práce s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z připojit [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Rychlé další prohlídky z ukázku HealthClinic.biz najdete v článku [Rychlé Azure vývojář nástroje prohlídky](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Různé Docker nástroje

[Některé nástroje skvělé docker (Steve Lasker blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Dobrý články

[Úvod k Microservices NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Prezentace

- [Steve Lasker: A Live Las Vegas 2016 – Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Úvod do technologie ASP.NET základní @ sestavit 2016 – místo, kam se na ukázku](https://channel9.msdn.com/Events/Build/2016/B810)
- [Vývoji aplikací .NET v kontejnerech, 9 kanálu](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
