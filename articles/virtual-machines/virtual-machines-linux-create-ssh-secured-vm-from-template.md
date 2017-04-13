<properties
    pageTitle="Vytvoření Linux OM pomocí Azure šablony | Microsoft Azure"
    description="Vytvořte v Azure pomocí šablony správce prostředků Azure OM Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Vytvoření Linux OM pomocí Azure šablony

Tento článek ukazuje, jak rychle nasadit Linux virtuálního počítače na Azure pomocí šablony Azure.  V článku vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure rozhraní příkazového řádku](../xplat-cli-install.md) se přihlásili `azure login`.

- Správce prostředků Azure režimu rozhraní příkazového řádku Azure _musí být v_ `azure config mode arm`.

Můžete taky rychle nasadíte Linux OM šablony pomocí [Azure portálu](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Příkaz rychlý přehled

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Podrobné informace

Šablony umožňují vytvářet VMs na Azure pomocí nastavení, které chcete přizpůsobit během prvního spuštění, nastavení, jako je uživatelská jména a názvy hostitelů. V tomto článku jsme jsou spuštění šablonu Azure využití systémem Ubuntu OM spolu s skupinu zabezpečení sítí (NSG) s portem 22 otevřené pro SSH.

Azure šablony správce prostředků jsou JSON soubory, které lze použít pro simple jednorázové úkoly jako spuštění se systémem Ubuntu OM jako hotový v tomto článku.  Azure šablony lze také vytvářet složité Azure konfigurace celé prostředí jako testování zařízením nebo výrobní zásobníku nasazení.

## <a name="create-the-linux-vm"></a>Vytvoření Linux OM

Následující příklad ukazuje, jak chcete zavolat `azure group create` vytvořit skupinu zdrojů a nasazení zabezpečená SSH Linux OM současně pomocí [této šablony správce prostředků Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Myslete na to, ve vaší uvedeném příkladu je potřeba použít názvy, které jsou jedinečné pro prostředí. V tomto příkladu `myResourceGroup` jako název skupiny zdrojů a `myVM` jako název OM.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Výstup by měl vypadat následujícím výstupního bloku:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Tento příklad nasazené OM pomocí `--template-uri` parametr.  Můžete taky stáhnout nebo vytvoření šablony místně a předávat šablony pomocí `--template-file` parametr s cestu k souboru šablony jako argument. Rozhraní příkazového řádku Azure výzvu k zadání parametrů požadovaných šablonou.

## <a name="next-steps"></a>Další kroky

Hledat [Galerie šablon](https://azure.microsoft.com/documentation/templates/) a zjistíte, jaké rámce aplikace pro nasazení Další.
