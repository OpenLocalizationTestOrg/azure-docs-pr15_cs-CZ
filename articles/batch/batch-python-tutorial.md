<properties
    pageTitle="Kurz – Začínáme s klientem Azure dávku Python | Microsoft Azure"
    description="Další základní koncepty Azure list a jak se dají službu dávka s jednoduchý scénář"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Začínáme s klientem Python dávku Azure

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Naučte se základy [Azure dávku] [ azure_batch] a [Dávku Python] [ py_azure_sdk] klienta jako probereme tato aplikace malých dávku vytvořené v Python. Podíváme na použití dvou ukázkové skripty použít službu dávku zpracuje paralelní zátěž Linux virtuálních počítačích v cloudu a jak pracovat s [Azure úložiště](./../storage/storage-introduction.md) pro soubor pracovní a načítání. Budete další běžné pracovního postupu aplikace dávku a získat základní principy hlavní komponenty dávku, třeba projekty, úkoly, fondů a výpočet uzlů.

![Pracovní postup řešení dávku (basic)][11]<br/>

## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto článku se předpokládá být praxi Python a znalost Linux. Dále předpokládá, že se vám povede požadavkům účtu vytváření, které jsou uvedeny níže Azure a dávku a úložiště služby.

### <a name="accounts"></a>Účty

