<properties 
    pageTitle="LOB aplikace testovacím prostředí | Microsoft Azure" 
    description="Naučte se vytvářet založené na webu řádek podnikové aplikaci v hybridním prostředí cloudu pro IT pro nebo vývoj testování třetí strana." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Nastavení webové aplikace založené na LOB v cloudu hybridní pro účely testování

Toto téma vás provede jednotlivými kroky vytvoření simulovaný hybridního prostředí cloudu pro účely testování řádek webové aplikace business (LOB) umístěná v Microsoft Azure. Tady je Výsledná konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Toto nastavení se skládá ze:

- Simulovaný místní síti hostované v Azure (VNet testovací podsíť).
- Křížové místní virtuální síti hostované v Azure (TestVNET).
- Připojení k síti VPN VNet VNet.
- Webový server LOB, SQL server a vedlejší domény řadiče domény v síti virtuální TestVNET.

Zajišťuje základna a běžných počátečního bodu, kde můžete provést tyto akce:

- Můžete vyvíjet a otestujte LOB aplikací na informace služby Internetové informační hostovaný jiným back-end databáze SQL serveru 2014 v Azure.
- Dělat testy z této simulovaný hybridní cloudové IT zátěží na projektu.

Existují tři hlavní fáze pro nastavení toto hybridní cloudu testovacím prostředí:

1.  Nastavte si simulovaný hybridního prostředí cloudu.
2.  Konfigurace systému SQL server (SQL1).
3.  Konfigurace serveru LOB (LOB1).

