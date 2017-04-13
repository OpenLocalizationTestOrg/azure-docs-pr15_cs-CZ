<properties
    pageTitle="Azure příkazy rozhraní příkazového řádku v režimu služba Správa | Microsoft Azure"
    description="Příkazy Azure rozhraní příkazového řádku (rozhraní příkazového řádku) v režimu Správa služby Správa nasazení v modelu klasické nasazení"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure příkazy rozhraní příkazového řádku v režimu Správa služby Azure (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Můžete taky, [Přečtěte si o všechny příkazy modelu správce prostředků](virtual-machines/azure-cli-arm-commands.md)a použijte rozhraní příkazového řádku migrace [zdrojů](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) z klasického do modelu správce prostředků.

Tento článek poskytuje syntaxi a možnosti pro rozhraní příkazového řádku Azure příkazy by běžně používané k vytváření a správě Azure zdrojů v modelu klasické nasazení. Máte přístup k tyto příkazy spuštěním rozhraní příkazového řádku v režimu Správa služby Azure (asm). Toto není úplný odkaz a rozhraní příkazového řádku verze může zobrazit mírně odlišnou příkazů a parametrů. 

Jak začít, nejprve [nainstalovat Azure rozhraní příkazového řádku](xplat-cli-install.md) a [připojte k předplatnému Azure](xplat-cli-connect.md).

Aktuální syntaxe příkazu a možnosti na příkazovém řádku zadejte `azure help` nebo nápovědu k příkazu konkrétní `azure help [command]`. Taky najdete příklady rozhraní příkazového řádku v dokumentaci pro vytváření a správě určité služby Azure.

Volitelné parametry jsou zobrazeny v hranatých závorkách (například `[parameter]`). Všechny ostatní parametry jsou potřeba.

Kromě volitelné parametry specifické příkaz zde uvedeny jsou tři volitelné parametry, které lze použít k zobrazení podrobných výstupu například možnosti žádost a stavů. `-v` Parametr obsahuje podrobný výstup a `-vv` parametr poskytuje i podrobnější podrobný výstup. `--json` Možnost výstupy výsledek ve formátu nezpracovanými json.

## <a name="setting-asm-mode"></a>Nastavení asm režimu

Pomocí následujícího příkazu povolit příkazy v režimu Správa služby Azure rozhraní příkazového řádku.

    azure config mode asm

>[AZURE.NOTE] Správce prostředků Azure režimem a Správa služby Azure rozhraní příkazového řádku se vzájemně vylučují. Prostředků vytvořené v jednom režimu, nemůžete se ovládat z v režimu.

## <a name="manage-your-account-information-and-publish-settings"></a>Správa informací o účtu a nastavení publikování
Pomocí informací služby Azure předplatné je jedním ze způsobů, ke kterým rozhraní příkazového řádku můžete připojit ke svému účtu. (Najdete v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku](xplat-cli-connect.md) pro další možnosti.) Tyto informace můžete získat z portálu Azure klasické v souboru nastavení publikovat podle níže uvedeného popisu. Soubor nastavení publikování můžete importovat jako trvalý místní nastavení konfigurace, které rozhraní příkazového řádku pro další operace. Potřebujete importovat vaše nastavení publikování jednou.

**stažení účtu možnosti]**

Tohoto příkazu otevře prohlížeč .publishsettings soubor stáhnout z portálu Microsoft Azure klasické.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**účet importovat [možnosti] &lt;soubor >**


Příkaz importuje publishsettings souboru nebo certifikát tak, aby ji může používat nástroj v budoucích relace.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Soubor publishsettings může obsahovat (to znamená název předplatného a ID) podrobnosti více než jedno předplatné. Při importu souboru publishsettings prvního předplatného se používá jako výchozí popis. Pokud chcete použít jiné předplatné, spusťte tento příkaz:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**účet zrušte zaškrtnutí možnosti]**

Příkaz odebere uložené publishsettings, které jste importovali. Pomocí tohoto příkazu po ukončení pomocí nástroje na tomto počítači a chcete zajistit, že nástroj nelze použít s účtem v budoucích relace.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**seznam účtů možnosti]**

Seznam importovaných předplatná

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**účet nastavit [možnosti] &lt;předplatného&gt;**

Nastavení aktuálního předplatného

###<a name="commands-to-manage-your-affinity-groups"></a>Příkazy pro správu skupin spřažení

**seznam skupin spřažení účtu možnosti]**

Vypíše Azure spřažení skupiny.

Spřažení skupiny můžete nastavit, pokud skupiny virtuálních počítačích přesahuje více fyzických počítačů. Skupina spřažení omezuje fyzických počítačů by měl zavřít vzájemně největšímu zmenšit latence sítě.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**Vytvoření účtu spřažení skupiny [možnosti] &lt;název&gt;**

Tento příkaz vytvoří skupinu spřažení

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**Skupina spřažení účtů zobrazit [možnosti] &lt;název&gt;**

Tento příkaz zobrazí podrobnosti o skupině spřažení

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**účet spřažení skupinu odstranit [možnosti] &lt;název&gt;**

Příkaz odstraní zadané spřažení skupiny

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Příkazy pro správu prostředí účtu

**Přehled Obálka účtů možnosti]**

Seznam prostředí účtu

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**Obálka účet zobrazit možnosti [] [prostředí]**

Zobrazit podrobnosti účtu prostředí

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**Obálka účet přidat [možnosti] [prostředí]**

Příkaz přidá prostředí k tomuto účtu

**Obálka účtu nastavení [] [prostředí]**

Příkaz nastaví prostředí účtu

**účet Obálka odstranit [možnosti] [prostředí]**

Příkaz odstraní zadaný prostředí z účtu

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Příkazy pro správu klasické virtuálních počítačích
Následující diagram ukazuje, jak klasické Azure virtuálních počítačích hostovaný v provozním prostředí nasazení služby Azure cloudu.

