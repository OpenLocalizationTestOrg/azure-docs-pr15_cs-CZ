<properties
    pageTitle="Vytvoření Docker tabulkami hosts v Azure s Docker počítači | Microsoft Azure"
    description="Popisuje použití počítače Docker vytvoření docker tabulkami hosts v Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Použití Docker počítače s ovladači Azure

[Docker](https://www.docker.com/) patří mezi nejoblíbenější přístupy virtualization, která využívají Linux kontejnery spíše než virtuálních počítačích tak izolace dat aplikace a výpočetních na sdíleného zdroje. Toto téma popisuje, kdy a jak tuto funkci používat [Docker počítače](https://docs.docker.com/machine/) ( `docker-machine` příkaz) k vytvoření nové VMs Linux v Azure povolené jako hostitel docker kontejnerů Linux.


## <a name="create-vms-with-docker-machine"></a>Vytvoření VMs s Docker počítače

Vytvoření docker hostitele VMs v Azure s `docker-machine create` příkaz pomocí `azure` ovladač argument možnost ovladač (`-d`) a všechny argumenty. 

Následující příklad vycházejí z předpokladu, výchozí hodnoty, ale otevřete port 80 v OM k Internetu a otestujte s kontejnerem nginx něco v ní `ops` přihlášení uživatele k SSH a volání nových OM `machine`. 

Typ `docker-machine create --driver azure` zobrazíte možnosti a jejich výchozími hodnotami. Můžete taky přečíst [si přečtěte následující dokumentaci Docker Azure ovladač](https://docs.docker.com/machine/drivers/azure/). (Vezměte v úvahu, že máte dvojúrovňové ověřování povoleno, zobrazí se výzva ověření pomocí druhý faktor.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Výstup by měl vypadat asi takto, podle toho, jestli máte dvojúrovňové ověřování nakonfigurovali ve vašem účtu.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Konfigurace docker prostředí

Teď zadejte `docker-machine env <VM name>` zobrazíte, co potřebujete ke konfiguraci prostředí. 

```bash
docker-machine env machine
```

Který bude vytištěn spolu prostředí informace, které vypadá přibližně takto. Poznámka: IP adresu byl přiřazen, které budete potřebovat k testování OM.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Můžete buď spuštění příkazu navrhovanou konfiguraci nebo proměnné prostředí můžete nastavit sami. 

## <a name="run-a-container"></a>Spuštění kontejneru

Teď můžete spustit na jednoduché webový server otestovat, zda vše funguje správně. Tady jsme pomocí obrázku standardních nginx určit, zda by měly naslouchají relacím na port 80 a, že pokud OM restartuje kontejneru by měl restartujte i (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Výstup by měl vypadat nějak takto:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testování kontejneru

Zkontrolujte pracovního kontejnery pomocí `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

A zkontrolujte pracovního kontejner, zadejte `docker-machine ip <VM name>` zobrazíte IP adresu (Pokud jste zapomněli z `env` příkaz):

![Průběžný ngnix kontejneru](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Další kroky

Pokud jste zúčastněnými, můžete vyzkoušet Azure [Docker OM rozšíření](virtual-machines-linux-dockerextension.md) se stejnou operaci pomocí rozhraní příkazového řádku Azure nebo šablony správce Azure zdroje. 

Další příklady práce s Docker najdete v článku [práce s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z připojit [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Rychlé další prohlídky z ukázku HealthClinic.biz najdete v článku [Rychlé Azure vývojář nástroje prohlídky](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

