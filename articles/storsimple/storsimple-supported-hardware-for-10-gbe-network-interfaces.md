<properties 
   pageTitle="Rozhraní hardware pro StorSimple 10 GbE | Microsoft Azure"
   description="Popisuje připojitelné vysílačů (SFP) podporované malé formuláře faktory, kabely a přepínače 10 rozhraní sítě GbE na zařízení s StorSimple."
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

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Podporované hardware pro 10 rozhraní sítě GbE na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Tento článek obsahuje informace o doplňující hardwaru, se kterými spolupracuje Microsoft Azure StorSimple zařízení.

## <a name="list-of-devices-tested-by-microsoft"></a>Seznam zkoušený společností Microsoft

Microsoft má otestovat následující malé uspořádání připojitelné vysílačů (SFP), kabely a přepínače zajistit, aby fungovaly optimálně s zařízení. (V následujících tabulkách se aktualizují při zkoušce nového hardwaru.)

### <a name="sfp-transceivers"></a>SFP + vysílače

|Zkontrolujte|Model|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kabely

|S. Ne. |Zkontrolujte|Model|
|---|---|---|
| 1.|Cisco|SFP H10GB CU1M|
| 2.|Cisco|SFP H10GB CU2M|
| 3.|Cisco|SFP H10GB CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Přepínače

|S. Ne.|Zkontrolujte|Model|
|---|---|---|
| 1. |Cisco|N3K C3172PQ 10GE|
| 2. |Cisco|N3K C3048 ZM F|
| 3. |Cisco|N5K C5596UP IM|

## <a name="list-of-devices-tested-in-the-field"></a>Seznam zařízení testovat v poli

Toto téma obsahuje seznam zařízení, které byly úspěšně nasazeny v poli StorSimple zákazníci. Tyto netestovali společností Microsoft, ale je pravděpodobné, že pro práci s StorSimple zařízení.
 
| Parametr                         | Hodnota                                    |
|-----------------------------------|------------------------------------------|
| Přepnutí nastavení                     | Juniper                                  |
| Přepnout modelu                    | ex4550 32F                               |
| Verze operačního systému přepínač | JunOS 12.3R9.4                           |
| Zásuvné modelu                     | Porty integrovaný PIC (0)                    |
| Přepnutí vysílač                  | Juniper                                  |
| Vysílač modelu               | Čísla dílu 740 021308 <br></br> Čísla dílu 740 030658                   |
| Verze firmwaru vysílač    | Revize 01 verze 0,0 (uvedeny)            |
| Kabel modelu                     | Oboustranný tisk můstků LC/LC 50/125µ, OM3, LSZH |
| StorSimple modelu                | 8600                                     |
| Verzi softwaru StorSimple     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Seznam zkoušený poskytovatelem OEM (Mellanox)  

Mellanox má otestovat následující malé uspořádání připojitelné vysílačů (SFP), kabely a přepínače zajistit, aby fungovaly optimálně s Mellanox síťová rozhraní například 10 rozhraní sítě GbE na zařízení s StorSimple.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabely a moduly nepodporuje Mellanox 

V následující tabulce jsou uvedeny kabely a moduly nepodporuje Mellanox. Tyto netestovali společností Microsoft, ale je pravděpodobné, že pro práci s StorSimple zařízení.

