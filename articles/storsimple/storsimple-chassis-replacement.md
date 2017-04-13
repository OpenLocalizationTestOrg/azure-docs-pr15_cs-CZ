<properties 
   pageTitle="Nahrazení skříni na zařízení StorSimple | Microsoft Azure"
   description="Popisuje, jak odebrat a nahradit skříni StorSimple primární přílohu nebo přílohu EBOD."
   services="storsimple"
   documentationCenter=""
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

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Nahrazení skříni na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak odebrat a nahradit šasi v zařízení StorSimple 8000 řady. Model StorSimple 8100 je jednu přílohu zařízení (jeden podvozek), zatímco 8600 duální přílohu zařízení (dva podvozek). U modelu 8600 jsou potenciálně dvou šasi, které může selhat v zařízení: skříni primární přílohu nebo šasi EBOD přílohu.

V obou případech je prázdná skříni náhradní dodané společností Microsoft. Žádné Power a chlazení moduly (PCMs), moduly řadiče plné stavu diskových jednotek (disky SSD) pevných discích (pevných disků) nebo EBOD moduly budou však započítávány.

>[AZURE.IMPORTANT] Před odebráním a nahrazování skříni, projděte si informace zabezpečení v [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Odebrání skříni

Proveďte následující kroky odebrat skříni na zařízení s StorSimple.

#### <a name="to-remove-a-chassis"></a>Chcete-li odebrat podvozku

1. Ujistěte se, že zařízení StorSimple vypnout a od ní odpojilo ze všech zdrojů power.

2. Odebrání všech sítě a kabely přidružení zabezpečení, v případě potřeby.

3. Odebrání regálů jednotky.

4. Odebrání všech jednotky a poznamenejte si sloty, ze kterých se odeberou. Další informace najdete v tématu [Odebrání diskovou jednotku](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Na přílohu EBOD (Pokud je to šasi, které se nepodařilo) odeberte moduly EBOD řadiče domény. Další informace najdete v tématu [Odebrání EBOD zařízení](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Na hlavní přílohu (Pokud je to šasi, které se nepodařilo) odeberte řadiče a poznamenejte si sloty, ze kterých se odeberou. Další informace najdete v tématu [odebrání zařízení](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Instalace skříni

Proveďte následující kroky k instalaci skříni na zařízení s StorSimple.

#### <a name="to-install-a-chassis"></a>Chcete-li nainstalovat podvozku

1. Připojte skříní v regálů. Další informace najdete v tématu [regálů připojení zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) nebo [regálů připojení StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Po skříni je připojen v regálu a nainstalujte moduly řadiče domény na stejném místě, které již byly nainstalovány v.

3. Nainstalujte jednotek ve stejném umístění a sloty, které již byly nainstalovány v.

    >[AZURE.NOTE] Doporučujeme, abyste nejprve nainstalujte disky SSD v paticích a nainstalujte pevných disků.

2. K ní připojit regálu a součástí nainstalovaný připojte zařízení ke zdrojům odpovídající power a zapněte zařízení. Podrobnosti najdete v tématu [kabel zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [kabel StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).

