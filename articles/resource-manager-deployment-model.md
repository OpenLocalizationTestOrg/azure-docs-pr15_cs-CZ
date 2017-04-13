<properties
   pageTitle="Správce prostředků a classic nasazení | Microsoft Azure"
   description="Popisuje rozdíly mezi nasazení modelu správce prostředků a klasickou (nebo služba Správa) nasazení modelu."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure správce prostředků porovnání klasické nasazení: princip modelů nasazení a stav zdroje

V tomto tématu informace o správce prostředků Azure a klasické nasazení modelů, stav svého zdrojů a proč svých prostředcích byly nasazeny s jedním nebo další. Správce prostředků a modelů klasické nasazení představují dvěma různými způsoby nasazení a správu Azure řešení. Práce s nimi přes dva různé sady rozhraní API a nasazené zdrojů může obsahovat důležité rozdíly. Dva modely nejsou vzájemně úplně kompatibilní. Toto téma popisuje tyto rozdíly.

Pro zjednodušení nasazení a řízení zdrojů, Microsoft doporučuje používat Správce prostředků pro všechny nové zdroje. Pokud je to možné doporučuje se přeinstalujte existujících zdrojů pomocí Správce prostředků.

Pokud začínáte správci zdrojů, je vhodné nejprve zkontrolujte terminologie podle [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Historie modelů nasazení

Azure původně k dispozici pouze klasické nasazení modelu. V tomto modelu jednotlivé zdroje existoval nezávisle; Umožňuje seskupit související materiály jste udělali. Místo toho bylo nutné ručně sledování prostředků, které tvoří řešení nebo aplikací a nezapomeňte spravovat je v koordinovaný přístup. Nasazení řešení, bylo nutné vytvořit jednotlivé zdroje jednotlivě prostřednictvím portálu classic nebo vytvořit skript, který nasazený všechny zdroje ve správném pořadí. Pokud chcete odstranit řešení, bylo nutné jednotlivě odstranit jednotlivé zdroje. Není snadno můžete použít a aktualizovat zásady řízení přístupu pro související materiály. Nakonec nelze použít značky na zdroje je označit za podmínek, které vám pomůžou sledování svých prostředcích a spravovat fakturaci.

V 2014 zavádí Azure správce zdrojů, které přidali koncepci skupina zdroje. Skupina zdroje je kontejner pro zdroje majících společné životního cyklu. Správce prostředků nasazení modelu nabízí několik výhod:

- Můžete nasadit, Správa a sledovat všechny služby řešení pro skupinu, spíše než zpracování těchto služeb samostatně.
- Lze opakovaně nasazení řešení během svého životního cyklu a spolehlivosti zavedení zdrojů v konzistentního stavu.
- Řízení přístupu můžete použít pro všechny zdroje v skupina zdroje a tyto zásady automaticky, použijí se při přidávání nových zdrojů Skupina zdroje.
- Značky můžete vyrovnat zdroje logicky uspořádání všech zdrojů v předplatného.
- JavaScript Object Notation (JSON) lze definovat infrastrukturu pro vaše řešení. Soubor JSON se označuje jako šablonu správce prostředků.
- Můžete definovat závislosti mezi zdroje tak jejich nasazením ve správném pořadí.

Pokud správce prostředků bylo přidáno, všechny zdroje se zpětně přidala do výchozí skupiny zdrojů. Pokud vytvoříte zdroj implementací klasické teď zdroje se automaticky vytvoří v rámci výchozí skupina zdroje pro službu, i když jste nezadali tohoto pole Skupina zdroje na nasazení. Ale jenom stávající skupinou zdroje není znamená, že zdroje byla převedena na modelu správce prostředků. Podíváme na zpracování modelů dvou nasazení v následující části každé služby. 

## <a name="understand-support-for-the-models"></a>Princip podpory pro modely 

Při rozhodování, jaký model nasazení můžete u zdrojů, existují tři scénáře je potřeba vědět:

1. Služba podporuje správce prostředků a obsahuje pouze jednoho typu.
2. Služby podporuje správce prostředků, ale obsahuje dva typy – jeden pro správce prostředků a jeden pro classic. Tento scénář se týká jenom pro virtuálních počítačích, úložiště účtů a virtuální sítě.
3. Služba nepodporuje správce prostředků.

Chcete-li zjistit, zda službu podporuje správce prostředků, najdete v článku [Správce prostředků podporované poskytovatelů](resource-manager-supported-services.md).

Klasický nasazení musí používat službu, kterou chcete použít nepodporuje správce prostředků.

Pokud služba podporuje správce prostředků a **není** virtuálního počítače, úložiště účtu nebo virtuální sítě, můžete použít Správce prostředků bez jakékoli komplikace.

Pro virtuálních počítačích úložiště účty a virtual sítích Pokud zdroje byl vytvořený prostřednictvím klasické nasazení, musíte dál pracovat pomocí klasické operace. Správce prostředků operace musí používat virtuálního počítače, úložiště účtu nebo virtuální sítě byl vytvořený prostřednictvím Správce prostředků nasazení. Tento rozdíl získáte přehlednější, pokud vaše předplatné obsahuje kombinaci zdroje vytvořený prostřednictvím Správce zdrojů a klasické nasazení. Kombinace zdrojů můžete vytvořit neočekávané výsledky, protože zdroje nepodporují stejné operace.

V některých případech příkaz Správce prostředků můžete získat informace o zdroji vytvořený prostřednictvím klasické nasazení nebo může provádět správce úloh například přesunutí klasické zdrojů do jiné skupiny zdrojů. Ale takovýchto případech neměli dát dojem, že typ podporuje operace správce prostředků. Předpokládejme například, že máte skupina zdroje, který obsahuje virtuální počítač, který byl vytvořený pomocí klasické nasazení. Pokud spustíte tento příkaz Správce prostředků Powershellu:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Vrátí virtuálního počítače:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Správce prostředků rutiny **Get-AzureRmVM** však vrátí jenom virtuálních počítačích nasazených pomocí Správce prostředků. Následující příkaz nevrací virtuálního počítače vytvořený prostřednictvím classic nasazení.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Pouze zdroje vytvořené pomocí značek podpory správce prostředků. Nelze použít značky klasické zdroje.

## <a name="resource-manager-characteristics"></a>Správce prostředků vlastnosti

Které vám pomohou porozumět dvou modelů, pojďme si rozebrat charakteristických vlastností Správce prostředků typy:

- Vytvořený prostřednictvím [Azure portálu](https://portal.azure.com/).

     ![Azure portálu](./media/resource-manager-deployment-model/portal.png)

     Pro využití, ukládání a sítě zdrojů máte možnost používání správce prostředků nebo klasický nasazení. Vyberte **Správce prostředků**.

     ![Správce prostředků nasazení](./media/resource-manager-deployment-model/select-resource-manager.png)

- Vytvořený pomocí verze správce prostředků rutiny prostředí PowerShell Azure. Tyto příkazy s formátem *Slovesné AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Vytvořený prostřednictvím [Azure správce prostředků REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) pro ZBÝVAJÍCÍ operace.

- Vytvořený prostřednictvím rozhraní příkazového řádku Azure příkazy spustit v režimu **arm** .

        azure config mode arm
        azure group deployment create 

- Typ zdroje nezahrnuje **(klasické)** v názvu. Následující obrázek znázorňuje typ účtu **úložiště**.

    ![v prohlížeči](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Vlastnosti klasické nasazení

Můžete taky vědět, model klasické nasazení se nachází model služby správy.

Vytvoření v modelu klasické nasazení zdrojů sdílí následující vlastnosti:

- Vytvořený prostřednictvím [klasické portálu](https://manage.windowsazure.com)

     ![Klasický portálu](./media/resource-manager-deployment-model/classic-portal.png)

     Nebo portálu Azure a zadáte **klasické** nasazení (pro využití úložiště a sítě).

     ![Klasický nasazení](./media/resource-manager-deployment-model/select-classic.png)

- Vytvořený prostřednictvím služby správy verzi rutiny prostředí PowerShell Azure. Tyto názvy příkaz s formátem *Slovesné AzureNoun*.

        New-AzureVM 

- Vytvořený prostřednictvím [Služby správy REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) pro ZBÝVAJÍCÍ operace.
- Vytvořený prostřednictvím rozhraní příkazového řádku Azure příkazy spustit v režimu **asm** .

        azure config mode asm
        azure vm create 

- Typ zdroje obsahuje název **(classic)** . Následující obrázek znázorňuje typ účtem **úložiště (klasické)**.

    ![klasický typ](./media/resource-manager-deployment-model/classic-type.png)

Portál Azure slouží k přidávání a používání zdrojů, které jsou vytvořené pomocí klasické nasazení.

## <a name="changes-for-compute-network-and-storage"></a>Změny výpočetním, sítě a úložiště

Následující diagram zobrazuje výpočetním, sítě a úložiště zdrojů pomocí Správce prostředků.

![Správce prostředků architektura](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Poznámka: následující vztahy mezi zdroje:

- Všechny zdroje máte v rámci skupiny zdrojů.
- Virtuální počítač závisí na konkrétní úložiště účtu vymezuje zprostředkovatele prostředků úložiště ukládat jeho disků v úložišti objektů blob (povinné).
- Virtuální počítač odkazuje konkrétní NIC podle zprostředkovatele zdroje (povinné) a dostupné nastavení vymezuje zprostředkovatele pro využití prostředků (volitelné).
- NIC odkazuje virtuálního počítače přiřazenou IP adresu (povinné), podsítě virtuální sítě pro virtuálního počítače (povinné) a skupiny zabezpečení síti (volitelné).
- Podsítě v síti virtuální odkazuje síťové skupiny zabezpečení (volitelné).
- Instance Vyrovnávání zatížení odkazuje fondu back-end IP adres, které obsahují NIC virtuálního počítače (nepovinné) a odkazy načíst vyrovnávání veřejné nebo soukromé IP adresy (nepovinné).

Toto jsou součástí a jejich vztahů pro klasické nasazení:

![klasický architektura](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Klasický řešení pro hostování virtuální počítač zahrnuje:

- Povinný cloudové služby, která funguje jako kontejner pro hostování virtuálních počítačích (výpočetním). Virtuálních počítačích jsou automaticky součástí síťová karta (NIC) a IP adresy přidělil Azure. Navíc cloudové služby obsahuje instanci Vyrovnávání zatížení externí, veřejnou IP adresu a výchozí koncové body umožňuje vzdálené plochy a vzdálený PowerShell přenosy serveru s Windows virtuálních počítačích a zabezpečené prostředí (SSH) návštěvníci na základě Linux virtuálních počítačích.
- Povinný úložiště účet, který ukládá VHD virtuálního počítače, včetně operačního systému disků dočasný a dalších dat (úložiště).
- Volitelné virtuální sítě, která funguje jako další kontejner, ve kterém můžete vytvořit strukturu jako a určete podsítě, na kterém virtuální počítač nachází (sítě).

Následující tabulka popisuje změnami interakce výpočetním, sítě a úložiště poskytovatelů zdroje:

 Položky | Klasický | Správce prostředků
 ---|---|---
| Cloudová služba pro virtuálních počítačích |  Cloudové služby je kontejner při blokování virtuálních počítačích požadované dostupnost z platformy a vyrovnávání zatížení. | Cloudové služby je už objekt potřebných pro vytvoření virtuálního počítače pomocí nový model. |
| Virtuální sítě | Virtuální sítě vynechán virtuálního počítače. Pokud, nelze virtuální sítě nasadit pomocí Správce prostředků. | Virtuální počítač vyžaduje virtuální sítě, které byly nasazeny pomocí Správce prostředků. |
| Účty úložiště | Virtuální počítač vyžaduje účet úložiště obsahujícího VHD operační systém, disků dočasný a další data. | Virtuální počítač vyžaduje účet úložiště ukládat jeho disků v úložišti objektů blob. |
| Dostupnost sady | Dostupnost na platformu byla zadána nakonfigurováním stejné "AvailabilitySetName" na virtuálních počítačích. Maximální počet poruch domény byla 2. | Dostupnost je zdroj zveřejněné příslušným Microsoft.Compute poskytovatele. Virtuálních počítačích, které vyžadují dostupnost musí být součástí sady dostupnosti. Maximální počet poruch domény je teď 3. |
| Spřažení skupiny | Skupiny spřažení byly nutné pro vytváření virtuálních sítí. Však zavedením místní virtuální sítě, který se požadované už. |Pro zjednodušení, v rozhraní API vystavená prostřednictvím Správce prostředků Azure neexistuje koncept spřažení skupiny. |
| Vyrovnávání zatížení    | Vytvoření Cloudová služba poskytuje Vyrovnávání zatížení implicitní virtuálních počítačích nasazení. | Vyrovnávání zatížení je zdroj zveřejněné příslušným Microsoft.Network poskytovatele. Rozhraní primární sítě virtuálních počítačích, které je třeba rozloženy by měl odkazování na Vyrovnávání zatížení. Vyrovnávání zatížení může být vnitřní a vnější. Instanci Vyrovnávání zatížení odkazuje fondu back-end IP adres, které obsahují NIC virtuálního počítače (nepovinné) a odkazy načíst vyrovnávání veřejné nebo soukromé IP adresy (nepovinné). [Další informace.](../articles/resource-groups-networking.md) |
|Virtuální IP adresa | Při virtuálního počítače se přidá do cloudové služby, zobrazuje cloudovým službám výchozí VIP (virtuální IP adresa). Virtuální IP adresu je adresu přidruženou k vyrovnávání zatížení implicitní.    | Veřejnou IP adresu je zdroj zveřejněné příslušným Microsoft.Network poskytovatele. Veřejnou IP adresu lze statického (vyhrazeno) nebo dynamického. Dynamické veřejnou IP adresy můžete přidělovat Vyrovnávání zatížení. Veřejné IP adresy můžete zabezpečená pomocí skupin zabezpečení. |
|Rezervovaná IP adresa|   Můžete rezervovat IP adresy v Azure a přidružit Cloudová služba a ujistěte se, že je na IP adresu rychlých.   | Lze vytvořit veřejnou IP adresu "Statického" režimu a nabízí možnost "Rezervovaná IP adresu". Statická veřejnou IP adresy můžete jenom přidělovat Vyrovnávání zatížení teď. |
|Veřejnou IP adresu (PIP) za OM | Veřejné adresy IP lze také přidružený k virtuálního počítače přímo. | Veřejnou IP adresu je zdroj zveřejněné příslušným Microsoft.Network poskytovatele. Veřejnou IP adresu lze statického (vyhrazeno) nebo dynamického. Však pouze dynamické veřejnou IP adresy můžete přidělovat rozhraní sítě získat veřejnou IP za OM teď. |
|Koncové body| Zadávání koncové body potřebné k nakonfigurovali ve počítače virtuální být otevřený přes připojení pro některé porty. Jedna z běžné způsoby připojení k virtuálních počítačích, provést nastavení vstupní koncové body. | Na Vyrovnávání zatížení dosáhnout stejné možnosti povolení koncové body na konkrétní porty připojit se k VMs je možné konfigurovat příchozí pravidla překladu síťových adres. |
|Název DNS| Cloudová služba byste dostali, implicitní globálně jedinečný název DNS. Příklad: `mycoffeeshop.cloudapp.net`. | Názvy DNS jsou volitelné parametry určených na veřejnou IP adresu zdroje. Úplný název domény je ve formátu následující - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Rozhraní sítě | Primární a sekundární síťové rozhraní a jeho vlastností byly rozumí konfigurace sítě virtuálního počítače. | Rozhraní sítě je zdroj zveřejněné příslušným Microsoft.Network poskytovatele. Se virtuální počítač není stejným životním cyklu rozhraní sítě. Odkazuje virtuálního počítače přiřazenou IP adresu (povinné), podsítě virtuální sítě pro virtuálního počítače (povinné) a skupiny zabezpečení síti (volitelné). |

Další informace o připojení virtuální sítě z různých nasazení modelů najdete v tématu [připojení přes virtuální sítě z různých nasazení modelů na portálu](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migrace z klasického správci zdrojů

Pokud budete chtít migrovat zdroje z klasické nasazení Správce prostředků nasazení, najdete v článku:

1. [Technická hloubkové postupy na podporované platformy migrace z classic správci zdrojů Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Podporované platformy migrace IaaS zdrojů z klasického správci zdrojů Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migrace zdrojů IaaS od klasického správci zdrojů Azure pomocí prostředí PowerShell Azure](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migraci zdrojů IaaS z klasické správci zdrojů Azure pomocí rozhraní příkazového řádku Azure](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Můžete vytvořit virtuální počítači pomocí Správce prostředků Azure nasazovat ve virtuální sítě nebo účtu úložiště vytvořené pomocí klasické nasazení?**

Toto není podporovaná. Správce prostředků Azure nelze použít pro nasazení virtuálního počítače do virtuální sítě, který byl vytvořený pomocí klasické nasazení.

**Můžete vytvořit virtuální počítači správcem zdrojů Azure z obrázku uživatele, který byl vytvořený pomocí rozhraní API Správa služby Azure?**

Toto není podporovaná. Můžete však zkopírovat virtuální pevný disk soubory z úložiště účet, který byl vytvořený pomocí rozhraní API Správa služby a je přidat do nového účtu vytvořené pomocí Správce prostředků Azure.

**Co je dopad na kvóta pro svoje předplatné?**

Kvóty pro virtuálních počítačích, virtuálních sítí a úložiště účtů vytvořených pomocí Správce prostředků Azure jsou odděleně od jiných kvóty. Každého předplatného získá kvóty vytvoření zdrojů pomocí nového rozhraní API. Další informace o dalších kvóty [tady](../articles/azure-subscription-service-limits.md).

**Můžete nadále používat automatické skripty pro vytváření virtuálních počítačích, virtuálních sítí a úložiště účty prostřednictvím rozhraní API Správce prostředků?**

Automatizace a skripty, které jste integrované pokračovat v práci pro existující virtuálních počítačích virtuální sítě vytvořené v části Správa služby Azure režimu. Však skripty potřeba aktualizovat použití schématu nové vytvoření stejné zdroje v režimu správce prostředků.

**Kde najdu své ukázek šablon správce prostředků Azure?**

Komplexní sady starter šablony se nachází na [Azure správce prostředků rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Další kroky

- Projděte si postup vytvoření šablony, která definuje virtuálního počítače, úložiště účet a virtuální sítě, najdete v článku [Správce prostředků šablony návodu](resource-manager-template-walkthrough.md).
- Příkazy pro nasazení šablony najdete v tématu [nasazení aplikace šablonou správce prostředků Azure](resource-group-template-deploy.md).
