<properties
   pageTitle="Použití koncovku Azure Docker OM | Microsoft Azure"
   description="Naučte se používat rozšíření Docker OM rychle a bezpečné nasazení Docker prostředí v Azure pomocí šablon správce prostředků."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Vytvoření prostředí Docker v Azure pomocí koncovku Docker OM
Docker je Správa oblíbených kontejner a obrázků platformy, která umožňuje rychle pracovat s kontejnery na Linux (a Windows taky). V Azure, což jsou různé způsoby nasadíte Docker podle svých potřeb. Tento článek se zaměřuje na pomocí koncovku Docker OM a správce prostředků Azure šablon. 

Další informace o různých metodách nasazení, včetně použití Docker počítače a služby Azure kontejneru najdete v následujících článcích:

- Chcete rychle vzoru aplikace můžete vytvořit jednoho hostitele Docker použití [Docker zařízení](./virtual-machines-linux-docker-machine.md).
- Pro větší a další stabilní prostředí můžete použít rozšíření Azure Docker OM, které podporuje také [Docker vytvořte](https://docs.docker.com/compose/overview/) generovat konzistentní kontejneru nasazení. Tento článek podrobnosti, s příponou OM Docker Azure.
- Vytvářet řešení připravené na výrobní scalable prostředích poskytující další nástroje pro plánování a Správa nástroje můžete nasazovat [Docker Swarm clusteru v kontejneru služby Azure](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Přehled rozšíření Azure Docker OM
Rozšíření Azure Docker OM nainstaluje a nakonfiguruje Docker démon, Docker klienta a Docker vytvořte v počítači virtuální Linux (OM). Pomocí koncovku Azure Docker OM, máte další ovládací prvek a funkcí než jednoduše pomocí Docker počítače nebo vytváření hostiteli Docker sami. Následující funkce, například [Docker vytvořte](https://docs.docker.com/compose/overview/), zkontrolujte koncovku Azure Docker OM vyhovuje robustnější vývojář nebo provozním prostředí.

Azure správce prostředků šablony definovat celé struktury prostředí. Šablony umožňují vytváření a konfigurace zdroje, jako jsou hostiteli Docker VMs, úložiště, na základě rolí přístup k ovládacím prvkům (RBAC) a diagnostice. Můžete znovu použít tyto šablony vytvořit další nasazení konzistentní. Další informace o správce prostředků Azure a šablon najdete v tématu [Přehled Správce prostředků](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Nasazení šablony s příponou OM Docker Azure
Vytvoření systémem Ubuntu OM, který používá příponu Azure Docker OM instalace a konfigurace hostitele Docker použijeme existující šablony rychlý úvod. Můžete zobrazit šablonu tady: [jednoduché nasazení se systémem Ubuntu OM s Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Je třeba [Nejnovější rozhraní příkazového řádku Azure](../xplat-cli-install.md) nainstalovaný a přihlášení k lyncu v režimu správce prostředků následujícím způsobem:

```
azure config mode arm
```

Nasazení šablony pomocí Azure rozhraní příkazového řádku, zadáním šablony URI. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` umístění. Použijte vlastní název skupiny prostředků a umístění následujícím způsobem:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Přijetí pokynů název účtu úložiště, zadejte uživatelské jméno a heslo a zadejte název DNS. Výstup je podobný jako v následujícím příkladu:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Vrátí Azure rozhraní příkazového řádku do příkazového řádku po jen pár sekund, ale hostitele Docker stále vytvořili a nakonfigurovali tak, že přípona OM Docker Azure. Trvá několik minut pro nasazení na Dokončit. Můžete zobrazit údaje o používání stav hostitele Docker `azure vm show` příkaz.

Následující příklad zkontroluje stav OM s názvem `myDockerVM` (výchozí název ze šablony - neměňte tímto názvem) ve skupině zdroje s názvem `myResourceGroup`. Zadejte název, který jste vytvořili v předchozím kroku skupina zdroje:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Výstup `azure vm show` příkaz je podobný jako v následujícím příkladu:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

V horní části výstup, se zobrazí `ProvisioningState` z OM. Zobrazí se při `Succeeded`nasazení proběhla a můžete SSH bude v angličtině.

Ke konci výstup `FQDN` zobrazí plně kvalifikovaný název domény hostitele Docker. Tento plně kvalifikovaný název domény je co použijete k SSH na hostitele Docker zbývající kroky.


## <a name="deploy-your-first-nginx-container"></a>Nasazení prvním kontejneru nginx
Po zavedení dokončí, SSH na nové hostitele Docker z místního počítače. Zadejte uživatelské jméno a plně kvalifikovaný název domény následujícím způsobem:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Po přihlášení k hostiteli Docker Pojďme spusťte nginx kontejner:

```
sudo docker run -d -p 80:80 nginx
```

Výstup je podobný v následujícím příkladu nginx obrázek se stáhne a začít kontejner:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Kontrola stavu kontejnery spuštěna hostitele Docker následujícím způsobem:

```
sudo docker ps
```

Výstup podobně jako v následujícím příkladu je znázorňující, že kontejneru nginx je spuštěn a porty TCP 80 a 443 a předávané:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Kontejner v akci zobrazíte otevřete webový prohlížeč a zadejte úplný název hostitele Docker:

![Průběžný ngnix kontejneru](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker OM rozšíření šablony odkaz
Existující šablony rychlý úvod v předchozím příkladu. Můžete taky nasadíte koncovku OM Docker Azure pomocí šablony správce prostředků. Postup, přidejte do následující šablony správce prostředků definování `vmName` ze svého OM řádně podporovat:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Můžete najít podrobnější informace o použití šablon správce prostředků v tématu [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Další kroky
Můžete chtít, [nakonfigurujte funkce Docker TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/)nebo nasadit kontejnery pomocí [Docker vytvořte](https://docs.docker.com/compose/overview/). Další informace o prodloužení OM Docker Azure samotné najdete v článku [GitHub projektu](https://github.com/Azure/azure-docker-extension/).

Přečtěte si další informace o dalších možnostech nasazení Docker v Azure:

- [Použití Docker počítače s ovladači Azure](./virtual-machines-linux-docker-machine.md)  
- [Začínáme s Docker a vytváření definovat a spuštění aplikace více kontejneru na Azure virtuálního počítače](virtual-machines-linux-docker-compose-quickstart.md).
- [Nasazení služby Azure kontejneru obrázku](../container-service/container-service-deployment.md)
