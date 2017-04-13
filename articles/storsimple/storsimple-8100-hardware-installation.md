<properties
   pageTitle="Instalace StorSimple 8100 zařízení | Microsoft Azure"
   description="Popisuje, jak rozbalit regálových připojení a kabel StorSimple 8100 zařízení před nasazení a konfiguraci softwaru."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Rozbalení, regálů připojení a kabel StorSimple 8100 zařízení

## <a name="overview"></a>Základní informace

Vaše Microsoft Azure StorSimple 8100 je jednu přílohu, připojených regálů zařízení. Tento kurz vysvětluje, jak rozbalit, připevnění regálu a hardware kabel StorSimple 8100 zařízení před konfigurace a nasazení StorSimple zařízení.

## <a name="unpack-your-storsimple-8100-device"></a>Rozbalení StorSimple 8100 zařízení

V následujících krocích je jasné, podrobné pokyny k rozbalení StorSimple 8100 úložné zařízení. Toto zařízení se dodává v jeden rámeček.

### <a name="prepare-to-unpack-your-device"></a>Příprava přechodu na Rozbalit zařízení

Než rozbalit zařízení, přečtěte si následující informace.

![Ikona varování](./media/storsimple-safety/IC740879.png)![hodně tloušťky ikonu](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

1. Ujistěte se, že máte k dispozici pro správu tloušťky přílohu, pokud ho jsou ruční zpracování dva lidé. Plně nakonfigurované přílohu můžete porovnali až 32 kg (70 libry.).
1. Umístěte do pole na plochu ploché, úrovně.

Potom proveďte následující postup rozbalit zařízení.

#### <a name="to-unpack-your-device"></a>Rozbalit zařízení

1. Kontrola pole a pěnu balení crushes, kusů, vodu poškozené nebo jiných zřejmé poškození. Pokud pole nebo balení vážně poškozený, nebude možné otevřít pole. Ujistěte se, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) pomůže můžete určit, zda zařízení je dobré funkční.

2. Rozbalení pole. Následující obrázek znázorňuje rozbalené zobrazení StorSimple zařízení.

     ![Rozbalení úložné zařízení](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Rozbalené zobrazení úložné zařízení**

     Popisek | Popis
     ----- | -------------
     1     | Balení pole
     2     | Dolní pěnu
     3     | Zařízení
     4     | Začátek pěnu
     5     | Accessory pole


3. Po rozbalení pole, ujistěte se, že máte:

   - 1 zařízení jednu přílohu
   - 2 kabely power
   - 1 křížový ethernetového kabelu
   - 2 kabely sériové konzoly
   - 1 převaděče sériové USB sériové přístup pomocí protokolu
   - 1 šroubovákem T10 proti poškození
   - 4 QSFP-na-adaptéry SFP + pro použití s 10 rozhraní sítě GbE
   - 1 regálů připojení kit (2 straně profilů s připojením hardwaru)
   - Získání si přečtěte následující dokumentaci Začínáme

    Pokud nedostali jednotlivých položek uvedené, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md).

Dalším krokem je připojení regálů zařízení.

## <a name="rack-mount-your-storsimple-8100-device"></a>Připojení regálů StorSimple 8100 zařízení

Další kroky ve standardní regálů 19 palec s přední a zadní příspěvky nainstalovat StorSimple 8100 úložné zařízení. Zařízení StorSimple 8100 obsahuje jeden primární přílohu.

Instalace se skládá z několik kroků, přičemž každý z nich je popsán v následující postupy.

> [AZURE.IMPORTANT]
StorSimple zařízení musí být regálů připojených správné činnosti.

### <a name="prepare-the-site"></a>Příprava na web

Zařízení musíte mít nainstalovanou na standardní regálů 19 palec obsahující přední a zadní příspěvky. Příprava instalace regálů pomocí následujícího postupu.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Příprava na web pro instalaci regálů

1. Ujistěte se, bezpečně ponechá zařízení na plochou stabilní a úroveň pracovní povrchový (nebo podobné).

2. Ověřte, zda na web, kde chcete nastavit standardní elektrické z nezávisle na zdroj nebo regálů power rozdělení celku (PDU) s UPS (UPS).

3. Zkontrolujte, že tento jeden úsek 2 u je k dispozici v regálů, ve kterém chcete, aby byla připojte zařízení.

