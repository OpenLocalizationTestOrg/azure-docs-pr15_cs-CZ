<properties 
    pageTitle="Jak sledovat do cloudové služby | Microsoft Azure" 
    description="Zjistěte, jak sledovat cloudové služby na portálu Azure klasické." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Jak sledovat Cloudovým službám

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Můžete sledovat `key` měřítka pro cloudové služby Azure klasické portálu. Můžete nastavit úroveň sledování a minimální podrobného pro každou roli služby a můžete přizpůsobit zobrazení sledování. Podrobný sledování data se ukládají v úložišti účtu, který se dostanete mimo na portálu. 

Sledování se zobrazí v portálu Azure klasické jsou snadnému. Můžete zvolit, které chcete sledovat v seznamu metriky na stránce **Sledování** metriky a můžete si vybrat které metriky vykreslování hodnot v průběhu v grafech metriky na stránce **Sledování** a na řídicím panelu. 

## <a name="concepts"></a>Koncepty

Ve výchozím nastavení je minimální sledování vynechaný nové služby cloudu pomocí výkonnosti shromážděné z hostitelského operačního systému instancí role (virtuálních počítačích). Minimální metriky se omezuje procento využití procesoru, dat v, Data, výkon disku pro čtení a psaní výkon disku. Nakonfigurováním podrobného sledování nebudete dostávat další metriky podle data o výkonu ve virtuálních počítačích (role instance). Podrobný metriky povolit bližší analýzu problémů, ke kterým dochází při operacích aplikace.

Ve výchozím nastavení je výkon data z role instance odběr a převést z instanci rolí intervalech 3 minuty. Po povolení podrobného sledování data čítačů nezpracovanými výkonu je agregované pro každou roli instanci a všech instancí role pro každou roli intervalech 5 minut, 1 hodinu a 12 hodin. Souhrnná data odstraněna po 10 dnů.

Po povolení podrobného sledování agregovaná sledování data uložená v tabulkách ve vašem účtu úložiště. Povolit podrobného sledování role, musíte nakonfigurovat připojovací řetězec diagnostiky propojující účtu úložiště. Různé úložiště účtů můžete použít pro různé role.

Všimněte si, že povolení podrobného sledování zvýší úložiště náklady spojené s ukládání dat, přenos dat a transakce úložiště. Minimální sledování nevyžaduje účet úložiště. Data pro metriky, které jsou umístěné na minimální sledování úrovni nejsou uloženy v účtu úložiště, i když nastavíte sledování úroveň podrobného.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Jak: konfigurace sledování cloudovým službám

Následující postupy používá ke konfiguraci podrobného nebo minimální sledování Azure klasické portálu. 

### <a name="before-you-begin"></a>Než začnete

- Vytvoření účtu úložiště pro uložení údajů. Různé úložiště účtů můžete použít pro různé role. Další informace najdete v tématu Nápověda k **Úložiště účty**nebo zjistěte, [jak vytvořit účet úložiště](/manage/services/storage/how-to-create-a-storage-account/).