![Azure technické diagramu](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Vytvoření nové** vytvoří jednotku v úložišti objektů blob (to znamená. e:\ v diagramu); **Připojit** připojí již vytvořeného, ale nepřipojené disk do virtuálního počítače.

**OM vytvořit [možnosti] &lt;název dns > &lt;obrázek > &lt;uživatelské jméno > [heslo]**

Příkaz vytvoří Azure virtuálního počítače. Ve výchozím nastavení vytvoří se ve vlastním cloudové služby virtuální počítače (OM). Můžete určit, že virtuální počítač má být přidán existující cloudové služby, pomocí možnosti - c, jak je uvedeno v tomto poli.

OM vytvořit příkaz, jako je portál Azure klasické, pouze vytvoří virtuálních počítačích v provozním prostředí nasazení. Pokud chcete vytvořit virtuální počítač v testovacím prostředí nasazení do cloudové služby i žádným způsobem. Pokud vaše předplatné nemá existující účet Azure úložiště, na příkaz vytvoří.

Zadejte umístění přes – umístění parametr, nebo můžete zadat skupinu spřažení přes parametr – spřažení skupiny. Pokud ani za předpokladu, že se zobrazí výzva k zadání jednu ze seznamu platná umístění.

Zadané heslo musí být 8 123 znaků a požadavkům složitost hesla operačního systému, který používáte pro tento virtuální počítač.

Pokud plánujete používat SSH ke správě nasazeném virtuálního počítače Linux (stejně jako obvykle v případě), je nutné povolit SSH prostřednictvím -e možnost při vytváření virtuálních počítače. Není možné povolit SSH po vytvoření virtuálního počítače.

Virtuálních počítačích Windows můžete povolit RDP později přidáním port 3389 jako koncový bod.

Následující volitelné parametry jsou podporované pro tento příkaz:

**- c, – připojení** vytvořit virtuální počítač uvnitř nasazení již vytvořené ve službě hostingu. Pokud vyberete tuto možnost není použit - vmname, název nového virtuálního zařízení je vytvořen automaticky.<br />
**n, název – OM** Zadejte název virtuální počítač. Tento parametr má název hostitelské služby ve výchozím nastavení. Pokud není zadán - vmname, název pro nový virtuální počítač je vytvořen jako &lt;název služby >&lt;id >, kde &lt;id > označuje počet existující virtuálních počítačích službu a 1. Například pokud tímto příkazem přidat virtuální počítač hostitelské službě Moje_služba obsahující jeden existující virtuálního počítače, nový virtuální počítač se jmenuje MyService2.<br />
**-u, – objektů blob adresy url** Zadejte URL úložiště objektů blob cílové pro niž chcete vytvořit virtuální počítač disku. <br />
**ž písma, velikosti – OM** Určete velikost virtuální počítač. Platné hodnoty jsou: "ExtraSmall", "Small", "Střední", "Velké", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Výchozí hodnota je "Malé". <br />
**-r** Přidá RDP připojení do virtuálního počítače v systému Windows. <br />
**-e-ssh** Přidá SSH připojení do virtuálního počítače v systému Windows. <br />
**-t, – ssh certifikátu.** Určuje SSH certifikát. <br />
**-s** Předplatné <br />
**-o, – komunity** Zadaný obrázek je obrázek komunity <br />
**-w** Virtuální síťový název <br/>
**-l, – umístění** Určuje umístění (například "severní centrální NAS"). <br />
**-, – spřažení skupiny** Určuje spřažení skupinu.<br />
**-w, – virtuální síťový název** Určení virtuální sítě, ke kterému chcete přidat nový virtuální počítač. Virtuální sítě můžete nastavit a spravovat z portálu Microsoft Azure klasické.<br />
**-b, – názvy podsítí** Určuje název podsítě přiřadit virtuální počítač.

V tomto příkladu je MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB obrázek poskytovanou platformu. Další informace o operačním systému obrázky přehled OM obrázek.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**OM vytvořit z &lt;název dns > &lt;role soubor >**

Příkaz vytvoří Azure virtuální počítač ze souboru role JSON.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**seznam OM možnosti]**

Tento příkaz zobrazí Azure virtuálních počítačích. Možnost – json Určuje, zda budou vráceny v původním formátu JSON.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**v seznamu umístění OM možnosti]**

Vypíše všechny dostupné účet Azure umístění.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**OM zobrazit [možnosti] &lt;název >**

Tento příkaz zobrazí podrobnosti o Azure virtuálního počítače. Možnost – json Určuje, zda budou vráceny v původním formátu JSON.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**OM odstranit [možnosti] &lt;název >**

Příkaz odstraní Azure virtuálního počítače. Ve výchozím nastavení se neodstraní tento příkaz objektů blob Azure, ze kterého se vytvářejí disk operačního systému a místo na disku data. Chcete-li odstranit objektů blob a virtuálního počítače, na které je založeno, zadejte -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**[možnosti] spuštění OM &lt;název >**

Tento příkaz spustí Azure virtuálního počítače.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**[možnosti] restartování OM &lt;název >**

Tento příkaz restartuje Azure virtuálního počítače.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**vypnutí OM [možnosti] &lt;název >**

Tento příkaz vypne Azure virtuálního počítače. Pomocí možnosti -p může zadat pro využití prostředků nejsou vydána vypnutí.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**zachycení OM &lt;OM název > &lt;název cílové obrázek >**

Tento příkaz zaznamená obrázek Azure virtuálního počítače.

Obrázek virtuálního počítače lze zachytit pouze pokud stav virtuálního počítače je **Zastaveno**. Vypnutí virtuální počítač před pokračováním.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**export OM [možnosti] &lt;OM název > &lt;cesta k souboru >**

Tento příkaz: Exportuje obrázek Azure virtuálního počítače do souboru

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Příkazy pro správu koncové body Azure virtuálního počítače
Na následujícím obrázku vidíte architektura typické nasazení několika instancích klasické virtuálního počítače. V tomto příkladu port 3389 je otevřen v počítači každého virtuální (přístup pomocí protokolu RDP). Je taky interní IP adresu (například 168.55.11.1) v jednotlivých virtuální počítač, který používá Vyrovnávání zatížení provoz směrovat na virtuální počítač. Tento interní IP adresu lze také komunikace mezi virtuálních počítačích.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Externí požadavky na virtuálních počítačích absolvovat Vyrovnávání zatížení. Z toho důvodu nelze zadat žádosti o týkající se určitého virtuálního počítače na nasazení s více virtuálních počítačích. Nasazení s více virtuálních počítačích musí se konfigurovat mapování portů mezi virtuálních počítačích (OM port) a vyrovnávání zatížení (lb port).

**Vytvoření koncového bodu OM &lt;OM název > &lt;lb port > [OM port]**

