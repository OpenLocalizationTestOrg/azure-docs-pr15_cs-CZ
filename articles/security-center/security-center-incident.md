<properties
   pageTitle="Zpracování incidentu zabezpečení v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument vám pomůže při používání funkcí Centrum zabezpečení Azure zpracovávání zabezpečení incidentem."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Zpracování incidentu zabezpečení v Centru zabezpečení Azure 
Triaging a vyšetřování výstrah zabezpečení může být časově náročný pro i nejčastěji kvalifikovaných analytiky zabezpečení a pro mnoho je těžké si dokonce vědět, kde začít. Pomocí [technologie pro analýzu](security-center-detection-capabilities.md) připojení informací mezi různých [výstrah zabezpečení](security-center-managing-and-responding-alerts.md)Centrum zabezpečení může poskytnout jednoho zobrazení útok kampaní a všech souvisejících upozornění – můžete rychle zjistit, jaké akce mohl přijal a zdrojích bylo vliv.

Tento dokument popisuje, jak použít funkcí upozornění zabezpečení v Centru zabezpečení pro usnadnění zpracování zabezpečení incidentem.


## <a name="what-is-a-security-incident"></a>Co je incidentu zabezpečení?

V Centru zabezpečení je zabezpečení incidentu agregaci všechna upozornění zdroje, které zarovnání vzorky [Ukončit řetězce](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Dlaždice [Výstrah zabezpečení](security-center-managing-and-responding-alerts.md) a zásuvné zobrazí incidentem. K incidentu odkryjete seznamu související upozornění, která umožňuje získat další informace o každém výskytu.

## <a name="managing-security-incidents"></a>Správa zabezpečení události

Aktuální incidentem zabezpečení můžete zkontrolovat pohledem na dlaždici upozornění zabezpečení. Přístup na portál Azure a postupujte podle pokynů v tématu Další informace o jednotlivých incidentu zabezpečení:

1. Na řídicím panelu Centra zabezpečení zobrazí se dlaždici **výstrah zabezpečení** .

    ![Dlaždice výstrah zabezpečení v Centru zabezpečení](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klikněte na této dlaždici rozbalte a pokud je zjištěno incidentu zabezpečení, se zobrazí v části grafu upozornění zabezpečení, jak je ukázáno v následujícím příkladu:

    ![Incidentu zabezpečení](./media/security-center-incident/security-center-incident-fig2.png)

3.  Oznámení, že incidentu popis zabezpečení má jinou ikonu ve srovnání s ostatní upozornění. Klikněte na ni zobrazíte další informace o této události.

    ![Incidentu zabezpečení](./media/security-center-incident/security-center-incident-fig3.png)

4.  Na **incidentu** zásuvné, zobrazí se další podrobnosti týkající se tohoto incidentu zabezpečení, který obsahuje celou popis, závažnosti (což je v tomto případě vysoké), jeho aktuálním stavu (v tomto případě je stále *aktivní*, což znamená, uživatel nepřijal akce *zavřete* ho – stačí klepnutím pravým tlačítkem myši na incidentu v zásuvné **výstrah zabezpečení** ) , napaden zdroje (v tomto případě *VM1*) nápravné kroky pro případ, a v dolní části okna máte upozornění, které jsou součástí tohoto incidentu. Pokud chcete získat další informace o jednotlivých upozornění, klepněte na a jiné zásuvné otevře se, jak je ukázáno v následujícím příkladu:

    ![Incidentu zabezpečení](./media/security-center-incident/security-center-incident-fig4.png)

Informace o tomto zásuvné budou lišit podle toho, na upozornění. Další informace o tom, jak spravovat tyto upozornění číst [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) . Některé důležité informace týkající se tato možnost:

- Nový filtr umožňuje přizpůsobit zobrazení k incidentu pouze nebo jenom upozornění. 
- Upozornění na stejné může existovat v rámci události (pokud existuje) i mají být zobrazeny jako samostatný upozornění. 
- Zavřením incident nebude zavřete související upozornění.

## <a name="see-also"></a>Viz taky

V tomto dokumentu se naučíte používat funkci incidentu zabezpečení v Centru zabezpečení. Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md)
- [Funkce rozpoznávání Centrum zabezpečení Azure](security-center-detection-capabilities.md)
- [Centrum zabezpečení Azure plánování a příručka](security-center-planning-and-operations-guide.md)
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md)
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md)– najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/)– najít blog týkající se Azure zabezpečení a dodržování předpisů.
