<properties 
pageTitle="Spuštění úkoly při spuštění v Azure Cloudovým službám | Microsoft Azure" 
description="Spuštění úkoly slouží k přípravě prostředí cloudové služby aplikace. Tím se naučíte fungování spuštění úkolů a jak je" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Jak ke konfiguraci a spuštění úlohy ke cloudové službě

Spuštění úkoly slouží k provádění operací, před spuštěním roli. Operacích, které můžete provádět zahrnutí instalace součásti, registrace komponenty modelu COM, nastavení klíče registru a od dlouho spuštěný proces.

>[AZURE.NOTE] Spuštění úkoly platí není na virtuálních počítačích, pouze pro webové služby cloudu a pracovní role.

## <a name="how-startup-tasks-work"></a>Fungování spuštění úkoly

Spuštění úkoly jsou akce provedené před vaše role začátky a jsou definované v souboru [ServiceDefinition.csdef] pomocí prvek [úkolu] v rámci elementu [spuštění] . Často spuštění patří dávku soubory, ale mohou být konzoly aplikací ani dávku soubory, které zahájily skriptů Powershellu.

Proměnné předejte informace na úkolech při spuštění a místní úložiště mohou sloužit k předání informací mimo spuštění úkolu. Například proměnná prostředí můžete zadat cestu k programu, který chcete nainstalovat a soubory můžete být došlo k zápisu místní úložiště, který potom jde přečíst později tak, že vaše role.

Spuštění úkolu můžete zaznamenat informace a chyby do adresáře zadaného proměnná **TEMP** . Během spouštění úkolu proměnná **TEMP** změní *C:\\zdroje\\temp\\[guid]. [ Rolename]\\RoleTemp* adresář při provozu v cloudu.

Spuštění úkoly můžete taky spouštět několikrát mezi restartování počítače. Například úkolu spuštěním se spustí pokaždé, když recykluje roli a rolí recykluje nemusí vždy obsahovat restartovat počítač. Spuštění úlohy by se měly zapisovat způsobem, který umožňuje pracovat několikrát bez problémů.

Spuštění úlohy musí končit **if** (nebo s kódem) nula ke spouštění dokončete. Pokud úkol spuštění končí nenulového **if**, roli nejde spustit.


## <a name="role-startup-order"></a>Pořadí spuštění role

Následující postup při spuštění role v Azure:

1. Instanci označen jako **začátek** a neobdrží přenosy.

2. Všechny úkoly při spuštění zpracují podle jejich **taskType** atribut.
    - **Jednoduché** úkoly zpracují synchronní, jeden po druhém.
    - **Pozadí** a **popředí** úkoly jsou začali asynchronní, rovnoběžně s úkolem spuštění.  
       
    > [AZURE.WARNING] Služby IIS nemusí být nakonfigurován pro plně ve fázi úkolů při spuštění v procesu spuštění specifické dat nemusí být k dispozici. Spuštění úkoly, které vyžadují specifické dat používejte [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).

3. Spustit proces role Host (hostitel) a vytvoření webu ve službě IIS.

