<properties
   pageTitle="Nasazení uzlu 3 Deis clusteru | Microsoft Azure"
   description="Tento článek popisuje, jak vytvořit 3 uzel Deis clusteru v Azure pomocí šablony správce prostředků Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Nasazení uzlu 3 Deis obrázku

Tento článek vás provede s zřizování [Deis](http://deis.io/) clusteru v Azure. Zahrnuje všechny potřebné kroky vytvářet potřebné certifikátů na nasazení a stejné měřítko ukázku aplikace **přejděte** na nově vytvořený obrázku.

Na následujícím obrázku vidíte architektura nasazení systému. Správce systému má na starosti obrázku pomocí nástrojů, jako jsou **deis** a **deisctl**Deis. Připojení se vytvářejí prostřednictvím Vyrovnávání zatížení Azure, odkud připojení na jednu člena uzlů v clusteru. Access klientů nainstalovali aplikace prostřednictvím Vyrovnávání zatížení stejně. V tomto případě vyrovnávání zatížení přepošle umožnění datových přenosů do Deis směrovači mřížky, která dále routs umožnění datových přenosů do odpovídající Docker kontejnery hostiteli clusteru.

  ![Diagram architektury nasazeném clusteru Desis](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Chcete-li spustit následujícími kroky, budete potřebovat:

 * Aktivní Azure předplatné. Pokud nemáte, můžete získat bezplatné záznam na [azure.com](https://azure.microsoft.com/).
 * Pracovní nebo školní id pro používání skupin Azure prostředků. Pokud máte osobním účtem a protokolu pomocí id služeb Microsoft, je potřeba [vytvořit pracovní id od vaší osobní](virtual-machines-windows-create-aad-work-id.md).
 * Buď – v závislosti na klientské operační systémy – [Azure PowerShell](../powershell-install-configure.md) nebo [Azure rozhraní příkazového řádku pro Mac a Linux, Windows](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL bude použito k vygenerování potřebné certifikáty.
 * Libovolná klient, například [Libovolná flám](https://git-scm.com/).
 * Testování ukázkové aplikaci, musíte taky serveru DNS. Můžete všechny servery DNS nebo služby, které podporují záznamy A zástupných znaků.
 * Počítači spusťte Deis klientských nástrojích. Můžete použít místní počítač nebo virtuálního počítače. Tyto nástroje je možné spouštět na téměř libovolného Linux rozdělení, ale podle následujících pokynů použít systémem Ubuntu.

## <a name="provision-the-cluster"></a>Zřízení clusteru

V této části které budete používat šablonu [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) z úložiště otevřít zdroj [azure rychlý úvod šablony](https://github.com/Azure/azure-quickstart-templates). Nejdřív budete kopírujete dolů na šablonu. Potom vytvoříte nový SSH pár klíče pro ověřování. A pak budete konfigurovat nové identifikátor pro můžete obrázku. A nakonec použijete skriptu prostředí nebo skript Powershellu zřízení clusteru.

1. Klonovat úložiště: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Přejděte do složky šablony:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Vytvořte nový klíčový pár SSH pomocí ssh-keygen:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Vytvoření certifikátu pomocí výše uvedených soukromý klíč:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Přejděte na [https://discovery.etcd.io/new](https://discovery.etcd.io/new) pro vytvoření nového obrázku token, které vypadá, třeba:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Každý cluster jádro operačního systému, musí mít jedinečný token této bezplatné služby. Další informace najdete v článku [si přečtěte následující dokumentaci jádro operačního systému](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Upravte soubor **cloudu config.yaml** nahradit stávající token **zjišťování** nový token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Upravit soubor **azuredeploy parameters.json** : otevření certifikátu jste vytvořili v kroku 4 v textovém editoru. Zkopírujte veškerý text mezi `----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` do parametru **sshKeyData** (musíte se odebrat všechny znaky nového řádku).

8. Změňte parametr **newStorageAccountName** . Jedná se o ukládání účet s operačním systémem OM disků. Tento název účtu musí být jedinečné.

9. Změňte parametr **publicDomainName** . Tím vytvoříte část názvu DNS spojené s veřejnou IP Vyrovnávání zatížení. Konečný plně kvalifikovaný název domény bude mít formát _[hodnotu tohoto parametru]_. _[Oblast]_. cloudapp.azure.com. Například pokud zadáte název jako deishbai32 a skupina zdroje je používaný oblastí Západ USA, pak konečný plně kvalifikovaný název domény pro vyrovnávání zatížení budou deishbai32.westus.cloudapp.azure.com.

10. Uložte soubor parametrů. A potom zřízení clusteru pomocí prostředí PowerShell Azure:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  nebo Azure rozhraní příkazového řádku:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Po zřízení skupina zdroje se zobrazí všechny zdroje ve skupině na Azure klasické portálu. Jak ukazuje následující obrázek, skupina zdroje obsahuje virtuální sítě s tři VMs, které jsou spojeny do stejné sady dostupnosti. Skupina obsahuje také při vyrovnávání zatížení, který má přidružená veřejnou IP.

  ![Skupina zřizování zdroje na Azure klasické portálu](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Instalace klienta

Potřebujete **deisctl** na ovládací prvek vaší Deis obrázku. Přestože deisctl je automaticky instalován do všech uzlech, je vhodné použít deisctl na samostatném pro správu počítač. Kromě toho protože všechny uzly budou nakonfigurována pouze soukromé IP adresy, musíte použít SSH tunneling prostřednictvím Vyrovnávání zatížení, tato role má veřejnou IP se připojit k počítačích uzel. Dále jsou kroky nastavení deisctl na samostatném systémem Ubuntu fyzické nebo virtuálního počítače.

1. Instalace deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Přidání soukromý klíč ssh agent:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Konfigurace deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Šablona definuje příchozí pravidla překladu síťových adres, které se mapují 2223 instance 1, 2224 instanci 2 a 2225 instanci 3. Pomocí nástroje deisctl to poskytuje redundance. Podívat se na tato pravidla na Azure klasické portálu:

![Překladu síťových adres pravidla pro vyrovnávání zatížení](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Šablony v současné době podporuje pouze clusterů uzel 3. Toto je z důvodu omezení v šabloně správce prostředků Azure definice pravidla překladu síťových adres, který nepodporuje smyčka syntaxe.

## <a name="install-and-start-the-deis-platform"></a>Nainstalujte a spusťte Deis platformy

Teď můžete deisctl nainstalujte a spusťte Deis platformy:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Spuštění platformu trvá dlouho (co nejvíc 10 minut). Spuštění Tvůrce služby zvlášť, může trvat dlouho. A někdy trvá pár pokusech úspěšné: Pokud operace přestane reagovat, zadejte `ctrl+c` přerušit spuštění příkazu a opakování.

Můžete použít `deisctl list` k ověření, pokud jsou spuštěné všechny služby:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Blahopřejeme! Nyní máte spuštěné Deis clsuter na Azure! Dále si nasazení výběru přejít aplikace zobrazíte obrázku v akci.

## <a name="deploy-and-scale-a-hello-world-application"></a>Nasazení a měřítko Vítáme aplikace

Následující postup popisuje pro nasazení "Ahoj světe" přejděte aplikace clusteru. Kroky vycházejí [Deis si přečtěte následující dokumentaci](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Pro směrování síť fungovala správně musíte mít zástupných A záznam pro vaši doménu přejdete na veřejnou IP Vyrovnávání zatížení. Následující obrázek ukazuje záznam pro registraci ukázkové domény na GoDaddy:

    ![Záznam a Godaddy](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Instalace deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Vytvořte nový klíč SSH a potom přidejte veřejným klíčem do GitHub (samozřejmě můžete taky znovu použít existující klíče). Vytvořit nový klíčový pár SSH, můžete:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Dodejte GitHub id_rsa.pub nebo veřejným klíčem podle svého výběru. Můžete to provést pomocí přidat SSH klíčové tlačítko v obrazovce SSH klíče konfigurace:

  ![Github klíč](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Registrace nového uživatele:

        deis register http://deis.[your domain]
<p />
6. Přidání klíče SSH:

        deis keys:add [path to your SSH public key]
  <p />      
7. Vytvoření aplikace.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Libovolná nabízených spustí Docker obrázků vytvořené a nasazení, které bude trvat několik minut. Z prostředí může být někdy 10 krok (Pushing obrázek do soukromé úložiště) může přestat reagovat. V takovém případě můžete ukončit proces, odeberte aplikaci pomocí "deis aplikace: destroy – <application name> ` to remove the application and try again. You can use `deis apps:list" Zjistěte název aplikace. Pokud všechno funguje měli byste zkontrolovat přibližně na konci příkazu výstupy:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Ověřte, zda aplikace funguje:

        curl -S http://[your application name].[your domain]
  Měli byste vidět:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Měřítko aplikace na 3 instance:

        deis scale cmd=3
<p />
11. Pokud chcete, můžete použít deis informace zkontrolujte podrobnosti o aplikaci. Následující výstupy jsou z mé nasazení aplikace:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Další kroky

Tento článek se projít všechny potřebné kroky zřízení nového Deis clusteru v Azure pomocí šablony správce prostředků Azure. Šablona podporuje redundance nástroje připojení, jakož i Vyrovnávání zatížení pro nasazení aplikace. Šablona je také zabráněno pomocí veřejné adresy IP uzlech člena, které se uloží cenné veřejných zdrojů IP a nabízí další zabezpečené prostředí aplikacím Host (hostitel). Další informace naleznete v následujících článcích:

[Azure Přehled Správce zdrojů] [resource-group-overview]  
[Jak používat rozhraní příkazového řádku Azure] [azure-command-line-tools]  
[Správce Azure Powershellu s Azure zdroje] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
