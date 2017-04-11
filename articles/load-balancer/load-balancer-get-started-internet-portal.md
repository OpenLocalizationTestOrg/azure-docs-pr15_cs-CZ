<properties
   pageTitle="Vytvoření Vyrovnávání zatížení internetového v správce na portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení internetového v správce na portálu Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Vytvoření služby Vyrovnávání zatížení internetového pomocí portálu Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje nasazení modelu správce prostředků. Můžete taky [Naučte se vytvářet Vyrovnávání zatížení internetového pomocí klasické nasazení](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Toto se vztahuje pořadí jednotlivých úkolů, který má zbývá dokončit vytvářet Vyrovnávání zatížení a podrobně popisují, co právě probíhá provádění cílem.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Co je potřeba vytvořit Vyrovnávání zatížení internetového?

Potřebujete vytvořit a nakonfigurovat následující objekty, které chcete nasadit Vyrovnávání zatížení.

- Konfigurace front-end IP - obsahuje veřejnou IP adres pro příchozí v síti.

- Fond back-end adres – stránka obsahuje síťová rozhraní (NIC) virtuálních počítačích přijme síťová komunikace od Vyrovnávání zatížení.

- Vyrovnávání zatížení pravidla - obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení s portem ve fondu back-end adres.

- Příchozí pravidla překladu síťových adres – obsahuje pravidla mapování veřejné port na Vyrovnávání zatížení na port pro konkrétní virtuální počítač ve fondu back-end adres.

- Sond - obsahuje stavu sond použít ke kontrole dostupnosti instancí virtuálních počítačích ve fondu back-end adres.

Součásti vyrovnávání pomocí Správce prostředků Azure můžete získat další informace o zatížení u [Správce prostředků Azure podpora služby Vyrovnávání zatížení](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Nastavení Vyrovnávání zatížení Azure portálu

> [AZURE.IMPORTANT] V tomto příkladu předpokládá, že máte virtuální sítě s názvem **myVNet**. V nápovědě k [vytvoření virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) akce. Dále předpokládá je podsítě v rámci **myVNet** s názvem **LB podsítě být** a dvě VMs s názvem **web1** a **webu 2** respektive v rámci sady stejné dostupnost volali **myAvailSet** **myVNet**. Podívejte se na [Tento odkaz](../virtual-machines/virtual-machines-windows-hero-tutorial.md) na vytvoření VMs.


1. V prohlížeči přejděte na portál Azure: [http://portal.azure.com](http://portal.azure.com) a přihlásíte se svým účtem Azure.

2. V horní levé části obrazovky vyberte **Nový** > **sítě** > **Vyrovnávání zatížení.**

3. V zásuvné **Služba Vyrovnávání zatížení vytvořit** zadejte název pro vyrovnávání zatížení. Tady je označená jako **myLoadBalancer**.

4. Ve skupinovém rámečku **Typ**vyberte **veřejné**.

5. V části **veřejnou IP adresu**vytvořte nový veřejný IP s názvem **myPublicIP**.

6. V části pole Skupina zdroje vyberte **myRG**. Vyberte vhodné **umístění**a klikněte na **OK**. Vyrovnávání zatížení potom začne nasazení a bude trvat několik minut úspěšně nasazení.

![Aktualizace zdroje okruhem Vyrovnávání zatížení](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Vytvoření fondu back-end adres

1. Po úspěšném má zavedení Vyrovnávání zatížení, vyberte z v rámci svých prostředcích. V části Nastavení vyberte back-end fondů. Zadejte název vašeho back-end fondu. Klikněte na tlačítko **Přidat** směrem k horní části zásuvné, který se zobrazuje.

2. Klikněte na **Přidat do virtuálního počítače** do zásuvné **fondu back-end přidat** .  **Klikněte na dostupnost nastavit** v části **dostupnost nastavení** a vyberte **myAvailSet**. Potom vyberte v části virtuálních počítačích v zásuvné **Zvolte virtuálních počítačích** a klikněte na **web1** a **webu 2**, dvě VMs vytvořené pro vyrovnávání zatížení. Zajištění obě modré značky zaškrtnutí vlevo jak je znázorněno na následujícím obrázku. Potom klikněte na **Výběr** v této zásuvné následovaný OK v zásuvné **Zvolte virtuálních počítačích** a potom na **OK** v zásuvné **Přidat back-end fond** .

    ![Přidání do fondu adresu back- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Zkontrolujte, že vám oznámení rozevírací seznam obsahuje aktualizace týkající se ukládání fondu back-end Vyrovnávání zatížení kromě aktualizace rozhraní sítě pro VMs **web1** a **webu 2**.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Vytváření zkušební, LB pravidla a pravidla překladu síťových adres

1. Vytvořte zkušební stavu.

    V části nastavení Vyrovnávání zatížení vyberte sond. Klikněte na **Přidat** umístěný v horní části zásuvné.

    Existují dva způsoby, jak konfigurovat zkušební: HTTP nebo TCP. Tento příklad ukazuje, že HTTP, ale TCP možné konfigurovat podobným způsobem.
    Aktualizujte potřebné informace. Jak je uvedeno, **myLoadBalancer** načte zůstatek přenosy na Port 80. Vybraná cesta je HealthProbe.aspx, Interval je 15 sekund, a chybná prahová hodnota 2. Až budete hotovi, klikněte na **OK** vytvořte zkušební.

    Podržte ukazatel myši nad písmeno i další informace o těchto jednotlivé konfigurací a jak lze změnit na vyberte vašich požadavků na ikonu.

    ![Přidání zkušební](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Vytvoření pravidla Vyrovnávání zatížení.

    Klikněte na pravidla v části nastavení Vyrovnávání zatížení vyrovnávání zatížení. V nové zásuvné klikněte na **Přidat**. Zadejte název pravidla. Tady je HTTP. Zvolte frontend port a back-end port. Tady je vybrán 80 pro obě. Zvolte **LB back-end** jako Backendovou fondu a dříve vytvořené **HealthProbe** jako zkušební. Ostatní konfigurace lze nastavit podle vašich požadavků. Klikněte na OK uložte pravidlo pro vyrovnávání zatížení.

    ![Přidání pravidlo Vyrovnávání zatížení](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Vytvoření příchozí pravidla překladu síťových adres

    Klikněte na překladu síťových adres příchozí pravidla v části nastavení Vyrovnávání zatížení. V nové zásuvné, klikněte na **Přidat**. Pojmenujte příchozí pravidla překladu síťových adres. Tady je označená jako **inboundNATrule1**. Cíl by měl být veřejnou IP dřív vytvořili. Vyberte vlastní v rámci služby a zvolte protokol, který chcete použít. Tady je vybrán TCP. Zadejte číslo portu 3441 a cílový port v tomto případě 3389. Klikněte na OK uložte toto pravidlo.

    Po vytvoření první pravidlo opakujte tento krok pro druhý příchozí pravidla překladu síťových adres s názvem inboundNATrule2 z port 3442 na cílový port 3389.

    ![Přidání pravidlo pro příchozí připojení překladu síťových adres](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Odebrání Vyrovnávání zatížení

Pokud chcete odstranit Vyrovnávání zatížení, vyberte Vyrovnávání zatížení, který chcete odebrat. V zásuvné *Vyrovnávání zatížení* klikněte na **Odstranit** umístěný v horní části zásuvné. Klikněte na **Ano** po zobrazení výzvy.

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
