<properties 
    pageTitle="Úložiště objektů blob ukázková data v Azure | Microsoft Azure" 
    description="Ukázková data v úložišti objektů Blob Azure" 
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
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Úložiště objektů blob ukázková data v Azure


Tento dokument obsahuje odběr dat uložených v úložišti objektů blob Azure tak, že si ji stáhnete programově a odběr pomocí procedury napsané v Python.

**Proč ukázková data?**
Pokud datovou sadu, které chcete analyzovat velký, není vhodné dolů ukázková data, která chcete zmenšit velikost menší, ale zástupce a lépe. To usnadňuje Principy dat, průzkum a inženýrské funkce. Jeho role v procesu analýzy Cortana je to povolíte rychlé vytváření prototypů z funkcí zpracování dat a automatické učení modely.

**Nabídka** pod odkazů na témata, která popisují, jak vzorová data z různých prostředích úložiště. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Analytický nástroj vzorkování úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Stáhněte si a dolů ukázková data
1. Stažení dat z úložišti objektů blob Azure pomocí služby objektů blob z následující ukázkový Python kód: 

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

2. Načtení dat do Pandas dat snímku ze souboru stáhli nad.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Šipka dolů ukázkových dat pomocí `numpy`společnosti `random.choice` následujícím způsobem:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Teď můžete spolupracovat s rámečku výše uvedených dat s ukázkovými procent 1 pro další informace a funkce generování.

##<a name="heading"></a>Odeslání dat a načíst do výukové počítače Azure

Následující ukázkový kód umožňuje dolů ukázková data a použít ho přímo v Azure ML:

1. Psaní rámečku dat do místního souboru

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Odeslání místní souboru do Azure objektů blob pomocí následující ukázkové kódu:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Číst data z objektů blob Azure pomocí Azure ML [Importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , jak je znázorněno na následujícím obrázku:
 
![kulatý Readeru](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
