<properties
   pageTitle="Doporučené konvence pro Azure zdroje | Pokyny pro | Microsoft Azure"
   description="Doporučené konvence pro Azure zdroje. Pojmenování virtuálních počítačích, úložiště účty, sítí, virtuálních sítí, podsítí a jiné Azure entity"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Doporučené konvence pro Azure zdroje

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Volba názvu pro všechny zdroje v Microsoft Azure je důležité, protože:

- Není těžké později změnit název.
- Názvy musí splňovat kritéria typu konkrétní zdrojů.

Jednotné konvence usnadnit vyhledání zdroje. Můžete také určit role zdroje v řešení.

Tento článek je přehled názvů pravidla a omezení Azure zdrojů a sadu podle směrného plánu doporučení pro vytváření názvů.  Můžete těmito doporučeními jako výchozí bod pro vlastní konvence konkrétní vašim potřebám.  

Klíč k úspěchu s konvence je vytvoření a sledování různých aplikací a organizace. 

## <a name="naming-subscriptions"></a>Pojmenování předplatná

Při vytváření názvů Azure předplatná, zkontrolujte podrobného názvy Principy kontextu a účel každé předplatného vymazat.  Při práci v prostředí mnoho předplatných, můžete sledovat sdílené pojmenování vylepšit projekty.

Doporučené vzor pro pojmenování předplatné je:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Společnosti by obvykle shodovat pro každého předplatného. Některé společnosti však může být společnosti podřízené ve struktuře organizace. Tyto společnosti může spravovat centrální skupinou IT. V těchto případech může být odlišeny tím, že název nadřazené společnosti (*Contoso*) a název společnosti podřízené (*Severní větru*).

- Oddělení je název v rámci organizace, kde pracovní skupin osob. Tato položka v rámci oboru jako volitelné.

- Product řádek je konkrétní název produktu nebo funkci, která se provádí z v oblasti.
Toto je obecně volitelné interních služeb a aplikací. Ale doporučujeme používat pro veřejné služby, které vyžadují snadno oddělení a identifikace (například pro vymazat oddělení fakturačního záznamy).

- Prostředí je název, který popisuje životním cyklu nasazení aplikací nebo služeb, jako je vývoj, q & a nebo výrobní.

| Společnosti | Oddělení | Řádek produktů nebo služeb | Prostředí | Celé jméno  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Výrobní | Výrobní AwesomeService contoso SocialGaming |
| Contoso | SocialGaming | AwesomeService | Odchylka | Odchylka AwesomeService SocialGaming contoso |
| Contoso | HO | InternalApps | Výrobní | Contoso IT InternalApps výrobní |
| Contoso | HO | InternalApps | Odchylka | Contoso IT InternalApps odchylka |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Umožňuje zabránit nejednoznačnosti přípony

Při vytváření názvů zdrojů v Azure, doporučujeme používat běžné předpony nebo přípony identifikovat typ a kontext zdroje.  Při všechny informace o typech metadata kontext, je k dispozici programově, použití běžné přípony zjednodušuje vizuální identifikace.  Při zahrnutí přípony do vytváření názvů, je důležité jasně určit, zda připojí na začátku názvu (předpona) nebo na konci (přípona).  

Tady jsou například dvou možných názvů pro hostování modul výpočtu služby:

- SvcCalculationEngine (předpona)
- CalculationEngineSvc (přípona)

Přípony mohou odkazovat na různé aspekty, které popisují určitého zdroje. V následující tabulce jsou uvedeny příklady obvykle používá.

| Poměr | Příklad | Poznámky |
| ------ | ------- | ----- |
| Prostředí | vývoj, výrobní, q & a | Určuje prostředí pro zdroje |
| Umístění | uw (nám západní), ue (nám východ) | Určuje oblasti, do které daný zdroj nasazení |
| Instanci | 01, 02 | Pro zdroje, které mají víc pojmenované instanci (webových serverů, atd.). |
| Produkt nebo službu | Služba | Určuje produkt, aplikaci nebo službu, která podporuje zdroje |
| Role | SQL, web, zasílání zpráv | Určuje roli přidružené zdroje |

