<properties
 pageTitle="Správa uzlů výpočetním clusteru HPC Pack | Microsoft Azure"
 description="Další informace o nástrojích skript Powershellu k přidání, odebrání, spustit a zastavit uzly výpočetním clusteru HPC Pack v Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Správa číslo a dostupnosti uzlů výpočetním clusteru HPC Pack v Azure

Pokud jste vytvořili v Azure VMs HPC Pack obrázku, můžete způsoby, jak snadno přidat, odebrat, spusťte (poskytování) nebo ukončit (identitách) celá řada výpočetním uzel VMs clusteru. Provádět tyto úkoly spouštěly skripty Azure Powershellu, které jsou instalovány na uzel hlavy OM. Tyto skripty umožňují určit počet a dostupnosti zdrojů clusteru HPC Pack tak řízení nákladů.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Zjistit předpoklady pro

* **HPC Pack obrázku v Azure VMs** – vytvoření HPC Pack obrázku v modelu klasické nasazení pomocí aspoň HPC Pack 2012 R2 aktualizace 1. Například automatizovat nasazení pomocí aktuální obrázek HPC Pack OM v Azure Marketplace a skript Powershellu Azure. Informace a požadavky najdete v článku [Vytvoření clusteru HPC se skriptem HPC Pack IaaS nasazení](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Po nasazení, najdete na uzel skripty pro správu v % CCP\_složky Koš % Domů na uzel hlavy. Je třeba všech skripty spustit jako správce.

* **Azure publikovat nastavení soubor Správa certifikátů** - musíte udělat něco z následujícího na uzel hlavy:

    * **Soubor nastavení importu Azure publikovat**. K tomuto účelu, spusťte tyto rutiny prostředí PowerShell Azure na uzel hlavy:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Konfigurace certifikátu Azure správy na uzel hlavy**. Pokud máte soubor .cer, importujte ho do úložiště certifikátů CurrentUser\My a spusťte následující rutinu Powershellu Azure pro prostředí Azure (AzureCloud nebo AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Přidání výpočetní uzly VMs

Přidat výpočet uzly se skriptem **HpcIaaSNode.ps1 přidat** .

### <a name="syntax"></a>Syntaxe
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametry

* **Název_služby** – název cloudových služeb, které výpočetní uzly VMs se přidají do.

* **ImageName** - Azure OM obrázek název, který lze získat prostřednictvím portálu pro Azure klasické nebo rutiny prostředí PowerShell Azure **Get-AzureVMImage**. Obrázek musí splňovat následující kritéria:

    1. Musí být nainstalovaný operační systém Windows.

    2. HPC Pack musí být nainstalovaný v roli uzel výpočetním.

    3. Obrázek musí být privátní obrázku v kategorii uživatele není veřejné Azure OM obrázku.

* **Počet** - počet výpočetní uzly VMs chcete přidat.

* **InstanceSize** - velikost výpočetní uzly VMs.

* **DomainUserName** – uživatelské jméno domény, který se používá ke spojení nové VMs na doménu.

* **DomainUserPassword** - heslo uživatele domény.

* **NodeNameSeries** (volitelné) Pokud-pojmenování vzor pro uzly výpočetním. Formát musí být &lt; *kořenové\_název*&gt;&lt;*Start\_číslo*&gt;%. Například MyCN % 10 % označuje a řadě jmen uzel výpočetním počínaje MyCN11. Pokud není zadán, skript používá nakonfigurované uzel pojmenování řad HPC obrázku.

### <a name="example"></a>Příklad

Následující příklad přidává 20 velikost velkých výpočetní uzly VMs do cloudové služby *hpcservice1*, založené na obrázek OM *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Odebrat výpočet VMs uzel

Odebrat výpočet uzly se skriptem **HpcIaaSNode.ps1 odebrat** .

### <a name="syntax"></a>Syntaxe

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry

* **Název** - názvy uzlech má být odebrán. Zástupné znaky jsou podporovány. Název je název parametru nastavení. Nelze zadat **název** a **uzel** parametry.

* **Uzel** – HpcNode objektu pro uzly má být odebrán, který můžete získat prostřednictvím Powershellu HPC rutiny [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Nastavení název parametru je uzel. Nelze zadat **název** a **uzel** parametry.

* **DeleteVHD** (volitelné) – nastavení odstranit přidružené disků pro VMs, které jsou odebrány.

* **Platnost** (volitelné) – nastavení vynutit HPC uzlů v režimu offline před odebráním.

* **Potvrďte** (volitelné) - výzvy k potvrzení před spuštěním příkazu.

* **WhatIf** – nastavení k tomu, co by příčiny spuštění příkazu bez skutečně spuštění příkazu.

### <a name="example"></a>Příklad

Následující příklad přinutí offline uzly se jmény na začátku *HPCNode-CN -* a jejich odstraní uzly a jeho přidružený discích.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Spusťte výpočetním uzel VMs

Zahájení výpočet uzly se skriptem **Start HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxe

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametry

* **Název** - názvy uzlů jej lze spustit. Zástupné znaky jsou podporovány. Název je název parametru nastavení. Nelze zadat **název** a **uzel** parametry.

* **Uzel**– HpcNode objektu pro uzly jej lze spustit, který můžete získat prostřednictvím Powershellu HPC rutiny [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Nastavení název parametru je uzel. Nelze zadat **název** a **uzel** parametry.

### <a name="example"></a>Příklad

Následující příklad spustí uzly se jmény na začátku *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Ukončení výpočetní uzly VMs

Zastavit výpočet uzly se skriptem **Stop HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxe

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry


* **Název**- názvy uzly clusteru, aby ji ukončit. Zástupné znaky jsou podporovány. Název je název parametru nastavení. Nelze zadat **název** a **uzel** parametry.

* **Uzel** - HpcNode objektu pro uzly ukončení, které můžete získat prostřednictvím Powershellu HPC rutiny [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Nastavení název parametru je uzel. Nelze zadat **název** a **uzel** parametry.

* **Platnost** (volitelné) – nastavení vynutit HPC uzlů v režimu offline před ukončením je.

### <a name="example"></a>Příklad

Následující příklad vynucuje offline uzly se jmény na začátku *HPCNode-CN -* a pak ukončí uzlů.

Zastavit HPCIaaSNode.ps1 – název HPCNodeCN-*-platnost

## <a name="next-steps"></a>Další kroky

* Pokud budete potřebovat způsob, jak automaticky zvětšit nebo zmenšit uzlech podle aktuální zatížení projektů a úkolů na clusteru, přečtěte si článek [automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack v Azure podle pracovní zátěž obrázku](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
