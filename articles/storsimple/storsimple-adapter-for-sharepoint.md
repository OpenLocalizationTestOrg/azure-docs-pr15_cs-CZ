<properties 
   pageTitle="StorSimple adaptér pro službu SharePoint | Microsoft Azure"
   description="Popisuje, jak nainstalovat a nakonfigurovat nebo odeberte StorSimple pro SharePoint v serverové farmě SharePoint."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Nainstalujte a nakonfigurujte adaptér StorSimple pro službu SharePoint

## <a name="overview"></a>Základní informace

Adaptér StorSimple pro službu SharePoint je součást, který umožňuje poskytovat Microsoft Azure StorSimple flexibilní úložiště a ochrana dat na SharePoint serverové farmy. Můžete adaptér přesunout obsah binární velké objektu (BLOB) z obsahu databáze systému SQL Server Microsoft Azure StorSimple hybridní cloudu úložné zařízení.

Adaptér StorSimple pro službu SharePoint funguje jako poskytovatele služby vzdáleného úložiště objektů BLOB (RBS) a používá funkci vzdáleného úložiště objektů BLOB SQL serveru k ukládání nestrukturovaná obsahu služby SharePoint (formou objektů BLOB) na souborovém serveru, který je podporovaným StorSimple zařízení.

>[AZURE.NOTE] Karta StorSimple pro službu SharePoint podporuje SharePoint Server 2010 vzdálené objektů BLOB úložiště (kód RBS). Nepodporuje SharePoint Server 2010 externí objektů BLOB úložiště (produktu EBS).

- Pokud si Pokud chcete stáhnout adaptér StorSimple pro SharePoint, přejděte na [StorSimple adaptér pro službu SharePoint] [ 1] na webu Microsoft Download Center.

- Informace o plánování kód RBS a kód RBS omezení, přejděte na [rozhodnutí použít kód RBS v Sharepointu 2013] [ 2] nebo [plánování kód RBS (SharePoint Server 2010)][3].

