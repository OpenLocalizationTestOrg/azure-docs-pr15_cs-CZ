<properties
   pageTitle="Procházení a řízení zdrojů úložiště v Průzkumníkovi serveru | Microsoft Azure"
   description="Procházení a řízení zdrojů úložiště v Průzkumníkovi serveru"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Procházení a řízení zdrojů úložiště v Průzkumníkovi serveru

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Základní informace
Pokud jste si nainstalovali Azure nástroje pro Microsoft Visual Studio, můžete zobrazit objektů blob fronty a tabulková data z účtů úložiště Azure. Uzel Azure úložiště v Průzkumníku serveru zobrazuje data, která jsou v účtu emulátoru místní úložiště a jiných Azure úložiště účtů.

Průzkumník serveru ve Visual Studiu na řádek nabídek zobrazíte zvolte **zobrazení** **Průzkumníka serveru**. Uzel úložiště zobrazuje všechny účty úložiště, které jsou v části každý Azure předplatné/certifikát, který jste připojeni k. Pokud se nezobrazuje váš účet úložiště, můžete ho přidat pomocí kroků pokyny [dál v tomto tématu](#add-storage-accounts-by-using-server-explorer).

Spuštění v Azure SDK 2.7, můžete také nové Explorer cloudu můžete zobrazit a spravovat svoje Azure zdroje. Další informace najdete v tématu [Správa Azure zdrojů v Průzkumníkovi cloudu](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Zobrazení a Správa zdrojů úložiště ve Visual Studiu

Průzkumník serveru automaticky zobrazí se seznam objektů BLOB, fronty a tabulky ve vašem účtu emulátoru úložiště. Účet emulátoru úložiště je zobrazen v Průzkumníku serveru uzlu úložiště jako uzel **vývoj** .

Chcete-li zobrazit emulátoru účtu úložiště zdrojů, rozbalte uzel **vývoj** . Pokud emulátoru úložiště nebyla spuštěna při rozbalte uzel **vývoj** , bude automaticky vytvořena. To může trvat několik sekund. Můžete pokračovat v práci do jiných oblastí aplikace Visual Studio při spouštění emulátoru úložiště.

Zobrazení prostředků v účtu úložiště, rozbalte uzel účtu úložiště v Průzkumníku serveru. Následující podřízené uzly objevit:

- Objekty BLOB

- Fronty

- Tabulky

## <a name="work-with-blob-resources"></a>Práce se zdroji objektů Blob

Na uzel objekty BLOB zobrazí seznam kontejnerů účtu vybrané úložiště. Kontejnery objektů BLOB obsahují objektů blob soubory a uspořádání těchto objektů BLOB do složky a podsložky. Další informace najdete v tématu [Použití úložiště objektů Blob z .NET](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Vytvoření kontejneru objektů blob

1. Otevření místní nabídky pro uzel **objekty BLOB** a pak vyberte **Vytvořit kontejner objektů Blob**.

1. Zadejte název nového kontejneru v dialogovém okně **Vytvořit kontejner objektů Blob** a klikněte na **Ok**

    >[AZURE.NOTE] Jméno container objektů blob vždy začíná číslo (0 až 9) nebo malé písmeno (-z).

### <a name="to-delete-a-blob-container"></a>Chcete-li odstranit kontejneru objektů blob

- Otevření místní nabídky pro kontejneru objektů blob, který chcete odebrat a pak zvolte **Odstranit**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Chcete-li zobrazit seznam položky obsažené v kontejneru objektů blob

- Otevření místní nabídky pro jméno container objektů blob v seznamu a pak zvolte **Zobrazit objektů Blob kontejner**.

    Zobrazení obsahu kontejneru objektů blob se zobrazí na kartě jmenoval zobrazení objektů blob kontejner.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Pomocí tlačítek v pravém horním rohu zobrazení kontejneru objektů blob můžete dělat s objekty BLOB následující operace:

    - Zadání hodnoty filtru a jeho použití

    - Aktualizace seznamu objektů BLOB v kontejneru

    - Odeslání souboru

    - Odstranění objektů blob

      >[AZURE.NOTE] Odstranění souboru v kontejneru objektů blob neodstraní podkladová souboru. ho jenom odeberete z objektů blob kontejner.

    - Otevřete objekt blob

    - Uložení objektů blob do místního počítače

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Pokud chcete vytvořit složku nebo podsložku v kontejneru objektů blob

1. Zvolte kontejneru objektů blob v Průzkumníku serveru. V okně kontejneru klikněte na tlačítko **Nahrát objektů Blob** .

    ![Nahrání souboru do jiné složky objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. V dialogovém okně **Odeslat odkaz na nový soubor** klikněte na tlačítko **Procházet** a určete soubor, který chcete nahrát a pak zadejte název složky do pole **složka (volitelné)** .

    Stejným postupem, můžete přidat podsložek ve kontejner složkách. Pokud nezadáte název složky, soubor odeslat na webu nejvyšší úrovně z objektů blob kontejner. Tento soubor nebude ve složce zadané v kontejneru.

    ![Složky přidají do kontejneru objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Poklikejte na složku nebo stiskněte klávesu ENTER zobrazte obsah složky. Pokud se účastníte složek kontejneru, můžete Navigovat zpátky ke kořenové kontejneru tak, že kliknete na tlačítko **Otevřít nadřazený adresář** (šipka) nahoru.

### <a name="to-delete-a-container-folder"></a>Chcete-li odstranit složek kontejneru

 - Odstraňte všechny soubory ve složce

    >[AZURE.NOTE] Protože složek v kontejnerech objektů blob virtuální složky, vyprázdnit složku nelze vytvořit ani odstranění složky odstranit obsah souboru. Budete muset odstranit celý obsah složky na Odstranit složku.

### <a name="to-filter-blobs-in-a-container"></a>Pokud chcete filtrovat objektů BLOB v kontejneru

Můžete filtrovat objektů BLOB, které jsou zobrazeny zadáním běžné předpony.

Například, pokud zadáte předponu `hello` text filtr pole a pak zvolte **Spustit** (****!) tlačítko, zobrazí jenom objekty BLOB, které začínají na "Ahoj".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Pole filtru se rozlišují malá a, které nejsou podporovány filtrování se zástupnými znaky. Objekty BLOB můžete pouze filtrované podle předponu. Pokud používáte oddělovač k uspořádání objektů BLOB v hierarchii virtuální je možné předponu oddělovače. Například filtrování předponu HelloFabric / vrací všechny objekty BLOB začínající tento řetězec.

### <a name="to-download-blob-data"></a>Pokud si chcete stáhnout objektů blob dat

- V **Průzkumníku serveru**otevření místní nabídky pro jeden nebo více objektů BLOB a potom zvolte **Otevřít**, vyberte název objektů blob a potom klikněte na tlačítko **Otevřít** nebo poklikejte na název objektů blob.

    Průběh stahování objektů blob se zobrazí v okně **Protokolu činnosti Azure** .

    Objektů blob otevře v editoru výchozí pro tento typ souboru. Pokud operační systém rozpozná typ souboru, soubor se otevře v místním počítači nainstalovanou aplikaci; v opačném se zobrazí výzva a vyberte aplikaci, která odpovídá typu souboru objektů blob. Místní soubor, který se vytvoří při stahování objektů blob je označený jako jen pro čtení.

    Objektů BLOB dat je do místní mezipaměti a porovnáván objektů blob naposledy změněno ve službě objektů Blob. Pokud objektů blob byl aktualizován od posledního stažení, bude stahovat znovu. v opačném případě bude objektů blob načíst z místního disku. Ve výchozím nastavení se stáhne objektů blob dočasného adresáře. Ke stažení objektů BLOB určitých adresáři, otevřete místní nabídky pro názvy vybraných objektů blob a klikněte na **Uložit jako**. Při uložení objektů blob tímto způsobem není otevřen souboru blob a je vytvořen místní soubor s atributy pro čtení i zápis.

### <a name="to-upload-blobs"></a>Pokud chcete odeslat objekty BLOB

- Klikněte na tlačítko **Nahrát objektů Blob** v otevřeném kontejneru při zobrazení v zobrazení objektů blob kontejner.

    Můžete vybrat jeden nebo více souborů k nahrání a můžete nahrávat soubory libovolného typu. **Protokol činnosti Azure** zobrazuje průběh nahrávání. Další informace o tom, jak pracovat s daty objektů blob najdete v článku [používání služba úložiště objektů Blob Azure v .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Chcete-li zobrazit protokoly převedeny na objekty BLOB

- Pokud používáte Azure diagnostiky protokolování dat z Azure aplikace a jste převedli protokoly ke svému účtu úložiště, zobrazí se kontejnery, které se vytvářely Azure pro tyto protokoly. Zobrazení těchto protokolů v Průzkumníku serveru je snadný způsob, jak identifikovat problémy s aplikací, zejména pokud byla nasazení na Azure. Další informace o diagnostiky Azure najdete v článku [Shromáždit protokolování údajů pomocí diagnostických nástrojů Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>K získání adresy URL objektů blob

- Otevřete objektů blob místní nabídku a pak zvolte **Kopírovat adresu URL**.

### <a name="to-edit-a-blob"></a>Chcete-li upravit objektů blob

- Vyberte objekt blob a potom klikněte na tlačítko **Otevřít objektů Blob** .

    Soubor je stáhli dočasného umístění a otevřít v místním počítači. Musíte nahrajete objektů blob znovu po provedení změn.

## <a name="work-with-queue-resources"></a>Práce se zdroji fronty

Úložiště služby fronty jejichž hostitelem je účet Azure úložiště a můžete je povolit vaše cloudové služby role pro komunikaci mezi sebou a s jinými službami pomocí předávání mechanismus zpráv. Ve frontě programově až do cloudové služby a zpřístupníte prostřednictvím webové služby pro externí klienti. Získat přístup k fronty přímo pomocí Průzkumníka serveru ve Visual Studiu.

Pokud vyvinete do cloudové služby, který používá fronty, můžete pomocí aplikace Visual Studio můžete vytvořit fronty a pracovat s nimi interaktivně, zatímco vyvíjet a testovat kód.

V okně Průzkumník serveru můžete zobrazit fronty v účtu úložiště, vytvořit a odstranit fronty, otevřete fronty zobrazíte své zprávy a přidat zprávy do fronty. Při otevření fronty pro zobrazení můžete zobrazit jednotlivé zprávy a můžete provádět následující akce ve frontě pomocí tlačítek v levém horním:

- Aktualizaci zobrazení fronty

- Přidejte zprávu do fronty

- Dequeue horní zprávy.

- Vymazání celý fronty

Následující obrázek znázorňuje fronta obsahující dvě zprávy.

![Zobrazení fronty](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Další informace o úložiště služby fronty, najdete v článku [Postup: pomocí služby úložiště fronty](http://go.microsoft.com/fwlink/?LinkID=264702). Informace o webová služba úložiště služby fronty najdete v tématu [Fronty služby koncepty](http://go.microsoft.com/fwlink/?LinkId=264788). Další informace o odesílání zpráv do úložiště služby fronty pomocí aplikace Visual Studio najdete v tématu [Odesílání zpráv do fronty úložiště služby](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Úložiště služby fronty se liší od služby bus fronty. Podrobnosti o frontách bus služby najdete v článku fronty Bus služby, témata a předplatné.

## <a name="work-with-table-resources"></a>Práce se zdroji tabulky

Služba úložiště tabulek Azure ukládá velkých objemů strukturovaná data. Služba je, že NoSQL úložiště, který přijme ověřen hovory z uvnitř a mimo Azure cloudu. Azure tabulky jsou ideální pro ukládání strukturovaných, není relačních dat.

### <a name="to-create-a-table"></a>Vytvoření tabulky

1. V okně Průzkumník serveru vyberte uzel **tabulky** úložiště účtu a klikněte na **Vytvořit tabulku**.

1. V dialogovém okně **Vytvořit tabulku** zadejte název pro tabulku.

### <a name="to-view-table-data"></a>Chcete-li zobrazit data v tabulce

1. V okně Průzkumník serveru otevřete uzel **Azure** a poté rozbalte uzel **úložiště** .

1. Otevřete uzel úložiště účtu, který vás zajímá a pak otevřete uzel **tabulky** zobrazíte seznam tabulek účtu úložiště.

1. Otevření místní nabídky pro tabulky a pak zvolte **Zobrazit tabulku**.

    ![Azure tabulky v okně Průzkumník řešení](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tabulky jsou uspořádána entity (viz v řádcích) a vlastnosti (viz ve sloupcích). Na následujícím obrázku vidíte entity uvedené v **Návrhovém zobrazení tabulky**:

### <a name="to-edit-table-data"></a>Úpravy dat v tabulce

1. V **Návrhovém zobrazení tabulky**otevřete v místní nabídce entity (jeden řádek) nebo vlastnosti (jedné buňce) a klikněte na **Upravit**.

    ![Přidání nebo úprava Entity tabulky](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Entity v jedné tabulce nejsou povinná mít stejnou sadu vlastnosti (sloupce). Mějte na paměti následující omezení prohlížet a upravovat data v tabulce.
    - Se nedají zobrazit ani upravit binární data (byte[]) typ, ale můžete uložit v tabulce.

    - Hodnoty **PartitionKey** nebo **RowKey** nelze upravit, protože úložiště tabulek v Azure, které nejsou podporovány operaci.

    - Nemůžete vytvořit vlastnost nazvanou časového razítka, služby Azure úložiště použít vlastnost s tímto názvem.

    - Pokud zadáte hodnotu data a času, postupujte podle formátu, který je třeba oblast a jazyk nastavení počítače (například MM/DD/RRRR HH: mm: [dopoledne | Odpoledne] pro angličtinu USA).

### <a name="to-add-entities"></a>Chcete-li přidat entity

1. V **Návrhovém zobrazení tabulky**klikněte na tlačítko **Přidat entitu** , který se nachází v pravém horním rohu zobrazení tabulky.

    ![Přidání Entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. V dialogovém okně **Přidat entitu** zadejte hodnoty vlastnosti **PartitionKey** a **RowKey** .

    ![Entita dialogové okno Přidat](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Pečlivě zadejte hodnoty, protože je nelze měnit po zavření dialogového okna, pokud jste odstranili entita a jeho opětovné přidání.

### <a name="to-filter-entities"></a>Pokud chcete filtrovat entity

Můžete přizpůsobit nastavení entity, které se zobrazují v tabulce, pokud používáte Tvůrce dotazů.

1. Chcete-li spustit Tvůrce dotazů, otevřete tabulku pro zobrazení.

1. Zvolte tlačítko úplně vpravo na panel nástrojů zobrazení tabulky.

    Zobrazí se dialogové okno **Tvůrce dotazů** . Následující obrázek znázorňuje dotaz, který je vytvářeného v okně Tvůrce dotazů.

    ![Tvůrce dotazů](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Po dokončení sestavování dotazu, zavřete dialogová okna. Výsledný formulář text dotazu se zobrazí v textovém poli jako datové služby WCF filtr.

1. Spusťte dotaz, zvolte ikonu zelený trojúhelník.

    Můžete taky filtrovat entity data, která se zobrazí v **Návrhovém zobrazení tabulky** zadáte-li řetězec datové služby WCF filtru přímo do pole Filtr. Tento typ string je podobný SQL klauzule WHERE ale se odesílá na server žádost HTTP. Informace o tom, jak vytvářet filtr řetězce najdete v tématu [Sestavování řetězce filtr pro návrháře tabulky](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Následující obrázek ukazuje příklad platný filtr řetězce:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Aktualizace dat úložiště

Když Průzkumník serveru připojuje k nebo získává data z účtu úložiště, může trvat minutu pro dokončení operace. Pokud se nemůžete připojit, může operace časový limit. Načítání dat můžete pokračovat v práci v dalších částech Visual Studio. Operaci zrušit, pokud trvá příliš dlouho, klikněte na tlačítko **Zastavit Aktualizovat** na panelu nástrojů Průzkumník serveru.

#### <a name="to-refresh-blob-container-data"></a>Postup pro aktualizaci dat kontejneru objektů blob

- Vyberte uzel **objekty BLOB** pod **úložiště** a klikněte na tlačítko **Aktualizovat** na panelu nástrojů Průzkumník serveru.

- Abyste mohli aktualizovat seznam objektů BLOB, která se zobrazí, klikněte na tlačítko **Spustit** .

#### <a name="to-refresh-table-data"></a>Chcete-li aktualizovat data v tabulce

- Vyberte uzel **tabulky** pod **úložiště** a klikněte na tlačítko **Aktualizovat** .

- Abyste mohli aktualizovat seznam entit, která se zobrazí v **Návrhovém zobrazení tabulky**, klikněte na tlačítko **Spustit** v **Návrhovém zobrazení tabulky**.

#### <a name="to-refresh-queue-data"></a>Postup pro aktualizaci dat fronty

- Vyberte uzel **fronty** a klikněte na tlačítko **Aktualizovat** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Chcete-li aktualizovat všechny položky v účtu úložiště

- Vyberte název účtu a klikněte na tlačítko **Aktualizovat** na panelu nástrojů pro Průzkumník Server.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Přidání úložiště účty pomocí Průzkumníka serveru

Existují dva způsoby přidání úložiště účty pomocí Průzkumníka serveru. Vytvoření nového účtu úložiště v Azure předplatné nebo připojíte existujícího účtu úložiště.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Vytvoření nového účtu úložiště pomocí Průzkumníka serveru

1. V okně Průzkumník serveru otevřete místní nabídky pro uzel úložiště a klikněte na vytvořit účet úložiště.

    ![Vytvořte nový účet Azure úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Vyberte nebo zadejte následující informace pro nový účet úložiště v dialogovém okně **Vytvořit účet úložiště** .

    - Azure předplatné, ke kterému chcete přidat účet úložiště.

    - Název, který chcete použít pro nový účet úložiště.

    - Oblast nebo spřažení skupiny (například západní USA nebo jihovýchodní Asie).

    - Typ replikace, který chcete použít účtu úložiště, jako jsou například Geo nadbytečné.

1. Zvolte **vytvořit**.

    V seznamu **úložiště** v Průzkumníku řešení se zobrazí nový účet úložiště.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Pokud chcete připojit existujícího účtu úložiště pomocí Průzkumníka serveru

1. V okně Průzkumník serveru otevřete místní nabídky pro uzel Azure úložiště a klikněte na **Připojit externí úložiště**.

    ![Přidání existujícího účtu úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Vyberte nebo zadejte následující informace pro nový účet úložiště v dialogovém okně **Vytvořit účet úložiště** .

    - Název existujícího účtu úložiště, který chcete připojit. Můžete zadat název nebo ho vyberte ze seznamu.

    - Klíč účtu vybrané úložiště. Tato hodnota je obvykle poskytnuto po výběru účtu úložiště. Pokud budete potřebovat Visual Studio zapamatovatelné klíč účtu úložiště, políčko Zapamatovat účtu klíče.

    - Protokol sloužící k připojení k účtu úložiště, jako je HTTP, HTTPS nebo vlastní koncového bodu. Další informace o vlastních koncové body v tématu [jak nakonfigurovat připojovací řetězec](https://msdn.microsoft.com/library/azure/ee758697.aspx) .

### <a name="to-view-the-secondary-endpoints"></a>Chcete-li zobrazit vedlejší koncové body

- Pokud jste vytvořili úložiště účtu pomocí **Přístup pro čtení Geo nadbytečné** možnosti replikace, můžete zobrazit jeho sekundární koncové body. Otevření místní nabídky pro název účtu a klikněte na **Vlastnosti**.

    ![Sekundární koncové body úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Chcete-li odebrat účet úložiště Průzkumník serveru

- V okně Průzkumník serveru otevřete místní nabídku pro název účtu a klikněte na **Odstranit**. Pokud odstraněním účtu úložiště odebrán také všechny uložené klíčové informace k tomuto účtu.

    >[AZURE.NOTE] Pokud byste odstranili z Průzkumníka serveru účtu úložiště, nemá vliv účtu úložiště nebo všechna data, která obsahuje; jednoduše odebere odkaz z Průzkumníka serveru. Pokud chcete trvale odstranit účet úložiště, pomocí [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Další kroky

Informace o tom, že další informace o použití služby Azure úložiště, najdete v článku [přístup ke službám úložiště Azure](https://msdn.microsoft.com/library/azure/ee405490.aspx).
