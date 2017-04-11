<properties
    pageTitle="Role pro web a pracovní PHP | Microsoft Azure"
    description="Příručka k vytváření PHP web a pracovní rolí v Azure cloudové službě a konfiguraci PHP runtime."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Jak vytvořit PHP web a pracovní role

## <a name="overview"></a>Základní informace

Tento průvodce vám ukáže, jak vytvořit PHP webu nebo pracovního role ve vývojovém prostředí Windows, vyberte konkrétní verzi PHP "předdefinované" verzích k dispozici, změnit konfigurace PHP, povolit rozšíření, a nakonec nasazení Azure. Také popisuje, jak nakonfigurovat webu nebo pracovního roli používat modul runtime PHP (s vlastní konfigurace a rozšíření), který zadáte.

## <a name="what-are-php-web-and-worker-roles"></a>Co je web a pracovníka rolí PHP?

Azure nabízí tři výpočet modelů na verzi pro spuštění aplikace: Azure aplikaci služby Azure virtuálních počítačích a Azure Cloud Services. Všechny tří modelů domovské stránce podpory PHP. Cloudové služby, které zahrnuje web a pracovníka role, poskytuje *platformu jako služba (PaaS)*. V rámci do cloudové služby role webu poskytuje vyhrazené webový server služby (internetové) u hostitele front-end webových aplikací. Role pracovníka je možné spouštět asynchronní dlouho probíhajících a uvedená časově neomezená úkoly nezávisle interakci uživatele nebo vstupní.

Další informace o těchto možnostech naleznete v tématu [Výpočet hostingu poskytovaným Azure](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Stažení Azure SDK pro PHP

[Azure SDK PHP] se skládá z některé komponenty. Tento článek budou používat dva: Azure PowerShell a Azure emulátorů. Tyto dvě složky je možné nainstalovat prostřednictvím platformy Microsoft Web. Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Vytvoření projektu Cloudovým službám

Prvním krokem při vytváření PHP webu nebo pracovního role je vytvořit projekt služby Azure. projekt služby Azure slouží jako logické kontejner pro web a pracovní role, která obsahuje soubory [definice služby (.csdef)] a [konfiguraci služby (.cscfg)] projektu.

Pokud chcete vytvořit nový projekt služby Azure, Azure PowerShell spustili s oprávněními správce a spusťte následující příkaz:

    PS C:\>New-AzureServiceProject myProject

Tento příkaz vytvoří nový adresář (`myProject`) do kterého můžete přidat web a pracovní role.

## <a name="add-php-web-or-worker-roles"></a>Přidání PHP webu nebo pracovního role

Role webového PHP do projektu přidáte spusťte tento příkaz z v kořenovém adresáři projektu:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Pro pracovní roli použijte tento příkaz:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] `roleName` Parametr je nepovinný krok. Pokud je vynechán, bude automaticky vygenerují název role. Bude první role web vytvořen `WebRole1`, bude druhý `WebRole2`a tak dál. První pracovní role vytvořené bude `WorkerRole1`, druhý bude `WorkerRole2`a tak dál.

## <a name="specify-the-built-in-php-version"></a>Určení předdefinované PHP verze

Do projektu přidáte PHP webu nebo pracovního role soubory konfigurace projektu upravena tak, aby PHP nainstaluje každé webu nebo pracovního instance aplikace při nasazení. Zobrazit verzi PHP nainstalované ve výchozím nastavení, spusťte tento příkaz:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Výstup z výše uvedených příkaz bude vypadat podobně jako obsah zobrazený dole. V tomto příkladu `IsDefault` příznak nastavena na `true` pro PHP 5.3.17, označující, že bude na výchozí verzi PHP nainstalovaný.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Verze modulu runtime PHP můžete nastavit, aby verze PHP, které jsou uvedené. Například nastavení verze PHP (role s názvem `roleName`) do 5.4.0, použijte tento příkaz:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Dostupných verzí PHP může v budoucnosti změnit.

## <a name="customize-the-built-in-php-runtime"></a>Přizpůsobení předdefinovaných runtime PHP

Abyste měli úplnou kontrolu nad nastavením runtime PHP, které máte nainstalované při postupujte podle výše uvedené kroky, včetně změny `php.ini` nastavení a povolení rozšíření.

Přizpůsobení předdefinované runtime PHP, postupujte takto:

1. Přidání nové složky, s názvem `php`, Komu `bin` adresáře webu role. Pro roli pracovní ho přidejte na roli kořenový adresář.
2. V `php` složku, vytvořte jinou složku s názvem `ext`. Umístění některou `.dll` soubory s příponou (například `php_mongo.dll`), kterou chcete povolit v této složce.
3. Přidat `php.ini` soubor, který má `php` složky. Povolit vlastní rozšíření a nastavte libovolné PHP směrnice v tomto souboru. Například, pokud chcete zapnout `display_errors` na a povolit `php_mongo.dll` příponu, obsah svého `php.ini` soubor vypadat následovně:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Nastavení, které není výslovně nastavené v `php.ini` soubor poskytnout bude automaticky nastavit výchozí hodnoty. Pokračujte v paměti, že můžete přidat úplný `php.ini` soubor.

