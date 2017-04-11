<properties
   pageTitle="Instalace .NET na roli služby cloudu | Microsoft Azure"
   description="Tento článek popisuje, jak ručně nainstalovat .NET framework na Web služby cloudu a pracovní role"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Instalace .NET na roli služby cloudu 

Tento článek popisuje, jak nainstalovat .NET framework na Web služby cloudu a pracovní role. Tento postup slouží k .NET 4.6.1 nainstalovat Azure hosta OS řady 4. Nejnovější informace o operačním systémem Host verzích najdete v článku [Azure hosta OS aktualizací a SDK kompatibility matici](cloud-services-guestos-update-matrix.md).

Proces instalace .NET na váš web a pracovní role zahrnuje včetně instalační balíček .NET v rámci projektu cloudu a spuštění instalačního programu jako součást role spuštění úkoly.  

## <a name="add-the-net-installer-to-your-project"></a>Přidání instalační program .NET do projektu
- Stáhněte si web instalační program pro .NET framework, kterou chcete nainstalovat
    - [.NET 4.6.1 web Instalační služby systému](http://go.microsoft.com/fwlink/?LinkId=671729)
- Web rolí
  1. V **Okně Průzkumník řešení**, v části **role** v aplikaci project cloudové služby klikněte pravým tlačítkem na roli a vyberte **Přidat > Nová složka**. Vytvoření složky s názvem *Koš*
  2. Klikněte pravým tlačítkem na složku **Koš** a vyberte **Přidat > existující položku**. Vyberte .NET instalačního programu a přidejte do složky Koš.
- Pracovní rolí
  1. Klikněte pravým tlačítkem na roli a vyberte **Přidat > existující položku**. Vyberte instalační program .NET a přiřadit ji někomu roli. 

Tímto způsobem, obsah složky Role automaticky přidána do balíčku služby cloudu a budou používaný konzistentní místo ve počítače virtuální přidat soubory. Tenhle postup opakujte pro všechny web a pracovní role v cloudové služby, tak všechny role měli kopii instalačního programu.

> [AZURE.NOTE] Měli byste nainstalovat .NET 4.6.1 o svoji roli cloudové služby i v případě, že vaše aplikace zaměřuje .NET 4.6. OS hosta Azure včetně aktualizací [3098779](https://support.microsoft.com/kb/3098779) a [3097997](https://support.microsoft.com/kb/3097997). Instalace .NET 4.6 nad tyto aktualizace může způsobit problémy při spuštění aplikace .NET, abyste měli přímo nainstalovat .NET 4.6.1 místo .NET 4.6. Další informace najdete v článku [znalostní bázi Knowledge Base 3118750](https://support.microsoft.com/kb/3118750).

![Role obsah s Instalační služby systému souborů][1]

## <a name="define-startup-tasks-for-your-roles"></a>Definování spuštění úkoly pro vaše role
Spuštění úkoly umožňují provádění operací před spuštěním roli. Instalace .NET Framework v rámci úkolu spuštění zajistí, že rámci je nainstalovanou některou z aplikace kód spuštěný. Další informace o spuštění úlohy, viz: [Spuštění úkoly při spuštění v Azure](cloud-services-startup-tasks.md). 

1. Přidejte následující text k souboru *ServiceDefinition.csdef* uzlu **WebRole** nebo **WorkerRole** pro všechny role:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Výše uvedené konfigurace spustí příkaz konzoly *soubor install.cmd* s oprávněními správce tak, aby ji mohli nainstalovat .NET framework. Konfigurace můžete vytvořit také LocalStorage s názvem *NETFXInstall*. Spouštěcí skript nastaví složce temp a použijte zdroj místní úložiště tak, aby instalačního programu .NET framework bude stáhnout a nainstalovat z tohoto zdroje. Je důležité nastavte velikost tento zdroj minimálně 1 024 MB zajistit že rámci nainstaluje správně. Další informace o spuštění úlohy, viz: [běžné cloudové služby spuštění úkoly](cloud-services-startup-tasks-common.md) 

2. Vytvoření souboru **soubor install.cmd** a přidejte ho do všech rolí tak, že pravým tlačítkem myši na roli a výběr **Přidat > existující položku...**. Aby všechny role má nyní .NET instalační soubor jako soubor install.cmd.
    
    ![Role obsah se všemi soubory][2]

    > [AZURE.NOTE] Umožňuje vytvořit tento soubor editor prostého textu například Poznámkový blok. Pokud ve Visual Studiu vytvořte textový soubor a přejmenujte ho na "cmd" soubor může stále obsahovat značku pořadí bajtů UTF-8 a systémem první řádek skript bude výsledkem chyba. Pokud by bylo potřeba vytvořit pomocí aplikace Visual Studio opustit soubor dodejte REM (poznámky) první řádek souboru tak, aby se při spuštění ignorována. 

3. Přidejte následující skript soubor **install.cmd** :

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Skript nainstalovat kontroluje, zda zadaný .NET framework verze už nainstalovaný v počítači v registru. Pokud není nainstalovaný .NET verze je .net Web Installer spuštění. Můžou pomoct odstranit s všech problémů, bude skript zaznamenat všechny aktivity do do souboru nazvaného *startuptasklog-(currentdatetime) txt* uložené v místním úložišti *InstallLogs* .

    > [AZURE.NOTE] Skript dál uvidíte, jak nainstalovat .NET 4.5.2 nebo .NET 4.6 pro kontinuitu. Není potřeba nainstalovat .NET 4.5.2 ručně, jako je už k dispozici v Azure hosta OS. A nikoli jen samotnou .NET 4.6 nainstalujete přímo .NET 4.6.1 kvůli [3118750 znalostní bázi Knowledge Base](https://support.microsoft.com/kb/3118750).
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfigurace diagnostiky přenášet protokoly spuštění úkolu úložiště objektů blob 
Zjednodušit odstraňování všech problémů, instalaci můžete nakonfigurovat diagnostiky přenášet soubory protokolu generovaných spouštěcí skript nebo .NET instalační program k úložišti objektů blob Azure. Tento přístup zobrazíte protokoly jednoduše stahování souborů v úložišti objektů blob spíše než museli Vzdálená plocha do role.

Konfigurace diagnostiky otevřít *diagnostics.wadcfgx* a přidejte následující uzlu **adresářů** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

To nastaví její konfiguraci azure diagnostiky přenášet všechny soubory v adresáři *protokolu* ve sloupci Zdroj *NETFXInstall* účtu úložiště Diagnostika v kontejneru objektů blob *netfx nainstalovat* .

## <a name="deploying-your-service"></a>Nasazení služby 
Při nasazení služby spuštění úkoly spusťte a nainstalujte .NET framework, pokud už není nainstalovaná. Vaše role budou v stav zaneprázdněn a rozhraní instaluje i-li může restartovat vyžaduje instalaci rámec. 

## <a name="additional-resources"></a>Další zdroje informací

- [Instalace .NET Framework][]
- [Postup: zjištění, které verze .NET Framework jsou nainstalovány][]
- [Poradce při potížích s instalací .NET Framework][]

[Postup: zjištění, které verze .NET Framework jsou nainstalovány]: https://msdn.microsoft.com/library/hh925568.aspx
[Instalace .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Poradce při potížích s instalací .NET Framework]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
