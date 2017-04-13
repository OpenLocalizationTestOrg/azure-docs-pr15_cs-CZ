<properties
   pageTitle="Začínáme používat docker s roj na Azure"
   description="Popisuje, jak vytvořit skupinu VMs s příponou OM Docker a použijte roj k vytvoření Docker clusteru."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Jak používat docker s roj

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Toto téma ukazuje pomocí [docker](https://www.docker.com/) [swarm](https://github.com/docker/swarm) k vytvoření clusteru roj Správa přístupových práv v Azure velmi jednoduché. Čtyři virtuálních počítačích předtím vytvořil v Azure, jeden má fungovat jako správce roj a tři jako součást clusteru docker tabulkami hosts. Až to uděláte, můžete vidět obrázku a začněte používat docker na něm roj. Kromě toho hovory Azure rozhraní příkazového řádku v tomto tématu v režimu služby správy (asm). 

> [AZURE.NOTE] Toto téma používá docker s roj a v Azure rozhraní příkazového řádku *bez* použití **docker zařízení** chcete zobrazit, jak různých nástrojů spolupracují ale zůstávají nezávislé. počítač **docker** **– swarm** přepínače, které vám umožní využít **docker počítače** přímo přidáte roj uzlů. Příklad naleznete v dokumentaci [docker počítače](https://github.com/docker/machine) . Pokud zmeškané **docker počítači** spuštěná proti Azure VMs vás [naučí používat docker stroje s Azure](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Vytvoření docker tabulkami hosts virtuálních počítačích s Azure

Toto téma vytvoří čtyři VMs, ale můžete použít jiné číslo, které chcete. Volání následující s * &lt;heslo&gt; * nahrazuje heslo jste zvolili.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Až to budete mít by měl nebudou moct používat **azure OM seznamu** zobrazíte Azure VMs:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Instalace roj v předloze roj OM

Toto téma používá [kontejneru modelu instalace z docker swarm si přečtěte následující dokumentaci](https://github.com/docker/swarm#1---docker-image) – ale můžete taky SSH **roj předlohy**. V tomto modelu **swarm** stažení jako kontejner docker systém roj. Vrací provedeme tento krok *vzdáleně z našich přenosném počítači pomocí docker* pro připojení k **roj předlohy** OM a určit, aby použijte příkaz vytvoření id clusteru **roj vytvořit**. Id clusteru je **swarm** zjistí, jak členové skupiny roj. (Můžete taky klonovat úložiště a vytvořili sami, které umožňují Úplné řízení a povolit ladění.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Poslední řádek je id clusteru; Zkopírujte někde vzhledem k tomu, že budete používat se znovu připojíte uzel VMs předlohy roj vytvořit "roj". V tomto příkladu je id clusteru **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Jenom Pokud chcete vymazat, používáme naše místní docker instalace připojení k **roj předlohy** OM Azure a pokyny k **roj předlohy** ke stažení, instalace a spuštění příkazu **vytvořit** , který vrátí id naše obrázku, který používáme později pro účely zjišťování.
<!-- -->
> Chcete-li potvrdit, spusťte `docker -H tcp://` * &lt;hostname&gt; * ` images` seznam kontejneru procesy v počítači **roj předlohy** a na jiný uzel pro porovnání (protože jsme předchozí příkaz roj spustili s přepínačem **– SV** kontejneru byla odebrána ho po, tak pomocí **docker ps - a** nevrátí NIC).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Pokud znáte **docker**, budete vědět, že ostatní uzly mít žádné položky, protože byly stáhnout a spustit ještě žádné obrázky.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Připojit se ke uzel VMs naše docker clusteru

Pro každý uzel seznam koncový bod informací pomocí rozhraní příkazového řádku Azure. Tady vidíte jsme to udělat pro hostitele docker **roj uzel-1** za účelem získání na uzel docker port.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Použití **docker** a `-H` možnost tak, aby ukazovaly na uzel OM, klientovi docker spojení uzel a roj vytváříte předáním id clusteru a na uzel docker port (ten pomocí **– adresa**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Která vypadá všechno dobře. Potvrďte, že **swarm** běží na **roj uzel-1** , kterou jsme zadejte:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Opakujte pro všechny ostatní uzly v clusteru. V našem případě jsme to udělat pro **roj uzel-2** a **roj uzel-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Zahájit správu roj obrázku

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

a pak můžete vytvořit seznam se uzly clusteru:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Přejděte spustit věci ve vaší roj. Můžete vyhledávat inspiraci, najdete v článku [https://github.com/docker/swarm/](https://github.com/docker/swarm/)nebo možná [videa](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
