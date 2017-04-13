<properties
    pageTitle="Infrastruktura pojmenování pokyny | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro pojmenování v Azure infrastruktury služby."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Pokyny pro pojmenování infrastruktury

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na vědět, jak řešit zásady vytváření názvů pro všechny vaše různých Azure zdroje můžete vytvořit sadu logické a snadno identifikovat prostředků přes prostředí.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementace pokyny pro vytváření názvů

Rozhodnutí:

- Jaké jsou vaše konvence pro Azure zdroje?

Úkoly:

- Definování přípony pro používání různých zdrojů k zajištění konzistence.
- Definování názvů účtu úložiště uvedených požadavku, aby byly jedinečné.
- Pojmenování a distribuovat všichni účastníci zajistit soulad přes nasazení dokumentu

## <a name="naming-conventions"></a>Konvence pojmenování

Měli byste mít dobré pojmenování na místě před vytvořením něco v Azure. Pojmenování zajišťuje všechny zdroje předvídatelná název, což pomůže snížit administrativní zátěž souvisejících se správou zdroje.

Můžete sledovat určité skupiny konvence definované pro celou organizaci nebo pro konkrétní Azure předplatné a účtu. I když není těžké pak jednotlivé kontakty v rámci organizace vytvořit implicitní pravidla při práci s Azure zdroje týmu potřebujete-li práce na projektu na Azure, tento model není adekvátní dobře.

Souhlasíte se sadou konvence předem. Existuje několik tipů týkající se vytváření názvů, které protínají sady pravidel.

## <a name="affixes"></a>Přípony

Vzhled definovat pojmenování pochází jeden rozhodnutí, zda připojí na:

- Na začátek názvu (předpona)
- Na konec názvu (přípona)

Například, tady jsou dvou možných názvů pro pole Skupina zdroje pomocí `rg` opatřit:

- Rg webové aplikace (předpona)
- Web Appu Rg (přípona)

Přípony mohou odkazovat na různé aspekty, které popisují určitého zdroje. V následující tabulce jsou uvedeny příklady obvykle používá.

| Poměr                               | Příklady                                                               | Poznámky                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Prostředí                          | vývoj, stg, spotřeba                                                         | V závislosti na účel a název každého prostředí.                                                     |
| Umístění                             | usw (západní US), použijte (východoasijských USA 2)                                         | V závislosti na oblasti datacentra nebo oblasti v organizaci.                               |
| Azure součásti, služby nebo produktu | Rg pro pole Skupina zdroje, VNet virtuální sítě                        | V závislosti na produkt, pro které zdroj podporuje.                                          |
| Role                                 | SQL ora, sp, služby iis                                                      | V závislosti na rolích virtuální počítač.                                                              |
| Instanci                             | 01, 02, 03, atd.                                                       | Pro zdroje, které mají víc než jedna instance. Například Vyrovnávání zatížení webových serverů v cloudové službě. |


Při vytváření konvence pojmenování, ujistěte se, že jasně uvést které přípony můžete u jednotlivých typů zdrojů a v jakou pozici (předpona a přípona).

## <a name="dates"></a>Kalendářní data

Je často důležité určit datum vytvoření od názvu zdroje. Doporučujeme RRRRMMDD formát data. Tento formát zaručuje, že nejen úplné datum nastavené, ale taky tento dva zdroje, jejichž názvy se liší jenom na datum je seřazené podle abecedy a chronologicky ve stejnou dobu.

## <a name="naming-resources"></a>Pojmenování zdroje

Definování každého typu zdroje v zásady vytváření názvů, které by měl mít pravidla, které definují jak přiřadit názvy každému zdroji, která se vytvoří. Tato pravidla má použít pro všechny typy zdrojů, například:

- Předplatná
- Účty
- Účty úložiště
- Virtuální sítě
- Podsítí
- Dostupnost sady
- Skupiny zdrojů
- Virtuálních počítačích
- Koncové body
- Skupiny zabezpečení sítě
- Role

Zajistit, aby název obsahuje dost informacím, a zjistit, které zdroje záznam odkazuje, používejte popisných názvů.

## <a name="computer-names"></a>Názvy počítačů

Při vytváření virtuálního počítače (OM) se vyžaduje Microsoft Azure OM název může obsahovat maximálně 15 znaků které se používá pro název zdroje. Azure používá pro operační systém nainstalovaný v OM stejný název. Však tyto názvy nemusí být vždy stejné.

V případě, že virtuálního počítače se vytvoří ze souboru VHD obrázek, který již obsahuje operační systém, názvu OM v Azure můžou lišit od název počítače OM operační systém. Tato situace můžete přidat stupeň obtížnosti Správa OM, který proto nedoporučujeme. Přiřazení zdrojů Azure OM stejný název jako název počítače, který přiřadíte operační systém, které OM.

Doporučujeme, aby název Azure OM je stejná jako základní název počítače operační systém.

## <a name="storage-account-names"></a>Názvy účtů úložiště

Úložiště účty mají zvláštní pravidla pro jejich jména. Můžete použít jenom malá písmena a číslice. Další informace najdete v tématu [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) . Název účtu úložiště, spolu s core.windows.net, navíc by měl být globálně platné jedinečný název DNS. Například pokud účtu úložiště nazývá mystorageaccount, následující výsledné názvy DNS musí být v jedinečný:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 