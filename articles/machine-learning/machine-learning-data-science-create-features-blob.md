<properties
    pageTitle="Vytvoření funkcí pro datový úložiště objektů blob Azure pomocí Panda | Microsoft Azure"
    description="Jak vytvořit funkcí pro data, která je uložena v kontejneru objektů blob Azure s balíček Panda Python."
    services="machine-learning,storage"
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Vytvoření funkcí pro datový úložiště objektů blob Azure pomocí Panda

Tento dokument ukazuje, jak vytvořit funkcí pro data, která je uložena v kontejneru objektů blob Azure pomocí [Pandas](http://pandas.pydata.org/) Python balíčku. Po osnovy postup data načítáte do rámečku dat Panda, ukazuje, jak generovat kategorií funkcí pomocí skriptů Python indikátor hodnotami a binning funkce.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Této **nabídce** odkazů na témata, která popisují, jak vytvořit funkcí pro data v různých prostředích. Tento úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto článku se předpokládá vytvoření účtu úložiště objektů blob Azure a uložená vaše data. Pokud potřebujete pokyny pro účet nastavit, najdete v článku [Vytvoření účet Azure úložiště](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Načtení dat do rámečku Pandas dat
Při zkoumání a pracovat s datovou sadu, třeba stáhnout zdrojem objektů blob do místní souboru, který lze načíst v rámečku Pandas data. Tady jsou v tomto procesu postupujte podle těchto kroků:

1. Stažení dat z Azure objektů blob kódem následující ukázkové Python pomocí služby objektů blob. Nahraďte proměnnou kód pod určitých hodnot:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Načtení dat do dat – snímku Pandas z stažený soubor.

        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Teď jste připraveni na prozkoumání dat a generovat funkcí v této sadě dat.

##<a name="blob-featuregen"></a>Funkce generování

Následující dva oddíly předvedení generovat kategorií funkcí s hodnotami indikátor a binning funkce pomocí skriptů Python.

###<a name="blob-countfeature"></a>Indikátor hodnotu na základě funkce generování

Kategorií funkcí lze vytvořit takto:

1. Zkontrolujte výskyt sloupci kategorií:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Vytvořte indikátor hodnoty jednotlivých hodnot sloupce

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Připojit se ke sloupci indikátorů rámečku původní data

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Odebrání původní proměnnou:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Analytický nástroj Generátor binning funkce

Pro generování binned funkcí jsme postupovat takto:

1. Přidání řadu sloupců tak, aby intervalu číselné sloupce

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Převedení binning posloupnost boolean proměnných

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Nakonec připojení formální proměnné zpátky na původní data rámce

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Zápis dat zpět na objektů blob Azure a jinými Azure počítač přečíst

Poté, co jste využili data a vytvořili potřebné funkcí, můžete odeslat data (vzorky nebo featurized) do Azure objektů blob a používání Azure počítač přečíst pomocí následujících kroků: Další funkce můžete vytvářet v i Studio výukové počítače Azure.
1. Zapisovat rámečku dat do místního souboru

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Uložte data do objektů blob Azure následujícím způsobem:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory

        try:

        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)

        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Teď jde přečíst data z objektů blob používání modulu Azure počítače výukové [Importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , jak je vidět na následující obrazovce:

![kulatý Readeru](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
