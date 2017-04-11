<properties 
    pageTitle="Zkoumání dat v úložišti objektů blob Azure s Pandas | Microsoft Azure" 
    description="Návod na zkoumání dat, který je uložený v kontejneru objektů blob Azure pomocí Pandas." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Zkoumání dat v úložišti objektů blob Azure s Pandas

Tento dokument obsahuje informace, které můžete prozkoumat data, která je uložena v kontejneru objektů blob Azure pomocí [Pandas](http://pandas.pydata.org/) Python balíčku.

Následující **nabídky** odkazy na témata, která popisují, jak používat nástroje, které můžete prozkoumat data z různých prostředích úložiště. Tento úkol je kroku v [Procesu vědy dat]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte:

* Vytvoření účtu Azure úložiště. Pokud potřebujete pokyny najdete v článku [Vytvoření účet Azure úložiště](../storage/storage-create-storage-account.md#create-a-storage-account)
* Data uložená v účtu úložiště objektů blob Azure. Pokud potřebujete pokyny, přečtěte si článek [přesouvání dat z Azure úložiště a](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Načtení dat do Pandas DataFrame
Zkoumání a pracovat s datovou sadu, ho musíte nejdřív stáhnout zdrojem objektů blob do místní souboru, který lze načíst v Pandas DataFrame. Tady jsou v tomto procesu postupujte podle těchto kroků:

1. Stažení dat z Azure objektů blob s v následujícím příkladu kód Python pomocí služby objektů blob. Nahraďte proměnnou následující kód určitých hodnot: 

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

##<a name="blob-dataexploration"></a>Příklady použití Pandas průzkum

Tady je několik příkladů způsobů, jak prozkoumání dat pomocí Pandas:

1. Kontrola **počet řádků a sloupců** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Zkontrolovat** první nebo poslední pár **řádků** v následujících sadě dat:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Kontrola **datového typu** importované každý sloupec oblasti jako pomocí následující ukázkové kódu
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Vyberte **základní stat** pro sloupce ve uvedenou množinu dat podle těchto pokynů
 
        dataframe_blobdata.describe()
    
5. Podívejte se na počet faktur od pro každou hodnotu sloupce takto

        dataframe_blobdata['<column_name>'].value_counts()

6. **Určení počtu chybějících hodnot** a skutečný počet položek v každém sloupci pomocí následující ukázkové kódu

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Pokud máte pro konkrétní sloupce **chybějících hodnot** v datech, se můžete pustit následujícím způsobem:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Další způsob, jak nahrazení chybějících hodnot je s funkcí režimu:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Vytvoření grafu **histogramu** vykreslete rozdělení proměnnou pomocí proměnný počet intervalů 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Podívejte se na **korelace** mezi proměnnými pomocí scatterplot nebo použití funkce předdefinované korelace

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
