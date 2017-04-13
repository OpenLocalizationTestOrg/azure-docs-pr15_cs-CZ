<properties
   pageTitle="Spravovat svazky StorSimple | Microsoft Azure"
   description="Vysvětluje, jak přidat, upravit, sledovat a odstraňovat StorSimple svazky a jak jako offline je v případě potřeby."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Umožňuje spravovat svazky službu StorSimple správce

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak používat službu StorSimple správce můžete vytvořit a spravovat svazky na StorSimple zařízení a StorSimple virtuální zařízení.

Služba StorSimple správce je rozšíření Azure klasické portál, který umožňuje spravovat řešení StorSimple z jedné webové rozhraní. Kromě Správa svazky, můžete službu StorSimple správce a vytvořit a spravovat StorSimple služby a zařízení můžete spravovat, zobrazit upozornění a zobrazit a spravovat záložní zásady a zálohování katalogu.

> [AZURE.NOTE] Azure StorSimple můžete vytvořit tence zřizování svazky. Nemůžete vytvořit zřizování plně nebo částečně zřizování svazky systému Azure StorSimple.
>
> Tenké zřizování je upgradovaný, ve kterém se zobrazí volného překročení fyzické zdroje. Místo předem rezervace dostatečné úložiště Azure StorSimple pomocí tenké zřizování přidělit jenom dostatek místa pro aktuální předpisů. Pružná přírodní cloudového úložiště usnadňuje tento přístup, protože Azure StorSimple můžete zvětšit nebo zmenšit cloudového úložiště změna požadavkům.

## <a name="the-volumes-page"></a>Na stránce svazky

Na stránce **svazky** umožňuje spravovat úložiště svazky, které jsou zřízení na Microsoft Azure StorSimple zařízení pro iniciátory (servery). Zobrazí seznam svazky na zařízení s StorSimple.

 ![Svazky stránky](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Objem se skládá z řady atributy:

- **Název** – popisný název, který musí být jedinečný a pomůže identifikovat hlasitost. Tento název se také používá v sledování zpráv při filtrování na konkrétní hlasitost.

- **Stav** – lze do stavu online nebo offline. Pokud objem, pokud offline, je viditelné pro iniciátory (servery), které jsou povoleny přístup k používání hlasitost.

- **Kapacita** – Určuje, jak velké hlasitost, jak vnímat vyzývající (server). Kapacita Určuje celkovou kapacitu data, která mohou být uloženy vyzývající (server). Svazky jsou tence zřízenou a data deduplicated. To znamená, že vaše zařízení není předem fyzické kapacitou interně nebo přidělit v cloudu podle nakonfigurované hlasitost kapacity. Kapacita hlasitost je přidělit a spotřebované množství jako služba.

- **Typ** – typ hlasitost může být vrstvené nebo archivace (podtyp vrstveny)

- **Access** – určuje iniciátory (servery), které jsou povoleny přístup k tomuto svazku. Iniciátory, kteří nejsou členy ovládací prvek záznamu aplikace access (ACR) souvisí s hlasitost nezobrazí hlasitost.

- **Sledování** – Určuje, zda je sledován svazku. Objem bude mít sledování standardně při vytvoření. Sledování bude, ale, být zakázaný klonovat hlasitost. Chcete-li povolit sledování svazku, postupujte podle pokynů Monitor svazku.

Jsou nejčastější úkoly spojené s objemem:

- Přidání svazku
- Úprava svazku
- Odstranění svazku
- Objem jako offline
- Sledování svazku

## <a name="add-a-volume"></a>Přidání svazku

[Vytvoření svazku](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) během nasazení řešení StorSimple. Přidání nového svazku je podobným způsobem.

### <a name="to-add-a-volume"></a>Chcete-li přidat svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. Vyberte kontejneru hlasitost a klikněte na šipku v řádku odpovídajícím pro přístup k svazky přidružené k kontejner.

3. Klikněte na **Přidat** v dolní části stránky. Přidat spustí se Průvodce hlasitost.

     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes/AddVolume1.png)

