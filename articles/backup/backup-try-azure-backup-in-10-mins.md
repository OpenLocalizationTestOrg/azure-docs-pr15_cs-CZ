<properties
   pageTitle="Zjistěte, jak pomocí zálohujte souborů a složek z Windows Azure pomocí zálohování Azure pomocí Správce prostředků nasazení modelu | Microsoft Azure"
   description="Informace o vytváření trezoru, nainstalujte agenta služby Recovery a zálohování soubory a složky na Azure zálohovat data systému Windows Server."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="cfreeman"
   editor=""
   keywords="Postup zálohování; Postup zálohování"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.date="09/27/2016"
   ms.author="markgal;"/>

# <a name="first-look-back-up-files-and-folders-with-azure-backup-using-the-resource-manager-deployment-model"></a>Nejdřív najděte: zálohovat soubory a složky pomocí zálohování Azure pomocí Správce prostředků nasazení modelu

Tento článek vysvětluje, jak obecnějším údajům serveru Windows (nebo klienta Windows) souborů a složek na Azure pomocí zálohování Azure pomocí Správce prostředků. Je kurz určen vás provede základní informace. Pokud chcete začít používat zálohování Azure, jste na správném místě.

Chcete se dozvědět víc o zálohování Azure, najdete v tématu [Přehled](backup-introduction-to-azure-backup.md).

Zálohování souborů a složek na Azure vyžaduje těchto činností:

![Krok 1](./media/backup-try-azure-backup-in-10-mins/step-1.png) získat Azure předplatného (pokud ještě nemáte jednu).<br>
![Krok 2](./media/backup-try-azure-backup-in-10-mins/step-2.png) vytvořit služby Recovery trezoru.<br>
![Krok 3](./media/backup-try-azure-backup-in-10-mins/step-3.png) stáhněte všechny potřebné soubory.<br>
![Krok 4](./media/backup-try-azure-backup-in-10-mins/step-4.png) nainstalovat a přihlásit agenta obnovení služby.<br>
![Krok 5](./media/backup-try-azure-backup-in-10-mins/step-5.png) zálohovat soubory a složky.

![Postup zálohování počítači se systémem Windows Azure zálohování](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

## <a name="step-1-get-an-azure-subscription"></a>Krok 1: Stáhněte si předplatné Azure

Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/) , který umožňuje přístup k jiné služby Azure.

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2: Vytvoření trezoru obnovení služby

K obecnějším údajům soubory a složky, je potřeba vytvořit trezoru využití služeb v oblasti, ve které chcete data uložit. Potřebujete zjistit, jak chcete úložišti replikovat.

### <a name="to-create-a-recovery-services-vault"></a>Vytvoření trezoru obnovení služby

1. Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com/) pomocí předplatného Azure.

2. V nabídce rozbočovači klikněte na tlačítko **Procházet** a v seznamu zdrojů zadejte **Služby Recovery** a na tlačítko **služby Recovery trezorů**.

    ![Vytvoření obnovení služby trezoru kroku 1](./media/backup-try-azure-backup-in-10-mins/browse-to-rs-vaults.png) <br/>

