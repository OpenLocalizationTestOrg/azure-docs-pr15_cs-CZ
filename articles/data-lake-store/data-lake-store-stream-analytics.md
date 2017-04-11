<properties
   pageTitle="Do úložiště jezera dat můžete vysílat datovými proudy dat z datového proudu analýzy | Azure"
   description="Pomocí analýzy toku Azure toku dat do úložiště jezera dat Azure"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Toku dat z Azure úložiště objektů Blob do úložiště jezera dat pomocí analýzy toku Azure

V tomto článku se dozvíte, jak pro účely úložiště jezera dat Azure jako výstup projektu Azure toku analýzy. Tento článek ukazuje jednoduchý scénář načte data z Azure úložiště objektů blob (vstup), který data zapisuje data do úložiště jezera dat (výstup).

>[AZURE.NOTE] V současné době vytváření a konfigurace výstupy úložiště jezera dat pro analýzy toku je podporováno pouze v [Azure klasické portálu](https://manage.windowsazure.com). Některé části funkcí tento kurz se tedy používat portál klasické Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Povolení předplatného Azure** dat jezera úložiště veřejného (verze Preview). V tématu [pokyny](data-lake-store-get-started-portal.md#signup).

- **Účet azure úložiště**. Zadávání dat projektu toku analýzy bude používat kontejneru objektů blob z tohoto účtu. Pro tento kurz se předpokládá, že vytvoříte úložiště účet s názvem **datalakestoreasa** a kontejneru v rámci účet s názvem **datalakestoreasacontainer**. Po vytvoření kontejneru nahrajte Ukázka datového souboru do ní. Ukázka datového souboru můžete získat z [Azure dat jezera libovolná úložiště](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Odeslání dat do kontejneru objektů blob můžete různých klientů, například [Průzkumníka úložišť Azure](http://storageexplorer.com/).

    >[AZURE.NOTE] Pokud vytvoříte účet z portálu Microsoft Azure, zkontrolujte, že vytvoříte pomocí **klasické** nasazení modelu. Zajistíte, že účet úložiště můžete k nim získat přístup z portálu Microsoft klasické Azure, protože to je používáme k vytvoření úlohy toku analýzy. Pokyny k vytvoření účtu úložiště z portálu Azure pomocí klasického nasazení najdete v článku [Vytvoření účet Azure úložiště](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Nebo můžete vytvořit účet úložiště z portálu klasické Azure.

- **Úložiště jezera dat azure účtu**. Postupujte podle pokynů na [Začínáme s úložiště jezera Azure dat na portálu Azure](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy analýzy toku

Začněte tak, že vytvoříte úlohy toku analýzy, která obsahuje vstupní zdroj a cílový výstup. Pro účely tohoto návodu zdroje je kontejner objektů blob Azure a jezera úložiště je cílová.

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).

2. V levé dolní části obrazovky klikněte na **Nový**, **Datových služeb**, **Analýzy toku**, **Vytvořit**. Obsahují hodnoty, jak je ukázáno v následujícím příkladu a potom klikněte na **Vytvořit úlohy analýzy proudu**.

    ![Vytvoření úlohy analýzy toku] (./media/data-lake-store-stream-analytics/create.job.png "Vytvoření projektu toku analýzy")

## <a name="create-a-blob-input-for-the-job"></a>Vytvoření objektů Blob vstup pro danou úlohu

1. Otevřete stránku pro danou úlohu toku analýzy, klikněte na kartu **vstupů** a klikněte na tlačítko **Přidat vstup** spusťte průvodce.

2. Na stránce **Přidat vstup do práce** vyberte **toku dat**a klikněte na tlačítko vpřed.

    ![Přidat vstup do práce] (./media/data-lake-store-stream-analytics/create.input.1.png "Přidat vstup do práce")

3. Na stránce **Přidat toku dat k práci** vyberte **úložiště objektů Blob**a potom klikněte na tlačítko vpřed.

    ![Přidání datového proudu do projektu] (./media/data-lake-store-stream-analytics/create.input.2.png "Přidání datového proudu do projektu")

4. Na stránce **Nastavení úložiště objektů Blob** přidávejte k úložišti objektů blob, který použijete jako zdroj zadávání dat podrobnosti.

    ![Zadat objektů blob nastavení úložiště] (./media/data-lake-store-stream-analytics/create.input.3.png "Zadat objektů blob nastavení úložiště")

    * **Enter alias zadávání**. Toto je jedinečný název, který zadáte pro danou úlohu vstupní.
    * **Vyberte účet úložiště**. Ujistěte se, účtu úložiště je ve stejné oblasti jako úlohu toku analýzy nebo je možné za další náklady přesouvat data mezi oblastmi.
    * **Poskytovat kontejneru úložiště**. Můžete vytvořit nový kontejner nebo vyberte stávající kontejner.

    Klikněte na tlačítko vpřed.

5. Na stránce **Nastavení serializace** nastavte formát serializace jako **CSV**, oddělovač jako na **kartě**kódování jako **UTF8**a klikněte na zaškrtnutí.

    ![Zadání nastavení serializace] (./media/data-lake-store-stream-analytics/create.input.4.png "Zadání nastavení serializace")

6. Po dokončení Průvodce vstupní objektů blob se přidá na kartě **vstupů** a sloupci **diagnostiky** byste měli vidět **OK**. Můžete také explicitně testovat připojení k zadání pomocí tlačítko **Testovat připojení** dole.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Vytvoření úložiště jezera dat výstupu projektu

1. Otevřete stránku pro danou úlohu toku analýzy, klikněte na kartu **výstupy** a klikněte na tlačítko **Přidat výstup** spusťte průvodce.

2. Na stránce **Přidat výstup do práce** vyberte **Úložiště jezera dat**a klikněte na tlačítko vpřed.

    ![Přidat výstup do práce] (./media/data-lake-store-stream-analytics/create.output.1.png "Přidat výstup do práce")

3. Na stránce **Povolení připojení** Pokud jste již vytvořili účet úložiště jezera dat, klepněte na tlačítko **Povolit**. Jinak klepněte na **Přihlásit se** k vytvoření nového účtu. Tento kurz Předpokládejme, že už máte účet úložiště jezera dat vytvořili (jak je uvedeno v předpokladem). Můžete automaticky povolí pomocí přihlašovacích údajů, se kterými jste se přihlásili na portál klasické Azure.

    ![Povolte jezera úložiště dat] (./media/data-lake-store-stream-analytics/create.output.2.png "Povolte jezera úložiště dat")

4. Na stránce **Nastavení dat jezera úložiště** zadejte informace, jak je znázorněno níže snímek obrazovky.

    ![Nastavení zadat Data jezera úložiště] (./media/data-lake-store-stream-analytics/create.output.3.png "Nastavení zadat Data jezera úložiště")

    * **Enter alias výstupu**. Toto je jedinečný název, který stanoví výstupu projektu.
    * **Zadejte účet jezera úložiště**. Měli jste již vytvořili tak, jak je uvedeno v předpoklad.
    * **Zadejte cestu předponu vzorek**. To je nutné k identifikaci výstupní soubory, které úložiště jezera dat píší úlohy toku analýzy. Vzhledem k tomu názvy výstupy napsal projektu ve formátu GUID, včetně předpony vám pomůže identifikovat písemné výstupu. Pokud chcete-li jako součást předponu zahrnout datem a časovým razítkem Přesvědčte se, můžete zahrnout `{date}/{time}` ve vzorku předponu. Pokud jste to, jsou povoleny pole **Formát data **a **Času formátu** a můžete vybrat formát voleb.

    Klikněte na tlačítko vpřed.

5. Na stránce **Nastavení serializace** nastavit formát serializace jako **CSV**, oddělovač jako na **kartě**kódování jako **UTF8**a klepněte na značku zaškrtnutí.

    ![Zadat výstupní formát] (./media/data-lake-store-stream-analytics/create.output.4.png "Zadat výstupní formát")

6. Po dokončení práce s průvodci výstupu úložišti jezera přidá na kartě **výstupy** a sloupci **diagnostiky** byste měli vidět **OK**. Můžete také explicitně test připojení do výstupu pomocí tlačítko **Testovat připojení** dole.

## <a name="run-the-stream-analytics-job"></a>Spuštění úlohy toku analýzy

Spuštění úlohy toku analýzy, je třeba spustit dotaz na kartě Query. Pro účely tohoto návodu spuštěním ukázkový dotaz nahrazením zástupné symboly úlohu vstupní a výstupní aliasy, jak je znázorněno níže snímek obrazovky.

![Spuštění dotazu] (./media/data-lake-store-stream-analytics/run.query.png "Spuštění dotazu")

Klikněte na tlačítko **Uložit** v dolní části obrazovky a pak klepněte na tlačítko **Start**. V dialogovém okně vyberte **Vlastní čas**a potom vyberte datum z minulých chyb, například **1/1/2016**. Klikněte na značku zaškrtnutí úlohu spustíte. Může trvat až na pár minut úlohu spustíte.

![Nastavení pracovní doby] (./media/data-lake-store-stream-analytics/run.query.2.png "Nastavení pracovní doby")

Po spuštění projektu, klikněte na kartu **Monitor** zobrazíte zpracování data.

![Sledování práce] (./media/data-lake-store-stream-analytics/run.query.3.png "Sledování práce")

Nakonec můžete [Azure portál](https://portal.azure.com) si potřebujete založit účet úložiště jezera dat a ověřte, jestli data napsané úspěšně k tomuto účtu.

![Ověření výstup] (./media/data-lake-store-stream-analytics/run.query.4.png "Ověření výstup")

V podokně Průzkumník dat Všimněte si, že je výstup zapsán do složky určené v úložišti jezera nastavení výstupu (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Viz taky

* [Vytvoření clusteru HDInsight používat jezera úložiště dat](data-lake-store-hdinsight-hadoop-use-portal.md)
