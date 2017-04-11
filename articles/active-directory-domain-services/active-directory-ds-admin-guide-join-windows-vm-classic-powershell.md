<properties
    pageTitle="Služby Azure Active Directory Domain Services: Příručka pro správu | Microsoft Azure"
    description="Připojit se ke virtuálního počítače Windows do spravované domény pomocí prostředí PowerShell Azure a klasické nasazení modelu."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Spojení virtuálního počítače v systému Windows Server a spravovaných domény pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure klasické portálu - systému Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [Prostředí PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce zdrojů a klasické](../resource-manager-deployment-model.md). Tento článek se věnuje pomocí klasické nasazení modelu. Služby Domain Azure AD aktuálně nepodporuje modelu správce prostředků.

Tento postup předvedení vlastní sadu dostupných příkazů Azure Powershellu, které vytvoření a nastavení serveru s Windows Azure virtuálního počítače pomocí přístup stavební blok. Tento postup vám pomůže vytvořit serveru s Windows Azure virtuálního počítače a připojit k doméně spravovaných Azure AD Domain Services.

Tento postup podle doplnění-v – prázdné přístup k vytvoření sady příkazů Azure Powershellu. Tento postup může být užitečné, když začínáte prostředí PowerShell nebo budete chtít zjistit, co hodnoty můžete určit pro úspěšné konfiguraci. Zkušení uživatelé prostředí PowerShell můžete pořídit příkazy a nahradit své vlastní hodnoty pro proměnné (řádky začínající "Kč").

Pokud jste to ještě neudělali, postupujte podle pokynů v [tom, jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) nainstalovat prostředí PowerShell Azure ve vašem počítači. Potom otevřete příkazový prostředí Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Přidání účtu

1. Na příkazovém řádku prostředí PowerShell zadejte **Přidat AzureAccount** a klepněte na **Enter**.
2. Zadejte e-mailovou adresu přidruženou k vašemu předplatnému Azure a klikněte na **pokračovat**.
3. Zadejte heslo k vašemu účtu.
4. Klikněte na **přihlásit**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Nastavení účtu úložiště a předplatného

Nastavení účtu úložiště a Azure předplatného spuštěním tyto příkazy na příkazovém řádku prostředí Windows PowerShell. Nahrazení všechno, co do uvozovek, včetně < a > znaků s správná jména.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Název správné předplatného dá dostat z vlastnost SubscriptionName výstupu příkazu **Get-AzureSubscription** . Název účtu úložiště správné dá dostat z vlastnosti popisek výstupu příkazu **Get-AzureStorageAccount** po spuštění příkazu **Select AzureSubscription** .


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Krok 3: Podrobný návod - zřízení virtuálního počítače a připojit ji do spravované domény
Tady je odpovídající Azure PowerShell command vytvoření tohoto virtuálního počítače s prázdné řádky mezi každý blok pro snazší čitelnost.

Zadejte informace o zřídit virtuální počítač Windows.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

InstanceSize hodnoty D-Pošta nebo G řady virtuálních počítačích, najdete v článku [virtuální počítač a velikosti cloudové služby Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Zadejte informace o spravovaných domény.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Zadejte název cloudovou službu.

    $svcname="Contoso100-test"

Zadejte název virtuální sítě, ke které mají OM připojen. Zajistěte, aby byl doménu spravovanou AAD DS dostupné v tomto virtuální sítě.

    $vnetname="MyPreviewVnet"

Vyberte obrázek OM chcete použít pro zřízení OM.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurace OM – název sady OM, instance velikost a obrázek se nemusí používat.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Získejte oprávnění místního správce pro OM. Zvolte bezpečné místní heslo správce.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Získejte oprávnění pro uživatelský účet, které patří do skupiny "AAD Datacentrum správci" ke spojení OM a spravovaných domény. Nelze zadat název domény – například v našem příkladu, můžeme určit "Jan" uživatelské jméno.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurace OM - určete požadavek spojení domény a požadovaných pověření.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Nastavte podsítě OM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Volitelné: Přejděte na IP adresu domény. Pokud jste nastavili IP adresy Azure AD Domain Services spravovaných doménu serverů DNS pro virtuální sítě, tento krok není povinný.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Teď by Windows OM doméně.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skript zřízení OM Windows a automaticky připojovat se k doméně spravovaných AAD Domain Services
Tuto sadu příkazového prostředí PowerShell vytvoří virtuální počítač řádku obchodní serveru, který:

- Používá obrázky Windows serveru 2012 R2 Datacentra.
- Je další malé virtuálního počítače.
- Má název společnosti contoso testu.
- Automaticky domény připojen k doménu spravovaný contoso100.
- Je přidán do stejné virtuální sítě jako spravovanou domény.

Tady je celkového vzorku skript, který chcete vytvořit virtuální počítač Windows a automaticky připojit na doménu spravovaný Azure AD Domain Services.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Související obsah
- [Služby Domain Azure AD - příručce Začínáme](./active-directory-ds-getting-started.md)

- [Spravovat spravovaná domain Azure AD Domain Services](./active-directory-ds-admin-guide-administer-domain.md)
