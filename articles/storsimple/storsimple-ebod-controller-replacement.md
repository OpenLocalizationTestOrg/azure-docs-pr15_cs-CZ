<properties 
   pageTitle="Nahrazení řadiči StorSimple EBOD | Microsoft Azure"
   description="Vysvětluje, jak odebrat a nahradit jednu nebo obě řadiče EBOD na StorSimple 8600 zařízení."
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Nahrazení EBOD řadiče na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak chcete nahradit vadná modul řadiče EBOD na Microsoft Azure StorSimple zařízení. Pokud chcete nahradit modul EBOD řadiče domény, potřebujete:

- Odebrání vadná EBOD zařízení
- Instalace nové řadiče EBOD

Než začnete, zvažte následující informace:

- Prázdné moduly EBOD nutné ho do všechny nepoužívané sloty vložit. Příloha nebude ochlaďte správně Pokud pozici ještě zbývá otevřít.

- Správce EBOD je za provozu a lze odebrat nebo nahradit. Selhalo modulu neodebírat, dokud nedosáhnete náhradní. Po zahájení procesu náhradní dokončit je 10 minut.

>[AZURE.IMPORTANT] Než se pokusíte odstranit nebo nahradit libovolnou součást StorSimple, ujistěte se, kontrolujte [konvence ikonu zabezpečení](storsimple-safety.md#safety-icon-conventions) a další [bezpečnostní](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Odebrání EBOD zařízení

Před nahrazení selhalo EBOD řadiče domény modulu StorSimple zařízení, zkontrolujte, že ostatní EBOD řadiče domény modulu je aktivní a pracovního. Následující postup a tabulky je vysvětleno, jak odebrat modulu EBOD řadiče domény.

#### <a name="to-remove-an-ebod-module"></a>Chcete-li odebrat modul EBOD

1. Otevřete portál Azure klasické.

2. Přejděte na **zařízení** > **údržbu** > **Stav hardwaru**a ověřte, že stav LED modulu řadiče domény active EBOD je zelená a je červený Indikátor modulu řadiče selhalo EBOD.

3. Najděte modul selhalo řadiče EBOD zadní straně zařízení.

4. Odebrání kabely, která se připojují modul řadiče EBOD řadiči před provedením modulu EBOD mimo systému.

5. Poznamenejte si přesné port přidružení zabezpečení modulu EBOD řadiče domény, který byl připojený k řadiči. Budete požádáni o obnovení systému tuto konfiguraci po nahradit modulu EBOD. 

    >[AZURE.NOTE] Obvykle to bude A portu, který je označený jako **Host ve** v následujícím diagramu.

    ![Základní EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Obrázek 1** Zadní strana EBOD modul

  	|Popisek|Popis|
  	|:----|:----------|
  	|1|Poruchy LED|
  	|2|Power LED|
  	|3|Přidružení zabezpečení spojnic|
  	|4|Přidružení zabezpečení LED|
  	|5|Sériové porty factory pouze pro použití|
  	|6|Portu (hostitel v)|
  	|7|Port B (hostitel se)|
  	|8|C port (pouze Factory použijte)|

## <a name="install-a-new-ebod-controller"></a>Instalace nové řadiče EBOD

Následující postup a tabulky je vysvětleno, jak nainstalovat modul řadiče EBOD v zařízení s StorSimple.

#### <a name="to-install-an-ebod-controller"></a>Instalace řadiče EBOD

1. Zkontrolujte zařízení EBOD pro poškození, zejména konektor rozhraní. Pokud jsou všechny PIN ohnuty si odinstalovat nového řadiče EBOD.

2. S zámky v otevřené poloze přesuňte modulu do přílohu, dokud zámky zapojit.

    ![Instalace EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Obrázek 2**  Instalace modulu EBOD řadiče domény

3. Zavřete zámek. Klepnutím jste měli poslechněte si, jak věnuje zámek.

    ![Uvolnění EBOD zámek](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Obrázek 3**  Zavření zámek modul EBOD

4. Opětovné připojení kabely. Použití přesné konfigurace, který obsahoval před nahradit. Podívejte se na následující diagram a podrobnosti o tom, jak připojit kabely tabulce.

    ![Kabel zařízení 4U power](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Obrázek 4**. Opětovné připojení kabely

  	|Popisek|Popis|
  	|:----|:----------|
  	|1|Primární přílohu|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Řadiče 0|
  	|5|Řadiči 1|
  	|6|EBOD řadiče 0|
  	|7|EBOD řadiči 1|
  	|8|EBOD přílohu|
  	|9|Jednotky Power rozdělení.|

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).
