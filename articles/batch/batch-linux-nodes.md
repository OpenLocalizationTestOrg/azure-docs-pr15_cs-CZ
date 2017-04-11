<properties
    pageTitle="Linux uzlů v Azure dávku fondů | Microsoft Azure"
    description="Zjistěte, jak zpracuje paralelní výpočetním úloh na fondů Linux virtuálních počítačích v Azure listu."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Zřízení Linux výpočetním uzlů v Azure dávku fondů

Dávkové Azure slouží ke spuštění pracovního vytížení paralelní výpočetním na virtuálních počítačích Linux a Windows. Tento článek popisuje, jak vytvořit fondů Linux výpočetním uzlů ve službě dávku pomocí obou [Dávku Python] [ py_batch_package] a [.NET dávku] [ api_net] knihoven klienta.

> [AZURE.NOTE] [Balíčků aplikací](batch-application-packages.md) jsou aktuálně nepodporuje uzlech výpočetním Linux.

## <a name="virtual-machine-configuration"></a>Konfigurace virtuálního počítače

Když vytvoříte fond výpočetním uzlů v listu, máte dvě možnosti ze kdy vyberte požadovanou velikost uzel a operační systém: Cloud Services a konfigurace virtuálního počítače.

**Konfigurace služby cloudu** poskytuje že Windows výpočet uzly *pouze*. K dispozici výpočetním uzel velikosti jsou uvedeny v [velikosti Cloudovým službám](../cloud-services/cloud-services-sizes-specs.md)a dostupných operačních systémů, najdete v [Azure hostovaného OS vydáních a SDK kompatibility matici](../cloud-services/cloud-services-guestos-update-matrix.md). Když vytvoříte skupinu, která obsahuje uzly Azure Cloud Services, budete muset zadat jenom velikost uzel a jeho "operačních systémů," které se nacházejí ve výše uvedené články. Pro fondy Windows výpočet uzly, se používá nejčastěji Cloudovým službám.

