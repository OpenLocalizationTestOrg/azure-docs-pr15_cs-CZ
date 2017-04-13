<properties
   pageTitle="Spravovat svazky StorSimple (U2) | Microsoft Azure"
   description="Vysvětluje, jak přidat, upravit, sledovat a odstraňovat StorSimple svazky a jak jako offline je v případě potřeby."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Použití službu StorSimple správce ke správě svazky (aktualizace 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak používat službu StorSimple správce můžete vytvořit a spravovat svazky na StorSimple zařízení a StorSimple virtuální zařízení s nainstalovanou aktualizací 2.

Služba StorSimple správce je rozšíření na portálu Azure klasické, který umožňuje spravovat řešení StorSimple z jedné webové rozhraní. Kromě Správa svazky, můžete službu StorSimple správce a vytvořit a spravovat StorSimple služby a zařízení můžete spravovat, zobrazit upozornění a zobrazit a spravovat záložní zásady a zálohování katalogu.

## <a name="volume-types"></a>Typy hlasitosti

StorSimple svazky může být:

- **Místně připnuté svazky**: Data v těchto svazky zůstávají na místním zařízením StorSimple vždy.
- **Tiered svazky**: Data v těchto svazky můžete přepadového do cloudu.

Archivace hlasitost je typu vrstvené hlasitost. Větší velikost bloku deduplication použitá pro archivaci svazky umožňuje zařízení přenášet větší segmentů dat do cloudu. 

Pokud potřebujete, můžete změnit hlasitost typ z místní vrstveny nebo z vrstveny místních. Další informace přejděte k části [Změna typu svazku](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Místně připnuté svazky

Místně připnuté svazky jsou plně zřizování svazky, které nejsou osy dat do cloudu, zajištění místní záruky pro primární data nezávisle na cloud připojení. Data místně připnuté objemů není deduplicated a komprimovaných. snímky místně připnuté svazky jsou však deduplicated. 

Místně připnuté svazky jsou plně zřízení; Proto se musí mít dostatek místa na vašem zařízení byla vytvořená. Můžete vytvořit místně připnuté svazky až maximální velikosti doručovaných 8 TB na zařízení StorSimple 8100 a 20 TB na 8600 zařízení. StorSimple si vyhrazuje zbývající místní místo na zařízení pro zpracování dat, snímky a metadat. Zvětšit velikost místně připnuté hlasitost do místa na disku maximální, ale nemůžete zmenšení velikosti hlasitost po vytvoření.

Při vytváření místně připnuté hlasitost volné místo pro vytvoření vrstvené svazky menší. Platí také true: Pokud máte existující vrstvené svazky místa na disku pro vytváření místně připnuté svazky bude nižší než maximální limity uvedené výše. Další informace o místní svazky odkazují na [Nejčastější dotazy na místně připnuté svazky](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Vrstvené svazky

Vrstvené svazky jsou tence zřizování svazky, ve kterých často používané data zůstane místní na zařízení a méně častých dat automaticky vrstveny do cloudu. Tenké zřizování je upgradovaný, ve kterém se zobrazí volného překročení fyzické zdroje. Místo předem rezervace dostatečné úložiště StorSimple pomocí tenké zřizování přidělit jenom dostatek místa pro aktuální předpisů. Pružná přírodní cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvětšit nebo zmenšit cloudového úložiště změna požadavkům.

Pokud používáte vrstvené hlasitost pro archivaci dat, zaškrtnutí políčka **použít tento hlasitost méně často používané archivace dat** změní velikosti bloku deduplication hlasitost 512 kB. Pokud tuto možnost nevyberete, použije odpovídající vrstvené hlasitost bloku velikosti 64 kB. Větší velikost bloku deduplication umožňuje zařízení můžete urychlit přenos velké archivace dat do cloudu.

>[AZURE.NOTE] Archivace svazky vytvořené pomocí verze 2 před aktualizace StorSimple bude importovat jako vrstvené se zaškrtnutým políčkem kontrolovat archivaci.

### <a name="provisioned-capacity"></a>Zřizování kapacity

V nápovědě k tabulce maximální zřizování kapacity u jednotlivých typů zařízení a hlasitost. (Vezměte v úvahu, že nejsou k dispozici na virtuální zařízení místně připnuté svazky.)

|             | Maximální vrstvené velikost svazku | Maximální místně připnuté velikost svazku |
|-------------|----------------------------|------------------------------------|
| **Pole fyzicky zařízení** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuální zařízení**  |       |       |
| 8010                | 30 TB | NENÍ K DISPOZICI   |
| 8020               | 64 TB | NENÍ K DISPOZICI   |

## <a name="the-volumes-page"></a>Na stránce svazky

Na stránce **svazky** umožňuje spravovat úložiště svazky, které jsou zřízení na Microsoft Azure StorSimple zařízení pro iniciátory (servery). Zobrazí seznam svazky na zařízení s StorSimple.

 ![Svazky stránky](./media/storsimple-manage-volumes-u2/VolumePage.png)

Objem se skládá z řady atributy:

- **Název svazku** – popisný název, který musí být jedinečný a pomůže identifikovat hlasitost. Tento název se také používá v sledování zpráv při filtrování na konkrétní hlasitost.

- **Stav** – lze do stavu online nebo offline. Pokud svazku je offline, je viditelné pro iniciátory (servery), které jsou povoleny přístup k používání hlasitost.

- **Kapacita** – Určuje celkovou kapacitu data, která mohou být uloženy vyzývající (server). Místně připnuté svazky jsou plně zřízení a jsou umístěny na StorSimple zařízení. Vrstvené svazky jsou tence zřízenou a data deduplicated. S tence zřizování svazky vaše zařízení není předem fyzické kapacitou interně nebo přidělit v cloudu podle kapacity nakonfigurované svazku. Kapacita hlasitost je přidělit a spotřebované množství jako služba.

- **Typ** – označuje, zda je hlasitost **Tiered** (výchozí) nebo **místně připnuté**.

- **Zálohování** – označuje, zda výchozí zásady zálohování neexistuje hlasitost.

- **Access** – určuje iniciátory (servery), které jsou povoleny přístup k tomuto svazku. Iniciátory, kteří nejsou členy ovládací prvek záznamu o přístupu (ACR), který bude přidružený hlasitost nezobrazí hlasitost.

- **Sledování** – Určuje, zda je sledován svazku. Objem bude mít sledování standardně při vytvoření. Sledování bude, ale, být zakázaný klonovat hlasitost. Chcete-li povolit sledování svazku, postupujte podle pokynů [Monitor svazku](#monitor-a-volume). 

Postupujte podle pokynů v tomto kurzu provádět následující úkoly:

- Přidání svazku 
- Úprava svazku 
- Změna typu hlasitosti
- Odstranění svazku 
- Objem jako offline 
- Sledování svazku 

## <a name="add-a-volume"></a>Přidání svazku

[Vytvoření svazku](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) během nasazení řešení StorSimple. Přidání nového svazku je podobným způsobem.

#### <a name="to-add-a-volume"></a>Chcete-li přidat svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. V seznamu vyberte kontejneru hlasitost a poklikáním ji zobrazíte svazky přidružené k kontejneru.

3. Klikněte na **Přidat** v dolní části stránky. Přidat spustí se Průvodce hlasitost.

     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. V dialogu přidat průvodce svazku, klikněte v části **Základní nastavení**postupujte takto:

  1. Zadejte **název** pro hlasitost.
  2. V rozevíracím seznamu vyberte **Typ použití** . Pro úloh, které vyžadují dat je vždy k dispozici místně na zařízení, vyberte **Místně připnuté**. Pro všechny ostatní typy dat vyberte **Tiered**. (**Tiered** je výchozí nastavení).
  3. Pokud jste vybrali v kroku 2 **Tiered** , můžete vybrat zaškrtnutí políčka **použít tento hlasitost méně často používané archivace dat** konfigurace archivace hlasitost.
  4. Zadejte **Zřízení kapacita** hlasitost GB nebo TB. Maximální velikost u jednotlivých typů zařízení a hlasitost naleznete v tématu [Provisioned kapacity](#provisioned-capacity) . Podívejte se na **Dostupné kapacity** lze zjistit, jaká velikost úložiště je skutečně dostupné na vašem zařízení.

5. Klikněte na ikonu se šipkou![Ikonu se šipkou](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Pokud konfigurujete místně připnuté objem, zobrazí se tato zpráva.

    ![Změňte objem zadejte text zprávy.](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Klikněte na ikonu se šipkou ![ikonu se šipkou](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)znovu přejdete na stránku **Další nastavení** .

    ![Přidání dalších nastavení hlasitosti Průvodce](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. V části **Další nastavení**přidáte nový záznam řízení přístupu (ACR):
  
  1. V rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Můžete taky můžete přidat nové ACR. ACRs zjistit, které hosts můžou získat přístup ke svazky odpovídající hostiteli IQN s uvedený v záznamu. Pokud nezadáte ACR, zobrazí se tato zpráva.

        ![Určení ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Doporučujeme, zaškrtněte políčko **Povolit zálohy výchozí pro tento hlasitost** .
  3. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Vytvoření hlasitost se zadaným nastavením.

Hlasitost vašeho nového je nyní připravena k použití.

>[AZURE.NOTE] Když vytvoříte místně připnuté hlasitost a pak vytvořte jiný místně připnuté hlasitost okamžitě navíc, vytváření hlasitost postupně spuštění úlohy. První úlohy vytváření hlasitost musí být dokončen před zahájením následující úlohy vytváření hlasitost.

## <a name="modify-a-volume"></a>Úprava svazku

Upravte svazku, když potřebujete rozbalit nebo změna hosts, které mají přístup hlasitost.

> [AZURE.IMPORTANT] 
>
> - Je-li změnit velikost Hlasitost zařízení velikost svazku potřebujete-li změnit na hostiteli stejně. 
> - Zde popsané kroky straně hostitele platí pro Windows Server 2012 (2012R2). Postupy pro Linux nebo jiné operační systémy hostitele budou lišit. Při úpravách hlasitost na hostiteli jiné operační systém v nápovědě k pokyny operační systém vašeho hostitele. 

#### <a name="to-modify-a-volume"></a>Chcete-li změnit svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. V seznamu vyberte kontejneru hlasitost a poklikáním ji zobrazíte svazky přidružené k kontejner.

3. Vyberte svazku a v dolní části stránky klikněte na **změnit**. Spustí se Průvodce hlasitost změnit.

4. V Průvodci změnit hlasitost klikněte v části **Základní nastavení**můžete udělat následující:

  - Upravte **název**.
  - Převod **Typu použití** z místně připnuté na vrstveny nebo z vrstveny k místně připnuté (Další informace najdete v článku [Změna typu svazku](#change-the-volume-type) ).
  - Zvětšení **zřízení kapacity**. **Zřízení kapacita** můžete jenom zvýší. Po vytvoření otevřela nelze zmenšení svazku.

5. V oblasti **Další nastavení**můžete změnit ACR, za předpokladu, že hlasitost je offline. Pokud se jedná o online, musíte jako offline ho nejdřív. Před úpravami ACR v nápovědě k kroků v tématu [Vezměte objem offline](#take-a-volume-offline) .

    > [AZURE.NOTE] Možnost **Povolit výchozí zálohy** svazku nelze změnit.

6. Uložte provedené změny kliknutím na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Portál Azure klasické zobrazí aktualizace zprávu hlasitost. Zpráva úspěch zobrazí hlasitost po úspěšném aktualizaci.

7. Pokud jsou rozbalování svazku, proveďte následující postup na počítači s Windows Host (hostitel):

   1. Přejděte na **Správa počítače** ->**Správa disků**.
   2. Klikněte pravým tlačítkem myši **Správy disku** a vyberte **Prohledat discích**.
   3. V seznamu disků vyberte svazku, které jste změnili, klikněte pravým tlačítkem myši a potom vyberte **Rozšíření hlasitost**. Spustí se Průvodce rozšíření hlasitost. Klikněte na tlačítko **Další**.
   4. Dokončete Průvodce přijetí výchozí hodnoty. Po dokončení Průvodce hlasitost by měl zobrazit vyšší velikost.

    >[AZURE.NOTE] Pokud rozbalte místně připnuté hlasitost a potom rozbalte položku jiné místně připnuté hlasitost okamžitě navíc, spustit úlohy rozšíření hlasitost postupně. První úlohy rozbalení hlasitost musí být dokončen před zahájením další úlohy rozbalení hlasitost.

![Video k dispozici](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video k dispozici**

Podívejte se na video, který ukazuje, jak rozbalte svazku, klikněte [sem](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Změna typu hlasitosti

Typ lze změnit hlasitost z vrstveny místně připnuté nebo z místně připnuté na vrstvené. Tento převod však nesmí být časté výskyt. Důvody pro převod hlasitost z vrstveny místně připnuté zahrnují:

- Místní záruky které se týkají dostupnost dat a výkonu
- Odstranění čekacích dob cloudu a problémy s připojením cloudu.

Obvykle jde malé existující svazky, které chcete mít často přístup. Místně připnuté hlasitost je plně zřízení při vytvoření. Pokud převádíte vrstvené hlasitost místně připnuté objem, StorSimple ověří, že máte dostatek místa na vašem zařízení před spuštěním převod. Pokud máte dostatek místa, zobrazí se chyba a zruší operaci. 

> [AZURE.NOTE] Než začnete převod z vrstveny místně připnuté, ujistěte se, zvažte místo požadavky vaší úloh. 

Můžete chtít změnit místně připnuté hlasitost vrstvené objem, pokud budete potřebovat další místo zřízení jiné svazky. Při převodu místně připnuté hlasitosti vrstveny dostupné kapacity pro zvýšení zařízení velikostí vydané kapacity. Pokud problémy s připojením kvůli převodu svazku z místní typu vrstvené typ, místní svazku, které se projeví vlastnosti vrstvené objemu až do dokončení převodu. Je to proto může mít možno některá data do cloudu. Tato spilled data zůstanou zaplnit místní místo na zařízení, které nelze uvolnit tak, aby operace restartování a dokončit.

>[AZURE.NOTE] Převedení svazku může nějakou dobu trvat a nelze zrušíte převod po spuštění. Hlasitost zůstane online během převodu, může trvat zálohy, ale nemůžete rozbalte nebo obnovení hlasitost probíhající převod.  

Převod vrstvené místně připnuté hlasitost mohou nepříznivě ovlivnit výkon zařízení. Navíc následující faktory můžou zvětšit dobu potřebnou pro dokončení převodu:

- Existuje nedostatečná šířka pásma.

- Neexistuje žádné aktuální zálohování.

Aby byl minimalizován efekty, které mohou mít následujících skutečností:

- Zkontrolujte omezení zásady šířky pásma a ujistěte se, že je k dispozici vyhrazené šířky pásma 40 MB /.
- Naplánování převod největšího.
- Snímek cloudu trvat, než začnete převod.

Pokud převádíte více svazky (Podpora různých úloh), by měl tak, aby vyšší priorita svazky převedou nejdřív upřednostnit převod hlasitost. Například měli byste převést svazky hostujících virtuálních počítačích (VMs) nebo svazky se úlohami SQL před převedením svazky se úlohami sdílet soubor.

#### <a name="to-change-the-volume-type"></a>Změna typu hlasitosti

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. V seznamu vyberte kontejneru hlasitost a poklikáním ji zobrazíte svazky přidružené k kontejner.

3. Vyberte svazku a v dolní části stránky klikněte na **změnit**. Spustí se Průvodce hlasitost změnit.

4. Na stránce **Základní nastavení** změňte typ použití výběrem nový typ z rozevíracího seznamu **Typ použití** .

    - Chcete-li změnit typ na **místně připnuté**, StorSimple zkontroluje, pokud je dostatečnou kapacitu.
    - Pokud změníte typ na **Tiered** a svazku se použije pro archivaci dat, zaškrtněte políčko **použít tento hlasitost méně často používané archivace dat** .

        ![Zaškrtávací políčko archivu](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Klikněte na ikonu se šipkou ![ikonu se šipkou](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) přejdete na stránku **Další nastavení** . Pokud konfigurujete místně připnuté objem, zobrazí se tato zpráva.

    ![Změňte objem zadejte text zprávy.](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Klikněte na ikonu se šipkou ![ikonu se šipkou](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) pokračujte.

7. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) spustit proces převodu. Portálu Azure se zobrazí zpráva aktualizace hlasitost. Zpráva úspěch zobrazí hlasitost po úspěšném aktualizaci.

## <a name="take-a-volume-offline"></a>Objem jako offline

Budete muset jako offline svazku při plánování ho upravte nebo odstraňte ji. Po offline svazku není k dispozici pro přístup pro čtení i zápis. Je třeba vzít hlasitost offline na hostiteli i na zařízení. 

#### <a name="to-take-a-volume-offline"></a>Aby byl svazku offline

1. Ujistěte se, že dotyčný hlasitost nepoužívá v před přepnutím do offline režimu.

2. Převezmou hlasitost offline hostiteli první. Díky rizik poškození dat na hlasitost. Pro konkrétní kroky postupujte podle pokynů pro váš operační systém Host (hostitel).

3. Pokud po hostiteli offline, převezmou Hlasitost zařízení offline provedením následujících kroků:

  1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** . Na kartě **Hlasitost kontejnery** jsou uvedena v tabulkovém formátu hlasitost kontejnery, které jsou přidružené k ní.
  2. Vyberte kontejneru hlasitost a klikněte na ni zobrazte seznam všech svazky v kontejneru.
  3. Vyberte svazku a klikněte na **převést do offline**.
  4. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Jestli by se měla offline.

    Po offline svazku možnost **Přenést Online** bude k dispozici.

> [AZURE.NOTE] Příkaz **Přepnout do režimu Offline** odešle žádost o zařízení hlasitost jako offline. Pokud hosts stále používáte hlasitost, vede k přerušení připojení, ale hlasitost přepnutím do offline režimu se nezdaří. 

## <a name="delete-a-volume"></a>Odstranění svazku

> [AZURE.IMPORTANT] Objem můžete odstranit jenom v případě, že je offline.

Proveďte následující kroky pro odstranění svazku.

#### <a name="to-delete-a-volume"></a>Chcete-li odstranit svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. Vyberte kontejner hlasitost obsahující hlasitost, který chcete odstranit. Klikněte na kontejner hlasitost pro přístup na stránku **svazky** .

3. Všechny svazky přidružené k tento kontejner se zobrazují ve formátu tabulky. Zkontrolujte stav svazku, který chcete odstranit. Pokud není offline hlasitost, který chcete odstranit, jako offline ho nejdřív postupu v části [Vezměte objem offline](#take-a-volume-offline).

4. Po hlasitost je offline, klikněte na **Odstranit** v dolní části stránky.

5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hlasitost bude odstraněno a objeví na stránce **svazky** aktualizovaný seznam svazky v kontejneru.

    >[AZURE.NOTE]Pokud byste odstranili místně připnuté objem, nemusí být místa pro nové svazky aktualizované okamžitě. Službu StorSimple správce aktualizuje pravidelně místní dostupný prostor. Měli byste že počkejte pár minut, než se pokusíte vytvořit nové hlasitost.<br> Navíc pokud odstraníte místně připnuté hlasitost a potom odstraňte jiné místně připnuté hlasitost ihned poté, odstranění hlasitost postupně spuštění úlohy. První úlohy odstranění hlasitost musí být dokončen před zahájením další úlohy odstranění hlasitost.
 
## <a name="monitor-a-volume"></a>Sledování svazku

Sledování objemu umožňuje shromáždit můžu/O-související statistiky svazku. Sledování aktivované ve výchozím nastavení pro prvních 32 svazky, které jste vytvořili. Sledování další svazky je ve výchozím nastavení vypnutá. Sledování klonovaný svazky je také ve výchozím nastavení vypnutá.

Provedení následujících kroků můžete povolit nebo zakázat sledování svazku.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Zapnutí nebo vypnutí sledování objemu

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. Vyberte kontejneru hlasitost, ve kterém je umístěn hlasitost a klikněte na kontejner hlasitost pro přístup na stránku **svazky** .

3. Všechny svazky přidružené k této container jsou uvedeny v tabulkovém zobrazení. Klikněte na a vyberte hlasitost nebo klonovat hlasitost.

4. V dolní části stránky klikněte na **změnit**.

5. V Průvodci změnit hlasitost klikněte v části **Základní nastavení**vyberte z rozevíracího seznamu **Sledování** **Povolit** nebo **Zakázat** .

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [klonovat StorSimple hlasitost](storsimple-clone-volume.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).

 
