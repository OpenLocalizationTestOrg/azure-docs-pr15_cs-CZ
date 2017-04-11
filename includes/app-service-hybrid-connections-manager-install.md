
1. V zásuvné **hybridní připojení** klikněte na hybridní připojení, který jste právě vytvořili a potom klikněte na příkaz **Vzhled posluchače**.
    
    ![Klikněte na příkaz Vzhled posluchače](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Otevře se zásuvné **hybridní vlastnosti připojení** . V části **Místní hybridní Connection Manager**zvolte **Stáhnout a konfigurovat ručně**, pokud chcete stažený balíček HybridConnectionManager.msi uložit a zkopírujte připojovací řetězec brány.
    
    ![Instalace po kliknutí sem](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Příkazový řádek správce zadejte tento příkaz spusťte instalační program:

        start HybridConnectionManager.msi
 
7. Po spuštění instalačního programu klikněte na **Ne nyní**, a pak přejděte do složky %ProgramFiles%\Microsoft\HybridConnectionManager, spusťte HCMConfigWizard.exe a v dialogové okno **Řízení uživatelských účtů** kliknutím na **Ano** .
        
7. Vložte hybridní připojovací řetězec, který jste si zkopírovali a klikněte na **OK**. 
    
    ![Instalace](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Po dokončení instalace klikněte na **Zavřít**.
    
    ![Klikněte na tlačítko Zavřít](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    Ve sloupci **Stav** na zásuvné **hybridní připojení** nyní zobrazí **Připojit**. 
    
    ![Připojení stavu](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)