<properties
   pageTitle="Docker a vytváření na počítač virtuální | Microsoft Azure"
   description="Rychlý úvod a práce s vytváření a Docker na virtuálních počítačích Linux v Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Začínáme s Docker a vytváření definovat a spuštění aplikace na více kontejneru Azure virtuálního počítače

Začínáte používat Docker a [vytváření](http://github.com/docker/compose) definovat a spuštění aplikace na komplexní Linux virtuálního počítače v Azure. Vytváření můžete pomocí jednoduchý textový soubor definovat aplikace tvořené několika Docker kontejnery. Můžete pak číselník aplikace v jedné příkazu, který nemá všechno nasazení definovaný prostředí. 

Jako příklad v tomto článku se dozvíte, jak rychle nastavit blogu WordPress s back-end databáze MariaDB SQL na OM systémem Ubuntu. Taky můžete vytvářet nastavit složitější aplikací.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Krok 1: Nastavení Linux OM jako hostitel Docker

Můžete různých Azure postupy a k dispozici obrázky nebo správce prostředků šablony v Azure Marketplace a vytvořte Linux OM ji nastavit jako hostitel Docker. Například najdete v článku [použití koncovku Docker OM nasazení prostředí](virtual-machines-linux-dockerextension.md) a rychle vytvořte systémem Ubuntu OM s příponou OM Docker Azure pomocí [šablony rychlý úvod](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Při použití koncovku Docker OM vaší OM automaticky nastavit jako hostitel Docker a vytváření je už nainstalovaná. Příklad v tomto článku uvidíte, jak používat v [Azure rozhraní příkazového řádku pro Mac a Linux, systému Windows](../xplat-cli-install.md) (rozhraní příkazového řádku Azure) v režimu správce prostředků k vytvoření OM.

Základní příkaz z předchozího dokumentu vytvoří skupinu zdroje s názvem `myResourceGroup` v `West US` umístění a nasadí virtuálního počítače s příponou Azure Docker OM nainstalovaný:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Krok 2: Ověřte, že je nainstalovaný vytváření

Po dokončení nasazení SSH hostitele nové Docker pomocí služby DNS název uvedenou při nasazení. Můžete použít `azure vm show -g myDockerResourceGroup -n myDockerVM` a zobrazit podrobnosti OM, včetně názvu DNS.

Kontrola nainstalované vytváření na OM, spusťte tento příkaz:

```bash
docker-compose --version
```

Zobrazit výstup `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Pokud ještě jeden způsob slouží k vytvoření hostitel Docker a potřebovat k instalaci vytváření sami, najdete v [dokumentaci k vytváření](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Krok 3: Vytvoření souboru konfigurace docker compose.yml

Další vytvoříte `docker-compose.yml` soubor, který je pouze text konfigurační soubor, definujte kontejnery Docker ke spuštění v OM. Soubor Určuje obrázek dělat jednotlivé kontejneru (nebo je možné vytvořit z Dockerfile) potřebné proměnné a závislostí, portů a propojení mezi kontejnery. Informace o syntaxi souboru yml najdete v článku [vytváření souboru](http://docs.docker.com/compose/yml/).

Vytvoření `docker-compose.yml` souborů takto:

```bash
touch docker-compose.yml
```

Umožňuje přidat některá data do souboru Oblíbené textovém editoru. V následujícím příkladu `vi` editor:

```bash
vi docker-compose.yml
```

Následující příklad vložte textový soubor. Konfigurace používá obrázky z [Registru DockerHub](https://registry.hub.docker.com/_/wordpress/) k instalaci WordPress (Otevřít zdroj blogů a obsahu systému správy) a propojené back-end databáze MariaDB SQL. Zadejte vlastní `MYSQL_ROOT_PASSWORD` následujícím způsobem:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Krok 4: Začít vytvářet kontejnery

V adresáři stejný jako svého `docker-compose.yml` souboru, spusťte tento příkaz (v závislosti na prostředí, může být potřeba spustit `docker-compose` pomocí `sudo`.):

```bash
docker-compose up -d

```

Tento příkaz spustí kontejnery Docker podle `docker-compose.yml`. Trvá jednu až dvě pro tento krok pro dokončení minuty. Zobrazí se výstup podobně jako v následujícím příkladu:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Ujistěte se, možnost **d** při spuštění tak, aby kontejnery souvislá na pozadí.

Ověřte kontejnery nahoru, zadejte `docker-compose ps`. Měli byste vidět nějak:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Teď můžete připojit k WordPress přímo na OM na portu 80. Otevřete webový prohlížeč a zadejte název DNS vaší OM (například `http://myresourcegroup.westus.cloudapp.azure.com`). Teď byste měli vidět WordPress úvodní obrazovky, kde můžete provést instalaci a Začínáme s aplikací.

![WordPress úvodní obrazovky][wordpress_start]


## <a name="next-steps"></a>Další kroky

* Přejděte na [Docker OM rozšíření uživatelské příručce](https://github.com/Azure/azure-docker-extension/blob/master/README.md) pro další možnosti pro nastavení Docker a vytváření Docker OM. Jednou z možností je například vložit soubor yml vytváření (převést do formátu JSON) přímo v konfiguraci koncovku Docker OM.
* Podívejte se na [vytváření příkazový řádek](http://docs.docker.com/compose/reference/) a [uživatelská příručka](http://docs.docker.com/compose/) Další příklady vytvoření a nasazení aplikace více kontejner.
* Použití šablony pro správce prostředků Azure buď svůj vlastní nebo jeden přispěla od [komunity](https://azure.microsoft.com/documentation/templates/)nasazení Azure OM s Docker a nastavení se vytvářet aplikace. Šablona [nasazení blogu WordPress s Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) například používá Docker a vytváření k rychlému nasazení WordPress s back-end MySQL na OM systémem Ubuntu.
* Zkuste integrace Docker vytvořte s [Docker Swarm](virtual-machines-linux-docker-swarm.md) obrázku. Scénáře naleznete v tématu [Použití vytvořte s roj](https://docs.docker.com/compose/swarm/) .

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
