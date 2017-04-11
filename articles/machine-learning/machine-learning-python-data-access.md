<properties 
    pageTitle="Přístup k datové sady s knihovnou klientského počítače výukové Python | Microsoft Azure" 
    description="Instalace a používání knihovny klienta Python k přístupu a správa dat Azure počítače výukové bezpečně z místního prostředí Python." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Access datové sady s Python pomocí klienta knihovny Python výukové počítače Azure 

Náhled knihovny klienta Microsoft Azure počítače výukové Python umožní zabezpečený přístup sady dat Azure počítače výukové z místního prostředí Python a umožňuje vytváření a Správa datových sad v pracovním prostoru.

Toto téma obsahuje pokyny k:

* instalace knihovnu klienta Python výukové počítače 
* přístup a nahrajte datové sady, včetně pokyny, jak rychle se tak mohli ověřovat přistupovat k sady dat Azure počítače výukové z místního prostředí Python
*  přístup k intermediate datových sad z pokusů
*  Použijte knihovnu klienta Python umožňuje zobrazit výčet datové sady, přístup k metadat, číst obsah datovou sadu, vytvoříte nové datové sady a aktualizovat existující datové sady

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Zjistit předpoklady pro

Otestování knihovnu Python klienta v části následující prostředí:

 - Systém Windows, Mac a Linux
 - Python 2.7, 3.3 a 3.4

Obsahuje závislosti na následující balíčky:

 - požadavky
 - Python dateutil
 - pandas

