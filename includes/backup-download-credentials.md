## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Pomocí přihlašovacích údajů trezoru ověření se službou Azure zálohování

Místního serveru (Windows klienta nebo serveru Windows Server nebo správce ochranu dat) je potřeba ověřit s záložní trezoru ji zálohujte dat na Azure. Ověřování dosáhnete pomocí "trezoru přihlašovací údaje". Koncepce trezoru pověření je podobný pojem "nastavení publikování" soubor, který se používá v prostředí PowerShell Azure.

### <a name="what-is-the-vault-credential-file"></a>Co je soubor trezoru přihlašovacích údajů

Soubor trezoru přihlašovací údaje je certifikát generovaný portálu pro každou záložní trezoru. Na portálu pak odešle veřejný klíč pro služby Access řízení (ACS). Privátním klíčem certifikát je k dispozici uživatelům v rámci pracovního postupu, která je uvedena jako vstup v pracovním postupu registrace počítače. Tím se ověří počítače odeslání zálohování dat do identifikované trezoru služby Azure záložní.

Pověření trezoru se použije jenom při registraci pracovního postupu. Je uživatele odpovědností zajistit, že není ohroženo souboru trezoru přihlašovací údaje. Nacházející se v ruce jakékoli neoprávněné uživatelem, soubor přihlašovací údaje trezoru lze použít k registraci jiných počítačů proti stejné trezoru. Jako záložních dat je šifrovaná pomocí heslo, které patří k zákazníkovi, ale nemůže být ohroženo existující záložních dat. Zmírnění tento problém, trezoru přihlašovací údaje jsou nastavené platnosti v 48hrs. Můžete si stáhnout trezoru pověření záložní trezoru počtu opakování – ale jenom nejnovější trezoru pověření souboru platí při registraci pracovního postupu.

### <a name="download-the-vault-credential-file"></a>Stáhněte si soubor trezoru přihlašovacích údajů

Stažení souboru pověření trezoru prostřednictvím zabezpečeného kanálu z portálu Microsoft Azure. Služby Azure zálohování nezaznamená privátním klíčem certifikát, a privátním klíčem není zachován v portálu nebo službu. Pomocí následujících kroků trezoru pověření budou moct soubor stáhnout do místního počítače.

1.  Přihlaste se k [portálu Správa](https://manage.windowsazure.com/)
2.  Klikněte na **Využití služeb** v levém navigačním podokně a vyberte záložní trezoru, který jste vytvořili. Klikněte na ikonu cloudu pro přístup k zobrazení úvodní záložní trezoru.

    ![Rychlé zobrazení](./media/backup-download-credentials/quickview.png)

3.  Na stránce Snadné spuštění klikněte na **Stáhnout trezoru pověření**. Na portálu vygeneruje trezoru pověření soubor, který je k dispozici ke stažení.

    ![Ke stažení](./media/backup-download-credentials/downloadvc.png)

4.  Na portálu vygeneruje trezoru přihlašovací údaje pomocí kombinace názvu trezoru a aktuálním datem. Klepnutím na tlačítko stáhněte trezoru pověření místního účtu stahování složky **Uložit** nebo uložit jako vyberte z nabídky Uložit do zadejte umístění pro ni přihlašovací údaje trezoru.

### <a name="note"></a>Poznámka:
- Zajistit, že trezoru pověření se uloží do umístění, které můžete k nim získat přístup z počítače. Pokud je uložený na sdílet/SMB soubor, vyhledejte přístupová oprávnění.
- Soubor trezoru přihlašovacích údajů se používá pouze během pracovní postup registrace.
- Soubor přihlašovací údaje trezoru vyprší po 48hrs a si můžete stáhnout z portálu.
- V nápovědě k zálohování Azure [Nejčastější dotazy](../articles/backup/backup-azure-backup-faq.md) pro všechny dotazy týkající se pracovního postupu.
