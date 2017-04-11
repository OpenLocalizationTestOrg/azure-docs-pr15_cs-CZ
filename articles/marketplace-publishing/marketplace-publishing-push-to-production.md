<properties
   pageTitle="Nasazení vaše nabídka z Azure Marketplace | Microsoft Azure"
   description="Další informace o a projděte si pokyny pro nasazení vaši nabídku – virtuálního počítače obrázek vývojář služby, datové služby, atd – Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Nasazení vaše nabídka z Azure Marketplace
Až budete spokojeni s vaší nabídky (to znamená testování zákazníka scénáře marketing obsah, atd.) a budete chtít spustit, požádejte **nabízená výroby** na kartě **Publikovat** .  

1. Čtyři kroky podle NÁVODU stránce publikování portál by měl být dokončený a zelené. Virtuální počítač nabídky ověřte, že jsou řídit následujícími pokyny.

    ![Kreslení][img-pubportal-walkthru-checked]

2. Vyberte kartu **Publikovat** v seznamu na levé straně.

    ![Kreslení][img-pubportal-menu-publish]

3. Klikněte na tlačítko **požádat o schválení posunout výroby**. Po zadání požadavku, týmu schválení provede závěrečné revizi a pak vaši nabídku budou k dispozici v Azure Marketplace.

    ![Kreslení][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Po kliknutí na tlačítko schválení žádosti o posunout výroby, v případě virtuálních počítačích provádí následující kroky za scény. Budete moct zobrazit průběh jednotlivými kroky na kartě Publikovat ve publikování portálu. Je třeba zkontrolovat tuto stránku v pravidelných intervalech (stav zobrazuje "Uvedené"), dokud pro selhání informace, které potřebujete oprav od vašeho konce.

> - Nejdřív žádosti o výrobní půjde certifikační týmu, který ověřit virtuální pevný disk. Ale pokud aktualizujete už uvedených nabídky a žádost má máte jenom marketingové změnit, klikněte certifikační krok vynechán.
> - V dalším kroku žádost být obsahu ověření týmu, který ověření marketingového obsahu nabídky.
> - Jsou-li výše uvedené kroky úspěšné, ve výrobním schválené nabídky. V současné době stav stanou "podle" portál publikování. Tenhle stav "Uvedené" však neznamená dokončení procesu. Podle těchto kroků musejí být úplný před nabídky je k dispozici v Azure Marketplace.
> - Po schválení nabídky v výrobní v předchozím kroku replikace nabídky začít přes Azure datacentrech. Obecně trvá 24 48hours replikace dokončete ale můžou projevit až po týdnu v závislosti na velikosti virtuální pevný disk. Pokud aktualizujete už uvedených nabídky a má máte jenom marketingové změnit, pak replikace je však rychlejší.
> - Po dokončení replikace nabídky nebude k dispozici v Azure Marketplace.

> Vždy můžete odstranit nabídky dokud je ve stavu **koncept** (tedy nikdy **použít pro pracovní** nebo **nabízených výroby**). Na kartě **Historie** kliknutím na tlačítko **Zrušit koncept** v dolní části na stránce odstranění koncept.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Kontrolní seznam výrobní pro všechny nabídky virtuálního počítače

- Zajistit, aby byly partnera certifikované Microsoft Azure
- Na kartě skladové jednotky možnost "Skrýt skladové jednotky z Marketplace vzhledem k tomu, že by měl koupili vždy pomocí šablony řešení" mají označit jako Ano jenom v případě, že Skladovou je součástí šablony řešení. Ve všech ostatních případech mají možnost vždy označit jako číslo.
- Pamatujte si: Neměňte SKU viditelnost nastavení po Skladovou koncovém. Tato funkce nepodporujeme.
- Ujistěte se, že loga pravidlům Azure Marketplace logo uvedeny níže.
- Nabídka a SKU popis by neměly být stejný.
- Název společnosti SKU a nabízejí dlouhé souhrnné by neměly být stejný.
- Název SKU a nabízejí souhrn by neměly být stejný.
- Názvy SKU neměli k nabídce shodovat s více skladové jednotky.

**Pokyny k používání logo Azure Marketplace**

- Návrh Azure má jednoduchou barevné palety. Udržujte počet primární a sekundární barvy na loga nízké.
- Barvy motivu portálu Azure jsou bílé a černé. Vyhněte se tedy tyto barvy podle barvy pozadí svého loga. Použití některé barvy, které byste měli přesně svého loga výrazném Azure portálu. Doporučujeme jednoduchý primární barvy. Pokud používáte průhledným pozadím, ujistěte se, že logo/textu není zbělá nebo zčerná.
- Nepoužívejte pozadí s barevným přechodem na logo.
- Vyhněte se umístění textu, i vaší společnosti nebo název značky na logo.
- Vzhled a chování loga by měl být "ploché" a vyhněte se přechody.
- Logo neměli roztažen.

**Další pokyny pro obrázek loga:**

- Logo obrázek je nepovinný krok. Publisheru můžete vypnout nahrát logo obrázek. **Ale jednou nahraný ikonu obrázek nepůjde odstranit ze publikování portálu. V té době partnera musí postupujte podle pokynů Azure Marketplace obrázek ikony dalšího nebude schválené nabídky výroby.**
- Publisher zobrazované jméno, název SKU a nabídky dlouho souhrnné zobrazují barevně bílé písma. Proto neměli byste uchovávání světlou barvu pozadí na ikonu obrázek. Černé, bílé a průhledné pozadí není povoleno pro obrázek ikony.
- Vydavatel zobrazují název, název SKU, nabídky dlouho souhrnné a na tlačítko Vytvořit jsou vložené programově uvnitř logo obrázek po nabídky přejde uvedené. Při vytváření logo obrázek tak neměli zadat libovolný text. Nechejte prázdné místo na pravé straně protože text (tedy Publisher zobrazované jméno, název SKU nabídky dlouho souhrnné) bude obsahovat programově tak, že nám myší tam. Prázdný text by měl být 415 × 100 na pravé straně (a posun podle 370px vlevo).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Nabízí další výrobní kontrolního seznamu pro už uvedených virtuálního počítače

- Zkontrolujte, jestli se už nabídky se stejným názvem nabídky z vaší společnosti. Pokud ano, měli byste do nabídky stávající místo abyste vytvářeli novou duplicitní nabídku Přidat novou verzi SKU.
- Datový disk neměňte mezi dvou verzí stejného SKU.
- Azure Marketplace nepodporuje ceny změn uvedených skladové jednotky, jako ovlivňuje fakturace stávajících zákazníků. Ujistěte se, že se nezmění ceny SKU uvedených v oblasti, kde je k dispozici SKU. Však můžete přidat nové skladové jednotky nebo přidat nové oblasti do existující SKU.


## <a name="next-steps"></a>Další kroky
Jakmile nabídky dostane živou, otestujte scénáři zákazníka a ověřte, že všechny smlouvy a funkce fungují správně v provozním prostředí jako testovaná a schválená v testovacím prostředí.

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
