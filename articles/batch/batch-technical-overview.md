<properties
    pageTitle="Základní informace o služby Azure dávku | Microsoft Azure"
    description="Další informace o použití služby Azure dávka pro rozsáhlé paralelní a HPC úloh"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Základní informace o Azure listu

Azure dávku umožňuje efektivní spouštění aplikací ve velkém měřítku paralelní a výkonných výpočetních (HPC) v cloudu. Je služba platformy, která plánuje náročné práci dělat spravované sady virtuálních počítačích a můžete automaticky měřítko výpočet zdroje podle potřeb své úlohy.

Ke službě dávku definujete Azure výpočetním materiály ke spuštění aplikace paralelně a ve velkém měřítku. Můžete spustit na vyžádání nebo naplánovaných úloh a není potřeba ručně vytvářet, konfigurovat a spravovat HPC clusteru, jednotlivé virtuálních počítačích, virtuální sítě nebo složité projektu a úkolu plánování infrastruktury.

## <a name="use-cases-for-batch"></a>Případy použití pro list

Dávkové je spravovaných Azure služba, která se používá pro *dávkové zpracování* nebo *dávku výpočetních*– spuštění velké množství podobné úkoly k získání požadovaného výsledku. Výpočet dávku nejčastěji používanou organizacím, které pravidelně procesu, transformace a analýza velkých objemů dat.

Dávkové pracuje s aplikací vnitřně paralelní (označovaná taky jako "embarrassingly paralelně") a úloh. Vnitřně paralelní úloh se snadno dělí na více úkolů, které provedení práce současně ve více počítačích.

![Paralelní úkoly][1]<br/>

Pár příkladů úloh, které zpracovávají běžně pomocí tento postup je:

* Modelování finančních rizik
* Prostředí a otázek hydrologie analýza dat
* Vykreslování obrázků, analýza a zpracování
* Kódování média a převod kódování
* Analýza genetické sekvence
* Inženýrské funkce namáhání analýzy
* Testování software

Dávku můžete provádět výpočty paralelní krokem zmenšení na konci a provést složitější úloh HPC třeba aplikací [Rozhraní předávání zpráv (MPI)](batch-mpi.md) .

