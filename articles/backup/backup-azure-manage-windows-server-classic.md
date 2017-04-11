<properties
    pageTitle="Správa trezorů Azure zálohování a servery pomocí klasické nasazení modelu Azure | Microsoft Azure"
    description="Pomocí tohoto kurzu se dozvíte, jak spravovat trezorů Azure zálohování a servery."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jimpark;markgal"/>


# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Správa trezorů Azure zálohování a servery pomocí klasické nasazení modelu

> [AZURE.SELECTOR]
- [Správce prostředků](backup-azure-manage-windows-server.md)
- [Klasický](backup-azure-manage-windows-server-classic.md)

V tomto článku najdete základní informace dostupné prostřednictvím portálu Azure klasické a Microsoft Azure záložní agent záložní správy úkolů.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků nasazení modelu.

## <a name="management-portal-tasks"></a>Správa portálu úkoly
1. Přihlaste se k [portálu Správa](https://manage.windowsazure.com).

2. Klikněte na **Obnovení služby**a potom klikněte na název záložního trezoru zobrazíte stránku rychlý Start.

    ![Obnovení služby](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Můžete změnit výběrem možností v horní části stránky rychlý Start, zobrazí se úlohy správy k dispozici.

![Správa karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Řídicí panel
Vyberte **řídicího panelu** zobrazíte přehled použití serveru. **Přehled použití** obsahuje:

- Počet servery Windows registrovaných do cloudu
- Počet Azure virtuálních počítačích chráněné v cloudu
- Celková velikost úložiště spotřebované množství v Azure
- Stav poslední úloh

V dolní části na řídicí panel můžete provádět následující úkoly:

- **Správa certifikátů** – Pokud certifikát byla použita k registraci serveru a pak slouží k aktualizaci certifikát. Pokud používáte trezoru přihlašovací údaje, nepoužívejte **Spravovat certifikát**.
- **Odstranění** - odstraní aktuální záložní trezoru. Pokud záložní trezoru už používá, můžete odstranit a uvolnit tak úložného prostoru. **Odstranění** povolen pouze po odstranění všech registrovaných serverech z trezoru.

![Zálohování řídicího panelu úkolů](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrovaná položek
Vyberte **Registrované položky** zobrazit názvy serverů, které jsou registrované trezoru.

![Registrovaná položek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**Typ** filtru výchozí hodnoty pro Azure virtuálního počítače. Pokud chcete zobrazit názvy serverů, které jsou registrované trezoru, vyberte **systému Windows server** z rozevírací nabídky.

Tady můžete provádět následující úkoly:

- **Povolit nové registrace** - při zvolení této možnosti serveru můžete **Průvodce registrací** ve službě Microsoft Azure záložní agent místní registrace serveru s záložní trezoru podruhé. Možná budete muset znovu zaregistrujte kvůli chybě v certifikátu nebo pokud serveru musel znovu sestaví.
- **Odstranění** - odstraní serveru ze záložní trezoru. Uložená data spojená se serverem je odstraněny všechny okamžitě.

    ![Úkoly registrované položky](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Chráněný položek
Vyberte **Chráněné položky** zobrazit položky, které byly zálohovala ze serverů.

![Chráněný položek](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurace

Na kartě **Konfigurovat** můžete vybrat možnost redundance odpovídající úložiště. Nejvhodnější doba k vyberte možnost redundance úložiště je nejlepší po vytvoření trezoru a před všech počítačích jsou registrované k němu.

>[AZURE.WARNING] Po registrované položky do trezoru, možnost redundance úložiště je uzamčený a nelze změnit.

![Konfigurace](./media/backup-azure-manage-windows-server-classic/configure.png)

V tomto článku najdete další informace o [redundance úložiště](../storage/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Backup agent úkoly

### <a name="console"></a>Konzoly

Otevřete **Microsoft Azure Backup agent** (najdete ho vyhledáním počítači *Microsoft Azure Backup*).

![Zálohování agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Pomocí **Akce** dostupné na pravé straně konzole záložní agent je možné provádět následující úkoly správy:

- Registrace serveru
- Plán zálohování
- Obecnějším údajům
- Změna vlastností

![Agent konzoly akce](./media/backup-azure-manage-windows-server-classic/console-actions.png)

>[AZURE.NOTE] **Obnovení dat**najdete v článku [Obnovení souborů systému Windows server nebo Windows klientský počítač](backup-azure-restore-windows-server.md).

### <a name="modify-an-existing-backup"></a>Upravit existující zálohu

1. Ve službě Microsoft Azure Backup agent klikněte na **Naplánovat zálohu**.

    ![Naplánování zálohy systému Windows Server](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

2. V **Plánu zálohováním** ponechte vybranou možnost **změny zálohování položek nebo časy** a klikněte na tlačítko **Další**.

    ![Úprava naplánované zálohování](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

3. Pokud chcete přidat nebo změnit položky v dialogovém okně **Vybrat položky zálohování** , klikněte na **Přidat položky**.

    Můžete taky nastavení **Vyloučení** z této stránky v průvodci. Pokud chcete vyloučit soubory nebo typy souborů, přečtěte si postup pro přidání [vyloučení nastavení](#exclusion-settings).

4. Vyberte soubory a složky, které chcete zálohovat a klikněte na **OK**.

    ![Přidání položek](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)

5. Určete **záložní plán** a klikněte na tlačítko **Další**.

    Můžou plánovat denně (na maximálně 3krát denně) nebo týdenní zálohy.

    ![Zadejte svůj časový plán zálohování](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

    >[AZURE.NOTE] Podrobně v tomto [článku](backup-azure-backup-cloud-as-tape.md)je vysvětleno určující plán zálohování.

6. Výběr **Zásad uchovávání informací** pro záložní kopie a klikněte na tlačítko **Další**.

    ![Vyberte zásady uchovávání informací](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)

7. Na obrazovce pro **potvrzení** zkontrolujte informace a klepněte na tlačítko **Dokončit**.

8. Po dokončení Průvodce vytvořením **plánu zálohování**, klepněte na tlačítko **Zavřít**.

    Po úpravě ochranu, kde můžete potvrdit, že zálohování způsobují správně tak, že přejdete na kartu **úlohy** a potvrzení, že se změny projevily ve úlohy zálohování.

### <a name="enable-network-throttling"></a>Povolit omezení sítě  
Agent Azure zálohování poskytuje Throttling kartou, která umožňuje určit, jak se používá šířka pásma během převodu dat. Tento ovládací prvek může být užitečné v případě potřeby obecnějším údajům dat během pracovní dobu, ale nechcete, aby při zálohování rušit jiných internetový provoz. Omezení dat přenos platí pro zálohování a obnovení aktivity.  

Chcete-li povolit omezení:

1. **Zálohování agent**klikněte na **Změnit vlastnosti**.

2. Zaškrtněte políčko **Povolit využití šířky pásma Internetu omezení pro zálohování** .

    ![Omezení sítě](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)

3. Po povolení omezení zadejte povolené šířku pásma pro přenos zálohování dat během **pracovní doby** a **nejsou pracovní doby**.

    Hodnoty šířky pásma začínala 512 kB sekundu (kb/s) a můžete přejít až 1023 megabajtů (MB /). Můžete taky určit zahájení a dokončení **pracovní doby**a které dny v týdnu jsou považovány za pracovních dnů. Čas mimo určený počet hodin práce se považuje za není pracovní doby.

4. Klikněte na **OK**.

## <a name="exclusion-settings"></a>Nastavení vyloučených položek

1. Otevřete **Microsoft Azure Backup agent** (najdete ho vyhledáním počítači *Microsoft Azure Backup*).

    ![Otevřít záložní agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

2. Ve službě Microsoft Azure Backup agent klikněte na **Naplánovat zálohu**.

    ![Naplánování zálohy systému Windows Server](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

3. V Průvodci záložní plánu ponechte vybranou možnost **změny záložní položky nebo časy** a klikněte na tlačítko **Další**.

    ![Změnit plán](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

4. Klikněte na **vyloučení nastavení**.

    ![Vyberte položky, aby se vyloučila](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)

5. Klikněte na **Přidat vyloučených položek**.

    ![Přidání vyloučení](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)

6. Vyberte umístění a potom klikněte na **OK**.

    ![Vyberte umístění pro vyloučených položek](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)

7. Přidání přípona souboru v poli **Typ souboru** .

    ![Vyloučení podle typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Přidání příponu MP3

    ![Příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Pokud chcete přidat jinou příponu, klikněte na možnost **Přidat vyloučení** a zadejte jiný soubor s příponou (Přidání rozšíření JPEG).

    ![Jiný příklad typ souboru](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)

8. Po přidání všech rozšíření, klikněte na **OK**.

9. Pokračujte v Průvodci zálohování plánu po kliknutí na **Další** až ke **stránce pro potvrzení**klikněte na **Dokončit**.

    ![Potvrzení vyloučených položek](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Další kroky
- [Obnovení systému Windows Server nebo klienta Windows z Azure](backup-azure-restore-windows-server.md)
- Další informace o Azure zálohování najdete v tématu [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)
- Navštivte [Fórum komunity Azure zálohování](http://go.microsoft.com/fwlink/p/?LinkId=290933)