Příkaz vytvoří koncový bod virtuálního počítače. Taky můžete -u nebo--povolení přímé serveru zpět můžete určit, zda chcete-li povolit přímé serveru vraťte se na tento koncový bod, ve výchozím nastavení zakázaná.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**OM koncový bod vytvořit více [možnosti] &lt;OM název > &lt;lb port > [:&lt;OM port > [:&lt;protokol > [:&lt;povolit přímé serveru zpět > [:&lt;lb název sady > [:&lt;zkušební protokol > [:&lt;zkušební port > [:&lt;zkušební cesta > [:&lt;interní lb název >]]] {1 -*}**

Vytvoření více OM koncové body.

**Odstranění koncového bodu OM [možnosti] &lt;OM název > &lt;název koncového bodu >**

Příkaz odstraní strojového virtuální koncového bodu.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**seznam koncový bod OM &lt;OM název >**

Vypíše všechny koncové body virtuálního počítače. Možnost – json Určuje, zda budou vráceny v původním formátu JSON.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**aktualizace koncový bod OM [možnosti] &lt;OM název > &lt;název koncového bodu >**

Tento příkaz aktualizuje OM koncového bodu na nové hodnoty pomocí těchto možností.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**koncový bod OM zobrazit [možnosti] &lt;OM název >**

Tento příkaz zobrazí podrobnosti o koncové body na virtuálního počítače

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Příkazy pro správu obrázků Azure virtuálního počítače

Virtuální počítač obrázky je zachycený obsah už nakonfigurované virtuálních počítačích, které můžou být replikovat podle potřeby.

**seznam obrázků OM možnosti]**

Tento příkaz obdrží seznam virtuálního počítače obrázky. Existují tři typy obrázků: vytvořil Microsoft, obrázky, které jsou s předponou "MSFT", obrázky vytvořil třetí strany, které jsou s předponou název dodavatele a obrázky můžete vytvořit. Vytvoření obrázky, můžete zachytit existující virtuálního počítače nebo vytvořit obrázek z vlastní VHD nahráli k úložišti objektů blob. Další informace o používání vlastní VHD najdete v článku Vytvoření OM obrázek.
Možnost – json Určuje, zda budou vráceny v původním formátu JSON.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**Zobrazit obrázek OM [možnosti] &lt;název >**

Tento příkaz zobrazí podrobnosti o obrázku virtuálního počítače.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**Odstranit obrázek OM [možnosti] &lt;název >**

Příkaz odstraní obrázku virtuálního počítače.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**Vytvořit obrázek OM &lt;název > [zdroj]**

Příkaz vytvoří obrázku virtuálního počítače. Vlastní VHD souborů odeslány do úložiště objektů blob a vytvoří obrázek virtuálního počítače odtud. Potom použijete tento obrázek virtuálního počítače k vytvoření virtuálního počítače. Parametry umístění a OS jsou vyžadovány.

>[AZURE.NOTE]V současnosti tento příkaz podporuje odesílání souborů pevné VHD. Nahrajte soubor dynamické VHD, použijte [virtuální pevný disk Azure nástroje pro přechod](https://github.com/Microsoft/azure-vhd-utils-for-go).

Některé systémy stanovení limity popisovače souborů na obrázku. Překročení omezení nástroj zobrazí chyba limitu popisovače souborů. Spuštění příkazu znovu pomocí -p &lt;číslo > parametr omezit maximální počet paralelní nahrávání. Výchozí maximální počet dat parallel je 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Příkazy pro správu dat disků Azure virtuálního počítače

Disků dat jsou VHD soubory v úložišti objektů blob něhož tak, že virtuálního počítače. Další informace o tom, jsou nasazeny disků datový úložiště objektů blob najdete v článku Azure technické diagram uvedené výše.

Příkazy pro připojení disků dat (připojit azure OM disku a azure OM disku připojit nová) přiřadit disku připojená data podle protokolu SCSI číslo logické logické jednotky (jednotky). První disku dat připojeného k počítači virtuální přiřazen logické jednotky 0 je dalšího přiřazené 1 logické jednotky a tak dál.

Po odpojení disk dat s diskem azure OM odpojení příkaz, použít &lt;logické jednotky&gt; parametrů určujících disku, který má odpojit.

>[AZURE.NOTE] Měli vždy odpojíte disků dat v obráceném pořadí od logickou jednotku nejvyšší sudým, který byl přiřazen. Vrstvy Linux SCSI nepodporuje odpojení logické jednotky nižšími čísly během logické jednotky vyšší sudým pořád připojení. Třeba byste neměli odpojíte logické jednotky 0-li logické jednotky 1 je pořád.

**OM disku zobrazit [možnosti] &lt;název >**

Tento příkaz zobrazí podrobnosti o Azure disku.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**seznam disků OM [možnosti] [OM název]**

Tento příkaz seznamy Azure disků nebo disků připojené k zadané virtuálního počítače. Pokud je spuštěn s parametrem název virtuálního počítače, vrátí všech discích připojené k virtuální počítač. Logické jednotky 1 se vytvoří pomocí virtuálního počítače a jiných uvedených discích připojeni samostatně.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Provedením tohoto příkazu bez název parametru virtuálního počítače vrátí všech discích.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**Odstranit disku OM [možnosti] &lt;název >**

Příkaz odstraní Azure disku z osobní úložiště. Disk musí odpojí od virtuální počítač, než se odstraní.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**Vytvoření OM disku &lt;název > [zdroj]**

Tento příkaz uloží na server a zaregistruje Azure disku. – objektů blob adresy url, – umístění, nebo – je nutné zadat spřažení skupinu. Pokud tento příkaz s [zdroj] nahrát soubor VHD určený a vytvoří obrázek. Tento obrázek můžete pak připojíte k virtuálního počítače pomocí OM disku připojit.

Některé systémy stanovení limity popisovače souborů na obrázku. Překročení omezení nástroj zobrazí chyba limitu popisovače souborů. Spuštění příkazu znovu pomocí -p &lt;číslo > parametr omezit maximální počet paralelní nahrávání. Výchozí maximální počet dat parallel je 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**nahrání disku OM [možnosti] &lt;zdrojová cesta > &lt;objektů blob url > &lt;klíč účtu úložiště >**

Tento příkaz umožňuje nahrání disk OM

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**připojit OM disku &lt;OM název > &lt;název disku obrázek >**

Tento příkaz připojí existující disk v úložišti objektů blob k existující virtuální počítač nasazenou v do cloudové služby.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**OM disku připojit nové &lt;OM název > &lt;velikost v gb > [objektů blob adresa url]**

Tento příkaz připojí disk dat Azure virtuálního počítače. V tomto příkladu je 20 velikost nový disk v gigabajty připojí. Volitelně můžete objektů blob adresy URL jako argument poslední explicitně zadat objektů blob cílové vytvořit. Pokud nezadáte objektů blob adresy URL, objektů blob automaticky vygenerují.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**odpojení OM disku &lt;OM název > &lt;logické jednotky >**

Tento příkaz odpojí disk dat připojených k Azure virtuálního počítače. &lt;logické jednotky > identifikuje disku, který má být oddělit. Seznam disků spojené s diskem předtím, než je odebrat, použijte seznam disků OM &lt;OM název >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Příkazy pro správu služby Azure cloud services

Služby Azure cloud services jsou aplikací a služeb hostovaných na web rolí a kolegy. Správa služby Azure cloud services můžete použít následující příkazy.

**vytvoření služby [možnosti] &lt;název_služby >**

Tento příkaz vytvoří do cloudové služby

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**Služba zobrazit [možnosti] &lt;název_služby >**

Tento příkaz zobrazí podrobnosti o Azure cloudové služby

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**seznam služeb možnosti]**

Tento příkaz seznamy služby Azure cloud services.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**Služba odstranit [možnosti] &lt;název >**

Příkaz odstraní Azure cloudové služby.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Vynucení odstranění, zadejte `-q` parametr.


## <a name="commands-to-manage-your-azure-certificates"></a>Příkazy pro správu Azure certifikátů

Služby Azure certifikáty jsou certifikáty SSL spojený s vaším účtem Azure. Další informace o Azure certifikáty najdete v článku [Správa certifikátů](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**seznam služeb certifikátu možnosti]**

Tento příkaz zobrazí Azure certifikáty.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Vytvoření certifikátu služby &lt;předponu dns > &lt;soubor > [heslo]**

Tento příkaz odešle certifikát. Nezadáte zadání hesla pro certifikáty, které jsou chráněné heslem.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Služba certifikátu odstranit [možnosti] &lt;Miniatura >**

Příkaz odstraní certifikát.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Příkazy pro správu webové aplikace

Konfigurace webové přístupných osobám s postižením tak, že URI je Azure webovou aplikaci. Jsou webové aplikace použitý ve virtuálních počítačích, ale nemusíte myslete podrobnosti o vytváření a nasazení virtuálního počítače. Tyto informace jsou pro vás uskutečněných jednotlivými Azure.

**seznam webů možnosti]**

Vypíše webové aplikace.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**Web nastavení [] [jméno]**

Příkaz nastaví možnosti konfigurace pro webovou aplikaci [webu název]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**Web deploymentscript možnosti]**

Tento příkaz generuje skript vlastního nasazení

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**Vytvoření webu [možnosti] [jméno]**

Příkaz vytvoří web app a místního adresáře.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Název webu musí být jedinečná. Na webu nemůže vytvořit se stejným názvem jako existující web DNS.

**procházení webu [možnosti] [jméno]**

Tento příkaz otevře webovou aplikaci v prohlížeči.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Zobrazit webu [možnosti] [jméno]**

Tento příkaz zobrazí podrobnosti pro web app.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**odstranění webu [možnosti] [jméno]**

Příkaz odstraní do webových aplikací.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **odkládací webu [možnosti] [jméno]**

Tento příkaz Zamění dva sloty web app.

Tento příkaz podporuje následující další možnosti:

**- q nebo **– tichý **: výzvu k potvrzení. Tuto možnost použijte v automatické skripty.


**zahájení webu [možnosti] [jméno]**

Tento příkaz spustí do webových aplikací.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**Zastavit webu [možnosti] [jméno]**

Tento příkaz zastaví do webových aplikací.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**restartování webu [možnosti] [jméno]**

Tento příkaz zastaví a potom začne zadaný web appu.

Tento příkaz podporuje následující další možnosti:

**– úsek** &lt;úsek >: název úsek restartovat.


**ze seznamu webů možnosti]**

Tento příkaz zobrazí webová umístění aplikace.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Příkazy pro správu nastavení webové aplikace aplikace

**seznam webů appsetting [možnosti] [jméno]**

Vypíše nastavení aplikace přidané do web appu.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**appsetting webu přidat [možnosti] &lt;keyvaluepair > [jméno]**

Příkaz Nastavení aplikace přidá do webové aplikace jako pár hodnoty klíče.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**odstranění webu appsetting [možnosti] &lt;klíč > [jméno]**

Příkaz odstraní nastavení zadané aplikace z web appu.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**Web appsetting zobrazit [možnosti] &lt;klíč > [jméno]**

Tento příkaz zobrazí podrobnosti o nastavení zadané aplikace

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Příkazy pro správu certifikátu web app

**seznam webů certifikátu [možnosti] [jméno]**

Tento příkaz zobrazí seznam certifikáty web app.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**certifikát webu přidat [možnosti] &lt;cestě certifikátu > [jméno]**

**Odstranění certifikátu webu [možnosti] &lt;Miniatura > [jméno]**

**Web certifikát zobrazit [možnosti] &lt;Miniatura > [jméno]**

Tento příkaz zobrazí podrobnosti o certifikátu

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Příkazy pro správu váš web app připojovací řetězec

**seznam webů connectionstring [možnosti] [jméno]**

**Přidání webu connectionstring [možnosti] &lt;connectionname > &lt;hodnotu > &lt;typ > [jméno]**

**odstranění webu connectionstring [možnosti] &lt;connectionname > [jméno]**

**Web connectionstring zobrazit [možnosti] &lt;connectionname > [jméno]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Příkazy pro správu dokumentů výchozí web app

**seznam webů defaultdocument [možnosti] [jméno]**

**Přidání webu defaultdocument [možnosti] &lt;dokument > [jméno]**

**odstranění webu defaultdocument [možnosti] &lt;dokument > [jméno]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Příkazy pro správu nasazení vaší webové aplikace

**seznam webů nasazení [možnosti] [jméno]**

**nasazení serveru zobrazit [možnosti] &lt;commitId > [jméno]**

**opětovném webu nasazení nasazení [možnosti] &lt;commitId > [jméno]**

**Web nasazení github [možnosti] [jméno]**

**nasazení uživatele webu nastavení [] [uživatelské jméno] [průchodu]**

###<a name="commands-to-manage-your-web-app-domains"></a>Příkazy pro správu domén web app

**seznam domény webů [možnosti] [jméno]**

**domény webu přidat [možnosti] &lt;dn > [jméno]**

**odstranění domény webu [možnosti] &lt;dn > [jméno]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Příkazy pro správu mapování rutiny web app

**Obslužná rutina seznamu možnosti [] [jméno]**

**Obslužná rutina webu přidat [možnosti] &lt;rozšíření > &lt;procesor > [jméno]**

**odstranění webu rutiny [možnosti] &lt;rozšíření > [jméno]**

###<a name="commands-to-manage-your-web-jobs"></a>Příkazy pro správu webu úlohy

**seznam úloh webu [možnosti] [jméno]**

Tento příkaz seznam všech projektů webu v části web app.

Tento příkaz podporuje následující další možnosti:

+ **– Typ projektu** &lt;typ projektu >: nepovinné. Typ webjob. Platná hodnota je "aktivované" nebo "nepřetržitý". Ve výchozím nastavení vrátíte webjobs všechny typy.
+ **– úsek** &lt;úsek >: název úsek restartovat.

**Web projektu zobrazit [možnosti] &lt;název_úlohy > &lt;hodnota vlastnosti jobType > [jméno]**

Tento příkaz zobrazí podrobnosti o konkrétní web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– Typ projektu** &lt;typ projektu >: povinné. Typ webjob. Platná hodnota je "aktivované" nebo "nepřetržitý".
+ **– úsek** &lt;úsek >: název úsek restartovat.

**Odstranění úlohy webu [možnosti] &lt;název_úlohy > &lt;hodnota vlastnosti jobType > [jméno]**

Příkaz odstraní zadaný web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy > požadované. Název webjob.
+ **– Typ projektu** &lt;typ projektu > požadované. Typ webjob. Platná hodnota je "aktivované" nebo "nepřetržitý".
+ **-q** nebo **– Tichý**: výzvu k potvrzení. Tuto možnost použijte v automatické skripty.
+ **– úsek** &lt;úsek >: název úsek restartovat.

**Web projektu nahrát [možnosti] &lt;název_úlohy > &lt;hodnota vlastnosti jobType > <jobFile> [jméno]**

Příkaz odstraní zadaný web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– Typ projektu** &lt;typ projektu >: povinné. Typ webjob. Platná hodnota je "aktivované" nebo "nepřetržitý".
+ **– soubor úlohy** &lt;soubor úlohy >: povinné. Soubor projektu.
+ **– úsek** &lt;úsek >: název úsek restartovat.

**Web zahájení práce [možnosti] &lt;název_úlohy > &lt;hodnota vlastnosti jobType > [jméno]**

Tento příkaz spustí zadaný web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– Typ projektu** &lt;typ projektu >: povinné. Typ webjob. Platná hodnota je "aktivované" nebo "nepřetržitý".
+ **– úsek** &lt;úsek >: název úsek restartovat.

**ukončení práce webu [možnosti] &lt;název_úlohy > &lt;hodnota vlastnosti jobType > [jméno]**

Příkaz zastavení úlohy zadaný web. Pouze nepřetržitý úlohy, můžete ji ukončit.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– úsek** &lt;úsek >: název úsek restartovat.

###<a name="commands-to-manage-your-web-jobs-history"></a>Příkazy pro správu historie úlohy Web

**Seznam historie webu projektu [možnosti] [název_úlohy] [jméno]**

Tento příkaz zobrazí historii výskytů zadaný web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– úsek** &lt;úsek >: název úsek restartovat.

**Zobrazení historie úlohy webu [možnosti] [název_úlohy] [identifikátor runId] [jméno]**

Tento příkaz získá podrobnosti projektu spustit zadaný web projektu.

Tento příkaz podporuje následující další možnosti:

+ **– název úlohy** &lt;název úlohy >: povinné. Název webjob.
+ **– Spustit id** &lt;id spustit >: nepovinné. Id spuštění historie. Pokud není zadán, zobrazte nejnovější spustit.
+ **– úsek** &lt;úsek >: název úsek restartovat.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Příkazy pro správu diagnostických nástrojů vaší webové aplikace

**protokol serveru stahovat [možnosti] [jméno]**

Stáhněte si soubor ZIP, který obsahuje webovou aplikaci diagnostiky.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**Web protokolu zadní [možnosti] [jméno]**

Příkaz připojí terminálu k služba streamování protokolu.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**protokol serveru nastavení [] [jméno]**

Tento příkaz nakonfiguruje diagnostiky možnosti pro web app.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Příkazy pro správu webová aplikace úložiště

**webu úložiště větev [možnosti] &lt;větev > [jméno]**

**odstranění webu úložiště [možnosti] [jméno]**

**synchronizace úložiště webů [možnosti] [jméno]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Příkazy pro správu měřítka web app

**režim měřítko webů [možnosti] &lt;režimu > [jméno]**

**Web měřítko instance [možnosti] &lt;instance > [jméno]**


## <a name="commands-to-manage-azure-mobile-services"></a>Příkazy pro správu služby Azure Mobile

Služby Azure Mobile spojují sada Azure služeb, které povolují back-end možnosti pro vaše aplikace. Příkazy mobilní služby rozděleny následující kategorie:

+ [Příkazy pro správu instance mobilní služby](#Mobile_Services)
+ [Příkazy pro správu konfigurace mobilní služby](#Mobile_Configuration)
+ [Příkazy pro správu mobilních služeb tabulek](#Mobile_Tables)
+ [Příkazy pro správu mobilních služeb skriptů](#Mobile_Scripts)
+ [Příkazy pro správu naplánovaných úloh](#Mobile_Jobs)
+ [Příkazy Zobrazit mobilní služby](#Mobile_Scale)

Následující možnosti použít na většině příkazy mobilní služby:

+ **-h** nebo **– Nápověda**: zobrazení výstup informace o použití.
+ **– Ne `<id>` ** nebo **– předplatné `<id>` **: použití konkrétní předplatné, zadaný jako `<id>`.
+ **-v** nebo **– podrobného**: napište podrobný výstup.
+ **– json**: psaní JSON výstupu.

### <a name="Mobile_Services"></a>Příkazy pro správu instance mobile service

**mobilní umístění možnosti]**

Vypíše zeměpisné lokality podporovaných službami Mobile.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobilní vytvoření [možnosti] [název_služby] [sqlAdminUsername] [sqlAdminPassword]**

Příkaz vytvoří mobilní služba spolu databáze a SQL server.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Tento příkaz podporuje následující další možnosti:

+ **- r `<sqlServer>` ** nebo **SQL Server – `<sqlServer>` **: použití existující databáze SQL serveru, zadaný jako `<sqlServer>`.
+ **-d `<sqlDb>` ** nebo **– sqlDb `<sqlDb>` **: použít existující databázi SQL, zadaný jako `<sqlDb>`.
+ **-l `<location>` ** nebo **– umístění `<location>` **: vytvoření služby v určitém umístění, zadaný jako `<location>`. Spusťte azure mobilní umístění zobrazíte dostupná umístění.
+ **– sqlLocation `<location>` **: vytvoření SQL serveru v konkrétní `<location>`; Výchozí umístění mobilních služeb.

**mobilní odstranit [možnosti] [název_služby]**

Příkaz odstraní Mobilní služba spolu s jeho databáze a SQL server.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Tento příkaz podporuje následující další možnosti:

+ **d** nebo **– deleteData**: odstranění všech informací o této mobilní služby z databáze.
+ **–** nebo **– deleteAll**: odstranění databáze a SQL server.
+ **-q** nebo **– Tichý**: výzvu k potvrzení. Tuto možnost použijte v automatické skripty.

**mobilní seznamu možnosti]**

Tento příkaz zobrazí mobilních služeb.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**mobilní zobrazit možnosti [] [název_služby]**

Tento příkaz zobrazí podrobnosti o mobilních služeb.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**mobilní restart [možnosti] [název_služby]**

Tento příkaz restartuje instanci mobilních služeb.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**mobilní protokolu [možnosti] [název_služby]**

Tento příkaz vrátí Mobilní služba protokoly odfiltrování všechny typy protokol, ale `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Tento příkaz podporuje následující další možnosti:

+ **- r `<query>` ** nebo **– dotazu `<query>` **: spustí dotaz zadané.
+ **-t `<type>` ** nebo **– typ `<type>` **: filtrování vrácené protokoly položkou `<type>`, což může být `information`, `warning`, nebo `error`.
+ **-k `<skip>` ** nebo **– Přeskočit `<skip>` **: vynechá počet řádků určený `<skip>`.
+ **-p `<top>` ** nebo **– horní `<top>` **: vrátí určitý počet řádků určený `<top>`.

> [AZURE.NOTE] **– Dotazu** parametr přednost před **– Typ**, **– Přeskočit**, a **– horní**.

**mobilní obnovit možnosti [] [unhealthyservicename] [healthyservicename]**

Tento příkaz obnoví chybná Mobilní služba přesunutím správný mobilní služby v jiné oblasti.

Tento příkaz podporuje následující další možnosti:

**-q** nebo **– Tichý**: potlačit vyzývat k potvrzení obnovení.

**mobilní klíč obnovit možnosti [] [název_služby] [typ]**

Příkaz znovu vytvoří klávesu application mobilních služeb.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Klíčové typy jsou `master` a `application`.

> [AZURE.NOTE] Při obnovení klíče může být klienti, které pomocí klávesy staré nelze získat přístup k služba mobile. Při obnovení klávesu application, měli byste aktualizovat aplikace s nové hodnoty klíče.

**mobilní klíč nastavení [] [název_služby] [typ] [hodnota]**

Příkaz nastaví klíč mobilní služby na určitou hodnotu.


### <a name="Mobile_Configuration"></a>Příkazy pro správu konfigurace mobilní služby

**seznam mobilní konfigurace [možnosti] [název_služby]**

Tento příkaz zobrazí seznam možností konfigurace mobilních služeb.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**konfigurace mobilního zobrazení [možností] [název_služby] [klíč]**

Tento příkaz získá možnost konkrétní konfigurace pro mobilní služba v tomto případě dynamické schéma.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**mobilní konfigurace nastavení [] [název_služby] [klíč] [hodnota]**

Příkaz nastaví možnost konkrétní konfigurace pro mobilní služba v tomto případě dynamické schéma.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Příkazy pro správu mobilních služeb tabulek

**seznam mobilní tabulky [možnosti] [název_služby]**

Tento příkaz zobrazí seznam všech tabulek v mobilních služeb.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**mobilní v této tabulce zobrazit možnosti [] [název_služby] [název tabulky]**

Tento příkaz zobrazí vrátí podrobností o konkrétní tabulky.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**mobilní tabulka vytvořit [možnosti] [název_služby] [název tabulky]**

Tento příkaz vytvoří tabulku.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Tento příkaz podporuje následující další možnosti:

+ **-p `&lt;permissions>` ** nebo **– oprávnění `&lt;permissions>` **: seznam oddělený středníkem `<operation>` = `<permission>` dvojice, kde `<operation>` je `insert`, `read`, `update`, nebo `delete` a `&lt;permissions>` je `public`, `application` (výchozí), `user`, nebo `admin`.

**mobilní dat přečíst [možnosti] [název_služby] [název tabulky] [query]**

Tento příkaz načte data z tabulky.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Tento příkaz podporuje následující další možnosti:

+ **-k `<skip>` ** nebo **– Přeskočit `<skip>` **: vynechá počet řádků určený `<skip>`.
+ **-t `<top>` ** nebo **– horní `<top>` **: vrátí určitý počet řádků určený `<top>`.
+ **-l** nebo **– seznam**: vrátí data ve formátu seznamu.

**aktualizace mobilní tabulky [možnosti] [název_služby] [název tabulky]**

Tento příkaz Odstranit oprávnění na tabulky změní na pouze pro správce.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Tento příkaz podporuje následující další možnosti:

+ **-p `&lt;permissions>` ** nebo **– oprávnění `&lt;permissions>` **: seznam oddělený středníkem `<operation>` = `<permission>` dvojice, kde `<operation>` je `insert`, `read`, `update`, nebo `delete` a `&lt;permissions>` je `public`, `application` (výchozí), `user`, nebo `admin`.
+ **– deleteColumn `<columns>` **: seznam oddělený středníkem sloupce, které chcete odstranit, jako `<columns>`.
+ **-q** nebo **– Tichý**: Odstraní sloupce bez zobrazení výzvy k potvrzení.
+ **– addIndex `<columns>` **: seznam oddělený středníkem sloupce, které chcete zahrnout do indexu.
+ **– deleteIndex `<columns>` **: seznam oddělený středníkem sloupce, které chcete vyloučit z indexu.

**mobilní tabulky odstranit [možnosti] [název_služby] [název tabulky]**

Příkaz odstraní tabulku.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Zadejte parametr - q pro odstranění tabulky bez potvrzení. Můžete zabránit tomu blokování automatizaci skriptů.

**mobilní data zkrátit [možnosti] [název_služby] [název tabulky]**

Tento příkaz: Odstraní všechny řádky dat z tabulky.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Příkazy pro správu skriptů

Příkazy v této části jsou sloužící ke správě skripty serveru, které patří k mobilní služba. Další informace najdete v tématu [práce s skripty serveru služby Mobile](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**seznam Mobile skriptů [možnosti] [název_služby]**

Tento příkaz zobrazí seznam registrovaných skripty, včetně tabulky a Plánovač skriptů.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**stahování mobilní skriptu [možnosti] [název_služby] [název_skriptu]**

Tento příkaz stáhne vložení skriptu z tabulky TodoItem do souboru nazvaného `todoitem.insert.js` v `table` podsložky.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Tento příkaz podporuje následující další možnosti:

+ **-p `<path>` ** nebo **– cesta `<path>` **: místo v souboru, ve kterém chcete uložit skript, kde aktuální pracovní adresář je výchozí hodnota.
+ **-f `<file>` ** nebo **– soubor `<file>` **: název souboru, ve kterém ho uložit do.
+ **-o** nebo **– Přepsat**: přepsat existující soubor.
+ **-c** nebo **– konzoly**: skript ke konzole ne do souboru.

**mobilní skript nahrát [možnosti] [název_služby] [název_skriptu]**

Tento příkaz odešle skript s názvem `todoitem.insert.js` z `table` podsložky.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Název souboru se skládají z názvy tabulek a operace. Musí být umístěné v podsložce tabulky relativní umístění, kde příkaz spustit. Můžete taky použít **-f `<file>` ** nebo **– soubor `<file>` ** parametr zadejte jiný název souboru a cestu k souboru, který obsahuje skript a zaregistrovat.


**mobilní skript odstranit [možnosti] [název_služby] [název_skriptu]**

Tento příkaz odebere z tabulky TodoItem existující skript vložit.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Příkazy pro správu naplánovaných úloh

Příkazy v této části jsou sloužící ke správě naplánovaných úloh, které patří k mobilní služba. Další informace najdete v tématu [plánování úloh](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**seznam mobilní úloh [možnosti] [název_služby]**

Tento příkaz zobrazí naplánovaných úloh.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**mobilní úlohy Vytvořit [možnosti] [název_služby] [název_úlohy]**

Tento příkaz vytvoří úlohu s názvem `getUpdates` , který je naplánováno spuštění každou hodinu.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Tento příkaz podporuje následující další možnosti:

+ **-i `<number>` ** nebo **– interval `<number>` **: interval úlohy jako celé číslo. Výchozí hodnota je `15`.
+ **-u `<unit>` ** nebo **– intervalUnit `<unit>` **: jednotku _intervalu_, který může být jeden z následujících hodnot:
    + **Funkce MINUTE** (výchozí)
    + **Hodina**
    + **den**
    + **měsíc**
    + **žádná** (na vyžádání úlohy)
+ **-t `<time>`** **– čas spuštění `<time>` ** Počáteční čas dané při prvním spuštění skriptu ve formátu ISO. Výchozí hodnota je `now`.

> [AZURE.NOTE] Nové úlohy vytvořené v zakázaném stavu protože skript musí pořád nahrát. Pomocí příkazu **Odeslat mobilní skript** Nahrát skript a příkaz **Aktualizovat mobilní úlohy** povolte úlohu.

**aktualizace mobilní úlohy [možnosti] [název_služby] [název_úlohy]**

Následující příkaz povolí zakázaného `getUpdates` projektu.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Tento příkaz podporuje následující další možnosti:

+ **-i `<number>` ** nebo **– interval `<number>` **: interval úlohy jako celé číslo. Výchozí hodnota je `15`.
+ **-u `<unit>` ** nebo **– intervalUnit `<unit>` **: jednotku _intervalu_, který může být jeden z následujících hodnot:
    + **Funkce MINUTE** (výchozí)
    + **Hodina**
    + **den**
    + **měsíc**
    + **žádná** (na vyžádání úlohy)
+ **-t `<time>`** **– čas spuštění `<time>` ** Počáteční čas dané při prvním spuštění skriptu ve formátu ISO. Výchozí hodnota je `now`.
+ **- `<status>` ** nebo **– Stav `<status>` **: stav projektu, který může být buď `enabled` nebo `disabled`.

**mobilní úlohy Odstranit [možnosti] [název_služby] [název_úlohy]**

Příkaz odebere getUpdates úlohy ze serveru seznam úkolů.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Odstranění úlohy odstraněny také odeslaný skript.

### <a name="Mobile_Scale"></a>Příkazy Zobrazit mobilní služby

Příkazy v této části slouží k měřítko mobilních služeb. Další informace najdete v tématu [měřítka mobilních služeb](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**mobilní měřítko zobrazit možnosti [] [název_služby]**

Tento příkaz zobrazí měřítko informace, včetně nastavený režim výpočetním a počet instancí.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**mobilní měřítko změnit možnosti [] [název_služby]**

Tento příkaz měnit měřítka službu mobilní bezplatná na premium režimu.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Tento příkaz podporuje následující další možnosti:

+ **- c `<mode>` ** nebo **– computeMode `<mode>` **: režim výpočetním musí být buď `Free` nebo `Reserved`.
+ **-i `<count>` ** nebo **– numberOfInstances `<count>` **: počet instancí při spuštění v režimu rezervovaná.

> [AZURE.NOTE] Když nastavíte výpočetním režimu na `Reserved`, všechny mobilních služeb ve stejné oblasti spustit v režimu premium.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Příkazy pro povolení funkcí verze preview pro Mobile Service

**mobilní Náhled seznamu možnosti [] [název_služby]**

Tento příkaz zobrazí náhled funkcí dostupných na zadané služby a jestli jsou povolené.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**mobilní náhled povolit možnosti [] [název_služby] [Název_funkce]**

Tento příkaz umožňuje funkci zadané náhledu pro služba mobile. Po povolení funkce se nedá zakázat mobilní služby.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Příkazy ke správě mobilních služeb rozhraní API

**mobilní rozhraní api seznamu možnosti [] [název_služby]**

Tento příkaz zobrazí seznam mobilní služby vlastní rozhraní API, kterou jste vytvořili mobilní službě.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**mobilní rozhraní api vytvořit [možnosti] [název_služby] [apiname]**

Vytvoří vlastní rozhraní API mobilní služby

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Tento příkaz podporuje následující další možnosti:

**-p** nebo **– oprávnění** &lt;oprávnění >: seznam oddělený středníkem &lt;metoda > =&lt;oprávnění > dvojice.

**mobilní rozhraní api aktualizace [možnosti] [název_služby] [apiname]**

Příkaz Aktualizovat rozhraní API pro vlastní zadaný služba mobile.

Tento příkaz podporuje následující další možnosti:

Tento příkaz podporuje následující další možnosti:

+ **-p** nebo **– oprávnění** &lt;oprávnění >: seznam oddělený středníkem &lt;metoda > =&lt;oprávnění > dvojice.
+ **-f** nebo **– vynucení**: potlačí všechny vlastní změny souboru metadat oprávnění.

**mobilní rozhraní api odstranit [možnosti] [název_služby] [apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Příkaz odstraní rozhraní API pro vlastní zadaný mobilních služeb.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Příkazy pro správu aplikace nastavení mobilní aplikace

**mobilní appsetting seznamu možnosti [] [název_služby]**

Tento příkaz zobrazí aplikace nastavení mobilní aplikace pro zadané služby.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**mobilní appsetting přidat [možnosti] [název_služby] [jméno] [hodnota]**

Příkaz přidá vlastní nastavení aplikace mobilní službě.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**mobilní appsetting odstranit [možnosti] [název_služby] [jméno]**

Příkaz odebere nastavení zadané aplikace mobilní službě.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**mobilní appsetting zobrazit možnosti [] [název_služby] [jméno]**

Příkaz odebere nastavení zadané aplikace mobilní službě.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Správa nástroj Místní nastavení

ID předplatného a výchozí název účtu úložiště je místní nastavení.

**seznam konfigurace možnosti]**

Tento příkaz zobrazí konfigurace nastavení.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**Konfigurace nastavení [možností] &lt;název&gt;,&lt;hodnota&gt;**

Tento příkaz ke změně nastavení konfigurace.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Příkazy pro správu služby Bus

Umožňuje spravovat svůj účet služby Bus tyto příkazy

**SB názvů zaškrtnutí [možnosti] &lt;název >**

Zkontrolujte, zda obor názvů bus služby právních a k dispozici.

**vytváření názvů SB &lt;název > &lt;umístění >**

Vytvoří obor Bus služby.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**odstranění názvů SB &lt;název >**

Odebrání obor.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**seznam názvů SB**

Seznam všechny obory názvů vytvořené pro váš účet.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**obor názvů SB ze seznamu**

Zobrazte seznam všech dostupných názvů umístění.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**Zobrazit názvů SB &lt;název >**

Zobrazení podrobností o konkrétní obor názvů.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**obor názvů SB ověření &lt;název >**

Zkontrolujte, jestli je k dispozici.

## <a name="commands-to-manage-your-storage-objects"></a>Příkazy pro správu úložiště objektů

###<a name="commands-to-manage-your-storage-accounts"></a>Příkazy pro správu účtů úložiště

**seznam účtů úložiště možnosti]**

Tento příkaz zobrazí účty úložiště vašeho předplatného.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**úložiště účet zobrazit možnosti]<name>**

Tento příkaz zobrazí informace o účtu zadaný úložiště včetně vlastnosti URI a účtu.

**Vytvoření účtu úložiště možnosti]<name>**

Tento příkaz vytvoří účet úložiště na základě zadaných možností.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Tento příkaz podporuje následující další možnosti:

+ **-e** nebo **– Popisek** &lt;popisek >: název účtu úložiště.
+ **-d** nebo **– Popis** &lt;popis >: účtu úložiště popis.
+ **-l** nebo **– umístění** &lt;název >: zeměpisná oblast ve kterém chcete vytvořit účet úložiště.
+ **-** nebo **– spřažení skupiny** &lt;název >: spřažení skupiny, ke které chcete přidružit účtu úložiště. 
+ **– Typ**: označuje typ účtu chcete vytvořit: buď standardní úložiště s redundance možnost (LRS/ZRS/GRS/RAGRS) nebo Premium úložiště (PLRS).

**nastavení účtu úložiště možnosti]<name>**

Příkaz Aktualizovat účtu zadaný úložiště.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Tento příkaz podporuje následující další možnosti:

+ **-e** nebo **– Popisek** &lt;popisek >: název účtu úložiště.
+ **-d** nebo **– Popis** &lt;popis >: účtu úložiště popis.
+ **-l** nebo **– umístění** &lt;název >: zeměpisná oblast ve kterém chcete vytvořit účet úložiště.
+ **– Typ**: označuje nový typ účtu: buď standardní úložiště s redundance možnost (LRS/ZRS/GRS/RAGRS) nebo Premium úložiště (PLRS).

**Odstranění účtu úložiště možnosti]<name>**

Příkaz odstraní účtu zadaný úložiště.

Tento příkaz podporuje následující další možnosti:

**-q** nebo **– Tichý**: výzvu k potvrzení. Tuto možnost použijte v automatické skripty.

###<a name="commands-to-manage-your-storage-account-keys"></a>Příkazy pro správu úložiště účtu klíče

**klávesy účtu úložiště seznam možnosti]<name>**

Tento příkaz zobrazí primárních a sekundárních klávesy pro účet zadaný úložiště.

**klávesy účtu úložiště obnovení možnosti]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Příkazy pro správu úložiště kontejneru

**seznam kontejneru úložiště [možnosti] [předponu]**

Tento příkaz zobrazí seznam container úložiště pro účet zadaný úložiště. Účet úložiště nastavil připojovací řetězec nebo klíč účtu a název účtu úložiště.

Tento příkaz podporuje následující další možnosti:

+ **-p** nebo **-předponu** &lt;předponu >: předponu jméno container úložiště.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v režimu ladění.

**úložiště container zobrazit možnosti [] [container]**
**úložiště container vytvořit [možnosti] [container]**

Příkaz vytvoří kontejneru úložiště pro účet zadaný úložiště. Účet úložiště nastavil připojovací řetězec nebo klíč účtu a název účtu úložiště.

Tento příkaz podporuje následující další možnosti:

+ **– kontejneru** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-p** nebo **-předponu** &lt;předponu >: předponou úložiště kontejneru název.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: připojovací řetězec úložiště
+ **– ladění**: spustí příkaz úložiště v režimu ladění.

**odstranit kontejner úložiště [možnosti] [kontejneru]**

Příkaz odstraní kontejneru zadaný úložiště. Účet úložiště nastavil připojovací řetězec nebo klíč účtu a název účtu úložiště.

Tento příkaz podporuje následující další možnosti:

+ **– kontejneru** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-p** nebo **-předponu** &lt;předponu >: předponu jméno container úložiště.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v režimu ladění.

**Kontejner úložiště nastavení [] [kontejneru]**

Příkaz nastaví seznam řízení přístupu pro kontejner úložiště. Účet úložiště nastavil připojovací řetězec nebo klíč účtu a název účtu úložiště.

Tento příkaz podporuje následující další možnosti:

+ **– container** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-p** nebo **-předponu** &lt;předponu >: předponou úložiště kontejneru název.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v režimu ladění.

###<a name="commands-to-manage-your-storage-blob"></a>Příkazy pro správu vašeho úložiště objektů blob

**úložiště objektů blob seznamu možnosti [] [kontejneru] [předponu]**

Tento příkaz vrátí seznam úložiště objektů BLOB v kontejneru zadaný úložiště.

Tento příkaz podporuje následující další možnosti:

+ **– container** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-p** nebo **-předponu** &lt;předponu >: předponou úložiště kontejneru název.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v režimu ladění.

**úložiště objektů blob zobrazit možnosti [] [kontejneru] [objektů blob]**

Tento příkaz zobrazí podrobnosti o zadaný úložiště objektů blob.

Tento příkaz podporuje následující další možnosti:

+ **– kontejneru** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-p** nebo **-předponu** &lt;předponu >: předponu jméno container úložiště.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v ladění.

**úložiště objektů blob odstranit [možnosti] [kontejneru] [objektů blob]**

Tento příkaz podporuje následující další možnosti:

+ **– container** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-b** nebo **– objektů blob** &lt;blobName >: název úložiště objektů blob odstranit.
+ **-q** nebo **– Tichý**: odebrání zadaný blob úložiště bez potvrzení.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v ladění.

**úložiště objektů blob nahrát [možnosti] [soubor] [kontejneru] [objektů blob]**

Tento příkaz zadaný soubor nahrát objektů blob specified\ úložiště.

Tento příkaz podporuje následující další možnosti:

+ **– container** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-b** nebo **– objektů blob** &lt;blobName >: název úložiště objektů blob nahrát.
+ **t –** nebo **– blobtype** &lt;blobtype >: typ úložiště objektů blob: stránky nebo bloku.
+ **-p** nebo **– Vlastnosti** &lt;vlastnosti >: vlastnosti úložiště objektů blob pro nahrané soubor. Vlastnosti jsou klíč = s pár hodnotu a oddělené pomocí semicolon(;). Typ obsahu, contentEncoding, contentLanguage a cacheControl jsou k dispozici vlastnosti.
+ **m –** nebo **– metadata** &lt;metadat >: úložiště objektů blob metadat pro nahrané soubor. Metadata mají klíčový = dvojice d oddělené středníkem (;).
+ **– concurrenttaskcount** &lt;concurrenttaskcount >: maximální počet požadavků souběžné nahrát.
+ **-q** nebo **– Tichý**: přepsat zadaný blob úložiště bez potvrzení.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v ladění.

**úložiště objektů blob stažení [možnosti] [kontejneru] [objektů blob] [cíl]**

Tenhle příkaz je možné stáhnout zadaný úložiště objektů blob.

Tento příkaz podporuje následující další možnosti:

+ **– container** &lt;kontejneru >: název kontejneru úložiště chcete vytvořit.
+ **-b** nebo **– objektů blob** &lt;blobName >: název úložiště objektů blob.
+ **-d** nebo **– cíl** [cíl]: cesta k stažení cílové souboru nebo složky.
+ **-m** nebo **– checkmd5**: md5sum zaškrtnutí pro stažený soubor.
+ **– concurrenttaskcount** &lt;concurrenttaskcount > maximální počet požadavků souběžné nahrát
+ **-q** nebo **– Tichý**: přepsat cílového souboru bez potvrzení.
+ **-** nebo **– název účtu** &lt;název účtu >: název účtu úložiště.
+ **-k** nebo **– klíč účtu** &lt;accountKey >: klíč účtu úložiště.
+ **-c** nebo **– připojovací řetězec** &lt;connectionString >: úložiště připojovací řetězec.
+ **– ladění**: spustí příkaz úložiště v ladění.

## <a name="commands-to-manage-sql-databases"></a>Příkazy pro správu databáze SQL

Použijte tyto příkazy pro správu databáze SQL Azure

###<a name="commands-to-manage-sql-servers"></a>Příkazy pro správu servery SQL.

Tyto příkazy používat ke správě serverech SQL

**Vytvoření serveru SQL server &lt;administratorLogin > &lt;administratorPassword > &lt;umístění >**

Vytvoření databázového serveru

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**SQL server zobrazit &lt;název >**

Zobrazit podrobnosti o server.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**Seznam serveru SQL**

Vytvořte požadovaný seznam servery.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL server odstranit &lt;název >**

Odstraní server

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Příkazy pro správu databáze SQL

Použijte tyto příkazy můžete spravovat databáze SQL.

**Vytvoření databáze SQL [možnosti] &lt;webu název serveru > &lt;název databáze > &lt;administratorPassword >**

Vytváří instanci databáze

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**databáze SQL zobrazit [možnosti] &lt;webu název serveru > &lt;název databáze > &lt;administratorPassword >**

Zobrazit podrobnosti o databázi.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**seznam databáze SQL [možnosti] &lt;webu název serveru > &lt;administratorPassword >**

Seznam databází.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**databáze SQL odstranit [možnosti] &lt;webu název serveru > &lt;název databáze > &lt;administratorPassword >**

Odstraní databáze.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Příkazy ke správě pravidel brána firewall systému SQL Server

Tyto příkazy používat ke správě pravidel brána firewall systému SQL Server

**SQL firewallrule vytvořit [možnosti] &lt;webu název serveru > &lt;Název_pravidla > &lt;startIPAddress > &lt;endIPAddress >**

Vytvořte pravidlo brány firewall pro systém SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL firewallrule zobrazit [možnosti] &lt;webu název serveru > &lt;Název_pravidla >**

Zobrazit podrobnosti pravidla bránu firewall.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**seznam firewallrule SQL [možnosti] &lt;webu název serveru >**

Seznam pravidla brány firewall.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL firewallrule odstranit [možnosti] &lt;webu název serveru > &lt;Název_pravidla >**

Příkaz odstraní pravidla brány firewall.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Příkazy pro správu virtuálních sítí

Používat tyto příkazy pro správu virtuální sítí

**síť vnet vytvořit [možnosti] &lt;umístění >**

Vytvořte virtuální sítě.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**sítě vnet zobrazit &lt;název >**

Zobrazte podrobnosti o virtuální sítě.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**seznam vnet sítě**

Seznam všech existujících virtuální sítí.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**odstranění sítě vnet &lt;název >**

Odstraní zadaný virtuální sítě.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**export sítě [– cesta k souboru]**

Konfigurace rozšířené sítě můžete exportovat místně konfiguraci sítě. Konfigurace exportovaného sítě zahrnuje nastavení serveru DNS, nastavení virtuální sítě, nastavení místní sítě webu a další nastavení.

**import sítě [– cesta k souboru]**

Importujte konfigurace místní síti.

**síť Server_DNS zaregistrovat [možnosti] &lt;dnsIP >**

Registrace serveru DNS, který chcete použít pro překlad v konfiguraci sítě.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**seznam Server_DNS sítě**

Seznam všech serverů DNS registraci v konfiguraci sítě.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**síť Server_DNS unregister [možnosti] &lt;dnsIP >**

Odebere konfiguraci sítě položky serveru DNS.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

