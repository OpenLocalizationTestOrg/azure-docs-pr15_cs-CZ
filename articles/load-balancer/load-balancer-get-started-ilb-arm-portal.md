<properties
   pageTitle="Začít vytvářet Vyrovnávání zatížení vnitřní v správce na portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení vnitřní v správce na portálu Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Vytvoření Vyrovnávání zatížení vnitřní v portálu Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Vytvořit Vyrovnávání zatížení interní Azure portálu

Pomocí následujících kroků k vytvoření Vyrovnávání zatížení vnitřní z portálu Microsoft Azure.

1. Otevřete prohlížeč a přejděte na [portál Azure](http://portal.azure.com)přihlášení pomocí účtu Azure.
2. V horní levé části obrazovky, klikněte na **Nový** > **sítě** > **Vyrovnávání zatížení**.
3. V zásuvné **Služba Vyrovnávání zatížení vytvořit** zadejte **název** pro vyrovnávání zatížení.
4. V části **schéma**klikněte na **vnitřní**.
5. Klikněte na **virtuální sítě**a potom vyberte virtuální síť, kde chcete vytvořit Vyrovnávání zatížení.

    >[AZURE.NOTE] Pokud nevidíte virtuální sítě, který chcete použít, zkontrolujte **umístění** používáte pro vyrovnávání zatížení a ho měnit.

6. Klikněte na **podsítě**a pak vyberte místo, kam chcete vytvořit Vyrovnávání zatížení podsítě.
7. V části **přiřazení IP adresy**klikněte na **dynamické** nebo **statické**, podle toho, jestli chcete IP adresa Vyrovnávání zatížení opravit (statická) nebo ne.

    >[AZURE.NOTE] Pokud vyberete statickou IP adresu, budete muset zadat adresu pro vyrovnávání zatížení.

8. V části **Skupina zdroje** zadejte název nové skupiny prostředků pro vyrovnávání zatížení, nebo klikněte na tlačítko **Vybrat stávající** a vyberte existující skupiny zdrojů.
9. Klikněte na **vytvořit**.

## <a name="configure-load-balancing-rules"></a>Konfigurace pravidel pro vyrovnávání zatížení

Po vytvoření Vyrovnávání zatížení přejděte na zdroje Vyrovnávání zatížení ji nakonfigurovat.
Budete muset před konfigurace služby Vyrovnávání pravidlo zatížení konfigurace nejdřív fondu back-end adres a zkušební.

### <a name="step-1-configure-a-back-end-pool"></a>Krok 1: Konfigurace fond back-end

1. Na portálu Azure klikněte na tlačítko **Procházet** > **Vyrovnávání zatížení**a potom klikněte na Vyrovnávání zatížení jste vytvořili výše.
2. V **Nastavení** zásuvné klikněte na **back-end fondů**.
3. V zásuvné **fondy adres back-end** klikněte na **Přidat**.
4. V zásuvné **fondu přidat back-end** zadejte **název** skupiny back-end a pak klikněte na **OK**.

### <a name="step-2-configure-a-probe"></a>Krok 2: Nakonfigurujte zkušební

1. Na portálu Azure klikněte na tlačítko **Procházet** > **Vyrovnávání zatížení**a potom klikněte na Vyrovnávání zatížení jste vytvořili výše.
2. V **Nastavení** zásuvné klikněte na **sond**.
3. V zásuvné **sond** klikněte na **Přidat**.
4. V zásuvné **Přidat zkušební** zadejte **název** pro zkušební.
5. V části **Protocol (protokol)**vyberte **HTTP** (pro weby) nebo **TCP** (pro jiné aplikace na základě TCP).
6. V části **Port**zadejte port při přístupu k zkušební.
7. V části **Path** (pouze sond HTTP) zadejte cestu pro použití zkušební.
8. V části **Interval** způsob často probe aplikace.
9. V části **Chybná prahová hodnota**zadejte, kolik pokusy by měl nezdaří před virtuálního počítače back-end označena jako chybná.
10. Klikněte na **OK** vytvořte zkušební.

### <a name="step-3-configure-load-balancing-rules"></a>Krok 3: Nastavení pravidel pro vyrovnávání zatížení

1. Na portálu Azure klikněte na tlačítko **Procházet** > **Vyrovnávání zatížení**a potom klikněte na Vyrovnávání zatížení jste vytvořili výše.
2. V **Nastavení** zásuvné klikněte na **Vyrovnávání zatížení pravidla**.
3. V zásuvné **Vyrovnávání zatížení pravidla** klikněte na **Přidat**.
4. V zásuvné **Přidat načíst vyrovnávání pravidla** zadejte **název** pravidla.
5. V části **Protocol (protokol)**vyberte **HTTP** (pro weby) nebo **TCP** (pro jiné aplikace TCP založeny).
6. V části **Port**zadejte jména že klienti port připojit v Vyrovnávání zatížení.
7. V části **port back-end**zadejte port pro použití v fondu back-end (obvykle port Vyrovnávání zatížení a port back-end jsou stejná).
8. V části **back-end fondu**vyberte fondu back-end, kterou jste vytvořili výše.
9. V části **zachování relace**vyberte požadovaný způsob relace uchovávat.
10. V části **nečinnosti (v minutách)**zadejte časový limit nečinnosti.
11. V části **Plovoucí IP (zpáteční přímé server)**klikněte na **Zakázané** nebo **Povolit**.
12. Klikněte na **OK**.

## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)