<properties
   pageTitle="Použít na řídicím panelu Správce StorSimple zařízení | Microsoft Azure"
   description="Popisuje řídicí panel StorSimple Správce služeb zařízení a jak se používá k zobrazení metriky úložišť a připojené iniciátory a pořadové číslo a IQN najít."
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
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>Použít na řídicím panelu Správce StorSimple zařízení

## <a name="overview"></a>Základní informace

Řídicí panel StorSimple Správce zařízení vám dává přehled o informace pro konkrétní StorSimple zařízení, na rozdíl od řídicí panel služeb, které vám informace o všech zařízení obsaženy v řešení Microsoft Azure StorSimple.

![Stránky řídicího panelu zařízení](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Řídicí panel obsahuje následující informace:

- **Oblast grafu** – zobrazí se metriky úložišť relevantní v oblasti grafu v horní části řídicího panelu. V tomto grafu můžete zobrazit metriky celkové primární úložiště (množství dat, který napsal hosts do zařízení) a celkové cloudového úložiště využívané zařízení průběhu určitého časového období.

     V tomto kontextu *primární* odkazuje na celkové množství dat, který napsal hostiteli a se dají rozdělit podle typu objem: *primární vrstveného úložiště* obsahuje místně uložené data a data vrstveny do cloudu; *primární místně připnuté úložiště* obsahuje jenom dat uložených místně. *Cloudového úložiště*na druhé straně je měření celkovou kapacitu data uložená v cloudu. Platí to i vrstvené dat a zálohování. Poznámka: data uložená v cloudu je deduplicated komprimovány, že primární označuje velikost úložiště používali data deduplicated a komprimovány. (Můžete porovnat těchto dvou čísel na určitou představu o rychlosti komprese.) Pro oba primární a cloudového úložiště, zobrazené částky budou založeny na sledování počet_plateb nakonfigurujete. Například pokud se rozhodnete frekvenci jeden týden, potom grafu zobrazí data pro každý den předchozího týdne.

     Graf můžete nakonfigurovat následujícím způsobem:

     - Částka cloudového úložiště spotřebované množství časem zobrazíte přepínač **Používá CLOUDOVÉ úložiště** . Celková velikost úložiště vzniku hostitelem zobrazíte vyberte požadované možnosti **Primární VRSTVENY úložiště použita** a **primární MÍSTNĚ PŘIPNUTÉ úložiště** . V příkladu jsou vybrány obě možnosti. proto graf znázorňuje množství úložiště pro cloudu a primární úložiště. Všimněte si, že všechny primární úložiště před instalací aktualizace 2 je znázorněn řádku **Primární VRSTVENY POUŽIJE úložiště** .
     - Pomocí rozevírací nabídky v pravém horním rohu grafu můžete určit 1 týden, 1 měsíc, 3 měsíce nebo roku 1 časové období. Všimněte si, že je aktualizovat jenom jednou za den nejvyšší úrovně graf a proto projeví na předchozí den součty.

     Další informace najdete v tématu [použití službu StorSimple správce ke sledování StorSimple zařízení](storsimple-monitor-device.md).

- **Přehled použití** – v oblasti **Přehled použití** uvidíte množství primární úložiště, množství zřizování úložiště a kapacita maximální velikosti úložiště pro vaše zařízení. Porovnání číselným použití maximálního počtu úložiště, který je k dispozici, zjistíte na první pohled když potřebujete získat další úložiště. Všimněte si, že tento přehled se aktualizuje každých 15 minut a rozdíl v interval aktualizace může zobrazit různý než zobrazené v oblasti grafu výše, které se aktualizuje denně. Další informace najdete v tématu [použití službu StorSimple správce ke sledování StorSimple zařízení](storsimple-monitor-device.md).


- **Upozornění** – oblasti **upozornění** obsahuje přehled upozornění na vašem zařízení. Upozornění jsou seskupená podle závažnosti a počet není uvedený počtu upozornění na každé úrovni závažnosti. Kliknutím na tlačítko upozornění závažnosti otevřete obory zobrazení karty upozornění na zobrazovat jenom upozornění na této úrovně závažnosti zařízení.

- **Úlohy** – oblasti **projekty** se zobrazí výsledek poslední aktivity projektu. To je dosažení systému funguje očekávaným způsobem, nebo ho můžete upozorněním, že budete muset udělat nápravu. Další informace o naposledy dokončené projekty zobrazíte kliknutím na **úlohy úspěšně posledních 24 hodin**.

- Užitečné informace například modelu zařízení pořadové číslo, stav, popis a počet svazky obsahuje oblasti **můžete rychle podívat** na pravé straně řídicího panelu.

Můžete taky nakonfigurovat převzetí a zobrazení připojeného iniciátory na řídicím panelu zařízení.

Běžné úkoly, které lze provádět na této stránce jsou:

- Zobrazení připojeného iniciátory

- Vyhledání pořadové číslo zařízení

- Vyhledání cílové zařízení IQN

## <a name="view-connected-initiators"></a>Zobrazení připojeného iniciátory

Můžete zobrazit kliknutím na **zobrazení připojených iniciátory** odkazu v oblasti **Rychlý přehled** s řídicím panelem zařízení připojených k zařízení iniciátory iSCSI. Tato stránka obsahuje tabulkových seznam iniciátory, které úspěšném připojení do zařízení. Pro každý vyzývající uvidíte:

- ISCSI kvalifikovaný název IQN () z připojeného vyzývající.

- Název ovládacího prvku záznamu přístup (ACR), který zajišťuje toto připojení vyzývající.

- IP adresy připojeného vyzývající.

- Rozhraní sítě, která vyzývající připojen k úložné zařízení. Tyto rozsahu dat 0 až DATA 5.

- Všechny svazky připojeného vyzývající bude moct přistupovat podle aktuální konfigurace ACR.

Pokud najdete v článku neočekávané iniciátory v tomto seznamu nebo se nezobrazuje očekávanými výsledky, zkontrolujte konfiguraci ACR. Než 512 iniciátory můžete připojit k zařízení.

## <a name="find-the-device-serial-number"></a>Vyhledání pořadové číslo zařízení

Pořadové číslo zařízení vám může hodit při konfiguraci Microsoft funkce v/v (MPIO) na zařízení. Takto zobrazíte zařízení pořadové číslo.

#### <a name="to-find-the-device-serial-number"></a>K vyhledání pořadové číslo zařízení

1. Přejděte na **zařízení** > **řídicího panelu**.

2. V pravém podokně řídicího panelu vyhledejte na **první pohled dával** oblast.

3. Přejděte dolů a vyhledejte pořadové číslo.

## <a name="find-the-device-target-iqn"></a>Vyhledání cílové zařízení IQN

Možná budete muset cílové zařízení IQN při konfiguraci ověřovací kód programu ověřování CHAP (Handshake Protocol) na vašem zařízení StorSimple. Takto zobrazíte cílové zařízení IQN.

### <a name="to-find-the-device-target-iqn"></a>K vyhledání cílové zařízení IQN

1. Přejděte na **zařízení** > **řídicího panelu**.

1. V pravém podokně řídicího panelu vyhledejte na **první pohled dával** oblast.

1. Přejděte dolů a vyhledejte cíl IQN.

## <a name="next-steps"></a>Další kroky

- Další informace o [řídicí panel služeb StorSimple správce](storsimple-service-dashboard.md).
- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
