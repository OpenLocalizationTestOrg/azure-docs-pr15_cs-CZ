<properties
    pageTitle="Nastavení serveru SQL Server virtuálního počítače jako server Poznámkový blok IPython | Microsoft Azure"
    description="Nastavte si dat vědy virtuálního počítače s SQL Server a IPython Server."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení serveru SQL Azure virtuálního počítače jako serveru IPython Poznámkový blok pro pokročilé technologie pro analýzu

Toto téma ukazuje, jak zřídit a konfigurace systému SQL Server virtuální počítač má být použit jako součást prostředí pro výzkum cloudové data. Virtuální počítač Windows nastaven podpůrných nástrojů, jako jsou IPython Poznámkový blok, Průzkumníka úložišť Azure a AzCopy, jakož i další nástroje, které jsou vhodné pro datové vědy projekty. Azure Explorer úložiště a AzCopy, například nabídněte pohodlný předávat data k úložišti objektů blob Azure ze svého místního počítače a stáhněte si ho do místního počítače z úložiště objektů blob.

Galerie Azure virtuálního počítače obsahuje několik obrázků, které obsahují Microsoft SQL Server. Vyberte obrázek SQL Server OM, je vhodné pro vaše potřeby data. Doporučené obrázky jsou:

- SQL Server 2012 SP2 Enterprise pro malé a střední dat velikosti
- SQL Server 2012 SP2 Enterprise optimalizovaná pro DataWarehousing úloh pro formáty Get Directions: obrovské dat

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise obrázek **neobsahuje datový disk**. Bude potřeba přidat a/nebo připojit jeden nebo více virtuální pevných discích pro ukládání data. Při vytváření Azure virtuální počítač má disk pro operační systém jednotky C a dočasné diskové jednotky D. Nepoužívejte jednotku D pro ukládání data. Jak naznačuje název obsahuje dočasné úložiště. Vzhledem k tomu, že není uložena v Azure úložiště nabízí žádné redundance nebo zálohování.


##<a name="Provision"></a>Připojení k portálu klasické Azure a zřízení virtuálního počítače SQL serveru

