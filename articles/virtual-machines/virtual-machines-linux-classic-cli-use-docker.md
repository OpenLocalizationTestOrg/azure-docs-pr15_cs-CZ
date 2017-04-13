<properties
    pageTitle="Použití přípony OM Docker Linux na Azure"
    description="Popisuje Docker a rozšíření Azure virtuálních počítačích a ukazuje, jak programově vytváření virtuálních počítačích na Azure, které jsou docker tabulkami hosts z příkazového řádku pomocí rozhraní příkazového řádku Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Pomocí koncovku Docker OM z příkazového řádku Azure rozhraní (Azure rozhraní příkazového řádku)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Toto téma popisuje, jak vytvořit virtuálního počítače s příponou OM Docker v režimu management (asm) služby v Azure rozhraní příkazového řádku na libovolné platformě. [Docker](https://www.docker.com/) patří mezi nejoblíbenější přístupy virtualization, která využívají [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) spíše než virtuálních počítačích tak izolace dat a výpočetních na sdíleného zdroje. Rozšíření Docker OM a [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) slouží k vytvoření Docker OM, který je hostitelem libovolný počet kontejnery pro používání aplikací v Azure. Diskuse uceleném kontejnery a jejich výhody najdete v tématu [Docker vysokou úroveň Tabule](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Jak používat rozšíření OM Docker s Azure
Rozšíření Docker OM pomocí služby Azure, musíte nainstalovat verzi [Azure příkazového řádku rozhraní](https://github.com/Azure/azure-sdk-tools-xplat) (Azure rozhraní příkazového řádku) vyšší než 0.8.6 (jak je z tohoto psaní aktuální verze není 0.10.0). Rozhraní příkazového řádku Azure můžete nainstalovat na Windows, Mac a Linux.


Dokončení procesu používání Docker na Azure je jednoduchá:

+ Instalace Azure rozhraní příkazového řádku a jeho závislostí na počítači, ze kterého chcete ovládací prvek Azure (ve Windows, půjde Linux rozdělení spuštěný jako virtuální počítač)
+ Příkazy Azure rozhraní příkazového řádku Docker slouží k vytvoření hostitel OM Docker v Azure
+ Příkazy místní Docker ke správě Docker kontejnery v Docker OM v Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Instalace Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)

Instalace a konfigurace rozhraní příkazového řádku Azure, najdete v tématu [instalace rozhraní Azure příkazového řádku](../xplat-cli-install.md). Potvrďte instalaci zadejte `azure` na příkazovém řádku a po krátkou chvíli trvat, než byste měli vidět obrázek Azure rozhraní příkazového řádku ASCII, které jsou uvedeny základní příkazy k dispozici. Pokud instalace fungoval správně, je třeba stíhat psát `azure help vm` a zjistíte, že jeden z uvedených příkazů je "docker".