Při vytváření konkrétní konvence pro vaše firma nebo projektů, je hlavně zvolte jednu sadu přípony a jejich umístění (nebo název předpona).

## <a name="naming-rules-and-restrictions"></a>Pojmenování pravidla a omezení

Každý typ zdroje nebo službu v Azure vynutí sadu názvů omezení a rozsah. konvence pojmenování nebo vzorku musí dodržovat nutné naming pravidla a obor.  Například název virtuálního počítače odpovídá názvu DNS (a tedy musí být jedinečný ve všech Azure), název VNET má obor vymezený na vytvořeném v rámci skupiny zdrojů.

Obecně nemuseli žádné speciální znaky (`-` nebo `_`) jako první nebo poslední znak v libovolné jméno. Tyto znaky způsobí selhání většina ověřovacích pravidel.

| Kategorie | Služba či entitu | Rozsah | Délka | Velikost písmen | Platné znaky | Navrhované vzorek | Příklad |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Pole Skupina zdroje | Pole Skupina zdroje | Globální | 1-64 | Malá a velká písmena | Alfanumerický, podtržení a pomlčka | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Pole Skupina zdroje | Nastavení dostupnosti | Pole Skupina zdroje | 1-80 | Malá a velká písmena | Alfanumerický, podtržení a pomlčka | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Obecné | Značka | Přidružená entita | 512 (jméno), 256 (hodnota) | Malá a velká písmena | Alfanumerický | `"key" : "value"` | `"department" : "Central IT"` |
| Výpočet | Virtuální počítač | Pole Skupina zdroje | 1-15 | Malá a velká písmena | Alfanumerický, podtržení a pomlčka | `<name>-<role>-<instance>` | `profx-sql-001` |
| Úložiště | Název účtu úložiště (data) | Globální | 3 až 24 | Malá písmena | Alfanumerický | `<service short name><type><number>` | `profxdata001` |
| Úložiště | Název účtu úložiště (disků) | Globální | 3 až 24 | Malá písmena | Alfanumerický | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Úložiště | Název kontejneru | Úložiště účtu | 3 – 63 |   Malá písmena | Alfanumerický a přerušované čáry | `<context>` | `logs` |
| Úložiště | Název objektů BLOB | Kontejner | 1 024 1 | Malá a velká písmena | Všechny adresy URL znak | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Úložiště | Název fronty | Úložiště účtu | 3 – 63 | Malá písmena | Alfanumerický a přerušované čáry | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Úložiště | Název tabulky | Úložiště účtu | 3 – 63 |Malá a velká písmena | Alfanumerický | `<service short name>-<context>` | `awesomeservice-logs` |
| Úložiště | Název souboru | Úložiště účtu | 3 – 63 | Malá písmena | Alfanumerický | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Sítě | Virtuální sítě (VNet) | Pole Skupina zdroje | 2-64 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<service short name>-[section]-vnet` | `profx-vnet` |
| Sítě | Podsítě | Nadřazené VNet | 2 – 80 | Velká a malá písmena | Alfanumerický, podtržení, přerušované čáry a období | `<role>-subnet` | `gateway-subnet` |
| Sítě | Rozhraní sítě | Pole Skupina zdroje | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Sítě | Skupina zabezpečení sítě | Pole Skupina zdroje | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Sítě | Skupina zabezpečení sítě pravidla | Pole Skupina zdroje | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<descriptive context>` | `sql-allow` |
| Sítě | Veřejnou IP adresu | Pole Skupina zdroje | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<vm or service name>-pip` | `profx-sql1-pip` |
| Sítě | Vyrovnávání zatížení | Pole Skupina zdroje | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `<service or role>-lb` | `profx-lb` |
| Sítě | Načtení konfigurace rovnováha pravidel | Vyrovnávání zatížení | 1-80 | Velká a malá písmena | Alfanumerický, přerušované čáry, podtržení a období | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Uspořádání zdroje se značkami

Správce prostředků Azure podporuje značek entity pomocí libovolného textových řetězců k identifikaci kontext a zjednodušit automatizaci.  Například značku `"sqlVersion: "sql2014ee"` by bylo možné identifikovat VMs v nasazení systém SQL Server 2014 Enterprise Edition pro spuštění automatické skript vůči je.  Značky bude použito rozšířit a vylepšení kontextu straně konvence zvolili.

