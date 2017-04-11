<properties
    pageTitle="Visual Studio šablony pro Azure dávku | Microsoft Azure"
    description="Zjistěte, jak můžete pomocí těchto šablony projektů Visual Studio implementace a spustit dávku Azure pracovního vytížení náročné"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio šablony projektů pro list Azure

**Správce úloh** a **šablon aplikace Visual Studio úkolu procesoru** pro list zadejte kód, který vám pomohou při provádění a spusťte náročné úloh v listu s nejméně množství plánování řízené úsilí. Tento dokument popisuje tyto šablony a obsahuje pokyny k jejich použití.

>[AZURE.IMPORTANT] Tento článek popisuje pouze informace k dispozici tyto dva šablony a předpokládá, že znáte dávku služby a klíčové pojmy týkající se k němu: fondů, výpočet uzly projektů a úkolů, Správce úloh, proměnné a další relevantní informace. Další informace najdete v [Základy Azure dávku](batch-technical-overview.md) [dávku přehled funkcí pro vývojáře](batch-api-basics.md)a [začít pracovat s knihovnu Azure dávka pro .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Všeobecný přehled

Správce úloh a procesor úloh šablony můžete použít k vytvoření dvě užitečné složky:

* Správce úlohu implementující rozdělovač projektu, můžete rozdělí práci na více úkolů, které lze spustit nezávisle souběžně.

* Procesor úkolu, který slouží k provádění předzpracování a následné zpracování kolem příkazového řádku aplikace.

Například ve scénáři vykreslování videa rozdělovač úlohy by změní úlohy jednoho filmového stovky nebo tisíce samostatné úkoly, které chcete zpracovat jednotlivé snímky odděleně. Odpovídajícím způsobem procesor úkolu chcete spustit aplikaci vykreslování a všech závislá procesů, které jsou potřebné k vykreslování každý snímek, jakož i provádět žádné další akce (například kopírování vykreslená snímek do umístění pro uložení).

>[AZURE.NOTE] Šablony Správce úloh a procesor úkolu jsou nezávisle na sobě, tak, aby se rozhodnete sdělit nám obě nebo jenom jeden z nich, podle toho, požadavky výpočetním práce a na Předvolby.

Jak ukazuje následující obrázek, bude výpočetním projektu, který používá tyto šablony absolvovat tři fáze:

1. Kód klienta (například aplikace webové služby, atd.) odešle úlohu ke službě dávku v Azure určující jako její Správce úloh úkolů program Správce úloh.

2. Službu dávku běží Správce úlohu na uzel výpočetním a rozdělovač úlohy otevře zadaný počet úkol procesor úkoly na jako mnoho výpočet uzly podle potřeby na základě parametry a specifikace kód rozdělovač úlohy.

3. Procesor úkoly úkolu spustit nezávisle paralelně zpracování vstupních dat a generovat výstupní data.

![Diagram znázorňující interakci kód klienta se službou dávku][diagram01]

## <a name="prerequisites"></a>Zjistit předpoklady pro

Použití šablon dávku, budete potřebovat:

* Počítač s Visual Studio 2015 nebo novější, v něm už nainstalovaný.

* Dávkové šablon, které jsou dostupné v [Galerii Visual Studio] [ vs_gallery] jako rozšíření Visual Studia. Šablony můžete získat dvěma způsoby:

  * Instalace šablony pomocí dialogového okna **rozšíření a aktualizace** ve Visual Studiu (Další informace najdete v tématu [Vyhledání a použití Visual Studio Extensions][vs_find_use_ext]). V dialogovém okně **rozšíření a aktualizace** hledání a stáhněte si následující dvě rozšíření:

    * Azure Správce úloh dávka s rozdělovač projektu
    * Procesor pro platformu Azure dávku úkolu

  * Stažení šablon z Galerie online for Visual Studio: [Microsoft Azure dávku projektu šablony][vs_gallery_templates]

* Pokud plánujete používat funkci [Balíčků aplikací](batch-application-packages.md) pro nasazení Správce úloh a procesor úkolu s dávkou výpočet uzly, budete muset odkaz úložiště účtu k vašemu účtu dávku.

## <a name="preparation"></a>Příprava

Doporučujeme vytváření řešení, která může obsahovat vedoucím projektu, jakož i procesor úkolů, protože to můžete usnadnit její sdílení kód mezi váš správce úloh a programy procesor úkolů. Pokud chcete vytvořit toto řešení, postupujte takto:

1. Otevřete aplikaci Visual Studio 2015 a vyberte **soubor** > **Nový** > **projektu**.

2. V části **šablony**rozbalte **Jiné typy projektu**, klikněte na **Visual Studio řešení**a vyberte **Prázdné řešení**.

3. Zadejte název, který popisuje účel toto řešení (například "LitwareBatchTaskPrograms") a aplikace.

4. Pokud chcete vytvořit nové řešení, klikněte na **OK**.

## <a name="job-manager-template"></a>Správce úloh šablony

Správce úloh šablony můžete provádět správce úlohu, které můžete provádět následující akce:

* Rozdělte úlohy na více úkolů.
* Odeslání těchto úkolů spustit na listu.

>[AZURE.NOTE] Další informace o Správce úloh najdete v článku [Přehled funkcí dávka pro vývojáře](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Vytvoření vedoucím projektu pomocí šablony

Pokud chcete přidat správce úloh do řešení, které jste dříve vytvořili, postupujte takto:

1. Otevření existujícího řešení ve Visual Studiu 2015.

2. V okně Průzkumník, klikněte pravým tlačítkem myši na řešení, klikněte na tlačítko **Přidat** > **Nový projekt**.

3. V části **Visual Basic**klikněte na **cloudu**a potom klikněte na **Správce úloh Azure dávka s rozdělovač úlohy**.

4. Zadejte název, který popisuje aplikace a identifikuje tohoto projektu jako správce úloh (například "LitwareJobManager").

5. Pokud chcete vytvořit projektu, klikněte na **OK**.

6. Nakonec sestavte projekt vynutit Visual Studio načíst všechny odkazované balíčků NuGet a ověřte platnost projektu než začnete upravovat ho.

### <a name="job-manager-template-files-and-their-purpose"></a>Soubory šablony Správce úloh a jejich účel

Při vytváření projektu pomocí Správce úloh šablony generuje tří kategorií kód souborů:

* Hlavní programový soubor (Program.cs). Tato stránka obsahuje vstupní bod programu a zpracování nejvyšší úrovně výjimek. Neměli potřebujete běžným způsobem změnit.

* V rámci adresáři. Tato stránka obsahuje soubory zodpovědný za "často používaný text" práci provedenou program Správce úloh – rozbalování parametry, přidání úkolů do dávku atd. Neměli potřebujete obvykle změnit tyto soubory.

* Soubor rozdělovač úlohy (JobSplitter.cs). Toto je, kam jste umístili logiky specifická pro rozdělení úlohy na úkoly.

Samozřejmě můžete přidat další soubory podle potřeby na podporu kódu rozdělovač úlohy v závislosti na složitost úlohy rozdělení logiku.

Šablona také generuje standardní soubory projektu .NET například .csproj soubor, app.config, packages.config, atd.

Zbytek Tato část popisuje různé soubory a jejich kód struktury a vysvětluje, k čemu slouží jednotlivé třídy.

![Visual Studio Průzkumník řešení řešení šablony zobrazení Správce úloh][solution_explorer01]

**Soubory Framework**

* `Configuration.cs`: Zapouzdřuje načítání dat konfigurace úlohy například Podrobnosti o účtu dávku přihlašovací údaje účtu propojené úložiště, projektu a informace o úkolu a parametrů úlohy. Poskytuje taky přístup k definované dávku proměnné (viz nastavení prostředí pro úkoly v dokumentaci dávku) prostřednictvím Configuration.EnvironmentVariable předmětu.

* `IConfiguration.cs`: Abstrahuje provádění třídy konfigurace tak, aby bylo možné Jednotková testovací vaše úloha rozdělovač pomocí konfigurace falešné nebo mock objektu.

* `JobManager.cs`: Orchestrates komponent program Správce úloh. Je zodpovědný za Inicializace úlohy rozdělovač vyvoláním rozdělovač úlohy a odesílání úkoly vrácenou rozdělovač práce na úkolu odesílatele.

* `JobManagerException.cs`: Představuje chybu, které si žádá Správce úloh ukončíte. JobManagerException slouží k zalomit "očekávané" chyby kde konkrétní diagnostické informace lze zadat jako součást ukončení.

* `TaskSubmitter.cs`: Tento třídy odpovídá přidáním úkolů vrácenou rozdělovač úlohy na úlohy. Agregace třídy JobManager posloupnost úkolů do listů efektivní, ale včasná přidaná k projektu pak zavolá TaskSubmitter.SubmitTasks podprocesem pozadí pro každý list.

**Rozdělovač projektu**

`JobSplitter.cs`: Tento třídy obsahuje logiky specifická pro rozdělení úlohy na úkoly. Rámci vyvolá metodu JobSplitter.Split získat pořadí úkolů, které se přidají do projektu jako metoda vrátí je. Toto je třída, kde se vloží logickou práce. Implementace metody rozdělení vrátíte posloupnosti instancí CloudTask představující úkoly, do které chcete rozdělit úkoly.

**Standardní .NET příkazového řádku projektu soubory**

* `App.config`: Standardní .NET konfiguračního souboru aplikace.

* `Packages.config`: Soubor závislost balíčku standardní NuGet.

* `Program.cs`: Obsahuje vstupní bod programu a zpracování nejvyšší úrovně výjimek.

### <a name="implementing-the-job-splitter"></a>Provádění úloh příčky

Při otevření Správce úloh šablony projektu, bude mít projekt ve výchozím nastavení otevře soubor JobSplitter.cs. Logika rozdělení pro úkoly můžete ve vaší pracovní zátěž implementovat pomocí metody zobrazit Split() níže:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] V části s poznámkami `Split()` způsob je jenom část Správce úloh šablony kód, který je určená pro úpravy přidáním logiky úlohami rozdělením různých úkolů. Pokud chcete změnit jiné části šablony, zkontrolujte, zda jsou familiarized s fungování dávku a vyzkoušet si pár [ukázek kódu dávku][github_samples].

Implementace Split() má přístup k:

* Parametrů úlohy pomocí `_parameters` pole.
* Objekt CloudJob představující úlohy, prostřednictvím `_job` pole.
* Objekt CloudTask představující úlohu správce prostřednictvím `_jobManagerTask` pole.

Vaše `Split()` implementaci nemusí přímo přidání úkolů do projektu. Místo toho kódu, měly vrátit posloupnosti CloudTask objektů a tyto se přidají do projektu automaticky framework třídy, které vyvolat rozdělovač projektu. Je běžné používání C# na iterace (`yield return`) funkce pro provádění úloh příčky díky úloh, kterými spusťte systém co nejdříve, spíše než čeká se na všechny úkoly nutné přepočet.

**Chyba při rozdělovač projektu**

Pokud vaše úloha rozdělovač dojde k chybě, měli buď:

* Ukončení posloupnosti pomocí C# `yield break` použití příkazu funkce, ve kterém případ Správce úloh bude použit jako úspěšná. nebo

* Výjimku, ve kterém případ, který správce úloh se použije jako nebyl úspěšný a může v závislosti na klienta konfiguraci ho opakované).

