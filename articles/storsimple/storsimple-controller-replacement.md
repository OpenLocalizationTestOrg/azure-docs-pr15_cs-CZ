<properties 
   pageTitle="Nahrazení řadiči StorSimple zařízení | Microsoft Azure"
   description="Vysvětluje, jak odebrat a nahradit jednu nebo obě moduly řadiče domény na zařízení s StorSimple."
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Nahrazení modulu řadiče domény na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak odebrat a nahradit jednu nebo obě moduly řadiče StorSimple zařízení. Popisuje také logiku scénářích nahrazení jednoho a Duální řadiče.

>[AZURE.NOTE] Před provedením náhradní řadiče domény, doporučujeme vždy aktualizaci firmwaru řadiče domény na nejnovější verzi.
>
>Chcete-li zabránit poškození StorSimple zařízení, vysunout správce, dokud led je zobrazena jako jednu z následujících akcí:
>
>- Všechny indikátory jsou vypnuto.
>- Indikátor 3 ![zelené zaškrtnutí ikonu](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), a ![červený křížek ikonu](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) jsou blýskavé, a **na**LED 0 a LED 7.

Následující tabulka zobrazuje scénáře náhradní podporované řadiče domény.

|Případu|Nahrazení scénářů|Příslušných postupech|
|:---|:-------------------|:-------------------|
|1|Jeden řadiče je ve stavu selhání je další řadiče správný a aktivní.|[Nahrazení jednoho řadiče](#replace-a-single-controller), který popisuje [Logika za nahrazení jednoho řadiče domény](#single-controller-replacement-logic), jakož i [náhradní příčky](#single-controller-replacement-steps).|
|2|Obě řadiče selhalo a vyžadovat náhradní. Skříni, disků and.disk přílohu jsou funkční.|[Náhradní dvěma řadiči](#replace-both-controllers), který popisuje [Logika za náhradní dvěma řadiči](#dual-controller-replacement-logic), jakož i [náhradní příčky](#dual-controller-replacement-steps). |
|3|Jsou vyměnit řadiče z stejné zařízení nebo z různých zařízení. Šasi, disků a disku přílohy jsou funkční.|Zobrazí se zpráva upozorňující neshodu úsek.|
|4|Chybí jeden řadiče domény a další řadiče se nezdaří.|[Náhradní dvěma řadiči](#replace-both-controllers), který popisuje [Logika za náhradní dvěma řadiči](#dual-controller-replacement-logic), jakož i [náhradní příčky](#dual-controller-replacement-steps).|
|5|Jeden nebo oba řadiče se nezdařil. Pomocí sériové konzoly nebo prostředí Windows PowerShell Vzdálená nebudete mít přístup zařízení.|[Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) postup nahrazení ruční řadiče domény.|
|6|Řadiče mají různé vytvořit verzi, která mohou být způsobeno:<ul><li>Řadiče máte jinou verzi.</li><li>Řadiče mít k dispozici různé firmware verzi.</li></ul>|Pokud jsou různé verze software řadiče domény, nahrazení logických zjistí, že a aktualizuje verzi softwaru řadiči náhradní.<br><br>Pokud jsou různé verze firmwaru řadiče domény a je starší verze firmwaru **není** automaticky ji rozšířit, se zobrazí výstražná zpráva Azure klasické portálu. Má vyhledat aktualizace a nainstalujte aktualizace firmware.</br></br>Pokud jsou různé verze firmwaru řadiče domény a je automaticky ji rozšířit starší verze firmwaru, nahrazení logických řadiče zjistí to a po spuštění Správce firmware se automaticky aktualizují.|

Potřebujete odebrat modulu řadiče domény, pokud se nezdařila. Jeden nebo oba modulů řadiče domény může selhat, což může vést v jedné řadiče náhradní nebo nahrazení dvěma řadiči. Nahrazení postupy a logika za je najdete v těchto článcích:

- [Nahrazení jednoho řadiče](#replace-a-single-controller)
- [Nahrazení obou řadiče](#replace-both-controllers)
- [Odebrání zařízení](#remove-a-controller)
- [Vložení řadiči](#insert-a-controller)
- [Určení aktivní řadiče na vašem zařízení](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Před odebráním a nahrazování řadiči, projděte si informace zabezpečení v [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).

## <a name="replace-a-single-controller"></a>Nahrazení jednoho řadiče

Když jednu ze dvou řadiče na Microsoft Azure StorSimple zařízení selže, nepracuje nebo chybí, budete muset nahrazení jednoho řadiče. 

### <a name="single-controller-replacement-logic"></a>Nahrazení jednoho řadiče použití logických operátorů

V jedné řadiče náhradní odeberete nejdřív selhání řadiče. (Zbývající správce v zařízení je aktivní řadiče.) Pokud vložíte řadiče náhradní provést následující akce:

1. Náhradní správce spustí hned komunikaci s StorSimple zařízení.

2. Snímek virtuální pevný disk (virtuální pevný disk) aktivní řadiče zkopírována řadiči náhradní.

3. Snímek se mění tak, aby při náhradní správce spustí z této virtuálního pevného disku, bude rozpoznán jako úsporném řadiče.

4. Po dokončení úprav náhradní správce spustí jako úsporném správce.

5. Když jsou spuštěné obou řadiče, clusteru je online.

### <a name="single-controller-replacement-steps"></a>Postup nahrazení jednoho řadiče domény

Pokud jeden z řadiče v Microsoft Azure StorSimple zařízení selže, proveďte následující kroky. (Jiné zařízení musí být aktivní a spuštěnou. Případně obě řadiče nepovede špatnou, přejděte k [dvěma řadiči náhradní krokům](#dual-controller-replacement-steps).)

>[AZURE.NOTE] 30 – 45 minut u řadiče a restartujte úplně přizpůsobuje postup nahrazení jednoho řadiče domény může trvat. Celkový čas pro celý postup, včetně připojení kabely, je přibližně 2 hodin.

#### <a name="to-remove-a-single-failed-controller-module"></a>Chcete-li odebrat modul jednoho řadiče selhalo

1. Na portálu Azure klasické přejděte na službu StorSimple správce, klikněte na kartu **zařízení** a potom klikněte na název zařízení, které chcete sledovat.

2. Přejděte na **Údržba > Stav hardwaru**. Stav řadiče 0 a 1 řadiče by měl být červené, což znamená, se nepovede.

    >[AZURE.NOTE] Selhání řadiče v jedné řadiče náhradní je vždy řadiči úsporném režimu.

3. Najděte modul řadiče selhalo pomocí obrázek 1 a v následující tabulce.  

    ![Základní modulů primární přílohu zařízení](./media/storsimple-controller-replacement/IC740994.png)

    **Obrázek 1** Zadní strana StorSimple zařízení

  	|Popisek|Popis|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Řadiče 0|
  	|4|Řadiči 1|

4. U řadiče selhalo odebráním připojeného síťové kabely porty data. Pokud používáte 8600 model, taky odeberte kabely přidružení zabezpečení, která se připojují správce řadiči EBOD.

5. Postupujte podle kroků v tématu [odebrání zařízení](#remove-a-controller) odebrat selhání řadiče. 

6. Ve stejném úsek, ze kterého byla odebrána řadiče selhalo nainstalujte náhradní factory. To spustí logiky nahrazení jednoho řadiče domény. Další informace najdete v tématu [logiky nahrazení jednoho řadiče domény](#single-controller-replacement-logic).

7. Když logickou nahrazení jednoho řadiče přejde na pozadí, připojte kabely. Dávejte připojení všechny kabely stejným způsobem, že byli připojení před nahradit.

8. Po restartování správce kontrola **stavu řadiče** a **clusteru stav** na portálu Azure klasické ověření, že zařízení je správný stav a v úsporném režimu.

>[AZURE.NOTE] Pokud sledujete zařízení pomocí sériové konzoly, můžete vidět víc restartování při obnovení správce z postup nahrazení. Když se nabídka sériové konzola, pak víte dokončení nahradit. V nabídce není zobrazena do dvou hodin obrazovky při zahajování náhradní řadiče domény, kontaktujte [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Nahrazení obou řadiče

Při obou řadiče na Microsoft Azure StorSimple zařízení se nezdařil, jsou správně nebo chybí, budete muset nahradit obou řadiče. 

### <a name="dual-controller-replacement-logic"></a>Nahrazení logických dvěma řadiči

V dvěma řadiči nahrazení odeberte obou řadiče selhalo a vložte nahrazení. Po vložení dvou náhradní řadiče dojít následujících akcí:

1. Náhradní správce v pozici 0 zkontroluje takto:
 
   1. Používá se aktuálními verzemi software a firmware?

   2. To je součástí clusteru?

   3. Je spuštěný řadiče mezi dvěma účastníky a se skupinový?
                            
    Pravdivosti žádný z těchto podmínek, správce vyhledá nejnovější denní zálohování (umístěné v **nonDOMstorage** na disku S). Správce slouží ke kopírování nejnovější snímek virtuálního pevného disku ze zálohy.

2. Správce v pozici 0 pomocí snímku vlastní obrázek.

3. Správce v úsek 1 mezitím čeká řadiče 0 ke dokončit zobrazení a spuštění.

4. Když začnou řadiče 0, rozpozná řadiči 1 clusteru vytvořil řadiče 0, který spustí logiky nahrazení jednoho řadiče. Další informace najdete v tématu [logiky nahrazení jednoho řadiče domény](#single-controller-replacement-logic).

5. Navíc bude běžet obou řadiče a clusteru bude přechodu do online.

>[AZURE.IMPORTANT] Po dvěma řadiči nahrazení po konfiguraci zařízení StorSimple je důležité, že provedete ruční zálohování zařízení. Denní zálohy konfigurace zařízení nejsou spouštěný až po uplynutí 24 hodin. Práce s [Podporou společnosti Microsoft](storsimple-contact-microsoft-support.md) , aby ručním zálohovací zařízení.

### <a name="dual-controller-replacement-steps"></a>Postup nahrazení dvěma řadiči

Pokud obě řadiče v zařízení Microsoft Azure StorSimple nezdaří požaduje tento pracovní postup. Tato situace může nastat ve datacentru, ve kterém systém chlazení přestane fungovat a v důsledku toho obou řadiče nepovede v rámci krátký časový úsek. V závislosti na tom, jestli je zařízení StorSimple zapnuli nebo vypnuli a jestli používáte 8600 nebo datového modelu pro 8100 požaduje jinou sadu jednoduchých kroků.

>[AZURE.IMPORTANT] 45 minut může trvat na 1 hodinu u řadiče restartujte úplně před a obnovení postup nahrazení dvěma řadiči. Celkový čas pro celý postup, včetně připojení kabely, je přibližně 2,5 hodin.

#### <a name="to-replace-both-controller-modules"></a>Pokud chcete nahradit modulů řadiče domény

1. Pokud je zařízení vypnou a zůstanou vypnuté, tento krok přeskočit a přejít k dalšímu kroku. Pokud je zapnutá zařízení, vypněte zařízení.
                                        
    1. Pokud používáte modelu 8600, vypněte primární přílohu první a potom vypněte EBOD přílohu.

    2. Počkejte, dokud je úplně vypnout zařízení. Všechny LED pozadí zařízení budou vypnout.

2. Odeberte všechny síťové kabely připojené k porty data. Pokud používáte 8600 model, taky odeberte kabely přidružení zabezpečení, která se připojují primární přílohu EBOD přílohu.

3. Odebrání obou řadiče StorSimple zařízení. Další informace najdete v tématu [odebrání zařízení](#remove-a-controller).

4. Nejdřív vložte náhradní factory řadiče 0 a potom vložte řadiče 1. Další informace najdete v tématu [Vložení řadiči](#insert-a-controller). To spustí nahrazení logických dvěma řadiči. Další informace najdete v tématu [použití dvěma řadiči nahrazení logických](#dual-controller-replacement-logic).

5. Když logickou náhradní řadiče přejde na pozadí, připojte kabely. Dávejte připojení všechny kabely stejným způsobem, že byli připojení před nahradit. Najdete v článku podrobné pokyny pro modelu kabel zařízení udělejte [nainstalovat zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md).

6. Zapnutí StorSimple zařízení. Pokud používáte 8600 modelu:

    1. Ujistěte se, plášť EBOD dost první.

    2. Počkejte, dokud je spuštěný EBOD přílohu.

    3. Zapnutí primární přílohu.

    4. Po prvním řadiči restartování a je v pořádku stavu, bude mít systém.

    >[AZURE.NOTE] Pokud sledujete zařízení pomocí sériové konzoly, můžete vidět víc restartování při obnovení správce z postup nahrazení. Po zobrazení nabídky sériové konzoly pak víte dokončení nahradit. V nabídce není zobrazena 2,5 hodin obrazovky při zahajování náhradní řadiče domény, kontaktujte [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Odebrání zařízení

Pomocí následujícího postupu odeberete modul vadná řadiče StorSimple zařízení.

>[AZURE.NOTE] Na následujícím obrázku jsou u řadiče 0. U řadiče 1 tyto by vrátit zpět.

#### <a name="to-remove-a-controller-module"></a>Chcete-li odebrat modulu řadiče domény

1. Pochopit zámek modul mezi miniatury a ukazováčkem.

2. Opatrně vměstnat miniatury a ukazováčkem společně vydat řadiče zámek.

    ![Uvolnění řadiče zámek](./media/storsimple-controller-replacement/IC741047.png)

    **Obrázek 2** Uvolnění řadiče zámek

2. Umožňuje zámek jako ty snímků řadiče mimo skříni.

    ![Posouváním řadiče mimo šasi](./media/storsimple-controller-replacement/IC741048.png)

    **Obrázek 3** Posouváním řadiče mimo skříni

## <a name="insert-a-controller"></a>Vložení řadiči

Následující postup slouží k instalaci modulu předávaných factory řadiče po odebrání vadná modulu z vašeho zařízení StorSimple.

#### <a name="to-install-a-controller-module"></a>Instalace modulu řadiče domény

1. Zkontrolujte, zda je škodu ke spojnicím rozhraní. Modul neinstalovat některé ze spojek spojnice poškození nebo ohnuty.

2. Přesuňte modul řadiče do skříni během plně vydání zámek. 

    ![Posouváním řadiče do šasi](./media/storsimple-controller-replacement/IC741053.png)

    **Obrázek 4** Posouváním řadiče do skříni

3. Modul řadiče vložené zahájen zavření zámek nadále nabízená modul řadiče do skříni. Zámek bude vymění Příručka správce na místě.

    ![Zavření řadiče zámek](./media/storsimple-controller-replacement/IC741054.png)

    **Obrázek 5** Zavření zámek řadiče domény

4. Budete mít při přichytí zámek. Indikátor **OK** by se měla na.  

    >[AZURE.NOTE] Může trvat až 5 minut pro správce a LED aktivovat.

5. Ověřte, jestli je nahradit úspěšná, v portálu Azure klasické, přejděte na **zařízení** > **údržbu** > **Stav hardwaru**a ujistěte se, že jsou pořádku řadiči 0 a 1 řadiče (stav je zelené).

## <a name="identify-the-active-controller-on-your-device"></a>Určení aktivní řadiče na vašem zařízení

Existuje řada situací, například registrace nebo řadiče náhradní první zařízení, které vyžadují, abyste vyhledání aktivní řadiče na zařízení StorSimple. Aktivní řadiče zpracuje všechny disku firmware a sítě operace. Vyberte některou z následujících metod k identifikaci aktivní řadiče:

- [Používat portál Azure klasické k identifikaci aktivní správce](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Používat Windows PowerShell pro StorSimple k identifikaci aktivní správce](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Kontrola fyzické zařízení k identifikaci aktivní správce](#check-the-physical-device-to-identify-the-active-controller)

Každá z těchto postupů je popsána níže.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Používat portál Azure klasické k identifikaci aktivní správce

Na portálu Azure klasické, přejděte na **zařízení** > **údržbu**a posuňte do části **řadiče** . Tady můžete ověřit, řadiče domény které je aktivní.

![Určení aktivní řadiče Azure klasické portálu](./media/storsimple-controller-replacement/IC752072.png)

**Obrázek 6** Azure klasické portál zobrazující aktivní správce

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Používat Windows PowerShell pro StorSimple k identifikaci aktivní správce

Při přístupu zařízení pomocí sériové konzoly je zobrazen nápis zprávy. Nápis zpráva obsahuje základní zařízení informace třeba modelu, název, nainstalovaná verze softwaru a stav řadiče domény které přistupujete. Následující obrázek ukazuje příklad nápis zprávy:

![Sériové nápis zprávy](./media/storsimple-controller-replacement/IC741098.png)

**Obrázek 7** Hlavičku zprávy zobrazující řadiče 0 jako aktivní

Nápis zpráva slouží k určení na zařízení, které jste připojeni k aktivní nebo pasivní.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Kontrola fyzické zařízení k identifikaci aktivní správce

Chcete-li určit aktivní řadiče na vašem zařízení, najděte modrý ŘÍZENÉMU nad DATY 5 port na zadní straně primární přílohu.

Pokud tento Indikátor bliká, správce je aktivní a jiných zařízení je v úsporném režimu. Použijte následující diagram a tabulky jako pomocný.

![Základní primární přílohu zařízení s datové porty](./media/storsimple-controller-replacement/IC741055.png)

**Obrázek 8** Zadní strana primární přílohu s dat porty a protokoly sledování LED

|Popisek|Popis|
|:----|:----------|
|1-6|DATA 0 – 5 porty sítě|
|7|Modrá LED|


## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).