Srovnání dávku a další možnosti řešení HPC v Azure najdete v článku [řešení dávku a HPC](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Vývoj s listem

Zpracování paralelní úloh s listem obvykle probíhá programově jedním z rozhraní [API dávku](#batch-development-apis). Pomocí rozhraní API dávku vytváření a Správa skupin výpočetním uzlů (virtuálních počítačích) a naplánovat úlohy a úkolech dělat tyto uzly. Klientské aplikaci nebo službu, který vytváříte používá pro komunikaci s službu dávku rozhraní API dávku.

Můžete efektivně zpracovat velké úloh pro vaši organizaci nebo poskytování služby front-end zákazníkům tak, aby mohli spouštět projektů a úkolů – na vyžádání nebo při plánování – na jednom, stovky nebo dokonce tisíce uzlů. Můžete taky dávku jako součást větší pracovního postupu, spravuje nástrojů, jako jsou [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Až budete chtít dostanete dávku rozhraní API pro podrobnější vysvětlení funkcí, které poskytuje, podívejte se na [Přehled funkcí dávka pro vývojáře](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure účty, které budete potřebovat

Když vyvíjet řešení v dávku použijete tyto účty v Microsoft Azure.

- **Účet azure a předplatné** – Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele prospěch][msdn_benefits], nebo [bezplatný účet Azure][free_account]. Při vytváření účtu se vytvoří výchozího předplatného.

- **Dávkové účtu** – Pokud aplikace v práci se službou dávku názvu účtu adresu URL účet a přístupové klávesy slouží jako přihlašovací údaje. Všechny vaše dávku zdroje, jako jsou skupiny, výpočet uzly, projekty a úkoly souvisí s účtem dávku. Můžete [Vytvořit dávku účet](batch-account-create-portal.md) Azure portálu.

- **Úložiště účet** – dávku podporuje předdefinované práce se soubory v [Úložišti Azure][azure_storage]. Scénář téměř každý dávku používá Azure úložiště – pro přípravu programy spuštěné úkolů a data, která zpracování a úložištěm výstupní data, která vytvářejí. Vytvoření účtu úložiště, najdete v článku [účty adresářové služby Azure o úložiště](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Vývojové dávku rozhraní API

Aplikace a služby problémů přímé volání rozhraní REST API, použijte jednu nebo více následujících knihoven klienta nebo kombinaci obojího ke správě výpočetního zdrojů a spuštění paralelní pracovního vytížení ve velkém měřítku pomocí služby dávku.

| ROZHRANÍ API    | Rozhraní API odkaz | Ke stažení | Ukázky |
| ----------------- | ------------- | -------- | ------------ |
| **Dávkové REST** | [MSDN][batch_rest] | NENÍ K DISPOZICI | [MSDN][batch_rest] |
| **Dávkové .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Dávkové Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Dávkové Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Dávkové Java** (verze preview) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Správa zdrojů dávku

Kromě klientského rozhraní API můžete také následující ke správě zdrojů v rámci vašeho účtu dávku.

- [Dávkové rutiny prostředí PowerShell][batch_ps]: dávku Azure rutin v modulu [Azure PowerShell](../powershell-install-configure.md) umožňují přidávání a používání zdrojů dávka s prostředím PowerShell.

- [Rozhraní příkazového řádku Azure](../xplat-cli-install.md): Azure příkazového řádku rozhraní (Azure rozhraní příkazového řádku) je různé platformy nástrojů, které obsahuje příkazy prostředí pro komunikaci s mnoha Azure služby, třeba dávku.

- [.NET dávku správy](batch-management-dotnet.md) klientů library: také k dispozici prostřednictvím [NuGet][api_net_mgmt_nuget], můžete použít knihovnu klienta dávku správy .NET programově spravovat dávku účty kvót a balíčků aplikací. Odkaz pro knihovny správy je na [webu MSDN][api_net_mgmt].

### <a name="batch-tools"></a>Dávkové nástroje

Nejsou nutné k vytvoření řešení používající dávku, tady jsou některé důležité nástroje pro použití při vytváření a ladění dávku aplikací a služeb.

 - [Azure portál][portal]: můžete vytvořit, sledovat a odstranit dávku fondů úlohy a úkoly v portálu Azure dávku listy. Můžete zobrazit informace o stavu pro tyto a další zdroje informací při spuštění svého úloh a dokonce stahování souborů ve výpočetním uzlů v vaší fondů (Stáhnout selhalo úkolu `stderr.txt` při odstraňování potíží, například). Můžete taky stahovat soubory vzdálené plochy, které můžete použít k přihlášení k výpočtu uzlů.

 - [Azure Explorer dávku][batch_explorer]: dávka Explorer funkce je podobná dávku zdrojů správy jako portálu Azure, ale samostatně klientské aplikace Windows Presentation Foundation (WPF). Jednu dávku .NET ukázkové aplikace dostupná na [GitHub][github_samples], můžete ho taky vytvořit pomocí aplikace Visual Studio 2015 nebo výše a použijte ji k procházení a Správa zdrojů ve vašem účtu dávku během vyvíjet a ladění dávku řešení. Zobrazení projektu, fondu a podrobnosti úkolu stahovat soubory z výpočetním uzlů a připojte k uzly vzdáleně pomocí vzdálené plochy soubory, které si můžete stáhnout v Průzkumníkovi dávku.

 - [Microsoft Azure úložiště Explorer][storage_explorer]: při není sporná, i když nástroj Azure dávku, Explorer úložiště je jiné cenný nástroj mít při vývoji a ladění dávku řešení.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scénář: Měřítko, paralelní úlohu

Běžné řešení, které využívá rozhraní API dávku Pokud chcete provést interakci se službou dávku zahrnuje rozšiřování vnitřně paralelní prací – třeba vykreslování obrázky 3D scény – fondu výpočetním uzlů. Tento fond výpočetním uzlů může být farmě"vykreslení", která poskytuje desítky, stovky nebo tisíce jádra k vykreslování práce, například.

Následující diagram ukazuje běžné dávku pracovního postupu, s klientské aplikaci nebo hostovanou službu dávku spuštění paralelní úlohu.

![Pracovní postup řešení dávku][2]

V tomto scénáři běžné aplikace nebo služby zpracuje výpočetní pracovní zátěž v Azure listu provedením následujících kroků:

1. Nahrajte **vstupní soubory** a **aplikace** , která bude tyto soubory k vašemu účtu úložiště Azure zpracovávat. Zadávání soubory můžete mít všechna data, která bude zpracovávat vaše aplikace, například finanční modelování dat, soubory nebo videosoubory být převedou. Soubory aplikace může být libovolné aplikaci, která se používá pro zpracování dat, jako je aplikace 3D vykreslování nebo transcoder média.

2. Vytvoření dávky **fond** uzlů výpočetním ve vašem účtu dávku – jsou tyto uzly virtuálních počítačích, které se provedou úkolů. Zadáte vlastnosti, například [velikosti uzlu](./../cloud-services/cloud-services-sizes-specs.md), jejich operačního systému a umístění v úložišti Azure aplikace při instalaci po uzly připojení fondu (aplikaci, kterou jste odeslali v kroku #1). Můžete taky nakonfigurovat fondu [automaticky měřítko](batch-automatic-scaling.md)– dynamicky upravit počtu uzlů výpočetním ve fondu – odpověď pracovní zátěž, která vytvářet úkolů.

3. Vytvoření dávku **úlohy** dělat pracovní zátěž fondu výpočetním uzlů. Při vytváření úlohy přidružit k fond dávku.

4. Přidání **úkolů** do projektu. Po přidání úkolů do projektu službu dávku automaticky plánuje úkoly při spuštění v jednotlivých uzlech výpočetním ve fondu. Každému úkolu použije aplikace, která jste nahráli zpracuje vstupních souborů.

    - 4a. Před provede úkol, můžete si stáhnout data (vstupních souborů) se postupuje při výpočetní uzly, kterému je přiřazena. Pokud aplikace již není nainstalován na uzel (viz krok #2), je můžete stáhnout tady místo. Po dokončení soubory ke stažení na jejich přiřazené uzly provést úkoly.

5. Jak spustit úkoly, můžete dotaz dávku ke sledování průběhu práce a svých úkolů. Klientské aplikaci nebo službu informuje uživatele o ke službě dávku přes protokol HTTPS a vzhledem k tomu může sledování tisíce spuštěna tisíce výpočetním uzlů úkolů, ujistěte se, k vytvoření [dotazu službu dávku efektivně](batch-efficient-list-queries.md).

6. Jako dokončení úkolů se k základnímu úložišti Azure nahrát jejich Výsledná data. Soubory můžete také zadat přímo z výpočetním uzlů.

7. Pokud vaše sledování zjistí, že jste dokončili úkoly v práci, klientské aplikaci nebo službu můžete si stáhnout výstupní data pro další zpracování nebo hodnocení.

Mít na paměti Toto je jenom jeden ze způsobů, jak používat list a tento scénář popisuje jenom některých dostupných funkcí. Například provést [více úkolů současně](batch-parallel-node-tasks.md) v jednotlivých uzlech výpočetním a umožňuje [přípravu a dokončení úloh](batch-job-prep-release.md) připravit uzly vaše úlohy a pak vyčistit později.

## <a name="next-steps"></a>Další kroky

Teď, když máte všeobecný přehled službu dávku, je čas na Prozkoumat důkladněji se dozvíte, jak můžete jej zpracuje vaše náročné paralelní úloh.

- Přečtěte si [Přehled funkcí dávka pro vývojáře](batch-api-basics.md), základní informace pro kohokoli Příprava použít list. Tento článek obsahuje podrobnější informace o zdrojích služby dávku například fondů uzlech, projekty a úkoly a řadu funkcí rozhraní API, které lze použít při vytváření aplikace dávku.

- [Začínáme s knihovnou Azure dávku .NET](batch-dotnet-get-started.md) se dozvíte, jak používat C# a knihovně dávku .NET provádět jednoduché úlohu pomocí pracovního postupu běžné dávku. Tento článek by měl být jeden z první přestane během naučit používat službu dávku. Je také [verzi Python](batch-python-tutorial.md) kurzu.

- Stažení [ukázky na GitHub] [ github_samples] zobrazíte, jak C# i Python můžete rozhraní s listem do plánu a proces úloh vzorku.

- Podívejte se na [Dávku Naučná stezka] [ learning_path] určitou představu dostupných zdrojů pro vás jak se naučíte pracovat s listem.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