- Povolte Azure Diagnostika pro vaše role služby cloudu. V tématu [Konfigurace diagnostických nástrojů pro Cloudovým službám](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Zajistěte, aby byl připojovací řetězec diagnostiky prezentovat v konfiguraci Role. Nemůžete zapnout podrobného sledování tak, aby povolit Azure Diagnostika a zahrnout připojovací řetězec Diagnostika v konfiguraci Role.   

> [AZURE.NOTE] Projekty zacílení Azure SDK 2,5 automaticky neobsahuje připojovací řetězec Diagnostika v šabloně projektu. U těchto projektů potřebujete diagnostiky připojovací řetězec ručně přidat do konfigurace Role.

**Diagnostika připojovací řetězec ručně přidat do konfigurace rolí**

1. Otevřete projekt Cloudová služba ve Visual Studiu
2. Dvojitým kliknutím na **Role** otevřete návrháře rolí a klikněte na kartu **Nastavení**
3. Najděte nastavení s názvem **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Pokud toto nastavení není k dispozici klikněte na tlačítko **Přidat nastavení** přidat ke konfiguraci a změňte typ nové nastavení na **ConnectionString**
5. Nastavte hodnotu pro připojovací řetězec kliknutím na tlačítko **...** . Otevře se dialog umožňuje vybrat účet úložiště.

    ![Nastavení aplikace Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Chcete-li změnit úroveň sledování podrobného nebo minimální

1. [Azure klasické portál](https://manage.windowsazure.com/)otevřete stránku **Konfigurovat** pro nasazení služby cloudu.

2. **Úroveň**klikněte na **úplné** nebo **minimální**. 

3. Klikněte na **Uložit**.

Po zapnutí sledování podrobného by měly začít zobrazují monitorování dat na portálu Azure klasické do jedné hodiny.

Jako nezpracovaná data čítačů a agregovaná sledování data jsou uloženy v účtu úložiště v tabulkách kvalifikované podle ID nasazení role. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Postup: výstrahy pro metriky služby cloudu

Můžete jim zobrazovat upozornění založené na vaše cloudové služby, sledování metriky. Na stránce **Správa přístupových** portálu Azure klasické můžete vytvořit pravidlo, které výstrahu míru, jaká jste dosažení hodnota, kterou zadáte. Můžete taky mají e-maily odeslané při spuštění výstrahy. Další informace najdete v tématu [jak: dostávat oznámení a výstrahy pravidla v Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Postup: Přidání metriky metriky tabulky

1. [Azure klasické portál](http://manage.windowsazure.com/)otevřete stránku **Monitor** cloudovou službu.

    Ve výchozím nastavení zobrazuje tabulce metriky podmnožiny dostupné metriky. Podrobný metriky výchozí pro do cloudové služby, které se omezí na výkon čítači Paměť\Dostupné MB s daty agregované na úrovni role vidíte na obrázku. Vyberte další agregační a úrovni role metriky ke sledování na portálu Azure klasické pomocí **Metriky přidat** .

    ![Podrobné zobrazení](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Chcete-li přidat metriky metriky tabulky:

    1. Klikněte na **Přidat metriky** otevřete **Zvolte metriky**, vidíte na obrázku.

        První dostupné míru rozbalit a zobrazit možnosti, které jsou k dispozici. Každý metriky zobrazuje možnost začátek agregované sledování data pro všechny role. Kromě toho můžete jednotlivé role zobrazíte data.

        ![Přidání metriky](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Chcete-li vybrat metriky zobrazíte

        - Klikněte na šipku dolů tak, že metriky rozbalte možnosti sledování.
        - Zaškrtněte políčko u každé sledování možnosti, které chcete zobrazit.

        Až 50 metriky můžete zobrazit v tabulce metriky.

        > [AZURE.TIP] Podrobný sledování metriky seznam může obsahovat desítky metriky. Pokud chcete zobrazit posuvník, najeďte myší na pravé straně dialogového okna. Filtrování seznamu, klikněte na ikonu hledání a do pole Hledat zadejte text, jak je ukázáno v následujícím příkladu.
    
        ![Přidání metriky hledání](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Po dokončení výběru metriky, klikněte na tlačítko OK (zaškrtnuté).

    Vybrané metriky jsou přidané do tabulky metriky, jak je ukázáno v následujícím příkladu.

    ![metriky monitoru](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Z tabulky metriky odstranit metriky, klikněte na míru ho vyberte a pak klikněte na **Odstranit míru**. (Zobrazí jenom **Odstranit míru** obsahujících metriky vybraný.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Chcete-li přidat vlastní metriky metriky tabulky

**Podrobné** sledování úroveň obsahuje seznam výchozí metriky, která můžete sledovat na portálu. Kromě těchto můžete sledovat všechny vlastní metriky nebo výkonnosti definovány aplikací prostřednictvím portálu.

Následující kroky předpokládají jste zapnuli **podrobné** sledování úroveň a mít konfiguraci aplikace shromáždit a převést vlastní výkonnosti. 

Chcete-li zobrazit vlastní výkonnosti na portálu potřebujete aktualizovat konfigurace v kontejneru wad ovládacího prvku:
 
1. Otevřete objektů blob kontejneru wad ovládacího prvku ve vašem účtu úložiště diagnostických nástrojů. K tomuto můžete Visual Studio nebo jiných explorer úložiště.

    ![Průzkumník serveru Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Přejděte cesta objektů blob pomocí vzorku **DeploymentId/RoleName/RoleInstance** konfiguraci instanci rolí. 

    ![Průzkumník úložišť Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Stáhněte si konfiguračního souboru pro instanci rolí a aktualizovat tak, aby obsahovala všechny vlastní výkonnosti. Příklad ke sledování *zápisu disku bajty/s* *jednotku C:* přidejte následující text uzlu **PerformanceCounters\Subscriptions**

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Uložte změny a nahrajte soubor konfigurace zpět do stejného umístění přepsání existujícího souboru v objektů blob.
5. Způsobí přepnutí do režimu s komentářem v Azure klasické portálu konfiguraci. Pokud jste v režimu s komentářem už budete muset přepnout minimální a zpět podrobného.
6. Vlastní čítače teď budou k dispozici v dialogovém okně **Přidat metriky** . 

## <a name="how-to-customize-the-metrics-chart"></a>Jak se: přizpůsobení metriky grafu

1. V tabulce metriky vyberte až 6 metriky vykreslování hodnot v průběhu v diagramu metriky. Chcete-li vybrat metriky, zaškrtněte políčko na levé straně. Metriky odebrat z grafu metriky, zrušte zaškrtnutí políčka v tabulce metriky.

    Jakmile vyberete metriky v tabulce metriky, metriky se přidají do grafu metriky. Úzký zobrazení rozevíracího seznamu **n další** obsahuje metrických záhlaví, než se vejde zobrazení.

 
2. Pokud chcete přepnout mezi zobrazením relativních hodnot (konečná hodnota jenom pro každou míru) a absolutní hodnoty (osy Y zobrazí), vyberte relativní a absolutní v horní části grafu.

    ![Relativní a absolutní](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Chcete-li změnit časový rozsah graf zobrazuje metriky vyberte hodnotu 1 hodina, 24 hodin nebo 7 dní v horní části grafu.

    ![Sledování zobrazení období](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    V diagramu metriky řídicích panelů se liší metodu pro zakreslení metriky. Standardní sady metriky je k dispozici a metriky po přidání nebo odebrání tak, že vyberete metrických záhlaví.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Můžete přizpůsobit graf metriky na řídicím panelu

1. Otevřete řídicích panelů pro službu cloudu.

2. Přidání nebo odebrání metriky grafu:

    - Vykreslování hodnot v průběhu nové metriky, zaškrtněte políčko metriky v záhlavích grafu. Na úzký zobrazení klikněte na šipku dolů tak, že ** *n*??metrics** vykreslování hodnot v průběhu metriky, které nelze zobrazit do oblasti záhlaví grafu.

    - Pokud chcete odstranit metriky, která jsou zobrazena v grafu, zrušte zaškrtnutí políčka tak, že jeho záhlaví.

3. Přepínání mezi **relativní** a **absolutní** zobrazí.

4. Klikněte na 1 hodinu, 24 hodin nebo 7 dnů od data, která chcete zobrazit.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Postup: Access podrobného monitorování dat mimo Azure klasické portálu

Podrobný sledování data uložená v tabulkách v okně úložiště účty, které zadáte pro každou roli. Pro každý nasazení služby cloudu šest tabulky vytvořené pro roli. Dvě tabulky vytvořené pro jednotlivá pole (1 hodina a 12 hodin 5 minut). Některé z těchto tabulek ukládá úrovni role agregace; v tabulce ukládá agregace instancí role. 

Názvy tabulek s formátem takto:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

kde je:

- *deploymentID* je GUID přiřazené k nasazení služby cloudu

- *aggregation_interval* = 5 M, 1 H nebo 12 H

- agregace úrovni role = R

- agregace role instancí = RI

Například v následujících tabulkách by uložení podrobného sledování agregované na 1 hodinu intervalů:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