## <a name="use-your-own-php-runtime"></a>Použití vlastní PHP runtime
V některých případech místo výběrem předdefinované runtime PHP a konfigurace jak jsme je popsali výše můžete zadat vlastní PHP runtime. Například můžete použít stejné runtime PHP v roli webu nebo pracovního využívající ve vývojovém prostředí. Usnadňuje zajistit, že aplikace nebude změnit chování v provozním prostředí.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Konfigurace webové roli, kterou chcete použít vlastní PHP runtime

Abyste mohli nakonfigurovat roli web používat modul runtime PHP, který zadáte, postupujte takto:

1. Vytvoření projektu služby Azure a přidejte roli web PHP výše popsaným v tomto tématu.
2. Vytvořit `php` složky v `bin` složky, ve které je v kořenovém adresáři web role a potom přidáte PHP runtime (všechny binární soubory, konfiguraci soubory, podsložky, atd.) `php` složky.
3. (VOLITELNÉ) Pokud PHP runtime používá [Microsoft ovladače PHP pro systém SQL Server][sqlsrv drivers], budete muset konfigurace vaše role web nainstalovat [SQL Server Native klienta 2012] [ sql native client] pokud ho máte k dispozici. K tomuto účelu přidat [sqlncli.msi x64 instalační program] `bin` složky v kořenovém adresáři web role. Spouštěcí skript popisuje v dalším kroku tiše spusťte instalační program, při zřízení roli. Pokud PHP runtime nepoužívá Microsoft Drivers PHP pro systém SQL Server, odeberte ze skriptu zobrazené v dalším kroku následující řádek:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definování spuštění úkol, který nakonfiguruje [Služby (internetové)] [ iis.net] používat PHP runtime pro zpracování žádostí o `.php` stránky. K tomuto účelu otevřete `setup_web.cmd` soubor (v `bin` soubor role webového kořenový adresář) v textovém editoru a její obsah nahraďte tento skript:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Soubory aplikace dodejte role webového kořenový adresář. To bude webový server kořenový adresář.

6. Publikování aplikace podle popisu v části [publikovat aplikace](#how-to-publish-your-application) .

> [AZURE.NOTE] `download.ps1` Skript (v `bin` složky role webového kořenový adresář) se dá odstranit po provedení kroků jsme je popsali výše pro používání vlastní PHP runtime.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Konfigurace pracovníka roli, kterou chcete použít vlastní PHP runtime

Abyste mohli nakonfigurovat roli pracovníka používat modul runtime PHP, který zadáte, postupujte takto:

1. Vytvořte projekt služby Azure a přidejte pracovní role PHP výše popsaným v tomto tématu.
2. Vytvoření `php` složky v kořenovém adresáři roli kolegy a potom přidáte PHP runtime (všechny binární soubory, konfiguraci soubory, podsložky, atd.) `php` složky.
3. (VOLITELNÉ) Pokud PHP runtime používá [Microsoft ovladače PHP pro systém SQL Server][sqlsrv drivers], budete muset konfigurace vaše role pracovníka nainstalovat [SQL Server Native klienta 2012] [ sql native client] pokud ho máte k dispozici. K tomuto účelu dodejte [sqlncli.msi x64 instalační] roli pracovníka kořenový adresář. Spouštěcí skript popisuje v dalším kroku tiše spusťte instalační program, při zřízení roli. Pokud PHP runtime nepoužívá Microsoft Drivers PHP pro systém SQL Server, odeberte ze skriptu zobrazené v dalším kroku následující řádek:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definování spuštění úkol, který se přidá svůj `php.exe` spustitelný roli pracovního prostředí PATH při zřizování roli. K tomuto účelu otevřete `setup_worker.cmd` soubor (v kořenovém adresáři roli pracovníka), v textovém editoru a jeho obsah nahraďte tento skript:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Soubory aplikace dodejte role pracovníka kořenový adresář.

6. Publikování aplikace podle popisu v části [publikovat aplikace](#how-to-publish-your-application) .

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Spusťte aplikaci v emulátorů výpočetním a úložiště

Azure emulátorů poskytují místním prostředí, ve kterém můžete otestovat Azure aplikace před nasazením do cloudu. Existují některé rozdíly mezi emulátorů a Azure prostředí. To lépe, najdete v tématu [použití emulátor Azure úložiště pro vývoj a testování](./storage/storage-use-emulator.md).

Všimněte si, že musíte mít PHP nainstalovaná místně můžete emulátoru výpočetním. Emulátoru výpočetním pomocí místních instalačních PHP spusťte aplikaci.

Spustit projektu v emulátorů, spusťte následující příkaz z kořenového adresáře projektu:

    PS C:\MyProject> Start-AzureEmulator

Zobrazí se výstup nějak takto:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Zobrazí se vaše aplikace spuštěné v emulátor kliknutím otevřete webový prohlížeč a přejděte na místní adresou zobrazenou v výstup (`http://127.0.0.1:81` do výstupu příkladu výše).

Emulátorů ukončíte spusťte tento příkaz:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Publikování aplikace

Publikování aplikace, budete muset nejdřív importovat vaše nastavení publikování pomocí rutiny [AzurePublishSettingsFile importovat](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Pak můžete publikovat aplikace pomocí rutiny [AzureServiceProject publikovat](https://msdn.microsoft.com/library/azure/dn495166.aspx) . Informace o přihlášení najdete v tématu [jak instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md).

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře PHP](/develop/php/).

[Azure SDK PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definice služby (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Konfigurace služby (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[Instalační program sqlncli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