4. V dialogu přidat průvodce svazku, klikněte v části **Základní nastavení**postupujte takto:

  1. Zadejte **název** pro hlasitost.
  2. **Zřízení kapacita** hlasitost vyplnit GB nebo TB. Kapacita musí obsahovat minimálně 1 GB až 64 TB pro fyzické zařízení. Maximální kapacitu, kterou můžete zřízení svazku na zařízení virtuální StorSimple je 30 TB.
  3. Vyberte **Typ použití** hlasitost. Pokud používáte vrstvené hlasitost pro archivaci dat, zaškrtnutí políčka **použít tento hlasitost méně často používané archivace dat** změní velikosti bloku deduplication hlasitost 512 kB. Pokud tuto možnost nevyberete, použije odpovídající vrstvené hlasitost bloku velikosti 64 kB. Větší velikost bloku deduplication umožňuje zařízení můžete urychlit přenos velké archivace dat do cloudu. (Vrstvené svazky byly dříve označovaného jako primární svazky.)
  5. Klikněte na ikonu se šipkou ![ikonu se šipkou](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)přejdete na stránku **Další nastavení** .

        ![Přidání dalších nastavení hlasitosti Průvodce](./media/storsimple-manage-volumes/AddVolume2.png)

5. V části **Další nastavení**přidáte nový záznam řízení přístupu (ACR):

  1. V rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Můžete taky můžete přidat nové ACR. ACRs zjistit, které hosts můžou získat přístup ke svazky odpovídající hostiteli IQN s uvedený v záznamu.
  2. Doporučujeme povolit zálohy výchozí tak, že zaškrtnete políčko **Povolit zálohy výchozí pro tento hlasitost** .
   3. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Vytvoření hlasitost se zadaným nastavením.

Hlasitost vašeho nového je nyní připravena k použití.

## <a name="modify-a-volume"></a>Úprava svazku

Upravte svazku, když potřebujete rozbalit nebo změna hosts, které mají přístup hlasitost.

> [AZURE.IMPORTANT]
>
> - Je-li změnit velikost Hlasitost zařízení velikost svazku potřebujete-li změnit na hostiteli stejně.
> - Zde popsané kroky straně hostitele platí pro Windows Server 2012 (2012R2). Postupy pro Linux nebo jiné operační systémy hostitele budou lišit. Při úpravách hlasitost na hostiteli jiné operační systém v nápovědě k pokyny operační systém vašeho hostitele.

### <a name="to-modify-a-volume"></a>Chcete-li změnit svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Hlasitost kontejner** . Tato stránka obsahuje v tabulkovém formátu hlasitost kontejnery, které jsou přidružené k ní.

2. Vyberte kontejneru hlasitost a klikněte na ni zobrazte seznam všech svazky v kontejneru.

3. Na stránce **svazky** vyberte svazku a klikněte na tlačítko **změnit**.

4. V Průvodci změnit hlasitost klikněte v části **Základní nastavení**můžete udělat následující:

  - Pokud chcete upravit vrstvené hlasitost archivace objem tak, že zaškrtnete políčko **použít tento hlasitost méně často používané archivace dat** můžete změnit velikost bloku deduplication pro hlasitost 512 kB upravte **název** a **Typ** .
  - Zvětšení **zřízení kapacity**. **Zřízení kapacita** můžete jenom zvýší. Po vytvoření otevřela nelze zmenšení svazku.

    > [AZURE.NOTE] Po přiřazení svazku není možné změnit kontejneru hlasitost.

