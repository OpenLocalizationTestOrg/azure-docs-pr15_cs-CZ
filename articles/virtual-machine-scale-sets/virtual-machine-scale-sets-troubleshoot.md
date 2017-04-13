<properties
    pageTitle="Poradce při potížích s automatické měřítko sadami měřítko virtuálního počítače | Microsoft Azure"
    description="Poradce při potížích automatické měřítko sadami měřítko virtuálního počítače. Porozumět typické problémy a jejich řešení."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Poradce při potížích s automatické měřítko sadami měřítko virtuálního počítače

**Problém** – infrastrukturu neobsahovaly text jste vytvořili v Azure správce prostředků pomocí OM měřítko sady – například nasazení šablony takto: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – máte měřítko pravidel definované a funguje skvěle, s tím rozdílem, že bez ohledu na to, kolik zatížení umístění na VMs nebude automatické měřítko.

## <a name="troubleshooting-steps"></a>Návody na řešení potíží

Několik věcí k zamyšlení patří:

- Kolik jádra každý OM nemá a načítáte každý základní?
 Šablona rychlý úvod Azure příkladu výše obsahuje do_work.php skript, který načítá jednoho základní. Pokud používáte OM větší než jeden základní OM velikost jako Standard_A1 nebo D1 potom by potřeba spustit tento zatížení tisknutím. Kontrola, kolik cores vaše VMs kontrolou [velikosti pro Windows virtuálních počítačích v Azure](../virtual-machines/virtual-machines-windows-sizes.md)

- Kolik VMs v OM měřítko sadě máte práce na každý z nich?

    Měřítko, událostí se jenom provede po průměr procesoru přes **všechny** VMs v sadě měřítko větší než mezní hodnota postupně vnitřní podle pravidel automatické měřítko.

- Zmeškáte všech událostí měřítko?

    Zaškrtněte políčko že protokoly auditování do portálu Azure měřítko události. Možná jste udělali měřítkem nahoru a zmeškané měřítko dolů které. Můžete filtrovat podle "Měřítko".

    ![Protokolů auditování][audit]

- Se měřítko se změnami a škálování prahové hodnoty dostatečně liší?

    Předpokládejme, že jste nastavili pravidla zobrazit když je větší než 50 % víc než 5 minut a měřítko při průměr CPU je menší než 50 % průměr CPU. "Flapping" problém by způsobily při využití procesoru zavřít tento mezní akcím měřítko trvale zvětšení a zmenšení velikosti sady. Z toho důvodu službu Automatické měřítko se snaží zabránit "sklápěcí", které můžete projevovat jako není měřítko. Proto zkontrolujte, jestli škálování a měřítko limity dostatečně odlišné umožňuje místo mezi měřítka.

- Píšete šablony JSON?

    Není těžké si uděláte chyby, takže použití šablony, jako je nad který ověřené práce a small Přírůstková změna. 

- Můžete ručně měníte či oddálení?

    Zkuste znovu nasazení nastavte měřítko OM zdroje s nastavením jiné "kapacita" Chcete-li změnit počet VMs ručně. Tady je příklad šablony k tomuto: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – budete muset upravit šablonu, kterou chcete ho jako sady měřítko používá musí mít stejnou velikost počítače. Pokud můžete úspěšně změníte počet VMs ručně, pak víte, že je to problém automatické měřítko.

- Kontrola vaší Microsoft.Compute/virtualMachineScaleSet a Microsoft.Insights zdroje v okně [Průzkumníka zdroje Azure](https://resources.azure.com/)

    Toto je nezbytné nástroj řešení potíží, které uvidíte stav správce prostředků Azure zdrojů. Klikněte na předplatné a podívejte se na řešení skupina zdroje. V části zprostředkovatele pro využití prostředků podívejte se na OM měřítko sadu jste vytvořili a kontrolovat Instance zobrazení, které uvidíte stav na nasazení. Taky zaškrtněte instance zobrazení VMs v sadě měřítko OM. Potom přejděte do zprostředkovatele prostředků Microsoft.Insights a zkontrolujte, že dobře vypadají pravidla automatické měřítko.

- Diagnostické rozšíření práce a výstupu data o výkonu?

    __Aktualizace:__ Automatické Azure měřítko byla rozšířeného používat hostitele na základě metriky kanálem k odesílání zpráv už vyžadující příponu diagnostiky je třeba nainstalovat. To znamená následujícím: několik odstavců už vztahovat v případě, vytvoření aplikace neobsahovaly text přes nový kanál. Příklady Azure šablony, která byla převedena na použít kanálu Host (hostitel): https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Použití hostitele na základě metriky pro automatické měřítko je lepší z těchto důvodů:

    - Méně proměnné části jako bez přípony diagnostiky třeba nainstalovat.
    - Jednodušší šablony. Jenom přidejte přehledy automatické měřítko pravidla do existující šablony sadu měřítko.
    - Spolehlivější vytváření sestav a rychleji zahájení nového VMs.

    Pouze důvody, proč že můžete chtít mít ve tvaru přípony diagnostiky podle velikosti zároveň potřebnosti paměti diagnostiky vykazování nebo měřítko. Host (hostitel) na základě metriky nemá zprávu paměti.

    Si uvědomit pouze pokud, udělejte ve zbývající části tohoto článku stále používáte diagnostiky rozšíření vaší neobsahovaly text.

    Automatické měřítko v Azure správce můžete pracovat (ale už má) pomocí virtuálního počítače rozšíření s názvem koncovku diagnostických nástrojů. Posílá data o výkonu k účtu úložiště, které definujete v šabloně. Tato data se pak agregované službou Azure Monitor.

    Pokud službu přehledy nemůže přečíst data z VMs, by měl odeslat e-mail – například kdyby VMs dolů, takže sledujte e-mailu (jiný, který jste zadali při vytváření účet Azure).

    Taky můžete vyhledat a prohlížet data. Podívejte se na účet Azure úložiště pomocí Průzkumníka cloudu. Příklad použití [Visual Studio cloudu Průzkumníka](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), přihlaste se a vyberte Azure předplatné používáte, a název účtu úložiště diagnostiky odkazuje definici rozšíření Diagnostika v šabloně nasazení.

    ![Průzkumník cloudu][explorer]

    Tady uvidíte spoustu tabulky, kde jsou data z jednotlivých OM uložena. Pořizování Linux a míru procesoru jako příklad vypadat při posledních řádků. Průzkumník cloudu Visual Studio podporuje což je dotazovací jazyk tak spustíte dotaz jako "časové razítko gt data a času" 2016 – 02-02T21:20:00Z "" a ujistěte se, se zobrazí posledních události (předpokládá čas je UTC). Pracuje s daty, které vidíte v nějaké odpovídají pravidla měřítko nastavení? V následujícím příkladu procesoru počítače 20 začít zvýšením na 100 % nad poslední 5 minut.

    ![Úložiště tabulek][tables]

    Pokud není data, pak předpokládá, že je problém s příponou diagnostiky aplikaci VMs. Pokud jsou data tam, znamená to, že máte potíže s pravidly měřítko nebo se službou přehledy. Kontrola [stavu Azure](https://azure.microsoft.com/status/).

    Jakmile jste byli tímto postupem, pokud se budou dál problémy automatické měřítko může zkuste to na fórech na [webu MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)nebo [poskládat přetečení](http://stackoverflow.com/questions/tagged/azure)nebo přihlášení hovoru podpory. Připravte se na sdílení šablony a zobrazit údaje o výkonu.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