| S. Ne. | Rychlost | Model                 | Popis                                            | Zkontrolujte                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | Trpný měděné kabel SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | Trpný měděné kabel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | Trpný měděné kabel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | Trpný měděné kabel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kabel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kabel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kabel                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + SFP + 1m přímo připojit měděné kabel             | HP                    |
| 9.     | 10 GbE| 455883 B21 HP BLc     | 10Gb SR SFP + určovat, jestli se                                       | HP                    |
| 10.    | 10 GbE| 455886 B21 HP BLc     | 10Gb LR SFP + určovat, jestli se                                       | HP                    |
| 11.    |10 GbE | 487649 B21 HP BLc     | SFP + 0,5 m 10GbE měděné kabel                           | HP                    |
| 12.    |10 GbE | 487652 B21 HP BLc     | 1m 10GbE SFP + měděné kabel                             | HP                    |
| 13.    |10 GbE | 487655 B21 HP BLc     | 3m 10GbE SFP + měděné kabel                             | HP                    |
| 14.    |10 GbE | 487658 B21 HP BLc     | SFP + 7m 10GbE měděné kabel                             | HP                    |
| 15.    |10 GbE | 537963 B21 HP BLc     | SFP + 5m 10GbE měděné kabel                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m řadě C pasivní měděné SFP + kabel                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m řadě C pasivní měděné SFP + kabel                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1m B řady aktivní měděné SFP + kabel                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B řady aktivní měděné SFP + kabel                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR vysílač                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR vysílač                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kabel                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kabel                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0.65 m DAC kabel                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1.2 m DAC kabel                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m DÁD kabel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A QSA Mellanox | QSFP SFP + adaptér                                   | Technologie Mellanox |
| 28.    | 10 GbE| MC2309124 006 Mt      | Trpný měděné kabel 1 x SFP+ k QSFP 10 Gb/s 24awg 7 m   | Technologie Mellanox |
| 29.    | 10 GbE| MC2309124 007 Mt      | Trpný měděné kabel 1 x SFP+ k QSFP 10 Gb/s 24awg 7 m   | Technologie Mellanox |
| 30.    | 10 GbE| MC2309130 003 Mt      | Trpný měděné kabel 1 x SFP+ k QSFP 10 Gb/s 30awg 3 m   | Technologie Mellanox |
| 31.    | 10 GbE| MC2309130 00A Mt      | Trpný měděné kabel 1 x SFP+ k QSFP 10 Gb/s 30awg 0,5 m | Technologie Mellanox |
| 32.    | 10 GbE| MC3309124 005 Mt      | Trpný měděné kabel 1 24awg 10 Gb/s x SFP+ 5 m           | Technologie Mellanox |
| 33.    | 10 GbE| MC3309124 007 Mt      | Trpný měděné kabel 1 24awg 10 Gb/s x SFP+ 7 m           | Technologie Mellanox |
| 34.    | 10 GbE| MC3309130 003 Mt      | Trpný měděné kabel 1 30awg 10 Gb/s x SFP+ 3 m           | Technologie Mellanox |
| 35.    | 10 GbE| MC3309130 00A Mt      | Trpný měděné kabel 1 30awg 10 Gb/s x SFP+ 0,5 m         | Technologie Mellanox |

### <a name="switches-supported-by-mellanox"></a>Přepínače nepodporuje Mellanox 

V následující tabulce jsou uvedeny přepínače nepodporuje Mellanox. Tyto netestovali společností Microsoft, ale je pravděpodobné, že pro práci s StorSimple zařízení.

| S. Ne. | Rychlost | Model | Popis                                                         | Zkontrolujte              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733 B21  | HP ProCurve 6120XG 10GbE Ethernet zásuvné přepínač                      | HP    |
| 2.     | 10GbE | 538113 B21  | HP 10GbE předávací modul (druh)                                  | HP    |
| 3.     | 10GbE | EN4093      | Modul 10gigabitový Scalable přepnout struktury EN4093 IBM PureFlex systému | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE přepnout zásuvné                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 × 1GbE přepnout zásuvné                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | HP 1GbE přepnout modul – GbE2c vrstvy 2/3 Ethernet zásuvné přepnout       | HP    |
| 7.     | 1GbE  | 6120G       | XG ProCurve 6120G HP 1GbE přepnout zásuvné                              | HP    |

## <a name="next-steps"></a>Další kroky

[Další informace o součástech StorSimple hardware a stav](storsimple-monitor-hardware-status.md).
