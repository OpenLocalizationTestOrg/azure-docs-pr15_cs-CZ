### <a name="compression-support"></a>Podpora komprese  
Zpracování velkých sad dat může způsobit vstupu a výstupu a síťové problémů. Proto zkomprimovaná data v úložištích můžete nejen dosažení vyššího přenos dat v síti a ušetřit místo na disku, ale také přenést vylepšení výkonu při zpracování velké data. Komprese je v současné době podporované pro dat na základě souboru ukládá například objektů Blob Azure nebo místní soubor systém.  

> [AZURE.NOTE] Nastavení komprese nepodporuje data z **AvroFormat**, **OrcFormat**nebo **ParquetFormat**. 

Chcete-li komprese pro datovou sadu, vlastnost **Komprese** v datové sadě JSON jako v následujícím příkladu:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
V části **Komprese** obsahuje dvě vlastnosti:  
  
- **Typ:** kodek komprese, které můžou být **GZIP**, **Deflate** nebo **BZIP2**.  
- **Úroveň:** kompresi, který může být **Optimal** nebo **nejrychlejší**. 
    - **Nejrychlejší:** Operaci komprese měli dokončit co nejdříve, i když není výsledný soubor optimálně komprimovány. 
    - **Optimal**: operaci komprese by měl být optimálně komprimován, i když operace trvá delší dobu dokončení. 
    
    Další informace najdete v tématu [Úroveň komprese](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . 

Předpokládejme, že výše uvedené ukázkové datové slouží jako výstup aktivitu kopírovat, kopírovat aktivity zkomprimuje výstupní data s GZIP kodek pomocí optimální poměr a pak napište zkomprimovaná data do do souboru nazvaného pagecounts.csv.gz v úložišti objektů Blob Azure.   

Pokud zadáte vlastnost komprese v zadávání datovou sadu JSON, kanálu může číst zkomprimovaná data ze zdroje a zadáte vlastnost JSON, datové sady výstup aktivity kopírovat můžete napsat zkomprimovaná data do cíle. Tady je pár ukázkových situací: 

- Čtení GZIP komprimovány dat z Azure objektů blob, rozbalte ho a napište Výsledná data k databázi Azure SQL. V tomto případě definujete vstupní sadu objektů Blob Azure pomocí komprese JSON vlastnost. 
- Číst data ze souboru ve formátu prostého textu z místního systému souborů, komprimovat ho ve formátu GZip a zapisovat zkomprimovaná data objektů blob Azure. V tomto případě definujete datovou sadu objektů Blob Azure výstup pomocí komprese JSON vlastnost.  
- Čtení komprimovány GZIP dat z Azure objektů blob rozbalte ho, komprimovat pomocí BZIP2 a zapisovat Výsledná data objektů blob Azure. Definování vstupní datovou sadu objektů Blob Azure komprese typ nastavený na GZIP a datovou sadu výstup komprese typ nastavený na BZIP2 v tomto případě.   
