<properties
    pageTitle="Vytvoření dostupnost OM | Microsoft Azure"
    description="Naučte se vytvářet dostupnost nastavení pro svůj virtuálních počítačích pomocí Azure portál nebo prostředí PowerShell pomocí nasazení modelu správce prostředků."
    keywords="nastavení dostupnosti"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Vytvoření sady dostupnosti 

Až na portálu, pokud chcete OM jako součást dostupnost, je potřeba vytvořit dostupnost nejdřív nastavit.

Další informace o vytváření a používání sady dostupnosti najdete v tématu [Správa dostupnosti virtuálních počítačích](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Vytvoření dostupné nastavit před vytvořením vaší VMs pomocí portálu

1. V nabídce centrální klikněte na tlačítko **Procházet** a vyberte **Dostupnost sady**.

2. Na **dostupnost nastaví zásuvné**klikněte na **Přidat**.

    ![Snímek obrazovky ukazující na tlačítko Přidat pro vytváření dostupnost nové nastavení.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Na zásuvné **nastavení vytvořit dostupnost** zadejte informace o sadě.

    ![Snímek obrazovky s informacemi, budete muset zadat dostupnost sadu vytvoříte.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Název** - název by měl být 1-80 znaky tvořen čísla, písmena, období, podtržení a typ čáry. První znak musí být písmeno nebo číslici. Poslední znak musí být písmeno, číslo nebo podtržítka.
    - **Poruchy domény** – poruch domény definovat skupiny virtuálních počítačích majících společné zdroje a síťové vypínač. Ve výchozím nastavení VMs jsou oddělené mezi až tři poruch doménami a lze změnit na 1 až 3.
    - **Aktualizace domény** – pět aktualizace přiřazené domény ve výchozím nastavení a to můžete nastavit 1 až 20. Aktualizace domény označují skupiny virtuálních počítačích a základní fyzické hardware, který můžete restartování ve stejnou dobu. Například pokud jsme určení vyžadovaného pět aktualizaci domény, když více než pět virtuálních počítačích jsou nastaveny v rámci jedné nastavte dostupnost, šestým virtuální počítač bude umístit na tu samou doménu aktualizace jako první virtuální počítač sedmým znakem ve stejném UD jako druhý počítač virtuální atd. Pořadí nové nemusí jít po sobě, ale jenom jedna aktualizace domény bude nutné restartovat najednou.
    - **Předplatné** - vyberte použijte, pokud máte více než jedno předplatné.
    - **Pole Skupina zdroje** : Vyberte existující skupinu zdroje kliknutím na šipku a výběrem skupina zdroje ze seznamu dolů. Můžete taky vytvořit nové skupiny prostředků tak, že zadáte název. Název může obsahovat žádný z následujících znaků: písmen, čísla, období, pomlčky, podtržítka a levou nebo pravou kulatou závorku. Název nelze končit tečkou. Všechny VMs ve skupině dostupnost je potřeba vytvořit ve stejné skupině zdroje jako sada dostupnosti.
    - **Umístění** – vyberte umístění z rozevíracího seznamu.

4. Po dokončení zadávání informací, klikněte na **vytvořit**. Po vytvoření skupiny dostupnosti uvidíte ho v seznamu po aktualizaci na portálu.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Umožňuje vytvořit virtuální počítač a dostupné nastavit ve stejnou dobu na portálu

Pokud vytváříte novou OM na portálu, můžete taky vytvořit nové dostupnost nastavení pro OM při vytváření prvního OM v sadě.

![Snímek obrazovky zobrazující proces vytváření dostupnost nové nastavení během vytvoříte OM.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Přidání nového OM do existující sady dostupnosti

Pro každý další OM, kterou vytvoříte, která by měla patřit v sadě Ujistěte se, vytvořit ve stejné **pole Skupina zdroje** a pak vyberte existující dostupnost nastavit v kroku 3. 

![Snímek obrazovky ukazující, jak vybrat existující dostupnost nastavit, aby používal váš OM.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>Použití Powershellu pro vytvoření dostupnosti

Tento příklad vytvoří dostupné nastavte ve skupině **RMResGroup** prostředků v umístění **Západní USA** . To je třeba udělat předtím, než vytvoříte první OM, který bude v sadě.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Další informace najdete v tématu [AzureRmAvailabilitySet nový](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Řešení potíží

- Když vytvoříte OM, není-li požadovanou sadu dostupnost v rozevíracím seznamu na portálu může vytvořili jste ho v jiné skupině zdrojů. Pokud si nejste jisti skupina zdroje pro své dostupnosti nastavení, přejděte do nabídky centrální a klikněte na Procházet > dostupnost nastaví zobrazíte seznam dostupnost sady a které patří do skupiny zdrojů.


## <a name="next-steps"></a>Další kroky

Přidejte další úložiště vaší OM Další [data disku](virtual-machines-windows-attach-disk-portal.md).
