
<properties
   pageTitle="Začít vytvářet internetové Vyrovnávání zatížení v modelu klasické nasazení na portálu Azure klasické | Microsoft Azure"
   description="Naučte se vytvářet internetové Vyrovnávání zatížení v modelu klasické nasazení na portálu Azure klasické"
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Začít vytvářet internetové Vyrovnávání zatížení (klasický) Azure klasické portálu

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete také [Naučte se vytvářet internetové Vyrovnávání zatížení pomocí Správce prostředků Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Nastavení Vyrovnávání zatížení internetového pro virtuálních počítačích

Chcete-li načíst zůstatek v síti z Internetu přes virtuálních počítačích do cloudové služby, musíte vytvořit sadu Vyrovnávání zatížení. Tento postup předpokládá, že jste již vytvořili virtuálních počítačích a že jsou všechny v rámci stejné cloudové služby.

**Konfigurace sady Vyrovnávání zatížení pro virtuálních počítačích**

1. Na portálu Azure klasické klikněte na **virtuálních počítačích**a potom klikněte na název virtuálního počítače v sadě Vyrovnávání zatížení.

2. Klikněte na **koncové body**a potom klikněte na **Přidat**.

3. Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na šipku doprava.

4. Na stránce **Zadejte podrobnosti koncového bodu** :

    * Do pole **název**zadejte název koncového bodu nebo vyberte název ze seznamu předdefinované koncové body pro běžné protokoly.
    * Ve skupinovém rámečku **protokol**vyberte protokol požadovaný typ koncového bodu, TCP a UDP, podle potřeby.
    * Do **portu veřejné a soukromé Port**zadejte požadovaný virtuálního počítače použít, v případě potřeby čísla portů. Můžete soukromé portů a pravidla brány firewall v počítači virtuální kterým přesměrujete přenosy tak, že je vhodné pro aplikaci. Privátní číslo portu může být stejné jako veřejné port. Například pro koncový bod pro přenos web (HTTP), mohli byste přiřadit port 80 veřejných a privátních port.

5. Vyberte možnost **vytvořit sadu Vyrovnávání zatížení**a potom klikněte na šipku doprava.

6. Na stránce **Konfigurovat nastavení Vyrovnávání zatížení** zadejte název sady Vyrovnávání zatížení a přiřaďte hodnoty pro zkušební chování služby Vyrovnávání zatížení Azure. Vyrovnávání zatížení pomocí sond zjistit, zda jsou k dispozici pro příjem příchozích virtuálních počítačích v sadě Vyrovnávání zatížení.

7. Klikněte na značku zaškrtnutí vytvoření koncového bodu Vyrovnávání zatížení. Zobrazí se **Ano** ve sloupci **název sady Vyrovnávání zatížení** stránce **koncové body** virtuálního počítače.

8. Na portálu klikněte na **virtuálních počítačích**, klikněte na název další virtuálního počítače v sadě Vyrovnávání zatížení, klikněte na **koncové body**a potom klikněte na **Přidat**.

9. Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na **koncový bod přidat do existující sady Vyrovnávání zatížení**, vyberte název sady Vyrovnávání zatížení a pak klikněte na šipku doprava.

10. Na stránce **Zadejte podrobnosti koncový bod** zadejte název koncového bodu a potom klikněte na značku zaškrtnutí.

Další virtuálních počítačích v sadě Vyrovnávání zatížení opakujte kroky 8 10.



## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

