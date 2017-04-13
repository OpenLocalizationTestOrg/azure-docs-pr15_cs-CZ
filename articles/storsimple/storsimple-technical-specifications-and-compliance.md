<properties 
   pageTitle="Technické specifikace StorSimple | Microsoft Azure"
   description="Popisuje technické údaje a předpisy dodržování předpisů informací o StorSimple hardwaru součástí."
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

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technické údaje a dodržování předpisů pro zařízení StorSimple

## <a name="overview"></a>Základní informace

Hardwarové součásti zařízení Microsoft Azure StorSimple dodržovat technické specifikace a předpisy uvedené v tomto článku. Technické specifikace popisují Power a chlazení moduly (PCMs), diskové jednotky, kapacitu úložiště a přílohy. Informace o dodržování předpisů zahrnuje tyto věci jako mezinárodní organizace pro standardy, zabezpečení a emise a kabeláž.


## <a name="power-and-cooling-module-specifications"></a>Specifikace Power a chlazení modul  

Zařízení StorSimple má dvě, 100 240 v duální ventilace, vyhovujících zápisu SBB moduly Power chlazení (PCMs). Díky tomu nadbytečné power konfigurace. Když PCM nepovede, zůstane zařízení normálně na jiných PCM, dokud se nahradí modulu selhalo.  

Přílohu EBOD používá 580 W PCM a primární přílohu používá 764 W PCM. Následující tabulka uvádí technické specifikace přidružené PCMs.

| Specifikace           | 580 W PCM (EBOD)                                    | 764 W PCM (primární)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximální výstupní výkon    | 580 W                                               | 764                                                |
| Četnosti               | 50/60 Hz                                            | 50/60 Hz                                           |
| Výběr oblasti napětí | Automatické rozsahu: 90 – 264 V AC, 47/63 Hz               | Automatické rozsahu: 90-264 V AC, 47/63 Hz               |
| Maximální průvalu aktuální  | 20 A                                                | 20 A                                               |
| Opravný faktor Power | > nominal 95 % vstupní napětí                          | > nominal 95 % vstupní napětí                         |
| Harmonickými               | Splňuje EN61000-3-2                                   | Splňuje EN61000-3-2                                  |
| Výstup                  | 5 v úsporném režimu napětí @ 2.0 A                          | 5 v úsporném režimu napětí @ 2.7 A                         |
|                         | + 5 @ 42 A                                          | + 5 @ 40 A                                         |
|                         | + 12 v @ 38 A                                         | + 12 v @ 38 A                                        |
| Aktivní připojitelné           |  Ano                                                | Ano                                                |
| Přepínače a LED       | AC samostatnými přepínačem a pole indikátor stavu čtyři LED     | AC samostatnými přepínačem a pole indikátor stavu šest LED     |
| Přílohu chlazení       | Axiální ventilátory s ovládacím prvkem rychlost proměnné ventilace  | Axiální ventilátory s ovládacím prvkem rychlost proměnné ventilace |

 
## <a name="power-consumption-statistics"></a>Statistiky spotřeby energie  

V následující tabulce jsou data spotřebu typické power (hodnot, které se liší od zveřejněná) pro různé modely StorSimple zařízení. 
 
 Podmínky | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Ventilátory jednotky pomalá, nečinná | 1,45 A  |0.31 kW | 1057.76 BTU/hr | 3.19 A | 0.34 kW | 1160.13 BTU/hr 
 Přístup k pomalá, jednotky ventilátory | 1.54 A | 0.33 kW | 1126.01 BTU/hr | 3.27 A | 0.36 kW | 1228.37 BTU/hr 
 Ventilátory rychlé, jednotky nečinnosti, dvě PSUs technologii | 2.14 A | 0.49 kW  | 1671.95 BTU/hr | 4,99 A | 0.54 kW | 1842.56 BTU/hr 
 Ventilátory rychlé, jednotky nečinnosti, jeden PSU technologii jednu nečinnosti | 2.05 A | 0.48 kW | 1637.83 BTU/hr | 4.58 A | 0,50 kW | 1706.07 BTU/hr 
 Rychlé, jednotky ventilátory přístup, dvě PSUs technologii | 2.26 A | 0,51 kW | 1740.19 BTU/hr | 4.95 A | 0.54 kW | 1842.56 BTU/hr 
 Ventilátory rychlé jednotky přístup, jeden PSU technologii jednu nečinnosti | 2.14 A |0.49 kW | 1671.95 BTU/hr | 4.81 A  | 0.53 kW | 1808.44 BTU/hr 

