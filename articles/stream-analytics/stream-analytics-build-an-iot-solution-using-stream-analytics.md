<properties
    pageTitle="Vytvoření řešení IoT pomocí analýzy toku | Microsoft Azure"
    description="Začínáme – kurz toku analýzy IoT řešení scénáři mýtná celnice"
    keywords="IOT řešení, funkce okna"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Vytvoření řešení IoT pomocí analýzy toku

## <a name="introduction"></a>Úvod

V tomto kurzu se dozvíte, jak používat Azure toku technologie pro analýzu a dozvědět se víc v reálném čase z dat. Vývojáři můžete snadno kombinovat datových proudů dat, například datových proudů klikněte na protokoly a události generované zařízení s historickými záznamů nebo referenčních dat pro přehledy business. Jako služba výpočtu plně spravované, v reálném čase toku, který hostuje Microsoft Azure analýzy toku Azure poskytuje předdefinované odolnost proti chybám, zhoršeným latence a škálovatelnost zobrazíte můžete nahoru a systémem v minutách.

Po dokončení tohoto kurzu, budou moct:

-   Seznamte se s portálem Azure toku analýzy.
-   Konfigurace a nasazení streamování úlohy.
-   Srozumitelně vysvětlit praktických problémů a řešení pomocí jazyka dotazu toku analýzy.
-   Můžete vyvíjet streamování řešení pro zákazníky pomocí analýzy toku spolehlivé.
-   Pomocí sledování a protokolování experience Poradce při potížích.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Následující požadavky pro dokončení tohoto kurzu budete potřebovat:

-   Nejnovější verzi [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio 2015 nebo bezplatné [Aplikace Visual Studio komunity](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   [Azure předplatného](https://azure.microsoft.com/pricing/free-trial/)
-   Oprávnění správce v počítači
-   Stažení [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) ze služby Stažení softwaru
-   Volitelné: Zdrojového kódu generátor události TollApp v [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scénář Úvod: "Ahoj, placená!"


Placená stanice je běžné jevu. Setkáte je na mnoha rychlostních mosty a tunelů po celém světě. Každá placená stanice obsahuje více kabin placená. Při ručním kabin ukončíte čísla placených zaplatit attendant. Při automatické kabin kontroluje senzoru horní části každé stánku RFID kartu, která je opatřit windshield vaše vozidla jako předáte stánku placená. Není těžké si vizualizace průchod vozidla prostřednictvím těchto placená stanic jako události toku, přes který zajímavé operace může dělat.

![Obrázek automobilů na placená kabin](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Příchozí data

Tento kurz pracuje se dvěma datovými proudy dat. Nainstalovaných v úvodní a závěrečné stanic placená plodiny první proudu. Druhá proudu je statické vyhledávání sady dat, která obsahuje vozidla registračních údajů.

### <a name="entry-data-stream"></a>Položka datového proudu

Položka datového proudu obsahuje informace o automobilů zadávání placená stanic.


| TollID | EntryTime               | LicensePlate | Stav | Zkontrolujte   | Model   | VehicleType | VehicleWeight | Placená | Značka       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 12:01:00.000 2014-09-10 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 12:02:00.000 2014-09-10 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 12:02:00.000 2014-09-10 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 12:03:00.000 2014-09-10 | XYZ 1003     | CT    | Toyota | Koruny | 1           | 0             | 4    |           |
| 1      | 12:03:00.000 2014-09-10 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 12:05:00.000 2014-09-10 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Tady je stručný popis sloupce:

| Sloupec | Popis |
|--------------|--------------------------------------------------------------------|
| TollID       | ID stánku placená, které jednoznačně identifikuje placená stánku            |
| EntryTime    | Datum a čas přijetí vozidla stánku placená UTC |
| LicensePlate | Registrační počet vozidla                            |
| Stav        | Stav v USA                                           |
| Zkontrolujte         | Výrobce Auto                                 |
| Model        | Číslo modelu Auto                                 |
| VehicleType  | 1 pro osobní vozidla nebo 2 pro komerční vozidla       |
| WeightType   | Hmotnost vozidla v t; 0 osobních automobilů                   |
| Placená         | Placená hodnota v USD                                              |
| Značka          | Na e značku Auto umožňuje automatizovat platby; prázdné místo, kam ručně platné platby |


### <a name="exit-data-stream"></a>Ukončení toku dat

Konec datového proudu obsahuje informace o automobilů necháte stanice placená.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | CDE 1007         |

Tady je stručný popis sloupce:


| Sloupec | Popis |
|--------------|-----------------------------------------------------------------|
| TollID       | ID stánku placená, které jednoznačně identifikuje placená stánku         |
| ExitTime     | Datum a čas ukončení vozidla placená stánku UTC |
| LicensePlate | Registrační počet vozidla                         |

### <a name="commercial-vehicle-registration-data"></a>Obchodní vozidla registračních údajů

Kurz využívá statický snímek obchodní vozidla registrační databázi.


| LicensePlate | Atributy RegistrationId | Vypršela platnost |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| BAC 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Tady je stručný popis sloupce:


| Sloupec | Popis |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Registrační počet vozidla                         |
| Atributy RegistrationId     | ID registrace vozidla |
| Vypršela platnost | Stav registrace vozidla: 0, pokud je aktivní, vozidla registrace 1, pokud vypršela registrace                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Nastavení prostředí pro službu Azure toku Analytics

Tento kurz je třeba Microsoft Azure předplatné. Společnost Microsoft nabízí bezplatnou zkušební verzi pro služby Microsoft Azure.

Pokud nemáte účet Azure, můžete to udělat [žádost o bezplatnou zkušební verzi](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Zaregistrovat bezplatnou zkušební verzi, budete potřebovat mobilní zařízení, který může přijímat textové zprávy a platnou kreditní karty.

Ujistěte se, že tak, aby se můžete nejlépe využijete 200 $ uvolnit Azure platební postupujte podle kroků uvedených v části "Vyčistit účet Azure" na konci tohoto článku.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Zřízení Azure prostředky potřebné pro kurz

Tento kurz vyžaduje dva rozbočovače událostí pro příjem datových proudů *položky* a *zavřete* . Databáze SQL Azure zobrazí výsledky analýzy toku úlohy. Azure jsou uloženy referenčních dat o registracích vozidla.

Skript Setup.ps1 ve složce TollApp na GitHub slouží k vytvoření všechny požadované prostředky. V zájmu čas doporučujeme vám, abyste ho spusťte. Pokud chcete další informace o tom, jak nakonfigurovat tyto materiály na portálu Azure, podívejte se do dodatku "Konfigurace výuková prostředků Azure portálu".

Stáhněte si a uložte podpůrné [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) složek a souborů.

Otevřete **Microsoft Azure PowerShell** okno _jako správce_. Pokud ještě nemáte Azure Powershellu, postupujte podle pokynů v [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) případně ji nainstalujte.

Protože Windows automaticky blokuje .ps1, DLL soubory a .exe, budete muset nastavit zásady spuštění před spuštěním skriptu. Zkontrolujte, jestli že okno Azure PowerShell běží _jako správce_. Spusťte **neomezený zásady nastavení spouštění**. Po zobrazení výzvy zadejte **Y**.

![Snímek obrazovky s "Set-zásady spouštění neomezený" spuštěné v okně Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Spusťte **Get-zásady spouštění** abyste měli jistotu, že příkazu pracoval.

![Snímek obrazovky s "Get-zásady spouštění" spuštěné v okně Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Přejděte na adresář, který má skripty a generátor aplikace.

![Snímek obrazovky s "cd .\TollApp\TollApp" spuštěné v okně Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Typ **.\\ Setup.ps1** Chcete-li nastavoval účet Azure, vytvoření a konfigurace všechny požadované zdroje a generovat události. Skript oblasti náhodně vyzvedne vytvořit zdroje. A explicitně zadejte do oblasti, můžete předávat **– umístění** parametr jako v následujícím příkladu:

**. \\Setup.ps1 – umístění "Centrální USA"**

![Snímek obrazovky s Azure přihlašovací stránka](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Skript otevře **Přihlašovací** stránka Microsoft Azure. Zadejte svoje přihlašovací údaje účtu.

> [AZURE.NOTE] Pokud váš účet má přístup k víc předplatných, můžete být požádáni o zadání název předplatného, které chcete použít pro kurz.

Skript může trvat několik minut. Po dokončení, výstup by měl vypadat následující snímek.

![Snímek obrazovky s výstupu skriptu v okně Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Zobrazí se také jiného okna, která se podobá následující snímek. Tato aplikace odesílá události rozbočovače Azure události, které je potřeba spustit kurzu. Ano nevytvářejte ukončení aplikace a zavřete okno, dokud nedokončíte kurzu.

![Snímek obrazovky s "Odesílání události centrální data"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Je třeba neuvidíte všech vytvořený zdrojů v Azure portálu. Přejděte na <https://manage.windowsazure.com>a přihlaste se pomocí svých přihlašovacích údajů účtu.

### <a name="azure-event-hubs"></a>Rozbočovače Azure události

Klikněte na **Službu BUS** na levé straně portálu Azure zobrazíte rozbočovače události, které skript vytvořený v předchozí části.

![Služba Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Zobrazí se všechny obory názvů k dispozici ve vašem předplatném. Klikněte na ten, který začíná *tolldata* (tolldata4637388511 v našem příkladu). Klikněte na kartu **ROZBOČOVAČE události** .

![Karta rozbočovače události na portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Uvidíte dva rozbočovače události s názvem *položky* a *Ukončete* vytvořené v tomto názvů.

![Snímek obrazovky s "položka" a "konec" rozbočovače události Azure portálu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure kontejner úložiště

1. Klikněte na **úložiště** na levé straně portálu Azure zobrazíte kontejneru Azure úložiště, který slouží v tomto kurzu.

    ![Položka nabídky úložiště](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Klikněte na ten, který začíná *tolldata* (tolldata4637388511 v našem příkladu). Klikněte na kartu **kontejnerů** zobrazíte vytvořený kontejner.

    ![Karta kontejnery na portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Klikněte na kontejner **tolldata** zobrazíte nahraný JSON soubor, který má vozidla registračních údajů.

    ![Snímek obrazovky s registration.json soubor v kontejneru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Databáze Azure SQL

1. Klikněte na **SQL databáze** na levé straně portálu Azure zobrazíte databáze SQL použitý v tomto kurzu.

    ![Databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Klikněte na tlačítko **tolldatadb**.

    ![Snímek obrazovky s vytvořené databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Zkopírujte název serveru bez číslo portu (*webu název serveru*. database.windows.net, například).

## <a name="connect-to-the-database-from-visual-studio"></a>Připojení k databázi z aplikace Visual Studio

Pomocí aplikace Visual Studio pro přístup k výsledků dotazu v databázi výstupu.

Připojení k databázi SQL (cíl) z aplikace Visual Studio:

1. Otevřete aplikaci Visual Studio a potom klikněte na **Nástroje** > **připojit k databázi**.

2. Pokud se zobrazí výzva, klikněte na **Microsoft SQL Server** jako zdroj dat.

    ![Dialogové okno Změnit zdroj dat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. V poli **název serveru** vložte název, který jste zkopírovali v předchozím oddíle z portálu Microsoft Azure (to znamená *webu název serveru*. database.windows.net).

4. Klikněte na **použít ověřování serveru SQL**.

5. Do pole **uživatelské jméno** zadejte **tolladmin** a **123toll!** do pole **heslo** .

6. Klikněte na **Vybrat nebo zadat název databáze**a vyberte **TollDataDB** jako databázi.

    ![Přidání dialogové okno připojení](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Klikněte na **OK**.

8. Otevřete Průzkumníka serveru.

    ![Průzkumník serveru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Viz čtyři tabulky v databázi TollDataDB.

    ![Tabulky v databázi TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Generátor události: TollApp ukázkový projekt

Skript Powershellu, spustí se automatické posílat události pomocí aplikace TollApp vzorku. Není potřeba provést další kroky.

Ale pokud vás zajímají informace o implementaci najdete zdrojového kódu aplikace TollApp v GitHub [Vzorky/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Snímek obrazovky ukázek kódu zobrazené ve Visual Studiu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy toku analýzy

1. Na portálu Azure otevřete toku technologie pro analýzu a klikněte na tlačítko **Nový** v levém dolním rohu stránce pro vytvoření nového projektu analýzy.

    ![Tlačítko Nový](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Klikněte na **vytvořit**. Vyberte stejné oblasti, kde ostatní zdroje vytvářejí skriptem.

3. Nastavení **Účtu úložiště Místní sledování** vyberte **Vytvořit nový účet úložiště** a zadejte jedinečný název. Azure toku analýzy budou používat tento účet pro uložení sledování informací pro všechny budoucí projekty.

4. Klikněte na **Vytvořit toku analýzy úlohy** v dolní části stránky.

    ![Vytvoření možnost úlohy analýzy toku](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definujte vstupní zdroje

1. Klikněte na úlohu vytvořené analýzy na portálu.

    ![Snímek obrazovky s analýzy úlohy na portálu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Klikněte na kartu **vstupů** definovat zdrojová data.

    ![Na kartě vstupů](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Klikněte na **Přidat zadání vstupních hodnot**.

    ![Přidání vstupní možnost](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Klikněte na první stránce **datového proudu** .

    ![Možnost toku dat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Na druhé stránce průvodce klikněte na **Centrum události** .

    ![Možnost centrální události](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Zadejte **EntryStream** jako **vstupní ALIAS**.

7. Klikněte na rozevírací seznam **Události centrální** a vyberte účet, který začíná na "TollData" (například TollData9518658221).

8. Vyberte **položku** jako centrální název události a **všech** jako název zásady centrální události.

    Nastavení bude vypadat jako:

    ![Nastavení centrální událostí](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Na další stránce vyberte **Formát SERIALIZACE událostí** a **UTF8** **kódování** **JSON** .

    ![Nastavení serializace](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Klikněte na **OK** v dolní části stránky Průvodce dokončíte.

    ![Snímek obrazovky EntryStream vstupní na portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Teď, když jste vytvořili položky toku, sledujete stejný postup vytvoření konec proudu. Na třetí stránce průvodce nezapomeňte zadat hodnoty jako na následující snímek.

    ![Nastavení pro ukončení toku](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Jste definovali dvěma datovými proudy zadávání:

    ![Definované vstupní toky v portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Pak přidáte odkaz zadávání dat pro objektů blob souboru, který obsahuje auta registračních údajů.

11. Klikněte na **Přidat vstupní**a potom klikněte na **Odkaz DATA**.

    !["Přidat vstup" možnosti s vybranými daty odkaz](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Na další stránce vyberte úložiště účet, který začíná **tolldata**. Jméno container by měl být **tolldata**a název objektů blob ve skupinovém rámečku **Vzorek cesta** by měl být **registration.json**. Tento název souboru je velká a malá písmena a by měl být malá písmena.

    ![Nastavení úložiště blogu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Vyberte hodnoty, jak je vidět na následující snímek obrazovky na další stránce a potom klikněte na **OK** dokončete průvodce.

    ![Výběr JSON "Formát i serializace" a UTF8 "Kódování"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Nyní jsou definované všechny vstupy.

![Snímek obrazovky s tři definovaný vstupní hodnoty](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Definování výstup

1. Klikněte na kartu **výstup** a potom klikněte na **Přidat výstup**.

    ![Karta Výstup a možnost "Přidat výstup"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Klikněte na **databáze SQL**.

3. Vyberte název serveru, který byl použit v části "Připojení k databázi z aplikace Visual Studio" tohoto článku. Název databáze je **TollDataDB**.

4. Do pole **uživatelské jméno** zadejte **tolladmin** **123toll!** v poli **heslo** a **TollDataRefJoin** v poli **tabulky** .

    ![Nastavení databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure dotazu toku analýzy

Karta **dotaz** obsahuje jazyka SQL, který transformací příchozí data.

![Dotaz přidané na kartu dotazu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Tento kurz se pokusí několik zodpovědět firmy, které se vztahují k číslo placené dat a konstrukce toku analýzy dotazů, které lze použít v Azure toku analýzy poskytnout příslušnou odpověď.

Před spuštěním první práce toku analýzy Podíváme se na chvíli scénářů a je syntaxe dotazu.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Úvod do technologie pro analýzu toku Azure dotazovací jazyk
-----------------------------------------------------

Řekněme, že budete muset počtu vozidla zadané ve stánku placená. Protože je to nepřetržitý toku události, budete muset definovat "časového úseku." Chci změnit otázku jako "kolik vozidla zadejte ve stánku placená každých tři minut?". To je označované jako počet tumbling.

Podívejme se na Azure toku analýzy dotaz, který odpovídá tuto otázku:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Jak můžete vidět, používá Azure toku analýzy což je dotazovací jazyk, který je jako SQL a přidá několik rozšíření Chcete-li zadat dobu aspektech dotazu.

Další informace přečtěte si o [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [práce s okny](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce použitá v dotazu z webu MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testování Azure toku analýzy dotazů

Teď jste napsali prvního dotazu Azure toku analýzy, je potřeba ji otestujte pomocí ukázkové datové soubory umístěné ve složce TollApp v následujícím umístění:

**.. \\TollApp\\TollApp\\dat**

Tato složka obsahuje následující soubory:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Otázka 1: Počet vozidla zadávání placená stánku

1. Otevřete portál Azure a přejděte na vytvořená pracovní Azure toku analýzy. Klikněte na kartu **dotazu** a vložte dotaz s předchozím oddílem.

    ![Vložit na kartě Query dotazu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Ověřte tento dotaz s ukázkovými daty, klikněte na tlačítko **Testovat** . V dialogovém okně, které se otevře přejděte na Entry.json, soubor, který má ukázkových dat z datového proudu **EntryTime** události.

    ![Snímek obrazovky s Entry.json souboru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Ověřte, zda výstup dotazu očekávaným způsobem:

    ![Výsledků testu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Otázka 2: Celkové má se čas vykazovat pro každou auta projít placená stánku

Průměrná doba potřebného pro Auto projít čísla placených pomáhá posuďte efektivity procesu a na základě zkušeností zákazníků.

Celkový čas najdete je potřeba se připojit proudu EntryTime s ExitTime proudu. Připojí proudy TollId a LicencePlate sloupce. Operátor **připojení** vyžaduje zadejte časový volnost popisující přijatelné časový rozdíl mezi události, které jste se připojili. Funkce **DATEDIFF** bude umožňuje určit, aby události delší než 15 minut od sebe. Použijí se taky funkce **DATEDIFF** ukončíte a položky doby pro výpočet skutečný čas, který Auto tráví stanice placená. Poznámka: rozdílu druhých využívání **DATEDIFF** použijete příkaz **SELECT** spíše než podmínku **připojení** .

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Chcete-li otestovat dotaz, aktualizační dotaz na kartě **QUERY** úlohy:

    ![Aktualizované dotazu na kartě Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Klepněte na tlačítko **Testovat** a zadejte ukázková vstupní soubory EntryTime a ExitTime.

    ![Snímek obrazovky s vybranou vstupních souborů](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Zaškrtněte políčko otestovat dotaz a zobrazit výstup:

    ![Výstup test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Otázka 3: Sestava všechna obchodní vozidla s ukončenou platností registrace

Azure analýzy toku umožňuje statické snímky dat připojení pomocí časový datových proudů. Prokázat tuto funkci, použijte následující příklad otázky.

Pokud obchodní vozidla máte zaregistrované v rámci společnosti placená, ho projít stánku placená bez zastavení ke kontrole. Registrace obchodní vozidla vyhledávací tabulky budou používat k identifikaci všechna obchodní vozidla, které vypršela platnost registrace.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Testování dotazu pomocí referenčních dat, musíte definovat vstupní zdroj pro referenčních dat, který jste ještě neudělali.

1. Testování dotazu, vložte dotaz na kartu **dotazu** , kliknutím na **Testovat**a zadejte prostředcích dvě vstupní:

    ![Snímek obrazovky s vybranou vstupních souborů](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Zobrazte výstup dotazu:

    ![Snímek obrazovky s výstupu dotazu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Spuštění úlohy toku analýzy


Teď je čas na dokončení konfigurace a zahájení projektu. Uložit dotaz ze 3 otázku, která vytvoří výstup, který odpovídá schématu **TollDataRefJoin** výstupní tabulky.

Přejděte na úlohy **řídicích panelů**a klikněte na tlačítko **START**.

![Snímek obrazovky na tlačítko Start v řídicím panelu projektu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

V dialogovém okně, které se otevře čas **Zahájení výstup** změňte na **vlastní čas**. Změna hodiny na hodinu před aktuální čas. Tato změna zaručuje, že všechny události z centra událost jsou zpracovány od spuštění pro vytvoření události na začátku tohoto kurzu. Teď, klikněte na značku zaškrtnutí úlohu spustíte.

![Výběr vlastní dobu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Spuštění úlohy může trvat několik minut. Na stránce nejvyšší úrovně toku analýzy můžete zobrazit stav.

![Snímek obrazovky o stavu projektu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Výsledky kontroly ve Visual Studiu

1. Otevřete Průzkumníka serveru Visual Studio a klikněte na tabulku, **TollDataRefJoin** .
2. Klikněte na **Zobrazit Data tabulky** zobrazíte výstup práce.

    ![Výběr "Zobrazit Data v tabulce" v Průzkumníku serveru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Rozšiřování Azure toku analýzy úlohy

Azure analýzy toku je určená elastically měřítko tak, aby je zpracovat velké množství dat. Azure toku analýzy dotazu lze použít klauzuli **Oddíl tak, že** zjistit systém, který bude rozšiřování tento krok. **ID oddílu** je speciální sloupec, který systém přidá podle ID oddílu vstupní (centrální událostí).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Zastavit aktuální úlohu, aktualizační dotaz na kartě **QUERY** a otevřete kartu **Měřítko** .

    **Přenos jednotky** definovat množství výpočetního výkonu, který může přijímat projektu.

2. Přesuňte jezdec až 6.

    ![Snímek obrazovky s výběrem 6 streamování jednotky](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Přejděte na kartu **výstupy** a změňte název tabulky SQL na **TollDataTumblingCountPartitioned**.

Pokud spustíte úlohu teď můžete Azure toku technologie pro analýzu projektu rozdělením práce mezi více využití prostředků a dosáhnout lepší výkon. Dejte pozor, aby aplikace TollApp také odesílá události oddíly TollId.

## <a name="monitor"></a>Sledování

Na kartě **MONITOR** obsahuje statistik o spuštěná úloha.

![Snímek obrazovky s jednou variantou o tom, jak úlohy](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

**Protokoly operací** dostanete z karty **řídicího PANELU** .

![Možnost "Operace protokoly"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Snímek obrazovky protokolů operace kde navíc přehledně uvidíte stav úloh](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Zobrazíte další informace o zvláštní události klikněte na událost a potom klikněte na tlačítko **Podrobnosti** .

![Snímek obrazovky s podrobnosti o událost nastavit jako vybrané](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Uzavření

Tento kurz potřebná pro službu Azure toku analýzy. Prokázáno konfiguraci vstupy a výstupy pro danou úlohu toku analýzy. Pomocí scénáře placená dat, kurzu vysvětlení nejběžnějších problémů, které nastanou v prostoru dat v pohybu a jak můžete vyřešit pomocí jednoduchého SQL profesionálové dotazů v Azure toku analýzy. Kurz popsány konstrukce rozšíření SQL pro práci s daty časový. Ukázal jak připojit datových proudů, jak rozšířit toku dat s statické referenčních dat a jak rozšiřování dotazu k dosažení vyšší výkon.

I když tento kurz obsahuje dobré úvod, není dokončena jakýmkoli způsobem. Můžete najít další postupy dotazu pomocí jazyka SAQL na [Příklady dotazu pro běžné vzorce toku analýzy využití](stream-analytics-stream-analytics-query-patterns.md).
Vyhledejte [online si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/stream-analytics/) zobrazíte další informace o Azure toku analýzy.

## <a name="clean-up-your-azure-account"></a>Vyčistit účet Azure

1. Ukončete úlohu toku analýzy na portálu Azure.

    Skript Setup.ps1 vytvoří dva rozbočovače událostí a databázi SQL. Následující pokyny vám vyčistit materiály na konci kurzu.

2. V okně Powershellu, zadejte **.\\ CleanUp.ps1** zahájíte skript, který slouží k odstranění zdrojů použitých v tomto kurzu.

    > [AZURE.NOTE] Zdroje jsou označeny název. Zkontrolujte, že pečlivě zkontrolujte všechny požadované položky před potvrzení odstranění.

    ![Snímek obrazovky s procesu vyčištění](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
