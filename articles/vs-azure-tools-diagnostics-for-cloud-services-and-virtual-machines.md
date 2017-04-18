<properties
   pageTitle="Konfigurace diagnostických nástrojů pro Azure cloudovými službami a virtuálních počítačích | Microsoft Azure"
   description="Popisuje, jak nakonfigurovat diagnostické informace pro ladění služby Azure cloude a virtuálních počítačích (VMs) ve Visual Studiu."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfigurace diagnostických nástrojů pro Azure cloudovými službami a virtuálních počítačích

Pokud potřebujete Poradce při potížích s Azure cloudové služby nebo Azure virtuálního počítače, můžete nakonfigurovat Azure diagnostiky snadněji pomocí aplikace Visual Studio. Azure diagnostiky zaznamenává dat o systému a protokolování údajů o virtuálních počítačích a instance virtuální počítač se systémem cloudové služby a přepojí tato data do účtu úložiště podle svého výběru. Další informace o protokolování v Azure Diagnostika v tématu [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure](./app-service-web/web-sites-enable-diagnostic-log.md) .

V tomto tématu se dozvíte, jak můžete povolit a nakonfigurovat Azure diagnostiky ve Visual Studiu, před a po nasazení i v Azure virtuálních počítačích. Je také se dozvíte, jak vyberte typy diagnostické informace shromažďovat a jak po se shromažďují zobrazit informace.

Konfigurace Azure diagnostiky následujícími způsoby:

- Konfigurace nastavení diagnostických nástrojů v dialogovém okně **Konfigurace diagnostiky** ve Visual Studiu můžete změnit. Nastavení se ukládají do souboru s názvem diagnostics.wadcfgx (diagnostics.wadcfg v Azure SDK 2.4 nebo nižší). Můžete taky můžete přímo upravit konfiguračního souboru. Pokud ručně aktualizovat soubor konfigurace změny se projeví dalšího čas nasadíte cloudu služby Azure nebo spuštění služby v emulátor.

- Změna nastavení diagnostických nástrojů pro pracovního cloudové služby nebo virtuálního počítače pomocí Visual Studio **Cloudu Průzkumníka** nebo **Průzkumníka serveru** .

## <a name="azure-26-diagnostics-changes"></a>Změny diagnostiky Azure 2.6

Pro Azure SDK 2.6 projekty ve Visual Studiu tyto změny provedené. (Použijí tyto změny taky na novější verzi Azure SDK.)

- Místní emulátoru nyní podporuje diagnostických nástrojů. To znamená, že můžete shromažďovat data o diagnostických nástrojů a zajistěte, aby že aplikace je vytváření správné trasování, zatímco jste vývoj a testování ve Visual Studiu. Připojovací řetězec `UseDevelopmentStorage=true` umožňuje diagnostiky shromažďování dat a používáte cloudové služby projektu ve Visual Studiu pomocí emulátoru Azure úložiště. Všechna data diagnostických nástrojů se shromažďují účtu úložiště (vývoj úložiště).

