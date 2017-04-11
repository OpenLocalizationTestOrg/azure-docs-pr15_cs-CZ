## <a name="download-install-and-register-the-azure-backup-agent"></a>Stažení, instalace a zaregistrovat agenta zálohování Azure

Po vytvoření trezoru zálohování Azure, měli byste mít nainstalované agent na každý z počítače Windows (Windows Server, klienta systému Windows, System Center Data Protection Manager serveru nebo Azure zálohování serveru), který umožňuje zálohování dat a aplikací Azure.

1. Přihlaste se k [portálu Správa](https://manage.windowsazure.com/)

2. Klikněte na **Zotavení služby**a potom vyberte záložní trezoru, kterou chcete zaregistrovat se serverem. Zobrazí se stránka rychlý Start, které záložní trezoru.

    ![Rychlý start](./media/backup-install-agent/quickstart.png)

3. Na stránce Rychlý Start klikněte na možnost **systému Windows Server nebo System Center Data Protection Manager nebo Windows klienta** v části **Stáhnout Agent**. Klepněte na tlačítko **Uložit** ho zkopírujte do místního počítače.

    ![Uložení agent](./media/backup-install-agent/agent.png)

4. Po instalaci používat agent, poklikejte na MARSAgentInstaller.exe spustit instalaci agenta Azure zálohování. Zvolte instalace složce a pomocné povinné agenta. Umístění mezipaměti zadali, musíte mít volného místa, což je alespoň na úrovni 5 % záložních dat.

5.  Pokud používáte proxy server pro připojení k Internetu, na obrazovce **konfigurací Proxy** zadejte proxy server podrobnosti. Pokud používáte ověřeným proxy, zadejte uživatelské jméno a heslo podrobnosti na této obrazovce.

6.  Agent Azure zálohování instalace prostředí Windows PowerShell a .NET Framework 4.5 (pokud ještě není k dispozici) dokončete instalaci.

7.  Agent po instalaci používat, klikněte na tlačítko **pokračovat k registraci** s tímto pracovním postupem.

    ![Registrace](./media/backup-install-agent/register.png)

8. Na obrazovce přihlašovací údaje trezoru vyhledejte a vyberte soubor trezoru přihlašovací údaje, který jste stáhli.

    ![Přihlašovací údaje trezoru](./media/backup-install-agent/vc.png)

    Soubor přihlašovací údaje trezoru platí pouze pro 48 hodin (po stažení z portálu). Pokud dojde k každé chyby v tomto obrazovky (například "trezoru přihlašovací údaje soubor poskytovat vypršela") Login to přihlášení k portálu Azure a trezoru přihlašovací údaje budou moct soubor stáhnout znovu.

    Zajistěte, aby byl soubor trezoru přihlašovací údaje k dispozici v umístění, které jsou přístupné tak, že instalační program aplikace. Pokud se setkáte přístup související chyby, zkopírujte soubor přihlašovací údaje trezoru dočasné místo v tomto počítači a opakujte.

    Pokud dojde k chybě neplatné trezoru přihlašovacích údajů (například "neplatné trezoru přihlašovací údaje k dispozici") je buď poškozený soubor nebo znamená mít nejnovější pověření nesouvisející ke službě zotavení. Opakujte po stažení nový soubor pověření trezoru z portálu. Tato chyba obvykle dochází, pokud uživatel klikne na možnost **stahování trezoru pověření** v portálu Azure rychle po sobě. V tomto případě platí pouze druhý soubor trezoru přihlašovacích údajů.

9. Na obrazovce **Nastavení šifrování** můžete generovat přístupové heslo nebo zadat přístupové heslo (aspoň 16 znaků). Nezapomeňte uložit heslo na zabezpečeném místě.

    ![Šifrování](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Pokud heslo ztratili nebo zapomněli; Při obnovení záložních dat nelze Nápověda pro Microsoft. Koncový uživatel vlastní heslo šifrování a Microsoft nemá viditelnost do heslo používaný koncového uživatele. Uložte soubor na zabezpečeném místě je to potřeba během operace obnovení.

10. Po kliknutí na tlačítko **Dokončit** počítači se registrace úspěšně do trezoru a nyní jste připraveni začít zálohování na Microsoft Azure.

11. Při použití můžete upravit nastavení zadané během pracovního postupu registrace kliknutím na možnost **Změnit vlastnosti** v modulu snap mmc Azure zálohování v samostatné Microsoft Azure zálohování.

    ![Změna vlastností](./media/backup-install-agent/change.png)

    Můžete taky při použití Správce ochranu dat, můžete změnit nastavení nastavil během pracovního postupu registrace kliknutím na možnost **Konfigurovat** tak, že vyberete **Online** na kartě **Správa** .

    ![Konfigurace Azure zálohování](./media/backup-install-agent/configure.png)