**Konfigurace virtuálního počítače** poskytuje Linux a Windows obrázky výpočetním uzlů. K dispozici výpočetním uzel velikosti jsou uvedeny [velikosti virtuálních počítačích v Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) a [velikost virtuálních počítačích v Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Když vytvoříte skupinu, která obsahuje uzly konfiguraci virtuálního počítače, je nutné zadat velikost uzlech, odkaz obrázek virtuálního počítače a agenta uzel dávka SKU v jednotlivých uzlech je třeba nainstalovat.

### <a name="virtual-machine-image-reference"></a>Obrázek odkazu virtuálního počítače

Službu dávka používá k poskytování Linux výpočetním uzly [Nastaví měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) . Operační systém obrázky tyto virtuálních počítačích podle [Azure Marketplace][vm_marketplace]. Při konfiguraci odkaz obrázek virtuálního počítače zadáte vlastnosti virtuálního počítače obrázek Marketplace. Následující vlastnosti jsou potřeba při vytváření odkaz obrázek virtuálního počítače:

| **Přehled vlastností obrázků** | **Příklad** |
| ----------------- | ------------------------ |
| Aplikace Publisher         | Kanonický                |
| Nabídka             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Verze           | nejnovější                   |

> [AZURE.TIP] Další informace o těchto vlastností a jak zařadit Tržiště obrázků v [obrázek a vyberte Linux virtuálního počítače obrázky v Azure pomocí rozhraní příkazového řádku nebo Powershellu](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Všimněte si, že ne všechny obrázky Marketplace aktuálně kompatibilní s listem. Další informace najdete v tématu [uzel agent SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Agent uzel SKU

Agent uzel dávku je program, který běží v jednotlivých uzlech z fondu a poskytuje rozhraní příkaz a řízení mezi uzel a služba dávku. Existují různé implementace agent uzel jmenoval skladové jednotky, jiné operační systémy v počítačích. V podstatě při vytváření konfiguraci virtuálního počítače zadáte odkaz obrázek virtuálního počítače a zadejte agenta uzel nainstalovat na obrázek. Obvykle každý uzel agent SKU je kompatibilní s více obrázků virtuálního počítače. Tady je několik příkladů uzel agent skladové jednotky:

* batch.Node.Ubuntu 14.04
* batch.Node.centos 7
* batch.Node.Windows amd64

> [AZURE.IMPORTANT] Ne všechny obrázky virtuálního počítače, které jsou dostupné v Tržiště jsou kompatibilní s momentálně neexistuje uzel agentů dávku. Je nutné použít SDK dávka seznam dostupných uzel agent skladové jednotky a obrázky virtuálního počítače, s nimiž jsou kompatibilní. Zobrazení [seznamu virtuálního počítače obrázky](#list-of-virtual-machine-images) dál v tomto článku najdete další informace.

## <a name="create-a-linux-pool-batch-python"></a>Vytvoření fondu Linux: dávka Python

Následující fragment kódu znázorňuje příklad toho, jak používat [Microsoft Azure dávku klienta knihovny pro Python] [ py_batch_package] vytvoření fondu serveru se systémem Ubuntu výpočetním uzlů. Odkaz si přečtěte následující dokumentaci pro modul Python dávku, najdete na [balíček azure.batch] [py_batch_docs] na číst dokumenty.

Tento úryvek vytvoří [ImageReference] [ py_imagereference] explicitně a určuje všech jeho vlastností (Publisheru, nabídky, SKU, verze). Ve výrobním kódu, doporučujeme použít [list_node_agent_skus] [ py_list_skus] způsob, jak zjistit a vyberte z dostupných obrázek a uzel agent SKU kombinace za běhu.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Jak jsme zmínili dříve, doporučujeme, aby místo abyste vytvářeli [ImageReference] [ py_imagereference] výslovně, použít [list_node_agent_skus] [ py_list_skus] způsob, jak dynamicky vyberte z kombinací aktuálně podporované uzel agent/Marketplace obrázek. Následující úryvek Python ukazuje používání této metody.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Vytvoření fondu Linux: dávka .NET

Následující fragment kódu zobrazení příkladu použití [Dávku .NET] [ nuget_batch_net] klienta knihovny vytvoření fondu serveru se systémem Ubuntu výpočetním uzlů. Najdete v [dokumentaci dávku .NET] [ api_net] na webu MSDN.

Následující fragment kódu používá [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] metoda vyberte ze seznamu současnosti podporované Marketplace obrázek a uzel agent SKU kombinace. Tento postup je vhodné, protože seznam podporovaných kombinací můžou se lišit od času. Nejčastěji se přidají podporované kombinace.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

I když předchozí úryvek používá [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] podporované způsob, jak dynamicky seznam a vyberte obrázek a uzel agent SKU kombinace (doporučeno), můžete taky nakonfigurovat [ImageReference] [ net_imagereference] explicitně:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Seznam obrázků virtuálního počítače

V následující tabulce jsou obrázky virtuálního počítače Marketplace, které jsou kompatibilní s dostupné uzel agentů dávku kdy naposledy aktualizovaly tohoto článku. Je důležité mít na paměti, že tento seznam není konečné, protože obrázky a uzel agentů může být přidán nebo kdykoli odebrat. Doporučujeme dávka aplikací a služeb vždy použít [list_node_agent_skus] [ py_list_skus] (Python) a [ListNodeAgentSkus] [ net_list_skus] (dávka .NET) určit, a potom vyberte z aktuálně dostupné skladové jednotky.

> [AZURE.WARNING] Následující seznam může kdykoli změnit. Vždy použijte k dispozici v rozhraní API dávku **seznamu uzel agent SKU** metod k seznamu a pak vyberte kompatibilní virtuální počítač a uzel agent skladové jednotky při spuštění dávky úloh.

| **Aplikace Publisher** | **Nabídka** | **Obrázek SKU** | **Verze** | **Agent uzel SKU ID** |
| ------- | ------- | ------- | ------- | ------- |
| Kanonický | UbuntuServer | 14.04.0-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 14.04.1-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 14.04.2-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 14.04.3-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 14.04.4-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 14.04.5-LTS | nejnovější | batch.Node.Ubuntu 14.04 |
| Kanonický | UbuntuServer | 16.04.0-LTS | nejnovější | batch.Node.Ubuntu 16.04 |
| Credativ | Debian | 8 | nejnovější | batch.Node.debian 8 |
| OpenLogic | CentOS | 7.0 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS | 7.1 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | nejnovější | batch.Node.centos 7 |
| OpenLogic | CentOS | 7.2 | nejnovější | batch.Node.centos 7 |
| Oracle | Oracle Linux | 7.0 | nejnovější | batch.Node.centos 7 |
| SUSE | openSUSE | 13.2 | nejnovější | batch.Node.opensuse 13.2 |
| SUSE | openSUSE přestupných | 42.1 | nejnovější | batch.Node.opensuse 42.1 |
| SUSE | SLES HPC | 12 | nejnovější | batch.Node.opensuse 42.1 |
| SUSE | SLES | 12 SP1 | nejnovější | batch.Node.opensuse 42.1 |
| Microsoft služby Active Directory | Standardní dat pro výzkum OM | Standardní dat pro výzkum OM | nejnovější | batch.Node.Windows amd64 |
| Microsoft služby Active Directory | Linux dat pro výzkum OM | linuxdsvm | nejnovější | batch.Node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Datacentra 2012 | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 R2 datacentru | nejnovější | batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows Server Technical Preview | nejnovější | batch.Node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Připojení k Linux uzlů

Během vývoje nebo při řešení problémů můžete narazit potřeba se přihlásit k uzlů v vašeho fondu. Na rozdíl od Windows výpočetním uzly nelze použít vzdálené plochy RDP (Protocol) se připojit k uzly Linux. Místo toho službu dávku umožňují přístup SSH v jednotlivých uzlech pro připojení ke vzdálené.

Následující fragment kódu Python vytvoří uživatele ve všech uzlech fond, který je potřebný pro připojení ke vzdálené. Potom vytiskne informace o jednotlivých uzlech připojení zabezpečené prostředí (SSH).

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Tady je ukázka výstup pro předchozí kód pro skupinu, která obsahuje čtyři uzly Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Všimněte si, že místo heslo, je možné zadat veřejným klíčem SSH při vytváření uživatele na uzel. V Python SDK to se provádí pomocí parametru **ssh_public_key** na [ComputeNodeUser][py_computenodeuser]. V .NET, to se provádí pomocí [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] vlastnost.

## <a name="pricing"></a>Ceny

Azure dávku jsou založeny na technologii Azure cloudovými službami a Azure virtuálních počítačích. Služba dávku sama je k dispozici zdarma, což znamená, že vám bude účtovaná jenom pro zdroje výpočetním, že dávka řešení používat. Když zvolíte **Konfigurace služby cloudu**, můžete se bude účtovat podle [Cloudovým službám ceny] [ cloud_services_pricing] strukturu. Když zvolíte **Konfiguraci virtuálního počítače**, můžete se bude účtovat založené na [virtuálních počítačích ceny] [ vm_pricing] strukturu.

## <a name="next-steps"></a>Další kroky

### <a name="batch-python-tutorial"></a>Kurz Python dávku

Výukové důkladněji o tom, jak pracovat s listem pomocí Python najdete v článku [Začínáme s klientem Python dávku Azure](batch-python-tutorial.md). Jeho companion [Ukázka kódu] [ github_samples_pyclient] s funkcí helper `get_vm_config_for_distro`, zobrazující jiný postup získání konfiguraci virtuálního počítače.

### <a name="batch-python-code-samples"></a>Dávkové Python ukázky

Podívejte se jiných [Python kódu ukázky] [ github_samples_py] v [azure dávku vzorky] [ github_samples] úložiště na GitHub pro několik skripty, které ukazují, jak provádět běžné dávku operace, například fondu, projektu a vytvoření úkolů. [Soubor Readme pro] [ github_py_readme] , který doprovází Python ukázky obsahuje podrobnosti o tom, jak instalovat požadovaných balíčky.

### <a name="batch-forum"></a>Fórum komunity dávku

[Fórum komunity dávka Azure] [ forum] na webu MSDN je ideální místo k diskutovat o dávka se a položte otázky týkající se služby. Čtení příspěvků užitečné "stickied" a pošlete svoje otázky, kterým dochází při vytvoření dávky řešení.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