1.  Přihlaste se k [Portálu klasické Azure](http://manage.windowsazure.com/) pomocí svého účtu.
    Pokud nemáte účet Azure, navštěvujte blog o [Azure bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

2.  Na klasické portálu Azure v levé dolní části webové stránky klikněte na **+ Nový**klikněte na **Výpočet**, klikněte na **VIRTUÁLNÍHO počítače**a klikněte na **Z GALERIE**.

3.  Na stránce **vytvořit virtuální počítač** vyberte virtuální počítač obrázek obsahující podle vašich potřeb data SQL serveru a potom klikněte na další šipku v pravém dolním rohu stránky. Nejaktuálnější informace o podporovaných obrázky SQL Server na Azure najdete v tématu [Začínáme se serverem SQL Server ve virtuálních počítačích Azure](http://go.microsoft.com/fwlink/p/?LinkId=294720) v dokumentaci k [SQL Server ve virtuálních počítačích Azure](http://go.microsoft.com/fwlink/p/?LinkId=294719) nastavení.

    ![Výběrem Server SQL OM][1]

4.  Na první stránce **Konfiguraci virtuálního počítače** zadejte následující informace:

    -   Zadejte **název počítače virtuální**.
    -   V dialogovém okně **Nové uživatelské jméno** zadejte jedinečný uživatelské jméno pro OM místního účtu správce.
    -   V dialogovém okně **Nové heslo** zadejte silné heslo. Další informace najdete v tématu [Silného hesla](http://msdn.microsoft.com/library/ms161962.aspx).
    -   V dialogovém okně **POTVRDIT heslo** zadejte heslo znovu.
    -   Vyberte odpovídající **velikost** z rozevíracího seznamu.

     > [AZURE.NOTE] Velikost virtuální počítač není zadán během vytváření: A2 je nejmenší možnou velikostí doporučeného pro typ pracovního vytížení výroby. Minimální velikost doporučené virtuálního počítače je A3 při použití SQL Server Enterprise Edition. Vyberte A3 nebo vyšší při použití SQL Server Enterprise Edition. Při použití SQL Server 2012 nebo 2014 Enterprise optimalizované pro transakční úloh obrázků, vyberte A4.
Při použití SQL Server 2012 nebo 2014 Enterprise optimalizované pro obrázky s pracovního vytížení skladování dat vyberte A7. Velikost vybrané omezuje počet disků dat, které můžete konfigurovat. Nejaktuálnější informace o velikosti dostupné virtuálního počítače a počet jejích disků dat, které můžete připojit k počítači virtuální najdete v článku [Virtuálního počítače velikosti Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Ceny informace, najdete v článku [Virtuální ceny Macines](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Klikněte na další šipku v pravém dolním pokračovat.

    ![Konfigurace OM][2]

5.  Na druhé stránce **konfiguraci virtuálního počítače** konfiguraci zdroje pro sítě, ukládání a dostupnosti

    -   V dialogovém okně **Cloudové služby** vyberte **vytvořit nový cloudové služby**.
    -   Do pole **Název cloudové služby DNS** zadejte první část názvu DNS podle svého výběru tak, aby dokončením název ve formátu **TESTNAME.cloudapp.net**
    -   V dialogovém okně **Oblast a SPŘAŽENÍ skupiny/virtuální sítě** vyberte oblast hostitelem tento virtuální obrázek.
    -   V okně **Účtu úložiště**vyberte existující účet úložiště nebo vybrat automaticky generované.
    -   V dialogovém okně **Nastavit dostupnost** vyberte **(žádné)**.
    -   Přečtěte si a přijměte ceny informace.

6.  V části **koncové body** klikněte do prázdné rozevírací seznam v části **název**a vyberte **MSSQL** potom zadejte číslo portu instance Database Engine (**1433** pro výchozí instanci).

7.  Vaše OM SQL serveru můžete taky sloužit jako Server IPython Poznámkový blok, který bude možné konfigurovat později.
    Přidejte nový koncový bod k určení portu, můžete u serveru IPython Poznámkový blok. Ve sloupci **název** zadejte název, vyberte číslo portu vašeho nastavení pro veřejný port a 9999 privátní číslo portu.

    Klikněte na další šipku v pravém dolním pokračovat.

    ![Vyberte MSSQL a IPython porty][3]

8.  Přijměte výchozí možnost **instalace OM agent** zaškrtnuté a klikněte značku zaškrtnutí v pravém dolním rohu průvodci a dokončete OM zřizování proces.

    `![Možnosti konečný OM][4]

9.  Počkejte, zatímco Azure připraví virtuálního počítače. Stav virtuálního počítače a pokračujte očekávat:

    -   Spuštění (zřizování)
    -   Přerušili
    -   Spuštění (zřizování)
    -   Spuštění (zřizování)
    -   Spuštění


##<a name="RemoteDesktop"></a>Otevřete virtuálního počítače pomocí vzdálené plochy a dokončete nastavení

1.  Dokončení zřizování, klikněte na název počítače virtuální přejdete na stránku řídicího PANELU. V dolní části stránky klikněte na **Připojit**.

2.  Zvolte Otevřít rpd pomocí programu Windows Vzdálená plocha (`%windir%\system32\mstsc.exe`).

3.  V dialogovém okně **Zabezpečení systému Windows** zadejte heslo místního účtu správce, který jste zadali v předchozím kroku.
    (Můžete být požádáni o přihlašovací údaje virtuálního počítače.)

4.  Při prvním přihlášení na tomto počítači virtuální několik procesů může být nutné dokončit, včetně nastavení plochy, aktualizací a dokončení úkolů počáteční konfigurace systému Windows (sysprep). Po ukončení programu Windows sysprep dokončení instalace SQL serveru konfiguraci. Tyto úkoly může způsobovat zpoždění několik minut a dokončení. `SELECT @@SERVERNAME`až dokončení instalace SQL serveru, SQL Server Management Studio nemusí být visable na úvodní stránce nemusí vrátit správný název.

Jakmile jste připojeni k virtuální počítač s Windows Vzdálená plocha virtuální počítač funguje podobně jako jakýkoli jiný počítač. Připojení k výchozí instanci systému SQL Server SQL Server Management Studio (spuštěné v počítači virtuální) obvyklým způsobem.


##<a name="InstallIPython"></a>Instalace IPython Poznámkový blok a dalších podpůrných nástrojů

Pro nastavení vaší nové OM SQL Server sloužit jako serveru IPython Poznámkový blok a nainstalujte dalších podpůrných nástrojů takové AzCopy Průzkumníka úložišť Azure, užitečné balíčků Python dat pro výzkum a ostatní, je k dispozici zvláštní přizpůsobení skript pro vás. Chcete-li nainstalovat:

- Klikněte pravým tlačítkem myši na ikonu **Start systému Windows** a klikněte na **příkazovém řádku (Správci)**
- Zkopírujte následující příkazy a vložte do příkazového řádku.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Po zobrazení výzvy zadejte heslo podle svého výběru pro server IPython Poznámkový blok.
- Přizpůsobení skript automaticky několik po instalaci postupy, které zahrnují:
    + Instalace a nastavení serveru IPython poznámkového bloku
    + Otevření porty TCP v bráně firewall systému Windows pro koncové body vytvořili:
    + Pro vzdálené připojení SQL serveru
    + Pro připojení k serveru vzdálené IPython poznámkového bloku
    + Příprava ukázkových IPython poznámkových bloků nebo skripty SQL
    + Stažení a instalaci užitečné balíčků Python vědy dat
    + Stažení a instalaci Azure nástrojů, jako jsou AzCopy a Průzkumník úložišť Azure  
<br>
- Můžete přistupovat a spuštění IPython Poznámkový blok z libovolného místní nebo vzdálené prohlížeče pomocí adresy URL formuláře `https://<virtual_machine_DNS_name>:<port>`, kde je veřejné port IPython jste vybrali při zřizování virtuální počítač.
- Poznámkový blok IPython server běží jako služba pozadí a po restartování automaticky po restartování počítače virtuální.

##<a name="Optional"></a>Připojení dat disku podle potřeby

Pokud obrázek OM neobsahuje data disků, tedy disků než jednotku C: (s operačním systémem disk) nebo D jednotka (dočasné disk), potřebujete přidat jeden nebo více disků dat pro ukládání data. Obrázek OM pro SQL Server 2012 SP2 Enterprise optimalizovaná pro DataWarehousing úloh je předkonfigurovaná s další disk pro SQL Server data a souborů protokolu.

 > [AZURE.NOTE] Nepoužívejte jednotku D pro ukládání data. Jak naznačuje název obsahuje dočasné úložiště. Vzhledem k tomu, že není uložena v Azure úložiště nabízí žádné redundance nebo zálohování.

Připojit disků další data, postupujte podle kroků popsaných v [tom, jak připojit Disk dat do virtuálního počítače Windows](virtual-machines-windows-classic-attach-disk.md), který vás provede:

1. Připojení prázdné discích do virtuálního počítače zřízení v předchozích krocích
2. Inicializace nové discích v virtuálního počítače


##<a name="SSMS"></a>Připojení k SQL Server Management Studio a povolení kombinovaný režim ověřování

Databázový stroj SQL Server nelze použít ověřování systému Windows bez doménovém prostředí. Připojení k databázový stroj z jiného počítače, nakonfigurujte kombinovaný režim ověřování serveru SQL Server. Kombinovaný režim ověřování umožňuje ověřování serveru SQL Server a ověřování systému Windows. Režim ověřování SQL se při jedí data přímo z databáze SQL serveru OM v [Azure počítače výukové Studio](https://studio.azureml.net) používání modulu Import dat vyžaduje.

1.  Připojeného k počítači virtuální pomocí vzdálené plochy, použijte podokno Windows **hledání** a zadejte **SQL Server Management Studio** (SMSS). Kliknutím sem SQL Server Management Studio (SSMS). Chcete přidat zástupce SSMS na vaší ploše pro budoucí použití.

    ![Zahájení SSMS][5]

    Při prvním otevření Management Studio musí vytvořit prostředí Management Studio uživatele. Může to trvat několik minut.

2.  Při otevření, Management Studio zobrazí dialogové okno **připojit k serveru** . Do pole **název serveru** zadejte název virtuálního počítače se připojit k databázový stroj v Průzkumníkovi objektu.
    (Namísto konkrétního jména virtuálního počítače můžete taky použít **(místní)** nebo jedno období jako **název serveru**. Vyberte **Ověřování systému Windows**a nechejte ** *vaše\_OM\_název*\\vaše\_místní\_správce** do pole **uživatelské jméno** . Klikněte na **Připojit**.

    ![Připojení k serveru][6]

    <br>

     > [AZURE.TIP] Můžete změnit pomocí změnu klíče registru systému Windows nebo SQL Server Management Studio režim ověřování serveru SQL Server. Pokud chcete změnit režim ověřování pomocí změnu klíče registru, začněte **Nový dotaz** a proveďte následující skript:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    Chcete-li změnit režim ověřování pomocí serveru SQL Server management Studio:

3.  V **SQL Server Management Studio Průzkumník objektů**klikněte pravým tlačítkem myši na název instance serveru SQL Server (název virtuálního počítače) a klikněte na **Vlastnosti**.

    ![Vlastnosti serveru][7]

4.  Na stránce **zabezpečení** ve skupinovém rámečku **ověřování serveru**vyberte **SQL serveru a v režimu ověřování systému Windows**a potom klikněte na **OK**.

    ![Vyberte režim ověřování][8]

5.  V dialogovém okně **SQL Server Management Studio** klikněte na **OK** potvrďte požadovaného restartujte SQL serveru.

6.  V programu **Průzkumník objektů**klikněte pravým tlačítkem na serveru a pak klikněte na **Restartovat**. (Pokud je systém SQL Server Agent, ho taky restartování.)

    ![Restartování počítače][9]

7.  V dialogovém okně **SQL Server Management Studio** klepnutím na tlačítko **Ano** souhlasíte, že chcete restartujte SQL serveru.

##<a name="Logins"></a>Vytvořte přihlášení ověřování serveru SQL Server

Připojení k databázový stroj z jiného počítače, musíte vytvořit alespoň jeden přihlášení ověřování serveru SQL Server.  

Můžete vytvořit nový přihlášení serveru SQL Server programově nebo pomocí SQL Server Management Studio. Pokud chcete vytvořit nový uživatel členem pomocí ověřování serveru SQL programově, spusťte **Nový dotaz** a proveďte následující skript. Nahrazení < nové uživatelské jméno\> a < nové heslo\> s možností *uživatelského jména* a *hesla*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Zásady hesel upravte v případě potřeby (ukázkové kód vypne zásad vypršení platnosti kontroly a heslo). Další informace o přihlášení SQL serveru najdete v tématu [Vytvoření přihlášení](http://msdn.microsoft.com/library/aa337562.aspx).  

Vytvoření nového serveru SQL Server přihlášení pomocí SQL Server Management Studio:

1.  V **SQL Server Management Studio Object Explorer**rozbalte složku instance serveru, ve kterém chcete vytvořit nové přihlášení.

2.  Klikněte pravým tlačítkem myši na složku **zabezpečení** , přejděte na **Nový**a vyberte **... přihlášení**.

    ![Nové přihlášení][10]

3.  V dialogovém okně **přihlášení - nový** na kartě **Obecné** zadejte jméno nového uživatele do pole **přihlašovací jméno** .

4.  Vyberte **ověřování serveru SQL Server**.

5.  Do pole **heslo** zadejte heslo pro nové uživatele. Do pole **Potvrzení hesla** znova zadejte heslo.

6.  K jejímu vynucení možnosti zásady hesel pro složitostí i vynucení, vyberte **zásady pro heslo vynutit** (doporučeno). Toto je výchozí možnost, je-li vybrán ověřování serveru SQL Server.

7.  Pokud chcete vynutit heslo možnosti zásad vypršení platnosti, vyberte **vypršení platnosti hesla vynutit** (doporučeno). Vynucení zásady pro heslo musí být zaškrtnuté toto políčko Povolit. Toto je výchozí možnost, je-li vybrán ověřování serveru SQL Server.

8.  Přimět uživatele k vytváření nového hesla po prvním přihlášení se používá, vyberte možnost **uživatel musí změnit heslo při příštím přihlášení** (doporučeno-li toto přihlášení někoho jiného použít. Pokud přihlášení pro vlastní potřebu, není tato možnost slouží.) Vynucení vypršení platnosti hesla musí být zaškrtnuté toto políčko Povolit. Toto je výchozí možnost, je-li vybrán ověřování serveru SQL Server.

9.  V seznamu **výchozí databáze** vyberte výchozí databáze pro přihlášení. **hlavní** je výchozí nastavení pro tuto možnost. Pokud jste ještě nevytvořili databázi uživateli, nechte hodnotu nastavenou na **předlohy**.

10. V seznamu **výchozí jazyk** ponechte **výchozí** hodnotu.

    ![Vlastnosti přihlášení][11]

11. Pokud je to prvním přihlášení, který vytváříte, můžete určit toto přihlášení jako správce serveru SQL Server. Pokud ano, zaškrtněte na stránce **Role serveru** **členem**.

    > [AZURE.IMPORTANT] Členové členem role serveru mají úplnou kontrolu nad databázový stroj. Z bezpečnostních důvodů měli byste pečlivě omezit členství v této role.

    ![členem][12]

12. Klikněte na OK.

##<a name="DNS"></a>Zjištění názvu DNS virtuálního počítače

Připojení k SQL serveru databázový stroj z jiného počítače, musíte znát název systému DNS (Domain Name) virtuálního počítače.

(Toto je název, který používá k identifikaci virtuálního počítače k Internetu. Můžete použít na IP adresu, ale na IP adresu se může změnit adresa přesun Azure prostředky pro redundance nebo údržbu. Název DNS bude stabilní vzhledem k tomu můžete přesměrovaní na nové adresy IP.)

1.  Na portálu klasické Azure (nebo v předchozím kroku) vyberte **VIRTUÁLNÍCH počítačích**.

2.  Na stránce **VIRTUÁLNÍHO počítače instance** ve sloupci **Název DNS** najděte a zkopírujte název DNS pro virtuální počítač, který se objeví předcházet **http://**. (V uživatelském rozhraní se nemusí zobrazit celý název, ale klikněte na něj pravým tlačítkem myši a vyberte Kopírovat.)

##<a name="cde"></a>Připojení k databázový stroj z jiného počítače

1.  Na počítač připojený k Internetu otevřete SQL Server Management Studio.

2.  V dialogovém okně **připojit k serveru** nebo **připojit k databázový stroj** v poli **název serveru** zadejte název DNS virtuálního počítače (určenému v předchozí úkol) a veřejné koncový bod číslo portu ve formátu *Název_dns číslo_portu* například **tutorialtestVM.cloudapp.net,57500**.

3.  V dialogovém okně **ověření** vyberte **Ověřování serveru SQL Server**.

4.  V dialogovém okně **přihlášení** zadejte název přihlášení, který jste vytvořili v předchozích úkolu.

5.  Do pole **heslo** zadejte heslo přihlášení, který jste vytvořili v předchozích úkolu.

6.  Klikněte na **Připojit**.

##<a name="amlconnect"></a>Připojení k databázový stroj z Azure počítače výuka

Na pozdější fáze procesu týmu dat pro výzkum použijete [Azure počítače výukové Studio](https://studio.azureml.net) sestavovat a nasazovat modely výukové počítače. Jedí dat z databáze SQL serveru OM přímo do Azure počítače výukové pro vzdělávání nebo bodování, pomocí modulu **Importovat Data** do nové [Azure počítače výukové Studio](https://studio.azureml.net) testu. Toto téma se vztahuje v dalších podrobností pomocí Průvodce odkazy týmu dat pro výzkum obrázku. Úvod, najdete v článku [Co je Azure počítače výukové Studio?](machine-learning-what-is-ml-studio.md).

2.  V podokně **Vlastnosti** [modul importovat Data](https://msdn.microsoft.com/library/azure/dn905997.aspx)vyberte z rozevíracího seznamu **Zdroj dat** **Databáze SQL Azure** .

3.  Do textového pole **název databázového serveru** zadejte`tcp:<DNS name of your virtual machine>,1433`

4.  Zadejte uživatelské jméno SQL v textovém poli **název serveru uživatelského účtu** .

5.  Heslo uživatele sql zadejte do textového pole **heslo uživatelského účtu serveru** .

    ![Azure ML importovat Data][13]

##<a name="shutdown"></a>Vypnutí a zrušit virtuální počítač není při použití

Azure virtuálních počítačích jsou cenou jako **platí jenom pro můžete použít**. Abyste měli jistotu, že jste nejsou účtované bez použití virtuálního počítače, musí být ve stavu **Zastaveno (Deallocated)** .

> [AZURE.NOTE] Vypnutí virtuálního počítače z uvnitř (pomocí možností power Windows), se zastaví OM, ale zůstane přidělit. Abyste měli jistotu, že nejsou účtované, vždy zastavení virtuálních počítačích z [Azure klasické portálu](http://manage.windowsazure.com/). Můžete taky ukončit OM pomocí prostředí Powershell tak, že zavoláte ShutdownRoleOperation s "PostShutdownAction" rovno "StoppedDeallocated".

Vypnutí a zrušit virtuálního počítače:

1. Přihlaste se k [Portálu klasické Azure](http://manage.windowsazure.com/) pomocí svého účtu.  

2. V levém navigačním panelu vyberte **VIRTUÁLNÍCH počítačích** .

3. V seznamu virtuálních počítačích klikněte na název virtuálního počítače a přejděte na stránku **řídicího PANELU** .

4. V dolní části stránky klepněte na možnost **vypnout**.

![Vypnutí OM][15]

Virtuální počítač budou odebrána nikoli však odstranit. Kdykoli z portálu Microsoft Azure klasické může restartujte počítač virtuální.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Vaše Azure SQL Server OM je připraven k použití: Další krok?

Virtuální počítač je nyní připravena k použití v jiné vědecké cvičení vaše data. Virtuální počítač je také připravena k použití jako server IPython Poznámkový blok pro průzkum a zpracováním dat a dalších úlohách ve spojení s Azure počítače učení s pomocí týmu dat pro výzkum obrázku (TDSP).

Další kroky procesu vědy dat jsou namapované [Týmu dat pro výzkum obrázku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může zahrnovat kroky, které přesunutí dat do HDInsight, zpracovat a přehrajte si ho tam při přípravě na výukové z dat s Azure počítače učení.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