- **Azure účtu**: Pokud ještě nemáte předplatné Azure, [Vytvořit bezplatný účet Azure][azure_free_account].
- **Dávkové účtu**: Pokud máte předplatné Azure, [vytvořte účet Azure dávku](batch-account-create-portal.md).
- **Úložiště účtu**: Přečtěte si článek [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Ukázka kódu

Výuková Python [Ukázka kódu] [ github_article_samples] jednu z mnoha ukázek kódu dávku nachází v [azure dávku vzorky] [ github_samples] úložiště na GitHub. Stáhnout všechny vzorky kliknutím **klonovat nebo stahování > stáhnout ZIP** na domovskou stránku úložiště nebo po kliknutí na [azure dávku vzorky master.zip] [ github_samples_zip] přímý odkaz. Jakmile jste extrahovaných obsah souboru ZIP, se nacházejí dva skripty pro účely tohoto návodu `article_samples` directory:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python prostředí

Ukázka skriptu *python_tutorial_client.py* k počítači, místní spustíte musíte **Video interpreter Python** kompatibilní s verze **2.7** nebo **3.3 +**. Skript proběhly v Linux a Windows.

### <a name="cryptography-dependencies"></a>šifrování závislostí

Je třeba nainstalovat závislosti pro [šifrování] [ crypto] knihovny vyžadované `azure-batch` a `azure-storage` Python balíčků. Proveďte jednu z následujících operací vhodné pro svoji platformu nebo v nápovědě k [instalaci kryptografický] [ crypto_install] podrobnosti o další informace:

* Se systémem Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Pokud instalace pro Python 3.3 + na Linux pomocí ekvivalenty python3 závislostí Python. Například na systémem Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure balíčků

Potom nainstalujte balíčků **Azure list** a Python **Azure úložiště** . Lze provést pomocí **pip** a *requirements.txt* najdete tady:

`/azure-batch-samples/Python/Batch/requirements.txt`

Problém následující příkaz **pip** nainstalovat balíčků dávku a úložiště:

`pip install -r requirements.txt`

Nebo můžete nainstalovat [azure dávku] [ pypi_batch] a [úložišti azure] [ pypi_storage] Python balíčky ručně:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Budete muset připojit znak před příkazy s `sudo` Pokud používáte neoprávnění účet. Například `sudo pip install -r requirements.txt`. Další informace o instalaci Python balíčků, najdete v článku [Instalace balíčků] [ pypi_install] na readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Ukázka výuková kódu Python dávku

Ukázka výuková kódu dávku Python se skládá ze dvou Python skripty a několik datových souborů.

- **python_tutorial_client.PY**: bude spolupracovat s dávku a úložiště služby provádět paralelní pracovní zátěž uzlech výpočetní (virtuálních počítačích). Skript *python_tutorial_client.py* běží na místním počítači.

- **python_tutorial_task.PY**: skript, který běží na výpočet uzlů v Azure k provedení skutečné práce. Ve vzorku rozdělí *python_tutorial_task.py* text v soubor stáhnout z Azure úložiště (vstupní soubor). Potom vytvoří textový soubor (výstupní soubor), který obsahuje seznam horním tři slova, která se zobrazí v poli vstupní soubor. Po vytvoření výstupní soubor, *python_tutorial_task.py* , odešlou se soubor Azure úložiště. Díky tomu je k dispozici ke stažení klientský skript spuštěné v počítači. Skript *python_tutorial_task.py* běží paralelní více uzlů výpočetním ve službě dávky.

- **./data/taskdata\*txt**: tyto soubory ve formátu RTF tři zadávat spuštěné v operačním systému výpočetním uzlů úkolů.

Následující obrázek znázorňuje primární operace, které provádí pomocí skriptů klienta a úkol. Základní pracovní postup je typické pro mnoho výpočetním řešení, které jsou vytvořené s listem. Během neuvádí všechny funkce dostupné ve službě dávku, téměř každý dávku scénář zahrnuje částí tento pracovní postup.

![Příkladem dávku pracovního postupu][8]<br/>

[**Krok 1.**](#step-1-create-storage-containers) V úložišti objektů Blob Azure vytvořte **kontejnery** .<br/>
[**Krok 2.**](#step-2-upload-task-script-and-data-files) Nahrání souborů skript a zadání úkolu na kontejnery.<br/>
[**Krok 3.**](#step-3-create-batch-pool) Vytvoření dávky **fondu**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Při přihlášení ke fondu fondu **StartTask** stáhne skript úkolu (python_tutorial_task.py) uzlů.<br/>
[**Krok 4.**](#step-4-create-batch-job) Vytvoření dávky **projektu**.<br/>
[**Krok 5.**](#step-5-add-tasks-to-job) Přidání **úkolů** do projektu.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Úkoly jsou naplánované na uzly provést.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Každý úkol stáhne jeho zadávání dat z Azure úložiště a pak spuštění, nebude zahájen.<br/>
[**Krok 6.**](#step-6-monitor-tasks) Sledování úkolů.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Jak dokončení úkolů, uložení jejich výstupní data k základnímu úložišti Azure.<br/>
[**Krok 7.**](#step-7-download-task-output) Stáhněte si úkolu výstup z úložiště.

Jak provede tyto přesné kroky uvedené některé dávku řešení a může obsahovat mnoho další, ale ukázka znázorňuje běžných procesů v řešení dávku.

## <a name="prepare-client-script"></a>Připravte skript pro klienta

Před spuštěním vzorku, přidejte svoje dávku a úložiště přihlašovací údaje účtu *python_tutorial_client.py*. Pokud jste tak dosud neučinili, otevřete soubor v seznamu Oblíbené editor a aktualizovat následující řádky pomocí svých přihlašovacích údajů.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Nenajdete svoje dávku a úložiště přihlašovací údaje účtu do účtu zásuvné každé služby [Azure portál][azure_portal]:

![Dávkové přihlašovací údaje na portálu][9]
![úložiště pověření v portálu][10]<br/>

V následujících částech jsme analyzovat použijete skripty ke zpracování pracovního vytížení ve službě dávku kroky. Budeme rádi, když odkázat pravidelně na skriptů v editoru práci jak prostřednictvím zbytek tohoto článku.

Přejděte na následující řádek **python_tutorial_client.py** budete chtít začít krok 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Krok 1: Vytvoření úložiště kontejnery

![Vytvoření kontejnery v úložišti Azure][1]
<br/>

Dávkové podporuje předdefinované interakce s úložištěm Azure. Kontejnery ve vašem účtu úložiště poskytne soubory potřeby tak, že úkoly, které běží ve vašem účtu dávku. Kontejnery taky poskytují místo pro ukládání výstupu data, která vytvoří úkoly. První věc, kterou skriptu *python_tutorial_client.py* je vytvořit v [Úložišti objektů Blob Azure](../storage/storage-introduction.md#blob-storage)tři kontejnery:

- **aplikace**: Tento kontejner se uloží spuštění úkolů *python_tutorial_task.py*skriptu Python.
- **zadávání**: úkoly stáhne datové soubory zpracuje ze *vstupní* kontejner.
- **výstup**: při úkoly dokončit zpracování vstupní souboru, budou do kontejneru *výstup* nahraje výsledky.

Abyste mohli pracovat s účtem úložiště a vytvořit kontejnery, používáme [úložišti azure] [ pypi_storage] balíček, který chcete vytvořit [BlockBlobService] [ py_blockblobservice] objekt – "objektů blob klientovi." V okně úložiště účet pomocí klienta objektů blob potom vytvoříme tři kontejnery.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Po vytvoření kontejnery aplikaci teď nemůžete nahrávat soubory, které se použije úkoly.

> [AZURE.TIP] [Použití úložiště objektů Blob Azure z Python](../storage/storage-python-how-to-use-blob-storage.md) poskytuje dobrý přehled o práci s objekty BLOB Azure úložiště kontejnery. By měl být v horní části seznamu čtení, jak začít pracovat s listem.

## <a name="step-2-upload-task-script-and-data-files"></a>Krok 2: Odeslání úkolu skriptu a datových souborů

![Odeslání aplikace úkolu a vstupní (data) souborů pro kontejnery][2]
<br/>

V operaci nahrát soubor *python_tutorial_client.py* definováno kolekce cesty k souboru **aplikace** a **zadávání** platných v místním počítači. Potom se odešlou se tyto soubory kontejnery, které jste vytvořili v předchozím kroku.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Použití seznamu pochopení `upload_file_to_container` funkce nazývá pro každý soubor v kolekcí a dvě [ResourceFile] [ py_resource_file] vyplněné kolekcí. `upload_file_to_container` Funkce se zobrazí pod:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ py_resource_file] obsahuje úkoly v listu s adresou URL k souboru v úložišti Azure, která se stahuje do výpočetní uzly před spuštěním tohoto úkolu. [ResourceFile][py_resource_file]. Vlastnost **blob_source** Určuje úplnou adresu URL soubor uložených v úložišti Azure. Adresu URL mohou také obsahovat podpis sdílený přístup (přidružení zabezpečení), který obsahuje zabezpečený přístup k souboru. Většinu úkolů v listu patří *ResourceFiles* vlastnost, včetně:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

V tomto příkladu nepoužívá JobPreparationTask nebo JobReleaseTask typů úkolů, ale můžete další informace o nich v [Spustit přípravu a dokončení úlohy v listu Azure výpočet uzlů](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Sdílený přístup k podpisu (přidružení zabezpečení)

Sdílený přístup podpisy jsou řetězce, které poskytnutí zabezpečeného přístupu k kontejnerů a objektů BLOB v úložišti Azure. Skript *python_tutorial_client.py* používá obou objektů blob a kontejneru sdílený podpisy přístup a ukazuje, jak získat tyto řetězce podpis sdílený přístup z úložiště služby.

- **Kulatý sdílené podpisy přístup**: použití StartTask do fondu objektů blob sdílený přístup podpisy po stažení soubory úkolu skript a zadávání dat z úložiště (viz [Krok #3](#step-3-create-batch-pool) níže). `upload_file_to_container` Funkce v *python_tutorial_client.py* obsahuje kód, který získá každý objektů blob sdílený přístup podpis. Stejně tak tak, že zavoláte [BlockBlobService.make_blob_url] [ py_make_blob_url] v modulu úložiště.

- **Kontejner sdílený přístup podpis**: jako každý úkol dokončí svou práci na uzel výpočetním, odešle svůj výstupní soubor do kontejneru *výstup* v úložišti Azure. K tomu, používá *python_tutorial_task.py* podpis container sdílený přístup, který obsahuje zápisu do kontejneru. `get_container_sas_token` Funkce v *python_tutorial_client.py* získá kontejneru sdílený přístup podpis, který je pak předána jako argument příkazového řádku úkoly. Krok #5 [Přidání úkolů do projektu](#step-5-add-tasks-to-job), popisuje použití kontejneru přidružení zabezpečení.

> [AZURE.TIP] Podívejte se na řadu složené ze dvou částí na sdílený přístup podpisy [část 1: pochopení modelu přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytváření a používání přidružení zabezpečení se službou objektů Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), zobrazíte další informace o poskytnutí zabezpečeného přístupu k datům ve vašem účtu úložiště.

## <a name="step-3-create-batch-pool"></a>Krok 3: Vytvoření fondu dávku

![Vytvoření fondu dávku][3]
<br/>

Dávkové **fondu** je sada výpočetním uzly (virtuálních počítačích), na kterých spustí dávku úkolů projektu.

Po odeslání úkolu skriptu a datových souborů k tomuto účtu úložiště, spustí *python_tutorial_client.py* jeho interakci se službou dávku pomocí modulu dávku Python. K tomu nevyzve [BatchServiceClient] [ py_batchserviceclient] se vytvoří:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Pak fond uzlů výpočetním vytvořené v účtu dávka s volání `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Při vytváření fond definujete [PoolAddParameter] [ py_pooladdparam] určující několik vlastnosti skupiny:

- **ID** fondu (*id* – povinné)<p/>Stejně jako u většiny entit v listu, nového fondu musí mít jedinečný Identifikátor v rámci vašeho účtu dávku. Kód odkazuje na tomto fondu pomocí své ID a je jak identifikovat fondu Azure [portál][azure_portal].

- **Počet výpočetním uzlů** (*target_dedicated* - povinné)<p/>Tento parametr určuje, kolik VMs by měly být nasazeny z fondu. Je důležité Všimněte si, že všechny účty dávku mají výchozí **kvóty** , která omezuje počet **jádra** (a tedy výpočetním uzly) dávku účtu. Výchozí kvóty a pokyny ke [zvýšení kvótu](batch-quota-limit.md#increase-a-quota) (například maximální počet jádra ve vašem účtu dávku) můžete najít v [kvót a limity pro službu Azure dávku](batch-quota-limit.md). Pokud zjistíte, s dotazem, "Proč nebude Moje fondu dosáhla více než X uzly?" Příčinou může být tento základní kvóty.

- **Operační systém** uzly (*virtual_machine_configuration* **nebo** *cloud_service_configuration* - povinné)<p/>V *python_tutorial_client.py*vytvoříme fond Linux uzlů pomocí [VirtualMachineConfiguration][py_vm_config]. `select_latest_verified_vm_image_with_node_agent_sku` Fungovat v `common.helpers` zjednodušuje práci s [Azure Marketplace virtuálních počítačích] [ vm_marketplace] obrázky. Další informace o používání obrázků Marketplace najdete v článku [Poskytnutí Linux výpočet uzlů v Azure dávku fondů](batch-linux-nodes.md) .

- **Velikost výpočetní uzlů** (*vm_size* - povinné)<p/>Protože zadáváme Linux uzly pro naše [VirtualMachineConfiguration][py_vm_config], můžeme určit velikost OM (`STANDARD_A1` v tomto příkladu) z [velikosti virtuálních počítačích v Azure](../virtual-machines/virtual-machines-linux-sizes.md). Znovu [poskytování Linux výpočet uzlů v Azure dávku fondů](batch-linux-nodes.md) Další informace najdete.

- **Zahájení úkolu** (*start_task* - není povinné)<p/>Spolu s nad fyzický uzel vlastnosti, můžete také určit [StartTask] [ py_starttask] fondu (není potřeba). StartTask spustí v jednotlivých uzlech a uzel spojí fondu pokaždé, když se nerestartuje uzel. StartTask je užitečné především pro přípravu výpočetní uzly pro provádění úkolů, například k instalaci aplikace spuštěné úkolů.<p/>V této aplikaci ukázkové zkopíruje StartTask soubory, které ho stáhne z úložiště (které jsou definované pomocí vlastnosti **resource_files** StartTask) StartTask *pracovní adresář* *sdílené* adresář, který mají přístup k všechny úkoly na uzel. V podstatě se tím zkopíruje `python_tutorial_task.py` ke sdílenému adresáři v jednotlivých uzlech jako uzel spojí fondu, aby všechny úkoly, které běží na uzel k němu přístup.

Můžete zaznamenat volání `wrap_commands_in_shell` pomocná funkce. Tato funkce trvá kolekce samostatné příkazy a vytvoří jednu příkazového řádku odpovídající vlastnosti příkazového řádku úkolu.

Také výrazné v fragment kódu je použití dvě proměnné ve vlastnosti **příkazový_řádek** StartTask: `AZ_BATCH_TASK_WORKING_DIR` a `AZ_BATCH_NODE_SHARED_DIR`. Jednotlivých uzlech výpočetním fond dávku automaticky nakonfigurována několik proměnné, které jsou specifické pro dávky. Proces, který probíhá podle úkolu má přístup k tyto proměnné.

> [AZURE.TIP] Další informace o proměnné, které jsou k dispozici ve výpočetním uzlech fondu dávku, jakož i informace o úkolu pracovních adresářích, najdete **nastavení prostředí pro úkoly** a **soubory a adresáře** v [Přehled funkcí Azure dávku](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Krok 4: Vytvoření dávku

![Vytvoření dávku][4]<br/>

Dávkové **úlohy** je sada úkoly a je přidružená k fond výpočetním uzlů. Na přidružené fondu výpočetním uzly provést úkolů v projektu.

Použití úlohy nejen pro organizaci a sledování úkolů v související zatížení, ale také při uložení určitá omezení – například maximální runtime pro danou úlohu (a tím pádem svých úkolů) a úlohy především ostatní úlohy v okně dávku účet. V tomto příkladu ale úlohy je přidružený pouze fondu, který byl vytvořený v kroku #3. Žádná další vlastnosti jsou nakonfigurované.

Všechny úlohy dávku souvisí s fond konkrétních. Toto přiřazení označuje uzlů, které na provést úkolů projektu. Určení fondu pomocí [PoolInformation] [ py_poolinfo] vlastnost, jak je znázorněno fragment kódu.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Teď, když byl vytvořen projektu, úkoly přidají k provedení práce.

## <a name="step-5-add-tasks-to-job"></a>Krok 5: Přidání úkolů do projektu

![Přidání úkolů do projektu][5]<br/>
*(1) úkoly se přidají do projektu, (2) naplánováno spustit uzlech a (3) úkoly stahovat soubory dat pro zpracování*

Dávkové **úkoly** jsou požadované jednotlivé jednotky práce na uzly výpočetním provést. Úkol má příkazového řádku a spustit skripty nebo programů jiného, které zadáte do tohoto příkazového řádku.

Provádět skutečně práce, musí být přidán úkolů do projektu. Každý [CloudTask] [ py_task] je nastavena vlastnost příkazového řádku a [ResourceFiles] [ py_resource_file] (stejně jako u do fondu StartTask), která úkol stáhne na uzel před provedením příkazového řádku automaticky. Ve vzorku zpracuje každý úkol pouze jeden soubor. Proto svou kolekci ResourceFiles obsahuje jeden prvek.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Při přístupu k proměnné jako `$AZ_BATCH_NODE_SHARED_DIR` nebo spuštění aplikace nebyl nalezen na uzel `PATH`, příkaz Řádky úlohy musíte vyvolat prostředí explicitně, jako například s `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Tento požadavek je nutný úkolů spustit aplikaci na uzel `PATH` a neodkazuje všechny proměnné.

V `for` smyčka v fragment kódu, uvidíte, že příkazového řádku pro daný úkol je vytvořen s pět argumenty příkazového řádku předané *python_tutorial_task.py*:

1. **cesta k souboru**: uložených na uzel se jedná místní cesta k souboru. Když ResourceFile objektu v `upload_file_to_container` byla vytvořená v kroku 2 nahoře, název souboru používal pro tuto vlastnost ( `file_path` parametrů v konstruktoru ResourceFile). Tento údaj označuje, že soubor můžete najít ve stejném adresáři na uzel jako *python_tutorial_task.py*.

2. **NumWords**: horních *N* slova by se měly zapisovat do výstupní soubor.

3. **storageaccount**: název účtu úložiště, který vlastní kontejner, do kterého by měl nahráli výstupu úkolu.

4. **storagecontainer**: jméno container úložiště, na který mají výstup soubory nahrané.

5. **sastoken**: podpis sdílený přístup (přidružení zabezpečení), který obsahuje zápisu do kontejneru **výstup** v úložišti Azure. Skript *python_tutorial_task.py* používá tento sdílený přístup podpis při vytvoří jeho BlockBlobService odkaz. Zápis do kontejneru to poskytuje bez nutnosti přístupové klávesy účtu úložiště.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Krok 6: Sledování úkolů

![Sledování úkolů][6]<br/>
*Skript (1) sleduje úkoly stav dokončení a (2) úkoly nahrajte Výsledná data k základnímu úložišti Azure*

Po přidání úkolů do projektu jsou automaticky ve frontě a naplánovat provedení uzlech výpočetním v rámci fondu přidružený k projektu. Na základě nastavení, které zadáte, dávku pracuje s řízení fronty všech úkolů, plánování, opakování a další úkoly správy úkol za vás.

Sledování úkolů provádění mnoha způsoby. `wait_for_tasks_to_complete` Funkce v *python_tutorial_client.py* poskytuje jednoduchý příklad sledování úkolů pro určité stavu, v tomto případě [dokončen] [ py_taskstate] stavu.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Krok 7: Stažení výstup úkolu

![Stáhněte si úkolu výstup z úložiště][7]<br/>

Teď dokončení úlohy výstup úkoly můžete stáhnout z Azure úložiště. Důvodem je, s volání `download_blobs_from_container` v *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Volání `download_blobs_from_container` *python_tutorial_client.py* Určuje, že se mají stahovat soubory do adresáře Domů. Neváhejte změnit toto umístění výstupu.

## <a name="step-8-delete-containers"></a>Krok 8: Odstranění kontejnery

Protože vám bude účtovaná za dat uložených v úložišti Azure, je vždy vhodné odebrat všechny objekty BLOB, které jsou už není potřebné pro svou dávku úloh. V *python_tutorial_client.py*to lze provést pomocí tří volání [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Krok 9: Odstranění úlohy a fondu

V posledním kroku se zobrazí výzva k odstranění úlohy a fondu se vytvářely v *python_tutorial_client.py* skriptu. I když nejste účtovaná za projektů a úkolů, *jsou* účtovaná za výpočetním uzlů. Proto doporučujeme přidělit uzly pouze v případě potřeby. Odstranění nepoužitý fondů může být součást vašeho procesu údržbu.

BatchServiceClient [JobOperations] [ py_job] a [PoolOperations] [ py_pool] mít odpovídající odstranění metody, které se označují jako Pokud potvrzení odstranění:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Mějte na paměti, který vám bude účtovaná za pro využití prostředků – odstraníte nepoužívané fondů minimalizuje náklady. Také mějte na paměti, že odstranění fond odstraní všechny výpočetní uzlů v rámci tohoto fondu a, po které bude všechna data v jednotlivých uzlech obnovit po odstranění fondu.

## <a name="run-the-sample-script"></a>Spuštění ukázka skriptu

Když spustíte skript *python_tutorial_client.py* z výuková [Ukázka kódu][github_article_samples], výstup konzoly vypadá přibližně takto. Existuje pozastavit na `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` během do fondu výpočetním uzly vytvořené, spustit, a provedení příkazů do fondu zahájení úkolu. Použití [Azure portál] [ azure_portal] sledování vašeho fondu, výpočet uzly, projektu a úkoly během a po spuštění. Použití [Azure portál] [ azure_portal] nebo [Microsoft Azure úložiště Explorer] [ storage_explorer] zobrazíte prostředků úložiště (kontejnery a objekty BLOB), které jsou vytvořené v aplikaci.

>[AZURE.TIP] Spustit skript *python_tutorial_client.py* v rámci `azure-batch-samples/Python/Batch/article_samples` adresář. Používá pro relativní cestu `common.helpers` import modulu, takže možná uvidíte `ImportError: No module named 'common'` Pokud neměli skriptu z v rámci tento adresář.

Typické čas spuštění je **přibližně 5 až 7 minut** po spuštění vzorku ve výchozím nastavení.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Další kroky

Neváhejte upravte *python_tutorial_client.py* a *python_tutorial_task.py* experiment s jinou výpočetním scénáře. Například přidejte zpoždění spuštění *python_tutorial_task.py* simulovat dlouho probíhajících úkoly a sledovat na portálu. Zkuste přidání více úkolů nebo úpravu počtu uzlů výpočetním. Přidání logiky zjišťovat a povolit použití existující fond rychlost čas spuštění.

Teď, když znáte základní pracovní postup řešení dávku, je čas na dostanete k dalším funkcím dávku služby.

- Přečtěte si článek [dávku Azure přehled funkcí](batch-api-basics.md) , které doporučujeme, pokud jste ještě moc nedělali služby.
- Zahájení na jiných dávku vývoj článků ve **vývoj hloubkovou** [cesta výuky dávku][batch_learning_path].
- Podívejte se na různé provádění zpracování "horních N slova" úlohu s list v [TopNWords] [ github_topnwords] vzorku.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Vytvoření kontejnery v úložišti Azure"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Odeslání aplikace úkolu a vstupní (data) souborů pro kontejnery"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Vytvoření dávky fondu"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Vytvoření dávku"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Přidání úkolů do projektu"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Stáhněte si úkolu výstup z úložiště"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Pracovní postup řešení dávku (úplné diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Přihlašovací údaje dávku portálu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Úložiště pověření v portálu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Pracovní postup řešení dávku (minimální diagram)"
