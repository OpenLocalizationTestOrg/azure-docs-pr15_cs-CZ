<properties
   pageTitle="Instalace MongoDB na Linux OM | Microsoft Azure"
   description="Zjistěte, jak nainstalovat a nakonfigurovat MongoDB na počítač virtuální Linux v Azure pomocí nasazení modelu správce prostředků."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Nainstalujte a nakonfigurujte MongoDB na Linux OM v Azure
[MongoDB](http://www.mongodb.org) je Oblíbené zdroje otevřít, výkonné NoSQL databázi. Tento článek ukazuje, jak nainstalovat a nakonfigurovat MongoDB na Linux OM v Azure pomocí nasazení modelu správce prostředků. Příklady jsou uvedeny této podrobností jak chcete:

- [Ruční instalace a konfigurace základní instance MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
- [Vytvoření základní MongoDB instance pomocí šablony správce prostředků](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Vytvářet složité MongoDB sharded obrázku s otevřené nastaví pomocí šablony správce prostředků](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Zjistit předpoklady pro
Tento článek vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).
- přihlášení k lyncu se [Rozhraní příkazového řádku Azure](../xplat-cli-install.md)`azure login`
- rozhraní příkazového řádku Azure *musí být* při použití režimu správce prostředků Azure`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Ruční instalace a konfigurace MongoDB na virtuálního počítače
MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) Linux distros včetně červené klobouk / CentOS, SUSE, se systémem Ubuntu a Debian. Následující příklad vytvoří `CoreOS` OM pomocí SSH klíč uložený na `.ssh/azure_id_rsa.pub`. Příjem pokynů pro název účtu úložiště, název DNS a přihlašovací údaje správce:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Přihlaste se k OM pomocí veřejné adresy IP zobrazí na konci předchozí krok vytvoření OM:

```bash
ssh ops@40.78.23.145
```

Přidat zdrojů instalace MongoDB, vytvořit `yum` úložiště souborů takto:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

MongoDB repo soubor otevřen pro úpravy. Přidejte následující řádky:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Instalace pomocí MongoDB `yum` následujícím způsobem:

```bash
sudo yum install -y mongodb-org
```

Ve výchozím nastavení je na CentOS obrázky, které brání otevření MongoDB nevynucují SELinux. Instalace nástroje pro správu zásad a konfigurace SELinux umožňuje MongoDB při práci s jeho výchozí port TCP 27017 následujícím způsobem. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Spusťte službu MongoDB následujícím způsobem:

```bash
sudo service mongod start
```

Ověření instalace MongoDB připojením pomocí místní `mongo` klienta:

```bash
mongo
```

Nyní otestujte MongoDB instance přidáním některá data a potom hledání:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Pokud budete chtít, nakonfigurujte MongoDB, aby se spouštěl automaticky během restartování systému:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Vytvoření základní instance MongoDB na CentOS použít šablonu?
Vytvořit základní instance MongoDB na jeden OM CentOS pomocí následující Azure rychlý úvod šablony z Github. Tato šablona používá příponu vlastní skript Linux přidáte `yum` úložiště nově vytvořený OM CentOS a nainstalujte MongoDB.

- [Základní MongoDB instance na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` oblast. Zadejte vlastní hodnoty následujícím způsobem:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Rozhraní příkazového řádku Azure vrátí se výzva v rámci několik sekund, než vytváření nasazení, ale instalace a konfigurace trvá několik minut. Kontrola stavu nasazení s `azure group deployment show myResourceGroup`, zadejte název skupiny prostředků příslušným způsobem. Počkejte, dokud `ProvisioningState` zobrazuje "Byl úspěšný" před pokusem o SSH bude v angličtině.

Po zavedení dokončení SSH bude v angličtině. Získá IP adresu OM pomocí `azure vm show` příkazu jako v následujícím příkladu:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Těsně před koncem výstup `Public IP address` se zobrazí. SSH angličtině s IP adresu vašeho OM:

```bash
ssh ops@138.91.149.74
```

Ověření instalace MongoDB připojením pomocí místní `mongo` klienta takto:

```bash
mongo
```

Nyní otestujte instance přidáním některá data a hledání následujícím způsobem:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Vytvářet složité Sharded clusteru MongoDB na CentOS použít šablonu?
Můžete vytvořit složité MongoDB sharded cluster pomocí následující Azure rychlý úvod šablony z Github. Tato šablona sleduje [MongoDB sharded clusteru osvědčené postupy](https://docs.mongodb.com/manual/core/sharded-cluster-components/) k poskytování redundance a dostupnost. Šablona vytvoří dva shards s tři uzly ve všech sadách otevřené. Také vytvoření jedné config serveru otevřené sada se tři uzly plus dva `mongos` směrovači servery zajistit soulad aplikací z přes shards.

- [Shluk Sharding MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Nasazení tento složité clusteru sharded MongoDB vyžaduje víc než 20 jádra, které je obvykle výchozí počet základních na oblast pro předplatné. Otevřete žádost o konverzaci Azure podpory ke zvýšení počtu core.

Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` oblast. Zadejte vlastní hodnoty následujícím způsobem:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Rozhraní příkazového řádku Azure vrátí se výzva v rámci několik sekund, než vytváření nasazení, ale instalace a konfigurace můžete převzít řízení za hodinu dokončete. Kontrola stavu nasazení s `azure group deployment show myResourceGroup`, příslušným způsobem nastavení na název skupiny zdrojů. Počkejte, dokud `ProvisioningState` zobrazuje "Byl úspěšný" před připojením k VMs.


## <a name="next-steps"></a>Další kroky
V těchto příkladech se připojíte ke MongoDB instance místně z OM. Pokud chcete připojit k instanci MongoDB z jiného OM nebo sítě, zajistit, aby odpovídající [pravidla skupiny zabezpečení sítě se vytvářejí](virtual-machines-linux-nsg-quickstart.md).

Další informace o vytváření použití šablon najdete v tématu [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

Správce prostředků Azure šablony koncovku vlastní skript umožňuje stáhnout a spustit skripty na vaše VMs. Další informace najdete v tématu [použití Azure vlastní skript rozšíření s Linux virtuálních počítačích](virtual-machines-linux-extensions-customscript.md).