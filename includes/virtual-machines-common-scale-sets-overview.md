

U aplikace, která muset měřítko výpočetním zdrojů v měřítku, které operace implicitně vyvážení a přes poruch a aktualizace domény. Úvod do OM měřítko sady v příručce poslední [Azure blogu oznámení](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Podívejte se na tato videa Další informace o sadách OM měřítko:

 - [Označit Russinovichem mluví sady měřítko Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuální počítač měřítko nastaví s hoch Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Vytváření a správě OM měřítko nastaví

OM měřítko sady je to možné definovat a nasazené pomocí šablony JSON a [Rozhraní REST API](https://msdn.microsoft.com/library/mt589023.aspx) jenom jako jednotlivé VMs Azure správce prostředků. Proto lze použít jakékoli standardní nasazení metody správce prostředků Azure. Další informace o šablonách najdete v článku [vytváření správce prostředků Azure šablony](../articles/resource-group-authoring-templates.md).

Sadu příklad šablony pro OM měřítko sady najdete v tady úložiště GitHub šablony Azure rychlý úvod:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) – hledejte šablony s _vmss_ v názvu.

Na stránkách podrobnosti pro tyto šablony uvidíte tlačítko, které obsahuje odkazy na funkci portálu nasazení. Abyste mohli nasadit sadu měřítko OM, klikněte na tlačítko a potom vyplňte parametry, které jsou požadovány na portálu. Pokud si nejste jistí, jestli zdroje podporuje horního nebo smíšené písmena je bezpečnější vždy používat hodnoty parametrů samá velká písmena. Je také užitečné videa disekce šablony sadu měřítko OM tady:

[OM měřítko nastavit disekce šablony](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Měřítko OM měřítko nastavení a

Zvětšit nebo zmenšit počet virtuálních počítačích v sadě měřítko OM, jednoduše změnit vlastnost _kapacitu_ a přeinstalujte šabloně. Tento zjednodušení snadno napsat vlastní vrstva změny měřítka. Pokud chcete definovat vlastní měřítko události, které nejsou podporovány Azure automatické měřítko.

Pokud se znovu nasazení šablony Změna kapacity, můžete definovat mnohem menší šablonu, která obsahuje pouze SKU a aktualizované kapacity. Příklad je zobrazena zde: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Projděte si kroky, které vytvořit sadu měřítko, která automaticky měřítko, najdete v článku [Automaticky měřítko počítačích v sadě měřítko virtuálního počítače](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Sledování OM měřítko nastavení

Aktuálně doporučuje že pomocí [Průzkumníka zdroje Azure](https://resources.azure.com) zobrazíte OM měřítko sady. OM měřítko sady jsou zdroje v části Microsoft.Compute, abyste z tohoto webu si mohli prohlédnout rozbalením následující odkazy:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Nastavte měřítko OM scénáře

V této části jsou uvedeny některé typické OM měřítko sadu scénáře. Některé vyšší úroveň Azure služby (třeba dávku, struktury služby, služba Azure kontejneru) použije podobnému sledu.

 - **RDP / SSH OM měřítko nastavit instance** – nastavení měřítko A OM je vytvořena uvnitř VNET a jednotlivé VMs v nejsou přiřazený veřejnou IP adres. Toto je dobré proto, že nechcete, aby se obecně režijních výdaje na úkol a Správa přidělování samostatných IP adres pro všechny zdroje příslušnosti ve výpočetním mřížky a můžete snadno připojit k tyto VMs z jiných zdrojů ve vaší VNET včetně těch, které mají veřejnou IP adresy jako Vyrovnávání zatížení nebo samostatné virtuálních počítačích.

 - **Připojení k VMs pomocí pravidel překladu síťových adres** – můžete vytvořit veřejnou IP adresu, ji přiřadit Vyrovnávání zatížení a definovat příchozí pravidla překladu síťových adres, které mapování port na IP adresu na portem OM v sadě OM měřítko. Například

    Veřejné IP -> portů 50000 -> vmss\_0 -> Port 22 veřejnou IP - > Port 50001 -> vmss\_1 -> Port 22 veřejnou IP - > Port 50002 -> vmss\_2 -> Port 22

    Tady je příklad vytvoření sady měřítko OM využívající pravidla překladu síťových adres povolit SSH připojení ke každé OM v rozsahu nastavte pomocí jednoho veřejnou IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Tady je příklad dělá totéž s RDP a Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - Nastavení **připojení k VMs pomocí "jumpbox"** – Pokud vytvoříte OM měřítko a samostatně OM ve stejné VNET, samostatného OM a požadovanou sadu měřítko OM VMs můžete připojit k sobě navzájem pomocí interní IP adresy podle VNET/podsítě. Je-li vytvořit veřejnou IP adresu a přiřadit ji někomu samostatného OM můžete RDP nebo SSH samostatného OM a připojte z počítače na vaše OM měřítko sadu instance. Může se stát v tomto okamžiku, že je jednoduchý sady měřítko OM podstatě bezpečnější než jednoduchý samostatně OM s veřejnou IP adresu ve výchozím nastavení.

    Příklad tento přístup, tato šablona umožňuje vytvořit jednoduchý cluster Mesos obsahuje samostatně OM předlohy, která spravuje OM měřítko sady na základě clusteru VMs: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Kruhové Vyrovnávání zatížení OM měřítko nastavit instance** – Pokud chcete předvádění práce ve výpočetním clusteru VMs použití přístupu "kruhového", můžete nakonfigurovat Vyrovnávání zatížení Azure pomocí služby Vyrovnávání zatížení pravidla příslušným způsobem. Můžete také definovat sond k ověření aplikace pracuje pomocí příkazu ping porty s cesta Protocol (protokol), interval a žádost o.

    Tady je příklad vytvoří sadu měřítko OM VMs serverem webové služby IIS, který používá při vyrovnávání zatížení pro vyrovnávání zatížení, dostane každý OM. Taky používá protokol HTTP ping adresu URL na každý OM: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - pohled na typ zdroje Microsoft.Network/loadBalancers a networkProfile a extensionProfile v virtualMachineScaleSet.

 - **Zavedení OM měřítko nastavit jako ve výpočetním clusteru ve Správci clusteru PaaS** - škála OM sady jsou někdy popsána vztahem pracovníka generování další roli. Je platný popis, ale taky spuštění rizika matoucí měřítko sadu funkcí s PaaS v1 pracovníka role funkcemi. Znamená OM měřítko sady poskytují skutečného "role pracovníka" nebo pracovní zdroj, v tomto poskytují generalized výpočetním prostředek, který je/modul runtime platformy samostatné, přizpůsobitelná a integruje do IaaS správce prostředků Azure.

    PaaS v1 pracovníka roli, zatímco omezená z hlediska platformy/runtime podpory (jenom obrázky platformu Windows) obsahuje taky služeb, jako je VIP vyměňovat, konfigurovat nastavení upgradu, runtime/nasazení určitých nastavení aplikace které buď nejsou _zatím_ k dispozici v OM měřítko sady nebo budete dostávat jiných vyšší úroveň PaaS služby, jako třeba struktury služby. S tímto v si, že si může samostatně prohlížet sady OM měřítko jako infrastrukturu, který podporuje PaaS. Tedy PaaS řešení služby struktury, nebo můžete správci obrázku jako Mesos můžete vytvářet nad OM měřítko sady scalable využití vrstvě.

    Tady je příklad Mesos obrázku, který nasadí nastavte měřítko OM ve stejném VNET samostatně OM. Samostatný OM je předloha Mesos a sadu měřítko OM představuje sady podřízeného uzlu: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Budoucí verze [Služby Azure kontejneru](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) nasadí více komplexních/zesílený verzích tento scénář založená na sadách OM měřítko.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Pokyny OM měřítko sadu výkon a měřítka

- Během veřejné náhledu nevytvářejte víc než 500 VMs ve více sad měřítko OM najednou.
- Vytvoření plánu pro víc než 40 VMs jednoho účtu úložiště.
- Roztažení prvních písmen názvy účtů úložiště co nejvíc.  Příklad šablony VMSS v [Azure rychlý úvod šablon](https://github.com/Azure/azure-quickstart-templates/) jsou uvedeny příklady školá.
- Pokud používáte vlastní VMs, plánování maximálně 40 VMs na sadu měřítko OM, v účtu jednoho úložiště.  Budete potřebovat obrázek předem zkopírována do účtu úložiště, abyste mohli začít OM měřítko nastavení nasazení. V tématu Nejčastější dotazy týkající se další informace.
- Vytvoření plánu pro více než 2048 VMs za VNET.  Toto omezení se zvýší v budoucnu.
- Počet VMs můžete vytvořit je omezený jazykem kvótu core výpočetním v jakékoli oblasti. Budete muset kontaktujte zákaznickou podporu pro zvýšení limitu kvóty pro využití lepší i když máte vysoké omezení jádra pro použití s cloudovým službám nebo IaaS v1 dnes. K vytvoření dotazu kvótu spustíte tento příkaz Azure rozhraní příkazového řádku: _používání seznamu azure OM_a následujícího příkazu Powershellu: _Get-AzureRmVMUsage_ (Pokud je pro verzi prostředí PowerShell pod 1.0 použít _Get-AzureVMUsage_).

## <a name="vm-scale-set-frequently-asked-questions"></a>OM měřítko nastavit často kladené otázky

**OTÁZKA:** Kolik VMs budete mít v sadě měřítko OM?

**ODPOVĚĎ:** 100, pokud používáte platformu obrázky, které lze rozdělit mezi různými účty úložiště. Pokud používáte vlastní obrázky až 40, protože vlastní obrázky jsou omezené k účtu jednoho úložiště během zobrazení náhledu.

**Q** jakých dalších omezení zdrojů jsou pro OM měřítko sady?

**ODPOVĚĎ:** Jste vytvořit maximálně 500 VMs ve více sad měřítko jednotlivých oblastech období náhled. Stávající [Azure odebíraných službách limity /](../articles/azure-subscription-service-limits.md) použít.

**OTÁZKA:** Jsou podporované disků dat v rámci sady měřítko OM?

**ODPOVĚĎ:** Nejsou v první vydání. Možnosti ukládání dat jsou:

- Azure soubory (SMB sdílené jednotky)

- Jednotka OS

- Temp jednotka (místní, není podporované službou Azure úložiště)

- Externí data service (například vzdálená databáze Azure tabulek, objekty BLOB Azure)

**OTÁZKA:** Oblasti, které Azure podpory OM měřítko sady?

**ODPOVĚĎ:** Oblast, která podporuje správce prostředků Azure podporuje OM měřítko sady.

**OTÁZKA:** Jak vytvoříte OM měřítko sady pomocí vlastní obrázek?

**ODPOVĚĎ:** VhdContainers vlastnost ponechte prázdné, například:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**OTÁZKA:** Pokud mám snížit Moje OM měřítko nastavit kapacitu od 20 15, které se odstraní VMs?

**ODPOVĚĎ:** Virtuálních počítačích budou odebrány z měřítko nastavit rovnoměrně různých upgradem domén a domény poruch maximalizace dostupnosti.

**OTÁZKA:** Co je-li poté zvyšte schopnost 18 15?

**ODPOVĚĎ:** Pokud zvětšíte 18 VMs indexu 15 16 17 se vytvoří. V obou případech VMs vyvážení různých FDs a UDs.

**OTÁZKA:** Pokud chcete použít více rozšíření v sadě měřítko OM, můžu vynutit sekvenci spuštění?

**ODPOVĚĎ:** Nikoli přímo, ale pro rozšíření customScript skript můžou čekat na jinou příponu dokončete (například sledováním rozšíření protokolu – najdete v článku [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**OTÁZKA:** Fungují OM měřítko sady sadami Azure dostupnost?

**ODPOVĚĎ:** Ano. Implicitní dostupnost sada se 3 FDs a 5 UDs je sada OM měřítko. Nemusíte nic v části virtualMachineProfile konfigurace. V budoucích verzích OM měřítko sady mívají rozsahu víc klientů, ale teď nastavte měřítko je jediný dostupnost sadou.
