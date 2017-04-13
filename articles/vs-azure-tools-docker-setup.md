<properties
   pageTitle="Konfigurace hostitele Docker s VirtualBox | Microsoft Azure"
   description="Podrobné pokyny pro nastavení výchozí instanci Docker pomocí Docker počítače a VirtualBox"
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
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurace hostitele Docker s VirtualBox

## <a name="overview"></a>Základní informace
Tento článek vás provede konfigurace výchozí instanci Docker pomocí Docker počítače a VirtualBox. Pokud používáte [beta Docker pro systém Windows](http://beta.docker.com/), není nutné tuto konfiguraci.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Následující nástroje je třeba nainstalovat.

- [Podokno úloh Souprava nástrojů docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurace klienta Docker používat Windows PowerShell

Konfigurace klienta Docker, jednoduše otevřete prostředí Windows PowerShell a proveďte následující kroky:

1. Vytvořte instanci výchozí docker Host (hostitel).

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Ověření, že je výchozí instanci nakonfigurované a spuštěnou. (Byste měli vidět instance s názvem "Výchozí" spuštěný.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![výstup ls docker počítače][0]
 
1. Nastavení výchozích jako aktuálního hostitele a konfiguraci vašeho prostředí.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Aktivní kontejnery Docker zobrazení. V seznamu by měl být prázdný.

    ```PowerShell
    docker ps
    ```

    ![výstup ps docker][1]
 
> [AZURE.NOTE]Pokaždé, když restartujete počítač vývoj bude potřeba restartovat hostitele místní docker.
> K tomuto účelu problémů tento příkaz příkazového řádku: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
