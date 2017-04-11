<properties
    pageTitle="Přehled funkcí Azure dávka pro vývojáře | Microsoft Azure"
    description="Přečtěte si tyto funkce službu dávku a jeho rozhraní API z hlediska vývoj."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Přehled funkcí dávka pro vývojáře

V tomto přehledu základní součásti webové části služby Azure dávku probereme tato funkce primární služby a zdroje informací, které dávku umožňují vytvářet rozsáhlé paralelní výpočet řešení.

Zda sestavování distribuované výpočetní aplikace nebo služby, které problémy přímé [Rozhraní REST API] [ batch_rest_api] hovory ani používáte jedna [Dávka SDK](batch-technical-overview.md#batch-development-apis), které budete používat mnoho prostředků a funkcí popisované v tomto článku.

> [AZURE.TIP] Vyšší úrovně Úvod ke službě dávku najdete v článku [Základy Azure dávku](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Pracovní postup služby dávku

Následující zkratce je typické skoro všech aplikací a služeb, které používají dávku služby pro zpracování paralelní úloh:

1. **Datové soubory** , které chcete zpracovat k [Úložišti Azure] odešlete[ azure_storage] účtu. Dávkové obsahuje integrovanou podporu pro přístup k úložišti objektů Blob Azure a úkolů tyto soubory můžete stáhnout pro [Výpočet uzlech](#compute-node) při spuštění úkoly.

2. Nahrajte **soubory aplikace** , který se spustí úkolů. Tyto soubory může být binární soubory nebo skripty a jejich závislosti a zpracují úkoly ve své úlohy. Úkolů můžete stáhnout tyto soubory ze svého účtu úložiště nebo můžete použít funkci [balíčků aplikací](#application-packages) listu Správa aplikací a nasazení.

3. Vytvoření [fondu](#pool) výpočetním uzlů. Při vytvoření fondu určíte počtu uzlů výpočetním fondu, jejich velikost a operačního systému. Při spuštění každý úkol v práci, má přiřazenou na jeden z uzlů v vašeho fondu provést.

4. Vytvoření [projektu](#job). Úlohy spravuje kolekci úkolech. Přidružení jednotlivé úlohy fond konkrétních místo, kam se spustí této úlohy.

5. Přidání [úkolů](#task) do projektu. Každý úkol se spustí aplikace nebo skript, které jste nahráli zpracuje datové soubory, které ke stažení z vašeho účtu úložiště. Jak dokončení všech úkolů nahrávat jeho výstup k základnímu úložišti Azure.

6. Sledovat pokrok projektu a načíst výstupu úkolu z Azure úložiště.

Následující části se zabývají tyto a další prostředky dávku podporující nástroj distribuované výpočetní nefunguje.

> [AZURE.NOTE] Potřebujete [dávku účtu](batch-account-create-portal.md) používat službu dávku. Taky téměř všechna řešení pomocí [Úložišti Azure] [ azure_storage] účtu pro ukládání souborů a načítání. Dávkové aktuálně podporuje pouze **univerzální** úložiště typ účtu, podle pokynů v kroku 5 [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).

## <a name="batch-service-resources"></a>Dávkové služby zdrojů

Některé z následujících materiálů – účty výpočet, že uzly, fondů, projekty a úkoly – požaduje všechny řešení, které používají službu dávku. Ostatní, například plány projektu a balíčků aplikací jsou užitečné, ale nepovinný funkce.

- [Účet](#account)
- [Výpočet uzel](#compute-node)
- [Fond](#pool)
- [Úlohy](#job)

  - [Plány projektu](#scheduled-jobs)

- [Úkol](#task)

  - [Zahájení úkolu](#start-task)
  - [Správce úlohu](#job-manager-task)
  - [Příprava a vydání úlohy](#job-preparation-and-release-tasks)
  - [Úkol s více instancemi (MPI)](#multi-instance-tasks)
  - [Závislosti mezi úkoly](#task-dependencies)

- [Balíčků aplikací](#application-packages)

## <a name="account"></a>Účet

Účet dávku je jednoznačně entita v rámci služby dávku. Zpracování souvisí s účtem dávku. Při provádění operací ke službě dávku, musíte název účtu a jeden z jeho klíčů účtu. Můžete [vytvořit účet Azure dávku pomocí portálu Azure](batch-account-create-portal.md).

## <a name="compute-node"></a>Výpočet uzel

Výpočetní uzly je Azure virtuálního počítače (OM), který se snaží o zpracování část pracovní zátěž vaše aplikace. Velikost uzel zjistí počet procesoru jádra, kapacita paměti a místní systém velikost souboru přidělený uzel. Vytvářet skupiny Windows nebo Linux uzlů pomocí služby Azure Cloud Services nebo Marketplace virtuálních počítačích obrázky. Najdete v následující části [fondu](#pool) Další informace o těchto možnostech.

Spustitelný soubor nebo skript, který je podporovaný operační systém prostředí uzel by umožnit spuštění uzlů. Jedná se o \*.exe, \*cmd, \*bat a skriptů Powershellu pro Windows – a binárních dat, kostra domu a Python skriptů Linux.

Všechny výpočet taky zahrnovat uzlů v listu:

- Standardní [Struktura složek](#files-and-directories) a související [proměnné prostředí](#environment-settings-for-tasks) , které jsou k dispozici pro referenci úkolů.
- Nastavení **brány firewall** nakonfigurované k řízení přístupu.
- [Vzdálený přístup](#connecting-to-compute-nodes) k systému Windows (vzdálené plochy RDP (Protocol)) a uzly Linux (Secure prostředí (SSH)).

## <a name="pool"></a>Fond

Fond je sada uzlů, které spuštění aplikace na. Fondu lze vytvořit ručně vy nebo automaticky službou dávku při zadání práce zbývá dokončit. Můžete vytvořit a spravovat skupinu, která splňuje požadavky na zdroj aplikace. Fond lze použít pouze pomocí účtu dávku, ve kterém byl vytvořen. Účet dávky může obsahovat více než jeden fond.

Azure fondů dávku vytvářet nad platformu Azure výpočetním core. Poskytují rozsáhlé přidělení instalace aplikací, distribuce dat, sledování stavu a přizpůsobování počtu uzlů výpočetním v rámci fondu ([měřítka](#scaling-compute-resources)).

Každý uzel, která Přibyla do fondu se přiřadí jedinečný název a IP adresu. Při odebrání uzel z fondu budou ztraceny všechny změny, které jsou určené pro operační systém nebo soubory a jeho název a IP adresu vydané pro budoucí použití. Když uzel opustí fond, životnosti překročení.

Když vytvoříte fond, můžete zadat následujícími atributy:

- Výpočet uzel **operačního systému** a **verze**

    Máte dvě možnosti, když vyberete operační systém uzlů v vašeho fondu: **virtuální počítač** a **Konfigurace služby cloudu**.

    **Konfigurace virtuálního počítače** poskytuje Linux a Windows obrázky pro výpočet uzly z [Azure Marketplace virtuálních počítačích][vm_marketplace].
    Když vytvoříte skupinu, která obsahuje uzly konfiguraci virtuálního počítače, je nutné zadat nejen velikost uzly, ale **odkaz obrázek virtuálního počítače** a dávku **uzel agent SKU** v jednotlivých uzlech je třeba nainstalovat. Další informace o zadávání těchto vlastností fondu najdete v článku [poskytování Linux výpočet uzlů v Azure dávku fondů](batch-linux-nodes.md).

    **Konfigurace služby cloudu** poskytuje že Windows výpočet uzly *pouze*. K dispozici operační systémy pro fondy konfigurace služby cloudu jsou uvedeny v [Azure OS Host verzích a SDK kompatibilitou matici](../cloud-services/cloud-services-guestos-update-matrix.md). Když vytvoříte skupinu, která obsahuje uzly cloudové služby, budete muset zadat jenom velikost uzel a jeho *Operačních systémů*. Při vytváření skupiny Windows výpočet uzly, nejčastěji použít Cloudovým službám.

    - *Operačních systémů* také určuje, které verze .NET nainstalovaných s operačním systémem.
    - Jak pracovníka rolí v rámci služby cloudu, můžete určit *Operační systém verze* (Další informace o rolích kolegy, najdete v článku [Upozorňovat o cloudovým službám](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) v části [Cloudovým službám přehled](../cloud-services/cloud-services-choose-me.md)).
    - Jak rolemi kolegy, doporučujeme, abyste, že zadáte `*` pro *Operační systém verze* tak, aby automaticky upgradu uzly a neexistuje žádné řešení potřebná k řešení do nově vydána verze. Primární použití případ pro výběr určitý operační systém verze je pro zajištění kompatibility aplikace, které umožňuje zpětná kompatibilita testování provést před umožněním verze mají být aktualizovány. Po ověření můžete aktualizovat *Operační systém verze* fondu a může být nový obrázek s operačním systémem nainstalovanou – úlohy pracovního přerušilo se zařazena.

- **Velikost uzlů**

    **Konfigurace služby cloudu** výpočetním uzel velikosti jsou uvedeny v [velikosti Cloudovým službám](../cloud-services/cloud-services-sizes-specs.md). Dávkové podporuje všech velikostí Cloudovým službám kromě `ExtraSmall`.

    **Konfigurace virtuálního počítače** výpočet uzel velikosti jsou uvedeny v [velikosti virtuálních počítačích v Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) a [velikost virtuálních počítačích v Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Dávkové podporuje všech velikostí Azure OM kromě `STANDARD_A0` a premium úložiště (`STANDARD_GS`, `STANDARD_DS`, a `STANDARD_DSV2` řady).

    Při výběru velikosti uzlu výpočetním, zvažte možnost Vlastnosti a požadavky aplikace, které bude spouštět v jednotlivých uzlech. Aspekty jako tom, jestli je aplikace podprocesy a velikosti paměti zabere může pomoct zjistit velikost uzel vhodné a efektivní. Je typické, vyberte uzel velikost za předpokladu, že jeden úkol se spustí na uzel najednou. Je však možné mít více úkolů (a tedy víc instancí aplikace služby) [Spusťte paralelně](batch-parallel-node-tasks.md) výpočet uzly během provádění úloh. V tomto případě je běžné zvolte větší velikost uzel tak, aby zahrnoval lepší služba spuštění paralelní úkolu. Další informace najdete v tématu [zásady plánování úkolů](#task-scheduling-policy) .

    Jsou všechny uzlů v fond stejnou velikost. Pokud chcete-li spouštění aplikací s požadavky na systém pro různé a/nebo načtení úrovně, doporučujeme použít samostatné fondů.

- **Cíl počtu uzlů**

    Toto je počet výpočetním uzlů, které chcete nasadit ve fondu. To je uvedená jako *cílové* protože v některých případech může se stát vašeho fondu dosáhla požadovaného počtu uzlů. Fond nemusí být doručena požadovaného počtu uzlů Pokud nedosáhnete [základní kvóty](batch-quota-limit.md#batch-account-quotas) pro váš účet dávku – nebo pokud je automatické měřítko vzorec, který jste použili fondu, která omezuje maximální počet uzlů (viz následující oddíl "Měřítka zásad").

- **Změna měřítka zásad**

    Kromě určující statické počet uzlech, můžete místo psaní a použít [Automatické měřítko vzorec](#scaling-compute-resources) do fondu. Služba dávku pravidelně vyhodnotí vzorec a upraví počtu uzlů v rámci fondu podle různých fondu projektu a úkolu parametrů, které můžete určit.

- **Plánování zásad úkolů**

    Možnost konfigurace [Maximální počet uzlů úkolů](batch-parallel-node-tasks.md) určuje maximální počet úkolů, které lze spustit paralelně v jednotlivých uzlech výpočetním fondu.

    Výchozí konfigurace je, že jeden úkol po druhém běží na uzel, ale existují scénáře, kde je vhodné mít víc než jeden úkol současně na uzel. Viz [Příklad scénář](batch-parallel-node-tasks.md#example-scenario) v článku [souběžné uzel úkoly](batch-parallel-node-tasks.md) chcete zobrazit, jak můžete využít více úkolů na uzel.

    Můžete taky určit *Typ výplně* , která určuje, zda dávku rovnoměrně dvojstránky úkolů na všech uzlech do fondu nebo sady každý uzel s maximální počet úkolů před přiřazení úkolů do jiného uzlu.

- **Stav komunikace** výpočetním uzlů

    Ve většině případů úkoly pracovat nezávisle na sobě a nemusí vzájemně komunikovat. Jsou tu ale i některé aplikace, ve kterých musí úkoly komunikovat, jako je [MPI scénáře](batch-mpi.md).

    Můžete nakonfigurovat fond povolit komunikaci mezi uzly v it –**internodium komunikace**. Při zapnuté funkci internodium komunikace, uzlech Konfigurace služby cloudu fondů komunikovat s překrývajícími porty vyšší než 1100 a konfiguraci virtuálního počítače fondů Neomezovat přenosy na jakýkoli.

    Poznámka: povolení komunikace internodium také ovlivňuje umístění uzlů v rámci clusterů může omezit maximálního počtu uzlů v fond z důvodu omezení nasazení. Pokud vaše aplikace není nutné zadávat komunikaci mezi uzly, službu dávku můžete přidělit potenciálně velkého počtu uzlů fondu z mnoha různých clusterů a datacentrech povolit lepší výkon souběžné zpracování.

- **Zahájení úkolu** výpočetních uzlů

    Volitelné *zahájení úkolu* spustí v jednotlivých uzlech a uzel spojí fondu pokaždé, když nerestartuje nebo reimaged uzel. Zahájení úkolu je užitečné především pro přípravu výpočetním uzly pro provádění úkolů, jako je instalace aplikace spuštěné v operačním systému výpočetním uzlů úkolů.

- **Balíčků aplikací**

    Můžete zadat [balíčků aplikací](#application-packages) pro nasazení na výpočetních uzly ve fondu. Balíčků aplikací zadejte zjednodušené nasazení a správu verzí aplikace, které běží úkolů. Balíčků aplikací, které zadáte fondu nainstalovaných v každé uzel, který propojuje tohoto fondu a restartování nebo reimaged pokaždé, když na uzel. Balíčků aplikací jsou aktuálně nepodporuje uzlech výpočetním Linux.

- **Konfigurace sítě**

    Můžete zadat ID Azure [virtuální sítě (VNet)](../virtual-network/virtual-networks-overview.md) , ve kterém fondu výpočet uzly by měl být vytvořen. Požadavky pro zadávání VNet pro váš fond najdete v [Přidat fond k účtu] [ vnet] dávku REST API odkazu.

> [AZURE.IMPORTANT] Všechny účty dávku mají výchozí **kvóty** , která omezuje počet **jádra** (a tedy výpočetním uzly) dávku účtu. Výchozí kvóty a pokyny ke [zvýšení kvótu](batch-quota-limit.md#increase-a-quota) (například maximální počet jádra ve vašem účtu dávku) můžete najít v [kvót a limity pro službu Azure dávku](batch-quota-limit.md). Pokud zjistíte, s dotazem, "Proč se můj fondu dosáhla více než X uzly?" Příčinou může být tento základní kvóty.

## <a name="job"></a>Úlohy

Úlohy je sada úkoly. Spravuje způsob výpočtu prováděnou svých úkolů v jednotlivých uzlech výpočetním do fondu.

- Úlohy Určuje **fondu** , ve kterém má být spustit práce. Můžete vytvořit nový fond pro jednotlivé úlohy nebo použít jeden fond pro mnoho úloh. Můžete vytvořit skupinu pro jednotlivé úlohy, která je přidružená k plánu projektu, nebo všechny úloh, které jsou přidružené k plánu projektu.

- Můžete zadat volitelné **Priorita úloh**. Po odeslání úlohu s prioritou vyšší než úloh, které jsou v současnosti v průběhu úkolů pro danou úlohu vyšší prioritu vloženy do fronty před úkolů pro projekty nižší prioritou. Úkoly v nižší prioritou úloh, které už používáte nejsou přepnuto.

- Zadání určitých mezí úlohami můžete úlohy **omezení** :

    **Maximální wallclock času**, můžete nastavit tak, že pokud dlouhodobě spuštěná úloha delší než maximální wallclock čas, který není zadán, úkoly a všech jeho úkolů ukončí.

    Dávkové můžete zjišťovat a opakujte selhalo úkoly. **Maximální počet opakování úkolu** můžete zadat jako omezení, včetně toho, jestli je daný úkol *vždy* nebo *nikdy* opakované. Opakování úkolu znamená, že je znovu spustit zařazena úkol.

- Klientská aplikace můžete přidání úkolů do projektu nebo můžete zadat [úlohu správce](#job-manager-task). Správce úlohu s informacemi, je třeba vytvořit požadované úkoly v projektu s správce úlohu spuštěn na jeden z uzlů výpočetním ve fondu. Zpracování správce úlohu konkrétně tak, že dávka – to je ve frontě hned úlohy se vytvoří a restartování Pokud dojde k chybě. Správce úlohu je *potřeba* úloh, které jsou vytvářeny ve [plánu projektu](#scheduled-jobs) , protože je jediný způsob, jak definovat úkoly před je vytvořena projektu.

- Ve výchozím nastavení úlohy ponechání kurzoru aktivního stavu po dokončení všech úkolů v projektu. Toto chování můžete změnit tak, aby úkoly automaticky ukončen po dokončení všech úkolů v projektu. Nastavte vlastnost **onAllTasksComplete** projektu ([OnAllTasksComplete] [ net_onalltaskscomplete] v dávku .NET) do *terminatejob* automaticky ukončit úlohu při všech svých úkolů se ve vyplněném stavu.

    Všimněte si, že služba dávku považuje úlohu s *žádné* úkoly mít všechny úkoly dokončeny. Proto tato možnost slouží nejčastěji s [úlohu správce](#job-manager-task). Pokud chcete použít automatické úlohy ukončení bez vedoucím projektu, můžete by měl původně nastavte vlastnost **onAllTasksComplete** nového projektu na *noaction*a pak nastavte *terminatejob* až po dokončení přidávání úkolů do projektu.

### <a name="job-priority"></a>Priorita úloh

Priorita můžete přiřadit úloh, které vytvoříte v listu. Službu dávku hodnotu priority (priorita) úlohy používá k určení pořadí plánování úlohy v rámci účtu (Toto je nechcete být zaměňovány s [úlohy](#scheduled-jobs)). Priority (priorita) hodnoty rozsahu od -1000 do 1 000 s-1000 znamená nejnižší prioritu a 1000 nejvyšší. Můžete aktualizovat priority úlohy pomocí [Aktualizovat vlastnosti projektu] [ rest_update_job] operace (dávku ZBYTEK) nebo úpravou [CloudJob.Priority] [ net_cloudjob_priority] vlastnost (dávku .NET).

V rámci stejný účet mít vyšší prioritu úlohy plánování přednost před úlohami nižší prioritou. Práce s hodnotou vyšší priority do jednoho účtu nemá plánování priorit nad jiné úlohu s hodnotou nižší prioritou do jiného účtu.

Plánování přes fondů práce je nezávislý. Mezi různých fondů není zaručit, že úlohy vyšší prioritu naplánován nejdřív-li jeho přidružený fondu kratší než nedělá uzlech. Ve stejném fondu úlohy se stejnou úrovní priority mají stejné pravděpodobnost plánován.

### <a name="scheduled-jobs"></a>Naplánovaných úloh

[Funkce plány] [ rest_job_schedules] umožňují vytvářet opakované úlohy v rámci služby dávku. Plánování úlohy Určuje, kdy je vhodné spustit úlohy a obsahuje specifikace pro úlohy proběhnout. Můžete zadat dobu trvání plánu – jak dlouho a po na plán v podstatě – a jak často tohoto časového období vytvořenou úlohy.

## <a name="task"></a>Úkol

Úkol je jednotka výpočtu, který souvisí s úlohy. Běží na uzel. Úkoly přiřazené uzel pro spuštění nebo jsou ve frontě dokud uvolní uzel. Jednoduše umístění úkolu se spustí jeden nebo víc programů nebo skripty na uzel výpočetním k provedení práce, které potřebujete.

Po vytvoření úkolu je možné zadat:

- **Přepínač příkazového řádku** úkolu. Toto je příkazového řádku, který spustí aplikace nebo skriptu na uzel výpočetních.

    Je důležité mít na paměti, že příkazového řádku se nespustí skutečně v části představuje prostředí. Proto ho nemůžete využít nativně funkce shell jako [proměnnou](#environment-settings-for-tasks) rozšiřovací (platí to i `PATH`). Umožní využít výhod tato funkce je nutné vyvolat shell na příkazovém řádku – například spuštěním `cmd.exe` uzlech Windows nebo `/bin/sh` na Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    V případě úkolů se spuštění aplikace nebo skript, který není uveden v na uzel `PATH` nebo referenční proměnné, vyvolat prostředí explicitně do příkazového řádku úkolu.

- **Zdrojové soubory** , které obsahují data, která chcete zpracovat. Tyto soubory se automaticky zkopírují na uzel z úložiště objektů Blob účet Azure úložiště **univerzální** před provedením úkolu příkazového řádku. Další informace naleznete v částech [zahájení úkolu](#start-task) a [soubory a adresáře](#files-and-directories).

- **Proměnné** , který požaduje aplikace. Další informace naleznete v části [nastavení prostředí pro úkoly](#environment-settings-for-tasks) .

- **Omezení** , pod kterým by měl provést úkol. Pro příklad, maximální dobu umožňující úkol spustíte maximální počet opakovaných neúspěšných úkolu má opakovat a maximální doba které soubory v adresáři práce úkolu se zachovají.

- **Balíčků aplikací** nasadit výpočetní uzly, na které je naplánováno spustit. [Balíčků aplikací](#application-packages) zadejte zjednodušené nasazení a správu verzí aplikace, které běží úkolů. Balíčky aplikace na úrovni úkolu jsou užitečné zejména v sdílí fond prostředí, kde různé úlohy se spouštějí na jeden fond a fondu neodstraníte dokončení projektu. Pokud práce má míň úkoly než uzly ve fondu, balíčků aplikací úkolu Minimalizovat přenos dat od používaný aplikace pouze uzly spuštěné úkoly.

Kromě úkolů, které definujete provést výpočet na uzel, jinak úkoly podle těchto pokynů jsou také k dispozici službou dávku:

- [Zahájení úkolu](#start-task)
- [Správce úlohu](#job-manager-task)
- [Příprava a vydání úlohy](#job-preparation-and-release-tasks)
- [Více instancí úkoly (MPI)](#multi-instance-tasks)
- [Závislosti mezi úkoly](#task-dependencies)

### <a name="start-task"></a>Zahájení úkolu

Fond přiřazení **zahájení úkolu** , můžete připravit prostředí jeho uzlů. Můžete třeba provádět akce jako instalace aplikace spuštěné úkolů a od procesy na pozadí. Zahájení úkolu spouští při každém spuštění uzel pro zůstává v fondu – včetně při prvním uzel přidané do fondu a kdy se nerestartuje nebo reimaged.

Primární výhodou zahájení úkolu je, že může obsahovat všechny informace, které je třeba konfigurovat výpočetní uzly a instalace aplikace, které jsou potřeba pro provádění úkolů. Zvýšení počtu uzlů v fond tedy co nejjednodušší určující nový cíl počet uzel – dávku již obsahuje informace, které je potřeba nakonfigurovat nové uzly a příprava je pro přijetí úkoly.

Jak se Azure dávku úkoly, můžete zadat seznam **zdrojů souborů** v [Úložišti Azure][azure_storage], kromě **příkazového řádku** má provádět. Dávkové nejdřív zkopíruje zdroj soubory na uzel z Azure úložiště a spustí příkazového řádku. Fond zahájení úkolu obvykle obsahuje seznam souborů úkolu aplikace a její závislosti.

Ji však také obsahovat referenčních dat pro použití v všechny úkoly, které jsou spuštěné na uzel výpočetním. Například může provádět zahájení úkolu příkazového řádku `robocopy` operace Kopírovat soubory aplikace, (které jste zadali jako souborů prostředků a stáhl(a) na uzel) zahájení úkolu, [pracovní adresář](#files-and-directories) do [sdílené složky](#files-and-directories), a pak spusťte MSI nebo `setup.exe`.

> [AZURE.IMPORTANT] Dávkové aktuálně podporuje *pouze* **univerzální** úložiště jako typ účtu podle pokynů v kroku 5 [vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Dávku úkolů (včetně standardní úkoly, zahájení úkolů, Příprava úlohy a úlohy vydaná verze) zadejte zdrojové soubory, které jsou uloženy *pouze* v **univerzální** úložiště účty.

Je obvykle vhodné pro službu dávku až zahájení úkolu až po dokončení zvažovat uzel chtít přidělovat úkoly, ale můžete nakonfigurovat.

Když start úkol nepovede uzlu výpočetním, pak stát uzel aktualizuje tak, aby odrážela selhání a uzel není přiřazené úkoly. Zahájení úkolu může selhat, pokud se vyskytne problém kopírování jeho soubory zdroje z úložiště nebo procesu spouštět pomocí příkazového řádku vrátí nenulová ukončovacího.

Je-li přidat nebo aktualizovat počáteční úloha pro *stávající* fond, je nutné restartovat jeho výpočetním uzlů pro zahájení úkolu mohou být použity pro uzlech.

### <a name="job-manager-task"></a>Správce úlohu

Můžete obvykle používejte **úlohu správce** na ovládací prvek a/nebo sledovat provádění úlohy – například k vytvoření a odeslání úkolů v projektu, zjistit další úkoly posílání a určit, kdy práce je dokončeno. Správce úlohu však není omezena na těchto činností. Je plně fledged úkol, který může provádět všechny akce, které jsou potřeba pro daný úkol. Například úlohu správce může stáhnout soubor, který není zadán jako parametr analyzovat obsah tohoto souboru a odeslání další úkoly podle těchto obsah.

Další úkoly zahájení úkolu Správce úloh. Poskytuje tyto funkce:

- Automaticky odešle se jako úkol službou dávku při vytvoření projektu.

- Je naplánováno provést před další úkoly v projektu.

- Přidružený uzel je poslední má být odebrán z fondu, když je právě downsized fondu.

- Konec platnosti můžete stejným k ukončení všechny úkoly v projektu.

- Správce úlohu je uveden nejvyšší prioritu, když je se musí restartovat. Pokud nedělá uzel není k dispozici, může ukončit službu dávku jeden z dalších pracovního úkolů ve fondu uvolníte místo pro správce úlohy na spustit.

- Správce úlohu do jednoho úlohy není mají přednost před úkoly ostatní úlohy. Mezi projekty jsou pozorovanými pouze priority na úrovni projektu.

### <a name="job-preparation-and-release-tasks"></a>Příprava a vydání úlohy

Dávku poskytuje úlohy Příprava před úlohy spuštění instalačního programu. Uvolnění úlohy jsou po úlohy správy nebo vyčištění.

- **Příprava úlohu**: Příprava úlohu běží ve všech výpočetním naplánované provádět úkoly, před provedením všech dalších úlohy jsou. Příprava úlohu Kopírovat data, která platí pro všechny úkoly, ale je třeba jedinečné pro úkoly, můžete použít.
- **Uvolnění úlohu**: Po dokončení projektu vydání úlohu běží v jednotlivých uzlech ve fondu spouštět aspoň jeden úkol. Pomocí verze úlohu odstranění dat, které se zkopírují Příprava úlohu nebo komprimovat a nahrajte diagnostických protokolů dat, například.

Obě úlohy přípravu a vydání úkoly umožňují určit příkazovém řádku spusťte při je daný úkol. Nabízejí funkcí jako stahování souboru, zvýšenými oprávněními spuštění, vlastní proměnné, doba trvání maximální spuštění, počet opakování a doba uchovávání informací soubor.

Další informace o přípravě a vydání úlohy najdete v článku [Spustit přípravu a dokončení úlohy v listu Azure výpočet uzlů](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Úkol s více instancemi

[Více instancí úkolu](batch-mpi.md) je úkol, který je nakonfigurovaný na spuštění na víc výpočetní uzly současně. S více instancí úkolů můžete povolit výkonné výpočetní scénáře, které vyžadují skupiny uzlů výpočetním přiřazené společně zpracuje jeden pracovní zátěž (jako je zpráva předávání rozhraní (MPI)).

Podrobné informace o spuštění úlohy MPI v listu pomocí knihovnu .NET dávku najdete v článku [použití více instancí úkoly spouštění aplikací rozhraní předávání zpráv (MPI) v Azure dávku](batch-mpi.md).

### <a name="task-dependencies"></a>Závislosti mezi úkoly

[Závislosti mezi úkoly](batch-task-dependencies.md), jak název naznačuje, umožňují určit, že daný úkol závisí na dokončení dalších úkolů před jeho spuštění. Tato funkce se podporuje pro situace, kdy "podřízené" úkolu spotřebovává výstupu "nadřazeného" úkolu – nebo při odesílání dat úkolu provádí některé inicializace, která je potřebná podřízený úkol. Tuto funkci lze použít, musíte nejdřív povolit závislostí na dávku práce. Pak pro každý úkol, který závisí na jiném (nebo dalších), určete úkoly, které daný úkol závisí na.

Se závislostmi úkolu můžete nakonfigurovat scénáře takto:

* *taskB* závisí na *taskA* (*taskB* nezačnou spuštění dokud nebude dokončena *taskA* ).
* *taskC* závisí na *taskA* a *taskB*.
* Před spuštěním *taskD* závisí na oblasti úkoly, například úkoly *1* až *10*.

Podívejte se na [závislostí úkolů v Azure listu](batch-task-dependencies.md) a [TaskDependencies] [ github_sample_taskdeps] ukázka kódu v [azure dávku vzorky] [ github_samples] GitHub úložiště pro další podrobné informace o této funkci.

## <a name="environment-settings-for-tasks"></a>Nastavení prostředí pro úkoly

Každý úkol spouštět službou dávku má přístup k proměnné, které nastaví na výpočetních uzlů. Jedná se o proměnné definované službou dávku ([Služba definovaná][msdn_env_vars]) a vlastní proměnné, které můžete definovat úkolů. Aplikace a skripty úkolů provést mít přístup k tyto proměnné během provádění.

Na úrovni úkolu nebo projektu můžete nastavit vlastní proměnné naplňovat vlastnost *nastavení prostředí* pro tyto entity. Například, přečtěte si článek [Přidání úkolu do projektu] [ rest_add_task] operace dávku REST API () nebo [CloudTask.EnvironmentSettings] [ net_cloudtask_env] a [CloudJob.CommonEnvironmentSettings] [ net_job_env] vlastností v dávku .NET.

Klientské aplikaci nebo službu získat úkolu proměnné, služba definovaná i vlastní, [Přečtěte si informace o úkolu] [ rest_get_task_info] operace (dávku ZBYTEK) nebo přístupem k [CloudTask.EnvironmentSettings] [ net_cloudtask_env] vlastnost (dávku .NET). Procesy spouštění na uzel výpočetních můžou získat přístup ke těchto i jiných proměnné na uzel, například známá `%VARIABLE_NAME%` (Windows) nebo `$VARIABLE_NAME` syntaxe (Linux).

Úplný seznam všech definovaných služby proměnné můžete najít v [Výpočet uzel proměnné][msdn_env_vars].

## <a name="files-and-directories"></a>Soubory a adresáře

Každý úkol má *pracovní adresář* v části který předtím vytvořil nula nebo víc souborů a adresářů. Pracovní adresář mohou sloužit k ukládání program, který se spustí tak, že úkol, data, která zpracuje a výstup, kterou provede zpracování. Všechny soubory a adresáře úkolu jsou vlastní uživatelem úkolu.

Služba dávku zpřístupňuje část systému souborů na uzel jako *kořenový adresář*. Úkoly se dostanete do kořenového adresáře odkazování `AZ_BATCH_NODE_ROOT_DIR` proměnné. Další informace o používání proměnné najdete v článku [nastavení prostředí pro úkoly](#environment-settings-for-tasks).

V kořenovém adresáři obsahuje následující strukturu adresářů:

![Výpočet strukturu adresářů uzel][1]

- **sdílené**: Tento adresář poskytuje přístup pro čtení i zápis do *všech* úkolů, spuštěné v operačním systému uzel. Jakýkoli úkol, který běží na uzel můžete vytvořit, číst, aktualizovat a odstraňovat soubory v této složce. Úkoly se dostanete tento adresář odkazování `AZ_BATCH_NODE_SHARED_DIR` proměnné.

- **spuštění**: Tento adresář používá zahájení úkolu jako pracovní adresář. Všechny soubory, které stáhnou na uzel zahájení úkolu jsou uložené v tomto poli. Zahájení úkolu můžete vytvořit, číst, aktualizovat a odstraňovat soubory ve skupinovém rámečku tento adresář. Úkoly se dostanete tento adresář odkazování `AZ_BATCH_NODE_STARTUP_DIR` proměnné.

- **Úkoly**: V adresáři se vytvoří pro každý úkol, který běží na uzel. Přístup pomocí odkazů `AZ_BATCH_TASK_DIR` proměnné.

    V rámci adresáře každý úkol službu dávku vytvoří pracovní adresář (`wd`) nastavil jehož jedinečné cesta `AZ_BATCH_TASK_WORKING_DIR` proměnné. Tento adresář poskytuje přístup pro čtení i zápis k úkolu. Úkol můžete vytvořit, číst, aktualizovat a odstraňovat soubory ve skupinovém rámečku tento adresář. Tento adresář se zachovají podle omezení *RetentionTime* zadané pro daný úkol.

    `stdout.txt`a `stderr.txt`: tyto soubory jsou došlo k zápisu složce úkolu během provádění úkolu.

>[AZURE.IMPORTANT] Při odebrání uzel z fondu budou odebrány *všechny* soubory, které jsou uložené na uzel.

## <a name="application-packages"></a>Balíčků aplikací

Funkce [balíčků aplikací](batch-application-packages.md) poskytuje snadno řízení a nasazení aplikace do výpočetního uzlů v vaší fondů. Můžete odeslat a spravovat více verzí aplikací spusťte úkolů, včetně jejich binární soubory a soubory technické podpory. Pak můžete automaticky nasadit jednu nebo víc z těchto aplikací na výpočetních uzly ve vaší fondu.

Můžete zadat balíčků aplikací na úrovni fondu a úkol. Při zadávání balíčků aplikací fondu aplikace nasazené každé uzel fondu. Při zadávání balíčků aplikací úkol, aplikace je používaný pouze uzlech, které je naplánováno spustit alespoň jedno úkolů projektu, stačí před spuštěním úkolu příkazového řádku.

Dávku zpracovává informace o práci s úložištěm Azure ukládat balíčků aplikací a jejich pro výpočet uzlech, můžete zjednodušit režijních kód a Správa nasazení.

Další informace o funkci balíček aplikace, najdete v tématech [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md).

>[AZURE.NOTE] Když naopak přidáte balíčků aplikací fondu *existující* fond, je nutné restartovat jeho výpočetním uzlů pro nasazení na uzly balíčků aplikací.

## <a name="pool-and-compute-node-lifetime"></a>Fond a výpočetní životnost uzel

Při navrhování řešení dávku Azure, musíte návrh rozhodnutí o tom, a při vytváření skupiny a jak dlouho výpočet, že uzlů v těchto fondů uchovávat k dispozici.

Na jednu stranu ale můžete vytvořit fond pro jednotlivé úlohy, která odešlete a odstranit fondu hned po dokončení provádění svých úkolů. Vzhledem k tomu uzly jsou pouze při nutné a vypněte hned, jak budou nedělá to zajišťuje maximální využití. Když to znamená, že projekt musí čekat uzly, který se má přidělit, je důležité mít na paměti, že naplánováno pro spuštění hned uzly jednotlivě, jsou k dispozici přidělit, a úkol start dokončen. Znamená dávku *není* počkejte všech uzlů v rámci fondu jsou k dispozici před přidělování úkolů uzlů. Díky maximální využití všech dostupných uzlů.

Na druhou stranu ale-li s projekty začít okamžitě nejvyšší prioritu, můžete vytvořit fond předem a zpřístupnit jeho uzlů před odesláním úlohy. V tomto scénáři úkolů můžete ihned začít, ale uzlech mohou sednout nedělá během čekání pro ně chcete přiřadit.

Kombinované přístup se obvykle používá pro zpracování proměnných, ale probíhající, načíst. Máte skupinu, která se odesílají službě více úloh, ale můžete změnit velikost počtu uzlů nahoru nebo dolů podle projektu načíst (viz [Měřítko výpočet zdrojů](#scaling-compute-resources) v následující části). Můžete to uděláte předimenzování, na základě aktuální zatížení, nebo včasným, pokud budou předpovídat načíst.

## <a name="scaling-compute-resources"></a>Změna měřítka pro využití prostředků

Pomocí [Automatické měřítko](batch-automatic-scaling.md)máte službu dávku dynamicky upravit počtu uzlů výpočetním do fondu podle aktuálního pracovního vytížení a zdroje použití výpočetních nefunguje. Umožňuje provádět snížit celkové náklady spuštění aplikace pomocí pouze prostředky, které bude nutné a uvolněním těch, které už nepotřebujete.

Povolení automatické škálování [automatické změny velikosti vzorec](batch-automatic-scaling.md#automatic-scaling-formulas) a přiřadí mu fond tento vzorec. Služba dávku je definována vztahem určit cílový počtu uzlů v fondu pro další změny měřítka interval (interval, který můžete nakonfigurovat). Automatické změny velikosti nastavení fondu můžete určit při vytvoření, nebo povolení měřítko na fond později. Můžete taky aktualizovat měřítka nastavení fond povoleno škálování.

Jako příklad možná projekt vyžaduje odeslat velmi velké množství úkolů, který bude proveden. Změny měřítka vzorce můžete přiřadit fondu upraví počtu uzlů v fondu na základě aktuální číslo ve frontě úkoly a rychlosti dokončení úkolů v projektu. Služba dávku pravidelně vyhodnotí funkci a přizpůsobí fondu podle zátěží na projektu (uzly pro mnoho úlohy ve frontě přidání a odebrání uzlů pro žádné úlohy ve frontě nebo pracovního) a jiných vzorců nastavení.

Vzorec měřítka lze založené na následujících metriky:

- **Čas metriky** vycházejí statistiky shromažďované každých pět minut zadaný počet hodin.

- **Metriky zdroje** založené na využití procesoru, využití šířky pásma, spotřebu paměti a počtu uzlů.

- **Metriky úkolu** jsou založené na stav úkolu, například *Active* (ve frontě), *spuštění*nebo *Dokončeno*.

Při automatické měřítko snižuje se počet výpočetních uzlů v fond, je třeba uvážit postup v případě úkoly, které používáte při operaci snížení. Tak, aby zahrnoval, dávku poskytuje *možnost navracení zpět uzel* zahrnující ve vzorcích. Například můžete určit, že pracovního úkoly jsou přerušili okamžitě, přerušili okamžitě a potom zařazena pro spuštění na jiném uzlu nebo moct dokončit před uzel se odebere z fondu.

Další informace o automatické změny velikosti aplikace najdete v článku [automaticky měřítko výpočet uzlů v fondu dávku Azure](batch-automatic-scaling.md).

> [AZURE.TIP] Jak dosáhnout maximální využití prostředků pro využití, nastavte cílové počtu uzlů nula na konci projektu, ale chcete povolit spuštění úkoly k dokončení.

## <a name="security-with-certificates"></a>Cenného papíru s certifikátů

Potřebujete obvykle provádějte certifikáty šifrování a dešifrování citlivé informace pro úkoly, jako je klíč pro [účet Azure úložiště][azure_storage]. K podpoře to, můžete nainstalovat certifikáty na uzlů. Šifrované tajemství předán úkolů pomocí parametrů příkazového řádku nebo vložené do některý z materiálů úkolu a nainstalované certifikáty mohou sloužit k jejich dešifrování.

Použijte [certifikát přidat] [ rest_add_cert] operace (dávku ZBYTEK) nebo [CertificateOperations.CreateCertificate] [ net_create_cert] metoda (dávku .NET) přidáte účet dávku certifikát. Pak můžete přiřadit certifikát, který má fond nové nebo existující. Po přidružené k fond certifikát službu dávku nainstaluje certifikát v jednotlivých uzlech ve fondu. Služba dávku nainstaluje příslušných certifikátů při uzel spuštění systému, před spuštěním všech úkolů (včetně zahájení úkolu a úlohu správce).

Pokud přidáte do *existujícího* fondu certifikáty, je nutné restartovat jeho výpočetních uzlů certifikátů mohou být použity pro uzlů.

## <a name="error-handling"></a>Zpracování chyb

Je možné je nutné ke zpracování chyb úkolu a aplikace v rámci vašeho řešení dávku.

### <a name="task-failure-handling"></a>Chyba při zpracování úkolů
Selhání úkol rozdělit do těchto kategorií:

- **Plánování k chybám**

    Pokud z nějakého důvodu selže přenos souborů, které jsou určeny pro daný úkol, "plánování chyba" je nastavena pro daný úkol.

    Plánování chyby může dojít, pokud soubory zdroje úkolu přesunuli, účtu úložiště už není k dispozici nebo jiný problém byl zjištěn zabraňující úspěšné kopírování souborů na uzel.

- **Chyby aplikací**

    Proces, který není zadán pomocí příkazového řádku úkolu můžete také fungovat. Tento proces se považuje za se nezdařil při nenulová ukončovacího vrácení procesem, který se spustí úkolu (viz *kódy ukončení úkolu* v následující části).

    U aplikace chyby můžete nakonfigurovat dávku automaticky opakovat úkol nahoru na zadaný počet.

- **Omezení chyby**

    Můžete nastavit omezení, která určuje maximální spuštění doba trvání pro úkol nebo úkolů, *maxWallClockTime*. To může být užitečné pro ukončení "neodpovídá" úkoly.

    Pokud byla překročena maximální počet hodin, úkol se označí jako *dokončený*, ale ukončovacího nastavena na `0xC000013A` a *schedulingError* pole označeno jako `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Ladění chyb aplikace

- `stderr`a`stdout`

    Během spuštění aplikace mohou vznikat diagnostiky výstup, který můžete použít k řešení problémů. Jak je uvedeno v předchozí části [soubory a adresáře](#files-and-directories), službu dávku zapíše standardní chyba výstup a standardní výstup `stdout.txt` a `stderr.txt` souborů v adresáři úkolu na uzel výpočetních. Pomocí portálu Azure nebo jedna dávka SDK můžete stáhnout tyto soubory. Můžete třeba načíst tyto a další soubory pro řešení potíží s použitím [ComputeNode.GetNodeFile] [ net_getfile_node] a [CloudTask.GetNodeFile] [ net_getfile_task] v knihovně dávku .NET.

- **Kódy ukončení úkolu**

    Jak jsme zmínili dříve, je úkol označen jako neúspěšný službou dávku proces, který se spustí úkol chybovou nenulová ukončovacího. Jakmile úkol spustí proces, dávku naplní vlastnost kód ukončení úkolu s *vrátit kód procesu*. Je důležité si všimněte si, že kód ukončení úkolu **není** určen službu dávku – je určený podle samotný proces nebo operační systém, na kterém procesu spuštěn.

### <a name="accounting-for-task-failures-or-interruptions"></a>Účetní k úkolu nějaké vyrušování

Úkoly může občas dojít nebo přeruší. Může selhat samotnou aplikaci úkolu, může restartování uzel, na kterém běží úkol nebo uzel může odeberou z fondu během operace změny velikosti když zásady navracení zpět do fondu nastavené odebrat uzly okamžitě bez nutnosti čekání na úkoly k dokončení. Ve všech případech úkol může být automaticky zařazena tak, že dávka pro spuštění na jiném uzlu.

Je také možné k chybovému problému způsobilo úkolu zablokování nebo zabralo příliš mnoho času pro spuštění. Můžete nastavit maximální spuštění doby pro daný úkol. Pokud je překročena, dávku přeruší aplikaci úkolu.

### <a name="connecting-to-compute-nodes"></a>Připojení k výpočtu uzlů

Je možné provést další ladění a řešení potíží s přihlášením k výpočetní uzly vzdáleně. Portál Azure můžete použít ke stažení souboru vzdálené plochy RDP (Protocol) pro Windows uzly a získat informace o připojení zabezpečené prostředí (SSH) pro uzly Linux. To můžete taky udělat pomocí API dávku – například [Dávku .NET] [ net_rdpfile] nebo [Dávku Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Připojení k uzel prostřednictvím RDP nebo SSH, musíte nejdřív vytvoříte uživatele na uzel. K tomuto účelu slouží Azure portálu, [Přidání uživatelského účtu na uzel] [ rest_create_user] pomocí rozhraní REST API dávku volání [ComputeNode.CreateComputeNodeUser] [ net_create_user] metodu v dávku .NET nebo volání [add_user] [ py_add_user] metoda v modulu dávku Python.

### <a name="troubleshooting-bad-compute-nodes"></a>Poradce při potížích "bad" výpočet uzlů

V situacích, kdy některé úkoly selhává dávku klientské aplikace nebo služby zkontrolovat metadata selhalo úkoly k identifikaci špatně fungující uzlu. Každý uzel do fondu je uveden jedinečné ID a uzel spuštěn úkol je součástí metadata úkolu. Teď, když jste určili uzel problém, můžete provést několik akcí s ním:

- **Restartujte uzel** ([REST][rest_reboot] | [.NET][net_reboot])

    Restartování uzel můžete někdy uvolněte skryté problémy jako zablokované nebo chyb procesů. Všimněte si, že pokud váš fond používá zahájení úkolu nebo práce používá Příprava úlohu, jsou prováděna po restartování uzel.

- **Reimage uzel** ([REST][rest_reimage] | [.NET][net_reimage])

    To na uzel opětovná instalace operačního systému. Stejně jako u restartování uzel, zahájení úkolu a příprava úlohy jsou spusťte po uzel má reimaged.

- **Odebrání uzel z fondu** ([REST][rest_remove] | [.NET][net_remove])

    V některých případech je nutné úplně odebrat uzel z fondu.

- **Zakázání na uzel plánování úkolu** ([REST][rest_offline] | [.NET][net_offline])

    To efektivně trvá uzel "offline" takže žádné další úkoly přiřazené k němu, ale umožňuje uzel zůstat spuštěn a ve fondu. Umožňuje provádět další vyšetřování příčinu selhání bez ztráty dat selhalo úkolů – a bez uzel příčinou selhání dalších úkolů. Například můžete zakázat plánování na uzel, pak se [Přihlaste vzdáleně](#connecting-to-compute-nodes) prozkoumat na uzel protokoly událostí nebo provedení dalších Poradce při potížích s úkolů. Po dokončení svůj výzkum, můžete přepněte zpět do stavu online uzel povolením plánování úkolů ([REST][rest_online] | [.NET][net_online]), nebo proveďte jednu z ostatních akcí bylo popsáno dříve.

> [AZURE.IMPORTANT] U každé akce, které jsou popsané v této části – restartujte, reimage odebrat a plánování úkolů zakázat – budete moci určit způsob zpracování úkolů aktuálně spuštěných pro uzel po provedení akce. Třeba když zakážete plánování úkolů na uzel pomocí knihovnu dávku .NET klienta, můžete určit [DisableComputeNodeSchedulingOption] [ net_offline_option] hodnota výčtu můžete určit, zda chcete **Ukončit** spuštěný úkoly **Requeue** je při plánování na uzlech, nebo ji povolit spuštění úkoly k dokončení před provedením akcí (**TaskCompletion**).

## <a name="next-steps"></a>Další kroky

- Projděte si dávku ukázku podrobné v [Začínáme s knihovnou Azure dávka pro .NET](batch-dotnet-get-started.md)aplikace. Je také [verzi Python](batch-python-tutorial.md) kurzu, které se spouští úlohu uzlech výpočetním Linux.

- Stáhněte si a [Dávku Explorer] sestavení[ github_batchexplorer] vzorku projektu pro použití při vyvíjet řešení dávku. V programu Průzkumník dávky může provádět následující a další:
  - Sledování a pracovat s fondů, projekty a úkoly v rámci vašeho účtu dávku
  - Stáhněte si `stdout.txt`, `stderr.txt`a jiné soubory z uzlů
  - Vytvoření uživatelů v uzlech a stahování souborů RDP pro vzdálené přihlášení

- Zjistěte, jak [vytvářet skupiny uzlech výpočetním Linux](batch-linux-nodes.md).

- Navštivte [Fórum komunity Azure dávku] [ batch_forum] na webu MSDN. Fórum je vhodným místem pro klást otázky, jestli jenom osvojování si nebo jsou odborník na používání dávku.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