> [AZURE.NOTE] Docker obsahuje nástroje pro systém Windows, [Docker stroje](https://docs.docker.com/installation/windows/), které můžete také automatizovat vytváření poštovních docker klienta, který můžete použít k práci s Azure VMs jako docker tabulkami hosts.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Azure rozhraní příkazového řádku pro připojení k účtu Azure
Než budete moct použít rozhraní příkazového řádku Azure musíte přidružit přihlašovacích údajů účet Azure rozhraní příkazového řádku Azure o svoji platformu. V části [jak se připojit k předplatnému Azure](../xplat-cli-connect.md) vysvětluje, jak stáhnout a importujte soubor **.publishsettings** nebo vaše rozhraní příkazového řádku Azure přidružit id organizace.

> [AZURE.NOTE] Při použití jedné nebo jiné metody ověřování, takže nezapomeňte si přečíst výše uvedeného pochopit různé funkce dokumentu jsou určité rozdíly v chování.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Instalace Docker a použití koncovku OM Docker Azure
Podle [pokynů k instalaci Docker](https://docs.docker.com/installation/#installation) nainstalovat Docker místně na vašem počítači.

Pokud chcete používat Docker s Azure virtuálního počítače, musíte mít Linux obrázek použitý pro OM instalaci [Azure Linux OM Agent](virtual-machines-linux-agent-user-guide.md) . V současné době jsou pouze dva typy obrázků, které jsou zdrojem takto:

+ Obrázek se systémem Ubuntu z Galerie obrázků Azure nebo

+ Vlastní Linux obrázku, kterou jste vytvořili s agentem Azure Linux OM nainstalovali a nakonfigurovali. Další informace o tom, jak vytvořit vlastní OM Linux s agentem OM Azure najdete v článku [Agent OM Azure Linux](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Pomocí Galerie obrázků Azure

Z flám nebo terminálové relace použijte tento příkaz Azure rozhraní příkazového řádku a vyhledejte požadovaný obrázek posledních systémem Ubuntu v galerii OM můžete zadáním

`azure vm image list | grep Ubuntu-14_04`

a vyberte jednu z názvy obrázek, například `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`a zadejte následující příkaz Vytvořit novou OM tento obrázek.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

kde je:

+ * &lt;OM cloudservice název&gt; * je název OM, které budou převedeny na hostitelském počítači kontejneru Docker v Azure

+  * &lt;uživatelské jméno&gt; * je uživatelské jméno uživatele výchozí kořenové OM

+ * &lt;heslo&gt; * je heslo účtu *uživatelské jméno* , které splňují standardy složitost pro Azure

> [AZURE.NOTE] V současné době hesla musí být alespoň na úrovni 8 znaky, obsahovat jeden malá písmena a velká jeden znak, číslo a určité speciální znaky, například jednu z následujících znaků: `!@#$%^&+=`. Ne, období na konci předcházející věty není určité speciální znaky.

Pokud příkaz byl úspěšný, měli byste zkontrolovat přibližně, v závislosti na přesné argumenty a možnosti, které jste použili:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Vytváření virtuálních počítačů může trvat několik minut, ale po byla zajištěna (hodnoty stav je `ReadyRole`) spustí démona Docker (Docker služba) a můžete se připojit k hostiteli Docker kontejner.

Chcete-li otestovat OM Docker nevytvoříte Azure, typ

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

kde * &lt;OM název--použijete&gt; * je název virtuální počítač, který jste použili v hovoru na `azure vm docker create`. Měli byste vidět podobnou následující, což znamená, že je váš hostitel OM Docker nahoru a spuštěna v Azure a čeká se na svého příkazy. 

Teď můžete zkusit připojit pomocí klienta docker získá (v některých nastavení klienta Docker, například, na Mac, bude pravděpodobně nutné použít `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Stejně jako jisti, že je všechny pracovní, můžete zkontrolovat OM pro rozšíření Docker:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Ověřování OM docker Host (hostitel)

Kromě vytváření OM Docker `azure vm docker create` příkaz taky automaticky vytvoří potřebné certifikáty umožňuje klientský počítač Docker připojení k hostiteli Azure kontejneru pomocí HTTPS a certifikáty jsou uložené na počítačích klienta a hostitele podle potřeby. Při dalších pokusech existujících certifikátů opakovaně používat a nasdílel nového hostitele.

Ve výchozím nastavení certifikáty jsou uloženy v `~/.docker`, a Docker bude nakonfigurován pro spuštění na port **. 2376**. Pokud chcete použít jiný port nebo adresář a pak můžete použít jednu z těchto `azure vm docker create` parametry příkazového řádku pro nastavení Docker kontejneru hostovat OM použít jiný port nebo různé certifikáty pro připojení klientů:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Démona Docker na hostiteli nakonfigurovaný tak, aby poslouchat a ověřování připojení klientů na zadané číslo portu pomocí certifikátů generovaných `azure vm docker create` příkaz. Klientský počítač musí mít tato poukázky získat přístup k hostiteli Docker.

> [AZURE.NOTE] Hlavně host spuštěný bez tato poukázky budou všem, kteří mohou připojit k počítači osobám. Před úpravou výchozí konfigurace zajistit, aby vědomi rizik do svého počítače a aplikace.

## <a name="next-steps"></a>Další kroky

* Jste připraveni přejít na [Docker User Guide] a použijte Docker OM. Vytvoření OM Docker s podporou v novém portálu najdete v tématu [použití rozšíření OM Docker pomocí portálu].

* Rozšíření Azure Docker OM taky podporuje, vytvořte Docker, který využívá souboru deklarativní YAML Pokud chcete pořídit aplikace modelovat karta Vývojář v každém prostředí a generovat konzistentní nasazení. V tématu [Začínáme s Docker a vytváření definovat a spuštění aplikace více kontejneru na Azure virtuálního počítače].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Jak používat rozšíření OM Docker pomocí portálu]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker User Guide]: https://docs.docker.com/userguide/
 
[Začínáme s Docker a vytváření definovat a spuštění aplikace na více kontejneru Azure virtuálního počítače]:virtual-machines-linux-docker-compose-quickstart.md