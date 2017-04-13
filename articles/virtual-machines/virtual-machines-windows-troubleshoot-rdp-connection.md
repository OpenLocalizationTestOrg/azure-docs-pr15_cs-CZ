<properties
    pageTitle="Nelze RDP Azure angličtině | Microsoft Azure"
    description="Poradce při potížích s, když se nemůžete připojit k počítači se systémem Windows virtuální v Azure pomocí vzdálené plochy"
    keywords="Vzdálené plochy chyby, připojení ke vzdálené ploše chyba se nemůže připojit k OM, vzdálené plochy Poradce při potížích s"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Poradce při potížích s připojení ke vzdálené ploše Azure virtuálního počítače

Vzdálená plocha (RDP Protocol) připojení k serveru s Windows Azure virtuálního počítače (OM) může selhat různých důvodů je necháte nelze získat přístup k vaší OM. Se službou Vzdálená plocha na OM, síťové připojení nebo připojení ke vzdálené ploše klienta na hostitelském počítači může být problém. Tento článek vás provede některé nejčastější metody vyřešit problémy s připojením RDP. 

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [webu MSDN Azure a přetečení zásobníku fóra](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat případ Azure podpory. Přejděte na [Azure podporují webu](https://azure.microsoft.com/support/options/) a vyberte **Získat podporu**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Rychlé kroky
Po každém kroku Poradce při potížích zkuste se znovu připojit angličtině:

1. Obnovte konfigurace vzdálené plochy.
2. Kontrola skupiny zabezpečení sítě pravidla / Cloud Services koncové body.
3. Prohlédněte si OM konzoly protokoly.
4. Kontrola stavu OM zdroje.
5. Resetování hesla OM.
6. Restartujte svůj OM.
7. Přeinstalujte vaší OM.

Pokud potřebujete podrobnější kroky a vysvětlení pokračujte ve čtení.

> [AZURE.TIP] Pokud tlačítko **Připojit** pro vaše OM se zobrazuje šedě, na portálu a nejste připojení k Azure přes připojení [VPN k webu](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) nebo [Express směrování](../expressroute/expressroute-introduction.md) , potřebujete k vytvoření a přiřazení vaší OM veřejnou IP adresu, než budete moct použít RDP. Další informace o [veřejných IP adres v Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Způsoby, jak řešit problémy RDP
Poradce při potížích VMs vytvořené pomocí Správce prostředků nasazení modelu jedním z těchto způsobů:

- [Azure portál](#using-the-azure-portal) - skvělé, když potřebujete rychle obnovit RDP konfiguraci nebo uživatelské přihlašovací údaje a nemáte nainstalované Azure nástroje.
- [Azure PowerShell](#using-azure-powershell) – Pokud jsou pro vás pohodlnější s výzvou Powershellu, rychle obnovit RDP konfiguraci nebo uživatelské přihlašovací údaje pomocí rutin prostředí PowerShell Azure.

Můžete zjistit taky kroky k řešení potíží s VMs vytvořené pomocí [klasického nasazení modelu](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Poradce při potížích s pomocí portálu Azure
Po každém kroku Poradce při potížích připojte se k vaší OM znovu. Pokud se stále nemůžete připojit, zkuste dalším krokem.

1. **Obnovení připojení RDP**. Tento krok Poradce při potížích obnoví konfiguraci RDP při připojení ke vzdálené jsou zakázány nebo pravidel brány Windows Firewall blokuje RDP, například.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **vytvořit nové heslo** . Nastavení **režimu** obnovit **pouze konfiguraci** a potom klikněte na tlačítko **Aktualizovat** :

    ![Obnovení konfigurace RDP na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Pravidla ověření skupiny zabezpečení sítě**. Tento krok Poradce při potížích ověří, že máte pravidlo ve skupině zabezpečení sítě pro povolení RDP komunikace. Výchozí port pro RDP je TCP port 3389. Pravidlo pro povolení komunikace RDP nemusí být vytvořeny automaticky při vytváření vašeho OM.

    Vyberte svůj OM Azure portálu. Klikněte na **síť rozhraní** z podokna nastavení.

    ![Zobrazení síťových rozhraní pro OM Azure portálu](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Vyberte síťové rozhraní ze seznamu (je obvykle jenom):

    ![Vyberte síťové na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Vyberte **skupinu zabezpečení sítě** zobrazíte skupiny zabezpečení sítě přidružené rozhraní sítě:

    ![Vyberte skupinu zabezpečení sítě, na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Ověřte pravidlo pro příchozí připojení, která umožňuje RDP přenosy na TCP port 3389. Následující příklad ukazuje platné zabezpečení pravidlo, které umožňuje RDP přenosy. Zobrazí se `Service` a `Action` jsou správně nakonfigurované:

    ![Ověření pravidla RDP NSG na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Pokud nemáte pravidla, která umožňuje RDP přenos [vytvořit pravidlo, skupiny zabezpečení sítě](virtual-machines-windows-nsg-quickstart-portal.md). Počkejte, až TCP port 3389.

3. **Diagnostika spouštěcí OM revize**. Tento krok Poradce při potížích kontroluje protokolech OM konzoly a zjistit, pokud OM hlásí chybu. Ne všechny VMs mít spouštěcí diagnostiky povoleno, tak tento poradce při potížích krok mohou být nepovinné.
    
    Konkrétní řešení potíží se nad rámec v tomto článku, ale mohou poukazovat širší problém, který má vliv na RDP připojení. Další informace o zobrazení protokolů konzoly a snímek OM najdete v tématu [Spuštění Diagnostika pro VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Kontrola stavu OM zdroje**. Tento krok Poradce při potížích ověří, že nejsou žádné známé problémy s Azure platformy, které mohou ovlivnit připojení bude v angličtině.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **zdroje stavu** . Správný OM sestav které jsou **k dispozici**:

    ![Kontrola stavu OM zdroje na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Obnovení přihlašovací údaje uživatele**. Tento krok Poradce při potížích obnoví heslo místního účtu správce se jistí nebo zapomněli pověření.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **vytvořit nové heslo** . Ujistěte se, že je **režim** nastaven resetovat **heslo** a potom zadejte svoje uživatelské jméno a nové heslo. Nakonec klikněte na tlačítko **Aktualizovat** :

    ![Obnovení přihlašovací údaje uživatele na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Restartujte svůj OM**. Tento krok Poradce při potížích umí opravit všechny základní potížích, se kterými s OM samotné.

    Vyberte svůj OM Azure portálu a klikněte na kartě **Přehled** . Klikněte na tlačítko **Restartujte** :

    ![Restartujte OM na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Přeinstalujte vaší OM**. Tento krok Poradce při potížích znovu nasadí OM jiné hostiteli v rámci Azure opravit základní platformě nebo problémům se sítí.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **přeinstalujte** a potom klikněte na **přeinstalujte**:

    ![Přeinstalujte OM na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Po dokončení operace dojde ke ztrátě dat dočasné disku a dynamické IP adresy, které jsou přidružené k OM se automaticky aktualizují.

Pokud se stále dochází RDP problémy, můžete [Otevřít žádost o podporu](https://azure.microsoft.com/support/options/) nebo přečtěte si [podrobnější RDP Poradce při potížích koncepty a kroky](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Poradce při potížích s pomocí prostředí PowerShell Azure
Pokud jste to ještě neudělali, [Nainstalujte a nakonfigurujte nejnovější Azure Powershellu](../powershell-install-configure.md).

Následující příklady použití proměnných, jako `myResourceGroup`, `myVM`, a `myVMAccessExtension`. Vlastní hodnoty nahraďte tyto názvech proměnných a umístění.

> [AZURE.NOTE] Vytvoření nového přihlašovací údaje uživatele a konfigurace RDP pomocí rutiny prostředí PowerShell [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) . V následujících příkladech `myVMAccessExtension` je název, který je zadat jako součást procesu. Pokud jste dříve pracovali VMAccessAgent, můžete se název existující rozšíření pomocí `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` a zkontrolujte vlastnosti OM. Pokud chcete zobrazit název, vyhledejte v části "Rozšíření" výstupu.

Po každém kroku Poradce při potížích připojte se k vaší OM znovu. Pokud se stále nemůžete připojit, zkuste dalším krokem.

1. **Obnovení připojení RDP**. Tento krok Poradce při potížích obnoví konfiguraci RDP při připojení ke vzdálené jsou zakázány nebo pravidel brány Windows Firewall blokuje RDP, například.

    Následující příklad obnoví připojení RDP na OM s názvem `myVM` v `WestUS` umístění a ve skupině zdroje s názvem `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Pravidla ověření skupiny zabezpečení sítě**. Tento krok Poradce při potížích ověří, že máte pravidlo ve skupině zabezpečení sítě pro povolení RDP komunikace. Výchozí port pro RDP je TCP port 3389. Pravidlo pro povolení komunikace RDP nemusí být vytvořeny automaticky při vytváření vašeho OM.

    Nejdřív přiřadit všechna data konfigurace pro vaší skupinu zabezpečení sítě `$rules` proměnné. Následující příklad získá informace o skupině zabezpečení sítě s názvem `myNetworkSecurityGroup` ve skupině zdroje s názvem `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Teď zobrazte pravidla, která jsou nakonfigurovány pro tuto skupinu zabezpečení sítě. Ověřte pravidlo umožňuje port TCP 3389 pro příchozí připojení následujícím způsobem:

    ```powershell
    $rules.SecurityRules
    ```

    Následující příklad ukazuje platné zabezpečení pravidlo, které umožňuje RDP přenosy. Zobrazí se `Protocol`, `DestinationPortRange`, `Access`, a `Direction` jsou správně nakonfigurované:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Pokud nemáte pravidla, která umožňuje RDP přenos [vytvořit pravidlo, skupiny zabezpečení sítě](virtual-machines-windows-nsg-quickstart-powershell.md). Počkejte, až TCP port 3389.

3. **Obnovení přihlašovací údaje uživatele**. Tento krok Poradce při potížích obnoví heslo místního účtu správce, který určíte, když nejste jisti, nebo zapomněli pověření.

    Nejdřív zadejte uživatelské jméno a nové heslo přiřazením přihlašovací údaje pro `$cred` proměnné takto:

    ```powershell
    $cred=Get-Credential
    ```

    Teď aktualizujte údaje ve vaší OM. Následující příklad aktualizuje přihlašovací údaje na OM s názvem `myVM` v `WestUS` umístění a ve skupině zdroje s názvem `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Restartujte svůj OM**. Tento krok Poradce při potížích umí opravit všechny základní potížích, se kterými s OM samotné.

    Následující příklad restartuje OM s názvem `myVM` ve skupině zdroje s názvem `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Přeinstalujte vaší OM**. Tento krok Poradce při potížích znovu nasadí OM jiné hostiteli v rámci Azure opravit základní platformě nebo problémům se sítí.

    Následující příklad znovu nasadí OM s názvem `myVM` v `WestUS` umístění a ve skupině zdroje s názvem `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Pokud se stále dochází RDP problémy, můžete [Otevřít žádost o podporu](https://azure.microsoft.com/support/options/) nebo přečtěte si [podrobnější RDP Poradce při potížích koncepty a kroky](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Poradce při potížích s VMs vytvořené pomocí klasického nasazení modelu

Po každém kroku Poradce při potížích zkuste se znovu připojit angličtině.

1. **Obnovení připojení RDP**. Tento krok Poradce při potížích obnoví konfiguraci RDP při připojení ke vzdálené jsou zakázány nebo pravidel brány Windows Firewall blokuje RDP, například.

    Vyberte svůj OM Azure portálu. Klikněte **... Další** tlačítko a potom klikněte na **Obnovit vzdáleného přístupu**:

    ![Obnovení konfigurace RDP na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Koncové body ověření Cloudovým službám**. Tento krok Poradce při potížích ověří, budete mít koncové body na Cloud Services pro povolení RDP komunikace. Výchozí port pro RDP je TCP port 3389. Pravidlo pro povolení komunikace RDP nemusí být vytvořeny automaticky při vytváření vašeho OM.

    Vyberte svůj OM Azure portálu. Klikněte na tlačítko **koncové body** zobrazíte koncové body konfigurovaná pro vaše OM. Ověřte, že koncové body neexistují přenosy RDP na TCP port 3389.
    
    Následující příklad ukazuje platné koncové body, které povolují RDP přenosy:

    ![Ověření koncové body cloudové služby na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Pokud nemáte koncový bod, který umožňuje RDP přenos [Vytvoření koncového bodu Cloudovým službám](virtual-machines-windows-classic-setup-endpoints.md). Povolte TCP soukromé port 3389.

3. **Diagnostika spouštěcí OM revize**. Tento krok Poradce při potížích kontroluje protokolech OM konzoly a zjistit, pokud OM hlásí chybu. Ne všechny VMs mít spouštěcí diagnostiky povoleno, tak tento poradce při potížích krok mohou být nepovinné.
    
    Konkrétní řešení potíží se nad rámec v tomto článku, ale mohou poukazovat širší problém, který má vliv na RDP připojení. Další informace o zobrazení protokolů konzoly a snímek OM najdete v tématu [Spuštění Diagnostika pro VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Kontrola stavu OM zdroje**. Tento krok Poradce při potížích ověří, že nejsou žádné známé problémy s Azure platformy, které mohou ovlivnit připojení bude v angličtině.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **Zdroje stavu** . Správný OM sestav které jsou **k dispozici**:

    ![Kontrola stavu OM zdroje na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Obnovení přihlašovací údaje uživatele**. Tento krok Poradce při potížích obnoví heslo místního účtu správce, který určíte, když nejste jisti, nebo jste zapomněli pověření.

    Vyberte svůj OM Azure portálu. Posuňte se dolů v podokně nastavení do části **Podpora + Poradce při potížích** v dolní části seznamu. Klikněte na tlačítko **vytvořit nové heslo** . Zadejte svoje uživatelské jméno a nové heslo. Nakonec klikněte na tlačítko **Uložit** :

    ![Obnovení přihlašovací údaje uživatele na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Restartujte svůj OM**. Tento krok Poradce při potížích umí opravit všechny základní potížích, se kterými s OM samotné.

    Vyberte svůj OM Azure portálu a klikněte na kartě **Přehled** . Klikněte na tlačítko **Restartujte** :

    ![Restartujte OM na portálu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Pokud se stále dochází RDP problémy, můžete [Otevřít žádost o podporu](https://azure.microsoft.com/support/options/) nebo přečtěte si [podrobnější RDP Poradce při potížích koncepty a kroky](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Poradce při potížích s konkrétní chyby RDP
Můžete narazit konkrétní chybové zprávy při pokusu o připojení k OM prostřednictvím RDP. Zde jsou nejčastější chybové zprávy:

- [Relace byla odpojena, protože neexistují žádné vzdálené plochy licenci servery k dispozici licence](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Vzdálená plocha nenašli počítač "název"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Ověřování došlo k chybě. Místní autorita zabezpečení nelze kontaktovat](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Zabezpečení systému Windows chyby: nefungují svoje přihlašovací údaje](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Tento počítač nemůže připojit k počítači](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Další zdroje informací
Žádné z těchto chyb došlo k a pořád se nemůžete připojit k OM prostřednictvím vzdálené plochy, najdete v tématu podrobný [Průvodce pro připojení ke vzdálené ploše pro řešení](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure balíček diagnostics IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Návody na řešení potíží při přístupu k aplikacím na virtuálního počítače, najdete v článku [Poradce při potížích s přístup k spuštění aplikace v Azure OM](virtual-machines-linux-troubleshoot-app-connection.md).
- Pokud máte problémy se připojujete k Linux OM v Azure pomocí zabezpečené prostředí (SSH), přečtěte si článek [Poradce při potížích s SSH připojení k Linux OM v Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).
