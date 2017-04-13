<properties
    pageTitle="Nakonfigurovat integraci Azure klíčové trezoru pro systém SQL Server na Azure VMs (klasický)"
    description="Zjistěte, jak můžete automatizovat konfigurace systému SQL Server šifrování pro použití s Azure klíč trezoru. Toto téma vysvětluje, jak používat Azure klíč trezoru integrace se serverem SQL Server virtuálních počítačích vytvoření v modelu klasické nasazení."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Nakonfigurovat integraci Azure klíčové trezoru pro systém SQL Server na Azure VMs (klasický)

> [AZURE.SELECTOR]
- [Správce prostředků](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasický](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Základní informace
Existuje více funkce šifrování SQL serveru, jako jsou [průhledné šifrování (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sloupec úrovně šifrování (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx)a [zálohování šifrování](https://msdn.microsoft.com/library/dn449489.aspx). Tyto formuláře šifrování vyžadují, abyste spravovat a ukládat klíče, který používáte pro šifrování. Služby Azure klíč trezoru (AKV) slouží ke zlepšení zabezpečení a správa klávesy v umístění, zabezpečené a snadno dostupné. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje serveru SQL Server pomocí klávesy z trezoru klíč Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Pokud používáte systém SQL Server s místního počítače, je potřeba [postup, jak lze získat přístup Azure klíč trezoru z počítače místního serveru SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale pro systém SQL Server v Azure VMs, můžete ušetřit čas používáním funkce *Integrace trezoru klíč Azure* . S několika rutiny prostředí PowerShell Azure tuto funkci povolit můžete automatizovat konfigurace umožňující OM SQL pro přístup k trezoru klíče.

Při zapnuté funkci tuto funkci, ho automaticky instalace konektoru SQL serveru, nakonfiguruje poskytovatele EKM pro přístup k Azure klíč trezoru a vytvoří pověření, která umožňuje přístup k trezoru. Pokud dívali kroků v tématu dokumentace výše uvedených místního uvidíte, že tato funkce umožňuje automatizovat kroky 2 a 3. Jediné, co by ještě musíte udělat ruční je vytvořit klíčové trezoru a klíče. V ní celý instalaci systému SQL OM automatické. Po dokončení tohoto nastavení této funkce můžete provést příkazů T-SQL s cílem zahájit šifrování databáze nebo zálohy běžným způsobem.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Nakonfigurovat integraci AKV
Použití Powershellu ke nakonfigurovat integraci trezoru klíč Azure. Následující části obsahují přehled požadované parametry a potom ukázkový skript Powershellu.

### <a name="install-the-sql-server-iaas-extension"></a>Instalace SQL serveru IaaS rozšíření

Nejdřív si [nainstalovat SQL Server IaaS rozšíření](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Princip vstupních parametrů
V následující tabulce jsou uvedeny parametry potřebné ke spuštění skript Powershellu v následující části.

|Parametr|Popis|Příklad|
|---|---|---|
|**$akvURL**|**Adresa URL klíčové trezoru**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Hlavní název služby**|"fde2b411 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Tajná jistinu služby**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Název pověření**: integrace AKV vytvoří pověření v rámci SQL serveru, umožní OM mít taky přístup k klíčové trezoru. Vyberte název pro tento přihlašovací údaje.|"mycred1"|
|**$vmName**|**Virtuální počítač název**: název dříve vytvořené OM SQL.|"myvmname"|
|**$serviceName**|**Název služby**: název cloudové služby, který bude přidružený OM SQL.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Povolení integrace AKV pomocí prostředí PowerShell
Rutinu **New-AzureVMSqlServerKeyVaultCredentialConfig** vytvoří pro funkci Azure klíč trezoru integrace konfigurace objekt. **Nastavení AzureVMSqlServerExtension** nakonfiguruje integraci s parametrem **KeyVaultCredentialSettings** . Podle těchto kroků ukazují, jak používat tyto příkazy.

1. V prostředí PowerShell Azure nejdříve nakonfigurujte vstupní parametry specifické hodnoty podle popisu v předchozí části tohoto tématu. Tento skript je znázorněn příklad.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Potom použijte tento skript ke konfiguraci a povolení integrace AKV.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Rozšíření SQL IaaS agenta bude aktualizován toto nové konfigurace OM SQL.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
