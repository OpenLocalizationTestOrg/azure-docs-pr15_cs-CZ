<properties
    pageTitle="Přenést databáze serveru SQL Server na serveru SQL Server v virtuálního počítače | Microsoft Azure"
    description="Informace o tom, jak přenést databázi místního uživatele serveru SQL Server v Azure virtuálního počítače."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Přenést databázi serveru SQL Server SQL Server Azure VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Správce prostředků modelu.


Existuje několik metod pro migraci místního uživatele databáze aplikace SQL Server Azure VM. Tento článek stručně bude diskutovat o různých metodách, doporučit nejlepší metody pro různé scénáře a zahrnují [kurz](#azure-vm-deployment-wizard-tutorial) pro vás pomocí průvodce **nasazení databáze SQL Server Microsoft Azure VM** . 

Metoda pomocí průvodce **nasazení databáze SQL Server Microsoft Azure VM** podle [kurzu](#azure-vm-deployment-wizard-tutorial) je pouze klasické nasazení modelu. 

## <a name="what-are-the-primary-migration-methods"></a>Jaké jsou metody migrace?

Metody migrace jsou:

- Použití nasazení databáze SQL Server Průvodce Microsoft Azure VM
- Místní zálohování pomocí komprese a ručně zkopírujte záložní soubor do virtuálního počítače Azure
- Provedení zálohování na adresu URL a obnovení v Azure virtuálního počítače z adresy URL
- Odpojte a pak zkopírujete soubory dat a protokolů do úložiště objektů blob Azure a potom připojit k serveru SQL Azure VM z adresy URL
- Převést místního fyzického počítače na technologie Hyper-V virtuální pevný disk, odeslat do úložiště objektů Blob Azure a pak nasadit jako nový VM pomocí uložit VHD
- Dodat pevný disk pomocí systému Windows služby Import/Export
- Pokud máte nasazení technologie AlwaysOn prostory, použijte [Průvodce Azure repliky](virtual-machines-windows-classic-sql-onprem-availability.md) vytvořit repliku v Azure a převzetí služeb při selhání, přejdete instance Azure databáze uživatelů
- SQL Server [transakční replikace](https://msdn.microsoft.com/library/ms151176.aspx) umožňuje konfigurovat instanci serveru SQL Azure jako předplatitel a potom zakažte replikaci, přejdete instance Azure databáze uživatelů



## <a name="choosing-your-migration-method"></a>Volba způsobu migrace

Pro optimální datový přenos výkonu migrace databázové soubory do Azure VM, záložního souboru komprimovaný je obecně nejlepší způsob. Toto je metoda, která používá [databázi SQL Server Microsoft Azure VM Průvodce nasazení](#azure-vm-deployment-wizard-tutorial) . Tento průvodce je doporučená metoda pro migraci spuštění databáze místního uživatele na serveru SQL Server 2005 nebo vyšší pro SQL Server 2014 nebo vyšší, pokud záložní soubor komprimované databáze je menší než 1 TB.

Chcete-li minimalizovat výpadek během procesu migrace databáze, použijte možnost AlwaysOn nebo transakční replikace.

Pokud není možné použít výše uvedené metody, ručně přeneste databázi. Pomocí této metody, obvykle začnete s následuje kopii zálohy databáze databáze zálohování do Azure a potom provést obnovení databáze. Můžete také zkopírovat soubory databáze samotné do Azure a připojte je. Zde několik metod podle kterých lze provádět tento ruční proces migrace databáze do Azure VM.

> [AZURE.NOTE] Při upgradu na SQL Server 2014 nebo SQL Server 2016 ze starší verze serveru SQL Server, měli byste zvážit, zda je nutné provést změny. Doporučujeme adresy všechny závislosti na funkce nové verze serveru SQL Server nejsou podporovány jako součást migrace projektu. Další informace o podporovaných vydáních a scénáře naleznete v tématu [Upgrade na SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

V následující tabulce jsou uvedeny jednotlivé metody migrace a popisuje, kdy je nejvhodnější použití každé metody.

| Metoda  | Verze databáze zdroje  |  Určení verze databáze | Omezení velikosti zálohy databáze zdroj  | Poznámky  |
|---|---|---|---|---|
| [Použití nasazení databáze SQL Server Průvodce Microsoft Azure VM](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 nebo vyšší | SQL Server 2014 nebo vyšší | < 1 TB  | Nejrychlejší a nejjednodušší metodu použít, kdykoli je to možné migrovat do nové nebo existující instanci serveru SQL Server v Azure virtuálního počítače | 
| [Použití přidat repliku Azure Průvodce](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 nebo vyšší | SQL Server 2012 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Minimalizuje prostoje, používá se, když máte místním nasazením AlwaysOn |
| [Použití transakční replikace serveru SQL Server](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 nebo vyšší | SQL Server 2005 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Použijte pokud potřebujete minimalizovat prostoje a nemají místním nasazením AlwaysOn |
| [Místní zálohování pomocí komprese a ručně zkopírujte záložní soubor do virtuálního počítače Azure](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 nebo vyšší | SQL Server 2005 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Použijte pouze pokud nelze použít průvodce, například při cílové verze databáze je menší než SQL Server 2012 SP1 CU2 nebo velikosti zálohy databáze je větší než 1 TB (12,8 TB s SQL Server 2016) |
| [Provedení zálohování na adresu URL a obnovení v Azure virtuálního počítače z adresy URL](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 nebo vyšší | SQL Server 2012 SP1 CU2 nebo vyšší | < 12,8 TB pro SQL Server 2016, jinak < 1 TB | Obecně použití [zálohování na adresu URL](https://msdn.microsoft.com/library/dn435916.aspx) je ekvivalent ve výkonu pomocí průvodce a není zcela snadné |
| [Odpojte a pak zkopírujete soubory dat a protokolů do úložiště objektů blob Azure a potom připojit k serveru SQL Server v Azure virtuálního počítače z adresy URL](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 nebo vyšší | SQL Server 2014 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Tuto metodu použijte při plánování pro [ukládání těchto souborů pomocí služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/dn385720.aspx) a připojit je k serveru SQL Server spuštěna v modulu VM Azure, zvláště u velkých databází |
| [Převést na VHD Hyper-V místním počítači, odeslat do úložiště objektů Blob Azure a pak nasazení nového virtuálního počítače pomocí nahraného disku VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 nebo vyšší | SQL Server 2005 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Používá se při [uvedení licence serveru SQL Server](../sql-database/sql-database-paas-vs-sql-server-iaas.md), při přenesení databáze, která bude spuštěna na starší verzi serveru SQL Server nebo při přenášení systémové a uživatelské databáze společně jako součást přenesení databáze, které jsou závislé na jiných uživatelských databází a databází systému. |
| [Dodat pevný disk pomocí systému Windows služby Import/Export](#ship-hard-drive) | SQL Server 2005 nebo vyšší | SQL Server 2005 nebo vyšší | [Omezení úložiště Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Pomocí [Služby Import/Export Windows](../storage/storage-import-export-service.md) při ruční kopírování metoda je příliš pomalé, jako u velkých databází |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Výukový program Průvodce nasazení Azure VM

Přenést SQL Server 2005 pomocí průvodce **nasazení databáze SQL Server Microsoft Azure VM** v Microsoft SQL Server Management Studio, SQL Server 2008, SQL Server 2008 R2, SQL Server 2012, SQL Server 2014 nebo SQL Server 2016 místního uživatele databáze (až 1 TB) na SQL Server 2014 nebo SQL Server 2016 v Azure virtuálního počítače. Pomocí tohoto průvodce můžete přenést uživatele databáze existující Azure virtuálního počítače nebo Azure VM se serverem SQL Server vytvořené průvodcem během procesu migrace. Pokud přenášíte databázi na novější verzi serveru SQL Server, tato databáze upgradována automaticky během procesu.

Tato metoda je pouze klasické nasazení modelu. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Získat nejnovější verzi aplikace nasazení databáze SQL Server Průvodce Microsoft Azure VM

Použijte nejnovější verzi Microsoft SQL Server Management Studio pro SQL Server k zajištění, že máte nejnovější verzi Průvodce **nasazení databáze SQL Server Microsoft Azure VM** . Nejnovější verzi Průvodce zahrnuje nejnovější aktualizace k portálu Azure klasické a podporuje nejnovější Azure VM obrázky v galerii (starší verze Průvodce nemusí fungovat). Chcete-li získat nejnovější verzi Microsoft SQL Server Management Studio pro SQL Server, [stáhněte jej](http://go.microsoft.com/fwlink/?LinkId=616025) a nainstalujte v klientském počítači s připojením k databázi plánování migrace a k Internetu.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Konfigurovat existující virtuální počítač Azure a instance serveru SQL Server (pokud existuje)

Pokud přenášíte existující Azure VM, jsou požadovány následující kroky konfigurace:

- Konfigurace Azure VM a instance serveru SQL Server Chcete-li povolit připojení z jiného počítače pomocí následujícího postupu připojení [k instanci SQL serveru VM z SSMS v jiném počítači](virtual-machines-windows-sql-connect.md). Pouze SQL Server 2014 a SQL Server 2016 obrazy v galerii jsou podporovány při migraci pomocí průvodce.
- Na brány Microsoft Azure s privátní port 11435 konfigurujte otevřít koncový bod pro službu SQL Server Cloud adaptér. Tento port je vytvořen jako součást SQL Server 2014 nebo SQL Server 2016 dotační na Microsoft Azure VM. Adaptér Cloud také vytvoří pravidlo brány Windows Firewall povolit jeho příchozí připojení TCP na výchozí port 11435. Tento koncový bod umožňuje Průvodce Chcete-li využít službu Cloud adaptér zkopírujte záložní soubory z místní instance Azure VM. Další informace naleznete v tématu [Cloud adaptér, pro SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Vytvoření koncového bodu adaptéru cloudu](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Použití spustit nasazení databáze SQL Server Průvodce Microsoft Azure VM

1. Otevřete 2016 Microsoft SQL Server Microsoft SQL Server Management Studio a připojit k instanci serveru SQL Server obsahující databázi uživatelů, která bude migrace Azure VM.
2. Klepněte pravým tlačítkem myši databáze, který migrujete, přejděte na položku úkoly a potom klepněte na tlačítko nasazení Microsoft Azure VM.

    ![Spustit Průvodce](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Na stránce Úvod klepněte na tlačítko Další.
4. Na stránce nastavení zdroj připojení k instanci serveru SQL Server obsahující databázi, která chcete migrovat Azure VM.
5. Určete dočasné umístění pro záložní soubory. Pokud připojení ke vzdálenému serveru, je nutné zadat síťovou jednotku.

    ![Nastavení zdrojů](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Klepněte na tlačítko Další.
7. Na stránce Microsoft Azure přihlášení klepněte na tlačítko přihlášení a přihlásit se ke svému účtu Azure.
8. Vyberte předplatné, které chcete použít a klepněte na tlačítko Další.

    ![Azure přihlášení](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Na stránce nastavení nasazení, můžete zadat novou nebo existující službu cloudu název a název virtuálního počítače:

 - Zadejte nový název služby Cloud a Virtual Machine název k vytvoření nových Cloudová služba s novou Azure virtuálního počítače pomocí bitové kopie SQL Server 2014 nebo SQL Server 2016 galerie.

     - Pokud zadáte nový název služby Cloud, zadejte účet úložiště, který budete používat.

     - Pokud zadáte název existující cloudové služby, bude načtena a automaticky účet úložiště.

 - Zadejte název existující cloudové služby a nový název virtuálního počítače, chcete-li vytvořit nový virtuální počítač Azure v existující službu cloudu. Zadejte pouze bitové kopie SQL Server 2014 nebo SQL Server 2016 galerie.
 - Zadejte název existující cloudové služby a název virtuálního počítače, chcete-li použít existující Azure virtuálního počítače. Musí to bitové kopie pomocí bitové kopie SQL Server 2014 nebo SQL Server 2016 galerie.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Klepněte na tlačítko Nastavení
  - Pokud jste zadali existující název cloudové služby a virtuální počítač, se výzva k zadání uživatelského jména a hesla.

        ![Nastavení počítače Azure](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Pokud jste zadali název nového virtuálního počítače, budete vyzváni k vyberte obrázek ze seznamu galerie obrázků a zadejte následující informace:
      - Image – vyberte pouze SQL Server 2014 nebo SQL Server 2016
        - Uživatelské jméno
        - Nové heslo
        - Potvrzení hesla
        - Umístění
        - Velikost.
    - Navíc klikněte na tlačítko Přijmout generovaným certifikátem pro tento nový Microsoft Azure virtuálního počítače a potom klepněte na tlačítko OK.

    ![Azure nové nastavení počítače](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Zadejte název cílové databáze, pokud se liší od název zdrojové databáze. Pokud cílová databáze již existuje, systém bude automaticky zvýší hodnotu název databáze namísto přepsat existující databázi.
12. Klepněte na tlačítko Další a potom klepněte na tlačítko Dokončit.

    ![Výsledky](./media/virtual-machines-windows-migrate-sql/results.png)

13. Po dokončení průvodce připojit k virtuálnímu počítači a ověřte, že databáze byla migrována.
14. Pokud jste vytvořili nový virtuální počítač, konfigurace Azure virtuálního počítače a instance serveru SQL Server pomocí následujícího postupu připojení [k instanci SQL serveru VM z SSMS v jiném počítači](virtual-machines-windows-sql-connect.md).

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Zálohování souborů a kopírování VM a obnovení

Tuto metodu použijte, pokud nelze použít k nasazení databáze SQL Server Microsoft Azure VM Průvodce buď protože při migraci na verzi serveru SQL Server před SQL Server 2014 nebo záložního souboru je větší než 1 TB. Pokud záložní soubor je větší než 1 TB, musí prokládat protože maximální velikost VM disku je 1 TB. Přenést uživatele databáze pomocí této metody Ruční pomocí následujících obecných kroků:

1.  Proveďte úplnou zálohu databáze do místního umístění.
2.  Vytvořit nebo virtuálního počítače s verzí systému SQL Server, které chcete odeslat.
3.  Nastavení připojení podle svých požadavků. Viz [připojit k virtuálnímu počítači SQL serveru na Azure (Správce prostředků)](virtual-machines-windows-sql-connect.md).
4.  Zkopírujte záložní soubory v modulu VM pomocí vzdálené plochy, Průzkumníka Windows nebo příkazu copy na příkazovém řádku.

## <a name="backup-to-url-and-restore"></a>Zálohování a obnovení adresy URL

Použijte metodu [zálohování na adresu URL](https://msdn.microsoft.com/library/dn435916.aspx) při nasazení databáze SQL Server Microsoft Azure VM Průvodce nelze použít, protože záložní soubor je větší než 1 TB a při migraci z a do SQL serveru 2016. Pro databáze je menší než 1 TB nebo s verzí serveru SQL Server před SQL Server 2016 se doporučuje použít průvodce. SQL Server 2016 prokládané zálohovací sklady jsou podporovány, jsou vhodné pro výkon a přesáhnout omezení velikosti na blob. Velmi rozsáhlé databáze doporučujeme použití [Služby Import/Export Windows](../storage/storage-import-export-service.md) .

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Odpojit a zkopírujte adresu URL z adresy URL připojení

Tuto metodu použijte při plánování pro [ukládání těchto souborů pomocí služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/dn385720.aspx) a připojit je k serveru SQL Server spuštěna v modulu VM Azure, zvláště u velkých databází. Přenést uživatele databáze pomocí této metody Ruční pomocí následujících obecných kroků:

1.  Odpojení souborů databáze z instance místní databáze.
2.  Zkopírujte soubory odpojenou databázi do úložiště objektů blob Azure pomocí [Nástroje příkazového řádku AZCopy](../storage/storage-use-azcopy.md).
3.  Soubory databáze v Azure URL připojte k instanci serveru SQL Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Převést na VM a odešlete na adresu URL a nasadit jako nový VM

Tímto způsobem můžete přenést všechny systémové a uživatelské databáze v instanci serveru SQL Server v prostorách Azure virtuálního počítače. Umožňuje přenést celou instance SQL Server metodou ruční podle následujících obecných kroků:

1.  Převeďte fyzické nebo virtuální počítače Hyper-V VHD pomocí [Microsoft Virtual Machine Converter](http://technet.microsoft.com/library/dn873998.aspx).
2.  Odešle soubory VHD do úložiště Azure pomocí [rutiny přidat AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Nasazení nového virtuálního počítače pomocí nahraného disku VHD.

> [AZURE.NOTE] Chcete-li přenést celou aplikaci, zvažte použití [Azure obnovení webu](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Pevný disk příjemce

Použijte [metodu importu a exportu služby systému Windows](../storage/storage-import-export-service.md) k přenosu velkého objemu dat souborů do úložiště objektů Blob Azure v situacích, kdy odesílání v síti není proveditelné nebo výtažkovými. S touto službou je odeslat jeden nebo více pevných disků obsahující tato data Azure data Center, kde data nahraje k vašemu účtu úložiště.

## <a name="next-steps"></a>Další kroky

Další informace o spuštění serveru SQL Server na platformě Azure virtuálních počítačů naleznete v tématu [SQL Server na virtuálních počítačích Azure – přehled](virtual-machines-windows-sql-server-iaas-overview.md).

Pokyny k vytvoření virtuálního počítače Azure SQL Server z zachycený obrázek viz [Tipy & triky na klonování virtuálních počítačů Azure SQL z obrazů zachycených](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) v CSS SQL Server inženýři blogu.
