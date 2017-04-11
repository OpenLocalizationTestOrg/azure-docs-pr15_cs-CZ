<properties
    pageTitle="Instalace snadno aplikací a řízení v Azure dávku | Microsoft Azure"
    description="Použití funkce balíčky aplikace Azure listu snadno spravovat více aplikací a verze pro instalaci na listu výpočet uzlů."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Nasazení aplikace s balíčků aplikací dávku Azure

Funkce balíčky aplikace Azure listu obsahuje snadno Správa aplikací úkolu a jejich nasazení výpočetních uzlů v vašeho fondu. S balíčků aplikací můžete odeslat a spravovat více verzích aplikace, které dostanete úkolů, včetně jejich podpůrné soubory. Můžete pak automaticky nasadit jednu nebo víc z těchto aplikací na výpočetním uzly ve vaší fondu.

V tomto článku se dozvíte, jak nahrávat a Správa balíčků aplikací na portálu Azure. Můžete pak dozvíte, jak nainstalovat uzlech do fondu výpočetních s [Dávku .NET] [ api_net] knihovny.

> [AZURE.NOTE] Funkce balíčky aplikace zde popsané nahrazuje funkci "Dávku aplikace" k dispozici v předchozích verzích aplikace služby.

## <a name="application-package-requirements"></a>Požadavky na balíček aplikace

