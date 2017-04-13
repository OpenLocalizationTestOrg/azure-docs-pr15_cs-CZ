<properties
   pageTitle="Instalace StorSimple 8600 zařízení | Microsoft Azure"
   description="Popisuje, jak rozbalit regálových připojení a kabel StorSimple 8600 zařízení před nasazení a konfiguraci softwaru."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Rozbalení, regálů připojení a kabel StorSimple 8600 zařízení

## <a name="overview"></a>Základní informace
Vaše Microsoft Azure StorSimple 8600 je zařízení duální přílohu a se skládá z primárního a přílohu EBOD. Tento kurz vysvětluje, jak rozbalit a připevnění regálu a kabel StorSimple 8600 hardware zařízení před konfigurovat StorSimple software.

## <a name="unpack-your-storsimple-8600-device"></a>Rozbalení StorSimple 8600 zařízení

Podle těchto kroků poskytnout vymazat, podrobné pokyny rozbalit StorSimple 8600 úložné zařízení. Toto zařízení se dodává v dvě pole, jedno pro primární přílohu a jiné pro EBOD přílohu. Tyto dvě pole se pak umístí do jeden rámeček.

### <a name="prepare-to-unpack-your-device"></a>Příprava přechodu na Rozbalit zařízení

Než rozbalit zařízení, přečtěte si následující informace.


![Ikona varování](./media/storsimple-safety/IC740879.png)![hodně tloušťky ikonu](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

1. Ujistěte se, že máte k dispozici pro správu tloušťky zařízení, pokud ho jsou ruční zpracování dva lidé. Plně nakonfigurované přílohu můžete porovnali až 32 kg (70 libry.).
1. Umístěte do pole na plochu ploché, úrovně.

Potom proveďte následující postup rozbalit zařízení.

#### <a name="to-unpack-your-device"></a>Rozbalit zařízení

1. Kontrola pole a pěnu balení crushes, kusů, vodu poškozené nebo jiných zřejmé poškození. Pokud pole nebo balení vážně poškozený, nebude možné otevřít pole. Ujistěte se, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) pomůže můžete určit, zda zařízení je dobré funkční.

2. Otevření okna pro vnější a potom uzavře dvě pole odpovídá primární a EBOD přílohy. Teď můžete rozbalit primární a EBOD přílohy. Následující obrázek znázorňuje rozbalené zobrazení jednoho z přílohy.

    ![Rozbalení úložné zařízení](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Rozbalené zobrazení úložné zařízení**

     Popisek | Popis
     ----- | -------------
     1     | Balení pole
     2     | Přidružení zabezpečení kabely (na položku Příslušenství a kabely panelu)
     3     | Dolní pěnu
     4     | Zařízení
     5     | Začátek pěnu
     6     | Accessory pole

3. Po rozbalení Ujistěte se, že máte dvě pole:

  - 1 primární přílohu (primární přílohu a EBOD přílohu jsou ve dvou samostatných polích)
  - 1 EBOD přílohu
  - 4 power kabely 2 v rozevíracích seznamech
  - 2 přidružení zabezpečení kabely (primární přílohu připojení k EBOD přílohu)
  - 1 křížový ethernetového kabelu
  - 2 kabely sériové konzoly
  - 1 převaděče sériové USB sériové přístup pomocí protokolu
  - 4 QSFP-na-adaptéry SFP + pro použití s 10 rozhraní sítě GbE
  - 2 regálových připojení Kit (4 straně profilů s připojením hardwaru, 2 pro primární přílohu a EBOD přílohu), 1 v rozevíracích seznamech
  - Získání si přečtěte následující dokumentaci Začínáme

    Pokud nedostali jednotlivých položek uvedené, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md).  

Dalším krokem je připojení regálů zařízení.

## <a name="rack-mount-your-storsimple-8600-device"></a>Připojení regálů StorSimple 8600 zařízení

Další kroky ve standardní regálů 19 palec s přední a zadní příspěvky nainstalovat StorSimple 8600 úložné zařízení. Toto zařízení může mít dvě přílohy: primární přílohu a přílohu EBOD. Obě tyto muset regálů připojit.

Instalace se skládá z několik kroků, přičemž každý z nich je popsán v následující postupy.

