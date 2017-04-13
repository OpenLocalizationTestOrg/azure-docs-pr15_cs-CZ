<properties
    pageTitle="Linux a otevřít zdroje na Azure | Microsoft Azure"
    description="Obsahuje seznam Linux a výpočetních otevřít zdroj článků na Azure, včetně základní Linux využití a některé základní pojmy týkající se systém nebo nahrání obrázků Linux na Azure a další obsah týkající se konkrétních technologií a optimalizace."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux a otevřít zdroje na Azure

Najdete všechny si přečtěte následující dokumentaci, které potřebujete k vytvoření a správa na základě Linux virtuálních počítačích v modelu klasické nasazení.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Začínáme
- [Úvod k Linux na Azure](virtual-machines-linux-intro-on-azure.md)
- [Nejčastější dotazy o virtuálních počítačích Azure vytvořená pomocí klasické nasazení modelu](virtual-machines-linux-classic-faq.md)
- [Informace o obrázky virtuálních počítačích](virtual-machines-linux-classic-about-images.md)
- [Nahrání obrázek Distro](virtual-machines-linux-classic-create-upload-vhd.md) (a taky pokyny pomocí [Rozdělení Azure-Endorsed](virtual-machines-linux-endorsed-distros.md))
- [Přihlaste se k Linux OM pomocí portálu Azure klasické](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Nastavení

- [Instalace Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-install.md)


## <a name="tutorials"></a>Výukové programy

- [Nainstalovat zásobníku svítilna na počítač virtuální Linux v Azure](virtual-machines-linux-create-lamp-stack.md)
- [Skutečné na profilů webová aplikace na OM Azure](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Postup: instalace Apache Qpid kanálem C AMQP a Bus služby](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Databáze
- [Optimalizace výkonu MySQL na Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [Clusterů MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Spuštěna Cassandra s Linux Azure a otevření ze Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Vytvoření více hlavních clusteru MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Začínáme s Linux uzly výpočetním clusteru HPC Pack v Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Spuštění NAMD s Microsoft HPC Pack na Linux výpočet uzlů v Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Nastavení spuštění aplikace MPI Linux RDMA obrázku](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Pomocí koncovku Docker OM z příkazového řádku Azure rozhraní (Azure rozhraní příkazového řádku)](virtual-machines-linux-classic-cli-use-docker.md)
- [Použití přípony OM Docker z portálu Microsoft Azure](virtual-machines-linux-classic-portal-use-docker.md)
- [Používání docker počítače na Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Se systémem Ubuntu
- [Postup: clusterů MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Postup: Node.js a Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Postup: instalace a spuštění MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>Jádro operačního systému
- [Postup: použití jádro operačního systému na Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Plánování
- [Pokyny pro implementaci Azure infrastruktury služby](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Výběr Linux uživatelská jména](virtual-machines-linux-usernames.md)
- [Postup při konfiguraci dostupné pro virtuálních počítačích v modelu klasické nasazení](virtual-machines-linux-classic-configure-availability.md)
- [Naplánování plánované údržby na Azure VMs](virtual-machines-linux-planned-maintenance-schedule.md)
- [Správa dostupnosti virtuálních počítačích](virtual-machines-linux-manage-availability.md)
- [Plánovaná údržba Linux virtuálních počítačích v Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Nasazení
- [Vytvoření vlastní virtuálního počítače systémem Linux](virtual-machines-linux-classic-createportal.md)
- [Základní informace: zachycení Linux OM, aby se šablony](virtual-machines-linux-classic-capture-image.md)
- [Informace o-potvrzeného rozdělení](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Správa

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Jak obnovit heslo nebo vlastnosti SSH Linux](virtual-machines-linux-classic-reset-access.md)
- [Pomocí kořenového adresáře](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure zdroje

- [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md)
- [Rozšíření Azure OM a funkce](virtual-machines-windows-extensions-features.md)
- [Vložení vlastních dat do virtuálního počítače pomocí služby cloudu inicializace](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Úložiště

- [Připojení dat disku Linux OM](virtual-machines-linux-classic-attach-disk.md)
- [Odpojení Disk Data z Linux OM](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Sítě
- [Jak nastavit koncové body na klasické virtuálního počítače v Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Řešení potíží
- [Odstraňování potíží s připojením zabezpečené prostředí (SSH) na základě Linux Azure virtuálního počítače](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Poradce při potížích klasické nasazení vytvořit nový počítač virtuální Linux v Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Poradce při potížích klasické nasazení se restartováním nebo změna velikosti existující Linux virtuálního počítače v Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Odkaz

- [Azure příkazy rozhraní příkazového řádku v režimu Správa služby Azure (asm)](../virtual-machines-command-line-tools.md)
- [Správa služby Azure rozhraní REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Hlavní odkazy
Následující odkazy jsou určené pro blogy Microsoft, Technet stránky a externí weby spíše než si přečtěte následující dokumentaci Azure.com jako nahoře. Jsou Azure a výpočetního světa otevřít zdroj přesunutí rychlé cílů, je téměř jisté, že jsou následující odkazy zastaralý *navzdory* na to, že bychom všechno nejlepší k přidání novější témat neustálé odebrat zastaralé z nich. Pokud jsme jednu zmeškané, přejděte prosím dejte nám vědět v komentářích nebo odeslat požadavek vyžádané naše [GitHub repo](https://github.com/Azure/azure-content/).

- [ASP.NET 5 spuštěna Linux použití Docker kontejnerů](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Nasazení obrázek OM CentOS z OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [Infrastruktura SUSE aktualizací](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server pro spolupráci prostřednictvím SAP cloudu zařízení knihovny](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Vytváření vysoce dostupné Linux na Azure způsobem 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatizace zřizování Linux na Azure pomocí rozhraní příkazového řádku Azure, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS na Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Spuštění FreeBSD v Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Snadné nasazení FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Nasazení vlastní obrázek FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV Linux souborovém serveru](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 NoSql otevřít zdroj databáze služby Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Zkušenosti s CouchDb na Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [Službou CouchDB jako-s node.js, CORS a Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Redis v systému Windows služby Azure Redis mezipaměti](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Oznámení o stavu relace ASP.NET poskytovatele pro verzi Preview Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ nyní k dispozici v Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Velký dat
- [Instalace Hadoop Azure Linux VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relační databáze
- [Architektura dostupnost MySQL maximum v Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Instalace Postgres s corosync, pg_bouncer pomocí ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Vysoký výkon Linux computing (HPC)

- [Šablony rychlý úvod: číselník clusteru SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (a [příspěvek blogu](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Šablona rychlý úvod: vytvoření clusteru HPC s Linux výpočetním uzly](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, Správa a optimalizace

Jako světě devops Správa a optimalizace je úplně rozsáhlou a velmi rychle měnit, byste měli zvážit v seznamu výchozí bod.

- [Video: Azure virtuálních počítačích: pomocí Chef, Puppet a Docker pro správu Linux VMs](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Dokončení Průvodce automatického nasazení clusteru Kubernetes s jádro operačního systému a vlákna](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Podřízený Jenkins modul Plug-in pro Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins úložiště modul Plug-in pro Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Třetích stran: Modul Plug-in pro Azure podřízený Hudsonem](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Třetích stran: Modul Plug-in pro Azure úložiště Hudsonem](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Co je Chef a jak to funguje?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Jak pomocí Linux VMs služby Azure automatizaci](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Postup prostředí Powershell DSC Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker klienta DSC](https://github.com/anweiss/DockerClientDSC)

- [Modul plug-in balírna pro Azure](https://github.com/msopentech/packer-azure)
