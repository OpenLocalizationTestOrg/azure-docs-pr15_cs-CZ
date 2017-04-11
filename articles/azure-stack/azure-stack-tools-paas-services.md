<properties
    pageTitle="Nástroje a PaaS služby Azure zásobníku | Microsoft Azure"
    description="Zjistěte, jak začít s PaaS služby Azure vrstvě."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Nástroje pro Azure zásobníku

Stáhněte si nástroje píše níže. Pokud chcete být upozorňováni na nové služby a nástroje, postupujte podle #AzureStack na Twitter.

## <a name="template-tools"></a>Šablona nástroje

### <a name="azure-stack-github-templates"></a>Azure zásobníku Github šablony
Prozkoumejte rostoucí kolekci [Azure zásobníku GitHub šablon](https://github.com/Azure/AzureStack-QuickStart-Templates). Stejně jako [Azure](https://github.com/Azure/azure-quickstart-templates)tyto šablony "Rychlý Start" Správce prostředků Azure cíl vám usnadní zahájení práce jednoduché stavebních bloků a uvádí příklady chtít nasazení Microsoft Azure zásobníku Technical Preview doklad z koncept prostředí. Součástí příkladů první strany pracovní zátěž pro [ad bez ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014 – bez ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013 – bez ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), [101 jednoduché windows OM](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm)zachce i několik jednoduchých 101 šablon.


### <a name="marketplace-item-packaging-tool-and-sample"></a>Nástroj balení položku Marketplace a ukázkové
[Stáhněte si a použijte nástroj balení](http://www.aka.ms/azurestackmarketplaceitem) k vytvoření marketplace položek pro vlastní šablony k přidání do zásobníku Azure marketplace. Pokyny k vytvoření položky marketplace a jeho Příprava k vaší tenantům najdete v [položce vytvořit Marketplace](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Nástroje pro vývojáře


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Použití Visual Studia a Azure zásobníku TP2 na MAS CON01 virtuálního počítače
Pokud chcete pracovat se šablonami zásobníku Azure pomocí aplikace Visual Studio v konzole OM, musíte nainstalovat správný verzích požadované nástroje. Pomocí následujícího postupu instalace podporovaných verzí TP2.

1. Přihlaste se k počítači virtuální MAS CON01 pomocí přihlašovacích údajů azurestack\azurestackadmin pomocí připojení ke vzdálené ploše.
2. Nainstalujte a spusťte webové platformy.
3. Vyhledat a nainstalovat **Visual Studio komunity 2015 s Microsoft Azure SDK - 2.9.5**.
4. Pomocí funkce **Přidat nebo odebrat programy**, odinstalujte **Služeb Microsoft Azure PowerShell (Sept)** , který získá instalován jako součást sady SDK.
5. Otevření webové platformy.
6. Vyhledat a nainstalovat **Microsoft Azure PowerShell - 2 Technical Preview Azure vrstvě**. 
7. Restartujte počítač virtuální MAS CON01.
8. Přihlaste se k počítači virtuální MAS CON01 pomocí přihlašovacích údajů azurestack\azurestackadmin pomocí připojení ke vzdálené ploše.
9. Otevřete aplikaci Visual Studio a ověřit, můžete připojení k vrstvě Azure prostředí, získat šablony a tak dále. 

### <a name="azure-powershell-sdk"></a>Azure prostředí PowerShell SDK
Azure Powershellu je moduly, které poskytuje rutin ke správě Azure a zásobníku Azure pomocí prostředí Windows PowerShell. Pomocí rutin vytvářet, otestovat, nasazení a správu řešení a služeb Doručená prostřednictvím platformy Azure vrstvě.
[Stažení prostředí PowerShell SDK Azure](http://aka.ms/azStackPsh) a [nainstalujte prostředí PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure křížové platformu příkazového řádku rozhraní
Rychle nainstalujte Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku) použít sadu otevřít zdroj založený na prostředí příkazy pro vytváření a Správa zdrojů v Microsoft Azure vrstvě.

[Stáhněte si Windows rozhraní příkazového řádku](http://aka.ms/azstack-windows-cli)

[Stáhněte si Mac rozhraní příkazového řádku](http://aka.ms/azstack-linux-cli)

[Stáhněte si Linux rozhraní příkazového řádku](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Pokud jste na počítači Mac a Linux, otevřete taky rozhraní příkazového řádku pomocí příkazu`npm install -g azure-cli@0.9.11`</br>
> + Pokud dostáváte problémů ověření certifikátů, příkaz`set NODE_TLS_REJECT_UNAUTHORIZED=0`
