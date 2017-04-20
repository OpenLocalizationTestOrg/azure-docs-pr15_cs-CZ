<properties 
    pageTitle="Zpracování dat Azure blob s advanced analytics | Microsoft Azure" 
    description="Zpracování dat v úložišti objektů Blob Azure." 
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

#<a name="heading"></a>Zpracování dat Azure blob s advanced analytics

Tento dokument popisuje seznámení dat a generování funkce z dat uložených v úložišti objektů Blob Azure. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Načtení dat do datového rámce Pandas
S cílem prozkoumat a manipulovat s objekt dataset, je stáhnout z binárního rozsáhlého objektu zdroje do místního souboru, které lze načíst v rámci dat Pandas. Zde jsou kroky pro tento postup:

1. Stažení dat z Azure kulatý s následující kód Python vzorku pomocí služby blob. Nahradíte určité hodnoty proměnné v kódu níže: 

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


2. Číst data do data rámečku Pandas ze staženého souboru.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nyní jste připraveni k prozkoumání dat a generování funkce u tohoto objektu dataset.


##<a name="blob-dataexploration"></a>Průzkum

Následuje několik příkladů způsobů, jak prozkoumat data pomocí Pandas:

1. Kontrolovat počet řádků a sloupců 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Kontrolovat první a poslední několik řádků v sadě dat, jak je uvedeno níže:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Zkontrolujte požadovaný datový typ každého sloupce byla importována jako pomocí následující ukázkový kód
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Takto zkontrolujte základní statistiku pro sloupce v sadě dat
 
        dataframe_blobdata.describe()
    
5. Podívejte se na počet položek pro každou hodnotu sloupce takto

        dataframe_blobdata['<column_name>'].value_counts()

6. Počet chybějících hodnot versus skutečný počet položek v každém sloupci pomocí následující ukázkový kód

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Pokud chybí hodnoty pro určitý sloupec dat, můžete je přetáhnout takto:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Další způsob, jak nahradit chybějící hodnoty je funkcí režimu:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Vytvoření histogramu zobrazované pomocí proměnné počet přihrádek k vynesení distribuce proměnné 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Podívejte se na korelace mezi proměnnými scatterplot pomocí nebo pomocí funkce vestavěné korelace

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Funkce generování
    
Jsme může generovat Python pomocí následující funkce:

###<a name="blob-countfeature"></a>Hodnota ukazatele založené generace funkce

Kategorií funkcí lze vytvořit takto:

1. Kontrolovat rozdělení kategorií sloupců:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generování hodnot ukazatelů pro každý sloupec hodnot

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Spojení s původním snímkem dat sloupec indikátor 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Odeberte původní proměnnou:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Funkce generování binning

Pro generování binned funkce, doporučujeme postupovat takto:

1. Přidat posloupnost sloupců do přihrádky číselný sloupec.
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Převést na posloupnost booleovské proměnné binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Nakonec připojte fiktivní proměnné zpět na původní snímek dat

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Zápis dat objektů blob Azure a spotřebovávat v Azure Machine Learning

Poté, co jste využili data a vytvořit nezbytné funkce, můžete odeslat data (vzorky nebo featurized) pro Azure kulatý a spotřebovat v Azure Machine Learning pomocí následujících kroků: vytvořeny další funkce v také Studio učení počítače Azure. 
1. Zapisovat do místního souboru dat rámce

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Odešlete data do Azure blob takto:

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

3. Nyní lze číst data z binárního rozsáhlého objektu pomocí Azure Machine Learning [Importovat Data] [ import-data] modul, jak je znázorněno v následující obrazovky:
 
![binární rozsáhlý objekt čtecího zařízení][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