Doporučujeme používat Python rozdělení jako výše uvedené [Anaconda](http://continuum.io/downloads#all) nebo [zápoje](https://store.enthought.com/downloads/), které jsou součástí Python IPython a tři balíčky nainstalovaná. Přestože IPython nepožaduje sporná, i když, je skvělé prostředí pro práci s a interaktivní vizualizace dat.


###<a name="installation"></a>Jak nainstalovat knihovnu Azure počítače výukové Python klienta

Knihovna klienta Azure počítače výukové Python musí být nainstalovaný také k provedení těchto úkolů uvedené v tomto tématu. Je k dispozici z [Python balíčku Index](https://pypi.python.org/pypi/azureml). Případně ji nainstalujte ve vašem prostředí Python, spusťte tento příkaz z místního prostředí Python:

    pip install azureml

Můžete taky můžete stáhnout a nainstalovat ze zdrojů na [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Pokud máte na počítači nainstalovaný libovolná, můžete si můžete nainstalovat přímo z úložiště libovolná pip:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Fragmenty kódu Studio použít pro přístup k datové sady

Knihovny klienta Python umožňuje programové přístup k existující datových sad z pokusy, které jste spustili.

Z webového rozhraní Studio si můžete vygenerovat fragmenty kódu, které zahrnují všechny potřebné informace ke stažení a deserializaci datové sady jako Pandas DataFrame objekty ve vašem počítači umístění.

### <a name="security"></a>Zabezpečení pro přístup k datům

Fragmenty kódu poskytovanou Studio pro použití s knihovnou klienta Python obsahuje vaše id pracovní prostor a povolení tokenu. Jedná poskytují plný přístup do pracovního prostoru, musí být chráněny v, jako jsou hesla.

Z bezpečnostních důvodů funkce fragment kódu je k dispozici pouze uživatelům, kteří mají jejich roli nastavit jako **vlastník** pracovního prostoru. Vaše role se zobrazí v Azure počítače výukové Studio na stránce **Uživatelé** klikněte v části **Nastavení**.

![Zabezpečení][security]

Pokud vaše role není nastavena jako **vlastník**, můžete buď požadavek nového pozvání jako některé vlastníky nebo požádejte vlastníka pracovního prostoru, aby vám poskytovat fragment kódu.

Získat token se tak mohli ověřovat, můžete udělat jednu z následujících akcí:



- Požádejte o token z některé vlastníky. Vlastníci přístup k jejich se tak mohli ověřovat tokeny ze stránky nastavení jejich pracovní prostor Studio. V levém podokně vyberte **Nastavení** a klikněte na tlačítko **Se tak mohli OVĚŘOVAT TOKENY** zobrazíte tokeny primárních a sekundárních.  Přestože primární a sekundární se tak mohli ověřovat tokeny lze použít v kódu, doporučujeme vlastníci sdílet jenom tokeny sekundární se tak mohli ověřovat.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Zeptejte se má být převeden na role majitele.  K tomuto účelu, aktuální vlastník pracovního prostoru musí nejdřív zrušit pracovní prostor a pak znovu pozvat do ní jako některé vlastníky.

Jakmile vývojáři obdrželi id pracovní prostor a povolení tokenu, mají mít přístup k pracovního prostoru pomocí kódu bez ohledu na základě rolí.

Tokeny se tak mohli ověřovat je spravováno na stránce **Se tak mohli OVĚŘOVAT TOKENY** klikněte v části **Nastavení**. Můžete je obnovit, ale tento postup odebere přístup k předchozí token.

### <a name="accessingDatasets"></a>Datové sady přístup z místní Python aplikace

1. Ve počítače výukové studiu klikněte na navigačním panelu na levé straně **datové sady** .

2. Vyberte datovou sadu, které se mají přístup. V seznamu **Moje datové sady** nebo ze seznamu **UKÁZKY** vyberete datové sady.

3. Na panelu nástrojů dole klikněte na **Generovat dat přístupového kódu**. Pokud se data ve formátu kompatibilní s knihovnou Python klienta, toto tlačítko k dispozici.

    ![Datové sady][datasets]

4. Vyberte fragment kódu v okně, které se zobrazí a jeho zkopírování do schránky.

    ![Přístupový kód][dataset-access-code]

5. Vložte kód do poznámkového bloku místní Python aplikace.

    ![Poznámkový blok][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Přechodné datové sady přístup z počítače výukové pokusů

Po spuštění pokus ve počítače výukové studiu je možné pro přístup k intermediate datových sad z výstupu uzlů moduly. Přechodné datové sady jsou data, které byly vytvořeny a při spuštění nástroje modelu intermediate postup.

Přechodné datové sady přístupná, dokud formát dat kompatibilní s knihovnou Python klienta.

Podporované jsou tyto formáty (jsou konstanty pro tyto v `azureml.DataTypeIds` třídy):

 - Prostý text
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Formát můžete zjistit tak, že umístíte ukazatel myši nad uzel výstup modul. Zobrazí se spolu s názvu uzel v popisů ovládacích prvků.

Některé modulů kontroly, například [rozdělené] [ split] modul výstup do formátu s názvem `Dataset`, který neposkytuje podporu knihovnu Python klienta.

![Formát datové sady][dataset-format]

Je potřeba použít převodu modulu, například [převést do formátu CSV][convert-to-csv]k získání výstup do formátu podporované.

![Formát GenericCSV][csv-format]

Následující postup příklad, který vytvoří pokus, spustí jej a přistupuje intermediate datovou sadu.

1. Vytvoření nového testu.

2. Vložení modul **dospělého sčítání příjem binární klasifikace datovou sadu** .

3. Vložit [rozdělení] [ split] modulu a připojte vstupní do výstupu modul datovou sadu.

4. Vložení [převést do formátu CSV] [ convert-to-csv] modul a připojte jeho předávat na vstupu jeden [úkol rozdělit] [ split] modul výstupem.

5. Uložení testu, ho spusťte a čekat na ukončen.

6. Klikněte na uzel výstup na [převést na CSV] [ convert-to-csv] modul.

7. Po zobrazení místní nabídky vyberte **Generovat dat přístupového kódu**.

    ![Místní nabídka][experiment]

8. Vyberte fragment kódu a jeho zkopírování do schránky z podokna úloh.

    ![Přístupový kód][intermediate-dataset-access-code]

9. Vložte kód v poznámkovém bloku.

    ![Poznámkový blok][ipython-intermediate-dataset]

10. Můžete vizualizace dat pomocí matplotlib. Zobrazí se v histogramu pro sloupec Věk:

    ![Analytický nástroj Histogram][ipython-histogram]


##<a name="clientApis"></a>Použít knihovnu klienta počítače výukové Python dostupnost, číst, vytvářet a spravovat datové sady

### <a name="workspace"></a>Pracovní prostor

Pracovní prostor je vstupní bod pro knihovnu Python klienta. Poskytnutí `Workspace` třídy pomocí svého pracovního prostoru id a povolení token pro vytvoření instance:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Umožňuje zobrazit výčet datové sady

Chcete-li vytvořit výčet všechny datové sady v dané pracovního prostoru:

    for ds in ws.datasets:
        print(ds.name)

Vytvořit výčet jenom uživatel vytvořil datové sady:

    for ds in ws.user_datasets:
        print(ds.name)

Vytvořit výčet jenom datové sady příkladu:

    for ds in ws.example_datasets:
        print(ds.name)

Dostanete datovou sadu názvu (což je malá a velká písmena):

    ds = ws.datasets['my dataset name']

Nebo je můžete používat tak, že index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadata

Datové sady mít metadat kromě obsah. (Intermediate datové sady jsou výjimkou tohoto pravidla a nemusíte metadatům)

Některé hodnoty metadat přiřazené uživatelem v údaje o času vytvoření:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Ostatní jsou přiřazené Azure ml:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zobrazit `SourceDataset` třídy Další informace o dostupných metadata.


### <a name="read-contents"></a>Číst obsah

Fragmenty kódu dostupné ve počítače výukové Studio automaticky stáhnout a deserializaci datovou sadu Pandas DataFrame objekt. Důvodem je, s `to_dataframe` metoda:

    frame = ds.to_dataframe()

Pokud chcete stáhnout nezpracovanými daty a dělat deserializace sami, je to jednu z možností. V okamžiku Toto je toto jediná možnost formátů, například "ARFF", které nelze rekonstruovat knihovnu Python klienta.

Číst obsah jako text:

    text_data = ds.read_as_text()

Číst obsah jako binární:

    binary_data = ds.read_as_binary()

Můžete otevřít taky jen toku obsahu:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Vytvoření nové datové sady

Knihovna klienta Python umožňuje nahrát datových sad z aplikace Python. Tyto datové sady budou k dispozici pro použití v pracovním prostoru.

Pokud máte data v Pandas DataFrame, použijte následující kód:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Pokud už serializován dat, můžete:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Knihovna klienta Python je moct serializovat Pandas DataFrame do těchto formátů (jsou konstanty pro tyto v `azureml.DataTypeIds` třídy):

 - Prostý text
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Aktualizace stávající datové sady

Pokud se pokusíte nahrávat nové datové sady s názvem, který odpovídá existující datovou sadu, by měla získat chybu konfliktu.

Aktualizace stávající datovou sadu, musíte nejprve získat odkaz na existující datovou sadu:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Potom použijte `update_from_dataframe` serializovat a nahradit obsah datovou sadu na Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Pokud budete chtít serializovat data do jiného formátu, zadejte hodnotu pro volitelné `data_type_id` parametr.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Pokud chcete nastavit nový popis zadáním hodnoty pro `description` parametr.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Pokud chcete nastavit nový název zadáním hodnoty pro `name` parametr. Od této chvíle budete načtete datovou sadu pomocí pouze nový název. Následující kód aktualizace dat, název a popis.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`, `name` a `description` parametrů jsou volitelné a výchozí na předchozí hodnotu. `dataframe` Vždycky je potřeba parametr.

Pokud vaše data už serializován, použijte `update_from_raw_data` namísto `update_from_dataframe`. Pokud je jenom v `raw_data` namísto `dataframe`, funguje podobně.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