![Ikona varování](./media/storsimple-safety/IC740879.png)![hodně tloušťky ikonu](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

Ujistěte se, že máte k dispozici pro správu tloušťky, když se spravuje nastavení zařízení ručně dva lidé. Plně nakonfigurované přílohu můžete porovnali až 32 kg (70 libry.).

### <a name="rack-prerequisites"></a>Požadavky na regálů

Přílohu 8100 je určený pro instalaci ve standardní regálů 19 palec CAB s:

- Minimální hloubka 27.84 palce z regálů příspěvek do příspěvku
- Maximální tloušťky 32 kg pro zařízení
- Maximální zpět tlaku 5 Pascal (0,5 mm vodu sloupce).

### <a name="rack-mounting-rail-kit"></a>Připojování regálů železniční kit

Sadu připojení profilů slouží k použití s CAB 19 palec regálů. Profilů jste otestovali zpracovávání tloušťky maximální přílohu. Tyto profilů také umožní instalace více příloh bez ztráty místo v regálů.


#### <a name="to-install-the-device-on-the-rails"></a>Chcete-li nainstalovat zařízení na profilů

2. Tento krok proveďte jenom v případě, že vnitřní profilů není nainstalovaný na zařízení. Obvykle vnitřní profilů nainstalovaných u výrobce. Pokud nejsou nainstalovány profilů, nainstalujte železniční vlevo a vpravo železniční snímky k snímky stranu přílohu. Jsou připojit pomocí šest metrických šrouby na každé straně. Pomoct s orientací na šířku, železniční snímky označené **LH – přední** a **RH – přední**a je připevněna směrem dozadu přílohu má vřetenovitým ukončit.<br/>

    ![Připojení železniční snímky na přílohu skříň](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Attaching vnitřní železniční snímky k snímky přílohu**

       Popisek | Popis
       ----- | -----------
       1     | M 3 x 4 tlačítko vedoucí šrouby
       2     | Šasi snímků

3. Připojte vnější levé železniční a vnější správné železniční sestavení členům regálů CAB svislé. Hranatých závorkách označené **LH** **RH**a **této straně nahoru** provést vás správný orientaci.

4. Vyhledejte PIN rail přestane reagovat na přední a zadní soupravy rail přestane reagovat. Rozšíření železniční mezi příspěvky regálu a vložit do přední a zadní regálů příspěvek svislé člena holes zadané kódy PIN. Ujistěte se, že sestavení železniční úroveň.

5. Pomocí dvou ujednaných metrických šroubů zabezpečené sestavení rail přestane reagovat regálů svislé členy. Použijte jeden šroub na přední a jednu na zadní.

6. Opakujte tento postup u jiných železniční sestavení.<br/>

     ![Připojování snímků rail přestane reagovat na CAB regálů](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Připojení sestavení vnější rail přestane reagovat regálů**

     Popisek | Popis
     ----- | -----------
     1     | Upínací šroub
     2     | Přední regálů hranatých díry příspěvek šroub
     3     | Levé železniční přední umístění PIN
     4     | Upínací šroub
     5     | Levé železniční zadní umístění PIN


### <a name="mounting-the-device-in-the-rack"></a>Připojení zařízení v regálů

Používání profilů regálů, které jste si právě nainstalovali, proveďte následující kroky připojte zařízení v regálů.

#### <a name="to-mount-the-device"></a>Připojte zařízení

1. S asistent zvedněte přílohu a regálů profilů zarovnáte.

2. Pečlivě vložte zařízení profilů a potom nabízená zařízení úplně do regálů CAB.<br/>

    ![Vložení zařízení v regálů](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Připojení zařízení v regálů**


3. Odebrání levé a pravé přední příruby caps roztažením bezplatné Caps (čepice). Caps příruby jednoduše Přichytit na příruba.

5. Zabezpečené přílohu v regálů instalací jeden ujednaných šroub Phillips vedoucí přes každý příruby doleva a doprava.

4. Nainstalujte caps příruby stisknutím do požadované pozice a přichycení na místě.<br/>

     ![Instalace příruby Caps (čepice)](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instalace příruby Caps (čepice)**

     Popisek | Popis
     ----- | -----------
     1     | Šroub fixační přílohu

Dalším krokem je kabel zařízení pro power, sítě a sériový přístup.

## <a name="cable-your-storsimple-8100-device"></a>Kabelu StorSimple 8100

Následující postupy popisují kabel zařízení StorSimple 8100 power, sítě a sériové připojení.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete kabeláž zařízení, budete potřebovat:

- Úložné zařízení úplně rozbalili a regálů připojit.

- 2 kabely power dodaných se zařízením

- Přístup k 2 jednotky rozdělení Power (doporučeno).

- Síťové kabely

- Pokud sériové kabely

- Sériové převaděč USB s příslušný ovladač na počítači nainstalovaný (v případě potřeby)

- Pokud 4 QSFP-na-adaptéry SFP + pro použití s 10 rozhraní sítě GbE

- [Podporované hardware pro 10 rozhraní sítě GbE na zařízení s StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Kabeláž Power

Zařízení s i nadbytečné Power chlazení moduly (PCMs). Jak PCMs musí být nainstalovaná a připojení ke zdrojům různých power zajistit dostupnost.

Proveďte následující kroky k kabel zařízení power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Síťové kabely

Konfigurace aktivní záložní je vaše zařízení: kdykoli dané jednoho řadiče domény modulu je aktivní a zpracování všech disk a síť při řadiče domény modulu v úsporném režimu. Pokud řadiči selže, úsporném řadiče aktivován okamžitě a pokračuje na disku a síťových operací.

K podpoře tento převzetí nadbytečné řadiče domény, budete muset kabel síti zařízení podle následujících kroků.

#### <a name="to-cable-for-network-connection"></a>Na kabelové připojení k síti

1. Šest síťová rozhraní v každém řadiči má vaše zařízení: čtyři 1 GB/s a dvě 10 GB/s Ethernet portů. Určení různé porty dat na základní zařízení.

    ![Základní 8100 zařízení](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Zpátky ze zařízení zobrazující porty dat**

     Popisek   | Popis
     ------- | -----------
     0,1,4,5 |  1 rozhraní sítě GbE
     2, 3     | 10 rozhraní sítě GbE
     6       | Sériové porty

2. Viz následující obrázek pro síťové kabely. (Konfigurace minimální sítě se zobrazuje v plné čáry: modré. Konfiguraci není potřeba dostupnost a výkon zobrazený podle tečkovanými čarami.)


    ![Kabel 2 u zařízení sítích](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Síťové kabely pro zařízení**


  	|Popisek | Popis |
  	|----- | ----------- |
  	| A    | Místní sítě s přístupem k Internetu |
  	| B    | Řadiče 0 |
  	| C    | PCM 0 |
  	| D    | Řadiči 1 |
  	| PŘIROZENÉHO LOGARITMU E    | PCM 1 |
  	| F, G | Hosts |
  	| 0-5  | Rozhraní sítě |



Když kabeláž zařízení, minimální konfigurace vyžaduje:


- Nejméně dvě síťová rozhraní připojení v každém řadiči s jeden pro přístup ke cloudu a druhý pro iSCSI. DATA, která 0 port automaticky povolit a nakonfigurovat pomocí sériové konzoly zařízení. Kromě DATA 0 jiného datového portu také vyžaduje konfiguraci portálu Azure klasické. V tomto případě připojení dat 0 port primární LAN (síť s přístupem k Internetu). Ostatní data porty můžete být připojeni k segmentu SAN/iSCSI LAN (VLAN) sítě, v závislosti na plánovanou rolí.

- Stejné rozhraní v každém řadiči připojení ke stejné síti k zajištění dostupnosti, pokud dojde k selhání řadiče domény. Například pokud se rozhodnete DATA 0 a 3 dat pro některou řadiče připojení, musíte odpovídající DATA 0 a 3 datového připojení na jiné zařízení.

Mějte na paměti dostupnost a výkon:


- Pokud je to možné, nakonfigurujte v každém řadiči pár prvků rozhraní sítě pro přístup ke cloudu (1 GbE) a další pár iSCSI (10 GbE doporučeno).

- Pokud je to možné, připojte k dva různé přepínače k zajištění dostupnosti před selháním přepnout síťová rozhraní z každého zařízení. Obrázek znázorňuje dva 10 GbE sítě rozhraní DATA 2 a DATA 3 z každého zařízení připojené k dva odlišné přepínače.

Další informace najdete v příručce **Síťová rozhraní** klikněte v části [dostupnost požadavky pro zařízení s StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Pokud používáte SFP + vysílače s 10 rozhraní sítě GbE, použijte dodaná QSFP-SFP + adaptéry. Další informace přejděte na [hardware podporované pro 10 rozhraní sítě GbE na zařízení s StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Sériový port kabeláž

Proveďte následující kroky k kabel sériový port.

#### <a name="to-cable-for-serial-connection"></a>K kabel sériové připojení

1. Má vaše zařízení sériového portu v každé řadiče domény, který je označený ikonou klíče. Podívejte se do obrázku v části [síťové kabely](#network-cabling) vyhledejte sériové porty základní zařízení.

2. Určení aktivní řadiče na základní zařízení. Blikající modré LED znamená, že správce aktivní.

3. Použijte dodaná sériové kabely (v případě potřeby převaděče USB sériové přenosný počítač) a připojení konzoly nebo počítači (s emulace terminálu zařízení) sériový port aktivní řadiče domény.

4. Instalace ovladačů sériové USB (součástí zařízení) na vašem počítači.

5. Nastavení sériové připojení takto: 115 200 přenosová 8 datových bitů, 1 stop-bit, žádná a řízení toku nastavený na žádný.

6. Ověřte, zda je stisknutím klávesy Enter konzole funkční připojení. Nabídka sériové konzoly objevit.

>[AZURE.NOTE] **Správa lights-Out**: nainstalovaném zařízení ve vzdáleném datacentru nebo ve počítače místnosti s omezeným přístupem zkontrolujte, že sériová připojení k oběma řadiče připojeni vždy sériové konzoly přepínač nebo podobná zařízení. Pokud jsou tu uvedené přerušení sítě nebo neočekávané selhání díky mimo pásma vzdálené řízení a operace podpory.

Zařízení je teď zapojené power, přístup k síti a sériové připojení. Dalším krokem je ke konfiguraci software a nasazení zařízení.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [nasazení a konfiguraci místního StorSimple zařízení](storsimple-deployment-walkthrough.md).
