<properties
    pageTitle="Role web a pracovní Python s Visual Studio | Microsoft Azure"
    description="Základní informace o použití Python Tools for Visual Studio k vytvoření Azure cloudovým službám včetně web rolí a kolegy."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Role web a pracovní Python s Python Tools for Visual Studio

Tento článek obsahuje přehled použití Python web a pracovní role pomocí [Python Tools for Visual Studio][]. Se dozvíte, jak používat Visual Studio k vytvoření a nasazení základní cloudové služby, který používá Python.

## <a name="prerequisites"></a>Zjistit předpoklady pro

 - Visual Studio 2013 nebo 2015
 - [Python Tools for Visual Studio][] (PTVS)
 - [Azure SDK nástroje a 2013][] nebo [Azure SDK z a 2015][]
 - [Python 2.7 32bitová verze][] nebo [Python 3.5 32bitová verze][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Co je web a pracovní rolí Python?

Azure nabízí tři výpočet modelů na verzi pro spuštění aplikace: [Web Apps funkce v aplikaci služby Azure][execution model-web sites], [Azure virtuálních počítačích][execution model-vms]a [Azure Cloud Services][execution model-cloud services]. Všechny tří modelů domovské stránce podpory Python. Cloudové služby, včetně role web a pracovní poskytují *platformu jako služba (PaaS)*. V rámci do cloudové služby role webového poskytuje vyhrazené webový server služby (internetové) hostitele front-end webových aplikací roli pracovníka mohlo by umožnit spuštění asynchronní dlouho probíhajících a uvedená časově neomezená úkoly nezávisle interakci uživatele nebo vstupní.

Další informace najdete v tématu [Co je Cloudová služba?].

> [AZURE.NOTE]*Looking můžete vytvořit jednoduchý webu?*
Pokud nefunguje zahrnuje jenom jednoduché Web front-end, zvažte použití funkce lightweight Web Apps v aplikaci služby Azure. Můžete snadno upgradujete do cloudové služby roste do vašeho webu a změna vašim požadavkům. Viz <a href="/develop/python/">Středisko pro vývojáře Python</a> příspěvků, které tam rozvojem funkci Web Apps v aplikaci služby Azure.
<br />


## <a name="project-creation"></a>Vytvoření projektu

Ve Visual Studiu můžete vybrat **Cloudové služby Azure** v dialogovém okně **Nový projekt** v části **Python**.

![Dialogové okno Nový projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

V Průvodci Azure cloudové služby můžete vytvořit nový web a pracovní role.

![Dialogové okno služby Azure cloudu](./media/cloud-services-python-ptvs/new-service-wizard.png)

Šablona role pracovníka získáváte kód často používaný k připojení k účtu Azure úložiště nebo Bus služby Azure.

![Řešení služby cloudu](./media/cloud-services-python-ptvs/worker.png)

Kdykoli můžete přidat webu nebo pracovního role existující cloudové služby.  Je možné přidat existující projekty ve vašem řešení ani vytvářet nové.

![Přidání příkazu rolí](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Cloudová služba může obsahovat role implementovaná v různých jazycích.  Máte roli web Python implementovaná pomocí Django, s Python nebo se C# pracovníka role.  Můžete snadno komunikaci mezi vaše role pomocí služby Bus zpráv nebo fronty úložiště.

## <a name="install-python-on-the-cloud-service"></a>Nainstalovat Python do cloudové služby

>[AZURE.WARNING] Nastavení skripty, které jsou instalovány spolu Visual Studio (v době, kdy se naposledy aktualizovaly v tomto článku) nefungují. Tato část popisuje řešení.

Hlavní potíže s nastavením skripty se, že python není nainstalovat. Nejdřív definujte dva [úkoly při spuštění](cloud-services-startup-tasks.md) v souboru [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . Prvního úkolu (**PrepPython.ps1**) stáhne a nainstaluje modul runtime Python. Druhý úkol (**PipInstaller.ps1**) spustí pip nainstalovat nějaké závislosti, které by mohly být.

Dole skripty byly vytvořeny zacílení Python 3.5. Pokud chcete použít verzi 2.x python, nastavit proměnnou soubor **PYTHON2** **na** pro dva úkoly při spuštění a modulu runtime úkolu: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

Proměnné **PYTHON2** a **PYPATH** musí být přidán k úkolu spouštění pracovních. Proměnná **PYPATH** se používá pouze pokud proměnnou **PYTHON2** je nastavený **na**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Ukázka ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Dále vytvořte **PrepPython.ps1** a **PipInstaller.ps1** souborů v **. / intervalu** složky vaše role.

#### <a name="preppythonps1"></a>PrepPython.ps1

Tento skript nainstaluje python. Pokud je nastaveno proměnnou hodnotit **PYTHON2** **na** a pak Python 2.7 nainstaluje, jinak Python 3.5 nainstaluje.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Tento skript volá nahoru pip a všechny závislosti nainstaluje v souboru **requirements.txt** . Pokud proměnná hodnotit **PYTHON2** je nastavena **na** a pak Python 2.7 se použijí, v opačném případě bude použita Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Úprava LaunchWorker.ps1

>[AZURE.NOTE] V případě **pracovníka role** projektu je nutné **LauncherWorker.ps1** soubor spuštění spouštěcího souboru. V project **web role** soubor spuštění je definován místo ve vlastnostech projektu.

**Bin\LaunchWorker.ps1** byl vytvořen dělat spoustu přípravy, ale ve skutečnosti ho nefunguje. Nahraďte tento skript obsah v tomto souboru.

Tento skript zavolá **worker.py** soubor z python projektu. Pokud je nastaveno proměnnou hodnotit **PYTHON2** **na** a pak Python 2.7 se použijí, v opačném případě bude použita Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Šablony aplikace Visual Studio měli jste vytvořili **ps.cmd** souboru v **. / intervalu:** složky. Tohoto skriptu prostředí spojí se obálka skriptů Powershellu výše a poskytuje protokolování podle názvu prostředí PowerShell obálky s názvem. Pokud tento soubor nebyl vytvořen, přečtěte si, co by měl být v něm. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Spustit místně

Jestliže nastavíte svůj projekt služby cloudu jako projekt při spuštění a stisknutím klávesy F5, cloudovou službu poběží v místním Azure emulátoru.

Přestože PTVS podporuje spouštění v emulátor, ladění (například zarážky) nefunguje.

Ladění role pro web a pracovní, můžete nastavit projektu role jako projekt při spuštění a ladění, místo toho.  Můžete také nastavit více projektů po spuštění.  Klikněte pravým tlačítkem myši na řešení a potom vyberte **Nastavení spuštění projekty**.

![Vlastnosti projektu spuštění řešení](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Publikování na Azure

Pokud chcete publikovat, klikněte pravým tlačítkem projekt služby cloudu v řešení a vyberte **Publikovat**.

![Microsoft Azure publikovat přihlásit](./media/cloud-services-python-ptvs/publish-sign-in.png)

Postupujte podle pokynů průvodce. Pokud budete muset povolte Vzdálená plocha. Vzdálená plocha je užitečné, když potřebujete něco ladění.

Po dokončení konfigurace nastavení, klikněte na **Publikovat**.

Některé průběh se zobrazí v okně výstupu a potom se zobrazí okno Microsoft Azure aktivity protokolu.

![Okno protokol aktivity Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Nasazení bude trvat několik minut a potom webu nebo pracovního role budou pracovat na Azure!

### <a name="investigate-logs"></a>Prozkoumejte protokoly

Po virtuálního počítače cloudové služby spuštění systému a nainstaluje Python, podívejte se na protokoly k vyhledání všech zpráv o chybách. Tyto protokoly najdete v **C:\Resources\Directory\\{role} \LogFiles** složky. **PrepPython.err.txt** bude mít aspoň jednu chybu v něm z při pokusu skriptu zjišťování, pokud je nainstalovaná Python a **PipInstaller.err.txt** může stěžují zastaralé verzi pip.

## <a name="next-steps"></a>Další kroky

Podrobnější informace o práci s web a pracovníka role v nástrojích Python for Visual Studio najdete v dokumentaci PTVS:

- [Cloudové služby projekty][]

Další informace o použití Azure služeb z vaší webové pracovní rolí a, například pomocí úložišti Azure nebo služby Bus naleznete v následujících článcích.

- [Kulatý služby][]
- [Tabulku služba][]
- [Služba fronty][]
- [Service Bus fronty][]
- [Služba Bus témata][]


<!--Link references-->

[Co je Cloudová služba?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Kulatý služby]: ../storage/storage-python-how-to-use-blob-storage.md
[Služba fronty]: ../storage/storage-python-how-to-use-queue-storage.md
[Tabulku služba]: ../storage/storage-python-how-to-use-table-storage.md
[Service Bus fronty]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Služba Bus témata]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloudové služby projekty]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK nástroje a 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK nástroje pro a 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32bitová verze]: https://www.python.org/downloads/
[Python 3.5 32bitová verze]: https://www.python.org/downloads/