> [AZURE.TIP]Další výhodou značky je značky rozsahu skupiny zdrojů, umožňuje propojit a sladit entity přes různorodé nasazení.

Každý zdroj nebo skupina zdroje můžete mít maximálně **15** značky. Název značky se omezí na 512 znaků a hodnoty značky se omezí na 256 znaků.

Další informace o použití značek u zdroje podívejte se do [pomocí značek k uspořádání Azure zdroje](../resource-group-using-tags.md).

Některé běžné případy použití značek jsou:

- **Fakturace**; Seskupování zdrojů a přiřadí mu fakturace nebo nákladů zpátky kódy.
- **Identifikace kontextu služeb**; Určení skupiny zdrojů mezi skupinami zdroje pro společná operace a seskupení
- **Řízení přístupu a naplánovaný**; Správní role identifikace podle portfolia systému, služby, aplikace, instance, atd.

> [AZURE.TIP]Označení začátku – často značku.  Lepší mít směrný plán značky schématu na místě a upravte přes čas a nikoli s pozměnit ve skutečnosti.  

Příklad některých běžných značek postupů:

| Název značky | Klíč | Příklad | Komentář |
| -------- | --- | ------- | ------- |
| Faktury / vnitřní zpětné ID | billTo  | `IT-Chargeback-1234` | Vstupu a vnitřního výstupu nebo fakturační kód |
| Operátor nebo přímo zodpovědný jednotlivcem (DRI) | managedBy | `joe@contoso.com`  | Alias nebo e-mailovou adresu |
| Název projektu | název projektu | `myproject`  | Název projektu nebo produkt řádku |
| Verze projektu | verze projektu | `3.4`  | Verze projektu nebo produkt čáry |
| Prostředí | prostředí | `<Production, Staging, QA >` | Identifikátor URI prostředí | 
| Vrstvy | vrstvy | `Front End, Back End, Data` | Identifikace osy nebo role nebo kontext |
| Profil dat | dataProfile | `Public, Confidential, Restricted, Internal` | Utajení uložených ve zdroji dat |
 
## <a name="tips-and-tricks"></a>Tipy a triky

Některé typy zdrojů může vyžadovat další péče o pojmenovávání a konvence.

### <a name="virtual-machines"></a>Virtuálních počítačích

Zejména větší topologií pečlivě pojmenování virtuálních počítačích zjednodušuje identifikace rolí a účel každé počítače a povolení předvídatelně skriptování.

> [AZURE.WARNING]Každý virtuálního počítače v Azure má název Azure zdroje i název hostitelského operačního systému.  
> Pokud se název zdroje a název hostitele liší, Správa VMs může být obtížné a je nutno.
> Například virtuálního počítače vytvořeném z VHD to již obsahuje nakonfigurované operačního systému s název hostitele.

