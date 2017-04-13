<properties
   pageTitle="Nasazení kontejner technologie ASP.NET Core Linux Docker vzdáleného hostitele Docker | Microsoft Azure"
   description="Naučte se používat Visual Studio Tools for Docker nasadit webovou aplikaci ASP.NET Core kontejneru Docker spuštěna Azure Docker hostitele Linux OM"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Nasazení vzdáleného hostitele Docker kontejner technologie ASP.NET

## <a name="overview"></a>Základní informace
Docker je lehké kontejneru modul, podobně jako v některé způsoby, jak virtuální stroje, které vám pomohou hostitele aplikací a služeb.
Tento kurz vás provede instalaci aplikace ASP.NET základní Docker hostitele na Azure pomocí prostředí PowerShell pomocí rozšíření [Visual Studio 2015 Tools for Docker](http://aka.ms/DockerToolsForVS) .

## <a name="prerequisites"></a>Zjistit předpoklady pro
Následující potřebná k dokončení tohoto kurzu:

- Vytvořte Azure Docker hostitele OM podle popisu v [používání docker stroje s Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Aktualizace [aplikace Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Instalace [Aplikace Visual Studio 2015 Tools for Docker – verze Preview](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. vytvořit webovou aplikaci základní technologie ASP.NET
Vytvoření základní ASP.NET základní aplikace, které se použije v tomto kurzu se provede následující kroky.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. podporu Docker přidat

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. skript Powershellu DockerTask.ps1 použít 

1.  Otevřete příkazovém řádku prostředí PowerShell kořenového adresáře projektu. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Ověření vzdáleného je spuštěný host. Měli byste vidět stavu = spuštění 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Pokud používáte Docker Beta, nebude uvedený tady svého hostitele.

1.  Vytvořit pomocí aplikace parametr - Tvůrce dotazů

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Používáte Docker Beta, vynecháte argument - počítače
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Spusťte aplikaci pomocí parametr - spustit

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Používáte Docker Beta, vynecháte argument - počítače
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Po dokončení docker byste měli vidět výsledky podobná této:

    ![Zobrazení aplikace][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