5. V části **Další nastavení**můžete udělat toto:

  - Úprava ACRs, za předpokladu, že hlasitost je offline. Pokud se jedná o online, musíte jako offline ho nejdřív. Před úpravami ACR v nápovědě k kroků v tématu [Vezměte objem offline](#take-a-volume-offline) .
  - Úprava seznamu ACRs po hlasitost je offline.

    > [AZURE.NOTE] Možnost **Povolit zálohy výchozí pro tento hlasitost** svazku nelze změnit.

6. Uložte provedené změny kliknutím na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Portál Azure klasické zobrazí aktualizace zprávu hlasitost. Zpráva úspěch zobrazí hlasitost po úspěšném aktualizaci.

7. Pokud jsou rozbalování svazku, proveďte následující postup na počítači s Windows Host (hostitel):

   1. Přejděte na **Správa počítače** ->**Správa disků**.
   2. Klikněte pravým tlačítkem myši **Správy disku** a vyberte **Prohledat discích**.
   3. V seznamu disků vyberte svazku, které jste změnili, klikněte pravým tlačítkem myši a potom vyberte **Rozšíření hlasitost**. Spustí se Průvodce rozšíření hlasitost. Klikněte na tlačítko **Další**.
   4. Dokončete Průvodce přijetí výchozí hodnoty. Po dokončení Průvodce hlasitost by měl zobrazit vyšší velikost.

![Video k dispozici](./media/storsimple-manage-volumes/Video_icon.png) **Video k dispozici**

Podívejte se na video, který ukazuje, jak rozbalte svazku, klikněte [sem](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Objem jako offline

Budete muset jako offline svazku při plánování ho upravte nebo odstraňte ji. Po offline svazku není k dispozici pro přístup pro čtení i zápis. Je třeba vzít hlasitost offline na hostiteli i na zařízení. Proveďte následující kroky k svazku jako offline.

### <a name="to-take-a-volume-offline"></a>Aby byl svazku offline

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

### <a name="to-delete-a-volume"></a>Chcete-li odstranit svazku

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. Vyberte kontejner hlasitost obsahující hlasitost, který chcete odstranit. Klikněte na kontejner hlasitost pro přístup na stránku **svazky** .

3. Všechny svazky přidružené k tento kontejner se zobrazují ve formátu tabulky. Zkontrolujte stav svazku, který chcete odstranit. Pokud není offline hlasitost, který chcete odstranit, jako offline ho nejdřív postupu v části [Vezměte objem offline](#take-a-volume-offline).

4. Po hlasitost je offline, klikněte na **Odstranit** v dolní části stránky.

5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hlasitost bude odstraněno a objeví na stránce **svazky** aktualizovaný seznam svazky v kontejneru.

## <a name="monitor-a-volume"></a>Sledování svazku

Sledování objemu umožňuje shromáždit můžu/O-související statistiky svazku. Sledování aktivované ve výchozím nastavení pro prvních 32 svazky, které jste vytvořili. Sledování další svazky je ve výchozím nastavení vypnutá. Sledování klonovaný svazky je také ve výchozím nastavení vypnutá.

Provedení následujících kroků můžete povolit nebo zakázat sledování svazku.

### <a name="to-enable-or-disable-volume-monitoring"></a>Zapnutí nebo vypnutí sledování objemu

1. Na stránce **zařízení** vyberte zařízení, poklikejte na něj a potom klikněte na kartu **Kontejnery hlasitost** .

2. Vyberte kontejneru hlasitost, ve kterém je umístěn hlasitost a klikněte na kontejner hlasitost pro přístup na stránku **svazky** .

3. Všechny svazky přidružené k tento kontejner jsou uvedeny v tabulkovém zobrazení. Klikněte na a vyberte hlasitost nebo klonovat hlasitost.

4. V dolní části stránky klikněte na **změnit**.

5. V Průvodci změnit hlasitost klikněte v části **Základní nastavení**vyberte z rozevíracího seznamu **Sledování** **Povolit** nebo **Zakázat** .

    ![Upravit hlasitost základní nastavení](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [klonovat StorSimple hlasitost](storsimple-clone-volume.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
