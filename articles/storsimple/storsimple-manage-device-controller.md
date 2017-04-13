<properties
   pageTitle="Správa řadiče zařízení StorSimple | Microsoft Azure"
   description="Zjistěte, jak zastavit, restartujte, vypnout nebo obnovit řadiči StorSimple zařízení."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Správa řadiči StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento kurz popisuje různých operacích, které lze provádět řadiči zařízení StorSimple. Řadiče v zařízení s StorSimple jsou nadbytečné (mezi dvěma účastníky) řadiče v konfiguraci aktivní pasivní. V daném okamžiku jedinou řadiče je aktivní a je zpracování všech disk a síť. Zařízení je v pasivní režim. Pokud aktivní řadiče nepovede, pasivní řadiče aktivuje automaticky.

Tento kurz obsahuje podrobné pokyny ke správě řadiče zařízení pomocí:

- **Řadiče** část stránky **údržby** ve službě StorSimple správce
- Prostředí Windows PowerShell pro StorSimple.

Doporučujeme spravovat řadiče zařízení pomocí služby StorSimple správce. Pokud akce můžete provádět pouze pomocí prostředí Windows PowerShell pro StorSimple, kurzu vytvoří poznámku od něj.

Po přečtení tohoto kurzu, budou moct:

- Restartování nebo vypnutí řadiči StorSimple zařízení
- Vypnutí StorSimple zařízení
- Resetujte StorSimple zařízení na výchozí


## <a name="restart-or-shut-down-a-single-controller"></a>Restartování nebo vypnutí jednoho řadiče

Restartování řadiče domény nebo vypnutí nepožaduje jako součást operace normální systému. Vypnutí operace u řadiče jednoho zařízení jsou běžné pouze v případech, kdy komponenta hardwaru zařízení se nezdařilo vyžaduje náhradní. Restartování řadiče domény může být nutné také v případě, ve kterém výkon ovlivněn spotřebu paměti nadbytečné nebo nefunkční řadiče. Budete taky muset po náhradní řadiče domény úspěšné, restartujte řadiči Pokud chcete povolit a test nahrazený zařízení.

Restartování zařízení není skutečnosti připojeného iniciátory za předpokladu, že pasivní řadiče je k dispozici. Pokud pasivní řadiče není k dispozici nebo vypnut vypnuto a pak restartování aktivní správce může způsobit přerušení služby a výpadek služeb.

> [AZURE.IMPORTANT]

> - **Řadiči pracovního by nikdy odeberou fyzicky jako to může vést ztrátu redundance a lepší riziko výpadek služeb.**

