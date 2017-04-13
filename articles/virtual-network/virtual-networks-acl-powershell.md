<properties
   pageTitle="Jak spravovat seznamy řízení přístupu (ACL) koncové body pomocí prostředí PowerShell"
   description="Naučte se spravovat ACL pomocí prostředí PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Jak spravovat seznamy řízení přístupu (ACL) koncové body pomocí prostředí PowerShell

Můžete vytvořit a spravovat sítě seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell Azure nebo v portálu pro správu. V tomto tématu najdete postupy pro řízení přístupu základním krokům, které lze provést pomocí Powershellu. Seznam rutiny prostředí PowerShell Azure v tématu [Rutiny pro správu Azure](http://go.microsoft.com/fwlink/?LinkId=317721). Další informace o ACL najdete v tématu [Co je v seznamu pro řízení přístupu síti (ACL)?](virtual-networks-acl.md). Pokud chcete spravovat svůj ACL pomocí portálu pro správu, najdete v článku [jak nastavit koncové body do virtuálního počítače](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Správa sítě ACL pomocí prostředí PowerShell Azure

Rutiny prostředí PowerShell Azure můžete použít k vytváření, odebrání a konfigurace (nastavení) sítě seznamy řízení přístupu (ACL). Několik příkladů některé způsoby, jak můžete nakonfigurovat ACL pomocí prostředí PowerShell jsme jste zadali.

K načtení úplný seznam rutiny prostředí PowerShell ACL, použijte některý z následujících akcí:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Vytvoření ACL sítě pomocí pravidla, které umožňují přístup ze vzdáleného podsítě

Následující příklad ukazuje způsob, jak vytvořit nový ACL obsahující pravidla. Koncový bod virtuálního počítače je pak použít tento ACL. Pravidla ACL v následujícím příkladu vám umožní přístup ze vzdáleného podsítě. Pokud chcete vytvořit nové ACL sítě s pravidly povolení vzdáleného podsítě, otevřete Azure PowerShell ISE. Kopírování a vložení skriptu pod konfigurace skriptu s vlastními hodnotami a znovu spusťte skript.

1. Vytvořte nový objekt ACL sítě.

        $acl1 = New-AzureAclConfig

1. Nastavte pravidlo, které umožňují přístup z vzdálené podsítě. V následujícím příkladu je nastavit pravidlo *100* (který má přednost před pravidlo 200 nebo novější) přístupu *10.0.0.0/8* vzdálené podsítě koncový bod virtuálního počítače. Nahraďte hodnoty požadavcích konfigurace. Název "SharePoint ACL konfigurace" by měl být nahradit popisný název, který chcete zavolat toto pravidlo.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Pravidla nehledá opakujte rutinu nahrazení hodnot požadavcích konfigurace. Ujistěte se, pokud chcete změnit pravidlo číslo pořadí tak, aby odrážely pořadí, ve kterém chcete pravidla, která mají být použity. Nižší číslo pravidla přednost před vyšším číslem.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Pak můžete vytvořit nový endpoint (Přidat) nebo nastavení ACL pro existující endpoint (nastavení). V tomto příkladu jsme přidáte nový koncový bod virtuálního počítače s názvem "web" a aktualizovat koncový bod virtuálního počítače s nastavením ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Potom kombinovat rutin a následujícím způsobem. V tomto příkladu kombinované rutiny by vypadal takhle:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Odebrání sítě ACL pravidlo, které umožňují přístup z vzdálené podsítě

Následující příklad ukazuje způsob, jak odebrat pravidlo ACL sítě.  Pokud chcete odebrat pravidlo ACL sítě s pravidly povolení vzdáleného podsítě, otevřete Azure PowerShell ISE. Kopírování a vložení skriptu pod konfigurace skriptu s vlastními hodnotami a znovu spusťte skript.

1. Prvním krokem je zobrazíte objekt sítě ACL pro koncový bod virtuálního počítače. Pak budete odebrat ACL pravidlo. V tomto případě jsme odstraňujete ho podle ID pravidla. ID pravidla 0 tato akce odebere jenom ze seznamu ACL. Neodebere ACL objektu z koncového bodu virtuálního počítače.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Potom musí sítě ACL objekt vyrovnat endpoint virtuálního počítače a aktualizovat virtuální počítač.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Odebrání sítě ACL koncový bod virtuálního počítače

V některých případech se můžete chtít objekt sítě ACL odebrání endpoint virtuálního počítače. Aby je dostala, otevřete Azure PowerShell ISE. Kopírování a vložení skriptu pod konfigurace skriptu s vlastními hodnotami a znovu spusťte skript.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Další kroky

[Co je v seznamu pro řízení přístupu síti (ACL)?](virtual-networks-acl.md)
