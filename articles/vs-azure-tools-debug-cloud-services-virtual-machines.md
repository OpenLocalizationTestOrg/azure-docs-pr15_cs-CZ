<properties 
    pageTitle="Ladění Azure cloudové služby nebo virtuálního počítače ve Visual Studiu | Microsoft Azure"
    description="Ladění cloudové služby nebo virtuálního počítače ve Visual Studiu"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Ladění Azure cloudové služby nebo virtuálního počítače ve Visual Studiu

Visual Studio poskytuje různé možnosti ladění Azure cloudovými službami a virtuálních počítačích.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Ladění cloudové služby na místním počítači

Můžete si ušetřit čas a peníze pomocí Azure výpočet emulátoru ladění cloudové služby v místním počítači. Ladění službu místně před nasazením ho můžete zlepšit spolehlivosti a výkonu bez placení výpočetním přihlášení. Ale některé může dojít k chybám pouze při spuštění do cloudové služby v Azure samotné. Pokud povolíte vzdálené ladění při publikování služby a připojte k ní ladění role instanci můžete ladění tyto chyby.

Emulátor napodobuje služby Azure výpočet a běží ve vašem místním prostředí tak, aby si vyzkoušet a ladění cloudové služby, než ho nasadit. Emulátor zpracovává životním cyklu instancí rolí a poskytuje přístup k simulovaný zdrojů, jako je třeba místní úložiště. Když ladění nebo spuštění služby z aplikace Visual Studio, automaticky spustí emulátor jako pozadí aplikace a potom nasadí služby emulátor. Chcete-li zobrazit služby při spuštění v místním prostředí můžete emulátor. Můžete spustit na plnou verzi nebo express verzi emulátor. (Počínaje Azure 2.3, express verzi emulátor je výchozí hodnota.) V tématu [použití emulátoru Express ke spouštění a ladění místně do cloudové služby](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Ladění cloudové služby na místním počítači

1. Na řádku nabídek vyberte **ladění**, **Spustit ladění** ke spuštění projektu služby Azure cloudu. Jako alternativu můžete stisknutím F5. Zobrazí se zpráva s počáteční emulátoru výpočet. Po spuštění emulátor potvrdí ikonu na hlavním panelu ho.

    ![Azure emulátoru na hlavním panelu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Zobrazení uživatelského rozhraní pro emulátoru výpočetním po otevření místní nabídky pro Azure ikonu v oznamovací oblasti a potom vyberte **Zobrazit výpočet emulátoru uživatelského rozhraní**.

    V levém podokně v uživatelském rozhraní zobrazen služby, které jsou v současnosti nasazená emulátoru výpočetním a instance role, ve kterých se nepoužívá každé služby. Můžete vybrat služby nebo role zobrazíte životního cyklu, protokolování a diagnostických informací v pravém podokně. Pokud umístění fokusu na horním okraji však započítávány okno rozšíří vyplnění v pravém podokně.

1. Výběr příkazů v nabídce **ladění** a nastavte zarážky v kódu přecházet mezi aplikace. Jak přecházet mezi aplikací ladění podokna se aktualizují aktuální stav aplikace. Po ukončení ladění nasazení aplikace se odstraní. Pokud je aplikace roli web a nastavíte vlastnost spuštění akce spustit webový prohlížeč, Visual Studio spustí webová aplikace v prohlížeči. Pokud změníte počet výskytů role v konfiguraci služby, musí zastavení služby cloudu a restartujte ladění tak, aby ladění tyto nové instance roli.

    **Poznámka:** Po ukončení systém nebo ladění služby nejsou ukončit místním emulace a emulátoru úložiště. Musíte explicitně zastavit z oznamovací oblasti.


## <a name="debug-a-cloud-service-in-azure"></a>Ladění do cloudové služby Azure

Ladění do cloudové služby ze vzdáleného počítače, je třeba povolit tuto funkci explicitně při nasazení cloudové služby, aby povinné služby (například msvsmon.exe) nainstalovaných na virtuálních počítačích spuštěné instance role. Je-li neměli vzdálené ladění, když jste publikovali službu, budete muset znovu publikovat službu s ladění povolené.

Pokud povolíte vzdálené ladění do cloudové služby, není vykazovat výkon nebo vzniknou další poplatky. Nepoužívejte vzdálené ladění výrobní službu, protože klienty, kteří používají službu může negativně ovlivnit.

>[AZURE.NOTE] Při publikování do cloudové služby z aplikace Visual Studio můžete povolit **IntelliTrace** pro všechny role v této službě zaměřených .NET Framework 4 nebo .NET Framework 4.5. Pomocí **IntelliTrace**můžete prozkoumat událostí, ke kterým došlo v instanci rolí v minulosti a reprodukování kontextu v té době. V tématu [ladění publikované Cloudová služba s IntelliTrace a Visual Studia](http://go.microsoft.com/fwlink/?LinkID=623016) a [Pomocí IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Povolení vzdáleného ladění ke cloudové službě

1. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

1. Vyberte **pracovního** prostředí a konfiguraci **ladění** .

    Toto je seznam uvádí. Můžete se rozhodnout pro spusťte test prostředí v pracovním prostředí. Pokud povolíte vzdálené ladění v provozním prostředí, může ovlivnit nepříznivě uživatele. Zvolte konfiguraci verze, ale konfiguraci ladění provádí ladění snadnější.

    ![Zvolte konfiguraci ladění](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Postupujte podle obvyklých, ale zaškrtněte políčko **Povolit vzdálené ladění pro všechny role** na kartě **Upřesnit nastavení** .

    ![Ladění konfigurace](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Pokud chcete připojit ladění do cloudové služby Azure

1. V okně Průzkumník serveru rozbalte uzel cloudové službě.

1. Otevření místní nabídky pro role nebo instanci rolí, ke kterému chcete připojit a vyberte **Připojit ladění**.

    Ladění role ladění Visual Studio připojí k každý výskyt určitou roli. Ladění přerušíte na zarážku pro první instanci role spustí tento řádek kód který splňuje všechny podmínky, které zarážky. Pokud ladění instance ladění připojen ke jenom tuto instanci konce a na zarážku pouze v případě, že této konkrétní instanci spustí tento řádek kód který splňuje podmínky na zarážku.

    ![Připojit ladění](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Po ladění připojí k instanci, ladění jako obvykle. Ladění automaticky připojí k procesu odpovídající hostitele pro vaše role. Podle toho, co je roli ladění připojí k w3wp.exe, WaWorkerHost.exe nebo WaIISHost.exe. Pokud chcete ověřit proces, ke kterému je připojen ladění, rozbalte uzel instance v Průzkumníku serveru. [Azure Role architektury](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) najdete v tématu Další informace o Azure procesů.

    ![Kód typ dialogové okno Vybrat](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Jak identifikovat procesy, na které je připojen ladění, otevřete procesy dialogové okno, na řádku nabídek, výběr ladění, Windows a procesů. (Klávesová: kombinaci kláves Ctrl + Alt + Z) Odpojení konkrétní obrázku, otevřete místní nabídku a vyberte **Odpojit obrázku**. Nebo vyhledejte uzel instance v Průzkumníku serveru, tento proces najdete, otevřete místní nabídku a vyberte **Odpojit obrázku**.

    ![Ladění procesů](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Vyhněte se dlouhé zarážkami na zarážky při vzdáleném ladění. Azure zpracuje proces, který je zastaveno po dobu delší než několik minut jako odpovídat a ukončí odesílání přenosy na tuto instanci. Chcete-li zrušit moc dlouho, msvsmon.exe odpojí od procesu.

Odpojení ladění ze všech procesů v instancí nebo role, otevřete v místní nabídce role nebo instanci, kterou jste ladění a vyberte **Odpojit ladění**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Omezení vzdálené ladění v Azure

Z Azure SDK 2.3 vzdálené ladění platí následující omezení.

- S ladění povoleno, nelze publikovat do cloudové služby, ve které všechny role má víc než 25 instancí.

- Ladění používá porty 30400 k 30424, 31400 k 31424 a 32400 k 32424. Pokud se pokusíte použít tyto porty, nebude možné publikovat službu a jednu z následujících chybových zpráv zobrazí v protokolu činnosti Azure: 

    - Chyba ověřování souboru .cscfg proti .csdef soubor. 
    Rezervovaná port rozsah "rozsah" koncový bod Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector role "role" překrývá už definovaný port nebo oblast.
    - Pole přidělení se nezdařila. Opakovat později, zkusit zmenšit velikost OM nebo počet instancí role nebo zkuste nasazení do jiné oblasti.


## <a name="debugging-azure-virtual-machines"></a>Ladění Azure virtuálních počítačích

Programy spuštěné v operačním systému Azure virtuálních počítačích pomocí Průzkumníka serveru ve Visual Studiu můžete ladění. Pokud povolíte vzdálené ladění na Azure virtuálního počítače, nainstaluje Azure vzdálené ladění rozšíření virtuální počítač. Pak můžete připojit k procesy v počítači virtuální a ladění běžným způsobem.

>[AZURE.NOTE] Virtuálních počítačích vytvořený prostřednictvím zásobníku správce Azure zdroje můžete vzdáleně ladění pomocí Průzkumníka mraků ve Visual Studiu 2015. Další informace najdete v tématu [Správa Azure zdroje v Průzkumníkovi cloudu](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Ladění Azure virtuálního počítače

1. V okně Průzkumník serveru rozbalte uzel virtuálních počítačích a vyberte uzel virtuální počítač, který chcete ladění.

1. Otevření místní nabídky a vyberte **Povolit ladění**. Pokud se zobrazí dotaz, pokud jste si jistí, jestli chcete povolit ladění na virtuální počítač, vyberte **Ano**.

    Azure nainstaluje koncovku ladění vzdáleného počítače virtuální povolení ladění.

    ![Počítač virtuální povolení ladění příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Protokol Azure činnosti](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Po dokončení instalace vzdálené ladění rozšíření otevřete virtuálního počítače kontextové nabídky a vyberte **Připojit ladění**

    Azure obdrží seznam procesy v počítači virtuální a zobrazí ve připojit k obrázku dialogovým oknem.

    ![Příkaz ladění](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. V dialogovém okně **připojit k obrázku** potom **Vyberte** omezit výsledky seznamu zobrazíte jenom typy kód, který chcete ladění. Můžete ladění 32 - nebo 64bitová verze spravovaných kód a nativní kód.

    ![Kód typ dialogové okno Vybrat](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Vyberte procesy, které chcete ladění na virtuálního počítače a pak vyberte **Připojit**. Například můžete zvolit procesu w3wp.exe, pokud jste chtěli ladění do webových aplikací v počítači virtuální. Další informace najdete v článku [ladění jeden nebo více procesů ve Visual Studiu](https://msdn.microsoft.com/library/jj919165.aspx) a [Architekturu Role Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Vytvoření webu projektu a virtuálního počítače pro ladění

Před publikováním Azure projektu, který může být užitečné ji otestujte v prostředí obsažené ladění a testování scénáře, který podporuje a kde můžete nainstalovat testování a sledování aplikací. Jedním ze způsobů to je vzdáleně ladění aplikace v počítači virtuální.

Projekty Visual Studio ASP.NET nabízí možnost vytvořit po ruce virtuální počítač, který můžete použít k testování aplikace. Virtuální počítač zahrnuje běžně potřebují koncové body například Powershellu, Vzdálená plocha a WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>K vytvoření webu projektu a virtuálního počítače pro ladění

1. Ve Visual Studiu vytvořte novou webovou aplikaci ASP.NET.

1. V dialogovém okně Nový projekt ASP.NET v části Azure zvolte **virtuálního počítače** v rozevíracím seznamu. Nechte zaškrtnutým políčkem kontrolovat **vytvořit vzdálených prostředků** . Vyberte na tlačítko **OK** .

    Zobrazí se dialogové okno **vytvořit virtuální počítač na Azure** .


    ![Vytvoření dialogového okna ASP.NET web projektu](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Poznámka:** Budete vyzváni k přihlášení k účtu Azure, pokud jste už není přihlášení.

1. Vyberte nastavení pro virtuální počítač a pak vyberte **OK**. Další informace najdete v tématu [virtuálních počítačích]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    Název, který zadáte název DNS bude název virtuálního počítače. 

    ![Vytvoření virtuálního počítače na Azure dialogovým oknem](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure vytvoří virtuálního počítače a potom předpisy a konfiguruje koncové body, jako je Vzdálená plocha a nasazení webu



1. Po konfiguraci virtuální počítač vyberte uzel virtuálního počítače v Průzkumníku serveru.

1. Otevření místní nabídky a vyberte **Povolit ladění**. Pokud se zobrazí dotaz, pokud jste si jistí, jestli chcete povolit ladění na virtuální počítač, vyberte **Ano**. 

    Azure nainstaluje vzdálené ladění rozšíření do počítače virtuální povolení ladění.

    ![Počítač virtuální povolení ladění příkaz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Protokol Azure činnosti](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Publikování projektu, jak je uvedeno v [Postup: nasazení Web projektu pomocí publikování jedním kliknutím ve Visual Studiu](https://msdn.microsoft.com/library/dd465337.aspx). Vzhledem k tomu, že chcete ladění na virtuálního počítače, na stránce **Nastavení** průvodce **Publikovat Web** , vyberte **ladění** jako konfigurace. Zajistíte tím, že kód symboly jsou k dispozici při ladění.

    ![Nastavení publikování](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. V části **Možnosti publikování souboru**vyberte možnost **odebrat další soubory v cílové** projekt byl již nasazení starší současně.

1. Jakmile se publikuje projektu, v místní nabídce virtuálního počítače v okně Průzkumník serveru vyberte **Připojit ladění**

    Azure obdrží seznam procesy v počítači virtuální a zobrazí ve připojit k obrázku dialogovým oknem.

    ![Příkaz ladění](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. V dialogovém okně **připojit k obrázku** potom **Vyberte** omezit výsledky seznamu zobrazíte jenom typy kód, který chcete ladění. Můžete ladění 32 - nebo 64bitová verze spravovaných kód a nativní kód.

    ![Kód typ dialogové okno Vybrat](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Vyberte procesy, které chcete ladění na virtuálního počítače a pak vyberte **Připojit**. Například můžete zvolit procesu w3wp.exe, pokud jste chtěli ladění do webových aplikací v počítači virtuální. Další informace najdete v článku [ladění jeden nebo více procesů ve Visual Studiu](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Další kroky

- Shromáždění protokolů volání a události ze serveru vydání pomocí **Intellitrace** . V tématu [ladění publikované Cloudová služba s IntelliTrace a Visual Studia](http://go.microsoft.com/fwlink/?LinkID=623016).
- Podrobné informace Zaprotokolují kód spuštěný v rámci role, zda role běží ve vývojovém prostředí nebo v Azure pomocí **Diagnostiky Azure** . V části [protokolování operace shromažďování dat pomocí diagnostických nástrojů Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).
