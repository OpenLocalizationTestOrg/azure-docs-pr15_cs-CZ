<properties
    pageTitle="Předplatné a pokyny pro účty | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro předplatné a účty v Azure."
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

# <a name="subscription-and-accounts-guidelines"></a>Pokyny pro účty a předplatného

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na vědět, jak má přístup ke správě předplatného a účet jako prostředí a uživatele základní roste.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementace pokyny pro účty a předplatného

Rozhodnutí:

- K čemu sadu předplatná a proveďte účty, budete muset hostovat vaše IT vytížení nebo infrastruktury?
- Jak se dá rozdělí hierarchie, které chcete přizpůsobit vaše organizace?

Úkoly:

- Definování logické organizační hierarchii tak, jak se ke správě od úrovně předplatného.
- Podle tohoto logická hierarchie, definujte účty povinné a předplatná pod každým účtem.
- Vytvoření nastavených předplatná a účty používat stejný systém názvů.


## <a name="subscriptions-and-accounts"></a>Účty a předplatného

Pro práci s Azure, musíte jeden nebo více Azure předplatná. Zdroje virtuálních počítačích (VMs), nebo můžete virtuálních sítí, které v těchto předplatných.

- Podnikoví uživatelé mají obvykle Enterprise zápis, kterým je zdroj zcela nahoře v hierarchii a je přidružená k jedné nebo více účtů.
- Pro které se zobrazují koncovým a zákazníky bez zápisu Enterprise je zdroj navrchu účet.
- Předplatná jsou přidružené k účtům a může být jeden nebo víc předplatných na účet. Azure záznamy fakturační informace na úrovni předplatného.

Z důvodu omezení dvěma úrovněmi hierarchie na účtu/předplatné relace je důležité zarovnání konvence účty a předplatné k fakturaci potřeb. Například pokud globální společnost používá Azure, může zvolte mít účtů jednotlivých oblastech a mít předplatná spravované na úrovni oblasti:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Můžete například pomocí následující strukturu:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Pokud máte víc než jedno předplatné související s určitou skupinou oblast, konvence zahrnoval způsob, jak kódovat navíc dat na účtu nebo na název předplatného. Tento organizace umožňuje, massaging fakturační dat pro vytvoření nové úrovně hierarchie během fakturační sestavy:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Organizace může vypadat takto:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Připravili jsme podrobné fakturace pomocí ke stažení souboru pro jeden účet nebo pro všechny účty v dohodu organizace.


## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 