- Diagnostika úložiště účtu připojovací řetězec (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je uložen ještě jednou v souboru konfigurace (.cscfg) služby. V Azure SDK 2,5 účtu úložiště diagnostiky zadaná v souboru diagnostics.wadcfgx.

Existuje několik důležitých rozdíly mezi jak připojovací řetězec fungovala Azure SDK 2.4 v dřívější a jak to funguje v Azure SDK 2.6 a novějších verzích.

- V Azure SDK 2.4 a starších verzích připojovací řetězec byl použit jako modul runtime modul plug-in Diagnostika získání informací o účtu úložiště pro přenos protokolování diagnostiky.

- V Azure SDK 2.6 a v novějších verzích připojovací řetězec diagnostiky používá Visual Studio nakonfigurovat koncovku diagnostických nástrojů s informací o účtu odpovídající úložiště při publikování. Připojovací řetězec můžete nastavit jiné úložiště účty pro jiné službě konfigurace, které aplikace Visual Studio použije při publikování. Protože modul plug-in diagnostiky už není dostupná (po Azure 2,5 SDK), souboru .cscfg samostatně není možné povolit koncovku diagnostických nástrojů. Je třeba povolit koncovku samostatně pomocí nástrojů Visual Studia ATP Powershellu.

- Zjednodušit proces konfigurace koncovku diagnostiky pomocí prostředí PowerShell, výstup balíček aplikace Visual Studio obsahuje také veřejné konfigurace XML pro rozšíření diagnostických nástrojů pro každou roli. Visual Studio používá připojovací řetězec diagnostiky k naplnění informace o účtu úložiště účastní veřejné konfigurace. Veřejné konfigurace soubory vytvořené ve složce rozšíření a postupujte podle vzorku PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Všechny Powershellu na základě nasazení lze použít tento každou konfiguraci přiřadit roli.

- Připojovací řetězec do souboru .cscfg taky používá [Azure portálu](http://go.microsoft.com/fwlink/p/?LinkID=525040) pro přístup k datům diagnostiky tak, aby se zobrazil na kartě **Sledování** . Připojovací řetězec je potřeba ke konfiguraci služby zobrazení Podrobný monitorování dat na portálu.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrace projektů Azure SDK 2.6 a novější

Při migraci z Azure SDK 2,5 Azure SDK 2.6 nebo novější, pokud máte účet úložiště diagnostiky podle .wadcfgx soubor, potom zůstane tam. Umožní využít výhod flexibilní použití účty různých úložiště u různých úložiště konfigurací budete muset ručně přidat připojovací řetězec do projektu. Projekt se migraci z Azure SDK 2.4 nebo dřívější na aplikaci a Azure SDK 2.6, se zachovají řetězce Diagnostika připojení. Dejte pozor, změny v tom, jak připojení řetězce jsou považovány v Azure SDK 2.6 určené v předchozí části.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Způsob účtu úložiště diagnostických nástrojů Visual Studio

- Je-li připojovací řetězec Diagnostika v souboru .cscfg, Visual Studio se používá ke konfiguraci koncovku diagnostiky při publikování a při vytváření souborů xml veřejné konfigurace během balení.

- Je-li žádné diagnostiky připojovací řetězec v souboru .cscfg, pak Visual Studio přejde pomocí účtu úložiště zadaného v souboru .wadcfgx konfigurace koncovku diagnostiky při publikování a generování souborů xml veřejné konfigurace při sbalení.

- Diagnostika připojovací řetězec do souboru .cscfg přednost před účtu úložiště v souboru .wadcfgx. Je-li připojovací řetězec Diagnostika v souboru .cscfg, Visual Studio používá a ignoruje účtu úložiště v .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Co znamená "aktualizace vývoj úložiště připojení řetězce..." zaškrtávací políčko dělat?

Zaškrtávací políčko pro **aktualizaci vývoj úložiště připojovací řetězec pro diagnostiku a ukládání do mezipaměti s Microsoft Azure úložiště přihlašovací údaje účtu při publikování na Microsoft Azure** vám pohodlný způsob, jak aktualizovat všechny vývojové úložiště účtu připojení řetězce účet Azure úložiště zadaný při publikování.

Předpokládejme například, zaškrtněte toto políčko a připojovací řetězec diagnostiky Určuje `UseDevelopmentStorage=true`. Při publikování projektu do Azure Visual Studio bude automaticky aktualizován připojovací řetězec diagnostiky úložiště účet, který jste zadali v Průvodci publikovat. Ale pokud účet skutečné úložiště byla specifikována jako diagnostiky připojovací řetězec, pak tento účet bude použita.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostika funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK 2,5 a novější

Pokud upgradujete projektu z Azure SDK 2.4 2,5 SDK Azure nebo novější, byste měli mít na paměti následující funkce rozdíly diagnostických nástrojů.

- **Rozhraní API konfigurace jsou změněny** – programové konfigurace diagnostiky je k dispozici v Azure SDK 2.4 a starších verzích, ale se nedá použít v Azure SDK 2,5 a novějších verzích. Pokud konfigurace diagnostiky je aktuálně definované v kódu, musíte nakonfigurovat nastavení těchto přímo v migrované projektu v pořadí diagnostických nástrojů na pokračovat v práci. Konfigurační soubor diagnostiky Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx Azure SDK 2,5 a v novějších verzích.

- **Diagnostika pro cloudové služby aplikace je možné konfigurovat jenom na úrovni role, nikoli na úrovni instance.**

- **Pokaždé, když nasadíte aplikace se aktualizuje konfigurace diagnostiky** – Pokud změnit konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikaci to může způsobovat problémy s dostupná.

- **V Azure SDK 2,5 a novější, selhat výpisy nejsou nakonfigurováni v diagnostiky konfiguračního souboru, nejsou v kódu** – Pokud máte výpisy nakonfigurováno v kódu, budete muset ručně přepojit konfiguraci z kódu konfiguračního souboru, protože nejsou převedeny výpisy během migrace k Azure SDK 2.6.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Povolení Diagnostika v cloudu služby projektů před jejich nasazení

Ve Visual Studiu můžete shromažďovat data o diagnostických nástrojů pro role spuštěné v Azure, když spustíte službu v emulátor před nasazením. V souboru konfigurace diagnostics.wadcfgx jsou uloženy všechny změny nastavení diagnostických nástrojů ve Visual Studiu. Toto nastavení zadat účet úložiště uložení dat diagnostiky při nasazení cloudové služby.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Chcete-li povolit diagnostiky ve Visual Studiu před nasazením

1. V místní nabídce pro roli, která vás zajímá vyberte **Vlastnosti**a potom klikněte na kartu **Konfigurace** v okně **Vlastnosti** roli.

1. Ujistěte se, že je zaškrtnuté políčko **Povolit Diagnostika** v části **Diagnostics** .

    ![Přístup k možnost povolit diagnostických nástrojů](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Zvolte tři tečky (...) s cílem určit účet úložiště místo, kam chcete data diagnostiky uložený. Účet úložiště, které zvolíte bude na místo, kam se ukládají data diagnostiky.

    ![Určení úložiště účet, který chcete použít](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. V dialogovém okně **Vytvořit úložiště připojovací řetězec** určete, zda chcete připojit pomocí emulátoru úložiště Azure, předplatné Azure nebo ručně zadané přihlašovací údaje.

    ![Dialogové okno účet úložiště](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Pokud se rozhodnete Microsoft Azure emulátoru úložiště možnost připojovací řetězec je nastavena na UseDevelopmentStorage = true.

  - Pokud se rozhodnete možností předplatného, můžete zvolit název účtu a Azure předplatného, který chcete použít. Můžete tlačítka Správa účtů pro správu předplatného Azure.

  - Pokud zvolíte možnost ručně zadané přihlašovací údaje, zobrazí se výzva k zadání jména a klíč Azure účtu, který chcete použít.

1. Klikněte na **Konfigurovat** tlačítko zobrazíte dialogové okno **Konfigurace diagnostiky** . Jednotlivých kartách (s výjimkou **Obecné** a **Adresáře protokolu**) představuje zdroje diagnostiky dat, který můžete shromažďovat. Na výchozí kartu **Obecné**, nabízí následující možnosti získávání dat diagnostiky: **pouze chyby**, **všechny informace**a **vlastní plán**. Výchozí možnost **pouze chyby**, trvá nejnižších velikost úložiště, protože nemá převádět upozornění nebo sledování zpráv. Možnosti všechny informace přenáší většina informací a je proto většina drahé možnost z hlediska úložiště.

    ![Povolení Azure diagnostiky a konfigurace](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. V tomto příkladu přepínač **vlastní plán** , je možné upravit dat shromážděných.

1. Pole **Disková kvóta MB** určuje zbývajícího prostoru, který se má přidělit ve vašem účtu úložiště pro diagnostiky data. Pokud chcete, můžete změnit výchozí hodnota.

1. Na jednotlivých kartách diagnostiky dat chcete shromažďovat, vyberte jeho **Povolit převod <log type> ** zaškrtávací políčko. Například pokud chcete shromáždit protokoly aplikace, zaškrtněte políčko **Povolit převod protokoly aplikace** na kartě **Protokoly aplikace** . Také zadejte všechny ostatní informace požadované datové typy jednotlivých diagnostických nástrojů. Naleznete v části **zdroje dat diagnostiky konfigurovat** dál v tomto tématu informace o konfiguraci na jednotlivých kartách.

1. Po povolení kolekce všechna data diagnostických nástrojů, který chcete klikněte na tlačítko **OK** .

1. Spuštění projektu služby Azure cloudu ve Visual Studiu jako obvykle. Při použití aplikace protokolu informace, které jste povolili je uložené na účtu Azure úložiště, které jste zadali.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Povolení Diagnostika v Azure virtuálních počítačích

Ve Visual Studiu můžete shromažďovat data o diagnostických nástrojů pro Azure virtuálních počítačích.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Chcete-li povolit Diagnostika v Azure virtuálních počítačích

1. V **Průzkumníku serveru**zvolte uzel Azure a připojte k předplatnému Azure Pokud jste už není připojení.

1. Rozbalte položku **virtuálních počítačích** . Vytvoření nového virtuálního počítače nebo vybrat tu, která už existuje.

1. V místní nabídce pro virtuální počítač, který vás zajímá vyberte **Konfigurovat**. Zobrazí dialogové okno Konfigurace virtuálního počítače.

    ![Konfigurace Azure virtuálního počítače](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Pokud již není nainstalovaný je, přidejte rozšíření Microsoft sledování Agent diagnostiky. Tuto linku umožňuje shromáždit data diagnostických nástrojů pro Azure virtuálního počítače. V seznamu nainstalovaný rozšíření zvolte rozevírací nabídka dostupné rozšíření a klikněte na Microsoft sledování Agent diagnostiky.

    ![Instalace rozšíření Azure virtuálního počítače](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Další diagnostiky rozšíření jsou k dispozici pro virtuálních počítačích. Další informace najdete v tématu Azure OM rozšíření a funkce.

1. Klikněte na tlačítko **Přidat** můžete přidat rozšíření a příslušné dialogové okno **Konfigurace diagnostiky** zobrazit.

1. Klikněte na tlačítko **Konfigurace** můžete zadat úložiště účtu a potom klikněte na tlačítko **OK** .

    Jednotlivých kartách (s výjimkou **Obecné** a **Adresáře protokolu**) představuje zdroje diagnostiky dat, který můžete shromažďovat.

    ![Povolení Azure diagnostiky a konfigurace](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Na výchozí kartu **Obecné**, nabízí následující možnosti získávání dat diagnostiky: **pouze chyby**, **všechny informace**a **vlastní plán**. Výchozí možnost **pouze chyby**, trvá nejnižších velikost úložiště, protože nemá převádět upozornění nebo sledování zpráv. Možnost **všechny informace o** přenáší většina informací a je proto většina drahé možnost z hlediska úložiště.

1. V tomto příkladu přepínač **vlastní plán** , je možné upravit dat shromážděných.

1. Pole **Disková kvóta MB** určuje zbývajícího prostoru, který se má přidělit ve vašem účtu úložiště pro diagnostiky data. Pokud chcete, můžete změnit výchozí hodnota.

1. Na jednotlivých kartách diagnostiky dat chcete shromažďovat, vyberte jeho **Povolit převod <log type> ** zaškrtávací políčko.

    Například pokud chcete shromáždit protokoly aplikace, zaškrtněte políčko **Povolit převod protokoly aplikace** na kartě **Protokoly aplikace** . Také zadejte všechny ostatní informace požadované datové typy jednotlivých diagnostických nástrojů. Naleznete v části **zdroje dat diagnostiky konfigurovat** dál v tomto tématu informace o konfiguraci na jednotlivých kartách.

1. Po povolení kolekce všechna data diagnostických nástrojů, který chcete klikněte na tlačítko **OK** .

1. Uložte aktualizovaný projekt.

    Zobrazí se zpráva v okně **Microsoft Azure aktivity protokol** aktualizovala virtuální počítač.

## <a name="configure-diagnostics-data-sources"></a>Konfigurace diagnostiky zdroje dat

Po povolení shromažďování dat diagnostiky můžete přesně zdrojů dat, která chcete shromažďovat a jaké informace se shromažďují. Následuje seznam záložek v dialogovém okně **Konfigurace diagnostiky** a jaké jednotlivých možností konfigurace znamená.

### <a name="application-logs"></a>Protokoly aplikace

**Protokoly aplikace** obsahovat diagnostické informace vytvořené pomocí webové aplikace. Pokud chcete zachytit protokoly aplikací, zaškrtněte políčko **Povolit převod protokoly aplikace** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly aplikace ke svému účtu úložiště změnou hodnoty **Transfer období (min)** . Můžete taky změnit množství informací zachyceny v protokolu nastavení úrovně hodnoty protokolu. Můžete **podrobné** k získání dalších informací nebo vyberte **položku kritické** k zaznamenání jenom kritické chyby. Pokud máte poskytovatele konkrétní diagnostických nástrojů, který posílá protokoly aplikace, můžete je zachytit přidáním GUID poskytovatele do pole **Identifikátor GUID** .

  ![Protokoly aplikace](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Další informace o protokolech aplikací najdete v článku [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure](./app-service-web/web-sites-enable-diagnostic-log.md) .

### <a name="windows-event-logs"></a>Protokoly událostí systému Windows

Pokud chcete zachytit protokoly událostí Windows, zaškrtněte políčko **Povolit převod protokoly událostí systému Windows** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly událostí ke svému účtu úložiště změnou hodnoty **Transfer období (min)** . Zaškrtněte políčka u typů události, které chcete sledovat.

  ![Protokoly událostí](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Pokud používáte Azure SDK 2.6 nebo novější a chcete zadat uživatelského zdroje dat, zadejte ho v **<Data source name>** textové pole a potom klikněte na tlačítko **Přidat** vedle ní. Zdroje dat se přidá k souboru diagnostics.cfcfg.

Pokud používáte Azure SDK 2,5 a chcete zadat vlastní datové zdroje, můžete ji přidat `WindowsEventLog` část diagnostics.wadcfgx souborů, například jako v následujícím příkladu.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Výkonnosti

Informace výpočtového výkonu můžete vyhledat problematické oblasti systému a optimalizovat výkon systému a aplikací. Další informace naleznete v tématu [Vytvoření a použití výkonnosti v aplikaci Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Pokud chcete zachytit výkonnosti, zaškrtněte políčko **Povolit převod výkonnosti** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly událostí ke svému účtu úložiště změnou hodnoty **Transfer období (min)** . Zaškrtněte políčka u výkonnosti, které chcete sledovat.

  ![Výkonnosti](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Ke sledování výkonu čítač, který není uvedený, zadejte pomocí navrhované syntaxe a potom klikněte na tlačítko **Přidat** . Operační systém ve počítače virtuální Určuje, které výkonnosti můžete sledovat. Další informace o syntaxi naleznete v tématu [Nastavení dráhy čítač](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastruktura protokoly

Pokud chcete zachytit infrastruktury protokolů, které obsahují informace o Azure diagnostiky infrastruktury, modulu vzdáleného přístupu a modulu RemoteForwarder, zaškrtněte políčko **Povolit převod infrastruktury protokoly** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly ke svému účtu úložiště změnou hodnoty Transfer období (min).

  ![Protokolování diagnostiky infrastruktury](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Další informace najdete v tématu [Shromáždit protokolování údajů pomocí diagnostických nástrojů Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Protokol adresáře

Pokud chcete zachytit protokolu adresářů, které obsahují data shromážděná z adresáře protokolu pro žádosti o služby (internetové), se nepodařilo žádosti o nebo složky, které jste si vybrali, zaškrtněte políčko **Povolit převod protokolu adresářů** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly ke svému účtu úložiště změnou hodnoty **Přepojit období (min)** .

Můžete vybrat pole, která chcete shromažďovat, například **Služby IIS protokoly** a **Se nepodařilo požádat o** protokoly. Výchozí úložiště kontejneru názvy jsou k dispozici, ale pokud chcete, můžete změnit názvy.

Také můžete shromáždit protokoly z libovolné složky. Stačí zadat cestu v oddílu **hovoru z absolutní adresáře** a potom klikněte na tlačítko **Přidat adresář** . Pro zadaný kontejnery budou zachyceny protokoly.

  ![Protokol adresáře](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Protokoly trasování událostí pro Windows

Pokud používáte [Trasování událostí pro systém Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (trasování událostí pro Windows) a chcete zachytit protokoly trasování událostí pro Windows, zaškrtněte políčko **Povolit převod protokoly trasování událostí pro Windows** . Můžete zvětšit nebo zmenšit počet minut, kdy se převádí protokoly ke svému účtu úložiště změnou hodnoty **Přepojit období (min)** .

Události jsou zachyceny ze zdroje událostí a manifesty události, které zadáte. Chcete-li zdroj události, zadejte název v části **Události zdroje** a potom klikněte na tlačítko **Přidat zdroj události** . Podobně můžete zadat manifest událostí v části **Události manifesty** a klikněte na tlačítko **Přidat Manifest události** .

  ![Protokoly trasování událostí pro Windows](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Rozhraní trasování událostí pro Windows je podporované v ASP.NET prostřednictvím tříd v [System.Diagnostics.aspx] (obor názvů https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Obor názvů Microsoft.WindowsAzure.Diagnostics, které dědí z a rozšiřují standardní [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) třídy, umožňuje použití informačních [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) jako rámec protokolování v Azure prostředí. Další informace najdete v tématu [převzít řízení z zapnout protokolování a sledování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Výpisy

Pokud chcete zachytit zjistit, kdy dojde k chybě instanci rolí, zaškrtněte políčko **Povolit převod selhat vypíše** . (Protože ASP.NET má na starosti většina výjimek, tady je obecně užitečné pouze pro kolegy role.) Můžete zvýšit nebo snížit procento úložného prostoru věnované výpisy změnou hodnoty **Adresáře kvóty (%)** . Můžete změnit kontejneru úložiště jsou uložené výpisy, kde můžete určit, zda chcete zachytit výpis **úplné** nebo **Mini** .

Jsou uvedeny procesy aktuálně sledované. Zaškrtněte políčka u procesů, které chcete zachytit. Chcete-li přidat jiným procesem do seznamu, zadejte název obrázku a potom klikněte na tlačítko **Přidat obrázku** .

  ![Výpisy](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Přečtěte si téma [převzít řízení z zapnout protokolování a sledování v Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) a [Microsoft Azure diagnostiky část 4: vlastní protokolování součásti a Azure diagnostiky 1.3 změny](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) Další informace.

## <a name="view-the-diagnostics-data"></a>Zobrazení dat diagnostických nástrojů

Když získaných diagnostiky data do cloudové služby nebo virtuálního počítače, můžete ho zobrazit.

### <a name="to-view-cloud-service-diagnostics-data"></a>Chcete-li zobrazit data diagnostiky služby cloudu

1. Nasazení vaše cloudové služby, jako obvykle a pak ho spusťte.

1. Diagnostika data můžete zobrazit v grafu generovaný Visual Studio nebo tabulky ve vašem účtu úložiště. Data v sestavě zobrazíte otevřete **Průzkumníka cloudu** nebo **Průzkumník serveru**, otevřete v místní nabídce na uzel pro roli, která vás zajímá a pak zvolte **Zobrazit diagnostické Data**.

    ![Zobrazení diagnostických nástrojů dat](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Zobrazí se zpráva obsahujícího data k dispozici.

    ![Sestava diagnostických nástrojů Microsoft Azure ve Visual Studiu](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Pokud se nezobrazuje nejnovější data, může být potřeba počkat v období uplynout.

    Zvolte odkaz **aktualizace** okamžitě aktualizujte data nebo si vyberte interval **Automatické aktualizace** rozevíracího seznamu zobrazit data automaticky aktualizován. Export dat chyby, klikněte na tlačítko **Exportovat do souboru CSV** vytvořit soubor CSV, který můžete otevřít v tabulce.

    V **Průzkumníkovi cloudu** nebo **Průzkumník serveru**si potřebujete založit účet úložiště, který máte přidružený k nasazení.

1. Otevřete Diagnostika tabulek v prohlížeči tabulky a pak si data, která se shromažďují. Protokoly služby IIS a vlastní můžete po otevření kontejneru objektů blob. Kontrolou v následující tabulce najdete tabulku nebo objektů blob kontejner obsahující data, která vás zajímá. Kromě data pro tento soubor protokolu tabulky položky obsahují EventTickCount DeploymentId, Role a RoleInstance vám pomůže identifikovat, jaká virtuální počítač a rolí generovaného data a kdy. 

  	|Diagnostické dat|Popis|Umístění|
  	|---|---|---|
  	|Protokoly aplikace|Protokoly generované kódu tak, že zavoláte metody třídy System.Diagnostics.Trace.|WADLogsTable|
  	|Protokoly událostí|Tato data jsou z protokoly událostí Windows na virtuálních počítačích. Systém Windows ukládá informace v těchto protokoly, ale aplikací a služeb také používat k chyb sestav nebo protokolování informací.|WADWindowsEventLogsTable|
  	|Výkonnosti|Shromáždit data na libovolnou čítače výkonu, který je k dispozici v počítači virtuální. Operační systém poskytuje výkonnosti, které zahrnují hodně statistiky například časového využití a procesor paměti.|WADPerformanceCountersTable|
  	|Infrastruktura protokoly|Tyto protokoly se vytvářejí z infrastruktury diagnostiky sebe sama.|WADDiagnosticInfrastructureLogsTable|
  	|Protokoly služby IIS|Tyto protokoly záznam požadavky na web. Cloudová služba získá značné množství přenosů, tyto protokoly může být docela delší dobu, by měl shromažďovat a ukládat tato data pouze v případě, že budete potřebovat.|Můžete najít, že se nepodařilo žádost protokoly do kontejneru objektů blob v rámci služby iis failedreqlogs wad v části cesty pro tuto nasazení rolí a instance. Dokončení protokolů ve skupinovém rámečku wad iis logfiles můžete najít. V tabulce WADDirectories jsou vytvořeny položky pro každý soubor.|
  	|Výpisy|Tyto informace obsahuje binární obrázky cloudové služby procesu (obvykle role pracovníka).|kulatý wad deformační výpisy kontejneru|
  	|Vlastní protokoly|Protokoly data, která jste předdefinované.|Můžete použít v kódu umístění vlastní protokoly ve vašem účtu úložiště. Například můžete zadat vlastní objektů blob kontejneru.|

1. Pokud je zkrácen libovolný typ dat, můžete zkusit zvětšení vyrovnávací paměť pro příslušná data typu nebo zkrácení interval mezi přenosy dat z virtuálního počítače k účtu úložiště.

1. (volitelné) Vymazání dat z účtu úložiště občas ke snížení celkové náklady na úložiště.

1. Po provedení úplného nasazení diagnostics.cscfg souboru (.wadcfgx pro Azure SDK 2,5) se aktualizuje v Azure a cloudové služby vyzvedne změny nastavení diagnostických nástrojů. Pokud, místo toho aktualizujete stávajícího nasazení, soubor .cscfg neaktualizují v Azure. Pořád změníte nastavení diagnostických nástrojů však podle postupu v následující části. Další informace o provedení úplného nasazení a aktualizace stávajícího nasazení najdete v tématu [Publikování průvodce aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Zobrazení dat diagnostiky virtuálního počítače

1. V místní nabídce pro virtuální počítač zvolte **Zobrazit diagnostiky Data**.

    ![Zobrazení diagnostických nástrojů dat v Azure virtuálního počítače](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Otevře se okno **diagnostických nástrojů Souhrn** .

    ![Souhrnné diagnostických nástrojů Azure virtuálního počítače](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Pokud se nezobrazuje nejnovější data, může být potřeba počkat v období uplynout.

    Zvolte odkaz **aktualizace** okamžitě aktualizujte data nebo si vyberte interval **Automatické aktualizace** rozevíracího seznamu zobrazit data automaticky aktualizován. Export dat chyby, klikněte na tlačítko **Exportovat do souboru CSV** vytvořit soubor CSV, který můžete otevřít v tabulce.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Konfigurace cloudové služby diagnostiky po nasazení

Pokud jste řešení problému s obláčkem služby, které už spuštěný, možná budete chtít shromáždit data, která jste nezadali před původně nasazením roli. V tomto případě začnete můžete získat informace o těchto dat pomocí nastavení v okně Průzkumník serveru. Konfigurace diagnostických nástrojů pro jedna instance nebo všechny instance v roli, podle toho, zda otevřete dialogové okno Konfigurace diagnostiky z místní nabídky pro instance nebo roli. Pokud jste nakonfigurovali uzel role, všechny změny vliv na všechny instance. Pokud jste nakonfigurovali uzel instance, změny se použijí jenom vybraný výskyt.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Konfigurace Diagnostika pracovního cloudové služby

1. V okně Průzkumník serveru rozbalte uzel **Cloudovým službám** a potom rozbalte položku uzly vyhledejte roli nebo instanci, kterou chcete prozkoumat nebo obojí.

    ![Konfigurace diagnostických nástrojů](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. V místní nabídce pro instance uzel nebo uzel role zvolte **Nastavení diagnostických nástrojů aktualizace**a vyberte diagnostické nastavení, která chcete shromažďovat.

    Informace o nastavení najdete v článku **zdroje dat konfigurace Diagnostika** v tomto tématu. Informace o tom, jak zobrazit data diagnostiky najdete v článku **zobrazení diagnostiky data** v tomto tématu.

    Pokud změníte shromažďování dat v okně **Průzkumník serveru**, tyto změny budou nadále platit plně přeinstalujte cloudové služby, dokud. Pokud použijete výchozí nastavení publikování nejsou přepíšou změny, protože publikovat výchozí nastavení je aktualizaci stávajícího nasazení, spíše než udělat úplné nové nasazení. Abyste měli jistotu, že nastavení zrušte při nasazení, přejděte na kartu **Upřesnit nastavení** v Průvodci publikovat a zrušte zaškrtnutí políčka **Aktualizovat nasazení** . Když přeinstalujte pomocí tohoto zaškrtávacího políčka zrušeno nastavení vrátit odkazy v souboru .wadcfgx (nebo .wadcfg) jako sadu pomocí editoru Vlastnosti rolí. Pokud aktualizujete nasazení, bude Azure původní nastavení.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Poradce při potížích s problémy se službou Azure cloudu

Pokud narazíte na problémy s projektů cloudové služby, například roli zasekne ve "něčem pracuje" stav opakovaně recykluje nebo vyvolá chyba interním serveru, mají nástroje a postupů, které můžete použít k Diagnostika a oprava těchto problémů. Konkrétní příklady běžných problémů a řešení, jakož i přehled koncepty a nástroje pro Diagnostika a oprava tyto chyby najdete v článku [Azure PaaS výpočet diagnostiky Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Služba Q & A

**Co je vyrovnávací paměť a jak velká je třeba?**

Pro každou instanci virtuálního počítače kvóty omezit množství dat diagnostiky mohou být uloženy na místním pevném disku. Kromě toho je zadat vyrovnávací paměť o velikosti u jednotlivých typů diagnostiky data, která jsou dostupná. Tento vyrovnávací paměť funguje jako jednotlivé kvóty pro tento typ dat. Zaškrtnutím dolní části dialogového okna můžete zjistit celkový kvóty a velikosti paměti zůstane. Pokud chcete zadat větší rezerv nebo další typy dat, budete přiblíží celkové kvóty. Celková kvóty můžete změnit úpravou konfiguračního souboru diagnostics.wadcfg/.wadcfgx. Diagnostika data se ukládají na stejné souborů jako data aplikace, pokud vaše aplikace používá hodně místa na disku, neměli zvýšit celkové kvóty diagnostických nástrojů.

**Co je v období a jak dlouho je třeba?**

V období je množství času, který popisuje uplyne mezi data. Po každých přenos dat se přesune z místního systému souborů v počítači virtuální do tabulek ve vašem účtu úložiště. Pokud výši data, která se shromažďují překročí kvóty před koncem období přenos, starší data odstraněna. Můžete chtít snížit v období, pokud jste ztráty dat, protože data překračuje vyrovnávací paměť nebo celkové kvóty.

**Časové pásmo jsou časová razítka v?**

V místní časové pásmo datovém centru, který je hostitelem cloudové služby jsou časových razítek. Se používají následující tři sloupce časového razítka do tabulky protokolu.

  - **PreciseTimeStamp** je časové razítko trasování událostí pro Windows události. To znamená čas zaznamenané událost z klienta.

  - **Časové razítko** je PreciseTimeStamp zaokrouhleno dolů na hranici četnosti odesílání. Ano Pokud četnost nahrát 5 minut a událost čas 00:17:12, časové razítko budou 00:15:00.

  - **Časové razítko** je časové razítko niž vytvoření entity v tabulce Azure.

**Jak se při shromažďování diagnostické informace řízení nákladů?**

Výchozí nastavení (**úroveň protokolování** **chyb** a **přenos období** nastavit **1**minutu) slouží k minimalizaci náklady. Náklady výpočetním zvýší v případě shromáždění dalších diagnostických dat nebo zmenšit v období. Není shromáždit více dat, než je to potřeba a nezapomeňte zakázat shromažďování dat, když už nepotřebujete. Můžete vždy jej znovu zapnout, i za běhu, jak je vidět v předchozí části.

**Jak shromáždit protokoly se nepodařilo žádosti o služby IIS**

Ve výchozím nastavení není IIS shromáždit protokoly se nepodařilo žádost. Můžete nakonfigurovat IIS získat informace o jejich úpravy nastavení(Web.config)) pro váš web roli.

**Nezobrazují se sledování informací z RoleEntryPoint metody jako při spuštění. Co je?**

Metody RoleEntryPoint nazývají v kontextu WAIISHost.exe, ne IIS. Proto informace o konfiguraci v web.config, který obvykle umožňuje trasování neplatí. Tento problém můžete vyřešit, přidání config souboru projektu role web a název souboru, aby odpovídaly sestavení, která obsahuje kód RoleEntryPoint. V aplikaci project výchozí web role název souboru config by měl být WAIISHost.exe.config. Potom přidejte následující řádky na tento soubor:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Teď v okně **vlastností** nastavte vlastnost **Kopírovat do adresáře výstup** kopii **vždy**.

## <a name="next-steps"></a>Další kroky

Další informace o protokolování v Azure diagnostických nástrojů, najdete v článku [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](./cloud-services/cloud-services-dotnet-diagnostics.md) a [Povolit protokolování pro web apps v aplikaci služby Azure diagnostických nástrojů](./app-service-web/web-sites-enable-diagnostic-log.md).
