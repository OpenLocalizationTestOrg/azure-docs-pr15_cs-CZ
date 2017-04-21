<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Chcete-li nainstalovat aktualizace 1.2 z portálu Microsoft Azure klasické

1. Na stránce služby StorSimple vyberte svoje zařízení. Přejděte na **zařízení** > **údržbu**.

2. V dolní části stránky klikněte na položku **Zkontrolovat aktualizace**. K vyhledání dostupných aktualizací bude vytvořen projektu. Zobrazí se oznámení, když úloha byla úspěšně dokončena.

3. V části **Aktualizace softwaru** na stejné stránce uvidí, že jsou k dispozici nové aktualizace softwaru. Doporučujeme, abyste si prošli poznámky k verzi před instalací aktualizace 1.2 na vašem zařízení.

    ![Instalace aktualizací](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. V dolní části stránky klikněte na **Instalovat aktualizace**.

5. Zobrazí se výzva k potvrzení. Klikněte na **OK**.

6. Zobrazí dialogové okno **Instalovat aktualizace** . Vaše zařízení musí odpovídat kontroly uvedené v tomto dialogovém okně. Byly tyto kroky provést před aktualizace. Vyberte **pochopit výše uvedený požadavek a jsem připraven k aktualizaci zařízení**. Klikněte na ikonu zaškrtnutí.

    ![Potvrzovací zprávy](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Nastavení automatické kontroly před bude začít. Jedná se o:

    - **Zkontroluje, řadiče stavu** ověřte řadiče zařízení správný a online.
    
    - **Zkontroluje, stav součásti hardware** ověřte, jestli jsou všechny komponenty hardwaru na zařízení s StorSimple správný.
    
    - **Zkontroluje, DATA 0** povolte DATA 0 na vašem zařízení. Není-li toto rozhraní povolena, musíte ji povolit a potom akci opakujte.
    
    - **DATA 2 a 3 dat kontroluje** k ověření, že nejsou povoleny DATA 2 a 3 dat síťová rozhraní. Pokud je povoleno tyto rozhraní, pak musíte zakázat a pokuste se aktualizovat zařízení. Tato kontrola se provádí jenom v případě, že jsou aktualizace ze zařízení softwarem GA. Zařízení s verze 0.1, 0,2 nebo 0,3 nebudete potřebovat kontrola.
    
    - **Kontrola brány** na libovolném zařízení s verzí před aktualizace 1. Kontrola proběhne na všechny zařízení 1 softwarem před aktualizace ale selže na zařízeních, které mají brány nakonfigurován pro rozhraní sítě než DATA 0.
 
    Aktualizace 1.2 použije se jenom Pokud jsou všechny výše uvedené kontroly před aktualizací úspěšně dokončilo. Zobrazí se oznámení, že kontroly před aktualizací probíhají.
  
    ![Předem zkontrolujte oznámení](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Následujícím obrázku je příklad se nepodařilo před upgradem zaškrtnutí. Je třeba ověřit, zda jsou řadiče zařízení správný a online. Taky musíte se kontrola stavu součásti hardwaru. V tomto příkladu řadiče 0 a 1 řadiče součástí třeba pozornost. Možná budete muset kontaktovat Microsoft Support Pokud nemůžete řešení těchto problémů za vás.

     ![Před kontrola se nezdařila.](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Po instalaci aktualizace 1.2 na zařízení s StorSimple DATA 2 a 3 dat kontroly a Kontrola brány již nebude možné potřebné pro budoucí aktualizace. Před kontroly pořád budete vyzváni.


8. Po úspěšném dokončení před upgradem kontroly úloha aktualizace se vytvoří. Zobrazí se oznámení úspěšné vytvoření úloha aktualizace.
 
    ![Vytvoření projektu aktualizace](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Aktualizace se pak použije na vašem zařízení.
 
9. Sledování průběhu projektu aktualizace, klikněte na **Zobrazení projektu**. Na stránce **úlohy** uvidíte aktualizace průběhu. 

    ![Aktualizace průběhu projektu](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Aktualizace bude trvat několik hodin. Kdykoli můžete zobrazit podrobnosti projektu.

    ![Aktualizace podrobností projektu](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Po dokončení projektu, přejděte na stránku **údržby** a posuňte se dolů na **Aktualizace softwaru**.

12. Ověřte, že zařízení běží **StorSimple 8000 řady aktualizace 1.2 (6.3.9600.17584)**. **Poslední aktualizace data** je také třeba změnit.

    ![Stránky údržby](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Zobrazí údržbu režimu aktualizace jsou k dispozici. Tyto aktualizace jsou vyloučit aktualizace, které za následek prostoje zařízení a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení. Postupujte podle pokynů v [instalace aktualizací režimu údržby](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) nainstalovat tyto aktualizace prostřednictvím stránek prostředí Windows PowerShell pro StorSimple.

> [AZURE.NOTE] V určitých případech zpráva s upozorněním údržbu režimu aktualizace jsou k dispozici může zobrazovat až 24 hodin od úspěšně použití aktualizací režimu údržbu na zařízení.  