> [AZURE.IMPORTANT]
StorSimple zařízení musí být regálů připojených správné činnosti.



### <a name="site-preparation"></a>Příprava webu

Ve standardním regálů 19 palec obsahující přední a zadní příspěvky musí být nainstalovaný přílohy. Příprava instalace regálů pomocí následujícího postupu.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Příprava na web pro instalaci regálů

1. Přesvědčte se, že primární a EBOD přílohy bezpečné na ploché stabilní a vyrovnat pracovní plochu řady (nebo podobné).

2. Ověřte, zda na web, kde chcete nastavit standardní elektrické z nezávisle na zdroj nebo regálů Power rozdělení jednotky (PDU) s UPS (UPS).

3. Zkontrolujte, že úsek (2 u 2 X), které jeden 4U je k dispozici v regálů, ve kterém chcete připojit přílohy.

![Ikona varování](./media/storsimple-safety/IC740879.png)![hodně tloušťky ikonu](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

 Ujistěte se, že máte k dispozici pro správu tloušťky, když se spravuje nastavení zařízení ručně dva lidé. Plně nakonfigurované přílohu můžete porovnali až 32 kg (70 libry.).

### <a name="rack-prerequisites"></a>Požadavky na regálů

Příloh jsou sice pro instalaci ve standardní regálů 19 palec CAB s:

- Minimální hloubka 27.84 palce z regálů příspěvek do příspěvku
- Maximální tloušťky 32 kg pro zařízení
- Maximální zpět tlaku 5 Pascal (0,5 mm vodu sloupce)

### <a name="rack-mounting-rail-kit"></a>Připojování regálů železniční kit

Sadu připojení profilů bude určen k použití u CAB regálů 19 palců. Profilů jste otestovali zpracovávání tloušťky maximální přílohu. Tyto profilů také umožní instalace více příloh bez ztráty místo v regálů. Nejprve nainstalujte EBOD přílohu.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>Pokud chcete nainstalovat přílohu EBOD profilů

2. Tento krok proveďte jenom v případě, že vnitřní profilů není nainstalovaný na zařízení. Obvykle vnitřní profilů nainstalovaných u výrobce. Pokud nejsou nainstalovány profilů, nainstalujte železniční vlevo a vpravo železniční snímky k snímky stranu přílohu. Jsou připojit pomocí šest metrických šrouby na každé straně. Pomoct s orientací na šířku, železniční snímky označené **LH – přední** a **RH – přední**a je připevněna směrem dozadu přílohu má vřetenovitým ukončit.

    ![Připojování železniční snímky, kam chcete přílohu šasi](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Připojení k snímky přílohu železniční snímků**

    Popisek | Popis
    ----- | -----------
    1     | M 3 x 4 tlačítko vedoucí šrouby
    2     | Šasi snímků

3. Připojte levé železniční a pravý železniční sestavení členům regálů CAB svislé. Hranatých závorkách označené **LH** **RH**a **této straně si** vás provede správnou orientací.

4. Vyhledejte PIN rail přestane reagovat na přední a zadní soupravy rail přestane reagovat. Rozšíření železniční mezi příspěvky regálu a vložit do přední a zadní regálů příspěvek svislé člena holes zadané kódy PIN. Ujistěte se, že sestavení železniční úroveň.

5. Zabezpečené sestavení rail přestane reagovat regálů svislé členy pomocí dvou metrických šroubů k dispozici. Použijte jeden šroub na přední a jednu na zadní.

6. Opakujte tento postup u jiných železniční sestavení.

     ![Připojování snímků rail přestane reagovat na CAB regálů](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Připojení sestavení rail přestane reagovat regálů**

     Popisek | Popis
     ----- | -----------
     1     | Upínací šroub
     2     | Přední regálů hranatých díry příspěvek šroub
     3     | Vlevo přední železniční umístění PIN
     4     | Upínací šroub
     5     | Levý zadní železniční umístění PIN

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Připojení EBOD přílohu v regálů

Používání profilů regálů, které jste si právě nainstalovali, proveďte následující kroky připojení EBOD přílohu v regálů.

#### <a name="to-mount-the-ebod-enclosure"></a>Chcete-li připojit EBOD přílohu

1. S asistent zvedněte přílohu a regálů profilů zarovnáte.

2. Pečlivě profilů vložte přílohu a potom nabízená ho úplně do regálů CAB.

    ![Vložení zařízení v regálů](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Připojení přílohu v regálů**

3. Odebrání levé a pravé přední příruby caps roztažením bezplatné Caps (čepice). Caps příruby jednoduše Přichytit na příruba.

4. Zabezpečené přílohu do regálů instalací jeden ujednaných šroub Phillips vedoucí přes každý příruby doleva a doprava.

4. Nainstalujte caps příruby stisknutím do požadované pozice a přichycení do místa.

     ![Instalace příruby Caps (čepice)](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instalace příruby Caps (čepice)**

     Popisek | Popis
     ----- | -----------
     1     | Šroub fixační přílohu


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Připojení primární přílohu v regálů

Po připojení přílohu EBOD bude muset připojit primární přílohu stejným postupem.

> [AZURE.NOTE]
>
> - Je možné mít několik prázdných bloků paměti v regálů mezi primární přílohu a EBOD přílohu.
> - Připojení k přílohu EBOD primární přílohu pomocí kabelu přidružení zabezpečení ujednaných 2m.
> - Existuje bez omezení relativní umístění hlavní jednotku EBOD jednotku. Proto primární přílohu mohou být umístěny v horním úsek a pod EBOD přílohu, nebo naopak.

Dalším krokem je kabel zařízení pro power, sítě a sériový přístup.

## <a name="cable-your-storsimple-8600-device"></a>Kabelu StorSimple 8600

Následující postupy popisují kabel zařízení StorSimple 8600 power, sítě a sériové připojení.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete kabel zařízení, budete potřebovat:

- Primární přílohu a přílohu EBOD úplně rozbalili
- 4 kabely power (2 každý pro hlavní stránku předlohy a přílohu EBOD), které jste dostali se zařízením
- 2 přidružení zabezpečení kabely dodaného s zařízení připojení přílohu EBOD k primární přílohu
- Přístup k 2 Power rozdělení jednotky (jednotek PDU) (doporučeno)
- Síťové kabely
- Pokud sériové kabely
- Převaděč sériové USB s příslušný ovladač na počítači nainstalovaný (v případě potřeby)
- Pokud 4 QSFP-na-adaptéry SFP + pro použití s 10 rozhraní sítě GbE
- [Podporované hardware pro 10 rozhraní sítě GbE na zařízení s StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>Přidružení zabezpečení a kabeláž Power

Primární přílohu a přílohu EBOD má vaše zařízení. Při této akci musí jednotky, které mají být společně kabelové připojení sériové připojené SCSI (přidružení zabezpečení) a power.

Při nastavování toto zařízení poprvé, proveďte kroky pro přidružení zabezpečení kabeláž nejdřív a pak postupujte podle pokynů pro power kabeláž.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Síťové kabely

Je vaše zařízení v úsporném režimu aktivní konfiguraci: kdykoli dané jednoho řadiče domény modulu je aktivní a zpracování všech disk a síť při řadiče domény modulu v úsporném režimu. Pokud dojde k selhání řadiče domény, úsporném řadiče okamžitě aktivuje a pokračuje všechny operace disku a sítě.

K podpoře tento převzetí nadbytečné řadiče domény, budete muset kabel síti zařízení uvedeno v následujících kroků.

#### <a name="to-cable-for-network-connection"></a>Na kabelové připojení k síti

1. Šest síťová rozhraní v každém řadiči má vaše zařízení: čtyři 1 GB/s a dvě 10 GB/s Ethernet porty. Podívejte se na následujícím obrázku k identifikaci dat porty základní zařízení.

     ![Základní 8600 zařízení](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Zpět z vašeho zařízení zobrazující porty dat**

     Popisek   | Popis
     ------- | -----------
     0,1,4,5 |  1 rozhraní sítě GbE
     2, 3     | 10 rozhraní sítě GbE
     6       | Sériové porty



1. Viz následující obrázek pro síťové kabely. (Konfigurace minimální sítě se zobrazuje v plné čáry: modré. Dostupnost a výkonu konfiguraci není potřeba zobrazený podle tečkovanými čarami.)

![Kabel zařízení 4U sítě](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Síťové kabely pro zařízení**

Popisek | Popis
----- | -----------
A    | Místní sítě s přístupem k Internetu
B    | Řadiče 0
C    | PCM 0
D    | Řadiči 1
PŘIROZENÉHO LOGARITMU E    | PCM 1
F    | EBOD řadiče 0
G    | EBOD řadiči 1
H, MŮŽU  | Hosts (například servery souboru)
0-5  | Rozhraní sítě
6    | Primární přílohu
7    | EBOD přílohu

Když kabeláž zařízení, minimální konfigurace vyžaduje:


- Nejméně dvě síťová rozhraní připojení v každém řadiči s jeden pro přístup ke cloudu a druhý pro iSCSI. DATA, která 0 port automaticky povolit a nakonfigurovat pomocí sériové konzoly zařízení. Kromě DATA 0 jiného datového portu také vyžaduje konfiguraci portálu Azure klasické. V tomto případě připojení dat 0 port primární LAN (síť s přístupem k Internetu). Ostatní data porty můžete být připojeni k segmentu SAN/iSCSI LAN (VLAN) sítě, v závislosti na plánovanou rolí.

- Stejné rozhraní v každém řadiči připojení ke stejné síti k zajištění dostupnosti, pokud dojde k selhání řadiče domény. Například pokud se rozhodnete DATA 0 a 3 dat pro některou řadiče připojení, musíte odpovídající DATA 0 a 3 datového připojení na jiné zařízení.

Mějte na paměti dostupnost a výkon:


- Pokud je to možné, nakonfigurujte v každém řadiči pár prvků rozhraní sítě pro přístup ke cloudu (1 GbE) a další pár iSCSI (10 GbE doporučeno).

- Pokud je to možné, připojte k dva různé přepínače k zajištění dostupnosti před selháním přepnout síťová rozhraní z každého zařízení. Obrázek znázorňuje dva 10 GbE sítě rozhraní DATA 2 a DATA 3 z každého zařízení připojené k dva odlišné přepínače. Další informace najdete v příručce **Síťová rozhraní** klikněte v části [dostupnost požadavky pro zařízení s StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] SFP + vysílače s 10 rozhraní sítě GbE použít ujednaných QSFP-SFP + adaptéry. Další informace přejděte na [hardware podporované pro 10 rozhraní sítě GbE na zařízení s StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Sériový port kabeláž

Proveďte následující kroky k kabel sériový port.

#### <a name="to-cable-for-serial-connection"></a>K kabel sériové připojení

1. Má vaše zařízení sériového portu v každé řadiče domény, který je označený ikonou klíče. Sériové porty naleznete v nápovědě k obrázku, která data se zobrazí porty na zadní straně zařízení.

2. Určení aktivní řadiče na základní zařízení. Blikající modré LED znamená, že správce aktivní.

3. Použijte dodaná sériové (v případě potřeby převaděče USB sériové přenosný počítač) a připojení konzoly nebo počítači (s emulace terminálu zařízení) sériový port aktivní řadiče domény.

4. Instalace ovladačů sériové USB (součástí zařízení) na vašem počítači.

5. Sériové připojení nastavení následujícím způsobem:
   - Přenosová 115 200
   - 8 datových bitů
   - 1 stop-bit
   - Žádná
   - Řízení toku nastavený na **žádný**

6. Ověřte, zda je stisknutím klávesy Enter konzole funkční připojení. Nabídka sériové konzoly objevit.

> [AZURE.NOTE] **Lights-Out správy:** Nainstalovaném zařízení ve vzdáleném datacentru nebo ve počítače místnosti s omezeným přístupem zkontrolujte, že sériová připojení k oběma řadiče připojeni vždy sériové konzoly přepínač nebo podobná zařízení. Díky mimo pásma vzdálené řízení a operace podpory pro nečekané přerušení sítě nebo neočekávané k chybám.

Jste dokončili kabeláž zařízení pro power, přístup k síti a sériové připojení. Dalším krokem je software nakonfigurovat tak na vašem zařízení.

## <a name="next-steps"></a>Další kroky

Teď jste připraveni na [nasazení a konfiguraci místního StorSimple zařízení](storsimple-deployment-walkthrough-u2.md).