## <a name="disk-drive-specifications"></a>Specifikace diskovou jednotku  

Zařízení s StorSimple podporuje až 12 3,5 formuláře faktor sériové připojené SCSI (přidružení zabezpečení) diskových jednotek. Skutečné jednotek lze kombinaci pevných látek jednotky (disky SSD) nebo pevných discích (pevných disků), v závislosti na konfiguraci produktu. 12 sloty diskovou jednotku najdete v konfiguraci 3 tak, že 4 před přílohu. Přílohu EBOD umožňuje další úložiště pro jiné 12 diskových jednotek. Toto jsou vždy pevných disků.  

## <a name="storage-specifications"></a>Specifikace úložiště
StorSimple zařízení nainstalovaný kombinaci jednotky pevného disku a jednotky pevných látek pro 8100 a 8600. Celková použitelné kapacita 8100 a 8600 jsou zhruba 15 TB a 38 TB. V následující tabulce dokumenty podrobnosti SSD, pevného disku a kapacity cloudu v souvislosti s kapacitou StorSimple řešení.

| Model zařízení / kapacity                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Počet pevných discích (pevných disků)              |   8                                                     |  19                                                     |
| Počet jednotek pevných látek (disky SSD)            |   4                                                     |  5                                                      |
| Jeden pevný disk kapacity                            |   4 TB                                                  |  4 TB                                                   |
| Jeden SSD kapacity                            |   400 GB                                                |  800 GB                                                 |
| Náhradní kapacity                                 |   4 TB                                                  | 4 TB                                                    |
| Použitelné pevný disk kapacity                            | 14 TB                                                   | 36 TB                                                   |
| Použitelné SSD kapacity                            | 800 GB                                                  | 2 TB                                                    |
| Celková použitelné kapacita *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximální řešení kapacita (včetně cloudu)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Celkovou kapacitu použitelné obsahuje kapacitu k dispozici pro data a metadata, buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Přílohu rozměry a specifikace weight (váha)  

Následující tabulka uvádí různé přílohu specifikace pro rozměry a tloušťky.  

### <a name="enclosure-dimensions"></a>Rozměry přílohu
V následující tabulce jsou uvedeny rozměry přílohu v milimetry a palců.

|Přílohu |Milimetry |Palce |
|----------|------------|-------| 
| Výška |87,9 | 3,46 |
| Šířka přes připojení příruby | 483 | 19.02 |
| Šířka přes těla přílohu | 443 | 17.44 |
| Název hloubkové od předního okraj přílohu textu | 577 | 22.72 |
| Název hloubkové operace panelech nejvzdálenější okraj přílohu | 630.5 | 24.82 |
| Název hloubkové z montážních příruby nejvzdálenější okraj přílohu |   603 | 23.74 |

### <a name="enclosure-weight"></a>Přílohu weight (váha)  

V závislosti konfiguraci nenačte primární přílohu můžete porovnali od 21 k 33 kg a vyžaduje dvě osoby zpracovat jeho. 
 
| Přílohu | Weight (váha) |
|-----------|--------| 
| Hmotnost (závisí na konfiguraci) |30 kg – 33 kg |
| Prázdný (instalován žádné jednotky) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specifikace prostředí přílohu  

V této části jsou uvedeny specifikace související s prostředí přílohu. Teplotu, vlhkost, výšku, absorbér, vibrace, orientaci, zabezpečení a elektromagnetické kompatibility (EMC) najdete v této kategorii.  

### <a name="temperature-and-humidity"></a>Teplotní a vlhkostní