4. Volat metodu [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

5. Instanci označen jako **připravena** a směrovat přenosy v instanci.

6. Volat metodu [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) .


## <a name="example-of-a-startup-task"></a>Příklad spuštění úkolu

Spuštění úkoly jsou definované v souboru [ServiceDefinition.csdef] v elementu **úkolu** . Atribut **Příkazový řádek** Určuje název a parametry spouštěcí dávkový soubor nebo konzoly příkaz atribut **executionContext** určuje úroveň oprávnění spuštění úkolu a atribut **taskType** Určuje, jak bude spuštěn úkol.

V tomto příkladu je proměnná prostředí **MyVersionNumber**vytvořené pro daný úkol při spuštění a nastavena na hodnotu "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

V následujícím příkladu dávkový soubor **Startup.cmd** data zapisuje řádku "aktuální verze není 1.0.0.0" k souboru StartupLog.txt v adresáři nastavil proměnná TEMP. `EXIT /B 0` Řádek zajišťuje úkolu spuštění končí **if** nula.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Ve Visual Studiu by měl vlastnost **zkopírujte do adresáře výstup** pro spuštění dávkový soubor nastavena na **Kopírovat vždy** Ujistěte se, že spouštěcí dávkový soubor správně nasazena do projektu na Azure (**approot\\Koš** Web rolí a **approot** pro pracovní role).

## <a name="description-of-task-attributes"></a>Popis úkolu atributy

V následujícím textu najdete atributy prvek **úkolu** v souboru [ServiceDefinition.csdef] :

**Příkazový řádek** - určuje příkazového řádku pro daný úkol při spuštění:

- Příkaz, s parametry volitelný přepínač příkazového řádku, který nebude zahájen úkol po spuštění.
- Často to je název cmd nebo bat dávkový soubor.
- Úkol je relativní AppRoot\\složky Koš nasazení. Proměnné nejsou rozbaleny při určování cesta a od úkolu. Pokud rozbalování prostředí je potřeba, můžete vytvořit malé cmd skript, který volá spuštění úkolu.
- Může být aplikace konzoly nebo dávkový soubor, který spustí [skript Powershellu](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** - určuje úroveň oprávnění pro daný úkol po spuštění. Úroveň oprávnění můžete omezený nebo zvýšenými:

- **omezené**  
Úkol spuštěním se spustí s stejná oprávnění jako role. Po **executionContext** atribut [Runtime] element se taky **omezené**se používají uživatelská oprávnění.

- **zvýšenými oprávněními**  
Úkol spuštěním se spustí s oprávněními správce. Díky spuštění úlohy nainstalovat aplikace, proveďte změny konfigurace služby IIS, proveďte změny v registru a další úroveň úkoly správce bez zvýšit úroveň oprávnění role samotný.  

> [AZURE.NOTE] Úroveň oprávnění úkolu spuštění nemusí být stejné jako roli samotný.

**taskType** - určuje způsob spuštění úlohy.

- **jednoduché**  
Úkoly synchronní, zpracují postupně po jednom, v pořadí podle [ServiceDefinition.csdef] soubor. Pokud jeden úkol **jednoduché** spuštění končí **if** nula, se spustí další **jednoduchý** úkolu při spuštění. Pokud nejsou žádné další **jednoduché** úkoly při spuštění provést, bude spuštěn roli samotné.   

    > [AZURE.NOTE] Pokud **jednoduchý** úkolu končí nenulového **if**, budou blokovány instance. Následující **jednoduchou** spuštění úkoly a role, nejde spustit.

    Abyste měli jistotu, že dávkový soubor má na konci **if** nulovou, provedení příkazu `EXIT /B 0` na konci procesu dávkový soubor.

- **pozadí**  
Úkoly jsou asynchronní, spouštět souběžně s spuštění roli.

- **popředí**  
Úkoly jsou asynchronní, spouštět souběžně s spuštění roli. Klíčové rozdíl mezi **popředí** a **pozadí** úkolu se vypočítá následujícím úkolu **popředí** zabrání roli z recyklace nebo vypnutí, dokud úkol dokončen. Úlohy na **pozadí** nemají toto omezení.

## <a name="environment-variables"></a>Proměnné

Proměnné se předávat informace k spuštění úkolu. Například můžete dát cesta objektů blob obsahující program, který chcete nainstalovat, čísla portů využívající vaše role nebo nastavení ovládacích prvků při spuštění úkolu.

Existují dva typy proměnných prostředí pro spuštění úkoly. statická proměnné a proměnné založené na členy třídy [RoleEnvironment] . Jsou oba prvky v [prostředí] část souboru [ServiceDefinition.csdef] a používají atribut prvek a **název** [proměnné] .

Statická proměnné používá atribut **value** elementu [proměnné] . Výše uvedený příklad vytvoří proměnnou prostředí **MyVersionNumber** , který má statická hodnota "**1.0.0.0**". Jiný příklad bude proměnná **StagingOrProduction** prostředí, které můžete ručně nastavte hodnoty "**přípravu**" nebo "**výrobní**" provádět jiné spuštění akce na základě hodnoty proměnnou prostředí **StagingOrProduction** vytvořením.

Proměnné založené na členy třídy RoleEnvironment nepoužívejte atribut **value** elementu [proměnné] . Místo toho podřízený prvek [RoleInstanceValue] s příslušnou hodnotou atribut **XPath** slouží k vytvoření proměnná prostředí podle konkrétního člena třídy [RoleEnvironment] . Hodnoty pro atribut **výraz XPath** pro přístup k různé hodnoty [RoleEnvironment] najdete [tady](cloud-services-role-config-xpath.md).



Například vytvořit proměnná prostředí, které je "**true**" instance běží ve výpočetním emulátoru a "**false**" při spuštění v cloudu, můžete tyto prvky [proměnné] a [RoleInstanceValue] :

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Další kroky
Zjistěte, jak provádět některé [Běžné úkoly při spuštění](cloud-services-startup-tasks-common.md) pomocí cloudové služby.

[Balíček](cloud-services-model-and-package.md) cloudové služby.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Úkol]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Při spuštění]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Za běhu]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Prostředí]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Proměnná]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx