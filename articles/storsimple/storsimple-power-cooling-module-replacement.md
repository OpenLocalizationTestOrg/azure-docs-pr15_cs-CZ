<properties 
   pageTitle="Nahrazení PCM na zařízení s StorSimple | Microsoft Azure"
   description="Vysvětluje, jak odebrat a nahradit Power a chlazení modul PCM () na vašem zařízení StorSimple"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Nahrazení Power a chlazení modul na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Výkon a chlazení modul PCM () v zařízení Microsoft Azure StorSimple se skládá z napájecí zdroj a ventilátory, které lze ovládat pomocí primární a EBOD přílohy. Existuje pouze jeden model PCM s certifikací pro každou přílohu. Primární přílohu certifikované pro 764 W PCM a přílohu EBOD certifikované pro 580 W PCM. Přestože PCMs primární přílohu a přílohu EBOD se liší, postup nahrazení je stejný.

Tento kurz vysvětluje postup:

- Odebrání PCM
- Instalace náhradní PCM

>[AZURE.IMPORTANT] Před odebráním a nahrazování PCM, projděte si informace zabezpečení v [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).

## <a name="before-you-replace-a-pcm"></a>Než změníte PCM

Mějte na paměti následující důležité problémů před nahradíte váš PCM:

- Pokud napájení PCM nepovede, nechte instalace modulu vadná, ale odebrat kabel. Ventilace zůstanou dostanete power od přílohu a nadále poskytovat správné chlazení. Když ventilace nepovede, musí PCM nahrazuje okamžitě.

- Před odebráním PCM, odpojte power PCM vypnutím přepínače hlavní (pokud existuje) nebo fyzicky odebráním kabel. Zajistíte upozornění v systému, že je power bezprostředně.

- Zkontrolujte PCM funkční pro operaci pokračování systém před nahrazením vadná PCM. Vadná PCM musí být nahrazeny plně funkční PCM co nejdříve.

- Nahrazení modul PCM trvá jenom pár minut, ale je potřeba dokončit do 10 minut po odebrání selhalo PCM nechcete, aby přehřátí.

- Všimněte si, že náhradní 764 W PCM moduly odeslání z továrny neobsahují modulu záložní battery. Budou muset battery odebrání vadná PCM a vložit ho do modulu náhradní před provedením nahradit. Další informace najdete v tématu Jak [Odebrat a vložit modulu záložní battery](storsimple-battery-replacement.md).


## <a name="remove-a-pcm"></a>Odebrání PCM

Když budete chtít ze svého zařízení Microsoft Azure StorSimple odeberete Power a chlazení modul PCM (), postupujte podle těchto pokynů.

>[AZURE.NOTE] Než odeberete vaší PCM, zkontrolujte, jestli máte správná náhradní (764 W pro primární přílohu) nebo 580 W EBOD přílohu.

#### <a name="to-remove-a-pcm"></a>Chcete-li odebrat PCM

1. Na portálu Azure klasické, klikněte na **zařízení** > **údržbu** > **Stav hardwaru**. Zkontrolujte stav součásti PCM v seznamu **Sdílené součásti** chcete zjistit, které se nezdařila PCM:

     - Pokud napájení PCM 0 selže, budou červené stav **Napájení v PCM 0** .

     - Pokud napájení PCM 1 selže, budou červené stav **Napájení v PCM 1** .

     - Pokud ventilace v PCM 1 selže, bude stav **chlazení 0 PCM 0** a **1 chlazení pro PCM 0** červený.

2. Vyhledejte selhalo PCM na zadní straně primární přílohu. Pokud používáte modelu 8600, označte primární přílohu pohledem systému jednotku identifikační číslo, na panelu přední LED displeji. Výchozí jednotky ID zobrazené na primární přílohu je **00**, zatímco ID jednotky zobrazeny na přílohu EBOD výchozí hodnota je **01**. Následující diagram a tabulka vysvětluje přední panel LED displeji.

    ![ID systému v přední OPS panel](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Obrázek 1** Přední panel zařízení  

  	|Popisek|Popis|
  	|:---|:-----------|
  	|1|Tlačítko Ztlumit|
  	|2|Systém power|
  	|3|Modul poruch|
  	|4|Logické poruch|
  	|5|Zobrazení ID jednotky|

3. Indikátor sledování LED pozadí primární přílohu lze také určit vadná PCM. Podívejte se na následující diagram a tabulky pochopit, jak používat LED vyhledejte vadná PCM. Například pokud osvětlen LED odpovídající **Ventilace nezdaří** , ventilace selhala. Podobně platí Pokud rozsvíceno LED odpovídající **AC selhání** napájení selhala. 

    ![Základní indikátoru zařízení PCM sledování LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Obrázek 2** Zadní strana PCM s indikátorem LED

  	|Popisek|Popis|
  	|:---|:-----------|
  	|1|AC dodávky elektrického proudu|
  	|2|Chyba při ventilace|
  	|3|Battery poruch|
  	|4|PCM OK|
  	|5|Dodávky elektrického proudu Datacentrum|
  	|6|Battery správný|

4. Podívejte se do na následujícím obrázku na pozadí zařízení StorSimple vyhledejte nezdařeném uložení modulu PCM. PCM 0 je na levé straně a PCM 1 je na pravé straně. Následující tabulka popisuje moduly.

     ![Základní modulů primární přílohu zařízení](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Obrázek 3** Zadní strana zařízení s moduly plug-in 

  	|Popisek|Popis|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Řadiče 0|
  	|4|Řadiči 1|

5. Vypnutí vadná PCM a odpojit kabel dodávky. Teď můžete odebrat PCM.

6. Pochopit zámek a na straně úchyt PCM mezi miniatury a ukazováčkem a vměstnat můžete otevřít na úchyt.

    ![Otevření PCM úchyt](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Obrázek 4** Otevření PCM úchyt

7. Uchopitelný na úchyt a odeberte PCM.

    ![Odebrání zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Obrázek 5** Odebrání PCM

## <a name="install-a-replacement-pcm"></a>Instalace náhradní PCM

Postupujte podle těchto pokynů k instalaci PCM v zařízení s StorSimple. Ujistěte se, že jste vložili modulu záložní battery před instalací náhradní PCM (platí pro 764 W PCMs pouze). Další informace najdete v tématu Jak [Odebrat a vložit modulu záložní battery](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Chcete-li nainstalovat PCM

1. Zkontrolujte, jestli máte správná náhradní PCM pro tento přílohu. Primární přílohu potřebuje 764 W PCM a přílohu EBOD musí 580 W PCM. Neměli byste použít 580 PCM W v primární přílohu nebo 764 PCM W v EBOD přílohu. Následující obrázek znázorňuje místo pro identifikaci tyto informace na popisek, který je opatřit PCM.

    ![Popisek PCM zařízení](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Obrázek 6** Popisek PCM

2. Vyhledat poškození přílohu, přičemž zvláštní pozornost ke spojnicím. 
                                        
    >[AZURE.NOTE] **Pokud jsou ohnuty libovolnou spojnici PIN není instalace modulu.**

3. Pomocí úchytu PCM v otevřené poloze přesuňte do okna přílohu modulu.

    ![Instalace zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Obrázek 7** Instalace PCM

4. Zavřete ručně PCM úchyt. Klepnutím jste měli poslechněte si, jak věnuje zámek úchyt. 
                                        
    >[AZURE.NOTE] Abyste měli jistotu, že máte provozují spojnice spojky, které můžete jemně tug na úchyt stisknutým zámek. Pokud PCM snímky se předpokládá, že zámek zavřel před provozují spojnice.

5. Připojte kabely power ke zdroji power a PCM.

6. Zabezpečení namáhání osvobození balíky. 

7. Zapněte PCM.

8. Ověřte, že byl úspěšný nahradit: v Azure klasické portálu StorSimple Správce služby, přejděte do **zařízení** > **údržbu** > **Stav hardwaru**. V seznamu **Sdílené součásti**stav PCM by měl být zelená. 
                                        
    >[AZURE.NOTE] Může trvat několik minut náhradní PCM úplně inicializace.

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).
