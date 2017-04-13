<properties
   pageTitle="Odstraňování chyb při klienta Docker v systému Windows pomocí aplikace Visual Studio | Microsoft Azure"
   description="Řešení problémů, ke kterým dojít při použití Visual Studio k vytvoření a nasazení webové aplikace pomocí aplikace Visual Studio Docker ve Windows."
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
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Poradce při potížích s vývoj Docker Visual Studio

Při práci s Visual Studio nástroje pro Docker Preview, může docházet k některé problémům kvůli přírodní náhled.
Následuje několik běžných problémů a řešení.


## <a name="unable-to-validate-volume-mapping"></a>Nelze ověřit mapování hlasitosti
Mapování hlasitost je nutné sdílení zdrojového kódu a binární soubory aplikace ve složce aplikace v kontejneru.  Mapování konkrétní hlasitost jsou obsaženy v souborech docker compose.dev.debug.yml a docker compose.dev.release.yml. Jak soubory se změní na hostitelském počítači, kontejnerů podle těchto změn v podobné struktura složek.

Povolit hlasitost mapování, otevřete **Nastavení** pomocí ikony panelu "moby" Docker pro Windows a potom na kartu **Sdílené jednotky** .  Ujistěte se, že písmeno, které hostuje projektu, jakož i písmeno, kde jsou uložená uživatelského profilu % jsou sdíleny kontrolu je a potom kliknutím na **použít**.

Otestovat, pokud hlasitost mapování funguje, jakmile sdílejí na jednotkách, opětovného vytvoření a F5 z aplikace Visual Studio nebo zkuste tímto příkazem Dotázat se:

*Na příkazovém řádku systému Windows*

*[Poznámka: předpokladem složky uživatelů se nachází na disku "C" a, že byl sdílený.  Pokud sdílíte jinou jednotku, aktualizovat podle potřeby]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*V kontejneru Linux*

```
/ # ls
```

Měli byste vidět v adresáři ze uživatelé/veřejné složky.
Pokud se zobrazí žádné soubory a složky /c/Users/Public není prázdné, není správně nakonfigurovaný hlasitost mapování. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Změna do adresáře červích zobrazíte obsah `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Poznámka:** *Při práci s Linux VMs, je systém souborů kontejneru malá a velká písmena.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Sestavení: "PrepareForBuild" úkol se neočekávaně nezdařilo.

Microsoft.DotNet.Docker.CommandLine.ClientException: Došlo k chybě pokouší připojit:

Ověřte, jestli že je spuštěný host docker výchozí. Otevřete příkazový řádek a provést:

```
docker info
```

Pokud tento příkaz vrátí chybu pokuste se začít v desktopové aplikaci **Docker pro Windows** .  Pokud v desktopové aplikaci běží klikněte na ikonu **moby** na hlavním panelu by měly být viditelné. Klikněte pravým tlačítkem na ikonu hlavního panelu a otevřete **Nastavení**.  Klikněte na **Obnovit** kartu a potom **Restart Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Ruční upgrade z verze 0.31 k 0,40


1. Zálohování projektu
1. Odstraňte následující soubory v projektu:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Zavřete řešení a odeberte ze souboru .xproj následující řádky:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Znovu otevřete řešení
1. Následující řádky odeberte ze souboru Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Odstraňte následující soubory související s Docker z project.json v publishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Odinstalujte v předchozí verzi a Docker nástroje 0,40 a potom **Přidat -> Podpora Docker** znovu nainstalovat z místní nabídky pro ASP.Net základní Web nebo aplikaci konzoly. Takto přidáte nové požadované artefakty Docker zpět do projektu. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dialogové okno chyby dojde při pokusu o **Přidání -> Podpora Docker** nebo ladění (F5) aplikace ASP.NET Core v kontejneru

Jsme občas zobrazila odinstalaci a instalaci rozšíření mezipaměti aplikace Visual Studio MEF (spravovat rozšiřitelnost Framework), může být poškozená. Když k tomu dojde, že mohou způsobit různých chyby dialogová okna při přidávání Docker podpory a/nebo pokusu o spuštění nebo ladění (F5) aplikace základní ASP.NET. Jako dočasné řešení provést následující kroky se dají odstranit nebo obnovit MEF mezipaměti.

1. Ukončete všechny instance aplikace Visual Studio
1. Otevřete %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Odstranění těchto složek
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otevřete aplikaci Visual Studio
1. Opakovat scénáře 
