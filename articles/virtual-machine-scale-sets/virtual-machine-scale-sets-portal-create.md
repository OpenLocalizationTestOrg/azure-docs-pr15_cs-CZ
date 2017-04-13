<properties
    pageTitle="Vytvořit virtuální počítač měřítko sadu pomocí portálu Azure | Microsoft Azure"
    description="Nasazení měřítko sady Azure portálu."
    keywords="virtuální počítač měřítko sady" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Vytvořit virtuální počítač měřítko sadu pomocí portálu Azure

Tento kurz se dozvíte, jak je snadné nastavte měřítko virtuálního počítače za několik minut, můžete vytvořit pomocí portálu Azure. Pokud nemáte předplatné Azure, než začnete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) .

## <a name="choose-the-vm-image-from-the-marketplace"></a>Vyberte obrázek, který OM z marketplace

Z portálu Microsoft můžete snadno nasadíte měřítko sada se CentOS, jádro operačního systému, Debian, otevřít Suse, červené klobouk Enterprise Linux, SUSE Linux Enterprise Server, se systémem Ubuntu Server nebo Windows Server obrázky.

Nejdřív přejděte na [portál Azure](https://portal.azure.com) ve webovém prohlížeči. Klikněte na `New`, vyhledejte `scale set`a klikněte `Virtual machine scale set` položky:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Vytvoření Linux virtuálního počítače

Teď můžete použít výchozí nastavení a rychle vytvořit virtuální počítač.

* V `Basics` zásuvné, zadejte název sady měřítko. Tento název bude základ plně kvalifikovaný název domény Vyrovnávání zatížení před sadu měřítko, proto se ujistěte, že je ve všech Azure jedinečný název.

* Vyberte požadovanou OS zadejte, zadejte požadované uživatelské jméno a vyberte, kdy ověřování typ upřednostňujete. Pokud se rozhodnete heslo, musí být alespoň 12 znaků dlouhé a setkání tři mimo čtyři tyto požadavky složitost: jeden malými písmeny znak, velká jeden znak, jedno číslo a jeden jinak. Další informace o [požadavcích na uživatelské jméno a heslo](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Pokud se rozhodnete `SSH public key`, být potřeba pouze vložit na svůj veřejný klíč, ne privátním klíčem:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Zadejte název skupiny požadovaný zdroj a umístění a potom klikněte na `OK`.

* V `Virtual machine scale set service settings` zásuvné: Zadejte název štítku požadované domény (základní plně kvalifikovaný název domény pro vyrovnávání zatížení před sadu měřítko). Tento popisek musí být jedinečný ve všech Azure.

* Vyberte požadovaný operační systém bitové, počet instancí a velikost počítače.

* Povolení nebo zakázání automatické měřítko a konfigurace Pokud je povoleno:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* V `Summary` klikněte na zásuvné po dokončení ověření `OK`.

* Nakonec na `Purchase` zásuvné, klikněte na `Purchase` zahájíte měřítka nastavení nasazení.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Připojení k OM v sadě měřítko

Po zavedení sadě měřítko, přejděte `Inbound NAT Rules` kartu Vyrovnávání zatížení sady měřítko:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Můžete se připojit k každý OM v rozsahu nastavte pomocí těchto pravidel překladu síťových adres. Například pro sadu Windows měřítko při překladu síťových adres pravidla na příchozí portů 50000, můžete se připojit na tomto počítači pomocí RDP na `<load-balancer-ip-address>:50000`. Pro sadu měřítko Linux by připojíte pomocí příkazu `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Další kroky

Dokumentace o nasazení měřítko sad z rozhraní příkazového řádku dokumentaci k [této](./virtual-machine-scale-sets-cli-quick-create.md).

Dokumentace o nasazení měřítko sad z prostředí PowerShell dokumentaci k [této](./virtual-machine-scale-sets-windows-create.md).

Dokumentace o nasazení měřítko sady z aplikace Visual Studio dokumentaci k [této](./virtual-machine-scale-sets-vs-create.md).

Obecné si přečtěte následující dokumentaci podívejte se na [stránce přehledu si přečtěte následující dokumentaci pro měřítko nastaví](./virtual-machine-scale-sets-overview.md).

Obecné informace podívejte se na [hlavní cílovou stránku pro měřítko sady](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

