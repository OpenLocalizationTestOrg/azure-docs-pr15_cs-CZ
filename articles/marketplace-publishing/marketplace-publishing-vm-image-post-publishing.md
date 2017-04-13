<properties
   pageTitle="Správa virtuálního počítače obrázek na Azure Marketplace | Microsoft Azure"
   description="Podrobné pokyny týkající se správy virtuálního počítače obrázek na Azure Marketplace po počáteční publikaci."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Po výrobní Průvodce pro virtuální počítač nabídky v Azure Marketplace

Tento článek vysvětluje, jak můžete aktualizovat nabídky k živé virtuálního počítače v Azure Marketplace. Také vás na postup přidání jednoho nebo více nové skladové jednotky do existující nabídky a odebrání živou nabídky virtuálního počítače nebo SKU z Azure Marketplace.

Po nabídky/SKU je připravené na [Portál Azure](http://portal.azure.com), nelze změnit pole uvedeny níže:

- **Nabízejí identifikátor:** [Portál publikování -> virtuálních počítačích -> Vybrat vaši nabídku -> kartu obrázky OM -> nabízejí identifikátor]
- **SKU identifikátor:** [Portál publikování -> virtuálních počítačích -> Vybrat -> vaši nabídku skladové jednotky -> karta přidat jinou SKU]
- **Namespace Publisheru:** [Portál publikování -> virtuálních počítačích -> návodu tab -> Řekněte nám o vaší společnosti (najdete v části "Kroku 2 zaregistrovat firma") -> Publisher Namespace -> Namespace]

Po nabídky/SKU je uvedená v [Azure Marketplace](http://azure.microsoft.com/marketplace), nelze změnit pole uvedeny níže:

- **Nabízejí identifikátor:** [Portál publikování -> virtuálních počítačích -> Vybrat vaši nabídku -> kartu obrázky OM -> nabízejí identifikátor]
- **SKU identifikátor:** [Portál publikování -> virtuálních počítačích -> Vybrat -> vaši nabídku skladové jednotky -> karta přidat jinou SKU]
- **Namespace Publisheru:** [Portál publikování -> virtuálních počítačích -> návodu -> karta Namespace Publisher Řekněte nám o firma (najdete v části Krok 2 registrovat) -> Namespace]
- **Porty** [Portál publikování -> virtuálních počítačích -> Vybrat vaši nabídku -> kartu obrázky OM -> Otevřít porty]
- **Změna uvedených SKU(s) ceny**
- **Změna modelu z uvedených SKU(s) fakturace**
- **Odebrání fakturačního oblastí seznamu SKU(s)**
- **Změna data disku počet hodnot v seznamu SKU(s)**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. o aktualizaci technické podrobnosti jinou SKU

Můžete přidat novou verzi uvedených SKU a znova publikovat vaši nabídku podle pokynů uvedených níže uvedených:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **OM obrázky** .
4. V oddílu **skladové jednotky** karty **OM obrázky** vyhledejte SKU, který chcete aktualizovat.
5. Poté přidat novou verzi počet SKU a klikněte na tlačítko **"a"** . Novou verzi by měl být X.Y.Z formátu, kde X, Y a Z jsou celá čísla. Verze změny by měl být pouze přírůstková.
6. Do pole **Adresa URL virtuální pevný disk OS** přidat sdílený přístup podpis, který URI vytvořené pro operační systém virtuálního pevného disku a uložte požadované změny.

    >[AZURE.IMPORTANT] Se nedá přírůstek/snižovat dat disku počet hodnot v seznamu SKU. Potřebujete k vytvoření nové SKU v tomto případě. Získáte v části [3. Jak přidat nové skladové jednotky klikněte v části nabídky k uvedení](#3-how-to-add-a-new-sku-under-a-live-offer) podrobné pokyny.

7. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
8. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. o aktualizaci netechnických podrobnosti o nabídku nebo jinou SKU

Můžete aktualizovat netechnických (marketing právní, podporují, kategorie) podrobnosti živou nabídky nebo SKU v Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 aktualizovat popis nabídky a loga

Můžete aktualizovat podrobnosti nabídky a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartě **marketingové** .
4. Klikněte na tlačítko **ANGLIČTINA (USA)** .
5. V nabídce na levé straně straně klikněte na kartu **Podrobnosti** . Na kartě **Podrobnosti** v části *Popis* můžete aktualizovat název nabídky, nabízejí souhrn, nabízejí dlouhé souhrn a uložte požadované změny.

    >[AZURE.NOTE] Zkontrolujte starat o následující při aktualizaci podrobností SKU.
    **Duplicitní text v části Popis nabídky a popis SKU není třeba zadávat. Duplicitní text pod názvem SKU a nabídky dlouho souhrnné není třeba zadávat. Není třeba zadávat duplicitní text pod názvem SKU a souhrn nabídky.**

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. V části *loga* na kartu **Podrobnosti** můžete aktualizovat loga. Ale zajistit, že loga řiďte se [pokyny Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (naleznete v části Krok 1: marketingového obsahu poskytují Marketplace -> Podrobnosti -> Azure Marketplace Logo pokyny).

    >[AZURE.NOTE] Obrázek ikony je nepovinný krok. Můžete se rozhodnete nahrát obrázek ikonu. Po nahrání obrázek ikonu potom je však žádné ustanovení a vymažete ho ze publikování portálu. V takovém případě postupujte podle [pokynů ikonu obrázek](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (naleznete v části Krok 1: marketingového obsahu poskytují Marketplace -> Podrobnosti -> Další pokyny pro obrázek loga nápis).

7. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí získáte tento [odkaz](marketplace-publishing-vm-image-test-in-staging.md).
8. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Aktualizujte popis SKU

Můžete aktualizovat podrobnosti SKU a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartě **marketingové** .
4. Klikněte na tlačítko **ANGLIČTINA (USA)** .
5. V nabídce na levé straně straně klikněte na kartu **plány jednotného zasílání zpráv** . Na kartě **plány jednotného zasílání zpráv** v části *skladové jednotky* můžete aktualizovat SKU název, SKU souhrnných a SKU popis podrobnosti a uložte požadované změny.

    >[AZURE.NOTE] Zkontrolujte starat o následující při aktualizaci podrobností SKU. **Duplicitní text v části Popis nabídky a popis SKU není třeba zadávat. Duplicitní text v části nadpis SKU a nabídky dlouho souhrnné není třeba zadávat. Není třeba zadávat duplicitní text pod názvem SKU a souhrn nabídky.**

6. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto odkaz
7. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 změnit existující odkazy nebo přidat nové odkazy

Můžete změnit existující odkazy nebo přidat nové odkazy a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartě **marketingové** .
4. Klikněte na tlačítko **ANGLIČTINA (USA)** .
5. V nabídce na levé straně straně klikněte na kartě **odkazy** .
6. Pokud chcete přidat nový odkaz, klikněte v části *odkazy* části klikněte na tlačítko **Přidat odkaz** . Otevře se dialogové okno *"Přidat odkaz"* . V tomto dialogovém můžete přidat odkaz nadpis a URL polí a uložte požadované změny. Můžete zadat jakýkoli odkaz, který obsahuje informace, které vám mohou pomoci zákazníků.
7. Pokud chcete aktualizovat nebo odstranit směřuje stávající odkaz, potom vyberte příslušný odkaz a klikněte na tlačítko Upravit nebo odstranit příslušným způsobem.

    >[AZURE.NOTE] Ujistěte se, že odkazy, které jste zadali v této části pracují správně, tak, jak tyto odkazy získat ověřit během výrobním procesu žádost o.

8. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí získáte tento [odkaz](marketplace-publishing-vm-image-test-in-staging.md).
9. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 změnit existující obrázek vzorku nebo přidat nový obrázek vzorku

Můžete změnit existující ukázkové obrázky nebo přidat nové ukázkové obrázky a znova publikovat vaši nabídku podle následujících kroků:

>[AZURE.NOTE] Pouze jedné ukázky obrázek se zobrazí v [https://portal.azure.com](https://portal.azure.com).

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartě **marketingové** .
4. Klikněte na tlačítko **ANGLIČTINA (USA)** .
5. V nabídce na levé straně straně klikněte na kartu **UKÁZKOVÉ obrázky** .
6. Pokud chcete přidat nový obrázek vzorku, v části *Ukázka obrázků* klikněte na tlačítko **Odeslat nový OBRÁZEK** a potom uložit změny.

    >[AZURE.NOTE] Obrázek vzorku včetně krok je volitelný.

7. Pokud chcete aktualizovat nebo odstranit existující obrázek vzorku, a pak najděte odpovídající Ukázkový obrázek a klikněte na tlačítko **Nahradit OBRÁZEK** nebo na tlačítko Odstranit příslušným způsobem.

8. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí získáte tento [odkaz](marketplace-publishing-vm-image-test-in-staging.md).
9. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 aktualizace obsahu právního charakteru

Můžete aktualizovat obsahu právního charakteru a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartě **marketingové** .
4. Klikněte na tlačítko **ANGLIČTINA (USA)** .
5. V nabídce na levé straně straně klikněte na kartu **právní** . V části *právní* můžete aktualizovat vaše zásady/podmínkám použití. Zadejte nebo vložte zásady/termíny v textovém poli *Podmínky použití* a uložte požadované změny.
6. Omezení počtu znaků pro právní podmínky použití je 1,000,000 znaků.
7. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
8. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 aktualizovat informace o podpoře

Můžete aktualizovat informace o podpoře a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **Podpora** .
4. V části *Technickým kontaktovat* **podporu** kartě můžete aktualizovat podrobné kontaktní informace. Tyto informace se používají pro interní komunikaci mezi partnera a Microsoft pouze.
5. V části *Zákaznická podpora* kartu **Podpora** můžete aktualizovat podpory kontakt jako **jméno, e-mailu, telefonu** a **Adresa URL podpory**. Tyto informace se používají pro interní komunikaci mezi partnera a Microsoft pouze.

    >[AZURE.NOTE] Pokud chcete jenom podporují e-mailu, zadejte formální telefonní číslo ve skupinovém rámečku **Zákaznická podpora** . V tomto případě e-mailu ujednaných použijí místo.

6. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
7. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 aktualizace kategorií

Můžete aktualizovat v části kategorie pro vaši nabídku a znova publikovat vaši nabídku podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com)
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **kategorie** .
4. V části *kategorie* můžete aktualizovat kategorie pro vaši nabídku a uložte požadované změny. Můžete vybrat až na pět kategorií v galerii Azure Marketplace.
5. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
6. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. postup, jak přidat nové skladové jednotky klikněte v části nabídky k seznamu

Podle pokynů uvedených níže uvedených můžete přidat nové skladové jednotky ve skupinovém rámečku živou nabídky:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **skladové jednotky** . Po kliknutí na tlačítko **Přidat A SKU**.  Otevře se dialogové okno Nový. Zadejte identifikátor SKU napsaných malými písmeny. Pokud chcete publikovat nové SKU s fakturační modelu BYOL, zaškrtněte políčko pro přenést e vlastní fakturační model(BYOL). Jinak zrušte zaškrtnutí políčka pro BYOL. Po kliknutí na značkami v dialogovém okně Vytvořit nové skladové jednotky. Pokud není pro fakturaci model BYOL pro nové skladové jednotky, pak fakturační modelu bude automaticky nastavit hodinové pro nové skladové jednotky. Pokud chcete povolit 30days bezplatnou zkušební verzi pro každou hodinu fakturační model, pak klikněte na možnost "Jeden měsíc" "neexistuje bezplatnou zkušební verzi?". V opačném případě vyberte "Bez zkušební verze". [Poznámka: možnost "je bezplatnou zkušební verzi dostupné?" se zobrazí pouze pokud není výběru BYOL v dialogovém okně při vytváření nové SKU.]

    >[AZURE.IMPORTANT] Možnost "Skrýt skladové jednotky z Marketplace vzhledem k tomu, že by měl koupili vždy pomocí šablony řešení" by měly být označené jako "Ano" jenom v případě, že schvalují pro publikování nabídky šablony k řešení v Azure Marketplace. V opačném tuto možnost vždy je nutné označit jako "Bez".

4. V nabídce na levé straně straně, klikněte na kartě **OM obrázky** a zjistit, nové skladové jednotky, který jste vytvořili.
5. Nastavení nové skladové jednotky, najdete pod odkazy KROKU 5 tento [odkaz](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) pokyny.
6. Přidat marketingové materiály pro nové skladové jednotky, najdete pod odkazy v části Krok 1: marketingového obsahu poskytují Marketplace -> Podrobnosti -> desetinnou čárkou 2 až 5 tohoto [odkazu](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Přidat informace o nové SKU cenách, najdete pod odkazy v části 2.1. Nastavení OM ceny tohoto [odkazu](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Po provedení změn, přejděte na kartu **publikování** a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
9. Po v pracovní testování vaši nabídku, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. jak chcete-li změnit počet disku dat pro uvedených SKU

Se nedá přírůstek/snižovat dat disku počet hodnot v seznamu SKU. V tomto případě je třeba vytvořit nové skladové jednotky. Získáte v části [3. Jak přidat nové skladové jednotky klikněte v části nabídky k živé](#3-how-to-add-a-new-sku-under-a-live-offer) podrobné pokyny.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. postup odstranění seznamu nabídka z Azure Marketplace

Existují různé aspekty, které je potřeba mít stará pro nečekané požadavek odebrat nabídky k živé. Postupujte podle pokynů a získat pokyny od týmu podpory odebrat uvedených nabídka z Azure Marketplace:

1.  Zvýšit požadavek podpory můžete pomocí tohoto [odkazu](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Vyberte typ problému jako **"Správa nabídky"** a vyberte kategorii jako **"Změnu nabídky nebo SKU už v výrobní"**
3.  Odeslání žádosti

Tým podpory vás provede odstranění nabídky/SKU.

>[AZURE.NOTE] Můžete kdykoli odstranit nabídky se nachází v stavem Koncept (tedy ne v pracovní nebo produkční) kliknutím na tlačítko **Zrušit koncept** klikněte v části **HISTORIE** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. jak odstranit uvedených SKU z Azure Marketplace

Podle pokynů uvedených níže uvedených můžete odstranit uvedených SKU z Azure Marketplace:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. Na bočním podokně vlevo klikněte na kartě **skladové jednotky** .
4. Vyberte SKU, který chcete odstranit a klikněte na tlačítko Odstranit týkající se tohoto SKU.
5. Po dokončení, přejděte na kartu publikovat na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat nabídky v Azure Marketplace.
6. Jakmile nabídky získá znovu publikovat v Azure Marketplace, Skladovou se odstraní z Azure Marketplace a portálu Azure.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. jak odstranit aktuální verzi uvedených SKU z Azure Marketplace

Podle pokynů uvedených níže uvedených můžete odstranit aktuální verzi uvedených SKU z Azure Marketplace. Po dokončení procesu Skladovou vrátit zpět předchozí verze.

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2.  Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3.  Na bočním podokně vlevo klikněte na kartu **OM obrázky** .
4.  Vyberte SKU jehož aktuální verzi chcete odstranit a klikněte na tlačítko Odstranit proti tuto verzi.
5.  Po dokončení, přejděte na kartu **Publikovat** na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat nabídky v Azure Marketplace.
6.  Jakmile nabídky získá znovu publikovat v Azure Marketplace, aktuální verzi uvedených SKU se odstraní z Azure Marketplace a portálu Azure. Skladovou se vrátit k předchozí verze.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. jak se vrátit výpis cena výrobní hodnoty
Můžu změnili ceny uvedených SKU (nebo jste odebrali fakturační oblastí seznamu SKU). Protože nejsou podporované v Azure Marketplace, budu chtít vrátit změny hodnoty výroby. Jak dosažení?

Postupujte podle pokynů uvedených níže:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **ceny** .
4. Na kartě ceny vyberte oblast, jehož ceny, který chcete obnovit.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. V případě SKU pomocí hodinách fakturační modelu obnovte ceny pro všechny vzorky jsou ve výrobním pro vybranou oblast. Pro skladové jednotky s fakturační modelu BYOL zpřístupnit Skladovou v oblasti zaškrtnutím políčka proti SKU v části Dostupnost SKU EXTERNALLY-LICENSED (BYOL) (viz snímek níže).

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Nyní klikněte na tlačítko **AUTOPRICE jiných trzích založené na ceny v Spojené státy**.

    >[AZURE.NOTE] Popisek na tlačítko se může lišit v závislosti na oblasti, které jste vybrali. Protože jsme zvolili Spojené státy při vytváření tento dokument, tak na tlačítko je označený jako "Automatického cenu dalších trzích podle cen v USA" ve následující snímek.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Otevře se Průvodce automatického cena. Na první stránce zobrazí výběru pro základní market. Volání oddíl a na další stránku můžete přejít kliknutím na tlačítko **"->"** .

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Možnost pro výběr jádra a plány se zobrazí na stránce 2. Vyberte požadované plány a jádra a klikněte na "->" tlačítko.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Stránka 3 zobrazí trzích/oblasti. Klikněte na tlačítko Přepnout vše vyberte všechny oblasti nebo ručně zaškrtněte políčka u oblastí. Klikněte na tlačítko "->" přejdete na další stránku.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Stránka 4 zobrazí kurzů. Klikněte na tlačítko Dokončit kroky. Průvodce obnoví ceny podle svého výběru.

11. Teď přejděte na kartu ceny a klikněte na tlačítko "Zobrazení Souhrn změny a".
Zaškrtnutím políčka "Koncept" v části "Zobrazení verze" a "Výrobní" v části "Porovnání s" (viz snímek níže). Pokud se zobrazí bez ceny rozdíl, znamená to, že ceny byl obnoven hodnoty výrobní úspěšně.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Po provedení změn, přejděte na kartu publikování a klikněte na tlačítko **NABÍZENÁ pracovní**. Podrobné pokyny k testování vaší nabídky v testovacím prostředí naleznete v tomto [odkaz](marketplace-publishing-vm-image-test-in-staging.md)
13. Po v pracovní testování vaši nabídku, přejděte na kartu publikovat na publikování portál a pak klepněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. jak se vrátit fakturační modelu výrobní hodnoty
Beru fakturační modelu uvedených SKU. Protože nejsou podporované v Azure Marketplace, budu chtít vrátit změny hodnoty výroby. Jak dosažení?

Postupujte podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **skladové jednotky** .
4. Klikněte na tlačítko Upravit vrátit fakturační modelu. Otevře se okno. Zaškrtněte nebo zrušte zaškrtnutí políčka **"fakturace a licence probíhá externě z Azure (označovaná taky jako přenést svůj vlastní licencí)"** příslušným způsobem.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Jednou najdete odpovědi na otázku 8 v tomto dokumentu se vrátit zpátky ceny.
6. Po, přejděte na kartu **Publikovat** na publikování portálem a nabízených nabídka přípravu ji otestujte. Jednou se dělají s testování nabídky a pak klikněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. jak pokud se chcete vrátit nastavení viditelnosti uvedených SKU hodnotě výrobní

Postupujte podle následujících kroků:

1. Přihlaste se k [portálu publikování](https://publish.windowsazure.com).
2. Přejděte na kartu **VIRTUÁLNÍCH počítačích** a vyberte vaši nabídku.
3. V nabídce na levé straně straně klikněte na kartu **skladové jednotky** .
4. Vyberte svůj SKU a vrátit se nastavení viditelnosti SKU hodnotě výroby.

    ![Kreslení](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Jednou jste hotovi se změnami a potom klikněte na tlačítko **Požádat o schválení k NABÍZENÁ k výrobní** znovu publikovat vaše nabídka z Azure Marketplace.

## <a name="see-also"></a>Viz taky
- [Začínáme: Jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
- [Principy prodejce přehledy vytváření sestav](marketplace-publishing-report-seller-insights.md)
- [Principy výběr sestav](marketplace-publishing-report-payout.md)
- [Změna vašeho poskytovatele cloudové řešení reseller pobídky](marketplace-publishing-csp-incentive.md)
- [Poradce při potížích s běžné problémy s publikováním na web Marketplace](marketplace-publishing-support-common-issues.md)
- [Získání podpory jako vydavatel](marketplace-publishing-get-publisher-support.md)
- [Vytvoření obrázku OM místní](marketplace-publishing-vm-image-creation-on-premise.md)
- [Vytvoření virtuálního počítače s Windows Azure náhled portálu](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