> - Následující postup platí jenom pro zařízení fyzicky StorSimple. Informace o tom, jak začít, zastavení a nové spuštění virtuální zařízení najdete v tématu [práce s virtuální zařízení](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Restartujte nebo vypnout řadiči jednoho zařízení tak, že na portálu Azure klasické StorSimple Správce služeb nebo prostředí Windows PowerShell pro StorSimple

Pokud chcete spravovat zařízení řadiče z portálu Microsoft Azure klasické, proveďte následující kroky.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Restartujte nebo vypnutí řadiči klasické portálu

1. Přejděte na **zařízení > údržbu**.

1. Přejděte na **Stav hardwaru** a ověřte, že stav řadiče na zařízení je v **pořádku**.

    ![Ověřte, jestli jsou pořádku řadiči StorSimple zařízení](./media/storsimple-manage-device-controller/IC766017.png)

1. V dolní části stránky **údržby** klikněte na **Spravovat řadiče**.

    ![Správa řadiče StorSimple zařízení](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Pokud nevidíte **Správa řadiče**, budete muset nainstalovat aktualizace. Další informace najdete v tématu [aktualizace StorSimple zařízení](storsimple-update-device.md).

1. V dialogovém okně **Změnit nastavení řadiče domény** postupujte takto:
    1. V rozevíracím seznamu **Vyberte zařízení** vyberte správce, který chcete spravovat. Možnosti jsou řadiče 0 a 1 řadiče domény. Tyto řadiče jsou také označena jako aktivní nebo pasivní.

        >[AZURE.NOTE] Řadiči nelze spravovat, pokud je k dispozici nebo zapnutá vypnuto a ta se nezobrazí v rozevíracím seznamu.

    2. Z rozevíracího seznamu **Vyberte akce** vyberte **restartovat řadiče domény** nebo **vypnout řadiče domény**.

        ![Restartujte pasivní řadiče domény StorSimple zařízení](./media/storsimple-manage-device-controller/IC766020.png)
    3. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-device-controller/IC740895.png).

To bude restartovat nebo vypnout správce. Následující tabulka obsahuje souhrn podrobností o co se stane v závislosti na výběr, které jste provedli v dialogovém okně **Změnit nastavení řadiče domény** .  


|Výběr #|Pokud se rozhodnete...|To se stane.|
|---|---|---|
|1.|Restartujte pasivní správce.|Úlohy se vytvoří Restartujte zařízení a budete upozorněni úspěšně vytvořený projektu. To zahájí restart řadiče domény. Restartování procesu můžete sledovat pomocí přístup k **služby > řídicí panel > Zobrazit protokoly operací** a potom filtrování podle parametry specifické pro vaši službu.|
|2.|Restartujte aktivní správce.|Zobrazí se následující upozornění: "Pokud restartujte aktivní Správce zařízení se nepovede přes pasivní řadiči. Chcete nadále?" </br>Pokud budete chtít pokračovat v operaci, dalším krokům budou shodovat s klávesovými zkratkami restartujte pasivní řadiči (viz výběr 1).|
|3.|Vypnutí pasivní správce.|Zobrazí se tato zpráva: "po dokončení vypnutí musíte stisknout tlačítko power na ovladači ho zapnout. Opravdu že chcete vypnout tohoto řadiče?" </br>Pokud budete chtít pokračovat v operaci, dalším krokům budou shodovat s klávesovými zkratkami restartujte pasivní řadiči (viz výběr 1).|
|4.|Vypněte aktivní správce.|Zobrazí se tato zpráva: "po dokončení vypnutí musíte stisknout tlačítko power na ovladači ho zapnout. Opravdu že chcete vypnout tohoto řadiče?" </br>Pokud budete chtít pokračovat v operaci, dalším krokům budou shodovat s klávesovými zkratkami restartujte pasivní řadiči (viz výběr 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Restartujte nebo vypnutí řadiči v prostředí Windows PowerShell pro StorSimple
Provedení následujících kroků můžete vypnout nebo restartovat jednoho řadiče na vašem zařízení StorSimple z portálu Microsoft Azure klasické.


1. Přístup k zařízení pomocí sériové konzoly nebo rámci těchto relací ze vzdáleného počítače. Připojení k řadiče 0 a 1 řadiče domény provedením kroků v tématu [Použití nátěrové připojení konzoly sériové zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup**.

1. V okně zprávy nápis, poznamenejte si řadiče domény jste připojeni k (řadiče 0 či 1 řadiče domény) a zda je aktivní nebo pasivní řadiči (úsporném režimu).
    - Vypnutí řadiči jednoho příkazového řádku zadejte:

        `Stop-HcsController`

        To bude ukončena správce, který jste připojeni k. Chcete-li zrušit aktivní řadiče, pak se nezdaří přes pasivní řadiče před jejím vypnutím.
    - Restartujte zařízení, do příkazového řádku zadejte:

        `Restart-HcsController`

        To bude Restartujte zařízení, které jste připojeni k. Pokud restartujte aktivní řadiče se nepovede přes pasivní řadiči před restartování.


## <a name="shut-down-a-storsimple-device"></a>Vypnutí StorSimple zařízení

Tento oddíl vysvětluje, jak vypnout spuštěné nebo selhání StorSimple zařízení ze vzdáleného počítače. Zařízení je normálně vypnuté po vypnutí řadiče zařízení. Vypnutí zařízení se provádí při zařízení fyzicky přesunut nebo se považuje mimo služby.

> [AZURE.IMPORTANT] Před vypnutím zařízení Kontrola stavu součásti zařízení. Přejděte na **zařízení > Údržba > Stav hardwaru** a ověřte, zda je Indikátor stavu všechny komponenty zelená. Správný zařízení bude mít zeleným stavovým. Pokud vaše zařízení je právě vypnout a nahrazení nefunkční komponenty, zobrazí se selhalo (červená) nebo jejich funkčnost bude omezena (žlutá) stav u odpovídajících komponenty.

#### <a name="to-shut-down-a-storsimple-device"></a>Vypnutí StorSimple zařízení

1. Pomocí postupu [Restartovat nebo vypnout řadiči](#restart-or-shut-down-a-single-controller) identifikovat a vypněte pasivní řadiče na vašem zařízení. Provedením této operaci v portálu Azure klasické nebo v prostředí Windows PowerShell pro StorSimple.
2. Opakujte výše uvedené krok vypnout aktivní správce.
3. Teď je třeba podívejte se na zadní rovině zařízení. Po dva řadiče úplně vypnout, stav LED v obou řadiči by měl blikající červená. Pokud potřebujete úplně vypnout zařízení v současné době, kliknutím na překlopte vypínač Power a chlazení moduly (PCMs) do polohy vypnuto. To měli vypnout zařízení.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Resetujte tovární výchozí nastavení

> [AZURE.IMPORTANT] Pokud potřebujete resetujte zařízení tovární výchozí nastavení, kontaktujte Microsoft Support. Níže popsaný postup bude použito pouze ve spojení s Microsoft Support.

Tento postup popisuje, jak obnovit zařízení Microsoft Azure StorSimple do výchozího nastavení factory pomocí Windows Powershellu pro StorSimple.
Resetování zařízení odebere všechna data a nastavení celého obrázku ve výchozím nastavení.

Proveďte následující kroky o resetování vašeho zařízení Microsoft Azure StorSimple tovární výchozí nastavení:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Obnovit do výchozího nastavení v prostředí Windows PowerShell pro StorSimple zařízení

1. Pomocí sériové konzoly přístup k zařízení. Zkontrolujte zprávu nápis ověřit, zda jste připojeni k aktivní řadiči.

1. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup**.

1. Do příkazového řádku zadejte tento příkaz Obnovit celého obrázku odebrat všechna data, metadat a řadiče domény nastavení:

    `Reset-HcsFactoryDefault`

    Obnovíte místo jedné řadiče, získáte pomocí rutiny [Obnovit HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) s `-scope` parametr.)

    Jestli chcete systém se restartuje tisknutím. Zobrazí se oznámení při obnovení úspěšně dokončil. V závislosti na modelu systému může to trvat 45 60 minut 8100 zařízení a 60 90 minut 8600 do konce tomto procesu.

    > [AZURE.TIP]

    > - Pokud používáte aktualizace 1.2 nebo starším použít `–SkipFirmwareVersionCheck` parametr přeskočit Kontrola verze firmwaru (jinak se zobrazuje Chyba neshodné firmware: Factory obnovit nemůže pokračovat kvůli neshodu verze firmware. ).

    > - Postup obnovit factory může selhat StorSimple zařízení se systémem aktualizace 1 nebo 1.1 na portálu pro státní správu provedli úspěšné řadiče jedné nebo dvou nahradit (náhradní řadiče dodaných s před aktualizace 1). Tím se stane, když továrny obnovit, že obrázek proběhne přítomnost soubor SHA1 řadiči, který neexistuje pro před aktualizace 1. Pokud se zobrazí, že tento factory obnovit selhání, kontaktujte Microsoft Support která vám pomůže s dalším krokům. Tento problém není vidět s náhradní řadiče, které byly odeslány z factory aktualizace 1 nebo novější software.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Otázky a odpovědi týkající se správy řadiče zařízení

V této části jsme vytvořili souhrn některé často kladené otázky týkající se správou zařízení řadiče StorSimple.

**OTÁZKA:** Co se stane, pokud jsou oba řadiče na svém zařízení správný a zapnutá na a restartujte nebo vypnout aktivní správce?

**ODPOVĚĎ:** Pokud jsou oba řadiče na vašem zařízení správný a zapnutá, zobrazí se výzva k potvrzení. Můžete:

- **Restartujte aktivní řadiče** – zobrazí se upozornění, že restartování aktivní řadiče způsobí převzít pasivní Správce zařízení. Správce restartuje.

- **Vypnout aktivní řadiče** – zobrazí se upozornění, že vypnutí aktivní řadiče bude mít za následek výpadek služeb. Musíte také stisknutím tlačítka power na zařízení zapnout správce.

**OTÁZKA:** Co se stane, pokud pasivní řadiče na svém zařízení není k dispozici nebo zapnutá vypnuto a restartujte nebo vypnout aktivní správce?

**ODPOVĚĎ:** Pokud je pasivní řadiče na vašem zařízení není k dispozici nebo vypnut vypnuto a budete chtít:

- **Restartujte aktivní řadiče** – zobrazí se oznámení, že pokračováním operace bude mít za následek dočasné přerušení služby a zobrazí se výzva k potvrzení.

- **Vypnout aktivní řadiče** – zobrazí se oznámení, že pokračováním operace bude mít za následek výpadek služeb a, že byste potřebovali stisknutím tlačítka power na jeden nebo oba řadiče zapnout zařízení. Zobrazí se výzva k potvrzení.

**OTÁZKA:** Kdy se řadiče restartování nebo vypnutí přestane průběhu?

**ODPOVĚĎ:** Pokud se nemusí podařit restartování nebo vypnutí řadiči:

- Probíhá aktualizace zařízení.

- Restartování řadiče domény už probíhá.

- Vypnutí řadiče domény už probíhá.

**OTÁZKA:** Jak lze zjistit, pokud byl správce nerestartuje nebo vypnout?

**ODPOVĚĎ:** Stav řadiče domény na stránce Údržba. Stav řadiče výskyt znamená, jestli má řadiči nerestartuje nebo vypnout. Stránka upozornění navíc bude obsahovat informační upozornění, pokud byl správce nerestartuje nebo vypnout. Operace restart a vypnutí řadiče domény se také zaznamenávají v protokolech operace. Další informace o protokoly operací přejděte na [Zobrazit protokoly operací](storsimple-service-dashboard.md#view-the-operations-logs).

**OTÁZKA:** Existuje žádný vliv na vstupně-výstupních důsledku převzetí řadiče domény?

**ODPOVĚĎ:** TCP připojení mezi iniciátory a řadiče domény active obnoví důsledku převzetí řadiče domény, ale bude znovu vytvoří, když pasivní řadiče předpokládá operace. V průběhu operace může být dočasně (méně než 30 sekund) přerušíte výstupní aktivita mezi iniciátory a zařízení.

**OTÁZKA:** Jak se vrátit Moje řadiče domény na služby po byl vypnout a odebrat?

**ODPOVĚĎ:** Vrátit řadiči služby, musíte vložením do skříni podle popisu v [modulu řadiče domény na zařízení s StorSimple nahradit](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Další kroky

- Pokud dojde k potížím s řadiči StorSimple zařízení, které nelze přeložit pomocí postupů uvedených v tomto kurzu, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md).

- Další informace o použití služby StorSimple správce, přejděte na článek [použití službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