| Přílohu        | Teplota oblast  | Okolního relativní vlhkost | Maximální živé žárovky   |
|------------------|----------------------------|---------------------------|--------------------|
| Provozní      | 5° C - 35 OC 41° F - 95° F    | 20 – 80 % bez kondenzačních- | 28 OC (82° F)        |
| Nefunkční  | -40° C - 70 OC (40° F - 158° F) | 5 – 100 % nekondenzující  | 29 OC (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Vzduchu výšku, absorbér, vibrace, orientaci, zabezpečení a EMC
 
| Přílohu          | Provozní specifikace                                                |
|--------------------|---------------------------------------------------------------------------| 
| Vzduchu            | Systém vzduchu je přední zadní. Systém musí spravovaný s instalací Nízkotlaké, zpětných výfukového. Zpět tlaku vytvořil regálů dveře a překážky neměli překročit 5 pascalech (0,5 mm vodu sloupce). |
| Výška, provozní  | metry-30 3045 metrů (metrů-100 na 10 000 stopy) s maximální provozní teploty zrušte hodnocení 5 oC nad 7000 metry. |
| Výška, mimo provozní  | metry-305 až 12 192 metrů (-1,000 metrů na 40 000 stopy) |
| Proudem, provozní  | sinus 10 ms, půlvlnný 5g |
| Proudem, mimo provozní  | sinus 30g 10 ms, půlvlnný |
| Vibrace, provozní  | 0.21g RMS 5 500 Hz náhodné |
| Vibrace, mimo provozní  | 1.04g RMS 2 200 Hz náhodné |
| Vibrace, přemístění  | 3g 2 200 Hz sinus |
| Orientace a připojování  | 19" regálů připojení (2 EIA jednotky) |
| Regálů profilů  | Přizpůsobit hloubka minimální 700 mm (31.50 palce) racků kompatibilní s IEC 297 |
| Zabezpečení a schválení  |   CE a UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Mezinárodní organizace pro standardy dodržování předpisů
Microsoft Azure StorSimple zařízení splňuje následující mezinárodní standardy:  

- CE - CS 60950-1  
- Hlášení IEC 60950-1  
- UL a cUL UL 60950-1  

## <a name="safety-compliance"></a>Zabezpečení dodržování předpisů  

Microsoft Azure StorSimple zařízení splňuje následující hodnocení zabezpečení:  

- Systém produktu typ schválení: UL, cUL CE  
- Dodržování předpisů zabezpečení: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC dodržování předpisů 

Microsoft Azure StorSimple zařízení splňuje následující EMC hodnocení.  

### <a name="emissions"></a>Emise

Zařízení kompatibilní se standardem EMC emise provedeny a vyzářený úrovní.  

- Provedeny emise omezení úrovní: 47 část CFR 15B předmětu A EN55022 třídy A CISPR předmětu A  
- Vyzářený emise omezení úrovní: 47 část CFR 15B předmětu A EN55022 třídy A CISPR předmětu A   

### <a name="harmonics-and-flicker"></a>Harmonickými a flickru  

Zařízení splňuje EN61000-3-2 a 3.  

### <a name="immunity-limit-levels"></a>Omezení úrovní odolnost  
Zařízení splňuje EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power kabel dodržování předpisů
  
Plug-in a sestavení kabel dokončení power musí splňovat standardy vhodné pro země, ve kterém se používá zařízení a musí mít bezpečnostní schválení, které jsou přijatelné v dané zemi. Následující tabulka uvádí standardy pro USA a Evropy.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Kabely power AC - USA (musí být NRTL uvedené)

| Součásti       | Specifikace                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Typ kabel       | Pole SV nebo SVT 18 AWG minimum, 3 vodičem 2.0 metry maximální délka položek |
| Moduly            | Moduly zemnicím typu Příloha 5-15P NEMA hodnocení 120 V, 10 A; nebo IEC 320 C14 250 V, 10 A |
| Socket          | IEC 320 C-13 250 V, 10 A                                         |

### <a name="ac-power-cords---europe"></a>Kabely power AC - Europe

| Součásti       | Specifikace                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Typ kabel       | Harmonizovány H05 VVF 3G1.0                                         |
| Socket          | IEC 320 C-13 250 V, 10 A                                         |

## <a name="supported-network-cables"></a>Podporované síťové kabely  

10 rozhraní sítě GbE DATA 2 a 3 dat naleznete [seznam podporovaných síťové kabely a moduly](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Další kroky

Teď jste připraveni nasazení StorSimple zařízení ve vaší datacentra. Další informace najdete v tématu [zařízení v místním nasazení](storsimple-deployment-walkthrough-u2.md).  
