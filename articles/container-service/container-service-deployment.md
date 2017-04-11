<properties
   pageTitle="Nasazení služby Azure kontejneru clusteru | Microsoft Azure"
   description="Nasazení služby Azure kontejneru clusteru pomocí portálu Azure, rozhraní příkazového řádku Azure nebo Powershellu."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Nasazení služby Azure kontejneru obrázku

Služba Azure kontejneru poskytuje rychlého nasazení Oblíbené kontejneru otevřít zdroj clusterů a průběhu řešení. Pomocí služby Azure kontejneru nástroje můžete nasazovat Datacentrum nebo OS a Docker Swarm clusterů šablonami správce prostředků Azure nebo portálu Azure. Nasazení těchto clusterů pomocí Azure virtuálního počítače měřítko sady a clusterů Využijte výhod Azure sítí a úložiště nabídky. Přístup ke službě kontejneru Azure, je nutné Azure předplatného. Pokud nemáte jeden, pak můžete registraci [bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Tento dokument vás provede nasazení služby Azure kontejneru clusteru pomocí [Azure portál](#creating-a-service-using-the-azure-portal), [Azure rozhraní příkazového řádku (rozhraní příkazového řádku)](#creating-a-service-using-the-azure-cli)a pak na [modul Azure Powershellu](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Vytvoření službu pomocí portálu Azure

Přihlaste se k portálu Azure, vyberte **Nový**a vyhledejte **Azure kontejneru služby**Azure Marketplace.

![Vytvoření nasazení 1](media/acs-portal1.png)  <br />

Vyberte **Službu Azure kontejner**a klikněte na **vytvořit**.

![Vytvoření nasazení 2](media/acs-portal2.png)  <br />

Zadejte následující informace:

- **Uživatelské jméno**: Toto je uživatelské jméno, které bude sloužit k účtu na každý z virtuálních počítačích a měřítko virtuálního počítače sad v kontejneru služby Azure obrázku.
- **Předplatné**: Vyberte Azure předplatné.
- **Pole Skupina zdroje**: Vyberte existující skupinu zdrojů nebo vytvořte nový účet.
- **Umístění**: Vyberte Azure oblast pro nasazení služby Azure kontejner.
- **Veřejný klíč SSH**: Přidání veřejným klíčem, který bude sloužit k ověření služba Azure kontejneru virtuálních počítačích. Nezapomeňte, že tento klíč obsahuje žádné konce řádků a obsahuje předpona "ssh-rsa" a 'username@domain' postfixový. By měl vypadat nějak následující: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm **. Pokyny k vytvoření klíčů zabezpečené prostředí (SSH) najdete v článcích [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) a [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) .

Až budete připraveni pokračovat, klikněte na tlačítko **OK** .

![Vytvoření nasazení 3](media/acs-portal3.png)  <br />

Vyberte typ průběhu. Tyto možnosti jsou:

- **Datacentrum/OS**: nasazuje Datacentrum/OS clusteru.
- **Swarm**: nasazuje Docker Swarm clusteru.

Až budete připraveni pokračovat, klikněte na tlačítko **OK** .

![Vytvoření nasazení 4](media/acs-portal4.png)  <br />

Zadejte následující informace:

- **Spočítání předloha**: počet předlohy v clusteru.
- **Počet Agent**: pro Docker Swarm, stane se toto počáteční číslo agentů v sadě agent měřítko. Pro Datacentrum/s operačním systémem půjde počáteční číslo agentů v sadě soukromé měřítko. Kromě toho je vytvořit sadu veřejné měřítko, obsahující předem počet agentů. Počet agentů v této sadě veřejné měřítko, je určený podle kolik předlohy byly vytvořeny v clusteru – jeden veřejné agent pro jednu předlohu a dva veřejné agentů pro tři nebo pět předloh.
- **Agent virtuálního počítače velikost**: velikost agent virtuálních počítačích.
- **Předpona DNS**: světě jedinečný název, který se bude používat pro Předpona klíčové části plně kvalifikované názvy domén pro službu.

Až budete připraveni pokračovat, klikněte na tlačítko **OK** .

![Vytvoření nasazení 5](media/acs-portal5.png)  <br />

Po dokončení ověření služby, klikněte na tlačítko **OK** .

![Vytvoření nasazení 6](media/acs-portal6.png)  <br />

Klikněte na **vytvořit** spusťte proces nasazení.

![Vytvoření nasazení 7](media/acs-portal7.png)  <br />

Pokud jste rozhodl připnout nasazení k portálu Azure, uvidíte stav nasazení.

![Vytvoření nasazení 8](media/acs-portal8.png)  <br />

Po dokončení nasazení služby Azure kontejneru clusteru je připraven k použití.

## <a name="create-a-service-by-using-the-azure-cli"></a>Vytvoření službu pomocí rozhraní příkazového řádku Azure

Vytvoření instanci služby Azure kontejneru pomocí příkazového řádku, je třeba Azure předplatného. Pokud nemáte jeden, pak můžete registraci [bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Je také třeba [nainstalovali](../xplat-cli-install.md) a [nakonfigurovali](../xplat-cli-connect.md) Azure rozhraní příkazového řádku.

Abyste mohli nasadit Datacentrum/OS nebo Docker Swarm clusteru, vyberte jednu z následujících šablon GitHub. Všimněte si, že obou těchto šablonách jsou uvedena stejná, s výjimkou orchestrator výchozí.

* [Šablona Datacentrum/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Šablona roj](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Potom se ujistěte, že rozhraní příkazového řádku Azure připojen k předplatné Azure. Pomocí následujícího příkazu můžete udělat toto:

```bash
azure account show
```
Vrátí-li účet Azure není, použijte následující příkaz přihlašovat rozhraní příkazového řádku pro Azure.

```bash
azure login -u user@domain.com
```

Potom nakonfigurujte nástroje Azure rozhraní příkazového řádku pro správce prostředků Azure použít.

```bash
azure config mode arm
```

Vytvoření skupiny Azure zdrojů a služba kontejneru clusteru pomocí následujícího příkazu, kde:

- **RESOURCE_GROUP** stejný název, který chcete použít pro tuto službu skupina zdroje.
- **Umístění** je Azure oblasti, kde bude vytvořen pole Skupina zdroje a nasazení služby Azure kontejner.
- **TEMPLATE_URI** je umístění souboru nasazení. Všimněte si, že to musí být nezpracovanými souboru, nikoli ukazatel na GitHub uživatelského rozhraní. Tato adresa URL najdete v GitHub vyberte požadovaný soubor azuredeploy.json a klikněte na tlačítko **suroviny** .

> [AZURE.NOTE] Spuštění příkazu prostředí vás vyzve k hodnoty parametrů nasazení.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Zadání parametrů šablony

Tato verze příkazu vyžaduje interaktivně definovat parametry. Pokud chcete zadat parametry, například ve formátu JSON řetězec, který můžete to udělat pomocí `-p` přepnout. Příklad:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Můžete taky poskytnout souboru ve formátu JSON parametrů pomocí `-e` přepnout:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Chcete-li zobrazit příklad parametry souboru s názvem `azuredeploy.parameters.json`, najděte ho šablonami služba Azure kontejneru v GitHub.

## <a name="create-a-service-by-using-powershell"></a>Vytvořit službu pomocí prostředí PowerShell

Můžete taky nasadit služby Azure kontejneru obrázku s prostředím PowerShell. Tento dokument je založen na verze 1.0 [modul Azure Powershellu](https://azure.microsoft.com/blog/azps-1-0/).

Abyste mohli nasadit Datacentrum/OS nebo Docker Swarm clusteru, vyberte jednu z následujících šablon. Všimněte si, že obě tyto šablony jsou uvedena stejná, s výjimkou orchestrator výchozí.

* [Šablona Datacentrum/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Šablona roj](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Před vytvořením clusteru předplatné Azure, ověřte, relaci Powershellu se přihlášení ke Azure. Lze provést pomocí `Get-AzureRMSubscription` příkaz:

```powershell
Get-AzureRmSubscription
```

Pokud budete potřebovat přihlásit Azure, použít `Login-AzureRMAccount` příkaz:

```powershell
Login-AzureRmAccount
```

Pokud jste nasazení do nové skupiny prostředků, musíte nejdřív vytvořit skupina zdroje. Vytvoření nové skupiny prostředků, můžete `New-AzureRmResourceGroup` příkaz a zadejte skupinu název a umístění oblast zdroje:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Až vytvoříte skupinu zdrojů, můžete vytvořit svůj cluster pomocí následujícího příkazu. Identifikátor URI požadovanou šablonu bude určeným pro `-TemplateUri` parametr. Spuštění příkazu prostředí PowerShell vás vyzve k hodnoty parametrů nasazení.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Zadání parametrů šablony

Pokud znáte Powershellu, víte, že můžete přepínat mezi dostupné parametry rutina stisknutím kláves znaménko minus (-) a potom stisknutím klávesy TAB. Stejnou funkci taky označené jako pracuje s parametry, které definujete v šabloně. Hned zadejte název šablony rutinu načte šabloně analyzuje parametry a přidá parametry šablony s příkazem dynamicky. To usnadňuje velmi zadejte hodnoty parametrů šablony. A pokud zapomenete potřeba parametr, prostředí PowerShell výzvu k zadání hodnoty.

Tady je příkaz úplné s parametrů. Zadání vlastní hodnoty pro názvy zdrojů.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Další kroky

Teď, když máte funkční obrázku najdete v článku tyto dokumenty pro připojení a správa podrobnosti:

- [Připojení k obrázku kontejneru služby Azure](container-service-connect.md)
- [Práce se službou Azure kontejner a Datacentrum/OS](container-service-mesos-marathon-rest.md)
- [Práce se službou Azure kontejner a Docker roj](container-service-docker-swarm.md)