3. V nabídce **trezorů obnovení služby** klikněte na **Přidat**.

    ![Vytvoření obnovení služby trezoru krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Zásuvné trezoru služby Recovery otevře, která vás vyzve, abyste jim poslali **název** **předplatného**, **pole Skupina zdroje**a **umístění**.

    ![Vytvoření služby Recovery trezoru kroku 5](./media/backup-try-azure-backup-in-10-mins/rs-vault-attributes.png)

4. Pole **název**zadejte popisný název k identifikaci trezoru.

5. Klikněte na **předplatné** zobrazíte dostupná seznam předplatných.

6. **Pole Skupina zdroje** zobrazíte seznamu dostupné zdroje skupin nebo klikněte na **Nový** k vytvoření nové skupiny prostředků.

7. Klikněte na **umístění** vyberte zeměpisná oblast pro trezoru. Tato možnost určuje zeměpisnou oblast, kde se odesílá záložní data.

8. Klikněte na **vytvořit**.

    Pokud nevidíte trezoru uvedené po jejím dokončením, klikněte na **Aktualizovat**. Při aktualizaci seznamu klikněte na název trezoru.

### <a name="to-determine-storage-redundancy"></a>Chcete-li zjistit redundance úložiště
Při prvním vytvoření trezoru služby Recovery zjistíte, jak replikovat úložiště.

1. Klikněte na nový trezoru otevřete řídicího panelu.

2. V **Nastavení** zásuvné, který se automaticky otevře s řídícího trezoru, klepněte na **Zálohování infrastruktury**.

3. V zásuvné zálohování infrastruktury klepněte na **Zálohování konfigurace** zobrazíte **typ replikace úložiště**.

    ![Vytvoření služby Recovery trezoru kroku 5](./media/backup-try-azure-backup-in-10-mins/backup-infrastructure.png)

4. Zvolte možnost replikace odpovídající úložiště pro trezoru.

    ![Seznam obnovení služeb trezorů](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Ve výchozím nastavení obsahuje trezoru geo nadbytečné úložiště. Pokud používáte Azure jako primární úložišti koncový bod, používejte dál jednotné geo nadbytečné úložiště. Pokud používáte Azure jako koncového bodu-primární úložišti, klikněte na místní nadbytečné úložiště, která bude snížit náklady na uchovávání dat v Azure. Další informace o [geo nadbytečné](../storage/storage-redundancy.md#geo-redundant-storage) a [místně nadbytečné](../storage/storage-redundancy.md#locally-redundant-storage) možnosti ukládání v tomto [Přehled](../storage/storage-redundancy.md).

Teď, když jste vytvořili trezoru, připravte infrastrukturu k obecnějším údajům souborů a složek stažením Microsoft Azure obnovení Services agent a trezoru přihlašovací údaje.

## <a name="step-3---download-files"></a>Krok 3: soubor ke stažení souborů

1. Klikněte na **Nastavení** na řídicím panelu služby Recovery trezoru.

    ![Otevřít záložní cíl zásuvné](./media/backup-try-azure-backup-in-10-mins/settings-button.png)

2. Klikněte na **Začínáme > Zálohování** na zásuvné nastavení.

    ![Otevřít záložní cíl zásuvné](./media/backup-try-azure-backup-in-10-mins/getting-started-backup.png)

3. **Zálohování hledání** klikněte na zásuvné zálohování.

    ![Otevřít záložní cíl zásuvné](./media/backup-try-azure-backup-in-10-mins/backup-goal.png)

4. Vyberte **místní** z kde je vaše pracovní zátěž spuštěna? v nabídce.

5. Vyberte **soubory a složky,** k čemu se chcete zálohovat? Nabídka a klikněte na **OK**.

### <a name="download-the-recovery-services-agent"></a>Stažení služby Recovery agenta

1. Klikněte na **Stáhnout Agent pro Windows Server nebo klienta Windows** v zásuvné **připravit infrastruktury** .

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-short.png)

2. V místní nabídce stahování klikněte na **Uložit** . Ve výchozím nastavení **MARSagentinstaller.exe** soubor se uloží do složky pro stahování.

### <a name="download-vault-credentials"></a>Stahování přihlašovacích údajů trezoru

1. Klikněte na **Stáhnout > Uložit** na zásuvné infrastruktury připravit.

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-download.png)

## <a name="step-4--install-and-register-the-agent"></a>Krok 4 – instalovat a registrovat agent

>[AZURE.NOTE] Povolení zálohování pomocí portálu Azure je brzy k dispozici. V současné době použijete k obecnějším údajům soubory a složky agentem služeb Microsoft Azure obnovení místní.

1. Vyhledejte a poklikejte na **MARSagentinstaller.exe** z složce Downloads (nebo uložený jinde).

2. Dokončete Průvodce nastavením agentem služeb Microsoft Azure obnovení. Dokončete průvodce potřebujete:

    - Vyberte umístění pro složku mezipaměti a instalaci.
    - Zadejte vašeho proxy server informací o serveru proxy server používáte pro připojení k Internetu.
    - Obsahují vaše uživatelské jméno a heslo podrobnosti použijete ověřeným proxy.
    - Pokud chcete stažený trezoru pověření
    - Uložte heslo šifrování na zabezpečeném místě.

    >[AZURE.NOTE] Pokud jste ztratili nebo zapomenete heslo, nemůže zajistit Microsoft obnovení záložních dat. Uložte soubor na zabezpečeném místě. Je potřeba k obnovení záložní.

Agent je teď nainstalovaný a váš počítač je registrovaná do trezoru. Jste připravení ke konfiguraci a naplánovat zálohování.

## <a name="step-5-back-up-your-files-and-folders"></a>Krok 5: Obecnějším údajům souborů a složek

Počáteční zálohování obsahuje dva klíčových úkolů:

- Plánování zálohování
- Vytvoření zálohy souborů a složek poprvé

Dokončete počáteční zálohování pomocí služby Microsoft Azure Recovery agent.

### <a name="to-schedule-the-backup"></a>Naplánování zálohování

1. Otevřete agenta Microsoft Azure obnovení Services. Najdete ji vyhledáním počítači **Microsoft Azure zálohování**.

    ![Spuštění služby Azure Recovery agent](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. V agenta obnovení služby klikněte na **Naplánovat zálohu**.

    ![Naplánování zálohy systému Windows Server](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Na stránce Začínáme Průvodce plánem zálohování klikněte na **Další**.

4. Na vyberte položky, které chcete zálohování stránky klikněte na **Přidat položky**.

5. Vyberte soubory a složky, které chcete zálohovat a potom klikněte na **OK**.

6. Klikněte na tlačítko **Další**.

7. Na stránce **Určit plán zálohování** určit **plán zálohování** a klikněte na tlačítko **Další**.

    Můžou plánovat denně (maximální rychlostí třikrát za den) nebo týdenní zálohy.

    ![Položky pro zálohování serveru](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

    >[AZURE.NOTE] Další informace o tom, jak určit plán zálohování, najdete v článku [Použití Azure záložní nahrazení infrastrukturu páskou](backup-azure-backup-cloud-as-tape.md).

8. Na stránce **Vyberte zásady uchovávání informací** vyberte **Zásady uchovávání informací** záložní kopie.

    Zásady uchovávání informací určuje doby trvání, u kterého budou uloženy zálohování. Místo jenom určující "ploché zásadu" pro všechny záložní body, můžete určit, zásady uchovávání informací různých podle toho, kdy dojde k zálohování. Zásady uchovávání informací denně, týdně, měsíční a roční podle vlastní potřeby můžete změnit.

9. Na stránce zvolit počáteční záložní typ zvolte typ počáteční zálohování. Ponechte možnost **automaticky v síti** vybranou a klikněte na tlačítko **Další**.

    Můžete zálohovat automaticky přes síť nebo můžete obecnějším údajům v offline režimu. Zbývající Tento článek popisuje postup zálohování automaticky. Pokud chcete provést zálohu offline, přečtěte si článek [Offline záložní pracovního postupu v Azure zálohování](backup-azure-backup-import-export.md) doplňující informace.

10. Na stránce potvrzení zkontrolujte informace a potom klikněte na **Dokončit**.

11. Po dokončení Průvodce vytvořením zálohy plánu, klepněte na tlačítko **Zavřít**.

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>K obecnějším údajům souborů a složek poprvé

1. Ve službě agent obnovení služby klikněte na **Zálohovat** dokončete počáteční ohlašovat v síti.

    ![Teď zálohy systému Windows Server](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

2. Na stránce potvrzení zkontrolujte nastavení, která zase teď průvodce použijete k obecnějším údajům v počítači. Potom klikněte na **Zpět**.

3. Klikněte na **Zavřít** zavřete průvodce. V takovém případě před dokončením procesu zálohování, zůstane v Průvodci běží na pozadí.

Po dokončení počáteční zálohování se zobrazí v konzole zálohování stav **dokončení projektu** .

![IR dokončení](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Otázky?
Pokud máte nějaké dotazy nebo pokud je všechny funkce, které chcete zobrazit však započítávány, [napište nám](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
- Pokud potřebujete další informace o [zálohování počítačích Windows](backup-configure-vault.md).
- Teď jste zálohovala soubory a složky, můžete [Spravovat trezorů a servery](backup-azure-manage-windows-server.md).
- Pokud potřebujete obnovení záložní, použijte tento článek obnovení [souborů na počítači Windows](backup-azure-restore-windows-server.md).