- [Konvence pro Windows Server VMs](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Účty úložiště a entity úložiště

Existují dva hlavní použití balení úložiště účty – zálohování disků pro VMs a ukládání dat v tabulkách, fronty a objekty BLOB.  Úložiště účty, které používají OM disků by měl postupujte podle konvence přidružení s nadřazené OM název (a potenciální potřebám u více účtů úložiště pro skladové jednotky kvalitní OM také použít příponu čísla).

> [AZURE.TIP]Úložiště účty – pro data nebo disků – postupujte podle pojmenování, která umožňuje určit více účtů úložiště využít (tedy vždy pomocí číselné přípony).

Je možné ji nakonfigurovat vlastního názvu domény pro přístup k datům ve vašem účtu Azure úložiště objektů blob.
Je výchozí koncový bod pro službu objektů Blob `https://mystorage.blob.core.windows.net`.

Ale pokud mapovat vlastní doménu (třeba www.contoso.com) na koncový bod objektů blob účtu úložiště, taky zpřístupníte dat ve vašem účtu úložiště objektů blob pomocí této domény. S vlastním názvem domény, například `http://mystorage.blob.core.windows.net/mycontainer/myblob` mohou mít přístup jako `http://www.contoso.com/mycontainer/myblob`.

Další informace o konfiguraci této funkce najdete v příručce [konfigurovat vlastní název domény pro váš koncový bod úložiště objektů Blob](../storage/storage-custom-domain-name.md).

Další informace o vytváření názvů objektů BLOB, kontejnerů a tabulky:

- [Pojmenování a odkazování na kontejnery, objekty BLOB a Metadata](https://msdn.microsoft.com/library/dd135715.aspx)
- [Pojmenování fronty a Metadata](https://msdn.microsoft.com/library/dd179349.aspx)
- [Pojmenování tabulky](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Název objektů blob může obsahovat libovolná kombinace znaků, ale rezervovaná znaky adresy URL, je nutné správně uvést. Vyhněte se názvy objektů blob končit tečkou (.), lomítko (/), nebo posloupnost nebo kombinací obou přístupů. Lomítko podle názvů, je **virtuální** oddělovač adresář. Nepoužívejte zpětné lomítko (\) v názvech objektů blob. Klient rozhraní API může ho povolit, ale pak se nepodařilo správně hash a podpisy nebude odpovídat.

Není možné změnit název účtu úložiště nebo kontejneru po jeho vytvoření.
Pokud chcete použít nový název, musíte ji odstranit a vytvořte nový účet.

> [AZURE.TIP] Doporučujeme vytvořit zásady vytváření názvů pro všechny účty úložiště a typy před věnovat vývoj nového služby nebo aplikace.

## <a name="example---deploying-an-n-tier-service"></a>Příklad: nasazení služby n osy

V tomto příkladu jsme určují konfigurace služby služba n vrstvy tvořené front-end IIS servery (hostované v systému Windows Server VMs), se serverem SQL Server (použitý ve dvou VMs serveru Windows), clusteru služby ElasticSearch (hostované v 6 Linux VMs) a účty přidružené úložiště, virtuálních sítí zdroje seskupit a služba Vyrovnávání zatížení.

Začneme bude tím, že definujete kontextové konvence pro tuto aplikaci:

| Entita | Systém názvů | Popis  |
| ------ | ---------- | ------------ |  
| Název služby | `profx` | Krátký název aplikace nebo služby nasazení |
| Prostředí | `prod` | Toto se týká provozní nasazení (na rozdíl od q & a, test atd.) |

Z tohoto podle směrného plánu jsme pak můžete přiřadit, konvence pro všechny typy zdrojů:

| Pole Typ zdroje | Base názvů | Příklad |
| ------------- | --------------- | ------- |
| Předplatné | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Pole Skupina zdroje | `servicename-rg` | `profx-rg` |
| Virtuální sítě | `servicename-vnet` | `profx-vnet` |
| Podsítě | `role-subnet` | `sql-vnet` |
| Vyrovnávání zatížení | `servicename-lb` | `profx-lb` |
| Virtuální počítač | `servicename-role[number]` | `profx-sql0` |
| Úložiště účtu | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Jak je vidět v následujícím diagramu:

![diagram topologie aplikací] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Topologie aplikací ukázka")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Ukázka – Azure rozhraní příkazového řádku skript pro nasazení výše uvedené ukázkové

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
