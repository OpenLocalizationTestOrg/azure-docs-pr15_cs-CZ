<properties 
    pageTitle="Obnovení aplikace v Azure" 
    description="Informace o obnovení aplikace ze zálohy." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Obnovení aplikace v Azure

Tento článek popisuje způsob obnovení aplikace v [Aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md) zazálohovanou jste dříve (viz [zpátky do vyšší vaši aplikaci distribuovali Azure](web-sites-backup.md)). Obnovení aplikace s jeho propojené databáze (databáze SQL nebo MySQL) na vyžádání předchozího stavu nebo vytvořte nové aplikace na základě jedné ze zálohy původní aplikace. Vytvoření nové aplikace, které se spouští paralelně na nejnovější verzi může být užitečné pro A a B testování.

Obnovení ze zálohy je k dispozici aplikace spuštěné v **Standardní** a **Premium** osy. Informace o škálování aplikace najdete v tématu [škálování aplikace v Azure](web-sites-scale.md). Vrstvy **Premium** umožňuje větší počet denního zálohování provádět než **Standardní** osy.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Obnovení aplikace z existující zálohu

1. Na zásuvné **Nastavení** aplikace v portálu Azure klepněte na **zálohování** zobrazíte zásuvné **zálohy** . Klepněte na tlačítko **Obnovit** na panelu příkazů. 
    
    ![Zvolte obnovit][ChooseRestoreNow]

3. V zásuvné **obnovení** nejdřív vyberte záložní zdroj výsledků. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    Možnosti **Zálohování aplikace** zobrazuje všechny existující zálohy aktuální aplikace a můžete snadno vybírat. 
    Možnosti **ukládání** umožňuje vybrat všechny záložní soubor ZIP z existující účet Azure úložiště a kontejneru ve vašem předplatném. 
    Pokud se pokoušíte obnovení záložní kopii jiné aplikace, použijte možnost **úložiště** .

4. Zadejte cíl pro obnovení aplikace v poli **obnovit cíl**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Pokud se rozhodnete **Přepsat**, se vymažou všechny existující data v aktuálním aplikace. Před kliknutím na **OK**, ujistěte se, že je přesně co chcete udělat.
    
    Můžete vybrat **Existující aplikace** obnovit zálohování aplikace do jiné aplikace ve stejné skupině provedena. Než použijete tuto možnost, měli jste již vytvořili jiné aplikace ve skupině zdroje s odrážející strukturu konfigurace databáze pro definovaná v aplikaci zálohování. 
    
5. Klikněte na **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Stažení a odstranění zálohy z účtu úložiště
    
1. V hlavním **Procházet** zásuvné portálu Azure vyberte **účty úložiště**.
    
    Zobrazí se seznam účtů existující úložiště. 
    
2. Vyberte účet úložiště, který obsahuje zálohy, kterou chcete stáhnout nebo odstranit.
    
    Zobrazí se zásuvné účtu úložiště.

3. V zásuvné accountn úložiště vyberte kontejner, který chcete
    
    ![Kontejnery zobrazení][ViewContainers]

4. Vyberte záložní soubor, který chcete stáhnout nebo odstranit.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klikněte na **Stáhnout** nebo **Odstranění** podle toho, co chcete udělat.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Sledování obnovení
    
1. Pokud chcete zobrazit podrobnosti o úspěšném nebo neúspěšném řešení operace obnovení aplikace, přejděte na zásuvné **Protokolu auditování** Azure portálu. 
    
    Zásuvné **protokolů auditování** zobrazí všechny operace, spolu s úroveň, stav, zdroje a čas-podrobnosti.
    
2. Zobrazení pomocí posuvníku dolů požadované obnovení a kliknutím ho vyberte.

Podrobnosti o zásuvné zobrazí dostupné informace o obnovení.
    
## <a name="next-steps"></a>Další kroky

Můžete taky zálohování a obnovení aplikace služeb aplikací pomocí rozhraní REST API (viz [ZBÝVAJÍCÍ použití zálohování a obnovení aplikace aplikaci služby](websites-csm-backup.md)).

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