V obou případech budou všechny úkoly již vrácené rozdělovač úlohy a přidá do úlohy nárok na spustit. Pokud nechcete, aby to bylo možné, pak můžete:

* Ukončit úlohu před vracení z rozdělovač projektu

* Formulace kolekci celý úkol před vrácení (to znamená, vrací `ICollection<CloudTask>` nebo `IList<CloudTask>` místo provádění vaše úloha rozdělovač pomocí iterace C#)

* Použít závislosti mezi úkoly, aby všechny úkoly jsou závislé na úspěšném dokončení Správce úloh

**Opakování Správce úloh**

Pokud správce úloh selže, může opakované službou dávku v závislosti na nastavení klienta opakovat. Obecně totiž bezpečný, když rozhraní přidá úkolů do projektu, ignoruje všechny úkoly, které již existují. Však při výpočtu úkoly drahé, které nechcete vzniknou náklady přepočet úkoly, které již byly přidány do zaměstnání, Naopak pokud opětovným spuštěním není možné zaručit generovat stejné ID úkolu potom chování "Ignorovat duplicity" nebude zahájení. V těchto případech měli navrhnout úlohy rozdělovač zjišťování práci, kterou už mělo a opakovat, například provedením CloudJob.ListTasks před spuštěním na úkoly.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Ukončete kódů a výjimky v šabloně Správce úloh

Kódy ukončení a výjimky poskytují mechanismus určit výsledek spuštění programu a umožňují identifikovat problémy s spuštění programu. Správce úloh šablony používá kódy ukončení a výjimky popsaná v této části.

Správce úlohu, který je implementovaná pomocí Správce úloh šablony můžete vrátit tři možné konec kódy:

| Kód | Popis |
|------|-------------|
| 0    | Správci úloha byla úspěšně dokončena. Kód rozdělovač projektu byl dokončen a všechny úkoly se přidala do projektu. |
| 1    | S výjimkou "očekávané" zčásti aplikace se nepovedlo správce úlohu. Výjimky byl přeložit JobManagerException diagnostické informace a, případně návrhy řešení chyby. |
| 2    | Správce úlohu nepovedlo se "". Výjimky byl přihlášení k lyncu na standardní výstup, ale Správce úloh se nepodařilo doplňte všechny požadované informace diagnostic nebo remediation. |

V případě úlohy správce úkol nepovede některé úlohy může pořád byly přidány ke službě před došlo k chybě. Tyto úkoly se spustí jako obvykle. Naleznete v tématu "Úlohy rozdělovač chyba" nad diskuse tato cesta kód.

Všechny informace vrácené výjimky zapisuje do stdout.txt a stderr.txt souborů. Další informace najdete v tématu [Zpracování chyb](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Informace o klientech

Tato část popisuje některé požadavky na klientské implementaci při vyvolání Správce úloh založeného na této šabloně. Zjistěte, [jak k předání parametry a proměnné z klientského kódu](#pass-environment-settings) podrobnosti o předávání parametry a nastavení prostředí.

**Povinné přihlašovací údaje**

Přidat úkoly Azure dávku, správce úlohu vyžaduje účet Azure dávku adresy URL a klíče. Musí uplynout v aplikaci proměnné s názvem YOUR_BATCH_URL a YOUR_BATCH_KEY. Můžete nastavit tyto ve Správci úloh nastavení prostředí úkolu. Například v jazyce C# klienta:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Úložiště pověření**

Klient obvykle, není nutné zadávat přihlašovací údaje účtu propojené úložiště pro správce úlohu, protože (a) nejčastěji vedoucí projektu není potřeba explicitně přístup k účtu propojené úložiště a (b) účtu propojené úložiště je často k dispozici pro všechny úkoly jako běžné nastavení prostředí projektu. Pokud nejsou poskytování propojené úložiště účtu pomocí běžného nastavení prostředí a Správce úloh vyžaduje přístup k základnímu úložišti propojené, pak zadejte propojené úložiště pověření následujícím způsobem:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Nastavení úkolů správce úloh**

Klient měli nastavit příznak *killJobOnCompletion* Správce úloh **false**.

To sice obvykle pro klienta nastavit *runExclusive* na **hodnotu false**.

Klient, používejte kolekci *resourceFiles* nebo *applicationPackageReferences* nechat úlohu správce spustitelný (a jeho požadované DLL) používaný výpočetní uzly.

Ve výchozím nastavení nebude Správce úloh opakovat, pokud se nezdaří. V závislosti na logiky úlohy správce klienta chtít povolit opakování přes *omezení*/*maxTaskRetryCount*.

**Úlohy nastavení**

Pokud rozdělovač úlohy posílá úkoly se závislostmi, klienta musíte nastavit projektu usesTaskDependencies true (pravda).

Úlohy rozdělovač modelu neobvyklé klientům chcete přidat úkoly k projektům nad rámec vytvoří rozdělovač projektu. Klient proto obvykle nastavte projektu *onAllTasksComplete* na **terminatejob**.

## <a name="task-processor-template"></a>Šablona procesor úkolu

Procesor úloh šablony můžete provádět procesor úkolu, který může provádět následující akce:

* Nastavte informace potřebné tak, že každý úkol dávku ke spuštění.
* Spusťte všechny akce vyžadované každý úkol dávku.
* Uložte úkolu výstupy trvalé úložiště.

I když procesor úkolu není vyžadované pro úkoly v listu, klíčové výhodou používání procesor úkolu je poskytuje Obálka provádět všechny akce provádění úkolů na jednom místě. Pokud například potřebujete k spuštění několika aplikací v kontextu jednotlivých úkolů nebo v případě potřeby zkopírujte data do trvalý úložiště po dokončení, jednotlivých úkolů.

Akce provádí procesoru úkolu může být jako jednoduché nebo složitá a tolik nebo málo podle potřeby tak, že vaše pracovní zátěž. Kromě toho implementací všechny akce úkolu do jednoho úkolu procesor můžete snadno aktualizovat nebo můžete přidat akce na základě změn aplikací ani pracovní zátěž požadavky. Však v některých případech úkolu procesor nemusí být optimální řešení pro implementaci podle toho, můžete přidávat nepotřebných složitosti, například při spuštění úlohy, které můžete rychle začít od jednoduchého příkazového řádku.

### <a name="create-a-task-processor-using-the-template"></a>Vytvoření úkolu procesor pomocí šablony

Přidání úkolu procesor do řešení, které jste dříve vytvořili, postupujte takto:

1. Otevření existujícího řešení ve Visual Studiu 2015.

2. V Průzkumníku klikněte pravým tlačítkem myši na řešení, klikněte na tlačítko **Přidat**a potom klikněte na **Nový projekt**.

3. V části **Visual Basic**klikněte na **Cloud**a klikněte na **Úkol procesor pro platformu Azure dávku**.

4. Zadejte název, který popisuje aplikace a identifikuje tohoto projektu jako procesor úkolu (například "LitwareTaskProcessor").

5. Pokud chcete vytvořit projektu, klikněte na **OK**.

6. Nakonec sestavte projekt vynutit Visual Studio načíst všechny odkazovaného balíčků NuGet a ověřte platnost projektu než začnete upravovat ho.

### <a name="task-processor-template-files-and-their-purpose"></a>Soubory šablony procesor úloh a jejich účel

Při vytváření projektu pomocí šablony procesoru úkolu generuje tří kategorií kód souborů:

* Hlavní programový soubor (Program.cs). Tato stránka obsahuje vstupní bod programu a zpracování nejvyšší úrovně výjimek. Neměli potřebujete běžným způsobem změnit.

* V rámci adresáři. Tato stránka obsahuje soubory zodpovědný za "často používaný text" práci provedenou program Správce úloh – rozbalování parametry, přidání úkolů do dávku atd. Neměli potřebujete obvykle změnit tyto soubory.

* Soubor procesoru úkolu (TaskProcessor.cs). Toto je, kam jste umístili logiky specifická pro provádění úkolu (obvykle tak, že volání na stávající spustitelný soubor). Před a po zpracování kód, například stahování další data a nahrávání souborů výsledek, půjde taky v tomto poli.

Samozřejmě můžete přidat další soubory podle potřeby na podporu kódu procesor úkolu v závislosti na složitost úlohy rozdělení logiky.

Šablony také generuje standardní .NET projektu například .csproj soubor, app.config, packages.config, atd.

Zbytek Tato část popisuje různé soubory a jejich kód struktury a vysvětluje, k čemu slouží jednotlivé třídy.

![Visual Studio Průzkumník řešení zobrazující řešení šablony procesor úkolu][solution_explorer02]

**Soubory Framework**

* `Configuration.cs`: Zapouzdřuje načítání dat konfigurace úlohy například Podrobnosti o účtu dávku přihlašovací údaje účtu propojené úložiště, projektu a informace o úkolu a parametrů úlohy. Poskytuje taky přístup k definované dávku proměnné (viz nastavení prostředí pro úkoly v dokumentaci dávku) prostřednictvím Configuration.EnvironmentVariable předmětu.

* `IConfiguration.cs`: Abstrahuje provádění třídy konfigurace tak, aby bylo možné Jednotková testovací vaše úloha rozdělovač pomocí konfigurace falešné nebo mock objektu.

* `TaskProcessorException.cs`: Představuje chybu, která vyžaduje Správce úloh ukončit. TaskProcessorException slouží k zalomit "očekávané" chyby kde konkrétní diagnostické informace lze zadat jako součást ukončení.

**Procesor pro platformu úkolu**

* `TaskProcessor.cs`: Spuštění úlohy. Rámci vyvolá metodu TaskProcessor.Run. Toto je třída, kde se vloží specifické pro aplikaci logickou úkolu. Implementace způsob spuštění:
  * Analyzovat a ověřte všechny parametry úkolu
  * Vytvořte příkazového řádku pro libovolný externí program, který chcete spustit
  * Protokolování diagnostiky informace, které může vyžadovat pro účely ladění
  * Spustit proces, který používá tuto příkazového řádku
  * Počkejte na ukončení procesu
  * Získání kódu ukončit proces lze zjistit, zda je úspěšný nebo
  * Uložit výstupní soubory, které chcete zachovat trvalé úložiště

**Standardní .NET příkazového řádku projektu soubory**

* `App.config`: Standardní .NET konfiguračního souboru aplikace.
* `Packages.config`: Soubor závislost balíčku standardní NuGet.
* `Program.cs`: Obsahuje vstupní bod programu a zpracování nejvyšší úrovně výjimek.

## <a name="implementing-the-task-processor"></a>Provedení úkolu procesor

Při otevření šablony projektu procesor úkolu bude mít projekt ve výchozím nastavení otevře soubor TaskProcessor.cs. Spuštění logiky pro úkoly, můžete ve vaší pracovní zátěž implementovat metodou Run() ukázáno v následujícím příkladu:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Oddílu s poznámkami metody Run() je jenom část kódu šablony procesoru úkolu, který je určená pro úpravy přidáním spuštění logiku úkolů ve vaší pracovní zátěž. Pokud budete chtít upravit jiné části šablony, zadejte nejprve seznamte s fungování dávku revizí dávku dokumentace a vyzkoušet pár ukázek kódu dávku.

Uložení výsledků a nakonec návratu s kódem konec je zodpovědný za spouštění příkazového řádku, spouštění jeden nebo více procesů, čekání na dokončení všech procesu metody Run(). Metoda Run() je nastavena na místo, kam implementace logiky zpracování úkolů. Procesor pro platformu framework úkolu vyvolá metodu Run() za vás; nemusíte volat sami.

Implementace Run() má přístup k:

* Parametry úkolu prostřednictvím `_parameters` pole.
* ID projektu a úkolu prostřednictvím `_jobId` a `_taskId` pole.
* Konfigurace úkolu prostřednictvím `_configuration` pole.

**Chyba při úkolu**

Když nepovede můžete k ukončení metody Run() vyvolání výjimky, ale to ponechá je nejvyšší úrovně obslužná rutina pod kontrolou kód ukončení úkolu. Pokud potřebujete řídit ukončovacího tak, aby se rozlišení různých typů nepovede, například pro účely diagnostiky nebo některé druhy poruch by měl ukončení projektu a ostatní neměli, má ukončit metody Run() vrácením nenulového ukončovacího. Toto je ukončovacího úkolu.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Ukončete kódů a výjimky v šabloně procesor úkolu

Kódy ukončení a výjimky poskytují mechanismus určit výsledek spuštění programu a pomáhají určit problémy s spuštění programu. Procesor pro platformu úloh šablony používá kódy ukončení a výjimky popsaná v této části.

Procesor pro platformu úkol, který implementovaná šablonou procesor úkolu se můžete vrátit tři možné konec kódy:

| Kód | Popis |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Procesor úkol byl dokončen. Všimněte si, že to, že program vyvolání byl úspěšný – pouze neznamená, že procesor úkolu vyvolala úspěšně a provést po zpracování bez výjimky. Význam ukončovacího závisí na programu vyvolané – obvykle ukončovacího 0 znamená, že bylo úspěšné program a jiné kódy konec znamená programu se nepodařilo. |
| 1    | Procesor úkolu skončil výjimku v očekávané"část aplikace. Výjimky byl přeložit do `TaskProcessorException` diagnostické informace a, případně návrhy řešení chyby. |
| 2    | Procesor úkolu nepovedlo se "". Výjimky byla přihlášena standardní výstup, ale procesoru úkolu se nepodařilo doplňte všechny požadované informace diagnostic nebo remediation. |

>[AZURE.NOTE] Program, který vyvoláte používá kódy ukončení 1 a 2 označíte režimy selhání, pomocí kódy ukončení 1 a 2 pro úkol procesor chyby při nejednoznačných. Kódy chyb procesor těchto úkolů charakteristické konec kódů můžete změnit úpravou výjimkou případů ve soubor Program.cs.

Všechny informace vrácené výjimky zapisuje do stdout.txt a stderr.txt souborů. Další informace naleznete v dokumentaci dávkové zpracování chyb.

### <a name="client-considerations"></a>Informace o klientech

**Úložiště pověření**

Pokud úkol procesor používá úložišti objektů blob Azure k zachování výstupy, například pomocí souboru konvence pomocné knihovny, pak potřebuje přístup k *buď* na cloudové úložiště klienta přihlašovací údaje *nebo* container objektů blob adresy URL, která obsahuje sdílený přístup podpis (přidružení zabezpečení). Šablona podporuje poskytnutí pověření prostřednictvím běžné proměnné. Klient lze následujícím způsobem předávat úložiště přihlašovacích údajů:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Účet úložiště bude k dispozici ve třídě TaskProcessor prostřednictvím `_configuration.StorageAccount` vlastnost.

Pokud chcete používat adresu URL kontejneru přidružením, můžete také předáte to prostřednictvím nastavení prostředí běžné úlohy, ale úkolu procesor šablona neobsahuje aktuálně předdefinované podporu.

**Nastavení úložiště**

Doporučujeme, aby úkol správce klienta nebo úlohy Vytvořit všechny kontejnery vyžadované úkoly před přidáním úkolů do projektu. Toto je povinný, že pokud použití adresy URL kontejneru přidružením jako takové adresa URL neobsahuje oprávnění k vytvoření kontejneru. Doporučujeme i v případě předáte přihlašovací údaje účtu úložiště, jako uloží každý úkol museli zavolat CloudBlobContainer.CreateIfNotExistsAsync na kontejner.

## <a name="pass-parameters-and-environment-variables"></a>Předání parametry a proměnných

### <a name="pass-environment-settings"></a>Nastavení prostředí průchodu

Informace o Správci úlohu ve formě nastavení prostředí můžete předat klienta. Tyto informace pak lze pomocí Správce úlohu při generování úkoly procesor úkolu, které se spustí v rámci projektu výpočetním. Příklady informace, které předáváte jako nastavení prostředí:

* Úložiště účet název účtu klávesám
* Adresa URL dávku účtu
* Klíč účtu dávku

Služba dávku obsahuje jednoduchý mechanismus předat nastavení prostředí pro správce úlohu pomocí `EnvironmentSettings` vlastnosti v [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Příklad, jak `BatchClient` instance dávku účtu, můžete předat jako proměnné z klienta kódu adresy URL a sdílené klíče pro ni přihlašovací údaje účtu dávku. Podobně pro přístup k účtu úložiště, který je propojený s klientem dávku, můžete přenést název účtu úložiště a klíč účtu úložiště jako proměnné.

### <a name="pass-parameters-to-the-job-manager-template"></a>Předat šabloně Správce úloh Parametry

V mnoha případech je užitečný pro předat správce úlohu, řízení projektu rozdělením obrázku nebo konfigurace úkoly pro práci na projektu parametry. Lze provést uložením souboru JSON jako soubor zdroje pro správce úlohu s názvem parameters.json. Parametry pak může být k dispozici v `JobSplitter._parameters` pole v šabloně Správce úloh.

>[AZURE.NOTE] Předdefinované parametr rutiny podporuje pouze řetězec řetězec slovníky. Pokud chcete předat komplexní JSON hodnoty jako hodnoty parametrů, bude muset předat jako řetězce a analyzovat v projektu příčky nebo upravit v rámci `Configuration.GetJobParameters` metody.

### <a name="pass-parameters-to-the-task-processor-template"></a>Předat parametry šabloně procesor úkolu

Můžete taky předáte parametry jednotlivé úlohy implementovaná pomocí šablony procesoru úkolu. Stejně jako se šablonou Správce úloh šablony procesor úkolů vyhledá zdrojový soubor s názvem

Parameters.JSON a když ho najít načte jako slovník parametrů. Existuje několik možností, jak předat parametry k úkolům procesor úkolu:

* Opakované použití parametrů úlohy JSON. To funguje taky v případě, že pouze parametrů jsou celého projektu, která (pro příklad na vykreslování na výšku a šířku). Uděláte to takto: při vytváření CloudTask v rozdělovač projektu, přidejte odkaz na objekt soubor parameters.json zdroje z úkolu Správce úloh ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) CloudTask ResourceFiles kolekce.

* Generovat a uložit dokument specifických úkolů parameters.json jako součást provádění rozdělovač úlohy a odkazovat tento objekt v kolekci souborů zdroje úkolu. Toto je nutné, pokud jiný úkoly mají různé parametry. Příkladem může být scénáři 3D vykreslování kde rejstřík rámeček předán úkolu jako parametr.

>[AZURE.NOTE] Předdefinované parametr rutiny podporuje pouze řetězec řetězec slovníky. Pokud chcete předat komplexní JSON hodnoty jako hodnoty parametrů, bude muset předat jako řetězce a analyzovat v editoru úkolu nebo upravit v rámci `Configuration.GetTaskParameters` metody.

## <a name="next-steps"></a>Další kroky

### <a name="persist-job-and-task-output-to-azure-storage"></a>Zachování výstupu projektu a úkolu k základnímu úložišti Azure

Je další užitečné nástroj ve vývoji řešení dávku [Azure dávkový soubor konvence][nuget_package]. Pomocí této .NET knihovně tříd (aktuálně v náhledu) v aplikacích dávku .NET snadno ukládat a načíst úkolu výstupy z Azure úložiště a. [Výstupní přetrvávají Azure dávku a úkolu](batch-task-output.md) obsahuje celou diskusi knihovnu a jeho použití.

### <a name="batch-forum"></a>Fórum komunity dávku

[Fórum komunity dávku Azure] [ forum] na webu MSDN je ideální místo k diskutovat o dávka se a položte otázky týkající se služby. Hlavy na přes pro užitečné "rychlé" příspěvky a pošlete svoje otázky, kterým dochází při vytváření dávku řešení.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