Tento pracovní zátěž vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo Visual Studio, najdete v článku [měsíční Azure dobru Visual Studio účastníky](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Příklad výrobní LOB aplikace umístěná v Azure najdete v článku Přehled architektura **aplikací Line of business** na [Microsoft Software architektura diagramů, modrotisků](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Fáze 1: Nastavení simulovaný hybridního prostředí cloudu

Vytvoření [simulovaného hybridní cloudu testovacím prostředí](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Protože tento testovacím prostředí nevyžaduje stavu Apl1 serveru v síti Corpnet podsítě, můžete ho vypnout nyní.

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Fáze 2: Konfigurace systému SQL server (SQL1)

Z portálu Microsoft Azure spusťte počítač DC2 v případě potřeby.

Dále vytvořte virtuálního počítače pro SQL1 s tyto příkazy na příkazovém řádku prostředí PowerShell Azure ve vašem počítači. Před spuštěním tyto příkazy, vyplňte proměnnými hodnotami a odeberte < a > znaky.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Připojení k SQL1 pomocí portálu Azure pomocí místního účtu správce systému SQL1.

Pak konfigurace pravidel brány Windows Firewall umožňuje testování základní spojení a provozu SQL serveru. Na úrovni správce prostředí Windows PowerShell příkazovém řádku SQL1 spusťte tyto příkazy.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi z adresy IP 192.168.0.4.

Dále přidejte disku navíc dat na SQL1 jako nový hlasitost s písmenem F:.

1.  V levém podokně Správce serveru klikněte na **soubor a úložiště služby**a potom klikněte na **disku**.
2.  V podokně obsah ve skupině **disků** klikněte na **disku 2** (s **oddílu** nastavení na **Neznámý**).
3.  Klikněte na **úkoly**a potom klikněte na **Nový hlasitost**.
4.  V polích před zahájení stránka průvodce nový objem, klikněte na tlačítko **Další**.
5.  Na stránce vyberte server a disku klikněte na **disku 2**a klikněte na tlačítko **Další**. Po zobrazení výzvy klikněte na **OK**.
6.  Na stránce zadání velikosti stránky hlasitost klikněte na **Další**.
7.  Na přiřazení na stránku písmeno nebo složku jednotka klikněte na **Další**.
8.  Na stránce nastavení systému vyberte soubor klikněte na **Další**.
9.  Na stránce Potvrďte výběr klikněte na **vytvořit**.
10. Až budete hotovi, klepněte na tlačítko **Zavřít**.

Na příkazovém řádku prostředí Windows PowerShell spusťte tyto příkazy na SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Pak připojení SQL1 doménu CORP Windows Server Active Directory s tyto příkazy příkazovém řádku prostředí Windows PowerShell na SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použijte účet CORP\User1 po zobrazení výzvy k zadání domény účtu pro ni přihlašovací údaje na příkaz **Přidat počítač** .

Po restartování počítače, připojení k SQL1 *s místního účtu správce systému SQL1*pomocí portálu Azure.

Potom nakonfigurujte 2014 serveru SQL pomocí jednotky F: pro nové databáze a uživatelských účtů oprávnění.

1.  Na obrazovce Start zadejte **SQL Server Management**a klikněte na **SQL Server 2014 Management Studio**.
2.  V části **připojit k serveru**klikněte na **Připojit**.
3.  V podokně stromového Průzkumník objektů **SQL1**klikněte pravým tlačítkem myši a potom klikněte na **Vlastnosti**.
4.  V okně **Vlastností serveru** klikněte na **Nastavení databáze**.
5.  Vyhledejte na **výchozí umístění databáze** a nastavte tyto hodnoty: 
    - **Data**zadejte cestu **f:\Data**.
    - **Protokol**zadejte cestu **f:\Log**.
    - **Zálohování**zadejte cestu **f:\Backup**.
    - Poznámka: Pouze nové databáze pomocí těchto umístění.
6.  Klikněte na **OK** zavřete okno.
7.  V podokně stromového **Průzkumník objektů** otevřete **zabezpečení**.
8.  Klikněte pravým tlačítkem myši **přihlášení** a potom klikněte na **Nové přihlášení**.
9.  V poli **přihlašovací jméno**zadejte **CORP\User1**.
10. Na stránce **Role serveru** klikněte **členem**a potom klikněte na **OK**.
11. Zavřete Microsoft SQL Server Management Studio.

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Fáze 3: Konfigurace serveru LOB (LOB1)

Nejprve vytvořte virtuálního počítače pro LOB1 s tyto příkazy příkazovém řádku prostředí PowerShell Azure ve vašem počítači.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Dál použijte portál Azure se připojit k LOB1 pomocí pověření místního účtu správce systému LOB1.

Potom nakonfigurujte pravidlo brány Firewall systému Windows umožňující přenosy pro účely testování základní připojení. Na úrovni správce prostředí Windows PowerShell příkazovém řádku LOB1 spusťte tyto příkazy.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi z adresy IP 192.168.0.4.

Pak spojení LOB1 a domény CORP Active Directory s tyto příkazy příkazovém řádku prostředí Windows PowerShell.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použijte účet CORP\User1 po zobrazení výzvy k zadání domény účtu pro ni přihlašovací údaje na příkaz **Přidat počítač** .

Po restartování počítače, použijte portál Azure se připojit k LOB1 pomocí CORP\User1 účet a heslo.

Potom konfigurace LOB1 pro službu IIS a otestujte přístup z počítače KLIENT1.

1.  Správci serveru, klikněte na **Přidat role a funkce**.
2.  Na stránce, **než začnete** klikněte na **Další**.
3.  Na stránce **Vyberte typ instalace** klikněte na **Další**.
4.  Na stránce **Vyberte cílový server** klikněte na **Další**.
5.  Na stránce **role serveru** klikněte na **Webový Server (IIS)** v seznamu **rolí**.
6.  Po zobrazení výzvy klikněte na **Přidat funkce**a klikněte na tlačítko **Další**.
7.  Na stránce **Vyberte funkce** klikněte na **Další**.
8.  Na stránce **Webový Server (IIS)** klikněte na **Další**.
9.  Na stránce **Vyberte služeb rolí** zaškrtněte nebo zrušte zaškrtnutí políček u služeb, které potřebujete k testování LOB aplikace a klikněte na tlačítko **Další**.
10. Na stránce **potvrzení vybrané možnosti instalace** klikněte na **instalovat**.
11. Počkejte na dokončení instalace součásti a pak klikněte na **Zavřít**.
12. Z portálu Azure připojit k počítači KLIENT1 CORP\User1 pověření pro účet a pak spustíte aplikaci Internet Explorer.
13. Na panelu Adresa zadejte **http://lob1/** a stiskněte klávesu ENTER. Byste měli vidět výchozí IIS 8 webovou stránku.

Toto je aktuální konfigurace.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Toto prostředí je nyní připravena nasazení aplikace založené na webu na LOB1 a otestovat funkce KLIENT1 v síti Corpnet podsítě.

## <a name="next-step"></a>Další krok

- Přidání nového virtuálního počítače pomocí [Azure portálu](virtual-machines-windows-hero-tutorial.md).