[Odkaz účet Azure úložiště](#link-a-storage-account) , musíte ke svému účtu dávku používat balíčků aplikací.

Funkce balíčky aplikace popisované v tomto článku je kompatibilní *pouze* s fondů dávku, které jsou vytvořené po 10 březen 2016. Balíčků aplikací nesmí být nasazené na výpočet uzlů v fondů vytvořené před tímto datem.

Tato funkce byla zavedená v [Dávku REST API] [ api_rest] verze 2015 12 01.2.2 a odpovídající [Dávku .NET] [ api_net] verze knihovny 3.1.0. Doporučujeme, abyste při práci s listem vždy používat nejnovější verze rozhraní API.

> [AZURE.IMPORTANT] V současné době pouze *CloudServiceConfiguration* fondů podporují balíčků aplikací. Balíčků aplikací nelze použít v fondů vytvořený s využitím VirtualMachineConfiguration obrázky. V tématu [virtuálního počítače](batch-linux-nodes.md#virtual-machine-configuration) o [poskytování Linux výpočet uzlů v Azure dávku fondů](batch-linux-nodes.md) Další informace o dva různé konfigurace.

## <a name="about-applications-and-application-packages"></a>O aplikacích a balíčků aplikací

V rámci Azure dávku *aplikace* odkazuje na sadu verzí binární soubory, které můžete automaticky stáhnout do výpočetního uzlů v vašeho fondu. Pro *balíčku aplikace* odkazuje na *konkrétní sadu* binární data a představuje dané *verzi* aplikace.

![Uceleném diagram aplikací a balíčků aplikací][1]

### <a name="applications"></a>Aplikace

Aplikace v listu obsahuje jednu nebo více aplikací balíčky a určuje možnosti konfigurace aplikace. Například aplikace můžete určit výchozí verze balíček aplikace nainstalovat na výpočetních uzly a zda lze jeho balení aktualizace nebo odstranit.

### <a name="application-packages"></a>Balíčků aplikací

Balíčku aplikace je zip soubor, který obsahuje binární soubory aplikace a podpůrné soubory, které jsou potřeba pro spuštění podle svých úkolů. Každý balíčku aplikace představuje určitou verzi aplikace.

Můžete zadat balíčků aplikací na úrovni fondu a úkol. Můžete použít jednu nebo víc z těchto balíčků a (volitelně) verzi při vytváření fondu nebo úkolů.

* **Balíčky fondu aplikací** jsou nasazeny *všech* uzlů v fondu. Zavedení uzel připojí fond a pokud, je restartování nebo reimaged.

    Balíčky fondu aplikací jsou vhodné v případě stačí fond spustit úkolů projektu. Jeden nebo více balíčků aplikací můžete určit, když vytvoříte fond a můžete přidat nebo aktualizovat existující fond balíčků. Pokud aktualizujete balíčky existující fondu aplikací, po restartování jeho uzlů nainstalovat nový balíček.

* **Balíčků aplikací úkolu** jsou nasazeny pouze výpočetní uzly provádět úkol, jednoduše před spuštěním úkolu příkazového řádku. Je-li balíčku zadaný aplikace a verzi už na uzel, není znovu nasadit a bude použito stávající balíček.

    Balíčků aplikací úkolu jsou užitečné sdílí fond prostředí, kde různé úlohy se spouštějí na jeden fond a fondu není odstraněna po dokončení projektu. Pokud práce má sníženou úkoly než uzly ve fondu, balíčků aplikací úkolu Minimalizovat přenos dat od používaný aplikace pouze uzly spuštěné úkoly.

    Další scénáře, které můžete využít balíčků aplikací úkolu jsou úlohy, které používají zejména velké aplikace, ale jenom malým počtem poštovních úkoly. Například předem zpracování fáze nebo sloučení úkolu, kde je aplikace předzpracování nebo sloučení těžké.

> [AZURE.IMPORTANT] Existuje omezení počtu aplikací a balíčků aplikací v rámci účet dávku, jakož i velikost balíčku maximální aplikace. Další informace o těchto omezení najdete v článku [kvót a limity pro službu Azure dávku](batch-quota-limit.md) .

### <a name="benefits-of-application-packages"></a>Výhody balíčků aplikací

Balíčků aplikací můžete zjednodušit kód ve vašem dávku řešení a snížení režijních povinné ke správě aplikace spuštěné úkolů.

Váš fond zahájení úkolu nemá k určení dlouhý seznam souborů konkrétního zdroje, které chcete nainstalovat uzlů. Nemusíte ručně spravovat více verzích aplikace souborů v úložišti Azure nebo na uzly. A nemusíte se bát generování [Adresy URL přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md) zajistit přístup k souborům ve vašem účtu úložiště. Dávkové funguje na pozadí s úložištěm Azure ukládat balíčků aplikací a jejich pro výpočet uzlech nasazení.

## <a name="upload-and-manage-applications"></a>Nahrávání a Správa aplikací

Můžete použít [Azure portál] [ portal] nebo do jiné knihovny [Dávku správy .NET](batch-management-dotnet.md) ke správě balíčků aplikací ve vašem účtu dávku. V části Další několik jsme nejdřív připojit k účtu úložiště, a pak diskutovat o přidání aplikací a balíčků a správa pomocí portálu.

### <a name="link-a-storage-account"></a>Propojení s účtem úložiště

Použití aplikace balíčků, je nutné nejprve propojit účet Azure úložiště ke svému účtu dávku. Pokud jste to ještě ještě účet úložiště pro váš účet dávku, portálu Azure se zobrazí upozornění při prvním kliknutí na dlaždici **aplikace** v zásuvné **dávku účtu** .

> [AZURE.IMPORTANT] Dávkové aktuálně podporuje *pouze* typ účtu úložiště **univerzální** podle pokynů v kroku 5, [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account), [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Kdy se propojení Azure úložiště klienta dávku účtu, odkaz *pouze* účtu úložiště **univerzální** .

![Bez upozornění úložiště účet nakonfigurovaný Azure portálu][9]

Služba dávku používá přidružený účet úložiště pro ukládání a vyhledávání balíčků aplikací. Poté, co jste propojili dva účty, můžete dávku automaticky nasazení balíčků uložené v propojený klient úložiště do výpočetního uzlů. Klikněte na **Nastavení účtu úložiště** na zásuvné **upozornění** a potom klepněte na **Účet úložiště** na zásuvné **Účtu úložiště** k propojení s účtem úložiště ke svému účtu dávku.

![Zvolte zásuvné účtu úložiště Azure portálu][10]

Doporučujeme vytvořit úložiště účtu *speciálně* pro použití s účtem dávku a vyberte ho v tomto poli. Podrobnosti o tom, jak vytvořit účet úložiště najdete v tématu "úložiště v vytvořit účet" [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Po vytvoření účtu úložiště, můžete pak propojit ho ke svému účtu dávku pomocí zásuvné **Účtu úložiště** .

> [AZURE.WARNING] Protože dávku používá k ukládání balíčků aplikací Azure úložiště, můžete se [Účtovaná jako normálně] [ storage_pricing] pro blok objektů blob data. Je potřeba vzít v úvahu velikost a počet balíčků aplikací a pravidelně odebrání nepoužívaných balíčků minimalizovat náklady.

### <a name="view-current-applications"></a>Zobrazit aktuální aplikace

Aplikace ve vašem účtu dávku zobrazíte kliknutím na položku nabídky **aplikace** v levé nabídce během zobrazování zásuvné **dávku účtu** .

![Dlaždice aplikace][2]

Otevře se zásuvné **aplikací** :

![Seznam aplikací][3]

**Aplikace** zásuvné zobrazí ID každé aplikace váš účet a následující vlastnosti:

* **Balíčky**– počet verzí přidružené k této aplikaci.
* **Výchozí verze**– verze nainstalované Pokud nastavíte žádost o fond nezadáte verze. Toto nastavení je nepovinný krok.
* **Povolit aktualizace**– hodnota, která určuje, zda balíček aktualizace, odstranění a přidání jsou povoleny. Pokud to je nastavena na hodnotu **Ne**, budou zakázány balíček aktualizace a odstranění aplikace. Můžete přidat jenom nové verze balíček aplikace. Výchozí hodnota je **Ano**.

### <a name="view-application-details"></a>Zobrazení podrobností o aplikaci

Klikněte na aplikace v **aplikacích** zásuvné otevřete zásuvné, který obsahuje podrobné informace o této aplikaci.

![Podrobnosti o aplikaci][4]

V zásuvné podrobnosti aplikace můžete nakonfigurovat následující nastavení pro aplikaci.

* **Povolit aktualizace**- určete, zda jeho balíčků aplikací lze aktualizovat nebo odstranit. Dál v tomto článku najdete v článku "Aktualizace nebo odstranění balíčku aplikace".
* **Výchozí verze**- určete výchozí balíčku aplikace pro nasazení pro výpočet uzlů.
* **Zobrazovaný název**- určete "popisný" název, který dávku řešení lze použít při zobrazuje informace o aplikaci, jako je třeba v uživatelském rozhraní služby, které poskytují zákazníky prostřednictvím dávku.

### <a name="add-a-new-application"></a>Přidat novou aplikaci

Pokud chcete vytvořit novou aplikaci, přidejte balíčku aplikace a zadejte ID aplikace nový, jedinečný. První balíčku aplikace, které přidáte s novým ID aplikace taky vytvořit nové aplikace.

Klikněte na **Přidat** na zásuvné **aplikací** otevřete zásuvné **nové aplikace** .

![Nové aplikace zásuvné Azure portálu][5]

**Nové aplikace** zásuvné obsahuje následující pole k určení nastavení balíčku aplikace a novou aplikaci.

**Id aplikace**

Toto pole identifikátor nové aplikace, což je vyměřené poplatky za jeho standardní Azure dávku ID ověřovacích pravidel:

* Může obsahovat libovolné kombinace alfanumerických znaků, včetně pomlčky a podtržítka.
* Nesmí obsahovat maximálně 64 znaků.
* Musí být jedinečný v rámci dávku účtu.
* Je případu zachovávání a písmena malá a velká písmena.

**Verze**

Určuje verzi balíčku aplikace, které odesíláte. Vyměřené poplatky za jeho následujících pravidel ověření jsou řetězce verze:

* Může obsahovat libovolné kombinace alfanumerických znaků, včetně pomlčky, podtržení a období.
* Nesmí obsahovat maximálně 64 znaků.
* Musí být jedinečný v rámci aplikace.
* Zachování a nerozlišují malá a velká první písmena.

**Balíček aplikace**

Toto pole udává souboru ZIP obsahujícího binární soubory aplikace a podpůrné soubory, které jsou potřebné ke spuštění aplikace. Klikněte na pole **Vyberte soubor** nebo na ikonu složky a vyhledejte a vyberte zip soubor, který obsahuje soubory aplikace.

Po výběru souboru klepnutím na **OK** spusťte nahrávání k základnímu úložišti Azure. Po dokončení operace nahrávání se vám zobrazí upozornění a zásuvné se zavře. V závislosti na velikosti souboru, který se odesílá a rychlosti připojení k síti může chvíli trvat operaci.

> [AZURE.WARNING] Před dokončením nahrávání operace nezavírejte zásuvné **nové aplikace** . Tím se zastavit nahrávání.

### <a name="add-a-new-application-package"></a>Přidání nového balíčku aplikace

Chcete-li přidat novou verzi balíčku aplikace pro existující aplikace, v zásuvné **aplikací** vyberte aplikaci, klikněte **balíčků**a potom na **Přidat** otevřete zásuvné **Přidat balíček** .

![Přidání zásuvné balíček aplikace Azure portálu][8]

Jak můžete vidět, pole odpovídají zásuvné **novou aplikaci** , ale je zakázán pole **id aplikace** . Stejně jako nové aplikace určení **verze** pro nový balíček, najděte soubor ZIP **balíčku aplikace** a potom klikněte na **OK** ho nahrajte balíček.

### <a name="update-or-delete-an-application-package"></a>Aktualizace nebo odstranění balíčku aplikace

Aktualizace nebo odstranění existující balíčku aplikace, otevřete zásuvné podrobnosti pro aplikaci, klikněte na **balíčků** otevřete zásuvné **balíčků** kliknutím na **tlačítko se třemi tečkami** v řádku balíčku aplikace, kterou chcete upravit a vyberte akci, kterou chcete provést.

![Aktualizace nebo odstranění balíčku Azure portálu][7]

**Aktualizace**

Když kliknete na **Aktualizovat**, zobrazí se zásuvné *balíček aktualizace* . Tento zásuvné podobá zásuvné *nový balíček aplikace* , ale jenom výběru pole balíčku aktivované, umožňuje zadat nový ZIP soubor k nahrání.

![Aktualizace balíčku zásuvné Azure portálu][11]

**Odstranění**

Když kliknete na tlačítko **Odstranit**, budete vyzváni k potvrzení odstranění verze balíčku a dávku odstraní balíček z Azure úložiště. Pokud byste odstranili na výchozí verzi aplikace, nastavení **Výchozí verze** odebírá z aplikace.

![Odstranění aplikace][12]

## <a name="install-applications-on-compute-nodes"></a>Instalace aplikací na výpočet uzlů

Teď jste viděli, jak spravovat balíčků aplikací pomocí portálu Azure, můžete probereme tato témata nasazení, aby výpočet uzly a spusťte s dávku úkoly.

### <a name="install-pool-application-packages"></a>Instalace balíčků fondu aplikací

Instalace balíčku aplikace ve všech výpočetních uzlech fond, zadejte jeden nebo více aplikací balíčku *odkazy* fondu. Balíčků aplikací, které zadáte fondu nainstalovaných v jednotlivých uzlech výpočetním uzel spojí fondu a pokud, uzel restartování nebo reimaged.

V dávku .NET určit jeden nebo více [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] při vytváření nového fondu nebo u existující fond. [ApplicationPackageReference] [ net_pkgref] třídy určuje ID aplikace a verzi nainstalovat do fondu výpočet uzlů.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Selhání nasazení balíčku aplikace z nějakého důvodu službu dávku označí uzel [nelze použít][net_nodestate], a pro spuštění na uzel naplánuje žádné úkoly. V tomto případě je nutné **Restartovat** uzel k novým nasazení balíčku. Plánování úkolů na uzel znovu můžete taky povolit restartování uzel.

### <a name="install-task-application-packages"></a>Instalace balíčků aplikací úkolu

Podobně jako fond zadáte balíčku aplikace *odkazy* pro daný úkol. Při plánování úkolu na uzel balíček stáhnout a extrahovaných jenom před provedením úkolu příkazového řádku. Pokud zadaný balíček a verze je už nainstalovaná na uzel, nestahuje balíčku a bude použito stávající balíček.

Balíček aplikace úkolu nainstalovat, nakonfigurovat úkolu [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] vlastnosti:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Spuštění nainstalovaných aplikací

Stahování a extrahovaných adresář s názvem v rámci balíčků, které jste zadali fondu úloha `AZ_BATCH_ROOT_DIR` uzlu. Dávku můžete vytvořit taky proměnná prostředí, která obsahuje cestu k adresář názvem. Příkazového řádku úkolu použijte tuto proměnnou prostředí při odkazování na aplikaci na uzel. Proměnná je v tomto formátu:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`a `version` jsou hodnoty, které odpovídají verzi aplikace a balíček jste zadali nasazení. Například, pokud je určený tuto verzi 2.7 aplikace *mixér* musí být nainstalovaný, příkazového řádku úkolu byste měli použít tuto proměnnou prostředí pro přístup k jeho soubory:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Pokud chcete zadat výchozí verze aplikace, je možné vynechat přípony verze. Pokud nastavíte jako výchozí verze aplikace *mixér*"2.7", úkolů můžete odkázat takto proměnnou a provádějí verze 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Následující fragment kódu ukazuje příklad úkolu příkazového řádku, který se spustí na výchozí verzi aplikace *mixér* :

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] V tématu [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) v [Přehled funkcí dávku](batch-api-basics.md) Další informace o nastavení prostředí uzel výpočetních.

## <a name="update-a-pools-application-packages"></a>Aktualizace fondu a balíčků aplikací

Pokud existující fond nakonfigurované už s balíčku aplikace, můžete zadat nový balíček pro fondu. Pokud zadáte nový odkaz balíček pro fond splnění následujících podmínek:

* Všechny nové uzlů, které připojení fondu a všechny existující uzel, který je restartování nebo reimaged nainstaluje se nově zadaný balíček.
* Výpočet, že uzlech, které jsou již ve fondu při aktualizaci odkazy na balíček nenainstaluje automaticky nový balíček aplikace. Výpočet tyto uzly musí být restartování nebo reimaged přijímat nový balíček.
* Při nasazení nový balíček vytvořené proměnné obsahovat odkazy na nový balíček aplikace.

V tomto příkladu je existující fond verze 2.7 aplikaci *mixér* nakonfigurování jako jednu z jejích [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Pokud chcete aktualizovat do fondu uzly s verzí 2.76b, zadejte novou [ApplicationPackageReference] [ net_pkgref] s novou verzí a potvrdit změny.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Teď nakonfigurované novou verzi, bude *Nový* uzel, který propojuje fondu verze 2.76b používaný k němu. Pokud chcete nainstalovat 2.76b v jednotlivých uzlech, které jsou *již* ve fondu, restartujte nebo reimage je. Všimněte si, že restartovaném uzly zachová soubory z předchozích nasazení balíčku.

## <a name="list-the-applications-in-a-batch-account"></a>Seznam aplikací v dávku účtu

Seznam aplikací a jejich balíčků účet dávku pomocí [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] metody.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Zalomit

S balíčků aplikací můžete předejít zákazníkům vyberte žádosti o svoji práci a určete přesnou verzi používat při zpracování úlohy s vašimi službami dávkách. Můžete vytvořit taky možnost pro vaši zákazníci k odesílání a sledovat své vlastní aplikace ve službě.

## <a name="next-steps"></a>Další kroky

* Rozhraní [REST API dávku] [ api_rest] také poskytuje podporu pro práci s balíčků aplikací. Například, najdete v článku [applicationPackageReferences] [ rest_add_pool_with_packages] prvek [Přidat fond s klientem] [ rest_add_pool] informace o tom, jak určit balíčků instalace pomocí rozhraní REST API. Podívejte se na [aplikace] [ rest_applications] podrobné informace o tom, jak získat informace o aplikaci pomocí rozhraní REST API dávku.

* Zjistěte, [Správa účtů dávku Azure a kvóty dávku správy .NET](batch-management-dotnet.md). [Správa .NET dávku] [ api_net_mgmt] knihovny můžete povolit účtu vytváření a odstraňování funkce dávku aplikace nebo služby.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Diagram souhrnně balíčků aplikací"
[2]: ./media/batch-application-packages/app_pkg_02.png "Dlaždice aplikace Azure portálu"
[3]: ./media/batch-application-packages/app_pkg_03.png "Aplikace zásuvné Azure portálu"
[4]: ./media/batch-application-packages/app_pkg_04.png "Aplikace zásuvné podrobností portálu Azure"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nové aplikace zásuvné Azure portálu"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualizace nebo odstranění balíčků rozevíracího seznamu Azure portálu"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nové zásuvné balíček aplikace Azure portálu"
[9]: ./media/batch-application-packages/app_pkg_09.png "Žádné propojené účet upozornění úložiště"
[10]: ./media/batch-application-packages/app_pkg_10.png "Zvolte zásuvné účtu úložiště Azure portálu"
[11]: ./media/batch-application-packages/app_pkg_11.png "Aktualizace balíčku zásuvné Azure portálu"
[12]: ./media/batch-application-packages/app_pkg_12.png "Potvrzení balíčku Azure portálu"