Zbytek přehled stručně popisuje role adaptér StorSimple pro SharePoint a SharePoint výkonu a kapacity omezení, které je třeba vědět před instalací a nakonfigurujte adaptér. Když si prohlédnete tyto informace, přejděte na [StorSimple adaptér pro instalaci](#storsimple-adapter-for-sharepoint-installation) zahájíte nastavení adaptér.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple adaptér výhody služby SharePoint

Na webu služby SharePoint je uložen obsah jako nestrukturovaná objektů BLOB data v jedné nebo více databází obsahu. Ve výchozím nastavení těchto databází hostiteli jsou počítačů, které se serverem SQL Server a jsou umístěny v serverové farmě SharePoint. Objekty BLOB můžete rychle zvětšit, náročné místní úložiště. Z tohoto důvodu můžete najít řešení jiné, levnější úložiště. SQL Server obsahuje technologii nazvanou vzdálené objektů Blob úložiště (RBS), který umožňuje ukládat obsah objektů BLOB v systému souborů, mimo databáze SQL serveru. Pole Kód RBS objekty BLOB mohou být umístěny v systému souborů na počítači, na kterém běží SQL Server nebo mohou být uloženy v systému souborů na jiném počítači serveru.

Pole Kód RBS vyžaduje, aby používal poskytovatele kód RBS, například adaptér StorSimple pro SharePoint, aby pole Kód RBS ve službě SharePoint. Adaptér StorSimple pro službu SharePoint spolupracuje pole Kód RBS, takže můžete přesunout objekty BLOB na server zálohovala systémem Microsoft Azure StorSimple. Microsoft Azure StorSimple uloží objektů BLOB data místně nebo v cloudu, založené na použití. Objekty BLOB, které jsou velmi aktivní (obvykle označovány jako úroveň 1 nebo žádanou dat) uloženy místně. Méně aktivní data a archivace data uložena v cloudu. Po povolení kód RBS na databázi obsahu je uložen každý nový obsah objektů BLOB vytvořené ve službě SharePoint na zařízení StorSimple a ne v databázi obsahu.

Microsoft Azure StorSimple provádění kód RBS obsahuje následující výhody:

- Přesunutí objektů BLOB obsahu na samostatný server můžete snížit načítání dotazu na serveru SQL, která lze zlepšit rychlostí reakce serveru SQL Server. 

- Azure StorSimple používá deduplication a komprese zmenšit velikost datového.

- Azure StorSimple poskytuje ochranu dat ve formě místního a cloudu snímky. Také pokud umístíte samotné databáze na StorSimple zařízení, můžete zálohovat databázi obsahu a objekty BLOB společně v havárie jednotný způsob. (Přesunutí databáze obsahu na zařízení je podporována pouze pro zařízení StorSimple 8000 řady. Tato funkce není podporována pro řadu 5000 nebo 7000.)

- Azure StorSimple obsahuje havárie obnovení funkcí včetně překlopení, obnovení souborů a objem (včetně zkušební obnovení) a rychlého obnovení dat.

- Software obnovení dat, například Kroll Ontrack PowerControls, můžete pomocí StorSimple snímky objektů BLOB dat k obnovení úrovně položky obsahu služby SharePoint. (Tento software obnovení dat je samostatného nákupu.)

- Adaptér StorSimple pro službu SharePoint připojí na portál Centrální správa SharePoint umožňuje spravovat celé řešení SharePoint z centrálního umístění.

Přesunutí objektů BLOB obsahu k systému souborů můžete poskytnout jiných úsporu nákladů a výhod. Například pomocí pole Kód RBS můžete omezit potřebu drahé úložiště úrovně 1 a, protože zmenší obsahu databáze, pole Kód RBS můžete omezit počet databází v serverové farmě SharePoint vyžaduje. Však jiných faktorů, jako je limity velikosti databáze a množství obsahu kód RBS, může mít vliv na požadavky na úložiště. Další informace o nákladech a výhody používání pole Kód RBS najdete v článku [plánování kód RBS (SharePoint Foundation 2010)] [ 4] a [rozhodnutí použít kód RBS v Sharepointu 2013][5].

### <a name="capacity-and-performance-limits"></a>Limity výkonu a kapacity

Než budete užívat kód RBS v řešení Sharepointu, byste měli znát testované výkonu a kapacity omezení SharePoint Server 2010 a SharePoint serveru 2013 a vztah tyto limity a přijatelného výkonu. Další informace najdete v tématu [softwarová omezení a limity pro SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Před konfigurace kód RBS, zkontrolujte následující:

- Ujistěte se, aby celkovou velikost obsahu (velikost databáze obsahu) plus velikost všechny přidružené externalized objektů BLOB nepřekročila limit velikosti pole Kód RBS nepodporuje SharePoint. Toto omezení je 200 GB. 

    **Míra obsahu databáze a velikost objektů BLOB**

     1. Spusťte dotaz v Centrální správy WFE. Spusťte prostředí Management Shell služby SharePoint a zadejte tento příkaz prostředí Windows PowerShell získat velikost databáze obsahu:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Tento krok získá velikost databáze obsahu na disku.

     2. Spuštění následující dotaz SQL v SQL Management Studiuo z pole SQL server na každou databázi obsahu a přidání výsledek k číslo získané v kroku 1.

        Na SharePoint 2013 databází obsahu zadejte:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Na databází obsahu Sharepointu 2010 zadejte:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Tento krok získá velikost objektů BLOB, které byly externalized.

- Doporučujeme ukládání všech objektů BLOB a databáze obsahu místně na StorSimple zařízení. Zařízení StorSimple se dvěma uzly clusteru vysoké dostupnosti. Umístění databáze obsahu a objekty BLOB na zařízení StorSimple zvyšuje dostupnost.

    Používejte tradiční SQL Server migrace doporučené postupy přesuňte databází obsahu do zařízení StorSimple. Přesunutí databáze až po veškerý obsah objektů BLOB z databáze byla přesunutá do sdílené složky pomocí pole Kód RBS. Pokud budete chtít přesunout databázi obsahu do zařízení StorSimple, doporučujeme konfigurace databáze obsahu úložiště v zařízení jako primární hlasitost.

- V aplikaci Microsoft Azure StorSimple, pokud pomocí vrstvené svazky, nejde žádným způsobem zajistit, že obsah uložená místně na StorSimple zařízení nebude vrstveny na Microsoft Azure cloudového úložiště. Proto doporučujeme používat StorSimple místně připnuté svazky ve spojení s SharePoint RBS. Zajistíte, že veškerý obsah objektů BLOB zůstane místně na zařízení StorSimple a není přesune do Microsoft Azure.

- Pokud není uložíte databází obsahu v zařízení StorSimple, použijte tradiční SQL Server dostupnost doporučené postupy, které podporují kód RBS. SQL Server clusterů podporuje kód RBS, při SQL Server odrážející strukturu nemá. 

>[AZURE.WARNING] Pokud jste kód RBS nepovolili, ale nedoporučujeme přesunutí databázi obsahu do zařízení StorSimple. Toto je netestované konfigurace.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple adaptér pro instalaci služby SharePoint

Před instalací adaptér StorSimple pro službu SharePoint konfigurace StorSimple zařízení a ujistěte se, že serverové farmě SharePoint a pro vytvoření instance serveru SQL Server splňují všechny podmínky. Tento kurz popisuje požadavky na konfiguraci, jakož i postupy instalace a upgrade adaptér StorSimple pro službu SharePoint. 

## <a name="configure-prerequisites"></a>Konfigurace požadavky

Před instalací adaptér StorSimple pro SharePoint, ujistěte se, StorSimple zařízení, serverové farmě SharePoint a SQL Server pro vytvoření instance: splnění následujících podmínek.

### <a name="system-requirements"></a>Požadavky na systém

Adaptér StorSimple pro službu SharePoint spolupracuje následující hardware a software:

- Podporované operačního systému Windows Server 2008 R2 SP1, Windows Server 2012 nebo Windows Server 2012 R2 

- Podporované verze služby SharePoint – SharePoint Server 2010 nebo SharePoint serveru 2013

- Podporované verze serveru SQL Server – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition nebo SQL Server 2012 Enterprise Edition

- Podporovaná zařízení StorSimple – StorSimple 8000 řady, StorSimple 7000 řady nebo StorSimple 5000 řady.

### <a name="storsimple-device-configuration-prerequisites"></a>Zjistit předpoklady pro konfiguraci StorSimple zařízení

Zařízení StorSimple blok zařízení a jako takové vyžaduje souborovém serveru, na které můžete data hostovaná. Doporučujeme použít samostatný server spíše než existujícího serveru ze serverové farmě Sharepointu. Tento soubor server musí mít na stejném místní síti (LAN) jako SQL serveru, který je hostitelem databází obsahu. 

>[AZURE.TIP] 
>
>- Pokud jste nakonfigurovali vaší farmě Sharepointu pro dostupnost, by měl nasadíte souborový server pro dostupnost taky.
>
>- Pokud není uložíte databáze obsahu na zařízení StorSimple, použijte tradiční dostupnost doporučené postupy, které podporují kód RBS. SQL Server clusterů podporuje kód RBS, při SQL Server odrážející strukturu nemá. 

Ujistěte se, že zařízení StorSimple správně nakonfigurované a že odpovídající objemy podporuje nasazení Sharepointu nakonfigurované a přístupné z počítače serveru SQL Server. Pokud již dosud nasazeném a nakonfigurovali StorSimple zařízení přejděte do [zařízení StorSimple místního nasazení](storsimple-deployment-walkthrough.md) . Všimněte si IP adresu StorSimple zařízení. budete potřebovat jeho během StorSimple adaptér pro instalaci služby SharePoint. 

Navíc zkontrolujte, jestli má být použita pro externalization objektů BLOB splňuje následující požadavky:

- Hlasitost musí být naformátovaný tak velikost jednotky přidělení 64 KB.

- Web front-end (WFE) a servery aplikace musíte mít přístup k hlasitost prostřednictvím cestu (Universal Naming Convention). 

- Serverové farmě SharePoint musí být nakonfigurované pro zápis hlasitost.

>[AZURE.NOTE] Po instalaci a konfiguraci adaptér všech objektů BLOB externalization musí absolvovat zařízení StorSimple (zařízení bude prezentovat svazky SQL serveru a správa vrstev úložiště). Pro objektů BLOB externalization nelze použít jiné cíle.
 
Pokud nebudete chtít použít StorSimple snímek správce pro provádění různých snímky dat objektů BLOB a databáze, je potřeba nainstalovat StorSimple snímek správce na databázovém serveru tak, aby ho pomocí služby SQL Redaktor implementovat Windows hlasitost stín kopie služby (VSS). 

>[AZURE.IMPORTANT] Správci snímek StorSimple nepodporuje zápis VSS. SharePoint a nelze pořizovat konzistenci aplikací snímky dat služby SharePoint. Ve scénáři SharePoint StorSimple snímek správce poskytuje jenom pád konzistentní zálohy. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>Předpoklady konfigurace farmě služby SharePoint

Zkontrolujte, že farmě SharePoint serveru správně nakonfigurované, následujícím způsobem:

- Ověřte, zda je v pořádku stavu serverové farmě SharePoint a zkontrolujte tyto věci: 

- Všechny SharePoint WFE aplikační servery registrované ve farmě běží a lze provést příkaz ping ze serveru, na kterém budete instalovat adaptér StorSimple pro službu SharePoint.

- Na každém WFE serveru a aplikační server běží služba SharePoint Timer (SPTimerV3 nebo SPTimerV4).

- Služba časovače služby SharePoint a fondu aplikací služby IIS, pod kterým je spuštěný webu Centrální správa SharePoint oprávnění správce. 

- Ujistěte se, že je zakázané Internet Explorer rozšířeného naplánovaný (IE ESC). Tímto postupem zakázání IE ESC:

    1. Ukončete všechny instance aplikace Internet Explorer.

    2. Spusťte správce serveru.

    3. V levém podokně klikněte na **Místní Server**.

    4. V podokně vpravo vedle **Rozšířeného zabezpečení aplikace Internet Explorer**klikněte **na**.

    5. V části **Správce**klepněte na **vypnout**.

    6. Klikněte na **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Vzdálené požadavky na úložiště objektů BLOB (RBS)

Ujistěte se, že používáte podporovanou verzi serveru SQL Server. Podporované a používat pole Kód RBS jsou pouze následující verze:

- SQL Server 2008 Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- SQL Server 2012 Enterprise Edition

Objekty BLOB můžete externalized na jenom těch, které představuje StorSimple zařízení pro SQL Server. Žádné cíle pro objektů BLOB externalization jsou podporovány.

Po dokončení všech kroků základní konfigurace, přejděte na [instalace adaptér StorSimple pro službu SharePoint](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Nainstalujte adaptér StorSimple pro službu SharePoint

Pomocí následujících kroků instalace adaptér StorSimple pro službu SharePoint. Pokud jsou přeinstalovat software, přečtěte si článek [Upgrade nebo přeinstalovat adaptér StorSimple pro službu SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Dobu potřebnou pro instalaci závisí na celkový počet SharePoint databází ve farmě SharePoint serveru.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>Konfigurovat pole Kód RBS

Po instalaci adaptér StorSimple pro službu SharePoint nakonfigurujte kód RBS podle následujícího postupu.

>[AZURE.TIP] Adaptér StorSimple pro SharePoint připojí do stránky Centrální správa SharePoint povolení RBS se povolit nebo zakázat na každou databázi obsahu ve farmě služby SharePoint. Však povolení nebo zakázání kód RBS na databáze obsahu způsobí, že resetování služby IIS, které se v závislosti na konfiguraci farmy, může přerušit krátkodobě dostupnost SharePoint webových front-end (WFE). (Faktorů, jako je použití vyrovnávání zatížení předřazený, aktuální zatížení serveru a tak dále, můžete omezit nebo odstranit tento narušení.) Ochrana uživatelů před přerušení, doporučujeme vám povolit nebo zakázat kód RBS pouze během plánované údržby.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Konfigurace uvolnění paměti

Při odstranění objektů z webu služby SharePoint, nejsou odstraněny automaticky z úložiště hlasitost kód RBS. Místo toho asynchronní osamocené objektů BLOB program údržby pozadí odstraní z úložiště souborů. Správci systému můžete naplánovat spuštění pravidelně tohoto procesu nebo ji mohli začít ji kdykoli je to nutné.

Tento program údržby (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) automaticky nainstalovaný na všechny WFE Sharepointu nebo aplikace serverech po povolení kód RBS. Aplikace je nainstalovaných v následujícím umístění: *spouštěcí jednotky*: 10.50\Maintainer\ \Program Files\Microsoft vzdáleného úložiště objektů Blob SQL

Informace o konfiguraci a pomocí programu údržbu najdete v tématu [Udržovat pole Kód RBS na SharePoint serveru 2013][8].

>[AZURE.IMPORTANT] Pole Kód RBS funkce maintainer program je náročný. Měli byste naplánovat spustit pouze v období light aktivitu na farmě Sharepointu.

### <a name="delete-orphaned-blobs-immediately"></a>Okamžité odstranění osamoceného objekty BLOB

Pokud potřebujete okamžité odstranění osamoceného objektů BLOB, můžete použít následující pokyny. Všimněte si, že tyto pokyny jsou určené příklad jak to můžete provést v prostředí Sharepointu 2013 se tyto prvky:

- Název databáze obsahu je WSS_Content.
- Název serveru SQL Server je SHRPT13 SQL12\SHRPT13.
- Název webové aplikace je SharePoint – 80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Upgrade nebo přeinstalovat adaptér StorSimple pro službu SharePoint

Použijte následující postup upgrade serveru SharePoint server a pak znovu nainstalujte StorSimple adaptér pro službu SharePoint jednoduše upgrade nebo ji znova nainstalujte adaptér v existující serverové farmě Sharepointu. 

>[AZURE.IMPORTANT] Před pokusem o upgradu své software služby SharePoint a upgrade nebo přeinstalaci adaptér StorSimple pro SharePoint, zkontrolujte následující informace:
>
>- Všechny soubory, které byly pomocí pole Kód RBS přesunuli do externí úložiště nebudou k dispozici až po dokončení instalace znovu povolena funkce kód RBS. Pokud chcete omezit vliv uživatele a zařízení, provádějte upgrade nebo přeinstalaci během plánované údržby.
>
>- Dobu potřebnou pro upgrade/přeinstalace se může lišit v závislosti na celkový počet databází služby SharePoint v serverové farmě SharePoint.
>
>- Po dokončení upgradu/přeinstalace musíte povolit kód RBS databází obsahu. Další informace najdete v tématu [Konfigurace kód RBS](#configure-rbs) .
>
>- Pokud pole Kód RBS jsou konfiguraci ve farmě služby SharePoint, který má velmi velkého počtu databáze (více než 200), mohou stránku **Centrální správa SharePoint** časový limit. V takovém případě po aktualizaci stránky. To neovlivní proces konfigurace.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple adaptér pro odebrání služby SharePoint

Následující postupy popisují, jak přesunout objekty BLOB zpět k obsahu databáze systému SQL Server a pak odinstalovat adaptér StorSimple pro službu SharePoint. 

>[AZURE.IMPORTANT] Budete muset přesunout objekty BLOB zpátky do databáze obsahu před odinstalujete software adaptér. 

### <a name="before-you-begin"></a>Než začnete 

Před přesunout data zpět k obsahu databáze systému SQL Server a zahájení procesu odebrání adaptér shromážděte následující informace:

- Názvy všechny databáze, u kterých je povolené kód RBS
- Cesty UNC nakonfigurované úložiště objektů BLOB

### <a name="move-the-blobs-back-to-the-content-databases"></a>Přesunutí objekty BLOB zpět do databáze obsahu

Než začnete s odinstalací adaptér StorSimple pro software služby SharePoint, musíte nejdřív migrujete všechny objekty BLOB, které byly externalized zpět k obsahu databáze systému SQL Server. Pokud se pokusíte odinstalovat adaptér StorSimple pro SharePoint, než se vrátíte všechny objekty BLOB na databází obsahu, zobrazí se následující upozornění.

![Zpráva upozornění](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>Přejděte objekty BLOB zpět databází obsahu 

1. Stáhněte si každý z externalized objektů.

2. Otevřete stránku **Centrální správa SharePoint** a přejděte na **Nastavení systému**. 

3. V části **Azure StorSimple**klikněte na **Konfigurovat StorSimple adaptér**.

4. Na stránce **Konfigurace adaptér StorSimple** kliknutím na tlačítko **Zakázat** pod každou databází obsahu, které chcete odebrat z externího úložiště objektů BLOB. 

5. Odstranění objektů ze služby SharePoint a pak je znovu odeslat.

Můžete taky můžete použít Microsoft` RBS Migrate()` rutiny prostředí PowerShell zahrnutých ve službě SharePoint. Další informace najdete v tématu [migrace obsahu nebo stmívání kód RBS](https://technet.microsoft.com/library/ff628255.aspx).

Jakmile se vrátíte objekty BLOB k databázi obsahu, přejděte k dalšímu kroku: [odinstalovat adaptér](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Odinstalace adaptér

Po přesunutí objektů BLOB zpět k obsahu databáze systému SQL Server, použijte jeden z následujících možností odinstalovat adaptér StorSimple pro SharePoint.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Odinstalace adaptér pomocí instalačního programu 

1. Přihlaste se k webový server front-end (WFE) se pomocí účtu s oprávněními správce.
2. Poklepejte na adaptér StorSimple pro Instalační služby SharePoint. Spustí se Průvodce nastavením.

    ![Průvodce instalací](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Klikněte na tlačítko **Další**. Zobrazí se další stránky.

    ![Průvodce instalací odebrat stránka](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Klikněte na tlačítko **Odebrat** vyberte proces odebrání. Zobrazí se další stránky.

    ![Průvodce instalací potvrzení stránka](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Klikněte na tlačítko **Odebrat** potvrďte odebrání. Zobrazí se na následující stránce průběh.

    ![Průvodce instalací průběh stránka](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Po dokončení odebrání se zobrazí na stránce dokončit. Klikněte na tlačítko **Dokončit** zavřete průvodce nastavením.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Odinstalace adaptér pomocí ovládacích panelů 

1. Otevřete ovládací panely a pak klikněte na **programy a funkce**.

2. Vyberte **StorSimple adaptér pro SharePoint**a pak klikněte na **odinstalovat**. 

## <a name="next-steps"></a>Další kroky

[Další informace o StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
