<properties
    pageTitle="Navrhování virtuálního počítače měřítko nastaví pro měřítko | Microsoft Azure"
    description="Další informace o navrhování sady virtuálního počítače měřítko pro měřítko"
    keywords="Nastaví virtuálního počítače měřítko Linux virtuálního počítače" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Navrhování OM měřítko nastaví pro měřítko

Toto téma popisuje navrhování sady měřítko virtuálního počítače. Informace o tom, co sady měřítko virtuálního počítače podívejte se do [Virtuálního počítače měřítko sady přehled](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Úložiště

Sada měřítko používá k ukládání OS disků VMs v sadě úložiště účty. Doporučujeme poměr 20 VMs za úložiště účet nebo nižší. Doporučujeme, aby se rozšířit mezi abecedy začátku název účtu úložiště. V tom pomáhá rozšířit zatížení mezi různými vnitřní systémy. Například v šabloně následující používáme uniqueString funkce šablony správce prostředků pro generování hash předponu, které jsou před názvy účtů úložiště: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Od verze rozhraní API "2016 – 03-30" OM měřítko sady ve výchozím nastavení "overprovisioning" VMs. S overprovisioning zapnutá, měřítka skutečně monitoru je nestabilní nahoru Další VMs než vyzývá k zadání nastavení a potom odstraní navíc VMs, které nespředený nahoru poslední. Overprovisioning zlepšuje úspěšnosti zřizovací. Nejsou fakturované pro tyto další VMs a jejich nepočítají do svého kvóty.

Když overprovisioning zlepšit úspěšnosti zřizovací, mohou způsobit matoucí chování aplikace, které není pomáhají řešit VMs zmizení neohlášené. Pokud chcete zapnout, overprovisioning vypnout, zkontrolujte, jestli máte následující řetězec do šablony: "overprovision": "hodnotu false". Další informace najdete v [dokumentaci rozhraní REST API nastavte měřítko OM](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Pokud vypnete overprovisioning, můžete můžete procházelo větší poměr VMs jednoho účtu úložiště, ale nedoporučujeme začátek nad 40.


## <a name="limits"></a>Omezení
Sada měřítko založená na vlastní obrázek (jedno integrované sami určili) musí vytvořit všechny VHD disku OS v rámci jednoho účtu úložiště. Maximální počet VMs v sadě měřítko založená na vlastní obrázek doporučené jako výsledek je 20. Pokud vypnete overprovisioning, můžete přejít až 40.

Sady měřítko založená na obrázku platformy je aktuálně 100 VMs (doporučujeme 5 účty úložiště pro tento měřítko).

Pro další VMs než tyto limity povolit, budete muset nasazení více sad měřítko, jak je uvedeno v [této šabloně](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).